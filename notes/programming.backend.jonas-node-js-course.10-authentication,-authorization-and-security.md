---
id: rpgo4g8pvfzg2or1rx6jqwa
title: '10 Authentication, Authorization and Security'
desc: ''
updated: 1663409164562
created: 1663409164562
isDir: false
latitude: 21.0313
longitude: 105.8516
altitude: 0
title_imported: '10 Authentication, Authorization and Security'
updated_imported: '2020-02-07T02:50:51.000Z'
created_imported: '2019-11-07T11:21:15.000Z'
---

[TOC]
# 2 Modelling Users

Create a schema for User Model

User has name, email, photo, password, passwordConfirm

**userModel file**

```js
const mongoose = require('mongoose');
const validator = require('validator');

const userSchema = new mongoose.Schema({
  name: {
    type: String,
    required: [true, 'Please provide a name'],
    maxlength: [15, 'A name must have less than 16 characters!!!'],
    minlength: [5, 'A name must have more than 4 characters!!!']
  },
  email: {
    type: String,
    required: [true, 'Please provide an email!!!'],
    unique: true,
    lowercase: true,
    validate: [validator.isEmail, 'Please provide a valid email!!!']
  },
  photo: String,
  password: {
    type: String,
    required: [true, 'Please provide a password!!!'],
    minlength: [8, 'A password must have more than 7 characters!!!']
  },
  passWordConfirm: {
    type: String,
    required: [true, 'Please provide a password Confirm!!!']
  }
});

const User = mongoose.model('User', userSchema);

module.exports = userSchema;

```

# 3 Creating New Users

**userRoutes.js file**

```js
const authController = require('../controller/authController');

router.post('/signup', authController.signup);
```

**authController.js file**
```js
const User = require('../models/userModel');
const catchAsync = require('../utils/catchAsync');

exports.signup = catchAsync(async (req, res, next) => {
  // to avoid someone want to take controll by set admin role in req.body
  const { name, email, password, passwordConfirm } = req.body;
  const newUser = await User.create({ name, email, password, passwordConfirm });

  res.status(201).json({
    status: 'success',
    data: { user: newUser }
  });
});

```

# 4 Managing Passwords
Manage our users passwords in the DB
+ check inputted password is equal to the confirmed password
+ encrypt password in the DB

Install `bcryptjs` on NPM :

```sh
npm i bcryptjs
```

**in userModel.js File**

```js
const bcrypt = require('bcryptjs');
...
passwordConfirm: {
    type: String,
    required: [true, 'Please comfirm a password!!!'],
    validate: {
      validator: function(el) {
        //this only works on CREATE or SAVE not Update!!!!
        //whenever we want to update a user
        //we will always have to use SAVE not FindOneAndUpdate
        return el === this.password;
      },
      message: 'Passwords are not the same!!!'
    }
  }
...  
//pre-middleware on save
// the encryption is then gonna be happened between the moment that we receive that data and the moment
// where it's actually persisted to the database

userSchema.pre('save', async function(next) {
  //Only run this function if password was actually modified
  if (!this.isModified('password')) return next();

  //hash the password with cost of 13
  this.password = await bcrypt.hash(this.password, 13);

  //delete passwordConfirm field
  this.passwordConfirm = undefined;
  next();
});
```

# 5 How Authentication with JWT Works

![b7f95e0c6bc5809ac621b84fc647eec8.png](/assets/b7f95e0c6bc5809ac621b84fc647eec8-ztlfsp0d6nv0.png)

![c8b68aff1a97b2820424940d94354232.png](/assets/c8b68aff1a97b2820424940d94354232-kc29r8d21iu3.png)

![72eee88d99bc762e3c09db0dfd20ef60.png](/assets/72eee88d99bc762e3c09db0dfd20ef60-7rvrv4bgrt5p.png)



website to decode: https://jwt.io/

paste the secret key to decode : 


![32d54c3ccb9bdcbdf9eceb5e44a71d6b.png](/assets/32d54c3ccb9bdcbdf9eceb5e44a71d6b-hk99b45yck2x.png)



# 6 Signing up Users

Install `jsonwebtoken` on NPM :

```sh
npm i jsonwebtoken
```

Github source: https://github.com/auth0/node-jsonwebtoken

## Create a JWT

In order to create a JSON web token, we will need — three things
1. Payload
2. Secret key
3. Signing options

