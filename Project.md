# Project

For your class project, you will add a new orthopedic and physical therapy module.

## Use Cases

### UC88: Orthopedic Office Visit

##### 88.1 Preconditions

An HCP is a registered user of the iTrust Medical Records system (UC2). The iTrust HCP user has authenticated himself or herself in the iTrust Medical Records system (UC3). The patient must be a registered patient in the iTrust Medical Records system.

##### 88.2: Main Flow

* A HCP chooses to view [S2], document [S3], or edit [S4] an orthopedic office visit for an iTrust patient [S1].

##### 88.3 Sub-flows

* [S1] The HCP enters a MID [E1] or name of a patient and confirms their selection [E2].
* [S2] An non-orthopedic/non-physical therapist HCP selects an orthopedic office visit from a list of prior visits. The HCP can view (but not edit) the details of the orthopedic office visit as described in [S3]. All events are logged (UC5).
* [S3] An orthopedic or physical therapist HCP [E3] documents the following information related to an orthopedic office visit and saves the orthopedic office visit record. All events are logged (UC5). After the orthopedic office visit record is saved, the orthopedic or physical therapist HCP is sent to an overview of all recorded orthopedic office visits for the patient. The following information may be entered, but not all is required. [E4] 
	* Date of the orthopedic office visit - current date. Required.
	* Injured Limb or joint. Required.
	* X-Ray. \<Image\>. Optional.
	* MRI. \<Image\>. Optional.
	* MRI Report. Optional
	* Add a diagnosis for a patient for diseases related to musculoskeletal health. They may diagnosis a patient with one or more diseases.
	    * Anterior cruciate ligament injury (S83.5)
	    * Meniscus tear (S83.20)
	    * Rheumatoid arthritis of hand (M05.44)
	    * Chondromalacia (M94.2)
	    * Congenital pes cavus (Q66.7)
	    * Whiplash injury (S13.4)
* [S4] An orthopedic or physical therapist HCP can return to an orthopedic office visit and modify or delete the fields of the orthopedic office visit [E3]. The event is logged (UC5). After the orthopedic office visit record is saved, the orthopedic or physical therapist HCP is sent to an overview of all recorded orthopedic office visits for the patient.


##### 88.4: Alternative Flows

* [E1] The HCP types an invalid patient medical identification number and is prompted to try again.
* [E2] The patient chosen is not the desired patient. The HCP does not confirm the selection and is prompted to try again.
* [E3] An HCP without the orthopedic or physical therapist specializations are unable to create or edit an orthopedic office visit. They are prompted to create a regular office visit (UC11).
* [E4] Invalid or missing inputs are flagged and an error message appropriate to the input is printed.

##### 88.5: Logging

| Transaction Code	| Verbose Description |	Logged In MID	| Secondary MID	| Use Case(s) Involved	|Type of Transaction	| Additional Information	| Patient Viewable |
| ------------------| --------------------| --------------  | --------------| ----------------------| ------------------- | ------------------------- | ------- | ---------|
| 8800	| Create orthopedic Office Visit |	Editor (HCP)	|Patient	|88	|Create	| Office Visit ID	| Yes
| 8801	| View orthopedic Office Visit|Viewer (HCP)|	Patient	|88	|View	|Office Visit ID|Yes
| 8802	| Edit orthopedic Office Visit|Editor (HCP)|Patient|88|Edit|Office Visit ID|Yes

### UC89: Physical Therapy Visit

##### 89.1 Preconditions

An HCP is a registered user of the iTrust Medical Records system (UC2). The iTrust HCP user has authenticated himself or herself in the iTrust Medical Records system (UC3). The patient must be a registered patient in the iTrust Medical Records system.

##### 89.2: Main Flow

* A HCP chooses to view [S2], document [S3], edit [S4], or order [S5] a physical therapy office visit for an iTrust patient [S1].

##### 89.3 Sub-flows

