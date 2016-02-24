# HW3

You will work in pairs (same group as HW2). 
However, there will be an individual component for HW3.P1.

## Participation

We expect that both partners will contribute to the assignment, which means that we expect to see at least one GitHub push for each person on the team. A team member without a GitHub push will have an automatic deduction of 10 points. The exceptions to this are 1) a documented technology problem that prevents pushing to GitHub on personal AND lab machines (notification to the teaching staff must occur a least 24 hours BEFORE the deadline) and 2) documented pair programming on the assignment. Pair programming documentation should occur in the commit messages for a push.

## New Feature

iTrust supports the ability for patients to record health data at home, i.e., telemedicine; however, patients have to manual record their information. This process is quite burdensome, they have both remember to a) record the health information and b) to enter their data on a daily basis, otherwise they would be unable to log data for a previous day.

To greatly improve the usability of the tool, you will be adding a new feature to import data from a fitness tracker.

The following requirements will be used to guide your implementation.

* The ability to import data from two different fitness wearables (Fitbit and Microsoft Band 2).
* Ability to load heart rate, weight, steps, and calories and view in iTrust.
* Ability for a patient to view a summary of imported data. 
* Ability for a HCP to to view a graphical line chart of data over time.

For testing purposes, you will be provided with an exported dataset (in .json format).

### HW3.P1

#### Solo (20)

Complete the REST portion of [Lab 4](https://github.com/CSC-326/Course/blob/master/Labs/Lab4.md)

You will do the following tasks:

* Write code for listBranches in a given repo under an owner. See [list branches](https://developer.github.com/v3/repos/#list-branches)
* Write code for [create a new repo](https://developer.github.com/v3/repos/#create)
* Write code for [creating an issue](https://developer.github.com/v3/issues/#create-an-issue) for a created repo.
* Write code for [editing a repo](https://developer.github.com/v3/repos/#edit) to enable wiki support.


#### Extra Credit (5)

* Write code for adding your lab TA [as a collaborator.](https://developer.github.com/v3/repos/collaborators/#add-user-as-a-collaborator)

#### Task Tracking (20)

* Select one of the technologies we've described in class. If using Trello or Pivotal, please add your grade TA to the board/system so they can see your plan.

You must create tasks that describe how you will handle this entire homework assignment, and include assignments to the person who will be primarily responsible for a task. Remember, the best way to break down tasks is into units of work that are about 2 hour chunks.

#### Design (60)

* Design a wireframe model for displaying data.
* Design a flow map for describing user experience steps in importing data and displaying it.
* Identify two design patterns that you will use in your implementation.
* Create a class diagram to representation the fitness data and implemented design patterns.

**Bonus**: Complete your and commit your design to your HW3.P2 by the soft deadline for an extra 10 points.

### HW3.P2

The new feature will be worth the following:

* Import (20)
* Heart rate (10)
* Patient Summary Report (10)
* HCP Patient Chart (20)
* New Unit Tests + Selenium Tests (20)
* Report (20)

##### Sample Data

* [Fitbit Weight](https://github.com/BioStack/FitLink/blob/master/samples/weight.json)
* [Fitbit Heart](https://github.com/BioStack/FitLink/blob/master/samples/heartrate.json)
* [Fitbit Steps](https://github.com/BioStack/FitLink/blob/master/samples/steps.json)
* [Fitbit Calories](https://github.com/BioStack/FitLink/blob/master/samples/calories.json)
* [Microsoft Band Daily Summary](https://github.com/CSC-326/Course/blob/master/HW/Daily_Summary_20160216_20160222.csv)
* [Microsoft Band Activity Summary](https://github.com/CSC-326/Course/blob/master/HW/Activity_Summary_20160216_20160222.csv)

**Note:**

* You do not have to write a selenium test case to verify the correctness of the chart. There are limitations in how htmlunitdriver can handle graphical components.
* You may fail a jenkins build if you do not add enough test cases to have sufficient code coverage.
* Selenium 2.48 does add support for SVG. If you upgrade selenium (which will break several unit tests and add support for testing SVG graphics, you'd be elligible for 20 extra credit points. Do not attempt to do this unless you've absolutely completed the entire assignment and have a working build. I'd recommend trying in a new branch.)

#### Report (20)

You must include the following report:

1. **Problem Statement:** A one or two paragraph description, using complete sentences and proper grammar, of the requirement modifications, additions, or enhancements. The problem statement should not include information about why the changes are made, but is an overview of the portion of iTrust that you are designing.  This is the introduction to the project document, and sets up the rest of information that the document conveys. 
2. **Design Decisions**: Describe the decisions that you made when making feature change.  This is a high level overview of the changes you’re making. Describe the changes at all layers of the system (JSP, Action, DAO, SQL).
3. **Black Box Test Plan**: Provide details about how to test that your new feature is made correct and the automated test inputs and test cases needed.

## Deadlines

Please submit the following by the given deadlines:
*For iTrust, you only need to use the HW3.P2 repo.*

#### HW3.P1

**Due:** Feb 26, 2016, midnight (HARD)

1. Submit on moodle (HW3.P1) your solo portion of assignment.

**Due:** Feb 26, 2016, midnight (SOFT DEADLINE)

1. Complete your created tasks in your task tracking as you have made some progress on tasks.
2. Commit to your README.md your design.
3. Write and unit tests and selenium test scripts for testing your new feature (do not have to pass yet).

#### HW3.P2

**Due:** March 7, 2016, midnight

1. All tasks should be marked complete in task tracker.
2. Implement your design for new feature. Include screenshots of your new feature in README.md
3. New selenium test scripts must pass.
4. Your project must build and pass all tests in Jenkins in order to receive full credit.
5. Report.
