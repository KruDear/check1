unction myFunction() {
  var ss = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('ใส่ชื่อชีต')
  var names = ss.getRange(1, 2,1,ss.getLastColumn()).getValues()[0]
  var check = ss.getRange(ss.getLastRow(), 2,1,ss.getLastColumn()).getValues()[0]
  var std = names.length-1
  var date = Utilities.formatDate(new Date(), 'GMT+7', 'dd/MM/')
  var year = Number(Utilities.formatDate(new Date(), 'GMT+7', 'yyyy'))+543
  
  var index =1

  var countNo = check.filter(x=> x == "ลา" || x == "ขาดเรียน" ).length
  var countSick = check.filter(x=> x == "ป่วย" ).length
  var countCheck = std-countNo
  var result = ""
  check.forEach((row,i)=>{
                if(row == "ป่วย" || row == "ขาดเรียน" || row == "ลา"){
    result+= "\n"+ (index++)+ "เลขที่ " + names[i]+ ": "+row
  }


    })
var msg = "📌ประจำวันที่ "+date+year+"\n 🏫 นักเรียนทั้งหมด "+std+" คน \n ✅ มาเรียน "+countCheck+" คน \n 🤒 ป่วย "+countSick+" คน\n ❌ ไม่มาเรียน "+countNo+" คน \n 📊 รายชื่อนักเรียน(ป่วย,ขาดเรียน,ลา) ได้แก่ \n"+result
    sendNotify(msg)
}  
var token ="ใส่token"//โทเคน
function sendNotify(msg){
let payloadJson = {
       "message": msg
    };

    let options = {
        "method": "post",
        "payload": payloadJson,
        "headers": {
            "Authorization": "Bearer " + token
        }
    };
    UrlFetchApp.fetch("https://notify-api.line.me/api/notify", options);
}
