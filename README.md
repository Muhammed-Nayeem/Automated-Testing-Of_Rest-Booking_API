## REST Booking API Automation-Testing with Postman & Newman

The purpose of this project is to use Postman and Newman to automate the testing of a REST booking API. I enable consistent and dependable validation of the API endpoints by automating the testing process, which minimizes manual labor and allows for quicker feedback on modifications.

### Features
---
- Tested the diverse <span style="color: black; font-weight: bold;">HTTP</span> methods including <span style="color: green; font-weight: bold;">GET</span>, <span style="color: yellow; font-weight: bold;">POST</span>, <span style="color: skyblue; font-weight: bold;">PUT</span>, <span style="color: purple; font-weight: bold;">PATCH</span>, and <span style="color: crimson; font-weight: bold;">DELETE</span>.
- Collection of tests covering with different API endpoints.
- Environment setup for easy switching between environments.
- Pre-request scripts for data setup.
- Test scripts for assertions and validations.

### API Documentation
---
- `Documentation Link :` [API Documentation - Click here to open documentation](https://documenter.getpostman.com/view/29174005/2sA35A95nd)

### Tools and Technologies
---
- Postman
- Newman
- JavaScript
- JSON

### Prerequisite
---
- Node.Js
- Newman
- Newman HTML Reporter Library

### Installation
---
1. `Node.Js :` [Download and Install Node.js on your machine from here](https://nodejs.org/en/download)
2. `Postman :` [Download and Install Postman on your machine from here](https://www.postman.com/downloads/)
3. `Clone This Repository :` 
```console
git clone https://github.com/Muhammed-Nayeem/Automated-Testing-Of_Rest-Booking_API.git
```
4. `Import the Postman Collection :`  

    - Open Postman.
    - Click on the Import button.
    - Select the file from the repository.

5. `Import the Postman environment :`  

    - In Postman, click on the gear icon in the top right corner.
    - Select Import and choose the file.

6. `Newman and Report Installation Process :`  

    - Newman Install Command
    ```console
    npm install -g newman
    ```
    - Newman Html Report Install Command
    ```console
    npm install -g newman-reporter-htmlextra
    ```

### Usage
---
- `Select Environment :`

    - In Postman, select the appropriate environment (e.g., Development, Production) from the top-right dropdown.

- `Run Collection :`
    
    - Select the imported collection from the Collections sidebar.
    - Click on the Runner button to open the collection runner.
    - Select the desired environment.
    - Click Start Test to run the collection.

- `View Results :`

    - Once the tests are complete, view the results in the Runner tab.
    - Detailed test results can be viewed for each request.

### Testing and Test Case Scenarios
---
1. **Create Booking**   
    - `Request URL :` https://restful-booker.herokuapp.com/booking/
    - `Request Method :` <span style="color: yellow; font-weight: bold;">POST</span>
    - **Pre-request Script :**
    ```js
      //Prepared Predefined random Data Set:
      let firstName = pm.variables.replaceIn("{{$randomFirstName}}");
      let lastName = pm.variables.replaceIn("{{$randomLastName}}");
      let totalPrice = pm.variables.replaceIn("{{$randomPrice}}");
      let depositPaid = pm.variables.replaceIn("{{$randomBoolean}}");
      let additionalNeeds = pm.variables.replaceIn("{{$randomProductName}}");

      //Date:
      const date = require("moment");
      const today = date();
      let checkIn = today.add(3, "d").format("YYYY-MM-DD");
      let checkOut = today.add(4, "d").format("YYYY-MM-DD");

      //Store Variables in Environment:
      pm.environment.set("firstName", firstName);
      pm.environment.set("lastName", lastName);
      pm.environment.set("totalPrice", parseInt(totalPrice));
      pm.environment.set("depositPaid", depositPaid);
      pm.environment.set("checkIn", checkIn);
      pm.environment.set("checkOut", checkOut);
      pm.environment.set("additionalNeeds", additionalNeeds);
    ```  
    - **Request Body :**
    ```json
      {
        "firstname": "{{firstName}}",
        "lastname": "{{lastName}}",
        "totalprice": {{totalPrice}},
        "depositpaid": {{depositPaid}},
        "bookingdates": {
            "checkin": "{{checkIn}}",
            "checkout": "{{checkOut}}"
        },
        "additionalneeds": "{{additionalNeeds}}"
      }
    ```  
    - **Tests :**
    ```js
      //Get the response code:
      let responseStatusCode = pm.response.code;

      //If the response code is 200, then do the certian task:
      switch(responseStatusCode) {
        case 200:
          let responseData = pm.response.json();
          pm.environment.set("bookingId", responseData.bookingid);
          pm.test(`Veryfying that Booking Creation is successfull of id-${pm.environment.get("bookingId")}`);
          break;
      
        default:
          pm.test(`Booking Dosen't Created Successfully!`);
      }
    ```
    - **Response Body :**
    ```json
      {
      "bookingid": 469,
      "booking": {
          "firstname": "Seth",
          "lastname": "Howell",
          "totalprice": 125,
          "depositpaid": false,
          "bookingdates": {
              "checkin": "2024-03-25",
              "checkout": "2024-03-29"
          },
          "additionalneeds": "Refined Steel Hat"
        }
      }
    ```