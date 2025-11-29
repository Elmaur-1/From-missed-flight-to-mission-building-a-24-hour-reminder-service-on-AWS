##  From missed flight to mission: building a 24-hour reminder service on AWS


A few months ago, my mom missed her flight. She’d been swamped all week, errands, and work. She thought the flight was later, got to the airport, it was too late her flight had gone. When she told me, I felt that gut drop of *“we could’ve avoided this.”* 

 


This wasn’t just about building a reminder service. It was about my journey into DevOps learning AWS resources one by one, connecting them, and seeing how cloud skills can solve everyday problems.



From a missed flight to a working solution, this project shows that even small ideas can become powerful learning experiences.
That one moment turned into this project.



## 🛠️ stack
- **Data:** Appointments saved in DynamoDB (name, date, time).  
- **Compute:** AWS Lambda scans appointments and decides who needs reminders.  
- **Notify:** Amazon SNS sends email/SMS notifications. 



##  Step-by-Step Walkthrough


### Create the DynamoDB Table

- **Table name:** `Appointment` 

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/tkutd7m850e4lsd0z8ag.png)

 
- **Primary key:** Partition key = `Name` (String)  
- **Attributes:**  
  - `Name`: e.g., “Dentist Visit”  
  - `Date`: ISO string, e.g., “2025-12-29” 
 
   
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ifnwwg6y1a1kc3l9mzq4.png)



### 2. Create the SNS Topic and Subscribe

- **Topic name:** `AppointmentReminders` (Standard) 

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/58ldbzzi4gl6y761hyp7.png)


- Copy the **Topic ARN** — you’ll need this for Lambda.  
- Add a subscription:  
  - Protocol: Email (or SMS)  
  - Endpoint: Your email address  
- Confirm the subscription via the AWS email link.  


## Create the Lambda Function (Node.js)



![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/dueeim8gwpv8di52oiek.png)



- Runtime:** Node.js 18.x  
- Handler:** `index.handler`  
- File name:** `index.js



![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0kpecryj3gayjqvksmus.png)


## Give Lambda the Right IAM Permissions


Attach a minimal inline policy to your Lambda’s execution role:



![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/dalc45h1dbzpzb9f77f6.png)



** choosing the specific action that the resources in DynamoDB will need.


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/yhwh7v186b9dndoyefh3.png)

 ** And for SNS


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/148fv4e4vtqqrl7vaptj.png)



** After that is done give a to the policy.


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/yhw4e91ekqlvoanrxcei.png)


** Review and click create.


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/cz8ubvjiuzpvu06dtp0i.png)



## Add a test item in DynamoDB:



![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zcz5u6s8byd2c8lol794.png)


## Run Lambda → Test event


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/13v7k8clp98hyes1l4l7.png)



## **Result** I tested with a dentist appointment and received an email reminder no more last-minute panics.   .


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0hjmr3n540fy5wufzz6h.png)



## 🚀 what I learned




+-----------------------+           +---------------------------+
|   User adds item      |           |   EventBridge (schedule)  |
|  (appointment input)  |           |       rate(1 day)         |
+-----------+-----------+           +-----------+---------------+
            |                                   |
            v                                   v
+-----------+-----------+           +-----------+---------------+
|  DynamoDB (Appointment) | <-----> |   Lambda function         |
|  Name, Date, Time       |         |  - Scan upcoming items    |
|                         |         |  - Filter within 24 hrs   |
+-----------+-----------+-+         |  - Publish reminders      |
                        |           +-----------+---------------+
                        |                       |
                        v                       v
              +---------+-----------+   +-------+----------------+
              |   SNS Topic         |   | Email/SMS Subscribers  |
              | AppointmentReminders|==>| (confirmed endpoints)  |
              +---------+-----------+   +------------------------+




what this project solve Forgetting is easy; systems aren’t.
24-hour timing gives you enough time to prepare.
The missed flight hurt, but it sparked something useful: a small, dependable system that gives you a 24-hour heads up for what matters. You don’t need a huge platform just a table, a function, a topic, and a schedule.

** Cloud skills solving everyday problems.**

This is my Devops/cloud engineering journey.









