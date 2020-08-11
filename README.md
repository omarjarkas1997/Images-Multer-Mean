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

##### Adjusting How files gets store

We can see that the files are being store in a non organized fashion. We can specify how the files need to be stored exactly using the below module found in multer.

            const storage = multer.diskStorage({
                destination: function(req, file, cb){

                },
                filename: function(req, file, cb) {
                    
                }
            });

In the destination, we pass the path where we want to store the file in the callback handler. In this case we specify the file we uploaded before.

In the filename, we specify how the file should look like. 
We can use:
1. **file.filename**, *the property is in the req body*
2. Assign a new date to the file name and concatinate this with the original name

            *new Date().toISOString() + file.originalname*


                const storage = multer.diskStorage({
                    destination: function(req, file, cb){
                        cb(null, './uploads/');
                    },
                    filename: function(req, file, cb) {
                        cb(null, new Date().toISOString().replace(/:/g, '-') + file.originalname ); 
                    }
                });


We need to pass the storage to multer in order for the middleware to work.

from: 

            const upload = multer({dest: 'uploads/'});
to: 
            const upload = multer({storage: storage});

#### Setting a limit to only accept certain files

            const upload = multer({storage: storage, limits: {
                fileSize: 1024 * 1024 * 5
            }});

#### You can also setup some filters

The below filter function onyl accepts file with jpeg or png extentions


            const fileFilter = (req, res, cb) => {

                if (file.mimetype === 'image/jpeg' || file.mimetype === 'image/png') {
                    // accept a file with the below cb
                    cb(null, true);
                }else {
                    // reject a file with the below cb
                    cb(null, false);
                }
            }

We must now add one additional property which is file filter to the upload defined earlier.


## Getting the files

1. First we need to change our db model to accept image info


                    var UserSchema = new Schema({
                    firstName: {
                        type: String,
                        required: true,
                        trim: true
                    },
                    lastName: {
                        type: String,
                        required: true,
                        trim: true
                    },
                    email: {
                        type: String,
                        unique: true,
                        required: true,
                        trim: true
                    },
                    password: {
                        type: String,
                        required: true
                    },
                    userImage: { type: String}
                });

2. Save the server file path on the db


                const user = new User({
                    _id: new mongoose.Types.ObjectId(),
                    firstName: req.body.firstName,
                    lastName: req.body.lastName,
                    email: req.body.email,
                    password: req.body.password,
                    userImage: req.file.path
                });


3. Make the file path publically available to veiw
                // Making images publically avaible 

                app.use(express.static('uploads'));

4. Viewing images

        http://localhost:3000/uploads/1597121153984omar.jpg

# Upload Files from Angular to Express js and Mongo











