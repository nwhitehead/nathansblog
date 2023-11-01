+++
title = 'Workspace Add-On vs. Editor Add-On'
date = 2023-10-31T11:34:23-06:00
draft = false
+++

I had hoped to write a full Google Workspace Add-On to do the ChatGPT functions, but apparently that
won't work. According to a StackOverflow [answer](https://stackoverflow.com/questions/75253890/how-to-add-a-custom-function-to-google-sheets-from-a-google-workspace-add-on/75265267#75265267), you need to make an Editor Add-On.

Documentation for making Editor Add-On custom functions: [custom functions](https://developers.google.com/apps-script/guides/sheets/functions)

Thinking about how to store the OpenAI API key: https://stackoverflow.com/questions/61540618/storing-api-keys-and-secrets-in-google-appscript-user-property

## Progress

I got `double` to work as a Sheet function. I created a new test spreadsheet, then clicked on `Extensions → Apps Script`. From there I typed up the `double` code.

```
/**
 * Multiplies an input value by 2.
 * @param {number} input The number to double.
 * @return The input multiplied by 2.
 * @customfunction
 */
function DOUBLE(input) {
  console.log('hello');
  return input * 2;
}
```

That worked. Apparently that is called a "container script" since it is closely associated with the spreadsheet.

Next I tried to make some UI hooks. This worked:
```
function onOpen() {
  var ui = SpreadsheetApp.getUi();
  ui.createMenu('Credentials & Authentication')
    .addItem('Set API key', 'setKey')
    .addItem('Delete API key', 'resetKey')
    .addItem('Delete all credentials', 'deleteAll')
  .addToUi();
}
```

You need to access the `getUi()` part inside `onOpen()`. Many examples online didn't show it that way, maybe it has changed over time.

The above method adds a new top level menu, which is not exactly what I wanted. I wanted something like the "GPT for Sheets and Docs" menu,
which appears in "Extensions".

To get it as a normal add-on menu, I switched to using `createAddonMenu()`. According to dev docs that is the right way to do it. Published add-ons need to
do it like that, can't create top-level menus.

It keeps closing my Apps Script tab. That is annoying!

Can also do `showSidebar()` to show HTML files. That is the way to go.

Now I need to figure out how to write nice HTML pages for sidebars with good CSS. Also need to get command line tools working, clicking in web IDE is ridiculous.
Using `clasp` now.