* [S1] The HCP enters a MID [E1] or name of a patient and confirms their selection [E2].
* [S2] An non-orthopedic/non-physical therapist HCP selects a physical therapy office visit from a list of prior visits. The HCP can view (but not edit) the details of the physical therapy office visit as described in [S3]. All events are logged (UC5).
* [S3] A physical therapist HCP [E3] documents the following information related to an physical therapy office visit and saves the physical therapy office visit record. All events are logged (UC5). After the physical therapy office visit record is saved, the physical therapist HCP is sent to an overview of all recorded orthopedic office visits for the patient. The following information may be entered, but not all is required [E4].
	* Date of the orthopedic office visit - current date. Required.
	* Wellness Survey Results. Required.
	* Wellness Score. (0-100) Calculated.
	* Add an exercise for a patient. Multiple exercises can be allowed.
	    * Quad Set: Slight Flexion
	    * Heel Slide (Supine)
	    * Calf Towel Stretch
	    * Straight Leg Raise
	    * Terminal Knee Extension
	    * Gastroc Stretch 
	    * Wall Slide 
	    * Proprioception: Step Over 
	    * Hip Abduction 
	    * Single-Leg Step Up 
* [S4] An orthopedic or physical therapist HCP can return to a physical therapy visit and modify or delete the fields of the physical therapy office visit [E3]. The event is logged (UC5). After the physical therapy office visit record is saved, the orthopedic or physical therapist HCP is sent to an overview of all recorded orthopedic office visits for the patient.
* [S5] An orthopedic can order a physical therapy treatment when documenting or editing an orthopedic office visit [UC88]. The order must specific a physical therapist. A physical therapist cannot create a physical therapy office visit without an existing doctor order specifying them [E5].

##### Wellness Survey 

| Do you have a any difficulty | Unable | Very difficult | Moderate | Little bit | No difficulty |
| ---------------------------- | ------ | ---------------| -------- | ---------- | --------------|
| Performing normal house work |        |                |          |            |               |
| Getting into or out of bath  |        |                |          |            |               |
| Waking between rooms         |        |                |          |            |               |
| Squatting?                   |        |                |          |            |               |
| Lift an object from floor    |        |                |          |            |               |
| Walking two blocks?          |        |                |          |            |               |
| Getting up or down stairs    |        |                |          |            |               |
| Standing for 1 hour          |        |                |          |            |               |
| Jumping                      |        |                |          |            |               |
| Running on uneven ground     |        |                |          |            |               |

* **Wellness score**: Answering unable on every questions results in a 0. Answering no difficulty results on every question results in a 100.

##### 89.4: Alternative Flows

* [E1] The HCP types an invalid patient medical identification number and is prompted to try again.
* [E2] The patient chosen is not the desired patient. The HCP does not confirm the selection and is prompted to try again.
* [E3] An HCP without the orthopedic or physical therapist specializations are unable to create or edit an physical therapist office visit. They are prompted to create a regular office visit (UC11).
* [E4] Invalid or missing inputs are flagged and an error message appropriate to the input is printed.
* [E5] A physical therapist cannot create a physical therapy office visit without an existing doctor order specifying them.

##### 89.5: Logging

| Transaction Code	| Verbose Description |	Logged In MID	| Secondary MID	| Use Case(s) Involved	|Type of Transaction	| Additional Information	| Patient Viewable |
| ------------------| --------------------| --------------  | --------------| ----------------------| ------------------- | ------------------------- | ------- | ---------|
| 8900	| Create physical therapy Office Visit |	Editor (HCP)	|Patient	|89	|Create	| Office Visit ID	| Yes
| 8901	| View physical therapy Office Visit|Viewer (HCP)|	Patient	|89	|View	|Office Visit ID|Yes
| 8902	| Edit physical therapy Office Visit|Editor (HCP)|Patient|89|Edit|Office Visit ID|Yes


### UC90: Orthopedic Surgery

##### 90.1 Preconditions

The administrator has authenticated himself or herself in the iTrust Medical Records system (UC2). The iTrust HCP user has authenticated himself or herself in the iTrust Medical Records system and is an orthopedic (UC3). The patient must be a registered patient in the iTrust Medical Records system.

