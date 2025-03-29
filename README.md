# File-Upload-In-Express-js
1. install multer.
    npm i multer
2.Make a GET method for file upload

 app.get("/upload",(req,res)=>{
     
    res.sendFile(__dirname+"/index.html");
});

and index.html

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">

    <title>Express file Upload with multer</title>
    
  </head>
  <body>

    <form
      action="http://localhost:3000/" 
      method="post" 
      enctype="multipart/form-data"
    >

     <input type="file" name="avatar" multiple>
     <input type="submit" name="submit">

    </form>
     
  </body>
</html>
   


