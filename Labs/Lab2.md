### Lab

[Coverage](https://docs.google.com/presentation/d/1wCRoa7g_aDY-nmq5ORnQoSbmpuAilHOobB-SXJISdh4/edit#slide=id.p)


### Branch Coverage

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


## Measuring Coverage.

You can use tools to help you measure coverage of your code and unit tests.

##### Install EclEmma.

##### Inspecting coverage

In your HW1.P2, run coverage on your tool.

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
