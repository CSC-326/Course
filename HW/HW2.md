# HW2.P1/HW2.P2

You will be fixing bugs, finding code smells, and refactoring code in iTrust.
See below for deadlines.

## Participation

This assignment must be completed as a pair/team. Lack of participation by a partner/teammate may result in the adjustment of grades.

We expect that both partners will contribute to the assignment, which means that we expect to see at least one GitHub push for each person on the team. A team member without a GitHub push will have an automatic deduction of 10 points. The exceptions to this are 1) a documented technology problem that prevents pushing to GitHub on personal AND lab machines (notification to the teaching staff must occur a least 24 hours BEFORE the deadline) and 2) documented pair programming on the assignment. Pair programming documentation should occur in the commit messages for a push.

## Task Tracking (20)

#### Trello/Pivotal/Github Issues 

* Select one of the technologies we've described in class. If using Trello or Pivotal, please add your grade TA to the board/system so they can see your plan.

You must create tasks that describe how you will handle this entire homework assignment, and include assignments to the person who will be primarily responsible for a task. Remember, the best way to break down tasks is into units of work that are about 2 hour chunks.

## Code Smells (30)

* Find 5 unique examples of message chains in iTrust. 2.5 points bonus to pair that finds longest one in each lab section.

* Find the longest method you can in iTrust. 2.5 points bonus to pair that finds the longest one in each lab section.

* BONUS 5 points: Identify an example of another code smell in iTrust, as listed on this [inspection slide](https://docs.google.com/presentation/d/16i-DAylCTAbzY6_F2nOVByLRs8e5AftlCd-MkgKlhJ0/edit#slide=id.gff38ba208_0_46). 

## Refactoring (50)

Sometimes new language features get added that make coding a little more streamlined. Often, old code will [never be updated](http://research.microsoft.com/apps/pubs/default.aspx?id=146635) to take advantage of a new feature.

Java 7 introduced a new feature, [try-with-resources](http://docs.oracle.com/javase/7/docs/technotes/guides/language/try-with-resources.html), that allows resources to be automatically disposed after they are being used.

##### Example before and after:

**BEFORE:**

```
	private void addInfection(Date date, double icd) throws SQLException, DBException {
		Connection conn = null;
		PreparedStatement ps = null;
		try{
			conn = factory.getConnection();
			ps = conn.prepareStatement("INSERT INTO officevisits(VisitDate,PatientID, hcpid, hospitalid) VALUES(?, 2, 9000000000, '1')");
			ps.setDate(1, new java.sql.Date(date.getTime()));
			ps.executeUpdate();
			ps.close();
			ps = conn.prepareStatement("INSERT INTO ovdiagnosis(VisitID, ICDCode) VALUES(?,?)");
			ps.setLong(1, DBUtil.getLastInsert(conn));
			ps.setDouble(2, icd);
			ps.executeUpdate();
		}
		catch (SQLException ex){
			throw ex;
		}
		finally{
			DBUtil.closeConnection(conn, ps);
		}
	}
```

**AFTER:**
```
	private void addInfection(Date date, double icd) throws SQLException, DBException {
		try (Connection conn = factory.getConnection();
			PreparedStatement ps1 = conn.prepareStatement("INSERT INTO officevisits(VisitDate,PatientID, hcpid, hospitalid) VALUES(?, 2, 9000000000, '1')");
			PreparedStatement ps2 = conn.prepareStatement("INSERT INTO ovdiagnosis(VisitID, ICDCode) VALUES(?,?)");
			)
		{
			ps1.setDate(1, new java.sql.Date(date.getTime()));
			ps1.executeUpdate();
			
			ps2.setLong(1, DBUtil.getLastInsert(conn));
			ps2.setDouble(2, icd);
			ps2.executeUpdate();
		}
		catch (SQLException ex){
			throw ex;
		}
	}
```

##### Refactor code using a finally block to use the new try-with-resources.

Refactor the following locations:

* edu.ncsu.csc.itrust.dao.mysql.AccessDAO.getSessionTimeoutMins()
* edu.ncsu.csc.itrust.dao.mysql.PatientDAO.removeAllRepresented(long)
* edu.ncsu.csc.itrust.dao.mysql.PatientDAO.removeAllRepresentee(long)
* edu.ncsu.csc.itrust.unit.dao.AutoIncrementTest.setUp()
* edu.ncsu.csc.itrust.unit.risk.factors.ChildhoodInfectionFactorTest.addInfection(Date, double)
* edu.ncsu.csc.itrust.unit.risk.factors.PriorDiagnosesFactorTest.addInfection(Date, double)
* edu.ncsu.csc.itrust.unit.DBBuilder.executeSQL(List<String>)

## Bug Fixes (40)

* Fix: https://github.ncsu.edu/engr-csc326-staff/iTrust-v21/issues/1
* Fix: https://github.ncsu.edu/engr-csc326-staff/iTrust-v21/issues/2
* Fix: https://github.ncsu.edu/engr-csc326-staff/iTrust-v21/issues/3
* Fix: TBD: BugHunt

## Selenium Tests for Each Bug (40)

In addition to using the Issue Tracker in GitHub, each team must create at least one Selenium test per bug to test the correct functionality.  Note that these tests should not pass until the bug is fixed in Homework 2 Part 2.

## Report (20)

You must include the following report:

1. **Problem Statement:** A one or two paragraph description, using complete sentences and proper grammar, of the requirement modifications, additions, or enhancements. The problem statement should not include information about why the changes are made, but is an overview of the portion of iTrust that you are designing.  This is the introduction to the project document, and sets up the rest of information that the document conveys. 
2. **Design Decisions**: Describe the decisions that you made when making fix.  This is a high level overview of the changes you’re making. Describe the changes at all layers of the system (JSP, Action, DAO, SQL).
3. **Black Box Test Plan**: Provide details about how to test that your fix is made correct and the automated test inputs and test cases needed.

## Deadlines

Please submit the following by the given deadlines:

#### HW2.P1

**Due:** Feb 10, 2016, midnight

1. Complete your creating tasks in your task tracking and you have made some progress on tasks.
2. Submit to your repo the 7 try-with-resources refactorings.
3. Indicate in a README.md, the code smells you've found. Include code snippets and written descriptions.
4. Write selenium test scripts for testing bug fixes (do not have to pass yet). 

#### HW2.P2

**Due:** Feb 17, 2016, midnight

1. All tasks should be marked complete in task tracker.
2. Implement bug fixes.
3. New selenium test scripts must pass.
4. Your project must build and pass all tests in Jenkins in order to receive full credit.
5. Report, detailing problem statement, design decision, and new black box tests.
