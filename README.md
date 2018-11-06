# Assignment 1 - API testing and Source Control.

Name: Ying Yan

## Overview.

This is an online accounting web app, and there are three models for the time being（users,expenditures and incomes models）.Each model corresponds to a file in routes.
Users model has the following properties: username(unique)/password/profile(has 3 nested properties:gender/email/phone);
Expenditures model has the following properties:username(unique)/date/payment/amount/description;
Incomes model has the following properties:username(unique)/date/incomingmode/amount/description;
Each file in routes includes some basic CRUD operations and fuzzy search, etc.

## API endpoints.

Model: users
 + GET /users - Get all users.
 + GET /users/:id/profile - Get one user's profile by id(include 'username' and 'password', but these two properties are unwritable).
 + GET /users/:username/info - Get one user's information by username.
 + GET /users/specificex/:username - Get all expenditure records with this username.
 + GET /users/specificin/:username - Get all income records with this username.
 + POST /users - Add new user basic information(just 'username' and 'password').
 + PUT /users/:id/addProfile - Update one existing user's profile(find this user by id).
 + DELETE /users/:username - Delete user by username.


Model: expenditures
 + GET /expenditures - Get all expenditure records.
 + GET /expenditures/:id - Get one specific expenditure record by id.
 + GET /expenditures/monthrecord/:date - Get all expenditure records in one month(fuzzy searching date).
 + GET /expenditures/inamountorder - Get all expenditure records in ascending order of amount.
 + GET /expenditures/indateorder - Get all expenditure records in ascending order of date.
 + GET /expenditures/tamounts - Get gross expenditure.
 + GET /expenditures/fuzzy/:description - Get expenditure records that matching the fuzzy description.
 + GET /expenditures/daterecord/:date - Get expenditure records of one date.
 + GET /expenditures/gettotal/:date - Get gross expenditure of one month.
 + POST /expenditures - Add new expenditure records.
 + PUT /expenditures/:id/changeExinfo - Update any attribute values of one expenditure record except value of 'username'(find record by id).
 + DELETE /expenditures/:id - Delete one expenditure record by id.
 + DELETE /expenditures/fuzzydelete/:date - Delete expenditure records of some dates(find records by fuzzy searching date).


Model: incomes
 + GET /incomes - Get all income records.
 + GET /incomes/:id - Get one specific record by id.
 + GET /incomes/monthrecord/:date - Get income records of one month(fuzzy searching date).
 + GET /incomes/inamountorder - Get all income records in ascending order of amount.
 + GET /incomes/indateorder - Get all income records in ascending order of date.
 + GET /incomes/tamounts - Get total revenue.
 + GET /incomes/fuzzy/:description - Get income records that matching the fuzzy description.
 + GET /incomes/daterecord/:date - Get income records of one date.
 + GET /incomes/monthamount/:date - Get total revenue of one month(find records by fuzzy searching date).
 + POST /incomes - Add new income records.
 + PUT /incomes/:id/changeIninfo - Update any attribute values of one income record except value of 'username'(find record by id).
 + DELETE /incomes/:id - Delete one income record by id.
 + DELETE /incomes/fuzzy/:incomingmode - Delete income records with some incomingmodes(find records by fuzzy searching incomingmode).





## Data storage.

My tests included the integration of MongoDB.(db name:"gullin",it has 3 collections:expendituredb/incomedb/userdb)

1. expendituredb:
   1) database schema
       let ExpenditureSchema = new mongoose.Schema({
           username:String,
           date: String,
           payment:String,
           amount: Number,
           description:  {type: String, default:null}
       },
       { collection: 'expendituredb' });
   2) json document structure example
       {
         "_id": {
          "$oid": "5bdd7ec0ef72153750b2df72"
       },
         "description": "Latte",
         "username": "gullveig",
         "date": "2018-10-15",
         "payment": "Alipay",
         "amount": 4,
         "__v": 0
       }

