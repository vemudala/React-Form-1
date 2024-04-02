vmd 



<html>  
<head>  
    <title>Upload Image</title>  
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css" />  
</head>
<style>
 .box
 {
  width:100%;
  max-width:600px;
  background-color:#f9f9f9;
  border:1px solid #ccc;
  border-radius:5px;
  padding:16px;
  margin:0 auto;
 }
 .msg
{
  color: red;
  font-weight: 700;
} 
</style>
<body style="position:absolute;left:20%;top:20%">  
    <div class="container">  
    <div class="table-responsive">  
    <h3 align="center"></h3>
    <form id="ajaxupload" method="post" enctype="multipart/form-data">
     <div class="box">
      <div class="form-group">
       <label for="file">Select File</label>
       <input type="file" name="file" id="upload-image" class="form-control"/>
      </div>  
      <div class="form-group">
       <input type="submit" id="upload" name="upload" value="Submit" class="btn btn-success" />
      </div>
      <p class="successmsg" style="display:none">Published Successfully</p>
     </div>
   </form>
   </div>  
  </div>
  <script src="https://code.jquery.com/jquery-3.7.1.js"></script>
<script type="text/javascript">
  $(document).ready(function(){
  
  let myurl = "http://10.15.4.124:9190/uptimereport/uploadFile";
    $("#ajaxupload").submit(function(e){
      e.preventDefault();
      var formval = new FormData(this);
      $.ajax({
        method: 'post',
        url: myurl,
		headers: { 
            "Authorization": "Basic YWRtaW46ZUAydE5Bdlo="
          },
        data: formval,
        cache: false,
        processData: false,
        contentType: false,
        success: function(data){
          $('.msg').html(data);
		  let myfileName=data.fileName;
		  		  //---------------api 2----------
		  var  dataObj= JSON.stringify([
  {
    "playlist": "Playlist1",
    "alerttext": myfileName,
    "defaulttext": "This is Message Zone",
    "startDate": "2024-01-11 11:00:00",
    "foreColor": "#000000",
    "backColor": "#FFFF00",
    "duration": 10,
    "fontSize": 18,
    "fontName": "Verdana",
    "fontStyle": "Bold",
    "textDirection": "",
    "userId": 1,
    "isActive": "Y",
    "layoutName": "Combo1",
    "zoneName": "Media"
  }
]);
console.log(dataObj);

$.ajax({
          type: "POST",
          url: 'http://10.15.4.124:9190/emergencymsgs/SaveAlertMsgPlaylists',
        
		  
		headers: { 
		 "content-type": "application/json",
            "Authorization": "Basic YWRtaW46ZUAydE5Bdlo="
          },
		data: dataObj
        }).done(function (data) {
       let playlistId=data;
	   // ===============api3-----------------
	     var  dataObj= JSON.stringify([ { "vmd": "3723350D3741334439",
"startDate": "2024-01-11 09:45:00",
"endDate": "2024-01-11 14:10:00",
"playlistID":playlistId,
"alertDt": "2024-01-11 09:45:00",
"userId": 1,
"status" : "Pending",
"active": "Y",
"zoneName": "Media"
} ]);
console.log(dataObj);

$.ajax({
          type: "POST",
          url: 'http://10.15.4.124:9190/emergencymsgs/SaveAlertMsg',
        
		  
		headers: { 
		 "content-type": "application/json",
            "Authorization": "Basic YWRtaW46ZUAydE5Bdlo="
          },
		data: dataObj
        }).done(function (data) {
       //alert(data, "playlist api3")
	   $('.successmsg').show();
      });
	   //------------------end of api3----------------
      });
       
		  //---------------end of api 2----------
        }
      });
    });
	$('.successmsg').click(()=>{
//$('.successmsg').fadeOut();
$('.successmsg').hide();

	});
  });
</script>
 </body>  
</html>