```js
// PAYLOAD
let payload = {
 data1: "Data 1",
 data2: "Data 2",
 data3: "Data 3",
 data4: "Data 4",
};
// PRIVATE and PUBLIC key
let privateKEY  = fs.readFileSync('./private.key', 'utf8');
let publicKEY  = fs.readFileSync('./public.key', 'utf8');
let i  = 'Mysoft corp';          // Issuer 
let s  = 'some@user.com';        // Subject 
let a  = 'http://mysoftcorp.in'; // Audience
// SIGNING OPTIONS
let signOptions = {
 issuer:  i,
 subject:  s,
 audience:  a,
 expiresIn:  "12h",
 algorithm:  "RS256"
};

const token = jwt.sign(payload, privateKEY, signOptions);
```

External Resource:  https://medium.com/@siddharthac6/json-web-token-jwt-the-right-way-of-implementing-with-node-js-65b8915d550e

## Verify the JWT

```js
let verifyOptions = {
 issuer:  i,
 subject:  s,
 audience:  a,
 expiresIn:  "12h",
 algorithm:  ["RS256"]
};

let legit = jwt.verify(token, publicKEY, verifyOptions);
```

## Implementation in the course

**in env file**
```env
JWT_SECRET = mOt!cai@kEy$tHat%dAi%dai^sUper*daI(super*str0ng-sc3r3t+k3y|for@J$W%T
#min 32characters
JWT_EXPIRES_IN = 30d
```


**in authController.js file**

```js
const jwt = require('jsonwebtoken');
// ...
const signToken = id =>
  jwt.sign({ id }, process.env.JWT_SECRET, {
    expiresIn: process.env.JWT_EXPIRES_IN
  });

exports.signup = catchAsync(async (req, res, next) => {
  // to avoid someone want to take controll by set admin role in req.body
  const { name, email, password, passwordConfirm } = req.body;
  const newUser = await User.create({ name, email, password, passwordConfirm });

  const token = signToken(newUser._id);

  res.status(201).json({
    status: 'success',
    token,
    data: { name, email } // not send back the password
  });
});
```

**response**
```json
{
  "status": "success",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjVkYzRlNWM3MzE0YTUyMWE3NzgzYzE0ZiIsImlhdCI6MTU3MzE4NDk2NywiZXhwIjoxNTc1Nzc2OTY3fQ.2sz19XTi1KKvH-Y7CPrbKIC7h2gRI76xgfzYK6sR0Lo",
  "data": {
    "user": {
      "_id": "5dc4e5c7314a521a7783c14f",
      "name": "Longo 10fff0",
      "email": "longd1234578@gmail.com",
      "password": "$2a$13$aJgAQbqY.LmENPhrTvaUteFjP3FTB9JS9U9zFpyO9hCuta7rs7Txi",
      "__v": 0
    }
  }
}
```

because we turn off the feature of set role directly from the web, so we cannot register an admin be signing up.

We sign a normal user and can set role after that directly from the DB.

# 7 Logging in Users

![b4e7ffb518b0f8d4750414dc724ce4ab.png](/assets/b4e7ffb518b0f8d4750414dc724ce4ab-1qjac7w5k7hu.png)

We need to get email password, check it correct or not and send back token.

**in authController.js**

```js
const signToken = id =>
  jwt.sign({ id }, process.env.JWT_SECRET, {
    expiresIn: process.env.JWT_EXPIRES_IN
  });
  
//...

exports.login = catchAsync(async (req, res, next) => {
  const { email, password } = req.body;

  // 1 check if email and password exist

  if (!email || !password)
    return next(new AppError('Please provide email & password!!!', 400));

  // 2 Check if user exits && password is correct

  const user = await User.findOne({ email }).select('+password'); //{email:email} === {email} in ES6 || +password because it was hidden in db

  // double check if user and password of that user is correct or not
  // write like that because if no user ~> no user.password ~> function go wrong : Cannot read property 'correctPassword' of null
  if (!user || !(await user.correctPassword(password, user.password)))
    return next(new AppError('Incorect Email or Password!!!', 401));

  // 3 If everything is ok, send token to client
  const token = signToken(user._id);

  res.status(200).json({
    status: 'success',
    token
  });
});
```

We need a function to compare password input and the current password in db.

We can write a instance method in userModel to compare it

**in userModel.js**
```js
// an instance medthod to check password correct or not

userSchema.methods.correctPassword = async (candidatePassword, userPassword) =>
  await bcrypt.compare(candidatePassword, userPassword);

const User = mongoose.model('User', userSchema);
```

# 8-9 Protecting Tour Routes

