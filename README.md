# KATA bank
Practice code (kata) about a banking system written in Java using BDD.

### Prerequisites
* Reading: https://cucumber.io/school

### Exercise
```feature
Feature: Withdraw from account
  As a client of the bank
  I want to withdraw money from my account
  In order to have cash

  Scenario: An existing client withdraws from his account
    Given an existing client named "pierre-jean" with 100.0 EUR in his account
    When he withdraws 10.0 EUR from his account
    Then the new balance is 90.0 EUR
```

This scenario is written in Gherkin language.
For sake of simplicity, think this application as an ATM.

#### BDD (develoment cycle)
BDD is an extension to TDD. As you can see, the requirements are still in low level of detail. In order to have more detail, in Cucumber method, we organize a little meeting among 3 amigos (tester, developer, product owner) and we did a good stuff, called "example mapping" to have an "shared understanding" and produce an "ubiquitous language" for this tiny project.

After the meeting, we resolved a user story "withdraw" and its output are the expected test cases and a low tech language for everyone. So cool until now, thank god.
```feature
Scenario Outline: A user withdraws money from an ATM

Given <Name> has a valid Credit or Debit card

And their account balance is <OriginalBalance>

When they insert their card

And withdraw <WithdrawalAmount>

Then the ATM should return <WithdrawalAmount>

And their account balance is <NewBalance>

    Examples:
      | Name   | OriginalBalance | WithdrawalAmount | NewBalance |
      | Eric   | 100             | 45               | 55         |
      | Gaurav | 100             | 40               | 60         |
      | Ed     | 1000            | 200              | 800        |

```

With the help of cucumber plugin, we got the generated test cases in Java.
Now, as a developer I feel very comfortable to code my application and put the developed interface into the body of test cases.

I will run these tests and correct my code if they fail, repeat until all tests are successful.

### DDD (Tackling the complexity of software development)

Domain driven design (DDD) is an approach to software development based on two main principles:
1. The primary focus of a software project should be the domain itself and not the technical details
2. Complex domain design should be based on a model.

#### Building blocks
Based on the exercise scenario and the result of our example mapping (BDD), we could easily identify some elements in our domain model:
1. Entities: Account and Widthdraw (transaction)
2. Aggregates: Widthdraws
3. Value Objects: WithdrawalAmount (Cash), OriginalBalance, NewBalance, Name
4. Repositories: ATM application (just a name). In real scenario, it should be a database.
5. Factory: ATM application. In real scenario, it should be a "transaction logging system".
6. Service: ATM application. In real scenario, it could be any banking system (SWIFT, ebanking, etc...)
7. Modules: None (or Withdraw, checking the credit card)

#### Supple design 
Due to the simplicity of scenario, no layered architecture and context mapping is used in this kata.

### Domain model/Conceptual model in simplest form
In the figure, it's the "conceptual" class diagram used to represent the domain model. 
This diagram is made by Visual Paradisgm based on text analysis tool.

In implementation model, the class diagram should be more detail. 

In domain model, only the names and the relations between the concepts (classes) are required.
We also got an ubiquitous language now !!!

![Conceptual class model](/images/conceptual_class_model.png)

The "true" domain model should be represented by many views (diagrams, specifications, requirement text, etc...)
We still miss some views here (sequence diagrams)
But the real magic when combine DDD & BDD is that we could run the test cases automatically to control the quality of code with the scenarios in our ubiquitous lanquage.

### Text analysis
Firstly, I use the textual analysis method (Abbott) to identify the candidate items for my model element (UML).
After evaluation, I stick with the following model:
[Model component: class] 
* Actor: client represented in Account table
* Object: balance represented in Balance table (90.0)
* Operation or Use case: Withdraw
* Instance: "pierre-jean" (name of client) , 10 (withdrawal amount)
* Conditions: existing client (authentication)

### Author
* Anh-Hoang LE
