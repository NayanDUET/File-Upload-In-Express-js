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



