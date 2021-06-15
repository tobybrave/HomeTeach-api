# HomeTeach-api
# HomeTeach Backend

## Project Description
  HomeTeach software provides a platform for experienced teachers in the society to register and retail their services.  

Teachers register and the organisation screens them to ensure the necessary standard are met.
Parents in search of teachers to assist their wards can subscribe, login and select their subject and are able to choose a teacher from the list of available teachers.  
Arrangements for classes are made and the subscriber will be debited at the end month end. Subscribers are able to rate/feedback teachers on the app so that future subscribers can be guided on their choice.


## Minimum Viable Product  
### Parent's Endpoints

| N  | Method | Endpoint                     | Description |
|:---: | :---: | :---: | --- |
| 1  | GET    | /                            | landing page
| 2  | POST   | /api/register                | create new user (parent) using the details provided in the request body and returns an object containing the created user  |
| 3  | POST   | /api/login                   | verifies request body against database credentials and returns a the user object with assigned token                   |
| 4  | POST   | /api/logout                  | removes token assigned to user, ends session and return null                                                            |
| 5  | GET    | /api/home                    | returns a list of user customized home view based on *** |
| 6  | GET    | /api/{user_id}               | returns an object of user information from database |
| 7  | ws     | /api/messages                | uses web socket for conversation between user and teacher |
| 8  | GET    | /api/bookmarks               | returns a list of bookmarked objects associated with user|
| 9  | PUT    | /api/bookmarks/{bookmark_id} | adds boookmark with specified id to the list of bookmarks and returns a modified list containing the newly added bookmark |
| 10 | DELETE | /api/bookmarks/{bookmark_id} | remove a bookmark with the specified id and returns a modified list excluding the bookmark id                         |
| 10 | GET    | /api/notifications           | returns list of notifications object|
| 11 | GET    | /api/search/?{query_details} | returns an array of object containing all related search results                                                         |
| 12 | GET    | /api/tutor/{tutor_id}        | returns an object of information associated with specified tutor e.g reviews, subjects id                                  |
| 13 | PUT    | /api/tutor/{tutor_id}        | subscribe or unsubscribe to/from tutor|
| 14 | POST   | /api/settings/payment   	   | process payment using information in the request body|
| 15 | PUT    | /api/settings/account   	   | updates user account with details from the request body and returns a modified object                                   |
| 16 | GET    | /api/settings/subscription   | returns list of tutor objects user is subscribed to|
| 17 | PUT    | /api/settings/subscription   | adds or removes tutor from list of tutors subscribed to and returns a modified list of tutors subscribed to             |
| 18 | POST   | /api/account/forgot-password | sends an email notification to user's email in database based on the request body deatails                              |
| 19 | POST   | /api/account/reset-password  | resets user's password , modifies user's password in database and redirects user to login page                       |
| 20 | POST   | /api/account/verify          | verifies user email or phone and authenticates user |

NB: all response are in json format, user refers to the parent and id type not ascertained yet i.e natural key or surrogate key

#### 1 [GET] /
- If request is successful:
  - respond with HTTP status code `200` (OK).
  - return the landing page.

- If there's an error in response:
  - respond with HTTP status code `500`.
  - return the following JSON: `{ error: "server is currently unavailable. Give it another try :)" }`.

#### 2 [GET] /api/register
- If all fields in request body is valid:
	- saves user to database.
  - respond with HTTP status code `201` (Created).
  - return the following JSON: `{ message: "account successfully created", user_id: "user_id", token: "somerandomgibberishgeneratedfortoken" }`.

- If any field is invalid:

  - return HTTP status code `400` (Bad Request).
  - return the following JSON: `{ error: "Provide a valid in the field" }`.

- If there's an error in response:
  - respond with HTTP status code `500` (Server Error).
  - return the following JSON: `{ error: "an error occurred while saving the user to database" }`.

#### 3 [POST] /api/login
- If login credentials are valid and correct:

  - assign token to user and log user in.
  - return HTTP status code `200` (OK).
  - - return the following JSON: `{ message: "login successful", user_id: "user_id", token: "somerandomgibberishgeneratedfortoken" }`.
  
- If any field is invalid or missing in request body:

  - respond with HTTP status code `400` (Bad Request).
  - return the following JSON: `{ error: "Credential is incorrect" }`.

- If an error occurred during login process:
  - respond with HTTP status code `500` (Server Error).
  - return the following JSON: `{ error: "an error occurred" }`.

#### 4 [POST] /api/logout

- If sign out was successful

  - responds with HTTP status code `200` (OK).
  
- If an error occurred during logout process:
  - respond with HTTP status code `500` (Server Error).
  - return the following JSON: `{ error: "an error occurred" }`.
  

### User Schema

A user in the database has the following structure:
