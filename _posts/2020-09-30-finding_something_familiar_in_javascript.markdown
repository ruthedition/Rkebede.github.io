---
layout: post
title:      "Finding Something Familiar in JavaScript"
date:       2020-09-30 18:32:11 +0000
permalink:  finding_something_familiar_in_javascript
---


I'm not going to lie, JavaScript was brutal. With every line of code I found my self wanting to go back to the comforts of Ruby. All the structure and simplicity that allowed you to write things and debug them without trying to figure out why half your page is either undefined or the batman theme song. 

Finally giving in I tried creating my own structure and organization. I added way to many spaces between functions and a lot of really detailed titles for those functions, and made sure they only had one job. As you can imagine this became exhausting and I began to find a new hatered for JavaScript. 

## Something Familiar

But towards the end there was some hope when we learned about Class Syntax. If we're being honest it's just a function with a bunch of functions in it, but it made me feel at ease seeing something so familiar. Classes were everything in Ruby and I was able to find peace with JavaScript. I was no longer adding spaces and stratigically grouping functions and having long titles. I was able to put all the functions that belonged to an object in one place. 

During my project I became aware of just how powerful JavaScript can really be, and my liking of it gradually grew. I was able to not only create an instance of a class, but I was then able to create another instance of a different class within it that was able to act independently when accessed. 

## Realize the Potential

Creating an instance in an instance that has the ability to act indpendently already sounds confusing, so let me show you the point where it all made sense. 

Here I am creating a budget instance using my Budget Class with the JSON budget object that came from my database. 

```
const budget = new Budget(budgetObj.id, budgetObj.income)
```

In my Budget Class I also have access to expenses which is set to an empty array upon creation, so I have the ability to redefine what expenses is.

```
budget.expenses = Expense.createExpensesForBudget(budgetObj)
```

I have redefined expenses in the budget instance to be whatever is returned from the function `createExpensesForBudget()` in the Expense class.

```
static createExpensesForBudget(budget) {
    return budget.expenses.map(expenseObj => {
      return new Expense(expenseObj.id, expenseObj.name, budget.id, expenseObj.amount)
    })
  }
	
```
	
Taking in a JSON representation of a budget, the function goes through and for each expense object in that budget objects expenses array it creates a new expense instance and fills it in with all the information that was available from the budget object that it was given! 


## Getting to know JavaScript

For the time being I don't think I can say I love JavaScript, but I can say I can see how powerful it is and I respect that. I will take comfort in the syntactic sugar that is Classes and use it to stay hopeful until I understand the depth of JavaScript and all that it really can do for us. 



