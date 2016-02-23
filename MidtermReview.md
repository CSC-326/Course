### Multiple Choice

1) Which of the following is a component of the Blackboard pattern?
- A) routine
- B) sink
- C) knowledge source
- D) repository
- E) None of the above

2) What type of architecture does pipe and filter fall under?
- A) data centered
- B) call and return
- C) data flow
- D) event-based
- E) None of the above

3) When adding or modifying code, what type of testing is done to ensure that existing functionality was
not broken as part of the change?
- A) Unit
- B) Integration
- C) System/Functional
- D) Acceptance
- E) Regression

4) Statement coverage is **subsumed by**

- A) Condition coverage.
- B) Branch and Condition coverage.
- C) MC/DC.
- D) B and C.
- E) None of the above.

5) What is a *NOT* a benefit of pair programming?
- A) Higher product quality
- B) Enhanced learning
- C) Improved cycle time
- D) All of the above
- E) None of the above

6) A Visitor is best described as a:
- A) A customer that works closely with developers
- B) A framework for scheduling REST api calls
- C) A data flow architectural style
- D) A behavioral design pattern 
- E) None of the above

### Short Answer

1) Name and define 3 corollary agile practices. 

2) Name and define 3 traditional configuration management practices. 

3) Name and define 3 event-based architecture patterns. 

4) Name and define 3 ORM styles. 


### Scenarios


1) A developer is constantly deleting unit tests that break when making code changes. What is one strategy that you can recommend to help? (6 points)


2) A developer tells you, we don't need a mocking, it will only add more complexity to the program. What is one reason you might give in support of using mocking? 


### Implementation

1) Code Smells and Refactoring

A) Identify 3 code smells in `withdraw()`. For each code smell, indicate where it occurs, and why it is a code smell.
<br>
<br>
<br>

B) Identify 2 refactoring methods that might help address the identified code smells. How?
<br>
<br>
<br>

C) Identify 2 design patterns that might help improve this code snippet. Why?
<br>
<br>
<br>

```
1    public void withdraw(final Money money) {
2        if (account.getType().isPremium()) {
3            if (account.isOverdraft()) {
4                account.substract(Money.newInstance(money.getAmount() 
                                   + money.getAmount() * account.overdraftFee(),
5                        money.getCurrency()));
6            } else {
7                account.substract(Money.newInstance(money.getAmount(), money.getCurrency()));
8            }
9        } else {
10            if (account.isOverdraft()) {
11                account.substract(Money.newInstance(money.getAmount() 
                                  + money.getAmount() * account.overdraftFee(),
12                        money.getCurrency()));
13            } else {
14                account.substract(Money.newInstance(money.getAmount(), money.getCurrency()));
15            }
16        }
17
18        if( account.getType().isPremium() )
19        {
20            Money amount = Money.newInstance(money.getAmount(), money.getCurrency());
21            if (!account.isOverdraft()) 
22            {
23            	double bonusPoints = 10;
24            	if ( amount.getAmount() > 100.0) {
25             	   result += bonusPoints * 1.2;
26            	}
27            	else if ( amount.getAmount() > 500.0) {
28            	    result += bonusPoints * 1.4;
29            	}
30           }
31        }
32
33		String fullName = account.getCustomer().getFullName();
34        String accountDescription = "";
35        accountDescription += "Account: IBAN: " 
                 + account.getIban() + ", Money: " + account.getMoneyAmount()
36        return fullName + accountDescription;
37    }
```

2) Measure branch coverage of a code snippet.

  **Test suite**:
  
  * numbers(0, 0, 0, "strictly")
  * numbers(88, 42, 42, "stricter")

```Java
public int void numbers(int x,int y, int z, string mode)
{
      if( x > 87 && y < 70 )
      {
          z = 33;
      }    
      else if( z < 42 )
      {
          if( mode == "strictly" )
          {
              return 0;
          }
      }
      else
      {
          if( mode != "stricter" )
          {
              return y = z / x;
          }
      }
      return 1;
  }
```

### Analyze

1) Two developers are debating whether a Model View ViewModel (MVVM) or a Model View Controller (MVC -- An architectural pattern variant of the Observer design pattern) is more appropriate for a data-entry heavy program with occasional visualization components. Describe some benefits of Model View ViewModel over MVC. Describe some disadvantages of MVVM.

2) Two developers are debating about what type of configuration management to use in their organization: traditional or modern. Provide two advantages of traditional CM and two limitations. Provide two advantages of modern CM and two limitations.
