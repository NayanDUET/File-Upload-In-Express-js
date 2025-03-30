# 📂 File Upload in Express.js with Multer

This guide will help you set up file uploads in an Express.js application using the `multer` middleware.

---

## 🚀 Installation

First, install `multer` using npm:

```sh
npm i multer

```
Then make a index.html file for 'GET' method

Here the index.html file

```sh
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

     <input type="file" name="avatar">
     <input type="submit" name="submit">

    </form>
     
  </body>
</html>
```
And 

```sh
app.get("/upload",(req,res)=>{
     
    res.sendFile(__dirname+"/index.html");
});

```
##For the single field file upload

```sh

const express = require('express');
const multer = require('multer');


const upload = multer({ 

    dest: 'uploads/' //here files be uploded
});

const app = express();
 

app.get("/upload",(req,res)=>{
     
    res.sendFile(__dirname+"/index.html");
});

app.post('/', upload.single('avatar'),(req, res, next)=> {
  // req.file is the `avatar` file
  res.send('file uploded');
});

app.listen(3000,()=>{
    console.log('3000 port is running...');
});

```

##For the single field multiple files upload 
and make sure that index.html file be
```sh
<input type="file" name="avatar" multiple>
<input type="submit" name="submit">
```
```sh
app.post('/', upload.array('avatar',5),(req, res, next)=> {
  // req.file is the `avatar` file
  res.send('file uploded');
});
```


##And For the multiple field files  upload
the index.html file be

```sh
<input type="file" name="avatar" multiple>
<input type="file" name="gallery" multiple>
<input type="submit" name="submit">
```

```sh
const cUploade = 
    upload.fields([

        {name: 'avatar', maxCount: 1 }, 
        { name: 'gallery', maxCount: 8}
    
       ]);

app.post('/', cUploade,(req, res, next)=> {
   res.send('file uploded');
});
```


###if you want ot control over the each feild in separate condition then

```sh

const upload = multer({ 

    dest: 'uploads/' ,
    limits:{
      fileSize : 1000000 //1MB
    },
    fileFilter :(req,file,cb)=>{

        
        if(file.fieldname === 'avatar')
        {
            if(file.mimetype === "image/png" || 
                file.mimetype === "image/jpg" ||
                file.mimetype === "image/jpeg")
            {
                  cb(null,true);
    
            }
            else
            {
                cb(new Error("Only .jpg,.png or .jpeg format allowed"));
            }
        }
        else if(file.fieldname === 'gallery')
        {
            if(file.mimetype === 'application/pdf')
            {
                  cb(null,true);
    
            }
            else
            {
                cb(new Error("Only pdf format allowed"));
            }
        }
        else
        {
            cb(new Error('There was an unkonw error'));
        }

    }
});

const app = express();
 

app.get("/upload",(req,res)=>{
     
    res.sendFile(__dirname+"/index.html");
});


const cUploade = 
    upload.fields([

        {name: 'avatar', maxCount: 1 }, 
        { name: 'gallery', maxCount: 8}
    
       ]);

app.post('/', cUploade,(req, res)=> {
   res.send('file uploded');
});
```
###This is the better way to Control/restriction in File system like Destination,filename

```sh
const storage = multer.diskStorage({ 

    destination:(req,file,cb) =>{

     cb(null,'uploads/');
    
    },
    filename:(req,file,cb)=>{

         const fileExt = path.extname(file.originalname); //dont forget to const path = require('path');
         const fileName = file.originalname
                          .replace(fileExt,"")
                          .toLowerCase()
                          .split(" ")
                          .join("-")+"-"+Date.now();
        cb(null,fileName+fileExt);
    },
    
});

const upload = multer({ 
   
    storage:storage,  //put here 
    limits:{
      fileSize : 1000000 //1MB
    },
    fileFilter :(req,file,cb)=>{
        if(file.mimetype === "image/png" || 
            file.mimetype === "image/jpg" ||
            file.mimetype === "image/jpeg")
        {
              cb(null,true);

        }
        else
        {
            cb(new Error("Only .jpg,.png or .jpeg format allowed"));
        }

    }
});

```



