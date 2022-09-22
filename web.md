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
  { action: "reg" }
  ```

- [Login] - Redirects to Authentication Screen with the following state data

  ```json
  { action: "login" }
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
