# InsuranceSuite Developer Fundamentals - Practice Questions & Mock Exam

## 📋 Exam Format Overview
- **Duration**: 90 minutes
- **Questions**: 60-75 questions
- **Passing Score**: 70%
- **Question Types**: Multiple choice, scenario-based, code analysis

---

## 🎯 Section 1: Guidewire Architecture & Platform (15-20 questions)

### Question 1
Which of the following best describes the Guidewire InsuranceSuite architecture?

A) Monolithic architecture with shared database
B) Microservices architecture with separate databases for each application
C) Service-oriented architecture with shared components and separate application databases
D) Client-server architecture with thick clients

**Answer: C**
*Explanation: InsuranceSuite uses a service-oriented architecture where applications like PolicyCenter, BillingCenter, and ClaimCenter share common components but maintain separate databases.*

### Question 2
What is the primary purpose of PolicyCenter in the InsuranceSuite?

A) Claims processing and settlement
B) Policy administration and underwriting
C) Billing and payment processing
D) Reinsurance management

**Answer: B**

### Question 3
Which component is responsible for managing the user interface in Guidewire applications?

A) GOSU Runtime Engine
B) Page Configuration Framework (PCF)
C) Entity Data Model (EDM)
D) Business Rules Engine

**Answer: B**

### Question 4
In Guidewire Cloud platform, what is the recommended approach for customizations?

A) Direct modification of base application code
B) Configuration-first approach with minimal custom code
C) Complete replacement of standard functionality
D) Only UI customizations are allowed

**Answer: B**

---

## 🏗️ Section 2: Data Model & Configuration (15-20 questions)

### Question 5
What is the Entity Data Model (EDM) in Guidewire?

A) A visual representation of database tables
B) A configuration framework for defining business entities and their relationships
C) A reporting tool for data analysis
D) A security model for user permissions

**Answer: B**

### Question 6
Which of the following is the correct way to define a one-to-many relationship in the EDM?

A) Using foreign key arrays
B) Using array properties on the parent entity
C) Using join tables for all relationships
D) Using embedded objects

**Answer: B**

### Question 7
Consider this entity relationship scenario:
```
Policy (1) → (Many) Claims
Claim (1) → (Many) ClaimContacts
```

How would you access all contacts for all claims of a policy in GOSU?

A) `policy.Claims.ClaimContacts`
B) `policy.Claims*.ClaimContacts`
C) `policy.Claims.flatMap(\c -> c.ClaimContacts)`
D) Both B and C are correct

**Answer: D**

### Question 8
What is the purpose of type lists in Guidewire?

A) To define custom data types
B) To provide predefined values for entity properties
C) To create database indexes
D) To manage user permissions

**Answer: B**

---

## 💻 Section 3: GOSU Programming (20-25 questions)

### Question 9
What is the output of the following GOSU code?
```gosu
var numbers : List<Integer> = {1, 2, 3, 4, 5}
var result = numbers.where(\n -> n % 2 == 0).sum()
print(result)
```

A) 15
B) 6
C) 9
D) 10

**Answer: B**
*Explanation: Filters even numbers (2, 4) and sums them: 2 + 4 = 6*

### Question 10
Which of the following is the correct syntax for creating an enhancement file?

A) `class PolicyEnhancement extends entity.Policy`
B) `enhancement PolicyEnhancement : entity.Policy`
C) `extend entity.Policy with PolicyEnhancement`
D) `enhance Policy { ... }`

**Answer: B**

### Question 11
What is the purpose of the safe navigation operator (?.) in GOSU?

A) To perform null checks automatically
B) To navigate through collections safely
C) To handle exceptions automatically
D) To optimize performance

**Answer: A**

### Question 12
Consider this GOSU code:
```gosu
var policy : Policy = null
var policyNumber = policy?.PolicyNumber ?: "UNKNOWN"
```
What will be the value of `policyNumber`?

A) null
B) Empty string
C) "UNKNOWN"
D) Runtime exception

**Answer: C**

### Question 13
Which method would you use to find all active policies for a specific account?

A) `Account.findActivePolicies()`
B) `Query.make(entity.Policy).compare(Policy#Account, Equals, account).select()`
C) `Policy.findByAccount(account)`
D) `account.Policies.where(\p -> p.Status == "ACTIVE")`

**Answer: D** (assuming policies are already loaded) or **B** for database query

---

## 🔧 Section 4: UI Configuration & PCF (10-15 questions)

### Question 14
What does PCF stand for in Guidewire development?

A) Policy Configuration Framework
B) Page Configuration Framework
C) Process Control Framework
D) Platform Configuration Framework

**Answer: B**

### Question 15
Which PCF element would you use to display a list of claims with sorting and filtering capabilities?

A) `<DetailViewPanel>`
B) `<InputSet>`
C) `<ListViewPanel>`
D) `<CardViewPanel>`

**Answer: C**

### Question 16
How do you make a field conditionally visible in PCF?

A) Using the `visible` attribute with a boolean expression
B) Using JavaScript to hide/show elements
C) Using CSS display properties
D) Using server-side rendering conditions

