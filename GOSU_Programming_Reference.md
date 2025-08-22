# GOSU Programming Language Reference for InsuranceSuite

## 🚀 Introduction to GOSU

GOSU is a statically-typed programming language that runs on the Java Virtual Machine (JVM). It's the primary language used for customizing and extending Guidewire InsuranceSuite applications.

## 📝 Basic Syntax

### Variables and Data Types

```gosu
// Variable declarations
var name : String = "John Doe"
var age : int = 30
var salary : BigDecimal = 75000.00
var isActive : boolean = true

// Type inference (GOSU can infer types)
var inferredString = "Hello World"  // String type inferred
var inferredNumber = 42             // int type inferred

// Constants
static final var PI : double = 3.14159
```

### Collections

```gosu
// Lists
var names = new ArrayList<String>()
names.add("Alice")
names.add("Bob")

// Alternative syntax
var numbers : List<Integer> = {1, 2, 3, 4, 5}

// Maps
var personData = new HashMap<String, Object>()
personData.put("name", "John")
personData.put("age", 30)

// Alternative syntax
var config : Map<String, String> = {
  "host" -> "localhost",
  "port" -> "8080",
  "protocol" -> "https"
}
```

### Control Structures

```gosu
// If-else statements
if (age >= 18) {
  print("Adult")
} else if (age >= 13) {
  print("Teenager")
} else {
  print("Child")
}

// Switch statements
switch (status) {
  case "ACTIVE":
    processActivePolicy()
    break
  case "EXPIRED":
    handleExpiredPolicy()
    break
  default:
    logUnknownStatus()
}

// Loops
for (name in names) {
  print(name)
}

for (i in 0..|10) {  // Range from 0 to 9
  print("Number: " + i)
}

while (condition) {
  // do something
}
```

## 🏗️ Object-Oriented Programming

### Classes and Inheritance

```gosu
// Base class
class Person {
  var _name : String
  var _age : int
  
  construct(name : String, age : int) {
    _name = name
    _age = age
  }
  
  property get Name() : String {
    return _name
  }
  
  property set Name(value : String) {
    _name = value
  }
  
  function getDisplayName() : String {
    return "Person: " + _name
  }
}

// Inheritance
class Employee extends Person {
  var _employeeId : String
  var _department : String
  
  construct(name : String, age : int, empId : String, dept : String) {
    super(name, age)
    _employeeId = empId
    _department = dept
  }
  
  override function getDisplayName() : String {
    return "Employee: " + _name + " (" + _employeeId + ")"
  }
  
  function getDepartment() : String {
    return _department
  }
}
```

### Interfaces

```gosu
interface Processable {
  function process() : void
  function validate() : boolean
}

class PolicyProcessor implements Processable {
  override function process() : void {
    // Implementation
  }
  
  override function validate() : boolean {
    // Validation logic
    return true
  }
}
```

## 🔧 Enhancement Files

Enhancement files are a powerful GOSU feature that allows you to add methods and properties to existing classes without modifying the original class.

### Creating Enhancement Files

```gosu
// File: PolicyEnhancement.gsx
enhancement PolicyEnhancement : entity.Policy {
  
  // Add a custom property
  property get IsHighValue() : boolean {
    return this.TotalPremium > 10000
  }
  
  // Add a custom method
  function calculateDiscountAmount(discountPercent : double) : BigDecimal {
    return this.TotalPremium * (discountPercent / 100)
  }
  
  // Add validation logic
  function validatePolicyLimits() : String {
    if (this.PolicyLimit < 50000) {
      return "Policy limit too low"
    }
    return null  // No validation errors
  }
  
  // Override existing behavior (use carefully)
  function customBusinessLogic() : void {
    // Custom implementation
    if (this.IsHighValue) {
      // Special handling for high-value policies
      this.assignToSeniorUnderwriter()
    }
  }
}
```

### Using Enhancements

```gosu
// In your business logic
var policy = new Policy()
policy.TotalPremium = 15000

// Use enhanced properties and methods
if (policy.IsHighValue) {
  var discount = policy.calculateDiscountAmount(5.0)
  print("Discount amount: " + discount)
}

var validationError = policy.validatePolicyLimits()
if (validationError != null) {
  throw new ValidationException(validationError)
}
```

## 🔍 Working with Entities

### Entity Access and Manipulation

```gosu
// Finding entities
var policies = Query.make(entity.Policy)
  .compare(Policy#Status, Equals, "ACTIVE")
  .compare(Policy#EffectiveDate, LessThanOrEquals, Date.Today)
  .select()

// Creating new entities
var newPolicy = new Policy() {
  :PolicyNumber = generatePolicyNumber(),
  :EffectiveDate = Date.Today,
  :Status = PolicyStatus.TC_DRAFT,
  :Product = findProductByCode("AUTO")
}

// Updating entities
policy.Status = PolicyStatus.TC_ACTIVE
policy.LastModified = Date.Now

// Working with related entities
for (coverage in policy.Coverages) {
  if (coverage.CoverageType == "LIABILITY") {
    coverage.Limit = coverage.Limit * 1.1  // Increase by 10%
  }
}
```

### Entity Relationships

```gosu
// One-to-many relationships
var policy = findPolicyByNumber("POL-12345")
for (claim in policy.Claims) {
  if (claim.Status == ClaimStatus.TC_OPEN) {
    processOpenClaim(claim)
  }
}

// Many-to-one relationships
var claim = findClaimByNumber("CLM-67890")
var policy = claim.Policy
var insured = policy.PrimaryInsured

// Creating related entities
var newClaim = new Claim() {
  :ClaimNumber = generateClaimNumber(),
  :Policy = policy,
  :LossDate = Date.Today,
  :Status = ClaimStatus.TC_DRAFT
}

policy.addToClaims(newClaim)
```

