# Tafada

## Overview
This README describes the work I've completed as a freelancer on the Tafada project.

In this README, I will summarize most of what I have done.

## Backend Structure

### Tree Structure
├── controllers
│ ├── authController.js
│ ├── dashboardController.js
│ └── plaidController.js
├── middlewares
│ └── auth.middleware.js
├── models
│ ├── cardModel.js
│ ├── otpModel.js
│ └── userModel.js
├── routes
│ ├── auth.js
│ ├── dashboard.js
│ └── plaid.js
├── uploads
│ └── note.txt
├── utils
│ ├── constants
│ │ └── api-constants.js
│ └── helpers
│ └── helpers.js
├── package.json
├── package-lock.json
├── pin.txt
├── pnpm-lock.yaml
├── README.md
└── server.js

### Models
- **user**
- **card**
- **otp**: This model is used to save temporary OTPs for users until they check them. After verification, the temporary OTP is removed, and it is saved in the user model.

### MVC Structure
- **Models**: Contain the database tables.
- **Routes**: Contain endpoints.
- **Controllers**: Contain functions for the endpoints.
- **Middlewares**: Act as a security layer, checking the user's token before executing the controller function.
- **Uploads**: Contain images and PDFs uploaded by users.
- **Utils**: Contain shared functions or constants.

### Important Functions and Comments
- The README provides detailed explanations of functions and comments, such as testing proposals and explanations about the use of certain code blocks.
- It mentions the use of a mock webhook function to simulate Plaid webhook requests, which is essential for testing but requires a live server.

## Main New Files

### Backend

#### Controllers
1. **authController.js**: Contains functions `sendOTP` and `checkOTP` for OTP handling.
    - `sendOTP`: Sends an OTP to the user's phone using textbelt API.
    - `checkOTP`: Verifies the OTP entered by the user.

2. **dashboardController.js**: Contains functions for managing user account information, image uploads, and Stripe transactions.
    - `addInfo`: Adds/edit user account information.
    - `uploadImage`: Manages user profile image uploads.
    - `myAccount`: Retrieves user information.
    - `createStripeCharge`: Creates a Stripe charge and returns a redirect link.
    - `checkStripeCharge`: Checks if the user has completed the payment.
    - `getTransactions`: Retrieves user transactions.

3. **plaidController.js**: Contains Plaid configuration and functions for managing Plaid integration.
    - `linkToken`: Gets a token for the user to log into their bank account.
    - `exchange_public_token`: Exchanges the user's public token for an access token.
    - `transferAmount`: Initiates and creates a transfer for the user.
    - `requestLimit`: Handles user requests for a limit, creates transfers, and initiates recurring transfers.
    - `dashboard`: Retrieves user analytics for the dashboard.
    - `webhookTransfer`: Manages user score changes when payments are made.

#### Utils/Helpers
- `changeScore`: A function that changes the user's score based on their activity and updates it in the database.
