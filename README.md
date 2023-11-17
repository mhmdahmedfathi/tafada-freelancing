# tafada
this is readme to describe what I have done as a freelancer in tafada project

in this readme, I will sammury most of what I have done

## Backend

tree structure 
├── controllers
│   ├── authController.js
│   ├── dashboardController.js
│   └── plaidController.js
├── middlewares
│   └── auth.middleware.js
├── models
│   ├── cardModel.js
│   ├── otpModel.js
│   └── userModel.js
├── routes
│   ├── auth.js
│   ├── dashboard.js
│   └── plaid.js
├── uploads
│   └── note.txt
├── utils
│   ├── constants
│   │   └── api-constants.js
│   └── helpers
│       └── helpers.js
├── package.json
├── package-lock.json
├── pin.txt
├── pnpm-lock.yaml
├── README.md
└── server.js


Models 

- user

- card

- otp which is needed to save the temporary otps for the user till they check them and then we can remove the temporary otp and   save it in the user model


** Note that I used MVC structure where 
    - models contain the db tables
    - routes contain endpoints
    - controllers contain functions of the endpoints
    - middlewares act as a layer of the secuirty as it cames before the function of the contoller to 
      check the token of the user and get the user from this token
    - uploads contain the images and pdf that the user will upload
    - utils contain the functions or constants that is shared between files


there are some functions or lines that are commented and written on them that they can be user for the testing, you can uncomment them but takecare as maybe you have to change the code in somewhere else as 

// // Testing proposal only
// const request = {
//   access_token: user.info.connected.credential.access_token,
//   webhook_code: 'RECURRING_TRANSACTIONS_UPDATE'
// };
// try {
//   const response = await client.sandboxItemFireWebhook(request);
// } catch (error) {
//   // handle error
//   console.log(error);
// }

starting from line 308 in plaidcontroller.

it is used to mock the webhook that the plaid used to send the change of status as plaid doesn't provide this feature (sending webhook requests to see the change in status) in sandbox so this function mock it, but to use it, you have to use live server and provide its url in line 30 (webhook value) ( change the first part only) 



Main New files :

## Backend : 

### controllers

#### - authController.js contain 2 main functions which are sendOTP and checkOTP

######  --- sendOTP -> is the function which get the phone and new_code boolean(which we use to check if the user wants to login using his code or wants new code) and in this function 
######   ---- generateOTP which generate random code contain number+small letter+ capital letters + special char 

######   check the phone to see if the user is existing on the db or not to send his name the message and then send the message using textbelt api and check if the response is OK then create new object in the OTP model and return status true to the front end else it will return failed to semd SMS error


######  --- checkOTP -> is the function which get the phone and new_code boolean and otp that the user written and then check new code bool

###### ---- if it was true then the user wants to change his OTP so it will find the new otp and compare them to see if they are the same and if not it will return error, else it will continue

###### ---- if it was false then the user wants to login using the existing code, so it checks the existing otp code in the model and if there is no user with this otp then it will return error saved otp is not correct, else it will continue

###### after that if the user found then it will set new token to the user using jwt and update the otp if the new code is true else it will just send OK status with the new token to the user

###### if the user is not found then it will create new user and made new token to him and delete the otp from the OTP model and send created status to the user 

#### dashboardController.js contains 6 main functions which are addInfo, myAccount, uploadImage, createStripeCharge, checkStripeCharge and getTransactions

##### - addInfo -> add info about the user when he edits  his account

##### - uploadImage -> add/edit image of the user that he uploads from his account

##### - myAccount -> get the info of the user and return it back to the front ( to save his session until he logs out)

##### - createStripeCharge -> get the id of the card that the user presses it and return the link which the user will redirect to it

##### - checkStripeCharge -> check if the user complete his payment 

##### - getTransactions -> return the transactions of the user which will appear in the transaction page


#### plaidController.js contains the configuration of the plaid client and secret (don't forget to change it to be the real secret not the sandbox) and it also contains 6 main functions which are linkToken,exchange_public_token, transferAmount, requestLimit, dashboard, webhookTransfer

##### - linkToken -> get the token for the user so he can log into his back account (don't forget to change the webhook to the real endpoint)

##### - exchange_public_token -> after the user logs into his account, this will exchange his public token to access token which will can used  

##### - transferAmount -> this is comman function that is used in more than one situation which will init transfer authority and then create transfer for the user

##### - requestLimit -> this is the function where the user asks for a limit so it made every check it wants and create the transfer and made recurring transfer 

##### - dashboard -> return the analytics of the user so it can appear in the dashboard

##### - webhookTransfer ->  when the user pays his recurring transfer, it will add the amount of the payment to the db and change his score


### utils/helpers 

#### - change score -> this function changes the score according to the process of the user and calculate the new score and save it