2. incomedb:
   1) database schema
       let IncomeSchema = new mongoose.Schema({
           username:String,
           date: String,
           incomingmode:String,
           amount: Number,
           description:  {type: String, default:null}
       },
       { collection: 'incomedb' });
   2) json document structure example
       {
         "_id": {
           "$oid": "5bda4402467b3521a4b4a3a9"
       },
         "description": "benefits",
         "username": "gullveig",
         "date": "2018-10-01",
         "incomingmode": "Alipay",
         "amount": 78,
         "__v": 0
       }

3. userdb:
   1) database schema
       let UserSchema = new mongoose.Schema({
           username: { type:String, unique:true, required: true, writable: false},
           password: { type:String, unique:true, required: true, writable: false,},
           profile:{ gender: String,
                     email: String,
                     phone: String
        },
       },
       { collection: 'userdb' });
   2) json document structure example
       {
         "_id": {
            "$oid": "5bda48a9467b3521a4b4a3b5"
         },
         "profile": {
         "gender": "female",
         "email": "1804094745@qq.com",
         "phone": "13046529705"
         },
         "username": "gullveig",
         "password": "qwert1997",
         "__v": 0
       }




## Sample Test execution.


 > Users\dell\donationwebs\accountingtestings
 > mocha -t 50000 test/routes/expenditure-test.js
            Expenditures
                POST /expenditures
            Successfully Connected to [ gullin ]
            Successfully Connected to [ gullin ]
            Successfully Connected to [ gullin ]
            (node:9816) DeprecationWarning: collection.ensureIndex is deprecated. Use createIndexes instead.
                    √ should return confirmation message and update database (1721ms)
                PUT /expenditures/:id/changeExinfo
                  when id is valid
                    √ should return a confirmation message and update database (292ms)
                  when id is invalid
                    √ should return a 404 and a message for invalid expenditure id
                DELETE /expenditures/:id
                  when id is valid
            (node:9816) DeprecationWarning: collection.findAndModify is deprecated. Use findOneAndUpdate, findOneAndReplace or findOneAndDelete instead.
                    √ should return a confirmation message and update database  (295ms)
                  when id is invalid
                    √ should return a 404 and a message for invalid expenditure id
                DELETE /expenditures/fuzzydelete/:date
            (node:9816) DeprecationWarning: collection.remove is deprecated. Use deleteOne, deleteMany, or bulkWrite instead.
                    √ should return a confirmation message and update database  (148ms)
                GET /expenditures
                    √ should return all the expenditures in an array (160ms)
                GET /expenditures/:id
                    √ should return a specific expenditure in an array (144ms)
                    √ should return a 404 and a message for invalid expenditure id
                GET /expenditures/daterecord/:date
                    √ should return a specific expenditure of one date in an array (150ms)
                    √ should return a message for daterecord which do not exist (143ms)
                GET /expenditures/inamountorder
                    √ should return all the expenditures in ascending order of amount in an array (150ms)
                GET /expenditures/indateorder
                    √ should return all the expenditures in ascending order of date in an array (151ms)
                GET /expenditures/monthrecord/:date
                    √ should return expenditure records of one month in ascending order of date in an array (148ms)
                    √ should return a message for monthlyrecord cannot found (142ms)
                GET /expenditures/fuzzy/:description
                    √ should return relevant expenditure records matching the fuzzy description in an array (154ms)
                    √ should return a message for no relevant records (146ms)
                GET /expenditures/tamounts
                    √ should return the total amounts of expenditures in an array (151ms)
                GET /expenditures/gettotal/:date
                    √ should return total amounts of expenditure of one month in an array (153ms)
                    √ should return a message for no relevant expenditure records (145ms)


                20 passing (6s)






 > Users\dell\donationwebs\accountingtestings
 > mocha -t 50000 test/routes/users-test.js
            Users
                POST /users
            Successfully Connected to [ gullin ]
            Successfully Connected to [ gullin ]
            Successfully Connected to [ gullin ]
            (node:7012) DeprecationWarning: collection.ensureIndex is deprecated. Use createIndexes instead.
                    √ should return confirmation message and update database (1588ms)
                PUT /users/:id/addProfile
                  when id is valid
                    √ should return a message and the information of this user updated (307ms)
                  when id is invalid
                    √ should return a 404 and a message for invalid user id
                DELETE /users/:username
            (node:7012) DeprecationWarning: collection.remove is deprecated. Use deleteOne, deleteMany, or bulkWrite instead.
                    √ should return a confirmation message and update database  (148ms)
                GET /users
                    √ should return all the users in an array (156ms)
                GET /users/:username/info
                    √ should return a specific user matching the username in an array (151ms)
                    √ should return a message for username that does not exist (164ms)
                GET /users/:id/profile
                    √ should return profile info of a user in an array (150ms)
                    √ should return a 404 and a message for invalid user id
                GET /users/specificex/:username
                    √ should return all the expenditure records of one user in an array (147ms)
                    √ should return a message for user has no expenditure records (153ms)
                GET /users/specificin/:username
                    √ should return all the income records of one user in an array (163ms)
                    √ should return a message for user who has no income records (163ms)


                13 passing (5s)







 > Users\dell\donationwebs\accountingtestings
 > mocha -t 50000 test/routes/income-test.js
            Incomes
                GET /incomes
            Successfully Connected to [ gullin ]
            Successfully Connected to [ gullin ]
            Successfully Connected to [ gullin ]
            (node:18376) DeprecationWarning: collection.ensureIndex is deprecated. Use createIndexes instead.
                    √ should return all the income records in an array (1640ms)
                GET /incomes/:id
                    √ should return a specific income record in an array (149ms)
                    √ should return a 404 and a message for invalid income id
                GET /incomes/daterecord/:date
                    √ should return specific income records of one date in an array (145ms)
                    √ should return a message for daterecord which do not exist (147ms)
                GET /incomes/inamountorder
                    √ should return all the income records in ascending order of amount in an array (169ms)
                GET /incomes/indateorder
                    √ should return all the income records in ascending order of date in an array (154ms)
                GET /incomes/monthrecord/:date
                    √ should return income records of one month in ascending order of date in an array (154ms)
                    √ should return a message for monthlyrecord cannot found (145ms)
                GET /incomes/fuzzy/:description
                    √ should return relevant income records matching the fuzzy description in an array (146ms)
                    √ should return a message for no relevant records (153ms)
                GET /incomes/tamounts
                    √ should return the total amounts of income records in an array (152ms)
                GET /incomes/gettotal/:date
                    √ should return total amounts of income of one month in an array (151ms)
                    √ should return a message for no relevant income records (147ms)
                POST /incomes
                    √ should return confirmation message and update database (178ms)
                PUT /incomes/:id/changeIninfo
                  when id is valid
                    √ should return a confirmation message and update database (305ms)
                  when id is invalid
                    √ should return a 404 and a message for invalid income id
                DELETE /incomes/:id
                  when id is valid
            (node:18376) DeprecationWarning: collection.findAndModify is deprecated. Use findOneAndUpdate, findOneAndReplace or findOneAndDelete instead.
                    √ should return a confirmation message and update database  (312ms)
                  when id is invalid
                    √ should return a 404 and a message for invalid income id
                DELETE /incomes/fuzzy/incomingmode
            (node:18376) DeprecationWarning: collection.remove is deprecated. Use deleteOne, deleteMany, or bulkWrite instead.
                    √ should return a confirmation message and update database  (148ms)


                20 passing (6s)

[ Markdown Tip: By indenting the above listing, GitHub will display it in a 'box' and preserve any formatting.]

## Extra features.

1. Before writing test code, I connected the node server to a background database dedicated to testing,so my test code runs on the basis of the test database, and it would not cause any changes to the original database.
2. I implemented test-isolation,so each test file could be repeatedly run as a whole.
3. I have committed the put operation test in income-test.js as a work unit ,because at that time I implemented the test in two steps and commit 2 messages on that branch.
4. I have rolled back the code several times to a previous version(git reset --hard ........).