##### 90.2 Main Flow

A HPC chooses to view [S2], document [S3], edit [S4], or order [S5] a surgical ophthalmology office visit for an iTrust patient [S1].

##### 90.3 Sub Flows

* [S1] The HCP enters a MID [E1] or name of a patient and confirms their selection [E2].
* [S2] A non-orthopedic HCP selects a surgical orthopedic office visit from a list of prior visits. The HCP can view (but not edit) the details of the surgical orthopedic office visit as described in in [S3]. All events are logged (UC5).
* [S3] An orthopedic HCP [E3] documents the following information related to a surgical orthopedic office visit and saves the surgical orthopedic office visit record. All events are logged (UC5). After the surgical orthopedic office visit record is saved, the orthopedic HCP is sent to an overview of all recorded surgical orthopedic office visits for the patient. The following information may be entered, but not all is required. [E4]
	* Date of the ophthalmology surgery office visit - current date. Required.
	* Surgery Type. Required
		* Total knee replacement
		* Total joint replacement
		* ACL reconstruction
		* Ankle replacement
		* Spine surgery
		* Arthroscopic surgery
		* Rotator cuff repair
	* Surgery Notes. Optional
* [S4] An orthopedic HCP can return to a surgical ophthalmology office visit and modify or delete the fields of the surgical ophthalmology office visit [E3]. The event is logged (UC5). After the surgical ophthalmology office visit record is saved, the ophthalmologist HCP is sent to an overview of all recorded surgical ophthalmology office visits for the patient.
* [S5] An orthopedic can order a surgery treatment when documenting or editing an orthopedic office visit [UC88]. The order must specific a orthopedic as the surgeon. A orthopedic cannot create a orthopedic surgery office visit without an existing doctor order specifying them [E5].

##### 90.4 Alternative Flows

* [E1] The HCP types an invalid patient medical identification number and is prompted to try again.
* [E2] The patient chosen is not the desired patient. The HCP does not confirm the selection and is prompted to try again.
* [E3] An HCP without the ophthalmologist specialization is unable to create or edit a surgical ophthalmology office visit. They are prompted to create a regular office visit (UC11).
* [E4] Invalid or missing inputs are flagged and an error message appropriate to the input is printed.
* [E5] A orthopedic cannot create a orthopedic surgery office visit without an existing doctor order specifying them.


##### 90.5 Logging

| Transaction Code	|Verbose Description	|Logged In MID	|Secondary MID	|Use Case(s) Involved	|Type of Transaction	|Additional Information	| Patient Viewable
| ----------------- | --------------------- | ------------- | ------------- | ----------------------| ------------|---------------------- | ----------------|
|9000	|Create Surgical Ophthalmology Office Visit|	Editor (HCP)|	Patient	|90	|Create	|Office Visit ID	|Yes
|9001	|View Surgical Ophthalmology Office Visit|	Viewer (HCP)	|Patient	|90	|View	|Office Visit ID	|Yes
|9002	|Edit Surgical Ophthalmology Office Visit|	Editor (HCP)	|Patient	|90	|Edit	|Office Visit ID	|Yes

### UC91: Patient View

##### 91.1 Preconditions

An patient and/or their dependent(s) is a (are) registered user(s) of the iTrust Medical Records system (UC2). The iTrust user has authenticated himself or herself in the iTrust Medical Records system (UC3). If a patient is a dependent of another patient, the relationship is established in the iTrust Medical Records system (UC58)(UC59).

##### 91.2: Main Flow

A patient selects an option to view their [S1] or their dependent's [S2] orthopedic/physical therapy office visit results [E1][E2].

##### 91.3 Sub-flows