![4993ff52c818ea2bc5aa598447757921.png](/assets/4993ff52c818ea2bc5aa598447757921-ruoptqjil07v.png)

Add Request Headers to the request.

The Syntax of HTTP Authorization request header:

`Authorization: Bearer <credentials>`

```http
@baseUrl = http://127.0.0.1:6969/api/v1/tours

### Get all tour
GET {{baseUrl}} HTTP/1.1
Authorization: Bearer eyJhbGciOiJIUzI1NiIXVCJ9...TJVA95OrM7E20RMHrHDcEfxjoYZgeFONFh7HgQ
```

We will create a protect middleware which is gonna run before
handlers


**in tourRouter.js**
```js
const { protect } = require('../controller/authController');
// ...
router
  .route('/')
  .get(protect, tourController.getAllTours)
  .post(tourController.createTour);
```
## An instance method to check Passord changed after Token isued or not

**In userModel**

```js
// an instance method to check Password changed after Token issued or not

userSchema.methods.changedPasswordAfter = function(JWTTimestamp) {
  if (this.passwordChangedAt) {
    const changedTimestamp = parseInt(
      this.passwordChangedAt.getTime() / 1000,
      10
    );

    return JWTTimestamp < changedTimestamp; // TRUE = Changed = Password was changed AFTER token was issued
  }

  return false; // NOT changed
};
```

## Implement the protect miđleware

**in authController.js**
```js
exports.protect = catchAsync(async (req, res, next) => {
  // 1 Getting token and check of it's there
  let token;

  if (
    req.headers.authorization &&
    req.headers.authorization.startsWith('Bearer')
  ) {
    token = req.headers.authorization.split(' ')[1];
  }

  if (!token)
    return next(
      new AppError('You are not logged in! Please log in to get access!!!', 401)
    );

  // 2 Verification token
  const verifiedToken = await promisify(jwt.verify)(
    token,
    process.env.JWT_SECRET
  ); // promisify(jwt.verify) will return a promise

  // 3 Check if user still exists
  const user = await User.findById(verifiedToken.id);

  if (!user)
    return next(new AppError('This user does no longer exist!!!', 401));

  //4 Check if user changed password after the token was issued
  if (user.changedPasswordAfter(verifiedToken.iat))
    return next(
      new AppError(
        'User has recently changed password! Please log in again to get access!!!',
        401
      )
    );

  req.user = user; // tranfer to the next middleware function

  next();
});
```


# 11 Authorization User Roles and Permissions

If user want to delete the Tour, he must be a admin or leadGuide.

So we can add a middleware to authorization user roles to some important routes.


**in tourRoutes.js**
```js
router
  .route('/:id')
  .get(authController.protect, tourController.getTour)
  .patch(authController.protect, tourController.updateTour)
  .delete(
    authController.protect,
    authController.restrictTo('tulanh', 'leadGuide'),
    tourController.deleteTour
  );
```

**in authController.js**

because a middleware function cannot get parameter directly, so we need to wrap it inside a normal function and then return a middleware like below:

```js
exports.restrictTo = (...roles) => {
  return (req, res, next) => {
    if (!roles.includes(req.user.role))
      return next(
        new AppError(
          'You do not have permission to perform this action!!!',
          403
        )
      );
    next();
  };
};
```
# 12 Password Reset Functionality Reset Token

## Purpose:
Allow user to reset his/her password when he/she forgot it.

## Requirement
- Generate a plain reset token to send to the user through email
- Encrypted that token before saving to the DB so no one can know about the real resetToken
- The token also need a expires time.
- User get the email, get the token,visit the link and can set a new Password.

## Steps
In forgot Password route, when someone POST email to reset password:

in userRoutes

1. Set the forgotPassword and resetPassword routes

in authController

