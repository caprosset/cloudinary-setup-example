# cloudinary-example


### 1. Create free Cloudinary account 
Register for free here: <https://cloudinary.com/users/register/free>



### Install packages

Install the following 3 packages in your project folder:
* cloudinary [https://www.npmjs.com/package/cloudinary]
* multer-storage-cloudinary [https://www.npmjs.com/package/multer-storage-cloudinary]
* multer [https://www.npmjs.com/package/multer]

In your terminal:
```
npm install cloudinary multer-storage-cloudinary multer --save
```


### Cloudinary & multer config

In your terminal:
```
mkdir config 
touch config/cloudinary.js
```

In config/cloudinary.js
```
const cloudinary = require('cloudinary');
const cloudinaryStorage = require('multer-storage-cloudinary');
const multer = require('multer');

require('dotenv').config();

cloudinary.config({
  cloud_name: process.env.CLOUDINARY_CLOUD_NAME,
  api_key: process.env.CLOUDINARY_API_KEY,
  api_secret: process.env.CLOUDINARY_API_SECRET
});

const storage = cloudinaryStorage({
  cloudinary: cloudinary,
  folder: 'destination-folder-in-cloudinary',
  allowedFormats: ['jpg', 'png', 'jpeg', 'gif'],
  transformation: [{ width: 500, height: 500, crop: 'limit' }]
});
 
const parser = multer({ storage: storage });

module.exports = parser;
```


### In the hbs view

Add the attribute ```enctype="multipart/form-data"``` to your form   
and the attribute ```type=file``` to the input field that will hold the image.

```
<form action="/endPointName" method="POST" enctype="multipart/form-data">
  <input type="file" name="photo" />
  <button type="submit">New GIF</button>
</form>
```


### In the router where we want to upload the image

Import the parser middleware    
Add it as an argument prior to the function that handles the POST request

```
const parser = require('./../config/cloudinary');

router.post('/endPointName', parser.single('photo'), (req, res, next) =>{
  
  const image_url = req.file.secure_url

}
```


#### Documentation
Cloudinary[https://cloudinary.com/documentation]