* [S1] The patient chooses to view all of their orthopedic/physical therapy office visit records.
* [S2] The patient chooses to view one of their dependent’s orthopedic/physical therapy office visit records.
* [S3] The patient selects the specific orthopedic/physical therapy office visit to view [S4].
* [S4] The patient can see, but not edit, the following:
	* Date of the office visit.
	* Attending orthopedic or physical therapist HCP
	* [if orthopedic office visit]
		* X-Ray results
		* MRI results
		* List of diagnosis
		* Physical Therapy Order
		* Surgery Order
	* [if physical therapy] 
		* Wellness Score.
		* List of exercises

##### 91.4: Alternative Flows

* [E1] The patient has no office visits, so the list is empty.
* [E2] The patient does not have permissions to view another patient’s orthopedic/physical therapy office visits (the other patient is not a dependent).

##### 91.5: Logging

|Transaction Code	|Verbose Description	|Logged In MID	|Secondary MID	|Use Case(s) Involved	|Type of Transaction	|Additional Information	|Patient Viewable|
| ------------------| ----------------------|---------------|-------------- | --------------------- | ------------|-----------------------| ---------------|
|9100	|View Patient Orthopedic/PT Office Visit	|Patient	|N/A	|91	|View	|Office Visit ID	|Yes
|9101	|View Dependent Orthopedic/PT Office Visit	|Patient	|Dependent Patient	|91	|View	|Office Visit ID|	Yes

### UC92: Schedule Physical Therapy/Orthopedic Appointment

##### 92.1 Preconditions

An HCP is a registered user of the iTrust Medical Records system (UC2). The patient must be a registered patient in the iTrust Medical Records system. The iTrust user has authenticated himself or herself in the iTrust Medical Records system.

##### 92.2 Main Flow

A patient may request [S1] an Physical Therapist or Orthopedic appointment with a registered Physical Therapist or Orthopedic by specifying a date and time. The HCP Physical Therapist or Orthopedic may then either accept[S2] or reject[S3] the appointment request after seeing it on the appointment request list [S4]. A patient can see if the HCP accepted or rejected their proposed appointment [S5].

##### 92.3 Sub Flows

* [S1] A registered patient may request an Physical Therapist or Orthopedic appointment with a registered Physical Therapist or Orthopedic by specifying a date or time and some optional comments.
* [S2] A Physical Therapist or Orthopedic may accept an appointment request.
* [S3] A Physical Therapist or Orthopedic may decline an appointment request.
* [S4] A Physical Therapist or Orthopedic can see a list of all appointment requests for them.
* [S5] A patient can see all requests for appointments they created along with their status.

##### 92.4 Alternative Flows

* [E1] If the HCP is not a Physical Therapist or Orthopedic the system shall not allow them to interact with Physical Therapist or Orthopedic appointment requests.
* [E2] The system shall stop a patient from requesting a Physical Therapy/Orthopedic appointment with a HCP who is not a Physical Therapist or Orthopedic.
* [E3] The system shall stop a Physical Therapist who does not have an order from a doctor from accepting a physical therapy appointment.


### iTrust Engineering Maintenance

The following are a set of non-functional set of requirements:

* Level 1: Refactor all `Connection` and `PreparedStatement` objects in edu.ncsu.csc.itrust.dao.mysql.* to use try-with-resources instead of manually calling `close()`.
* Level 2: Upgrade Selenium to >=2.48 and upgrade (or @Ignore) failing tests. (extra 3%)
* Level 3: Create a docker image for a running deployed version of iTrust: Java, MySql, Tomcat, iTrust war deployed to tomcat. (extra 4%)
* Level 4: Create a single apt-get, homebrew, or choco package that auto installs everything needed to develop iTrust. Must be demonstrated on a clean VM. *End the install pain, once and for all*. (extra 5%)

**You cannot receive credit for a higher level without completing all levels below it.**

## Timeline

You will deliver completed use cases and engineering changes in 6 iterations.
Starting with Iteration 1, you must deliver one at least one use case by the end of each iteration. You can continue working on non-functional engineering changes until the final iteration.

##### Iteration Worksheet

At the end of each iteration, you will submit a completed iteration worksheet at the end of the iteration (include in WORKSHEET.md). This will describe the tasks completed for your use case and scenarios.

