


https://github.com/user-attachments/assets/5fb4dd88-8e0c-43a9-80d0-d47bac3d767c


Create Objects from Spreadsheet  
1. Create Objects from Spreadsheet  
a. Create Course Object:
1.	Go to Object Manager in Salesforce.
2.	Click on Create Object from Spreadsheet.
3.	Follow the link to download the spreadsheet named Course.
4.	Upload the spreadsheet file.
5.	Map the fields from the spreadsheet to Salesforce fields.
6.	Complete the process to create the Course object.
b. Create Remaining Objects:
1.	Repeat the steps used for creating the Course object.
2.	Use the respective spreadsheets to create the following objects:
	Consultant
	Student
	Appointment
2. Create Relationships among the Objects  
a. Lookup Relationship for Appointment:
1.	Go to Object Manager and select the Appointment object.
2.	Create a Lookup Relationship between Appointment and Student.
3.	Create another Lookup Relationship between Appointment and Consultant.
b. Create Registration Object:
1.	In Object Manager, create a new object named Registration.
2.	Add necessary fields to store information regarding Student and Course details.
3.	Create a Lookup Relationship between Registration and Student.
c. Additional Lookup Relationship:
1.	Go to the Object Manager and select the Case object.
2.	Create a Lookup Relationship between Case and Student to store student queries related to immigration or visa application.
3. Configure the Case Object  
1.	In Object Manager, select the Case object.
2.	Edit the Type field:
	Add the values Immigration and Visa Application.
3.	Edit the Status field:
	Add the values Open and In-progress.
4. Create a Lightning App  
1.	Go to Setup in Salesforce.
2.	Search for App Manager in the Quick Find box.
3.	Click on New Lightning App.
4.	Enter the app name as EduConsultPro.
5.	Click Next through the setup screens.
6.	From Available Items, select and move Home, Students, Courses, Consultants, Appointments, Registrations, and Cases to Selected Items.
7.	From Available Profiles, select System Administrator and move it to Selected Profiles.
8.	Click Save & Finish.
 
 
Create a ScreenFlow for Student Admission Application process.  
1. Set Up the ScreenFlow  
1.	Go to Setup in Salesforce.
2.	In the Quick Find box, type Flow Builder and select Flow Builder.
3.	Click on New Flow and select Screen Flow.
4.	Click Create.
2. Add Screen Element - Student Info  
1.	Drag and drop a Screen element onto the canvas.
2.	In the Screen Properties pane, label it Student Info.
3.	Click on Fields, then click on the record variable input to create a new resource.
	Resource Name: StudentRecordRes
	Type: Record
	Object: Student
4.	Drag the required fields from the Student object onto the screen to collect student information.
5.	Click Done.
3. Create Student Record  
1.	Drag a Create Records element onto the canvas, after the Student Info screen.
2.	Label it Create Student Record.
3.	Select Create One Record under How Many Records to Create.
4.	Choose Use all values from a record under How to Set the Record Fields.
5.	For Record Variable, select StudentRecordRes (created in the Student Info screen).
6.	Click Done.
4. Add Screen Element - Course Selection  
1.	Drag another Screen element onto the canvas, after the Create Student Record element.
2.	Label it Course Screen.
3.	Add a Picklist component from the left panel.
	Label: Select Course
	Choices: IELTS, GRE, GMAT, Duolingo, TOEFL (each will create a corresponding variable).
4.	Click Done.
5. Add Decision Element - Selecting Course  
1.	Drag a Decision element onto the canvas, after the Course Screen.
2.	Label it Selecting Course.
3.	Under Outcomes, label the first outcome as Selected IELTS and set the condition:
	Resource: Select_Course
	Operator: Equals
	Value: IELTS