2. Create forgotPassword handler
    - Get the email from user's request and Select that user on DB
    - Generate the random resetToken ( send to user's email) by instance method in userModel

in userModel

3. write the createPasswordResetToken method
    - Generate the plain resetToken
    - Encrypt the plain resetToken to save to DB 
    - Set the passwordResetExpires property to save to DB
    - return the plain resetToken to authController

in authController
4. save the data - turn off the validateBeforeSave
5. Send email with plain resetToken to user
    - if error happend, reset the passwordResetToken and passwordResetExpires = undefined



Create Routes for them:

**in userRoutes.js**
```js
router.post('/forgotPassword', authController.forgotPassword);
router.patch('/resetPassword', authController.resetPassword);
```

**in authController.js**
```js
exports.forgotPassword = catchAsync(async (req, res, next) => {
  // 1 Get user based on POSTed email

  const user = await User.findOne({ email: req.body.email });

  if (!user)
    return next(new AppError('There is no user with email address!!!', 404));

  // 2 Generate the random reset

  const resetToken = user.createPasswordResetToken();
  await user.save({ validateBeforeSave: false }); // turn off the validate because passwordConfirm is null right now

  // 3) Send it to user's email

  res.status(200).json({
    status: 'success',
  });
});
```

# 13 Sending Emails with Nodemailer

```sh
npm i nodemailer
```

**in env file**
```js
// Mailtrap - the service to fake to send emails

EMAIL_HOST = smtp.mailtrap.io
EMAIL_PORT = 25
EMAIL_USERNAME = d3bc8dc5c92dfe
EMAIL_PASSWORD = 2b0536836403f4
```

**in utils/email.js**
```js
const nodemailer = require('nodemailer');
const catchAsync = require('../utils/catchAsync');

const sendEmail = catchAsync(async options => {
  // 1 Create a transporter

  const transporter = nodemailer.createTransport({
    host: process.env.EMAIL_HOST,
    port: process.env.EMAIL_PORT,
    auth: {
      user: process.env.EMAIL_USERNAME,
      pass: process.env.EMAIL_PASSWORD
    }
  });

  // 2 Define the email options
  const mailOptions = {
    from: 'Bastian Nguyen <hello@natour.io>',
    to: `${options.name} <${options.email}>`,
    subject: options.subject,
    text: options.message
    //html:
  };

  // 3 Actually send the email
  await transporter.sendMail(mailOptions);
});

module.exports = sendEmail;

```

**in authController.js**
```js
// 3) Send it to user's email

  const resetURL = `${req.protocol}://${req.get(
    'host'
  )}/api/v1/users/resetPassword/${resetToken}`;

  const message = `Forgot your password? Submit a PATCH request with your new password and passwordConfirm to: ${resetURL}.\nIf you didn't forget your password, please ignore this email!`;

  try {
    await sendEmail({
      name: user.name,
      email: user.email,
      subject: 'Your password reset token (valid for 10 min)',
      message
    });
  } catch (err) {
    user.passwordResetToken = undefined;
    user.passwordResetExpires = undefined;
    await user.save({ validateBeforeSave: false });
// need try catch to specify the action after error : clear the reset password data on DB


    return next(
      new AppError(
        'There was an error on sending email. Try again later!!!',
        500
      )
    );
  }

  res.status(200).json({
    status: 'success',
    message: 'Token sent to email!!!'
  });
```

# 14 Password Reset Functionality Setting New Password

**in authController.js**
```js
exports.resetPassword = catchAsync(async (req, res, next) => {
  // 1 Get user based on the token

  //encrypt the plain token user provided
  const hashedToken = crypto
    .createHash('sha256')
    .update(req.params.token)
    .digest('hex');

  // find the user on db
  const user = await User.findOne({
    passwordResetToken: hashedToken,
    passwordResetExpires: { $gt: Date.now() } // check token has not expired
  });
  // 2 if token has not expired and there is user on db, set the new password

  if (!user) next(new AppError('Token is invalid or has expired', 400));

  user.password = req.body.password;
  user.passwordConfirm = req.body.passwordConfirm;
  user.passwordResetToken = undefined; // reset
  user.passwordResetExpires = undefined; // reset

  await user.save(); // have to user save() to run validator !!!

  // 3 Update changedPasswordAt property for the user
  // do in userModel.js

  // 4 Log the user in, send JWT
  const token = signToken(user._id);

  res.status(200).json({
    status: 'success',
    token
  });
});
```

**In userModel.js**
```js
// Update changedPasswordAt property for the user
userSchema.pre('save', function(next) {
  if (!this.isModified('password') || this.isNew) return next();

  this.passwordChangedAt = Date.now();
  next();
});
```

## 15 Update the current user Password

## 16 Update the current user Data

-   // 1 Create error if user POSTs password data

-   // 2 Filtered out unwanted fields names that are not allowed to be updated
    -        // - Check the valid of input field but Bypass the password validation
    -        // - Not allow change role of user
            
-   // 3 Update user data
    - There are 2 ways to update data to db
    - 1: using `req.user.save({ validateBeforeSave: false })`
        - this way may create an wrong data
    - 2: using User.findByIdAndUpdate
    ```
    User.findByIdAndUpdate(req.user.id, req.body, {
        runValidators: true
      });
    ```
    
