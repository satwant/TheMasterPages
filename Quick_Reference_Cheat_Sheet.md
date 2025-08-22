# InsuranceSuite Developer Fundamentals - Quick Reference Cheat Sheet

## 🚀 Exam Day Quick Reference

### 📊 Exam Details
- **Duration**: 90 minutes
- **Questions**: 60-75 questions  
- **Passing Score**: 70%
- **Format**: Multiple choice, scenarios, code analysis

---

## 🏗️ Core Architecture

### InsuranceSuite Applications
- **PolicyCenter**: Policy administration & underwriting
- **BillingCenter**: Billing & payment processing  
- **ClaimCenter**: Claims management & processing
- **Architecture**: Service-oriented with shared components

### Key Components
- **EDM**: Entity Data Model - defines business entities
- **PCF**: Page Configuration Framework - UI configuration
- **GOSU**: Programming language for customizations
- **Business Rules Engine**: Configurable business logic

---

## 💻 GOSU Essentials

### Basic Syntax
```gosu
// Variables
var name : String = "value"
var number = 42  // Type inference

// Collections
var list : List<String> = {"a", "b", "c"}
var map : Map<String, Object> = {"key" -> "value"}

// Null Safety
var result = object?.property ?: "default"

// Loops
for (item in collection) { }
for (i in 0..|10) { }  // Range 0-9
```

### Enhancement Files
```gosu
enhancement PolicyEnhancement : entity.Policy {
  property get IsHighValue() : boolean {
    return this.TotalPremium > 10000
  }
  
  function customMethod() : String {
    return "Custom logic"
  }
}
```

### Entity Operations
```gosu
// Query
var policies = Query.make(entity.Policy)
  .compare(Policy#Status, Equals, "ACTIVE")
  .select()

// Create
var policy = new Policy() {
  :PolicyNumber = "POL-001",
  :EffectiveDate = Date.Today
}

// Relationships
policy.addToClaims(newClaim)
```

---

## 🎨 PCF (UI Configuration)

### Key Elements
- `<Screen>` - Top-level container
- `<DetailViewPanel>` - Form layouts
- `<ListViewPanel>` - Data tables with sorting/filtering
- `<InputSet>` - Groups of input fields
- `<TextInput>`, `<DateInput>`, `<RangeInput>` - Form controls

### Conditional Logic
```xml
<TextInput id="field" 
           visible="policy.Status == 'ACTIVE'"
           editable="perm.Policy.edit('field')" />
```

### Actions
```xml
<ToolbarButton id="customAction"
               action="doSomething()"
               label="Custom Action" />
```

---

## 🗄️ Data Model (EDM)

### Entity Relationships
- **One-to-Many**: Array property on parent
- **Many-to-One**: Foreign key property  
- **Many-to-Many**: Join entity with two foreign keys

### Type Lists
- System type lists: Predefined (PolicyStatus, ClaimStatus)
- Custom type lists: User-defined dropdown values
- Categories: Grouping mechanism for type lists

### Key Patterns
```gosu
// Access related entities
policy.Claims.where(\c -> c.Status == ClaimStatus.TC_OPEN)

// Navigate relationships
claim.Policy.PrimaryInsured.DisplayName
```

---

## ⚙️ Business Rules & Validation

### Validation Locations
- **Entity Enhancement**: Field/entity level validation
- **PCF**: Client-side validation attributes
- **Business Rules**: Configurable rule engine

### Common Patterns
```gosu
// Validation method
function validateEffectiveDate() : String {
  if (this.EffectiveDate < Date.Today) {
    return "Effective date cannot be in the past"
  }
  return null
}

// Event handler
@EventHandler
function onPolicyCreated(event : PolicyCreatedEvent) {
  // Custom logic
}
```

---

## 🔗 Integration Patterns

### REST APIs
```gosu
var client = new HttpClient()
var request = new HttpRequest() {
  :Url = "https://api.example.com/endpoint",
  :Method = HttpMethod.POST,
  :ContentType = "application/json"
}

var response = client.execute(request)
```

### Message Queues
- **Asynchronous processing**
- **Event-driven architecture** 
- **Error handling & retry**

### Best Practices
- Use Integration Gateway pattern
- Implement proper error handling
- Use credential management system
- Design for scalability

---

## 🎯 Common Exam Traps

### GOSU Gotchas
- **Null pointer exceptions** - Always use safe navigation
- **Collection operations** - Remember `.where()`, `.flatMap()`
- **Enhancement syntax** - `enhancement Name : entity.Type`
- **Type casting** - Use `as Type` for safe casting

### Architecture Misconceptions
- InsuranceSuite is **not** microservices architecture
- PCF is for **UI configuration**, not business logic
- Enhancement files **extend** entities, don't replace them
- Cloud platform favors **configuration over code**

### Integration Mistakes
- Don't embed integration logic in UI (PCF)
- Always use async patterns for external calls
- Implement proper authentication/authorization
- Handle network failures gracefully

---

## 📝 Quick Formulas & Calculations

### Premium Calculations
```gosu
// Basic discount formula
var discountAmount = basePremium * (discountPercent / 100)
var finalPremium = basePremium - discountAmount

// Multi-factor calculation
var totalDiscount = baseDiscount + loyaltyDiscount + volumeDiscount
var finalAmount = originalAmount * (1 - totalDiscount)
```

### Date Operations
```gosu
var futureDate = Date.Today.addDays(30)
var pastDate = Date.Today.addMonths(-6)
var isActive = policy.EffectiveDate <= Date.Today && 
               policy.ExpirationDate > Date.Today
```

---

## 🔍 Debugging Tips

### Common Issues
1. **NullPointerException**: Use `?.` operator
2. **ClassCastException**: Check types before casting
3. **ValidationException**: Implement proper validation
4. **ConcurrentModificationException**: Don't modify collections while iterating

### Debug Patterns
```gosu
// Safe property access
var value = object?.property?.subProperty ?: defaultValue

// Type checking
if (obj typeis SpecificType) {
  var typed = obj as SpecificType
}

// Collection safety
var copy = originalList.copy()
for (item in copy) {
  if (condition) {
    originalList.remove(item)
  }
}
```

---

## 🎓 Last-Minute Review

### Must Remember
1. **Enhancement files** extend existing entities
2. **PCF** is declarative UI configuration
3. **GOSU** is statically-typed, JVM-based
4. **EDM** defines entity relationships
5. **Integration** should be asynchronous
6. **Validation** belongs in enhancement files
7. **Business rules** are configurable
8. **Type lists** provide dropdown values

### Time Management
- **2 minutes per question** average
- **Mark difficult questions** for review
- **Don't spend too long** on any single question
- **Review flagged questions** if time permits

### Final Checklist
- [ ] Understand GOSU syntax and null safety
- [ ] Know PCF components and attributes  
- [ ] Recognize entity relationship patterns
- [ ] Identify proper integration approaches
- [ ] Understand validation best practices
- [ ] Know when to use enhancements vs business rules

---

## 🚀 Success Strategies

1. **Read questions carefully** - Look for key words
2. **Eliminate wrong answers** first
3. **Look for Guidewire best practices** in answers
4. **Consider maintainability** when choosing approaches
5. **Trust your preparation** - don't second-guess

**You've got this! 💪**

---

*Print this cheat sheet and keep it handy for quick review before your exam. Good luck! 🍀*