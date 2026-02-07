function doGet() {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var displayData = sheet.getDataRange().getDisplayValues();
  var rows = [];
  for (var i = 1; i < displayData.length; i++) {
    if(displayData[i][1]) {
      rows.push({
        rowId: i + 1, 
        sl: displayData[i][0], n: displayData[i][1], l: displayData[i][2],
        g: displayData[i][3], p: displayData[i][4], last: displayData[i][5]
      });
    }
  }
  return ContentService.createTextOutput(JSON.stringify(rows)).setMimeType(ContentService.MimeType.JSON);
}

function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var data = JSON.parse(e.postData.contents);
  if (data.action === 'delete') {
    sheet.deleteRow(parseInt(data.rowId));
    return ContentService.createTextOutput("Deleted").setMimeType(ContentService.MimeType.TEXT);
  }
  var lastRow = sheet.getLastRow();
  sheet.appendRow([lastRow, data.name, data.location, data.group, data.phone, data.lastDate]);
  return ContentService.createTextOutput("Success").setMimeType(ContentService.MimeType.TEXT);
}
