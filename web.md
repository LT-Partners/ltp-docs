# LTP Analytics Web Application Documentation

> This is the documentation for LTP Analytics Web Application for both Admin and Client.

Technologies Used:

- [ReactJS] - Frontend framework in JS
- [NodeJS] - Backend run time environment for JS
- [ExpressJS] - Backend framework working on NodeJS
- [Cloud SQL] - Database service by Google Cloud Platform
- [Cloud Storage] - Storage service by Google Cloud Platform
- [App Engine] - VM service by Google Cloud Platform
- [DataStudio] - Automated reports service by Google
- [BigQuery] - Big Data service for data manipulation by Google Cloud Platform

<br />

![App Flow](/_media/Web.jpg)

## Client

> Address - [Link](http://analytics.lt.partners)\
> Code Base - [Link](https://github.com/LT-Partners/Analytics-Client-FrontEnd)

### Landing Screen

![Landing Screen](/_media/landing.png)

Landing Screen has 3 options :

- [Register] - Redirects to Authentication Screen with the following state data

  ```json
  { 
    "action": "reg"
  }
  ```

- [Login] - Redirects to Authentication Screen with the following state data

  ```json
  { 
    "action": "login"
  }
  ```

- [Contact Us] - Sends email to hello@lt.partners

### Authentication Screen

#### Registration

![Registration Screen](/_media/register.png)

For registration, 4 inputs are required:

- [Full Name] - No restriction what so ever.
- [Email] - Should be a valid and active email. [Validator](https://www.npmjs.com/package/validator) is used for validating the emails. In backend RegEx Expression is used to validate the email format.

  ```js
  const re = /\S+@\S+\.\S+/;
  ```

- [Password] - [Password Validator](https://www.npmjs.com/package/password-validator) is used to check if the password given by the user complies with our rules. <br/> The rules are :
  - Minimum length = 6
  - Maximum length = 15
  - Atleast 1 UpperCase
  - Atleast 1 LowerCase
  - Atleast 1 Digit
  - Should not have space

  ```js
  let passwordSchema = new PasswordValidator();
  passwordSchema.is().min(6)
    .is().max(15)
    .has().uppercase(1)
    .has().lowercase(1)
    .has().digits(1)
    .has().not().spaces();
  ```

- [Confirm Password] - It should be same as [Password].

**CREATE MY ACCOUNT**

It calls the following API :

> POST - <https://ltpautomatedpublisherscorecard.uc.r.appspot.com/users/reg>

The request should have the following body :

```json
{
  "name": "Rishav Kumar",
  "email": "rishav@lt.partners",
  "password": "Sumon389"
}
```

Possible responses are :

- Status - **200** | Registration is successful.

  ```json
  {
    "id": 00,
    "name": "Rishav Kumar",
    "email": "rishav@lt.partners",
    "brand": "LTP Demo Brand",
  }
  ```

- Status - **500** | System errors.

  ```json
  {
    "message": "Error message."
  }
  ```

- Status - **500** | Email already exists in our servers.

  ```json
  {
    "message": "Email already in use."
  }
  ```

**Sign In**

Update's Authentication Screen state to the following data

```json
{ 
  "action": "login"
}
```

#### Login

![Login Screen](/_media/login.png)

For login, 2 inputs are required:

- [Email] - Should be an already registered and active email. [Validator](https://www.npmjs.com/package/validator) is used for validating the emails. In backend RegEx Expression is used to validate the email format.

  ```js
  const re = /\S+@\S+\.\S+/;
  ```

- [Password] - The password should be same as used while registering. [Password Validator](https://www.npmjs.com/package/password-validator) is used to check if the password given by the user complies with our rules. <br/> The rules are :
  - Minimum length = 6
  - Maximum length = 15
  - Atleast 1 UpperCase
  - Atleast 1 LowerCase
  - Atleast 1 Digit
  - Should not have space

  ```js
  let passwordSchema = new PasswordValidator();
  passwordSchema.is().min(6)
    .is().max(15)
    .has().uppercase(1)
    .has().lowercase(1)
    .has().digits(1)
    .has().not().spaces();
  ```

**LOG IN**

It calls the following API: 

> POST - <https://ltpautomatedpublisherscorecard.uc.r.appspot.com/users/login>

The request should have the following body :

```json
{
  "email": "rishav@lt.partners",
  "password": "Rishav11"
}
```

Possible responses are :

- Status - **200** | Login is successful.

  ```json
  {
    "id": 2,
    "name": "Rishav Kumar",
    "email": "rishav@lt.partners",
    "password": "Some hashed password",
    "brand": "",
    "created_at": "2022-08-14T15:14:12.000Z",
    "token": "Some token",
    "verified": 1,
    "admin_verified": 1,
    "image": "https://storage.googleapis.com/some-custom-image.png",
    "track_usage": 1,
    "team": 1
  }
  ```

- Status - **500** | Wrong Password.

  ```json
  {
    "message": "Wrong Password"
  }
  ```

- Status - **404** | User doesn't exist.

  ```json
  {
    "message": "User not found"
  }
  ```

- Status - **500** | System errors.

  ```json
  {
    "message": "Error message."
  }
  ```

**Forgot Password?**

Update's Authentication Screen state to the following data

```json
{ 
  "action": "forgot-pwd"
}
```

**Sing Up**

Update's Authentication Screen state to the following data

```json
{ 
  "action": "reg"
}
```

#### Reset Password Request

![Reset Password](/_media/reset.png)

For resetting the password, only 1 input is required:

- [Email] - Should be an already registered and active email. [Validator](https://www.npmjs.com/package/validator) is used for validating the emails. In backend RegEx Expression is used to validate the email format.

  ```js
  const re = /\S+@\S+\.\S+/;
  ```

**GET OTP**

It calls the following API

> POST - <https://ltpautomatedpublisherscorecard.uc.r.appspot.com/users/forgotpassword?email=#your_email_here#>

Once the request is completed, user is re-directed to Setting New Password by updating Authentication Screen's state to the following data

```json
{ 
  "action": "forgot-pwd-change"
}
```

#### Set New Password

![Set New Password](/_media/set-pwd.png)

For setting new password, 3 inputs are required:

- [OTP] - OTP is sent to the user's email.

- [Password] - [Password Validator](https://www.npmjs.com/package/password-validator) is used to check if the password given by the user complies with our rules. <br/> The rules are :
  - Minimum length = 6
  - Maximum length = 15
  - Atleast 1 UpperCase
  - Atleast 1 LowerCase
  - Atleast 1 Digit
  - Should not have space

  ```js
  let passwordSchema = new PasswordValidator();
  passwordSchema.is().min(6)
    .is().max(15)
    .has().uppercase(1)
    .has().lowercase(1)
    .has().digits(1)
    .has().not().spaces();
  ```

- [Confirm Password] - It should be same as [Password].

**CHANGE PASSWORD**

> POST - <https://ltpautomatedpublisherscorecard.uc.r.appspot.com/users/changepassword?email=#your_email_here#&otp=#received_otp_here#&password=#new_password_here#>

Once the request is completed, user get's a pop-up confirming password has been changed. Where on clicking on the **CONTUNUE** button, user is re-directed to Login by updating Authentication Screen's state to 

```json
{ 
  "action": "login"
}
```