An example sheet follows:

| Deliverable   | Item/Status   |  Issues/Tasks
| ------------- | ------------  |  ------------
| Use Case      | UC88          | &nbsp;
| Scenario      | 1             |  #33, #38, #78
| Scenario      | 2             |  [Pivotal Task](https://www.pivotaltracker.com/story/show/114636091)
| Scenario      | 3             |  [Trello Card](https://trello.com/c/diA1DaMw)
| Scenario      | &nbsp;        | &nbsp;
| Unit Tests    | Complete      | testAddVisit, testEditVisit, ...
| Selenium Tests| Incomplete    | scenario1, error1,...
| Jenkins       |  Green        | &nbsp;
| Design        | Complete      | [Link to design docs]()

* Github issues in a markdown referred to as `#33` will automatically turn into links when in same repo.
* You can link to trello cards by click on share inside a card to get a link.
* You can link to pivotal stories by clicking on the first button left of ID in detail view.
* You reuse the markdown of the above table for your worksheet.
* You are free to use any design notation (including text) you desire.

##### Demo

You will create a demo screencast for your final completed delivery. Some recommendations for [creating screencasts here](https://github.com/CSC-DevOps/Course/blob/master/HW/HW0.md#screencastsdemo-gifs-guidelines).

##### Stories and Tasks

You should practice agile by breaking scenarios down into smaller stories and tasks and plan how to test, implement, and deliver those changes in a week. Because you need to deliver a use case almost every week, you might consider having tasks that separately handle different layers of system. You will find this is a common situation in an agile team. Some suggested breakdowns include:

* Design
* Reports, scrum master, planning
* Creating database tables
* Creating testing data in generator
* Creating junit tests
* Scripting selenium
* Beans
* DAO actions
* html layout and design
* JSP logic/code

Use your iteration 0 to explore issues related to feasibility and design. Like how to store an image in a database? What code might we reuse?

##### Scenarios

You will want to create a concrete version of a scenario to make testing easier. For example:

**UC88 [S3]**

> Orthopedic HCP Dr. Brooke Boes authenticates into iTrust.  Dr. Boes searches for patient Andy 
Programmer and selects the option to add a new orthopedic office visit.  She enters the following 
information:
Date: 2/29/2016
Injured Limb or Joint: Left Knee
MRI: ![Image](http://www.orthop.washington.edu/sites/default/files/Portals/21/LiveContent/9141/Images/figure2.jpg)
MRI Results: Complete tear of ACL observed. Minor meniscal tear.  
Diagosis: Anterior Cruciate Ligament injury, meniscal tear

> Dr. Boes saves the office visit and then sees the office visit in the list of prior orthopedic 
office visits for Andy Programmer.

**UC89 [S5]**
> Orthopedic HCP Dr. Brooke Boes authenticates into iTrust.  Dr. Boes searches for patient Andy Programmer and selects the option to edit a orthopedic office visit.  She adds an order for physical therapy by selecting the following information:
Physical Therapy Order: Andrea Fuller

> Dr. Boes saves the office visit and then sees the office visit in the list of prior orthopedic 
office visits for Andy Programmer. Andrea Fuller can now create an physical therapy office visit for Andy Programmer.


## Evaluation

For each use case, you will be evaluated on the following:

* Jenkins Green Ball (15%)
* Iteration Worksheet and Demo (15%)
* Design and Report (10%)
* Black-box unit tests and selenium unit tests testing all flows and scenarios in use case (30%).
* Use case implementation (30%)

In total, project breakdown will be as follows:

* UC88 (15%)
* UC89 (15%)
* UC90 (15%)
* UC91 (15%)
* UC92 (15%)
* Non-functional: Engineering (15%)
* Demo (10%)

**Deadlines**

* Iteration 1: March 23rd, midnight
* Iteration 2: March 30th, midnight  
* Iteration 3: April  5th, midnight
* Iteration 4: April 13th, midnight
* Iteration 5: April 20th, midnight
