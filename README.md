# 🌴 Vacation Tracking System Design and Analysis

This repository contains the analysis and design for the **Vacation Tracking System** based on the book **"Object-Oriented Analysis and Design with Applications"**. The project includes requirements gathering, domain analysis, and system modeling using diagrams, flowcharts, and pseudocode.

---

## 📖 Table of Contents

- [📋 Requirements](#-requirements)
  - [Vision](#vision)
  - [Functional Requirements](#functional-requirements)
  - [Non-Functional Requirements](#non-functional-requirements)
  - [Constraints](#constraints)
- [🌐 Domain Definition](#-domain-definition)
- [🎭 Actors](#-actors)
- [🛠️ Use-Cases](#️-use-cases)
  - [Manage Time](#manage-time)
  - [Entities (Data Model)](#entities-data-model)
  - [Main Flow](#main-flow)
  - [Withdraw Flow](#withdraw-flow)
  - [Edit Flow](#edit-flow)
  - [Cancel Flow](#cancel-flow)
  - [State Machine Diagram](#state-machine-diagram)

- [📂 Resources](#-resources)
- [📜 License](#-license)

---

## 📋 Requirements

### Vision
The Vacation Tracking System (VTS) is designed to empower employees by allowing them to manage their vacation, sick leave, and personal time off directly through a web application. This system aims to:
- Simplify HR department processes.
- Minimize managerial overhead for non-core activities.
- Enhance employee autonomy and satisfaction.

### Functional Requirements
1. **Leave Request**
   - Apply to take a leave.
   - Withdraw my request (before completing).
   - Update my request (after completing).
   - Cancel my request (after completing).
   - Managers approve the request.
   - Managers reject the request.
   - Provides access to requests for the previous calendar year(History), and allows requests to be made up to a year and a half in the future.
2. **Notification**
   - Email to manager to approve the request.
   - Email to employee to know the status of the request.
3. **Logs**
   - Keeps activity logs for all transactions
4. **Controls**
   - Allows managers to directly award personal leave time (with system-set limits)
   - Enables the HR and system administration personnel to override all actions restricted by rules, with logging of those overrides
5. **Integratins & Interfaces**
   - Integration with existing intranet and single sign-on mechanisms.
   - Web service interface for internal systems to query vacation data
  
### Non-Functional Requirements
1. Scalability to handle a large number of employees and transactions.
2. Security for authentication, authorization, and sensitive data handling.
3. Performance to ensure quick request processing.
4. Compatibility with existing HR systems and intranet infrastructure

### Constraints
1. Uses existing hardware and middleware
2. Integrate with existing intranet portal system
3. uses the portal’s single-sign-on mechanisms for all authentication
4. Follow privacy and regulations of the company

---

## 🌐 Domain Definition
1. Inefficiency in Manual Processes:
   - Manual vacation tracking leads to errors and delays.
   - Overhead for HR departments and managers in handling leave requests.
2. Lack of Transparency:
   - Employees lack visibility into their vacation balances and status of their requests.
   - Managers lack centralized access to team vacation schedules.
3. Complex Rule Management:
   - Companies often have different rules for vacation eligibility, carryover limits, and approval processes.
   - Manual enforcement of these rules is error-prone.
4. Scattered Systems:
   - Existing solutions are often fragmented, requiring employees and managers to rely on multiple tools or manual communication.

---

## 🎭 Actors
1. **Employee**: The main user of this system. An employee uses this system to manage his or her vacation time.
2. **Manager**: An employee who has all the abilities and goals of a regular employee, but with the added responsibility of approving vacation requests for immediate subordinates. A manager may award subordinates comp time, subject to certain limits set in the system.
3. **Clerk**: A member of the HR department who has sufficient rights to view employees’ personal data and is responsible for ensuring that employees’ information in all HR systems is up to date and correct. An HR clerk can add or remove nearly any record in the system. In the real world, HR clerks may or may not be employees; however, if they are employees, they use two separate login IDs to manage these two different roles.
4. **System Admin**: A role responsible for the smooth running of the sys- tem’s technical resources (e.g., Web server, database) and for collecting and archiving all log files.

---

## 🛠️ Use-Cases

### Manage Time
We hvae different use cases(listed below) but will work on Manage Time one

- [x] Manage Time: Describes how employees request and view vacation time requests.
- [ ] Approve Request: Describes how a manager responds to a subordi- nate’s request for vacation time.
- [ ] Award Time: Describes how a manager can award a subordinate extra leave time (comp time).
- [ ] EditEmployeeRecord:DescribeshowanHRclerkeditsanemployee’s information in the system. This includes setting all the leave time allow- ances and the maximum time that can be awarded by the manager.
- [ ] Manage Locations: Describes how an HR clerk manages location records and their rules.
- [ ] Manage Leave Categories: Describes how an HR clerk manages leave categories and their rules.
- [ ] Override Leave Records: Describes how an HR clerk may override any rejection of leave time requests made by the rules in the system.
- [ ] Back Up System Logs: Describes how the system administrator backs up the system’s logs.

**Main Use Case: Manage Time**
1. Actor: Employee.
2. Goal: Submit vacation requests.
3. Preconditions: The employee is authenticated by the portal framework and identified as an employee of the company with privileges to manage his or her own vacation time.
4. Flows:
   - Main Flow: Employee selects and submits a vacation request.
   - Alternate Flows:
     - Withdraw a pending request.
     - Edit a pending request.
     - Cancel an approved request​.


### Entities (Data Model)
![data model](https://github.com/user-attachments/assets/fc82587c-41de-4563-a208-56103ace0eca)

### Main Flow
- Flowchart
  ![Manage Time Use Case](https://github.com/user-attachments/assets/f33524df-09f7-406a-a7db-ccfa377378df)

- Sequence Diagram
  ![Create Leave Request](https://github.com/user-attachments/assets/f1431f5c-775e-46c4-9b30-812f08fd549f)

- Pseudocode
  ```
  Request createRequest(RequestBody req){
      if(validateRequest(req)){
         savedReq = saveRequest(req);
          notifyManagerIfRequeriedManagerApproval();
          return savedReq;
      }
      throw BadRequestError;
  }

  boolean validateRequest(req){
    return validateRequestToAnddFrom(req.to , req.from) && validateCategoryBalance(req.categoryId , employeeId , reqBalance);
  }
  
  function validateRequestToAnddFrom(req.to , req.from){
    if(from > to && req.reqDate < from)
        return false;
    return true;
  }
  
  function validateCategoryBalance(req.categoryId , employeeId , reqBalance){
    Long empBalance = getEmployeeBalance(empId , categoryId)
    if(empBlanace > reqBalance)
        return true;
    return false;
  }
  
  getEmployeeBalance(empId , categoryId){
    API integration HR legacy system
  }
  ```
  
### Withdraw Flow
- Flowchart
  <p align="left">
    <img src="https://github.com/user-attachments/assets/df3d0956-9024-4eec-9ee2-f8a0a94578a6" />
  </p>


- Sequence Diagram
  ![Withdraw Req](https://github.com/user-attachments/assets/657b5380-b715-457c-947a-f81cf2bc40ca)

- Pseudocode
   ```
   boolean withdrawRequest(reqId){
      if(confirmWithdraw()){
          removeRequestFromManagerPendingReqList(reqId);
          updateStatusOfRequest(reqId,WITHDRAWN);
          notifyManagerIfRequeriedManagerApproval();

       return true;
      }
      return  false;
  }
  ```

### Edit Flow
- Flowchart
  <p align="left">
    <img src="https://github.com/user-attachments/assets/d54cbeeb-e028-48a9-9442-d8fe960bb05f" />
  </p>

- Sequence Diagram
  ![Edit Req drawio](https://github.com/user-attachments/assets/d6d6c5dd-743d-47a5-94e9-fdc22f0945f0)

- Pseudocode
   ```
  Request editRequest(RequestBody req){
      if(validateData(req)){
         updatedReq = updateRequest(req);
          return updatedReq;
      }
      throw BadRequestError;
  }
  ```

### Cancel Flow
- Flowchart
  <p align="left">
    <img src="https://github.com/user-attachments/assets/03e41031-1782-4323-8646-ca2749850b3d" />
  </p>

- Sequence Diagram
  ![Cancel Req drawio](https://github.com/user-attachments/assets/98f97503-6414-4757-8f0a-9525f5ed3663)

- Pseudocode
   ```
  Request createRequest(RequestBody req){
      if(confirmCancel(req)){
         updateEmployeeBalance(req.reqBalance);
          updateStatusOfRequest(reqId,CANCEL);
          notifyManagerIfRequeriedManagerApproval();
      }
      throw AbortError;
  }
  ```

### State Machine Diagram
![State Machine Diagram](https://github.com/user-attachments/assets/5bcb0358-7a7f-4a19-a5d3-13c6dbe6dd49)

---

## 📂 Resources

- **Chapter 12**: [Vacation Tracking System Requirements](#link-to-chapter-12-if-available)
- **Chapter 5**: [Diagram Notation Reference](#link-to-chapter-5-if-available)
- **External Resources**: [Google Search](https://www.google.com) for diagram references.

---

## 📜 License

This repository is licensed under the [MIT License](LICENSE).

---

💡 **Pro Tip**: Use the navigation links above for quick access to each part of the repository!

