# Images-Multer-Mean

### First We need to install the dependency
#### We will use multer in this case 

Multer is used to parse form data body. Essentially it works like body parse but for files inside the request body.

                npm install multer --save


We need to require the dependency in the file and and initialize it to varaible **upload** where income files will be stored in the specified directory.

            // Using multer in the file where we have the route
            const multer = require('multer');
            const upload = multer({dest: 'uploads/'});

We can see that we chose **'/uploads/'** as the folder to stores photos in. However, this directory in not accessible by default.
We need to set this directory to be statically accissable by our server. 

                    // Todo
                    // Insert the static permission here

##### The Router handle will be of the form below.
It need contains a middleware that allows incoming requested to be parsed

            router.post('/acceptImages', upload.single('productImage'), (req, res, next) => {
                console.log(req.file);
            });

###### Now in PostMan try to send a file to the server

When sending the request to the server, the request can contain the file/image and the request body to.
They are specified in ket value pairs.

After Sending the file to the server, we can see the it was directly saved in a folder named uploads. **(This is done without even giving permission to the statis folder)**