## 🎯 Common Patterns and Best Practices

### Null Safety

```gosu
// Safe navigation operator
var policyNumber = policy?.PolicyNumber
var insuredName = policy?.PrimaryInsured?.DisplayName

// Null coalescing
var displayName = person.PreferredName ?: person.FirstName ?: "Unknown"

// Null checks
if (policy != null and policy.Status != null) {
  processPolicy(policy)
}
```

### Exception Handling

```gosu
try {
  var result = performRiskyOperation()
  return result
} catch (ex : ValidationException) {
  logValidationError(ex.Message)
  throw ex  // Re-throw if needed
} catch (ex : Exception) {
  logGenericError("Unexpected error: " + ex.Message)
  return getDefaultValue()
} finally {
  cleanupResources()
}
```

### Working with Dates

```gosu
// Date creation and manipulation
var today = Date.Today
var futureDate = today.addDays(30)
var pastDate = today.addMonths(-6)

// Date comparisons
if (policy.EffectiveDate <= Date.Today and 
    policy.ExpirationDate > Date.Today) {
  // Policy is currently active
}

// Date formatting
var formattedDate = today.format("MM/dd/yyyy")
```

### String Operations

```gosu
// String concatenation
var fullName = firstName + " " + lastName

// String interpolation (template strings)
var message = "Policy ${policyNumber} for ${insuredName} is ${status}"

// String methods
var upperName = name.toUpperCase()
var trimmedValue = input.trim()
var isValidFormat = phoneNumber.matches("\\d{3}-\\d{3}-\\d{4}")
```

## 🧪 Testing in GOSU

### Unit Testing

```gosu
// Test class
class PolicyCalculationTest extends GosuTestCase {
  
  function testPremiumCalculation() {
    // Arrange
    var policy = createTestPolicy()
    policy.BasePremium = 1000
    
    // Act
    var totalPremium = policy.calculateTotalPremium()
    
    // Assert
    assertEquals(1150, totalPremium)  // Including fees and taxes
  }
  
  function testValidationRules() {
    var policy = createInvalidPolicy()
    
    try {
      policy.validate()
      fail("Expected validation exception")
    } catch (ex : ValidationException) {
      assertTrue(ex.Message.contains("Invalid policy limit"))
    }
  }
  
  private function createTestPolicy() : Policy {
    return new Policy() {
      :PolicyNumber = "TEST-001",
      :EffectiveDate = Date.Today,
      :ExpirationDate = Date.Today.addYears(1)
    }
  }
}
```

## 🔄 Integration Patterns

### REST API Calls

```gosu
// Making HTTP requests
function callExternalService(policyData : PolicyData) : String {
  var client = new HttpClient()
  var request = new HttpRequest() {
    :Url = "https://api.external-service.com/validate",
    :Method = HttpMethod.POST,
    :ContentType = "application/json"
  }
  
  request.setBody(policyData.toJSON())
  
  var response = client.execute(request)
  
  if (response.StatusCode == 200) {
    return response.Body
  } else {
    throw new IntegrationException("API call failed: " + response.StatusCode)
  }
}
```

### Event Handling

```gosu
// Event listener
class PolicyEventListener {
  
  @EventHandler
  function onPolicyCreated(event : PolicyCreatedEvent) : void {
    var policy = event.Policy
    
    // Send notification
    sendWelcomeEmail(policy.PrimaryInsured.EmailAddress)
    
    // Update external systems
    updateCRMSystem(policy)
  }
  
  @EventHandler  
  function onClaimSubmitted(event : ClaimSubmittedEvent) : void {
    var claim = event.Claim
    
    // Automatic assignment logic
    if (claim.LossAmount > 50000) {
      claim.assignToSeniorAdjuster()
    }
  }
}
```

## 📋 Exam Tips for GOSU

### Key Concepts to Master

1. **Enhancement Files** - Know how to create and use them
2. **Entity Manipulation** - CRUD operations, relationships
3. **Type System** - Understanding GOSU's type safety
4. **Collections** - Lists, Maps, and iteration
5. **Exception Handling** - Try-catch-finally blocks
6. **Null Safety** - Safe navigation and null coalescing
7. **Properties vs Methods** - When to use each

### Common Exam Scenarios

```gosu
// Scenario: Calculate policy premium with discounts
function calculateFinalPremium(policy : Policy) : BigDecimal {
  var basePremium = policy.BasePremium
  var totalDiscount = 0.0
  
  // Multi-policy discount
  if (policy.Account.Policies.Count > 1) {
    totalDiscount += 0.05  // 5% discount
  }
  
  // Good driver discount
  if (policy.PrimaryInsured.DrivingRecord.ViolationsCount == 0) {
    totalDiscount += 0.10  // 10% discount
  }
  
  var discountAmount = basePremium * totalDiscount
  return basePremium - discountAmount
}

// Scenario: Validation with multiple conditions
function validatePolicy(policy : Policy) : List<String> {
  var errors = new ArrayList<String>()
  
  if (policy.EffectiveDate == null) {
    errors.add("Effective date is required")
  }
  
  if (policy.ExpirationDate != null and 
      policy.EffectiveDate != null and
      policy.ExpirationDate <= policy.EffectiveDate) {
    errors.add("Expiration date must be after effective date")
  }
  
  if (policy.Coverages.IsEmpty) {
    errors.add("At least one coverage is required")
  }
  
  return errors
}
```

Remember: Focus on understanding the concepts and patterns rather than memorizing syntax. The exam will test your ability to solve problems using GOSU effectively!