4.	Repeat the above steps to create outcomes for GRE, GMAT, Duolingo, and TOEFL.
5.	Click Done.
6. Add Get Record Element - For Each Course  
1.	Drag a Get Records element onto the canvas, under the IELTS path of the Decision element.
2.	Label it Get IELTS Rec.
3.	Object: Course
4.	Condition Requirements: All Conditions are Met (AND)
	Field: Course Name
	Operator: Equals
	Value: Select_Course
5.	Click Done.
6.	Repeat the steps for the other course paths (GRE, GMAT, Duolingo, TOEFL).
7. Create Registration Record  
1.	Drag a Create Records element onto the canvas, after each Get Records element.
2.	Label the element as Create [Course] Registration Rec (e.g., Create IELTS Registration Rec).
3.	Select Create One Record under How Many Records to Create.
4.	Choose Use separate resources, and literal values under How to Set the Record Fields.
5.	Object: Registration
6.	Field: Course_Name__c
Value: Get_IELTS_Rec.Id
7.	Field: Student_Name__cValue: StudentRecordRes.Id
8.	Click Done.
9.	Repeat the steps for other courses (GRE, GMAT, Duolingo, TOEFL).
8. Create Email Text Template Variables  
1.	Click the Toggle Toolbox on the left corner, then click New Resource.
2.	Select Text Template as the Resource Type.
	API Name: StuRegistrationEmailTextTempBody
	View as plain text: Checked
	Body Text:
	vbnet
	Copy code
	Dear {!StudentRecordRes.Name},

Congratulations and welcome to EduConsultantPro!

We are delighted to inform you that your registration on our platform has been successfully completed. You are now part of our esteemed community dedicated to empowering students like you to achieve their educational and immigration aspirations.

(Other details and instructions...)

Thank you.
3.	Click Done.
4.	Repeat the steps to create an email text template for the email subject.
	API Name: StuRegistrationEmailTextTempSub
	Body Text: Subject: Welcome to EduConsultantPro - Registration Successful
9. Add Action Element - Send Email to Student  
1.	Drag an Action element onto the canvas, after all the Decision paths.
2.	Label it Send Email to Student.
3.	Under Set Input Values for Selected Action:
	Body: {!StuRegistrationEmailTextTempBody}
	Recipient Address List: {!StudentRecordRes.Email__c}
	Subject: {!StuRegistrationEmailTextTempSub}
4.	Click Done.
10. Add Success Screen Element  
1.	Drag a Screen element onto the canvas, after the Send Email to Student action.
2.	Label it Success Screen.
3.	From the left panel, search for the Display Text component and drag it to the main panel.
	Label: SuccessMessage
	Resource Picker:
	vbnet
	Copy code
	Dear {!StudentRecordRes.Name},

Congratulations and welcome to EduConsultantPro!

We are delighted to inform you that your registration on our platform has been successfully completed. You are now part of our esteemed community dedicated to empowering students like you to achieve their educational and immigration aspirations.

Your Registration details have been sent through mail kindly check it once.

Thank you.
4.	Click Done.
11. Save and Activate the Flow  
1.	Save the flow with the name EduConsultPro Student Flow.
2.	Click Activate to make the flow available for use.

