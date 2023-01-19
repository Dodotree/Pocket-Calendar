# Notes I wish I had when I started

VSCode debugger does not evaluate modern JS
To access console log  open BASH and "tail -f" UXP log file
Windows:
```sh 
tail -f "C:\Users\<User Name>\AppData\Local\Temp\UXPLogs\UXPErrorLogs_<date>.log"
```
Mac: 
```sh
tail -f "/Users/<my-username>/Library/Caches/UXPLogs/"
```

None of events for `app.dialogs.add({name:"My dialog"})` are working so far.
`document.createElement("dialog")` looks like much more flexible way of handling user interface, but is in process of change

VSCode highlites and formats after initial setup

## Documentation:
https://www.indesignjs.de/extendscriptAPI/
https://developer.adobe.com/indesign/uxp/uxp/  look everything under References menu, Guides, Code Samples
https://documentation.help/InDesign-CS3/pc_Swatch.html  sometimes helps in especially mysterious cases

Forum
https://community.adobe.com/t5/indesign/ct-p/ct-indesign?page=1&sort=latest_replies&lang=all&tabid=all&topics=label-uxpscripting

## Unexpected from outsider point of view:

It is possible to load separate files for configuration.
Some problems are easier to solve by setting preferences instead of attempting things to change later. For example:
Document:
```sh
    myDocument.viewPreferences.horizontalMeasurementUnits = MeasurementUnits.POINTS;
    myDocument.viewPreferences.verticalMeasurementUnits = MeasurementUnits.POINTS;
```
TextFrame:
```sh
    myTextFrame.textFramePreferences.autoSizingType = AutoSizingTypeEnum.HEIGHT_AND_WIDTH;
```

Tables are items of textFrames, table will be treated as a single character of the textFrame
To insert anything into textFrame you need insertionPoint, this will give you the last one
myTextFrame.insertionPoints.item(-1)

`.item(i)` give an element of the collection. You can not iterate through the collection by simply changing the index.
To iterate in bulk, you have to get `.everyItem().getElements()`, which will turn the collection into an array, where
`everyItem ()` Returns every Cell in the collection
`getElements ()` Resolves the object specifier, creating an array of object references.
https://community.adobe.com/t5/indesign-discussions/adjust-column-width-in-tables/m-p/6631680

You can have a table in the table. The column width does not adjust to the content. Columns initially fill the space evenly. If you decrease the width of 1 column, it will decrease the width of the whole table. It's better to set the table so that the column width is enough to fit the widest column and then decrease them one by one. Row height, though, will adjust automatically unless you will flag it as not adjustable and set the height. But row height will not change automatically if you change text leading, unlike with CSS font line height.

Setting paragraph, cell, table styles, and layers are easy and create convenient objects in InDesign tabs for further tuning. Also, you can assign individual styles to separate cells, etc. Though tables are not flexible and there's a chance that after tuning, you will need to redo the table with new styles. 

To merge cells in the table row was enough to merge both ends:
```sh
yearTable.rows.item(4).cells.item(0).merge( yearTable.rows.item(4).cells.item(4) );
```

`.place(path-to-img)` does not work outside of the `await`. Asynchronous functions and async/await pattern gone wrong demonstrate itself with infinite load sign. But if you tru to close file and then press "cancel" in pop up, it breaks from the await loop. Wrap main function in promice to fix this. 

```sh
await main();

async function main() {
    return new Promise(async(resolve, reject) => {
        resolve();
    });
}
```

Cloning, replicating, or copy-pasting objects is called `obj.duplicate()` and works much better than attempts to use app.copy() app.paste()

(to be continued)