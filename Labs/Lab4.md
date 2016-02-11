
### Pair synchronization (10 minutes)

Update your progress on Trello/Github issues assigned on HW2.

### Tomcat Deployment via Maven

##### Tomcat manager

Ensure your tomcat installation has access to a manager. Yes? Please skip on to next section.
http://localhost:8080/manager/status

If not... then first try [Eric's Maven instructions](https://github.ncsu.edu/ewhorton/iTrust-Resources/wiki/MavenConfiguration).

If *still* not... then try these steps.

* If you don't click "Use tomcat installation" as in Eric's instructions, then you'll have to find Eclipse's instance of tomcat located here: C:\Users\Chris\workspace\.metadata\.plugins\org.eclipse.wst.server.core\tmp0\conf\Catalina\localhost
* Check TOMCATDIR\conf\Catalina\localhost. Do you see a manager.xml? If not, create a manager.xml with the following content:

```
     <Context privileged="true" antiResourceLocking="false"
         docBase="${catalina.home}/webapps/manager">
     <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.0\.0\.1" />
     </Context> 
```
* Using Project Explorer in Eclipse. Place the following in Project Explorer > Servers > Tomcat v8.0 Server > tomcat-users.xml
```
<?xml version='1.0' encoding='utf-8'?>
<tomcat-users>  
  <role rolename="manager-gui"/>
  <role rolename="manager-script"/>
  <role rolename="manager-jmx"/>
  <role rolename="manager-status"/>
  <role rolename="admin-gui"/>
  <role rolename="admin-script"/>
  <user username="admin" password="admin" roles="manager-gui, manager-script, manager-jmx, manager-status, admin-gui, admin-script"/>
</tomcat-users>
```

##### Tomcat maven build

Clone https://github.com/CSC-326/JSPDemo.git

* Make sure you have an username and password that matches in your maven's pom.xml

```
<url>http://localhost:8080/manager/text</url>
<server>tomcat7</server>
<username>admin</username>
<password>admin</password>
```
You should be able to run and see this work and deploy to http://localhost:8080/JSPDemo/index.jsp

`mvn compile tomcat7:redeploy`

### JSP

You want to play with running a simple JSP program and connecting it with MySQL.

##### 1. Getting a MySQL Connection

If you go to http://localhost:8080/JSPDemo/users you will see a simple list of users printed out.

1) Modify the [ITrustUsersServlet](https://github.com/CSC-326/JSPDemo/blob/master/src/main/java/servlets/ITrustUsersServlet.java#L28) to print out list of users from the `itrust.users` table and print out each user's `MID`.

This snippet will help you get started.
```
try
      {
    	  Context initCtx = new InitialContext();
	      Context envCtx  = (Context) initCtx.lookup("java:comp/env");
	      DataSource ds   = (DataSource)envCtx.lookup("jdbc/itrust");	  	      
	      
	      try
	      (
		      Connection conn  = ds.getConnection();
		      Statement  stmt  = conn.createStatement();
		      ResultSet result = stmt.executeQuery("SELECT * FROM itrust.users");
	      )
	      {


	      } 
	      catch (SQLException e) 
	      {
	    	  e.printStackTrace();
	      }
	      catch(Exception e)
	      {
	    	  e.printStackTrace();
	      }
      }
      catch (NamingException e) 
      {
		e.printStackTrace();
      }
```

To help you debug your solution, you can right click on your project and click "Debug on Server", and choose an existing server.


2) See if you can get another column displayed on this servlet. But now, print it out in a simple table.


**REFLECTION**: There is some magic setup that helped this connection work out. Check out WEB-INF/web.xml and META-INF/context.xml. This makes it easier to access the connection string without having to hard code it in every file.

You can see all the project setup that was added to JSPDemo in order to support mysql, by checking the diff here: 
https://github.com/CSC-326/JSPDemo/commit/d14e79a4d56bdab26dcb93b278acdfa57e074c12

##### 2. How iTrust does it?


Look at WebRoot/global.jsp. It will declare a global object accessible by all pages:

```
DAOFactory prodDAO = DAOFactory.getProductionInstance(); 
```

Looking at edu.ncsu.csc.itrust.dao.ProductionConnectionDriver, you'll see the following familiar code:

```
	public Connection getConnection() throws SQLException {
		try {
			if (initialContext == null)
				initialContext = new InitialContext();
			return ((DataSource) (((Context) initialContext.lookup("java:comp/env"))).lookup("jdbc/itrust"))
					.getConnection();
		} catch (NamingException e) {
			throw new SQLException(("Context Lookup Naming Exception: " + e.getMessage()));
		}
	}
```

This further makes it easy for the entire code base to share one way to get the MySql connection information.

##### Resources

* [How tomcat connects to databases. Configuring web.xml and context.xml](https://www.mulesoft.com/tcat/tomcat-mysql).
* [Getting a data connection from tomcat pooled connection](http://www.java2s.com/Code/Java/JSP/UsingaDataSource.htm)

### REST

For HW3, you will do some interaction with a REST service. This will be your initial introduction to the idea.

**PLEASE REVIEW SLIDES**: https://github.com/CSC-326/Course/raw/master/Slides/RESTAPI_Frameworks.pptx

We'll test being able to use [github's REST api](https://developer.github.com/v3/) for programmatically interacting with repos.

1. Get a token. Go to your profile page on ncsu github.

![image](https://cloud.githubusercontent.com/assets/742934/12955762/8d8ae346-cff2-11e5-83ac-21cae5dc8531.png)

![image](https://cloud.githubusercontent.com/assets/742934/12955783/a741d0b0-cff2-11e5-9f95-4cfebe421756.png)

2. Download and build this sample code:

   1. git clone https://github.com/CSC-326/GithubREST
   2. npm install
   3. edit script.js to replace "YOUR TOKEN" with your generated token.
   4. node script.js

The code makes a call to get all a users repo.

```
   var options = {
		url: 'https://github.ncsu.edu/api/v3/users/' + userName + "/repos",
		method: 'GET',
		headers: {
			"User-Agent": "EnableIssues",
			"content-type": "application/json",
			"Authorization": token
		}
	};
```

3. Replace with your unityId

   HINT: You can debug REST api calls using curl:
   ```curl --request PATCH -H "Authorization: token YOURTOKEN" --data '{"name":"hw4","has_issues":"true"}' https://github.ncsu.edu/api/v3/repos/cjparnin/hw4```


4. Write code for listBranches in a given repo under an owner. See [list branches](https://developer.github.com/v3/repos/#list-branches)

5. Finally, see if you can [create a new repo](https://developer.github.com/v3/repos/#create). Note, you'll have to use the POST verb instead of GET.