![image](https://github.com/user-attachments/assets/59614fe3-31b4-43a9-bc30-3f1e4df6e5c2)

 
 
 
Create Users  
 1. Create a New User  
1.	Go to Setup:
	Log in to Salesforce.
	Click on the gear icon (Setup) in the top-right corner.
	In the Quick Find box, type Users and select Users under Administration.
2.	Create New User:
	Click on New User.
3.	Enter User Information:
	Last Name: Consultant
	Alias: Auto-filled (can be edited).
	Email: Enter the email address for the new user.
	Username: Enter a unique username (usually the same as the email).
	Nickname: Auto-filled (can be edited).
	Role: Select the appropriate role if needed.
	User License:Salesforce Platform
	Profile:Standard Platform User
4.	Fill in Other Mandatory Fields:
	Time Zone: Select the appropriate time zone.
	Locale: Select the appropriate locale.
	Email Encoding: Choose the email encoding.
	Language: Select the preferred language.
5.	Save the User:
	Click Save.
2. Configure the User Settings  
1.	Edit User Settings:
	Go back to Setup.
	In the Quick Find box, type Users and select Users.
	Find the user you just created (Consultant) and click Edit next to your name.
2.	Set Manager in Approver Settings:
	Scroll down to the Approver Settings section at the bottom.
	In the Manager field, select Consultant from the dropdown list.
3.	Save Changes:
	Click Save.

 ![image](https://github.com/user-attachments/assets/1b2a91e4-181a-45e7-a0d0-54b7b4fa7ee2)

 
 
Create an Approval Process for Property Object  
 1. Create Email Templates  
Step 1: Enable Lightning Email Templates  
1.	Go to Setup:
	Log in to Salesforce.
	In the Quick Find box, type Email Templates.
	Select Lightning Email Templates.
	Ensure that the toggle is turned on.
Step 2: Create a New Email Template Folder  
1.	Open the App Launcher:
	Click on the App Launcher (grid icon) in the top-left corner.
	Search for Email Templates and select it.
2.	Create a New Folder:
	Click New Folder.
	Name the folder as desired (e.g., "Appointment Templates").
	Set the access level, if needed, and click Save.
Step 3: Create a Submission Email Template  
1.	Create a New Email Template:
	Within the newly created folder, click New Email Template.
	Select the folder you just created.
	Enter the template details:
o	Template Name: Submission Template
o	Subject: [Subject you want to use]
o	HTML Body:
o	html
o	Copy code
o	Dear {{{Appointment__c.Student_Name__c}}},

I hope this email finds you well. I am writing to confirm the details of our upcoming appointment scheduled for {{{Appointment__c.Appointment_DateTime__c}}} regarding {{{Appointment__c.PurposeTopic__c}}}.

Appointment Details:
Appointment No : {{{Appointment__c.Name}}},
Student Name : {{{Appointment__c.Student_Name__c}}},
Consultant Name : {{{Appointment__c.Consultant__c}}},
Date & Time : {{{Appointment__c.Appointment_DateTime__c}}},
Purpose : {{{Appointment__c.PurposeTopic__c}}}

I want to assure you that I am looking forward to our meeting and am fully prepared to address any questions or concerns you may have regarding {{{Appointment__c.PurposeTopic__c}}}. Your success and satisfaction are my top priorities, and I am committed to providing you with the guidance and support you need.

If you have any specific topics or questions you would like to discuss during our appointment, please feel free to share them with me in advance. This will help ensure that our time together is as productive and beneficial as possible.

If for any reason you need to reschedule or cancel our appointment, please notify me at your earliest convenience so that we can make alternative arrangements.

Once again, thank you for choosing to work with me on this matter. I am confident that our collaboration will lead to positive outcomes and progress toward your goals.

If you have any questions or require further information before our scheduled appointment, please don't hesitate to reach out to me.

Looking forward to our meeting.

Best regards,

{{{Recipient.Name}}},

EduConsultantPro
	Save the Template.
Step 4: Create Approval and Rejection Email Templates  
1.	Repeat the above steps to create two more email templates:
	Approval Template: Customize the message for approval.
	Rejection Template: Customize the message for rejection.
2. Create an Approval Process  
Step 1: Start the Approval Process  
1.	Go to Setup:
	In the Quick Find box, type Approval Processes.
	Select Approval Processes.
2.	Select Object:
	Under Manage Approval Processes For, select Appointment.
3.	Create New Approval Process:
	Click Create New Approval Process.
	Select Use Jump Start Wizard.
Step 2: Configure the Approval Process  
1.	Enter Process Details:
	Process Name: Appointment Approval
	Unique Name: Auto-filled
	Entry Criteria: Set criteria if needed, or leave blank to apply to all records.
2.	Assign Approver:
	Under Select Approver, select Manager for the option Automatically assign an approver using a standard or custom hierarchy field.
3.	Set Next Automated Approver:
	Click Next.
	Next Automated Approver Determined By: Select Manager.
4.	Set Record Editability:
	From Record Editability Properties, select Administrators OR the currently assigned approver can edit records during the approval process.
5.	Save the Approval Process:
	Click Save.
Step 3: Configure Initial Submission Actions  
1.	View Approval Process Detail Page:
	After saving, click View Approval Process Detail Page.
2.	Add Field Update:
	Under Initial Submission Actions, click Add New --> Field Update.
	Field Update Name: Submitted
	Field to Update: Appointment: Status
	Specify New Field Value: Pending
3.	Add Email Alert:
	Under Initial Submission Actions, click Add New --> Email Alert.
	Description: Submission Email Alert
	Email Template: Select Submission Template.
	Recipient Type: Select your name or other recipients as needed.
Step 4: Configure Final Approval and Rejection Actions  
1.	Final Approval Actions:
	Add Field Update to update the status to Approved.
	Add an Email Alert using the Approval Email Template.
2.	Final Rejection Actions:
	Add Field Update to update the status to Rejected.
	Add an Email Alert using the Rejection Email Template.
Save and Activate the Approval Process  
 
 
 
 
 Create a Record Triggered Flow  
1. Configure the Start Element  
1.	Go to Setup:
	Log in to Salesforce.
	In the Quick Find box, type Flows and select Flows under Process Automation.
2.	Create a New Flow:
	Click on New Flow.
	In the Flow Builder, select Record-Triggered Flow.
	Click Create.
3.	Configure the Start Element:
	In the Configure Start window:
o	For Object, select Appointment.
o	For Trigger the Flow When, select A record is created.
	The flow will now trigger every time a new Appointment record is created.
2. Add an Action Element  
1.	Add an Action Element:
	After the Start element, drag the Action element from the left panel onto the canvas.
	In the Action field, search for and select Submit for Approval.
	Label the action as Approval SubFlow.
2.	Configure the Action Element:
	Set the RecordId field to {!$Record.Id}. This ensures that the record that triggered the flow (the Appointment record) will be submitted for approval.
3. Save and Activate the Flow  
1.	Save the Flow:
	Click on Save.
	Label the flow as EduConsultPro Approval Flow.
2.	Activate the Flow:
	Click on Activate to make the flow live.

![image](https://github.com/user-attachments/assets/57d5298a-171c-4167-a099-0afb6d595f33)

 
Create a ScreenFlow for Existing Student to Book an Appointment  
1. Create a New Screen Flow  
1.	Go to Setup:
	In Salesforce, go to the Setup menu.
	In the Quick Find box, type Flow and select Flow Builder under Process Automation.
2.	Create a New Flow:
	Click New Flow.
	Select Screen Flow.
	Click Create.
2. Add a Screen Element to Get Student Info  
1.	Add a Screen Element:
	Drag the Screen element from the left panel onto the canvas.
	In the Screen Properties pane, set the Label to Get Student Info.
2.	Add Text Components:
	From the left side panel, drag two Text components onto the screen.
	Set the labels as follows:
o	1st Text Component Label: Enter Student Name
o	2nd Text Component Label: Enter Student Email
3.	Click Done to save this screen.
3. Add a GET Record Element to Fetch Student Record  
1.	Add a GET Record Element:
	After the Screen element, drag the Get Records element onto the canvas.
	Label it as Get Rec.
2.	Configure the GET Record Element:
	Object: Select Student.
	Condition Requirements: Set to All Conditions Are Met (AND).
o	Field: Student Name
Operator: Equals
Value: !{Enter_Student_Name}
o	Field: Email__c
Operator: Equals
Value: !{Enter_Student_Email}
3.	Click Done to save this element.
4. Add a Decision Element to Choose Between Appointment or Case  
1.	Add a Decision Element:
	Drag the Decision element onto the canvas after the Get Record element.
	Label it as Appointment or Case.
2.	Configure the Decision Element:
	Outcome Label: Appointment
Resource: !{How_may_I_Help_you}
Operator: Equals
Value: !{Book_an_Appointment}
	Click the + icon to add another outcome.
o	Outcome Label: Case
Resource: !{How_may_I_Help_you}
Operator: Equals
Value: !{Create_a_Case}
3.	Click Done to save this element.
5. Add a Screen Element for Appointment Booking  
1.	Add a Screen Element:
	After the Decision element, on the Appointment path, add another Screen element.
	Label it as Appointment Booking Screen.
2.	Create a New Resource:
	Click on Fields in the screen editor.
	Select the record variable input and create a new resource called AppointmentRecordRes to display all fields in the Appointment object.
3.	Add Fields to the Screen:
	Drag all necessary fields (e.g., Date, Time, Purpose, etc.) from the Appointment object to the screen to collect student information.
4.	Click Done to save this screen.
6. Add a GET Record Element to Fetch Consultant Record  
1.	Add a GET Record Element:
	After the Appointment Booking Screen, drag another Get Records element onto the canvas.
	Label it as Get Consultant Rec.
2.	Configure the GET Record Element:
	Object: Select Consultant.
	Condition Requirements: Set to All Conditions Are Met (AND).
o	Field: Name
Operator: Equals
Value: !{AppointmentRecordRes.Consultant_Name__c}
3.	Click Done to save this element.
7. Create the Appointment Record using Create Records Element  
1.	Add a Create Records Element:
	After the Get Consultant Rec element, drag the Create Records element onto the canvas.
	Label it as Create Appointment.
2.	Configure the Create Records Element:
	How many records to create?: Select One.
	How to Set the Record Fields?: Select Use separate resources, and literal values.
	Object: Select Appointment.
3.	Set the Field Values:
	Field: Appointment_DateTime__c
Value: !{AppointmentRecordRes.Appointment_DateTime__c}
	Field: Consultant__c
Value: !{Get_Consultant_Rec.Id}
	Field: Notes__c
Value: !{AppointmentRecordRes.Notes__c}
	Field: PurposeTopic__c
Value: !{AppointmentRecordRes.PurposeTopic__c}
	Field: Student_Name__c
Value: !{Get_Rec.Id}
4.	Click Done to save this element.
8. Add a Screen Element for Confirmation  
1.	Add a Screen Element:
	After the Create Appointment element, add another Screen element.
	Label it as Confirmation Screen.
2.	Add Display Text:
	From the left panel, drag the Display Text component onto the screen.
	Label it as Appointment_Confirmation.
3.	Configure the Display Text:
	In the Resource picker box, paste the following:
	yaml
	Copy code
	Consultant Name: {!Get_Consultant_Rec.Name}
Date & Time: {!AppointmentRecordRes.Appointment_DateTime__c}
Notes: {!AppointmentRecordRes.Notes__c}
4.	Click Done to save this element.
9. Add a SubFlow Element for Creating a Case  
1.	Add a SubFlow Element:
	After the Decision element, on the Case path, add a SubFlow element.
	Search for and select Create a Case.
	Label it as Create Student Case.
2.	Click Done to save this element.
10. Save and Activate the Flow  
1.	Save the Flow:
	Label it as EduConsultantPro Existing Student Flow.
2.	Activate the Flow:
	Click on Activate to make it live.
 
Create a ScreenFlow to Combine all the flows at one place  
1. Add a Screen Element: Welcome Screen  
1.	Create a New ScreenFlow:
	Go to Setup, type Flow Builder in the Quick Find box, and select Flow Builder.
	Click on New Flow and select Screen Flow.
2.	Add a Screen Element:
	Drag a Screen element onto the canvas.
	In the Screen Properties pane, label it as Welcome Screen.
3.	Add Display Text Component:
	From the left side panel, search for Display Text and drag it to the main panel.
	Label the component as SuccessMessage.
	In the Resource Picker box, paste the following text:
4.	vbnet
5.	Copy code
6.	Welcome to EduConsultantPro

Your premier destination for education and immigration solutions!

At EduConsultantPro, we understand that embarking on educational or immigration journeys can be both exhilarating and daunting. That's why we're here to guide you every step of the way with expertise, dedication, and personalized support.

Whether you're seeking to pursue your academic dreams abroad, navigate the complexities of immigration processes, or enhance your professional skills through international opportunities, EduConsultantPro is your trusted partner.

Our team of seasoned consultants is committed to understanding your unique aspirations and crafting tailored strategies to help you achieve your goals efficiently and effectively. From selecting the right educational institution to navigating visa procedures, our comprehensive services cover all aspects of your journey.

At EduConsultantPro, we believe in fostering inclusive communities and unlocking the full potential of every individual. With our unwavering commitment to excellence and integrity, we strive to make your experience with us seamless and rewarding.

Welcome to EduConsultantPro – where your aspirations meet our expertise, and together, we pave the path to success. Let's embark on this transformative journey together!
7.	Click Done to save the screen element.
2. Add a Screen Element: Existing or New Student Confirmation Screen  
1.	Add Another Screen Element:
	Drag a Screen element after the Welcome Screen element.
	Label it as Existing or New Student Confirmation Screen.
2.	Add Radio Button Component:
	From the left side panel, add a Radio Button component.
	Set the Label to Are you an Existing Student.
3.	Create Choices:
	Click on Add Choice and type "Yes" in the input field, then click Create Yes choice.
	Repeat the above step to create a "No" choice resource.
4.	Click Done to save the screen element.
3. Add a Decision Element  
1.	Add a Decision Element:
	Drag a Decision element after the Existing or New Student Confirmation Screen element.
	Label it as Decision 1.
2.	Create Outcomes:
	Under Outcome Label, enter If Existing Student.
	Set the Condition as follows:
o	Resource: {!Are_you_a_Existing_Student}
o	Operator: Equals
o	Value: {!Yes}
	Click the "+" icon and repeat the steps to create an outcome for "No".
4. Add SubFlow Elements  
1.	Add SubFlow for Existing Student:
	Drag a SubFlow element onto the If Existing Student path after the Decision 1 element.
	Search for and select EduConsultantPro Existing Student Flow.
	Label it as Existing Student Flow.
2.	Add SubFlow for New Student:
	Drag a SubFlow element onto the If Not an Existing Student path after the Decision 1 element.
	Search for and select EduConsultantPro Student Flow.
	Label it as New Student Flow.
5. Save and Activate the Flow  
1.	Save the Flow:
	Click on Save.
	Label it as EduConsultantPro Combined Flow.
2.	Activate the Flow:
	Click on Activate to make the flow live.
 
Create a lightning app page  
1.	Open Lightning App Builder:
	From Setup, type "App Builder" in the Quick Find box.
	Click on Lightning App Builder.
2.	Create a New Home Page:
	Click on New.
	Select Home Page.
	Click Next.
3.	Configure the Home Page:
	Enter the Page Name as “EduConsultPro Home Page”.
	Choose the Standard Home Page template.
	Click Done.
4.	Add Components to the Page:
	In the page layout editor, drag the Flow component to the top-right region of the page.
	In the Flow component's configuration panel, search for and select the “EduConsultantPro Flow”.
5.	Save the Page:
	Click Save.
6.	Activate the Page:
	To make the page available in your application, click Activate.
	Choose the appropriate options to add it to the app (e.g., Default for specific profiles or as a utility).
7.	Publish the Page:
	Click Publish to make the page live and accessible in your Salesforce environment.
 
 
 
 ![image](https://github.com/user-attachments/assets/16a9148b-695a-4b33-9fda-37ea623b6b76)

 ![image](https://github.com/user-attachments/assets/6ab9f198-3386-4e6e-9d02-2817fd2e15d0)



 
 
 
 
 