**Answer: A**

### Question 17
What is the correct way to add a custom action button to a screen?

A) Modify the base PCF file directly
B) Create a new PCF file that extends the base screen
C) Add JavaScript event handlers
D) Use CSS to overlay buttons

**Answer: B**

---

## ⚙️ Section 5: Business Rules & Validations (8-12 questions)

### Question 18
Where should you implement field-level validation in Guidewire?

A) In the database constraints
B) In the PCF files
C) In entity enhancement files
D) In the business rules engine

**Answer: C**

### Question 19
What is the correct way to implement a validation that prevents saving a policy with an effective date in the past?

A) Add a database constraint
B) Create a validation method in the Policy enhancement
C) Add client-side JavaScript validation
D) Use PCF validation attributes

**Answer: B**

### Question 20
Which annotation is used to mark a method as an event handler in GOSU?

A) `@EventListener`
B) `@EventHandler`
C) `@OnEvent`
D) `@HandleEvent`

**Answer: B**

---

## 🔗 Section 6: Integration Techniques (8-12 questions)

### Question 21
What is the recommended approach for integrating with external REST APIs in Guidewire?

A) Direct HTTP calls from entity methods
B) Using the Integration Gateway pattern
C) Embedding API calls in PCF files
D) Using database triggers

**Answer: B**

### Question 22
Which of the following is true about message queues in Guidewire?

A) They are only used for internal communication
B) They provide asynchronous processing capabilities
C) They require custom database tables
D) They are not supported in cloud deployments

**Answer: B**

### Question 23
How should you handle authentication when calling external web services?

A) Hard-code credentials in the source code
B) Store credentials in configuration files
C) Use Guidewire's credential management system
D) Pass credentials as URL parameters

**Answer: C**

---

## 🎮 Scenario-Based Questions

### Scenario 1: Policy Premium Calculation
You need to implement a premium calculation that applies different discounts based on policy characteristics.

**Question 24**
Given this requirement: "Apply a 5% discount for customers with no claims in the past 3 years", which approach is most appropriate?

A) Modify the base premium calculation directly
B) Create an enhancement method that checks claim history and applies discount
C) Use a business rule to modify the premium
D) Add the logic to the PCF file

**Answer: B**

### Scenario 2: Data Integration
Your organization needs to sync policy data with an external CRM system whenever a policy is created or updated.

**Question 25**
What is the best architectural approach for this requirement?

A) Add sync logic directly to policy save methods
B) Use event handlers to trigger async integration processes
C) Create a batch job that runs periodically
D) Use database triggers to detect changes

**Answer: B**

### Scenario 3: UI Customization
Users need a new screen to display policy analytics with charts and graphs.

**Question 26**
How should you implement this requirement?

A) Modify existing policy detail screens
B) Create a new PCF file with custom visualization components
C) Use external reporting tools exclusively
D) Implement as a separate web application

**Answer: B**

---

## 🧠 Code Analysis Questions

### Question 27
Analyze this GOSU code and identify the issue:

```gosu
function calculatePremium(policy : Policy) : BigDecimal {
  var basePremium = policy.BasePremium
  var discountPercent = policy.Account.DiscountPercent
  
  return basePremium - (basePremium * discountPercent / 100)
}
```

A) Syntax error in the calculation
B) Missing null safety checks
C) Incorrect return type
D) No issues present

**Answer: B**
*Explanation: The code doesn't handle cases where policy, BasePremium, Account, or DiscountPercent might be null.*

### Question 28
What will this GOSU code return?

```gosu
var policies = account.Policies.where(\p -> p.Status == PolicyStatus.TC_ACTIVE)
return policies.HasElements
```

A) The number of active policies
B) True if there are any active policies, false otherwise
C) The first active policy
D) A list of active policies

**Answer: B**

---

## 📊 Mock Exam Summary

### Scoring Guide:
- **25-28 correct (89-100%)**: Excellent preparation, ready for certification
- **21-24 correct (75-86%)**: Good preparation, review weak areas
- **18-20 correct (64-71%)**: Additional study needed, focus on gaps
- **Below 18 (< 64%)**: Significant preparation required

### Key Areas to Focus On:
1. **GOSU Programming**: Syntax, enhancements, null safety
2. **Entity Data Model**: Relationships, queries, configurations
3. **Integration Patterns**: REST APIs, messaging, event handling
4. **PCF Configuration**: UI components, validation, conditional logic
5. **Business Logic**: Validation rules, calculations, event handlers

---

## 📚 Additional Practice Resources

### Online Practice Tests:
- Guidewire Community Forums
- Professional training providers
- Study group practice sessions

### Hands-On Practice:
- Set up a Guidewire development environment
- Create sample enhancement files
- Build custom PCF screens
- Implement integration scenarios

### Study Tips:
1. Practice coding in GOSU daily
2. Review official Guidewire documentation
3. Join developer community discussions
4. Create your own practice projects
5. Take timed practice exams

**Good luck with your certification exam! 🚀**