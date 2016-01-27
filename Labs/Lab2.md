### Lab

Review slides on [Coverage](https://docs.google.com/presentation/d/1wCRoa7g_aDY-nmq5ORnQoSbmpuAilHOobB-SXJISdh4/edit#slide=id.p)

**You may work with a partner.**

Use the chance to better understand statement and branch coverage. You will certainly be tested on this material. Practice now.

### Statement Coverage


##### Exercise

What is the statement coverage below

```
public void Simple(int i)
{
   if( i < 0 )
      return;
}
```

How many statements are there?

Answer: It depends. Coverage is typically done at a lower level than you see in your text editor. For example, there is actually an implicit return statement. You could say there are 3 statements. Object constructors can be counted, etc. In examinations, we will not be strict in what you consider a statement.

What is the statement coverage if you have a test suite of Simple(3)?

What is the statement coverage if you have a test suite of Simple(3), Simple(-3)?

##### Exercise

Given a list tweets with retweet count [ getRetweetCount(): 33 ], [getRetweetCount() : 3 ]?
What is the statement coverage below?

```
public Status mostRetweeted(List<Status> list)
{
	int max = 0;
	Status maxStatus = list.get(0);
	for( Status s : list )
	{
		if( max < s.getRetweetCount() )
		{
			max = s.getRetweetCount();
			maxStatus = s;
		}
	}
	return maxStatus;
}
```

Hmm. Does for loops count? Yes. Coverage calculates the dynamic execution of the code. So every iteration of the loop should be evaluated. If just one case hits a statement, that's enough as far as coverage is concerned.

### Branch Coverage

Branch coverage examines the number of branches (or decisions) taken during execution of a program.

Examine this example. You will be asked to extend it below.

##### Example

```
private static int maxValue(int[] chars) {

	if( chars.length == 0 )
	   return 0;

	int max = chars[0];
	for (int i = 0; i < chars.length; ktr++) {
		if (chars[i] > max) {
			max = chars[i];
		}
	}
	return max;
}
```

1. The program graph.
   ![programgraph](imgs/max.PNG)

2. There are 3 decision nodes and 6 branches.

3. For a testsuite with the following test cases, how many branches are covered?

* maxminValue( [1,1,1,1] );
* maxminValue( [3,2] );

**Answer: 4/6**

Why? 

* The true branch of the `.length == 0` is never taken.
* The true branch of `chars[i] > max` is never taken.

##### Exercise

Someone decided to extend the method to calculate the min and max at the same time.

```

// Calculate the min and max value of an array of characters.
// min is stored in [0], max in [1].
private static int[] maxminValue(int[] chars) 
{
	if( chars.length == 0 )
		return null;
	
	int[] minmax = {chars[0], chars[0]};

	for (int i = 0; i < chars.length; i++)
	{
		if (chars[i] > minmax[1]) {
			minmax[1] = chars[i];
		}
		else if( chars[i] < minmax[0] )
		{
			minmax[0] = chars[i];
		}
	}

	return minmax;
}
```

1. Draw the program graph.
2. How many branches and decisions are there in total?
3. What the branch coverage `__ / __` ?


The ability to answer this quesiton will seperate As from Bs on an exam.

## Measuring Coverage.

You can use tools to help you measure coverage of your code and unit tests.

##### Install EclEmma.

http://eclemma.org/

##### Inspecting coverage

In your HW1.P2, run coverage on your code.

Notice an interesting mistake in the unit test. It will actually pass if there are no links with `data-href` found, because rank will still be zero!

![image](https://cloud.githubusercontent.com/assets/742934/12628560/c77f30fc-c511-11e5-97c8-f19ed98dd9c8.png)

The underlying problem is related to a difference in how google renders the search result pages for the Selenium driver and a *real* browser. Instead, another selector can be used to find the appropriate link:

```
List<WebElement> links = this.driver.findElements(By.xpath("//cite"));
for( WebElement link : links )
{
	if( link.getAttribute("data-href").equals("http://agile.csc.ncsu.edu/iTrust/wiki"))
		break;
	rank++;
}
```

![image](https://cloud.githubusercontent.com/assets/742934/12628525/94e88774-c511-11e5-8868-2105e1cc16e1.png)

##### Reports

You can view reports on coverage here:

![image](https://cloud.githubusercontent.com/assets/742934/12628887/521d7fc4-c513-11e5-8336-d3251fd56e26.png)

### HW1.P2

Spend time on completing your HW1.P2 assignment.
