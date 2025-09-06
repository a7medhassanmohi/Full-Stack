# Lesson 2: Advanced JavaScript & Node.js Environment Setup

## Learning Objectives
- Master JavaScript ES6+ Features with comprehensive syntax understanding
- Install and configure Node.js development environment properly
- Build your first API using raw Node.js with detailed explanations
- Use Bash scripts for project management automation
- Understand modern JavaScript development patterns and best practices

---

## 1. JavaScript ES6+ Features: Syntax First, Then Examples

### 1.1 Variable Declarations: let, const vs var

#### Syntax Overview:
```javascript
// Variable declaration syntax
var variableName = value;     // Function-scoped, hoisted
let variableName = value;     // Block-scoped, not hoisted
const CONSTANT_NAME = value;  // Block-scoped, immutable reference
```

#### Key Differences:
1. **Scope**: `var` is function-scoped, `let` and `const` are block-scoped
2. **Hoisting**: `var` is hoisted and initialized with `undefined`, `let`/`const` are hoisted but not initialized
3. **Reassignment**: `var` and `let` can be reassigned, `const` cannot
4. **Redeclaration**: `var` allows redeclaration, `let`/`const` do not

#### Detailed Examples:

```javascript
// Example 1: Understanding Scope
function demonstrateScope() {
    // Function scope with var
    if (true) {
        var functionScoped = "I'm accessible outside this block";
        let blockScoped = "I'm only accessible in this block";
        const alsoBlockScoped = "Me too!";
    }
    
    console.log(functionScoped);    // ✅ Works: "I'm accessible outside this block"
    // console.log(blockScoped);    // ❌ Error: blockScoped is not defined
    // console.log(alsoBlockScoped); // ❌ Error: alsoBlockScoped is not defined
}

// Example 2: Hoisting Behavior
console.log(hoistedVar);    // ✅ undefined (not an error due to hoisting)
// console.log(notHoisted); // ❌ ReferenceError: Cannot access 'notHoisted' before initialization

var hoistedVar = "I was hoisted";
let notHoisted = "I cause an error if accessed before declaration";

// Example 3: Loop Behavior (Classic Interview Question)
// Problem with var in loops:
for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log(`var loop: ${i}`), 100);
    // Prints: 3, 3, 3 (because var is function-scoped)
}

// Solution with let:
for (let i = 0; i < 3; i++) {
    setTimeout(() => console.log(`let loop: ${i}`), 200);
    // Prints: 0, 1, 2 (because let creates new binding each iteration)
}

// Example 4: const with Objects and Arrays
const user = { name: "John", age: 25 };
// user = {}; // ❌ TypeError: Assignment to constant variable

user.name = "Jane";     // ✅ Allowed: modifying object properties
user.skills = ["JS"];   // ✅ Allowed: adding new properties
console.log(user);      // { name: "Jane", age: 25, skills: ["JS"] }

const numbers = [1, 2, 3];
// numbers = [4, 5, 6]; // ❌ TypeError: Assignment to constant variable
numbers.push(4);       // ✅ Allowed: modifying array contents
console.log(numbers);  // [1, 2, 3, 4]

// Example 5: Best Practices
const API_URL = "https://api.example.com";  // Use const for constants
let userCount = 0;                          // Use let when you need to reassign
const users = [];                           // Use const for objects/arrays you'll modify

// Never use var in modern JavaScript - it causes confusion
```

### 1.2 Arrow Functions: Syntax and Behavior

#### Syntax Overview:
```javascript
// Traditional function syntax
function functionName(parameters) {
    return expression;
}

// Arrow function syntaxes (multiple forms)
const functionName = (parameters) => { return expression; };  // Full form
const functionName = (parameters) => expression;              // Implicit return
const functionName = parameter => expression;                 // Single parameter
const functionName = () => expression;                        // No parameters
```

#### Key Differences from Regular Functions:
1. **this binding**: Arrow functions don't have their own `this`
2. **arguments object**: Arrow functions don't have `arguments`
3. **Constructor**: Arrow functions cannot be used as constructors
4. **Hoisting**: Arrow functions are not hoisted (they're expressions)

#### Detailed Examples:

```javascript
// Example 1: Basic Syntax Variations
// Traditional function
function addTraditional(a, b) {
    return a + b;
}

// Arrow function - full syntax
const addArrowFull = (a, b) => {
    return a + b;
};

// Arrow function - implicit return (most common for simple functions)
const addArrowShort = (a, b) => a + b;

// Single parameter - parentheses optional
const double = x => x * 2;
const square = x => x * x;

// No parameters - parentheses required
const getCurrentTime = () => new Date();
const getRandomId = () => Math.random().toString(36).substr(2, 9);

// Multiple statements - braces required, explicit return needed
const processUser = (user) => {
    console.log(`Processing user: ${user.name}`);
    const timestamp = new Date().toISOString();
    const processed = {
        ...user,
        processedAt: timestamp,
        id: getRandomId()
    };
    return processed; // explicit return required with braces
};

// Example 2: this Binding Behavior
const userObject = {
    name: "Alice",
    age: 30,
    
    // Regular function - 'this' refers to userObject
    greetRegular: function() {
        console.log(`Hello, I'm ${this.name} and I'm ${this.age} years old`);
        
        // Nested function problem with regular functions
        function innerFunction() {
            // console.log(this.name); // ❌ 'this' is undefined or refers to global object
        }
        
        // Solution 1: Arrow function preserves 'this'
        const innerArrow = () => {
            console.log(`Inner arrow: ${this.name}`); // ✅ 'this' refers to userObject
        };
        
        innerArrow();
    },
    
    // Arrow function - 'this' refers to enclosing scope (not userObject!)
    greetArrow: () => {
        // console.log(`Hello, I'm ${this.name}`); // ❌ 'this' doesn't refer to userObject
    },
    
    // Practical example: Using arrow functions in methods
    getFormattedInfo: function() {
        const details = ["name", "age"];
        
        // Arrow function preserves 'this' from the enclosing method
        return details.map(detail => `${detail}: ${this[detail]}`);
        
        // If we used regular function here:
        // return details.map(function(detail) {
        //     return `${detail}: ${this[detail]}`; // ❌ 'this' would be undefined
        // });
    }
};

// Example 3: Array Methods with Arrow Functions (Very Common Pattern)
const products = [
    { id: 1, name: "Laptop", price: 999, category: "Electronics", inStock: true },
    { id: 2, name: "Book", price: 15, category: "Education", inStock: true },
    { id: 3, name: "Phone", price: 599, category: "Electronics", inStock: false },
    { id: 4, name: "Desk", price: 299, category: "Furniture", inStock: true }
];

// Filter products in stock
const inStockProducts = products.filter(product => product.inStock);

// Get product names only
const productNames = products.map(product => product.name);

// Find expensive products (over $500)
const expensiveProducts = products.filter(product => product.price > 500);

// Calculate total value of in-stock products
const totalValue = products
    .filter(product => product.inStock)
    .reduce((total, product) => total + product.price, 0);

// Group products by category
const productsByCategory = products.reduce((groups, product) => {
    const category = product.category;
    groups[category] = groups[category] || [];
    groups[category].push(product);
    return groups;
}, {});

// Transform products with discount
const discountedProducts = products.map(product => ({
    ...product,
    originalPrice: product.price,
    price: product.price * 0.9, // 10% discount
    savings: product.price * 0.1
}));

// Example 4: Arrow Functions in Async Programming
const fetchUserData = async (userId) => {
    try {
        const response = await fetch(`/api/users/${userId}`);
        const userData = await response.json();
        return userData;
    } catch (error) {
        console.error(`Failed to fetch user ${userId}:`, error);
        throw error;
    }
};

// Using arrow functions with Promise chains
const processMultipleUsers = (userIds) => {
    return Promise.all(
        userIds.map(id => fetchUserData(id))
    ).then(users => 
        users.map(user => ({
            ...user,
            processedAt: new Date().toISOString()
        }))
    );
};
```

### 1.3 Template Literals: Syntax and Advanced Usage

#### Syntax Overview:
```javascript
// Basic template literal syntax
const string = `This is a template literal`;
const withVariable = `Hello, ${variableName}`;
const multiLine = `This is
a multi-line
string`;

// Tagged template literals (advanced)
const tagged = tagFunction`Template with ${variable}`;
```

#### Key Features:
1. **String interpolation**: `${expression}` embeds expressions
2. **Multi-line strings**: No need for `\n` or string concatenation
3. **Expression evaluation**: Any JavaScript expression works inside `${}`
4. **Tagged templates**: Custom processing of template literals

#### Detailed Examples:

```javascript
// Example 1: Basic Interpolation and Multi-line
const user = {
    firstName: "John",
    lastName: "Doe",
    age: 28,
    email: "john.doe@example.com"
};

// Instead of string concatenation
const oldWay = "Hello, " + user.firstName + " " + user.lastName + 
               "! You are " + user.age + " years old.";

// Template literal way (much cleaner)
const newWay = `Hello, ${user.firstName} ${user.lastName}! You are ${user.age} years old.`;

// Multi-line strings (very useful for HTML, SQL, etc.)
const htmlTemplate = `
    <div class="user-card">
        <h2>${user.firstName} ${user.lastName}</h2>
        <p class="email">${user.email}</p>
        <p class="age">Age: ${user.age}</p>
        <p class="status">Status: ${user.age >= 18 ? 'Adult' : 'Minor'}</p>
    </div>
`;

// SQL query example
const sqlQuery = `
    SELECT u.id, u.name, u.email, p.title
    FROM users u
    LEFT JOIN posts p ON u.id = p.user_id
    WHERE u.age >= ${user.age}
    AND u.active = true
    ORDER BY u.name ASC
    LIMIT 10
`;

// Example 2: Expression Evaluation Inside Template Literals
const product = {
    name: "Laptop",
    price: 999,
    taxRate: 0.08,
    inStock: true
};

const productInfo = `
    Product: ${product.name}
    Price: $${product.price.toFixed(2)}
    Tax: $${(product.price * product.taxRate).toFixed(2)}
    Total: $${(product.price + (product.price * product.taxRate)).toFixed(2)}
    Availability: ${product.inStock ? '✅ In Stock' : '❌ Out of Stock'}
    Discount: ${product.price > 500 ? '10% off for orders over $500!' : 'No discount available'}
`;

// Function calls inside template literals
const formatDate = (date) => {
    return date.toLocaleDateString('en-US', {
        year: 'numeric',
        month: 'long',
        day: 'numeric'
    });
};

const orderDetails = `
    Order Date: ${formatDate(new Date())}
    Estimated Delivery: ${formatDate(new Date(Date.now() + 7 * 24 * 60 * 60 * 1000))}
    Order ID: ${Math.random().toString(36).substr(2, 9).toUpperCase()}
`;

// Example 3: Dynamic Content Generation
const generateUserCard = (user, theme = 'light') => `
    <div class="user-card ${theme}">
        <div class="user-avatar">
            <img src="${user.avatar || '/default-avatar.png'}" alt="${user.name}" />
        </div>
        <div class="user-info">
            <h3>${user.name}</h3>
            <p class="email">${user.email}</p>
            <div class="user-stats">
                <span class="posts">Posts: ${user.postCount || 0}</span>
                <span class="followers">Followers: ${user.followers || 0}</span>
                <span class="joined">Joined: ${formatDate(new Date(user.joinDate))}</span>
            </div>
        </div>
        ${user.isVerified ? '<div class="verified-badge">✓ Verified</div>' : ''}
    </div>
`;

// Example 4: API Response Templates
const createAPIResponse = (success, data, message = null, statusCode = 200) => `{
    "success": ${success},
    "statusCode": ${statusCode},
    ${message ? `"message": "${message}",` : ''}
    "data": ${JSON.stringify(data, null, 2)},
    "timestamp": "${new Date().toISOString()}",
    "requestId": "${Math.random().toString(36).substr(2, 9)}"
}`;

// Usage
const successResponse = createAPIResponse(true, { id: 1, name: "John" }, "User created successfully", 201);
const errorResponse = createAPIResponse(false, null, "User not found", 404);

// Example 5: Tagged Template Literals (Advanced)
// Custom tag function for HTML escaping
const html = (strings, ...values) => {
    const escapeHtml = (str) => {
        return str.toString()
            .replace(/&/g, '&amp;')
            .replace(/</g, '&lt;')
            .replace(/>/g, '&gt;')
            .replace(/"/g, '&quot;')
            .replace(/'/g, '&#39;');
    };
    
    return strings.reduce((result, string, i) => {
        const value = values[i] ? escapeHtml(values[i]) : '';
        return result + string + value;
    }, '');
};

const userInput = '<script>alert("XSS Attack!")</script>';
const safeHtml = html`<div>User said: ${userInput}</div>`;
// Result: <div>User said: &lt;script&gt;alert("XSS Attack!")&lt;/script&gt;</div>

// Custom tag for highlighting search terms
const highlight = (strings, ...values) => {
    return strings.reduce((result, string, i) => {
        const value = values[i] ? `<mark>${values[i]}</mark>` : '';
        return result + string + value;
    }, '');
};

const searchTerm = "JavaScript";
const highlightedText = highlight`Learn ${searchTerm} and become a full-stack developer!`;
// Result: "Learn <mark>JavaScript</mark> and become a full-stack developer!"
```

### 1.4 Destructuring: Syntax and Patterns

#### Syntax Overview:
```javascript
// Object destructuring
const { property1, property2 } = object;
const { property: newName } = object;           // Renaming
const { property = defaultValue } = object;     // Default values
const { nested: { property } } = object;        // Nested destructuring

// Array destructuring
const [first, second] = array;
const [first, , third] = array;                // Skipping elements
const [first, ...rest] = array;                // Rest operator
```

#### Key Features:
1. **Extraction**: Pull out specific values from objects/arrays
2. **Renaming**: Assign extracted values to different variable names
3. **Default values**: Provide fallbacks for undefined properties
4. **Nested destructuring**: Extract from nested objects/arrays
5. **Rest operator**: Collect remaining elements

#### Detailed Examples:

```javascript
// Example 1: Object Destructuring Basics
const user = {
    id: 1,
    firstName: "John",
    lastName: "Doe",
    email: "john.doe@example.com",
    age: 28,
    address: {
        street: "123 Main St",
        city: "New York",
        zipCode: "10001",
        country: "USA"
    },
    preferences: {
        theme: "dark",
        notifications: true,
        language: "en"
    },
    skills: ["JavaScript", "Node.js", "React", "Python"]
};

// Basic extraction
const { firstName, lastName, email } = user;
console.log(firstName); // "John"

// Renaming during extraction
const { firstName: fName, lastName: lName } = user;
console.log(fName); // "John"

// Default values for missing properties
const { phone = "Not provided", age, isActive = false } = user;
console.log(phone); // "Not provided"

// Nested destructuring
const { address: { city, country } } = user;
console.log(city); // "New York"

// Extract both parent and nested properties
const { address, address: { street, zipCode } } = user;
console.log(address); // entire address object
console.log(street); // "123 Main St"

// Rest operator with objects
const { id, email: userEmail, ...otherDetails } = user;
console.log(otherDetails); // all properties except id and email

// Example 2: Array Destructuring Patterns
const colors = ["red", "green", "blue", "yellow", "purple"];
const coordinates = [10, 20, 30];
const matrix = [[1, 2], [3, 4], [5, 6]];

// Basic extraction
const [primary, secondary] = colors;
console.log(primary); // "red"

// Skipping elements
const [first, , third] = colors;
console.log(third); // "blue"

// Rest operator with arrays
const [mainColor, ...remainingColors] = colors;
console.log(remainingColors); // ["green", "blue", "yellow", "purple"]

// Default values
const [x, y, z = 0] = coordinates;
console.log(z); // 0 (from default since coordinates only has 2 elements)

// Nested array destructuring
const [[a, b], [c, d]] = matrix;
console.log(a, b, c, d); // 1, 2, 3, 4

// Swapping variables (elegant solution)
let var1 = "first";
let var2 = "second";
[var1, var2] = [var2, var1];
console.log(var1, var2); // "second", "first"

// Example 3: Function Parameter Destructuring
// Instead of passing entire objects and accessing properties inside
const createUserBadWay = (userObj) => {
    return {
        id: Date.now(),
        name: userObj.name,
        email: userObj.email,
        age: userObj.age || 18,
        role: userObj.role || 'user',
        createdAt: new Date()
    };
};

// Better: Destructure in parameters
const createUser = ({ name, email, age = 18, role = 'user' }) => {
    return {
        id: Date.now(),
        name,
        email,
        age,
        role,
        createdAt: new Date()
    };
};

// Usage
const newUser = createUser({
    name: "Alice Johnson",
    email: "alice@example.com",
    age: 25
});

// Array parameter destructuring
const calculateDistance = ([x1, y1], [x2, y2]) => {
    return Math.sqrt(Math.pow(x2 - x1, 2) + Math.pow(y2 - y1, 2));
};

const distance = calculateDistance([0, 0], [3, 4]); // 5

// Mixed destructuring in parameters
const createProduct = ({
    name,
    price,
    category,
    tags = [],
    dimensions: { width, height, depth } = {},
    availability: { inStock = true, quantity = 0 } = {}
}) => {
    return {
        id: Math.random().toString(36).substr(2, 9),
        name,
        price,
        category,
        tags,
        dimensions: { width, height, depth },
        availability: { inStock, quantity },
        createdAt: new Date()
    };
};

// Example 4: Destructuring in Loops and Array Methods
const products = [
    { id: 1, name: "Laptop", price: 999, category: "Electronics" },
    { id: 2, name: "Book", price: 25, category: "Education" },
    { id: 3, name: "Chair", price: 199, category: "Furniture" }
];

// Destructuring in for...of loops
for (const { name, price, category } of products) {
    console.log(`${name} (${category}): $${price}`);
}

// Destructuring in array methods
const productSummaries = products.map(({ name, price }) => `${name}: $${price}`);

const expensiveProducts = products.filter(({ price }) => price > 100);

const totalValue = products.reduce((sum, { price }) => sum + price, 0);

// Example 5: Advanced Destructuring Patterns
// Computed property names
const field = "email";
const { [field]: userEmail2 } = user;
console.log(userEmail2); // john.doe@example.com

// Destructuring return values from functions
const getUserInfo = () => {
    return {
        profile: { name: "John", age: 30 },
        settings: { theme: "dark" },
        stats: { posts: 42, followers: 150 }
    };
};

const {
    profile: { name: userName },
    settings: { theme },
    stats: { posts, followers }
} = getUserInfo();

// Destructuring with conditional logic
const processUserData = (userData) => {
    const {
        name,
        email,
        preferences: {
            notifications = true,
            theme = "light"
        } = {},
        address = null
    } = userData || {};
    
    return {
        name: name || "Unknown User",
        email: email || "no-email@example.com",
        hasAddress: Boolean(address),
        preferences: { notifications, theme }
    };
};
```

### 1.5 Spread Operator & Rest Parameters: Syntax and Usage

#### Syntax Overview:
```javascript
// Spread operator (expands/spreads elements)
const newArray = [...oldArray, newElement];
const newObject = { ...oldObject, newProperty: value };
functionCall(...argumentsArray);

// Rest parameters (collects elements)
const functionName = (first, ...restParameters) => { };
const [first, ...rest] = array;
const { property, ...rest } = object;
```

#### Key Concepts:
1. **Spread**: Takes an iterable and expands it into individual elements
2. **Rest**: Collects multiple elements into an array or object
3. **Same syntax (...)**: Context determines whether it's spread or rest
4. **Shallow operations**: Only works on the first level of nesting

#### Detailed Examples:

```javascript
// Example 1: Array Spread Operations
const fruits = ["apple", "banana", "orange"];
const vegetables = ["carrot", "lettuce", "tomato"];
const dairy = ["milk", "cheese"];

// Combining arrays (instead of concat)
const groceries = [...fruits, ...vegetables, ...dairy];
console.log(groceries); 
// ["apple", "banana", "orange", "carrot", "lettuce", "tomato", "milk", "cheese"]

// Adding elements while spreading
const extendedGroceries = ["bread", ...fruits, "eggs", ...vegetables];

// Copying arrays (shallow copy)
const fruitsCopy = [...fruits];
fruitsCopy.push("grape");
console.log(fruits);     // ["apple", "banana", "orange"] - unchanged
console.log(fruitsCopy); // ["apple", "banana", "orange", "grape"]

// Converting NodeList to Array (common in DOM manipulation)
// const divElements = [...document.querySelectorAll('div')];

// Finding max/min in array
const numbers = [5, 2, 8, 1, 9, 3];
const maxNumber = Math.max(...numbers); // 9
const minNumber = Math.min(...numbers); // 1

// Spreading strings (each character becomes array element)
const letters = [..."hello"];
console.log(letters); // ["h", "e", "l", "l", "o"]

// Example 2: Object Spread Operations
const baseUser = {
    id: 1,
    name: "John Doe",
    isActive: true
};

const userPreferences = {
    theme: "dark",
    notifications: true,
    language: "en"
};

// Combining objects
const completeUser = {
    ...baseUser,
    ...userPreferences,
    email: "john@example.com",
    createdAt: new Date()
};

// Overriding properties (order matters!)
const updatedUser = {
    ...baseUser,
    name: "John Smith",    // This overwrites the name from baseUser
    age: 30               // This adds a new property
};

// Conditional spreading
const includeEmail = true;
const userWithConditionalEmail = {
    ...baseUser,
    ...(includeEmail && { email: "john@example.com" })
};

// Nested object spreading (be careful - this is shallow!)
const userWithAddress = {
    ...baseUser,
    address: {
        ...baseUser.address, // This would be undefined in our case
        city: "New York",
        zipCode: "10001"
    }
};

// Example 3: Rest Parameters in Functions
// Traditional approach with arguments object
function sumOldWay() {
    let total = 0;
    for (let i = 0; i < arguments.length; i++) {
        total += arguments[i];
    }
    return total;
}

// Modern approach with rest parameters
const sum = (...numbers) => {
    return numbers.reduce((total, num) => total + num, 0);
};

console.log(sum(1, 2, 3, 4, 5)); // 15
console.log(sum(10, 20));         // 30

// Mixed parameters with rest
const greetUsers = (greeting, ...names) => {
    return names.map(name => `${greeting}, ${name}!`);
};

const greetings = greetUsers("Hello", "Alice", "Bob", "Charlie");
// ["Hello, Alice!", "Hello, Bob!", "Hello, Charlie!"]

// Rest with destructuring in parameters
const processOrder = ({ orderId, customerName, ...orderDetails }) => {
    console.log(`Processing order ${orderId} for ${customerName}`);
    console.log("Order details:", orderDetails);
    return { orderId, customerName, status: "processed" };
};

// Example 4: Advanced Spread Patterns
// Function composition with spread
const multiply = (a, b) => a * b;
const coordinates = [4, 5];
const result = multiply(...coordinates); // equivalent to multiply(4, 5)

// Cloning with modifications
const originalProduct = {
    id: 1,
    name: "Laptop",
    price: 999,
    specs: {
        ram: "8GB",
        storage: "256GB"
    }
};

// Shallow clone with price update
const discountedProduct = {
    ...originalProduct,
    price: originalProduct.price * 0.9,
    onSale: true
};

// Deep clone would require more work:
const deepClonedProduct = {
    ...originalProduct,
    specs: {
        ...originalProduct.specs,
        ram: "16GB" // Updating nested property
    }
};

// Array methods with spread
const array1 = [1, 2, 3];
const array2 = [4, 5, 6];

// Push multiple elements
array1.push(...array2);
console.log(array1); // [1, 2, 3, 4, 5, 6]

// Unshift multiple elements
const array3 = [7, 8, 9];
array3.unshift(...[0, 1, 2]);
console.log(array3); // [0, 1, 2, 7, 8, 9]

// Example 5: Practical Use Cases in API Development
// Merging request data with defaults
const createUserEndpoint = (requestData) => {
    const defaults = {
        role: 'user',
        isActive: true,
        createdAt: new Date(),
        permissions: []
    };
    
    const userData = { ...defaults, ...requestData };
    return userData;
};

// Extracting specific fields from request
const updateUserEndpoint = ({ id, ...updateFields }) => {
    // id is extracted, updateFields contains everything else
    return database.updateUser(id, updateFields);
};

// Building dynamic queries
const buildSearchQuery = (baseQuery, ...filters) => {
    return filters.reduce((query, filter) => ({
        ...query,
        ...filter
    }), baseQuery);
};

const searchQuery = buildSearchQuery(
    { status: 'active' },
    { category: 'electronics' },
    { priceRange: { min: 100, max: 1000 } }
);
```

### 1.6 Promises and Async/Await: Complete Guide

#### Syntax Overview:
```javascript
// Promise creation
const promise = new Promise((resolve, reject) => {
    // Async operation
    if (success) resolve(value);
    else reject(error);
});

// Promise consumption
promise.then(value => { }).catch(error => { });

// Async/await syntax
async function functionName() {
    try {
        const result = await promise;
        return result;
    } catch (error) {
        // handle error
    }
}
```

#### Key Concepts:
1. **Promise states**: Pending, Fulfilled, Rejected
2. **Promise chaining**: `.then()` returns a new promise
3. **Error propagation**: Errors bubble up through the chain
4. **Async/await**: Syntactic sugar over promises
5. **Concurrency**: `Promise.all()`, `Promise.race()`, etc.

#### Detailed Examples:

```javascript
// Example 1: Understanding Promise Basics
// Creating a basic promise
const createPromise = (shouldSucceed) => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (shouldSucceed) {
                resolve("Success! Operation completed.");
            } else {
                reject(new Error("Operation failed!"));
            }
        }, 1000);
    });
};

// Using the promise with .then() and .catch()
createPromise(true)
    .then(result => {
        console.log("Success:", result);
        return "Next step data"; // This becomes the resolved value for the next .then()
    })
    .then(nextData => {
        console.log("Next step:", nextData);
    })
    .catch(error => {
        console.error("Error caught:", error.message);
    })
    .finally(() => {
        console.log("This runs regardless of success or failure");
    });

// Promise chaining with transformations
const fetchUserData = (userId) => {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve({
                id: userId,
                name: `User ${userId}`,
                email: `user${userId}@example.com`
            });
        }, 500);
    });
};

const fetchUserPosts = (userId) => {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve([
                { id: 1, title: "My First Post", userId },
                { id: 2, title: "Learning JavaScript", userId }
            ]);
        }, 300);
    });
};

// Chaining promises to fetch related data
fetchUserData(1)
    .then(user => {
        console.log("User fetched:", user.name);
        return fetchUserPosts(user.id); // Return another promise
    })
    .then(posts => {
        console.log("Posts fetched:", posts.length);
        return posts.map(post => post.title); // Transform the data
    })
    .then(titles => {
        console.log("Post titles:", titles);
    })
    .catch(error => {
        console.error("Error in chain:", error);
    });

// Example 2: Async/Await - Cleaner Syntax
// The same operation using async/await
const fetchUserDataComplete = async (userId) => {
    try {
        console.log(`Fetching data for user ${userId}...`);
        
        const user = await fetchUserData(userId);
        console.log("User fetched:", user.name);
        
        const posts = await fetchUserPosts(user.id);
        console.log("Posts fetched:", posts.length);
        
        const titles = posts.map(post => post.title);
        console.log("Post titles:", titles);
        
        return { user, posts, titles };
    } catch (error) {
        console.error("Error fetching user data:", error);
        throw error; // Re-throw if you want calling code to handle it
    }
};

// Using the async function
fetchUserDataComplete(1)
    .then(result => console.log("Complete result:", result))
    .catch(error => console.error("Final error handler:", error));

// Example 3: Parallel Execution with Promise.all()
const fetchMultipleUsers = async (userIds) => {
    try {
        // All requests start simultaneously
        const userPromises = userIds.map(id => fetchUserData(id));
        
        // Wait for all to complete
        const users = await Promise.all(userPromises);
        
        console.log(`Fetched ${users.length} users simultaneously`);
        return users;
    } catch (error) {
        console.error("Error fetching users:", error);
        // If ANY promise rejects, Promise.all() rejects
    }
};

// Promise.allSettled() - Wait for all, regardless of success/failure
const fetchUsersAllSettled = async (userIds) => {
    const userPromises = userIds.map(async (id) => {
        try {
            return await fetchUserData(id);
        } catch (error) {
            return { error: error.message, id };
        }
    });
    
    const results = await Promise.allSettled(userPromises);
    
    const successful = results
        .filter(result => result.status === 'fulfilled')
        .map(result => result.value);
    
    const failed = results
        .filter(result => result.status === 'rejected')
        .map(result => result.reason);
    
    return { successful, failed };
};

// Example 4: Real-world API Simulation
const apiCall = (endpoint, data = null, delay = 1000) => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            // Simulate different responses
            if (endpoint === '/users' && !data) {
                resolve([
                    { id: 1, name: "Alice", email: "alice@example.com" },
                    { id: 2, name: "Bob", email: "bob@example.com" }
                ]);
            } else if (endpoint.startsWith('/users/')) {
                const userId = parseInt(endpoint.split('/')[2]);
                if (userId > 0 && userId <= 10) {
                    resolve({ id: userId, name: `User ${userId}`, email: `user${userId}@example.com` });
                } else {
                    reject(new Error("User not found"));
                }
            } else if (endpoint === '/users' && data) {
                resolve({ id: Date.now(), ...data, createdAt: new Date() });
            } else {
                reject(new Error("Invalid endpoint"));
            }
        }, delay);
    });
};

// Building a complete user service
class UserService {
    static async getAllUsers() {
        try {
            const users = await apiCall('/users');
            return users;
        } catch (error) {
            console.error("Failed to fetch users:", error);
            throw error;
        }
    }
    
    static async getUserById(id) {
        try {
            const user = await apiCall(`/users/${id}`);
            return user;
        } catch (error) {
            console.error(`Failed to fetch user ${id}:`, error);
            throw error;
        }
    }
    
    static async createUser(userData) {
        try {
            const newUser = await apiCall('/users', userData);
            console.log("User created successfully:", newUser);
            return newUser;
        } catch (error) {
            console.error("Failed to create user:", error);
            throw error;
        }
    }
    
    // Fetch user with retry logic
    static async getUserWithRetry(id, maxRetries = 3) {
        let lastError;
        
        for (let attempt = 1; attempt <= maxRetries; attempt++) {
            try {
                console.log(`Attempt ${attempt} to fetch user ${id}`);
                const user = await apiCall(`/users/${id}`);
                return user;
            } catch (error) {
                lastError = error;
                if (attempt < maxRetries) {
                    const delay = Math.pow(2, attempt) * 1000; // Exponential backoff
                    console.log(`Retrying in ${delay}ms...`);
                    await new Promise(resolve => setTimeout(resolve, delay));
                }
            }
        }
        
        throw new Error(`Failed to fetch user after ${maxRetries} attempts: ${lastError.message}`);
    }
    
    // Batch operations with error handling
    static async batchCreateUsers(userDataArray) {
        const results = [];
        
        for (const userData of userDataArray) {
            try {
                const user = await this.createUser(userData);
                results.push({ success: true, user });
            } catch (error) {
                results.push({ success: false, error: error.message, userData });
            }
        }
        
        const successful = results.filter(r => r.success).length;
        const failed = results.filter(r => !r.success).length;
        
        console.log(`Batch complete: ${successful} successful, ${failed} failed`);
        return results;
    }
}

// Example 5: Advanced Async Patterns
// Implementing timeout functionality
const withTimeout = (promise, timeoutMs) => {
    const timeout = new Promise((_, reject) => {
        setTimeout(() => reject(new Error('Operation timed out')), timeoutMs);
    });
    
    return Promise.race([promise, timeout]);
};

// Usage
const fetchWithTimeout = async () => {
    try {
        const user = await withTimeout(apiCall('/users/1'), 2000); // 2 second timeout
        console.log("User fetched within timeout:", user);
    } catch (error) {
        console.error("Error or timeout:", error.message);
    }
};

// Implementing queue/rate limiting
class RequestQueue {
    constructor(concurrency = 3) {
        this.concurrency = concurrency;
        this.running = 0;
        this.queue = [];
    }
    
    async add(promiseFunction) {
        return new Promise((resolve, reject) => {
            this.queue.push({ promiseFunction, resolve, reject });
            this.process();
        });
    }
    
    async process() {
        if (this.running >= this.concurrency || this.queue.length === 0) {
            return;
        }
        
        this.running++;
        const { promiseFunction, resolve, reject } = this.queue.shift();
        
        try {
            const result = await promiseFunction();
            resolve(result);
        } catch (error) {
            reject(error);
        } finally {
            this.running--;
            this.process(); // Process next item
        }
    }
}

// Usage
const queue = new RequestQueue(2); // Max 2 concurrent requests

const fetchUsersWithQueue = async () => {
    const userIds = [1, 2, 3, 4, 5];
    
    const promises = userIds.map(id => 
        queue.add(() => apiCall(`/users/${id}`))
    );
    
    try {
        const users = await Promise.all(promises);
        console.log("All users fetched with queue:", users.length);
        return users;
    } catch (error) {
        console.error("Error fetching users with queue:", error);
    }
};

---

## 2. Node.js Environment Setup: Professional Development Setup

### 2.1 Installing Node.js: Multiple Methods and Version Management

#### Method 1: Official Installer (Beginners)
```bash
# Visit https://nodejs.org/
# Download LTS (Long Term Support) version
# Follow installation wizard

# Verify installation
node --version    # Should show v18.x.x or higher
npm --version     # Should show 8.x.x or higher
```

#### Method 2: Node Version Manager (Recommended for Developers)
```bash
# Install nvm (Node Version Manager) on macOS/Linux
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

# Restart your terminal or source the profile
source ~/.bashrc  # or ~/.zshrc

# Install latest LTS Node.js
nvm install --lts
nvm use --lts

# Install specific version
nvm install 18.17.0
nvm use 18.17.0

# List installed versions
nvm list

# Set default version
nvm alias default 18.17.0

# Install latest npm
npm install -g npm@latest
```

#### Method 3: Package Managers
```bash
# Ubuntu/Debian
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt-get install -y nodejs

# macOS with Homebrew
brew install node

# Verify installation
node --version && npm --version
```

### 2.2 Understanding npm: Package Management Deep Dive

#### npm Basics and Configuration
```bash
# Check npm configuration
npm config list

# Set npm registry (if needed)
npm config set registry https://registry.npmjs.org/

# Set author information
npm config set init.author.name "Your Name"
npm config set init.author.email "your.email@example.com"
npm config set init.license "MIT"

# View global packages
npm list -g --depth=0

# Clean npm cache (if experiencing issues)
npm cache clean --force
```

#### Creating a Professional Project Structure
```bash
# Create project directory
mkdir fullstack-api-project
cd fullstack-api-project

# Initialize with custom configuration
npm init
# Answer the prompts or use npm init -y for defaults

# Alternative: Create package.json manually for full control
```

#### Professional package.json Structure
```json
{
  "name": "fullstack-api-project",
  "version": "1.0.0",
  "description": "A comprehensive fullstack API built with Node.js and modern JavaScript",
  "main": "src/server.js",
  "scripts": {
    "start": "node src/server.js",
    "dev": "nodemon src/server.js",
    "test": "jest --watchAll --no-cache",
    "test:coverage": "jest --coverage --no-cache",
    "test:ci": "jest --ci --coverage --watchAll=false",
    "lint": "eslint src/ --ext .js",
    "lint:fix": "eslint src/ --ext .js --fix",
    "format": "prettier --write 'src/**/*.js'",
    "format:check": "prettier --check 'src/**/*.js'",
    "build": "node scripts/build.js",
    "clean": "rimraf dist/ coverage/",
    "docs": "jsdoc src/ -d docs/",
    "prepublishOnly": "npm run test && npm run build"
  },
  "keywords": [
    "nodejs",
    "express",
    "api",
    "rest",
    "backend",
    "javascript",
    "fullstack"
  ],
  "author": {
    "name": "Your Name",
    "email": "your.email@example.com",
    "url": "https://yourwebsite.com"
  },
  "license": "MIT",
  "repository": {
    "type": "git",
    "url": "https://github.com/yourusername/fullstack-api-project.git"
  },
  "bugs": {
    "url": "https://github.com/yourusername/fullstack-api-project/issues"
  },
  "homepage": "https://github.com/yourusername/fullstack-api-project#readme",
  "engines": {
    "node": ">=18.0.0",
    "npm": ">=8.0.0"
  },
  "dependencies": {
    "express": "^4.18.2",
    "dotenv": "^16.3.1",
    "cors": "^2.8.5",
    "helmet": "^7.0.0",
    "morgan": "^1.10.0",
    "compression": "^1.7.4"
  },
  "devDependencies": {
    "nodemon": "^3.0.1",
    "jest": "^29.6.2",
    "supertest": "^6.3.3",
    "eslint": "^8.46.0",
    "prettier": "^3.0.1",
    "husky": "^8.0.3",
    "lint-staged": "^13.2.3",
    "rimraf": "^5.0.1",
    "jsdoc": "^4.0.2"
  },
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged",
      "pre-push": "npm test"
    }
  },
  "lint-staged": {
    "src/**/*.js": [
      "eslint --fix",
      "prettier --write",
      "git add"
    ]
  }
}
```

#### Understanding Dependencies vs DevDependencies
```bash
# Production dependencies (--save is default)
npm install express dotenv cors helmet

# Development dependencies
npm install --save-dev nodemon jest eslint prettier

# Global packages (use sparingly)
npm install -g @angular/cli  # Example, but prefer local installs

# Install from specific sources
npm install express@4.18.2           # Specific version
npm install express@latest            # Latest version
npm install github:expressjs/express  # From GitHub
npm install ./local-package           # Local package
npm install https://github.com/user/repo/tarball/main  # From URL
```

### 2.3 Project Structure: Industry Standards

#### Creating Professional Directory Structure
```bash
# Create comprehensive project structure
mkdir -p {src/{controllers,middleware,models,routes,utils,config,services},tests/{unit,integration,e2e},docs,scripts,public/{css,js,images},logs}

# Create essential files
touch {.env,.env.example,.gitignore,.eslintrc.js,.prettierrc,README.md,CHANGELOG.md,LICENSE}

# Create source files
touch src/{server.js,app.js}
touch src/config/{database.js,environment.js}
touch src/middleware/{auth.js,errorHandler.js,validation.js}
touch src/routes/{index.js,users.js,auth.js}
touch src/controllers/{userController.js,authController.js}
touch src/models/{User.js,Post.js}
touch src/services/{userService.js,emailService.js}
touch src/utils/{logger.js,helpers.js}

# Create test files
touch tests/setup.js
touch tests/unit/userController.test.js
touch tests/integration/users.test.js

# Create documentation
touch docs/{API.md,DEPLOYMENT.md}

# Create scripts
touch scripts/{build.js,deploy.js,seed.js}
```

#### Final Project Structure
```
fullstack-api-project/
├── src/                          # Source code
│   ├── app.js                    # Express app configuration
│   ├── server.js                 # Server entry point
│   ├── config/                   # Configuration files
│   │   ├── database.js           # Database configuration
│   │   └── environment.js        # Environment variables
│   ├── controllers/              # Route handlers/business logic
│   │   ├── authController.js     # Authentication logic
│   │   └── userController.js     # User-related logic
│   ├── middleware/               # Custom middleware
│   │   ├── auth.js              # Authentication middleware
│   │   ├── errorHandler.js      # Error handling middleware
│   │   └── validation.js        # Input validation
│   ├── models/                   # Data models
│   │   ├── User.js              # User model
│   │   └── Post.js              # Post model
│   ├── routes/                   # Route definitions
│   │   ├── index.js             # Main router
│   │   ├── auth.js              # Authentication routes
│   │   └── users.js             # User routes
│   ├── services/                 # Business services
│   │   ├── emailService.js      # Email functionality
│   │   └── userService.js       # User business logic
│   └── utils/                    # Utility functions
│       ├── helpers.js           # Helper functions
│       └── logger.js            # Logging utility
├── tests/                        # Test files
│   ├── setup.js                 # Test configuration
│   ├── unit/                    # Unit tests
│   │   └── userController.test.js
│   ├── integration/             # Integration tests
│   │   └── users.test.js
│   └── e2e/                     # End-to-end tests
├── docs/                         # Documentation
│   ├── API.md                   # API documentation
│   └── DEPLOYMENT.md            # Deployment guide
├── scripts/                      # Build and deployment scripts
│   ├── build.js                 # Build script
│   ├── deploy.js                # Deployment script
│   └── seed.js                  # Database seeding
├── public/                       # Static files
│   ├── css/                     # Stylesheets
│   ├── js/                      # Client-side JavaScript
│   └── images/                  # Images
├── logs/                         # Log files
├── .env                          # Environment variables (not in git)
├── .env.example                  # Environment template
├── .gitignore                    # Git ignore rules
├── .eslintrc.js                  # ESLint configuration
├── .prettierrc                   # Prettier configuration
├── package.json                  # Project configuration
├── package-lock.json             # Locked dependencies
├── README.md                     # Project documentation
├── CHANGELOG.md                  # Change log
└── LICENSE                       # License file
```

#### Essential Configuration Files

##### .gitignore
```bash
# Dependencies
node_modules/
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Environment variables
.env
.env.local
.env.development
.env.test
.env.production

# Logs
logs/
*.log

# Runtime data
pids/
*.pid
*.seed

# Coverage directory used by tools like istanbul
coverage/

# Build directories
dist/
build/

# IDE files
.vscode/
.idea/
*.swp
*.swo

# OS files
.DS_Store
Thumbs.db

# Temporary files
tmp/
temp/
```

##### .eslintrc.js
```javascript
module.exports = {
  env: {
    node: true,
    es2021: true,
    jest: true,
  },
  extends: [
    'eslint:recommended',
  ],
  parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module',
  },
  rules: {
    'no-console': 'warn',
    'no-unused-vars': ['error', { argsIgnorePattern: '^_' }],
    'no-var': 'error',
    'prefer-const': 'error',
    'eqeqeq': 'error',
    'no-trailing-spaces': 'error',
    'comma-dangle': ['error', 'always-multiline'],
    'semi': ['error', 'always'],
    'quotes': ['error', 'single'],
    'indent': ['error', 2],
  },
};
```

##### .prettierrc
```json
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2,
  "useTabs": false,
  "bracketSpacing": true,
  "arrowParens": "avoid"
}
```

---

## 3. Building Your First Complete API with Raw Node.js

Let's build a comprehensive API without any frameworks to understand the fundamentals:

### 3.1 Core Server Implementation

```javascript
// src/server.js - Main server file
const http = require('http');
const url = require('url');
const querystring = require('querystring');
const { readFileSync } = require('fs');
const path = require('path');

// Load environment variables
require('dotenv').config();

// In-memory database (in real applications, use MongoDB, PostgreSQL, etc.)
let users = [
  {
    id: 1,
    firstName: "John",
    lastName: "Doe",
    email: "john.doe@example.com",
    age: 28,
    role: "developer",
    isActive: true,
    createdAt: new Date('2023-01-15').toISOString(),
    updatedAt: new Date('2023-01-15').toISOString(),
  },
  {
    id: 2,
    firstName: "Jane",
    lastName: "Smith",
    email: "jane.smith@example.com",
    age: 32,
    role: "designer",
    isActive: true,
    createdAt: new Date('2023-02-20').toISOString(),
    updatedAt: new Date('2023-02-20').toISOString(),
  },
  {
    id: 3,
    firstName: "Bob",
    lastName: "Johnson",
    email: "bob.johnson@example.com",
    age: 25,
    role: "manager",
    isActive: false,
    createdAt: new Date('2023-01-10').toISOString(),
    updatedAt: new Date('2023-03-15').toISOString(),
  }
];

// Utility functions
const generateId = () => Math.max(...users.map(u => u.id), 0) + 1;

const getCurrentTimestamp = () => new Date().toISOString();

// Response helper functions
const sendResponse = (res, statusCode, data, headers = {}) => {
  const defaultHeaders = {
    'Content-Type': 'application/json; charset=utf-8',
    'Access-Control-Allow-Origin': '*',
    'Access-Control-Allow-Methods': 'GET, POST, PUT, DELETE, OPTIONS',
    'Access-Control-Allow-Headers': 'Content-Type, Authorization, X-Requested-With',
    'X-Powered-By': 'Node.js Custom API Server',
    'X-Response-Time': Date.now(),
  };

  res.writeHead(statusCode, { ...defaultHeaders, ...headers });
  res.end(JSON.stringify(data, null, 2));
};

const sendError = (res, statusCode, message, details = null) => {
  const errorResponse = {
    success: false,
    error: {
      message,
      statusCode,
      timestamp: getCurrentTimestamp(),
      ...(details && { details })
    }
  };
  sendResponse(res, statusCode, errorResponse);
};

const sendSuccess = (res, statusCode, data, message = null, meta = null) => {
  const successResponse = {
    success: true,
    data,
    timestamp: getCurrentTimestamp(),
    ...(message && { message }),
    ...(meta && { meta })
  };
  sendResponse(res, statusCode, successResponse);
};

// Validation functions
const validateEmail = (email) => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return emailRegex.test(email);
};

const validateUser = (userData, isUpdate = false) => {
  const errors = [];

  // Name validation
  if (!isUpdate || userData.firstName !== undefined) {
    if (!userData.firstName || userData.firstName.trim().length < 2) {
      errors.push("First name must be at least 2 characters long");
    }
  }

  if (!isUpdate || userData.lastName !== undefined) {
    if (!userData.lastName || userData.lastName.trim().length < 2) {
      errors.push("Last name must be at least 2 characters long");
    }
  }

  // Email validation
  if (!isUpdate || userData.email !== undefined) {
    if (!userData.email || !validateEmail(userData.email)) {
      errors.push("Valid email address is required");
    } else {
      // Check for duplicate email (exclude current user in updates)
      const existingUser = users.find(u => 
        u.email.toLowerCase() === userData.email.toLowerCase() &&
        (!userData.id || u.id !== userData.id)
      );
      if (existingUser) {
        errors.push("Email address is already in use");
      }
    }
  }

  // Age validation
  if (!isUpdate || userData.age !== undefined) {
    if (userData.age && (isNaN(userData.age) || userData.age < 1 || userData.age > 150)) {
      errors.push("Age must be a number between 1 and 150");
    }
  }

  // Role validation
  const validRoles = ['developer', 'designer', 'manager', 'admin', 'user'];
  if (!isUpdate || userData.role !== undefined) {
    if (userData.role && !validRoles.includes(userData.role.toLowerCase())) {
      errors.push(`Role must be one of: ${validRoles.join(', ')}`);
    }
  }

  return errors;
};

// Request body parser
const parseRequestBody = (req) => {
  return new Promise((resolve, reject) => {
    let body = '';
    req.on('data', chunk => body += chunk.toString());
    req.on('end', () => {
      try {
        resolve(body ? JSON.parse(body) : {});
      } catch (error) {
        reject(new Error('Invalid JSON in request body'));
      }
    });
    req.on('error', reject);
  });
};

// Route handlers
const handleGetUsers = (req, res, query) => {
  try {
    let filteredUsers = [...users];

    // Filtering
    if (query.active !== undefined) {
      const isActive = query.active.toLowerCase() === 'true';
      filteredUsers = filteredUsers.filter(user => user.isActive === isActive);
    }

    if (query.role) {
      filteredUsers = filteredUsers.filter(user =>
        user.role.toLowerCase() === query.role.toLowerCase()
      );
    }

    if (query.search) {
      const searchTerm = query.search.toLowerCase();
      filteredUsers = filteredUsers.filter(user =>
        user.firstName.toLowerCase().includes(searchTerm) ||
        user.lastName.toLowerCase().includes(searchTerm) ||
        user.email.toLowerCase().includes(searchTerm)
      );
    }

    // Sorting
    const sortBy = query.sortBy || 'id';
    const sortOrder = query.sortOrder === 'desc' ? -1 : 1;
    
    if (users[0] && users[0][sortBy] !== undefined) {
      filteredUsers.sort((a, b) => {
        if (a[sortBy] < b[sortBy]) return -1 * sortOrder;
        if (a[sortBy] > b[sortBy]) return 1 * sortOrder;
        return 0;
      });
    }

    // Pagination
    const page = Math.max(1, parseInt(query.page) || 1);
    const limit = Math.min(100, Math.max(1, parseInt(query.limit) || 10));
    const startIndex = (page - 1) * limit;
    const endIndex = startIndex + limit;

    const paginatedUsers = filteredUsers.slice(startIndex, endIndex);

    // Response metadata
    const totalUsers = filteredUsers.length;
    const totalPages = Math.ceil(totalUsers / limit);

    const meta = {
      pagination: {
        currentPage: page,
        totalPages,
        totalUsers,
        usersPerPage: limit,
        hasNextPage: page < totalPages,
        hasPrevPage: page > 1,
        nextPage: page < totalPages ? page + 1 : null,
        prevPage: page > 1 ? page - 1 : null,
      },
      filters: {
        active: query.active,
        role: query.role,
        search: query.search,
      },
      sorting: {
        sortBy,
        sortOrder: query.sortOrder || 'asc',
      },
    };

    sendSuccess(res, 200, paginatedUsers, 'Users retrieved successfully', meta);
  } catch (error) {
    sendError(res, 500, 'Internal server error while fetching users', error.message);
  }
};

const handleGetUser = (req, res, userId) => {
  try {
    const user = users.find(u => u.id === parseInt(userId));

    if (!user) {
      return sendError(res, 404, `User with ID ${userId} not found`);
    }

    sendSuccess(res, 200, user, 'User retrieved successfully');
  } catch (error) {
    sendError(res, 500, 'Internal server error while fetching user', error.message);
  }
};

const handleCreateUser = async (req, res) => {
  try {
    const userData = await parseRequestBody(req);

    // Validate input
    const validationErrors = validateUser(userData);
    if (validationErrors.length > 0) {
      return sendError(res, 400, 'Validation failed', validationErrors);
    }

    // Create new user
    const newUser = {
      id: generateId(),
      firstName: userData.firstName.trim(),
      lastName: userData.lastName.trim(),
      email: userData.email.toLowerCase().trim(),
      age: userData.age || null,
      role: userData.role ? userData.role.toLowerCase() : 'user',
      isActive: userData.isActive !== undefined ? userData.isActive : true,
      createdAt: getCurrentTimestamp(),
      updatedAt: getCurrentTimestamp(),
    };

    users.push(newUser);

    sendSuccess(res, 201, newUser, 'User created successfully');
  } catch (error) {
    if (error.message === 'Invalid JSON in request body') {
      return sendError(res, 400, 'Invalid JSON in request body');
    }
    sendError(res, 500, 'Internal server error while creating user', error.message);
  }
};

const handleUpdateUser = async (req, res, userId) => {
  try {
    const userIndex = users.findIndex(u => u.id === parseInt(userId));

    if (userIndex === -1) {
      return sendError(res, 404, `User with ID ${userId} not found`);
    }

    const updateData = await parseRequestBody(req);

    // Validate input for updates
    const validationErrors = validateUser({ ...updateData, id: parseInt(userId) }, true);
    if (validationErrors.length > 0) {
      return sendError(res, 400, 'Validation failed', validationErrors);
    }

    // Update user (only provided fields)
    const currentUser = users[userIndex];
    const updatedUser = {
      ...currentUser,
      ...(updateData.firstName && { firstName: updateData.firstName.trim() }),
      ...(updateData.lastName && { lastName: updateData.lastName.trim() }),
      ...(updateData.email && { email: updateData.email.toLowerCase().trim() }),
      ...(updateData.age !== undefined && { age: updateData.age }),
      ...(updateData.role && { role: updateData.role.toLowerCase() }),
      ...(updateData.isActive !== undefined && { isActive: updateData.isActive }),
      updatedAt: getCurrentTimestamp(),
    };

    users[userIndex] = updatedUser;

    sendSuccess(res, 200, updatedUser, 'User updated successfully');
  } catch (error) {
    if (error.message === 'Invalid JSON in request body') {
      return sendError(res, 400, 'Invalid JSON in request body');
    }
    sendError(res, 500, 'Internal server error while updating user', error.message);
  }
};

const handleDeleteUser = (req, res, userId) => {
  try {
    const userIndex = users.findIndex(u => u.id === parseInt(userId));

    if (userIndex === -1) {
      return sendError(res, 404, `User with ID ${userId} not found`);
    }

    const deletedUser = users.splice(userIndex, 1)[0];

    sendSuccess(res, 200, deletedUser, 'User deleted successfully');
  } catch (error) {
    sendError(res, 500, 'Internal server error while deleting user', error.message);
  }
};

// Statistics endpoint
const handleGetStats = (req, res) => {
  try {
    const stats = {
      totalUsers: users.length,
      activeUsers: users.filter(u => u.isActive).length,
      inactiveUsers: users.filter(u => !u.isActive).length,
      usersByRole: users.reduce((acc, user) => {
        acc[user.role] = (acc[user.role] || 0) + 1;
        return acc;
      }, {}),
      averageAge: users.length > 0 
        ? Math.round(users.filter(u => u.age).reduce((sum, u) => sum + u.age, 0) / users.filter(u => u.age).length)
        : 0,
      recentUsers: users
        .sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt))
        .slice(0, 5)
        .map(u => ({ id: u.id, firstName: u.firstName, lastName: u.lastName, createdAt: u.createdAt }))
    };

    sendSuccess(res, 200, stats, 'Statistics retrieved successfully');
  } catch (error) {
    sendError(res, 500, 'Internal server error while fetching statistics', error.message);
  }
};

// Health check endpoint
const handleHealthCheck = (req, res) => {
  const healthStatus = {
    status: 'healthy',
    timestamp: getCurrentTimestamp(),
    uptime: process.uptime(),
    version: '1.0.0',
    environment: process.env.NODE_ENV || 'development',
    memory: process.memoryUsage(),
    system: {
      nodeVersion: process.version,
      platform: process.platform,
      arch: process.arch,
    }
  };

  sendSuccess(res, 200, healthStatus, 'Server is healthy');
};

// Main request handler
const requestHandler = async (req, res) => {
  const startTime = Date.now();
  
  // Add request ID for tracking
  const requestId = Math.random().toString(36).substr(2, 9);
  req.requestId = requestId;

  // Parse URL and method
  const parsedUrl = url.parse(req.url, true);
  const path = parsedUrl.pathname;
  const method = req.method.toUpperCase();
  const query = parsedUrl.query;

  // Log request (in production, use proper logging library)
  console.log(`[${getCurrentTimestamp()}] ${requestId} ${method} ${path}`);

  // Add response headers for all requests
  res.setHeader('X-Request-ID', requestId);

  try {
    // Handle CORS preflight requests
    if (method === 'OPTIONS') {
      res.writeHead(200, {
        'Access-Control-Allow-Origin': '*',
        'Access-Control-Allow-Methods': 'GET, POST, PUT, DELETE, OPTIONS',
        'Access-Control-Allow-Headers': 'Content-Type, Authorization, X-Requested-With',
        'Access-Control-Max-Age': '86400', // 24 hours
      });
      res.end();
      return;
    }

    // Route matching
    if (path === '/health' && method === 'GET') {
      handleHealthCheck(req, res);
    } else if (path === '/stats' && method === 'GET') {
      handleGetStats(req, res);
    } else if (path === '/users' && method === 'GET') {
      handleGetUsers(req, res, query);
    } else if (path === '/users' && method === 'POST') {
      await handleCreateUser(req, res);
    } else if (path.match(/^\/users\/\d+$/) && method === 'GET') {
      const userId = path.split('/')[2];
      handleGetUser(req, res, userId);
    } else if (path.match(/^\/users\/\d+$/) && method === 'PUT') {
      const userId = path.split('/')[2];
      await handleUpdateUser(req, res, userId);
    } else if (path.match(/^\/users\/\d+$/) && method === 'DELETE') {
      const userId = path.split('/')[2];
      handleDeleteUser(req, res, userId);
    } else if (path === '/' && method === 'GET') {
      // API documentation endpoint
      const apiInfo = {
        name: 'Node.js Raw API',
        version: '1.0.0',
        description: 'A comprehensive REST API built with raw Node.js',
        endpoints: {
          'GET /health': 'Server health check',
          'GET /stats': 'User statistics',
          'GET /users': 'Get all users (with filtering, pagination, sorting)',
          'POST /users': 'Create a new user',
          'GET /users/:id': 'Get user by ID',
          'PUT /users/:id': 'Update user by ID',
          'DELETE /users/:id': 'Delete user by ID',
        },
        queryParameters: {
          'page': 'Page number for pagination (default: 1)',
          'limit': 'Items per page (default: 10, max: 100)',
          'active': 'Filter by active status (true/false)',
          'role': 'Filter by user role',
          'search': 'Search in name and email',
          'sortBy': 'Sort by field (default: id)',
          'sortOrder': 'Sort order: asc/desc (default: asc)',
        }
      };
      sendSuccess(res, 200, apiInfo, 'API information');
    } else {
      sendError(res, 404, `Endpoint ${method} ${path} not found`, {
        availableEndpoints: [
          'GET /',
          'GET /health',
          'GET /stats',
          'GET /users',
          'POST /users',
          'GET /users/:id',
          'PUT /users/:id',
          'DELETE /users/:id'
        ]
      });
    }
  } catch (error) {
    console.error(`[${getCurrentTimestamp()}] ${requestId} Error:`, error);
    sendError(res, 500, 'Internal server error', error.message);
  } finally {
    // Log response time
    const responseTime = Date.now() - startTime;
    console.log(`[${getCurrentTimestamp()}] ${requestId} Completed in ${responseTime}ms`);
  }
};

// Create and start server
const PORT = process.env.PORT || 3000;
const HOST = process.env.HOST || 'localhost';

const server = http.createServer(requestHandler);

server.listen(PORT, HOST, () => {
  console.log(`
🚀 Server is running!
📍 URL: http://${HOST}:${PORT}
🌍 Environment: ${process.env.NODE_ENV || 'development'}
⏰ Started at: ${getCurrentTimestamp()}

Available endpoints:
• GET  /           - API information
• GET  /health     - Health check
• GET  /stats      - User statistics  
• GET  /users      - Get all users
• POST /users      - Create user
• GET  /users/:id  - Get user by ID
• PUT  /users/:id  - Update user
• DELETE /users/:id - Delete user

Try it out:
curl http://${HOST}:${PORT}/users
curl http://${HOST}:${PORT}/health
  `);
});

// Graceful shutdown
process.on('SIGTERM', () => {
  console.log('SIGTERM received, shutting down gracefully...');
  server.close(() => {
    console.log('Server closed');
    process.exit(0);
  });
});

process.on('SIGINT', () => {
  console.log('SIGINT received, shutting down gracefully...');
  server.close(() => {
    console.log('Server closed');
    process.exit(0);
  });
});

module.exports = server; // Export for testing
```

### 3.2 Environment Configuration

```javascript
// src/config/environment.js
require('dotenv').config();

const config = {
  // Server configuration
  server: {
    port: process.env.PORT || 3000,
    host: process.env.HOST || 'localhost',
    environment: process.env.NODE_ENV || 'development',
  },
  
  // Database configuration (for future use)
  database: {
    url: process.env.DATABASE_URL || 'mongodb://localhost:27017/myapp',
    host: process.env.DB_HOST || 'localhost',
    port: process.env.DB_PORT || 27017,
    name: process.env.DB_NAME || 'myapp',
    username: process.env.DB_USERNAME || '',
    password: process.env.DB_PASSWORD || '',
  },
  
  // Security configuration
  security: {
    jwtSecret: process.env.JWT_SECRET || 'your-super-secret-key-change-this-in-production',
    jwtExpiresIn: process.env.JWT_EXPIRES_IN || '24h',
    bcryptRounds: parseInt(process.env.BCRYPT_ROUNDS) || 12,
  },
  
  // API configuration
  api: {
    version: '1.0.0',
    prefix: process.env.API_PREFIX || '/api/v1',
    rateLimit: {
      windowMs: parseInt(process.env.RATE_LIMIT_WINDOW_MS) || 15 * 60 * 1000, // 15 minutes
      maxRequests: parseInt(process.env.RATE_LIMIT_MAX_REQUESTS) || 100,
    },
  },
  
  // Email configuration (for future use)
  email: {
    service: process.env.EMAIL_SERVICE || 'gmail',
    user: process.env.EMAIL_USER || '',
    password: process.env.EMAIL_PASSWORD || '',
    from: process.env.EMAIL_FROM || 'noreply@yourapp.com',
  },
  
  // File upload configuration
  upload: {
    maxFileSize: parseInt(process.env.MAX_FILE_SIZE) || 5 * 1024 * 1024, // 5MB
    allowedTypes: (process.env.ALLOWED_FILE_TYPES || 'image/jpeg,image/png,image/gif').split(','),
    uploadDir: process.env.UPLOAD_DIR || './uploads',
  },
  
  // Logging configuration
  logging: {
    level: process.env.LOG_LEVEL || 'info',
    file: process.env.LOG_FILE || './logs/app.log',
  },
  
  // Development helpers
  isDevelopment: process.env.NODE_ENV === 'development',
  isProduction: process.env.NODE_ENV === 'production',
  isTest: process.env.NODE_ENV === 'test',
};

// Validation function
const validateConfig = () => {
  const requiredFields = [];
  
  if (config.isProduction) {
    requiredFields.push(
      'JWT_SECRET',
      'DATABASE_URL'
    );
  }
  
  const missing = requiredFields.filter(field => !process.env[field]);
  
  if (missing.length > 0) {
    throw new Error(`Missing required environment variables: ${missing.join(', ')}`);
  }
};

// Validate configuration on load
validateConfig();

module.exports = config;
```

### 3.3 Bash Scripts for Project Management

```bash
#!/bin/bash
# scripts/project-manager.sh

# Colors for better output
RED='\033[0;31m'
GREEN='\033[0;32m'
BLUE='\033[0;34m'
YELLOW='\033[1;33m'
NC='\033[0m' # No Color

# Project information
PROJECT_NAME="fullstack-api-project"
NODE_VERSION="18.17.0"

# Function to print colored output
print_status() {
    echo -e "${BLUE}[INFO]${NC} $1"
}

print_success() {
    echo -e "${GREEN}[SUCCESS]${NC} $1"
}

print_error() {
    echo -e "${RED}[ERROR]${NC} $1"
}

print_warning() {
    echo -e "${YELLOW}[WARNING]${NC} $1"
}

# Function to check if command exists
command_exists() {
    command -v "$1" >/dev/null 2>&1
}

# Setup development environment
setup_environment() {
    print_status "Setting up development environment..."
    
    # Check Node.js version
    if command_exists node; then
        CURRENT_NODE_VERSION=$(node --version)
        print_status "Current Node.js version: $CURRENT_NODE_VERSION"
    else
        print_error "Node.js is not installed!"
        exit 1
    fi
    
    # Check npm
    if command_exists npm; then
        NPM_VERSION=$(npm --version)
        print_status "Current npm version: $NPM_VERSION"
    else
        print_error "npm is not installed!"
        exit 1
    fi
    
    # Install dependencies
    if [ -f "package.json" ]; then
        print_status "Installing dependencies..."
        npm install
        if [ $? -eq 0 ]; then
            print_success "Dependencies installed successfully!"
        else
            print_error "Failed to install dependencies!"
            exit 1
        fi
    else
        print_error "package.json not found!"
        exit 1
    fi
    
    # Create .env file if it doesn't exist
    if [ ! -f ".env" ]; then
        if [ -f ".env.example" ]; then
            cp .env.example .env
            print_success "Created .env file from .env.example"
        else
            cat > .env << EOF
# Server Configuration
PORT=3000
HOST=localhost
NODE_ENV=development

# Database Configuration
DATABASE_URL=mongodb://localhost:27017/myapp

# Security Configuration
JWT_SECRET=your-super-secret-key-change-this-in-production
JWT_EXPIRES_IN=24h

# API Configuration
API_PREFIX=/api/v1
RATE_LIMIT_WINDOW_MS=900000
RATE_LIMIT_MAX_REQUESTS=100

# Logging Configuration
LOG_LEVEL=info
LOG_FILE=./logs/app.log
EOF
            print_success "Created default .env file"
        fi
    fi
    
    # Create logs directory
    mkdir -p logs
    print_success "Environment setup completed!"
}

# Start development server
start_dev_server() {
    print_status "Starting development server..."
    
    if [ -f "package.json" ]; then
        npm run dev
    else
        print_status "Starting with nodemon..."
        if command_exists nodemon; then
            nodemon src/server.js
        else
            print_warning "nodemon not found, starting with node..."
            node src/server.js
        fi
    fi
}

# Start production server
start_prod_server() {
    print_status "Starting production server..."
    NODE_ENV=production node src/server.js
}

# Run tests
run_tests() {
    print_status "Running tests..."
    
    if [ -f "package.json" ] && grep -q "\"test\"" package.json; then
        npm test
    else
        print_warning "No test script found in package.json"
    fi
}

# Run linting
run_linting() {
    print_status "Running ESLint..."
    
    if [ -f "package.json" ] && grep -q "\"lint\"" package.json; then
        npm run lint
    elif command_exists eslint; then
        eslint src/ --ext .js
    else
        print_warning "ESLint not configured or installed"
    fi
}

# Format code
format_code() {
    print_status "Formatting code with Prettier..."
    
    if [ -f "package.json" ] && grep -q "\"format\"" package.json; then
        npm run format
    elif command_exists prettier; then
        prettier --write 'src/**/*.js'
    else
        print_warning "Prettier not configured or installed"
    fi
}

# Build for production
build_project() {
    print_status "Building project for production..."
    
    # Run linting first
    run_linting
    
    # Run tests
    run_tests
    
    # Create build directory
    mkdir -p dist
    
    # Copy source files (in a real app, you might transpile or bundle)
    cp -r src/ dist/
    cp package.json dist/
    cp package-lock.json dist/
    
    print_success "Build completed! Files are in the 'dist' directory."
}

# Clean project
clean_project() {
    print_status "Cleaning project..."
    
    rm -rf node_modules
    rm -rf dist
    rm -rf coverage
    rm -rf logs/*.log
    
    print_success "Project cleaned!"
}

# Deploy to server (basic example)
deploy_project() {
    print_status "Deploying project..."
    
    # Build first
    build_project
    
    # Example deployment (you would customize this)
    SERVER_HOST=${1:-"your-server.com"}
    SERVER_USER=${2:-"ubuntu"}
    
    print_status "Deploying to $SERVER_USER@$SERVER_HOST"
    
    # This is just an example - customize for your deployment needs
    # rsync -avz --exclude 'node_modules' ./ "$SERVER_USER@$SERVER_HOST:/var/www/$PROJECT_NAME/"
    # ssh "$SERVER_USER@$SERVER_HOST" "cd /var/www/$PROJECT_NAME && npm install --production && pm2 restart $PROJECT_NAME"
    
    print_warning "Deployment script is just an example - customize for your needs!"
}

# Show project status
show_status() {
    print_status "Project Status:"
    echo "===================="
    
    # Node.js version
    if command_exists node; then
        echo "Node.js: $(node --version)"
    else
        echo "Node.js: Not installed"
    fi
    
    # npm version
    if command_exists npm; then
        echo "npm: $(npm --version)"
    else
        echo "npm: Not installed"
    fi
    
    # Project dependencies
    if [ -f "package.json" ]; then
        echo "Dependencies: $(cat package.json | grep -c '\".*\":')"
    else
        echo "Dependencies: package.json not found"
    fi
    
    # Git status
    if command_exists git && [ -d ".git" ]; then
        echo "Git branch: $(git branch --show-current)"
        echo "Git status: $(git status --porcelain | wc -l) modified files"
    else
        echo "Git: Not initialized"
    fi
    
    # Environment
    if [ -f ".env" ]; then
        echo "Environment: .env file exists"
    else
        echo "Environment: .env file missing"
    fi
}

# Main menu
show_menu() {
    echo ""
    echo "🚀 $PROJECT_NAME Management Script"
    echo "=================================="
    echo "1. Setup Environment"
    echo "2. Start Development Server"
    echo "3. Start Production Server"
    echo "4. Run Tests"
    echo "5. Run Linting"
    echo "6. Format Code"
    echo "7. Build Project"
    echo "8. Deploy Project"
    echo "9. Clean Project"
    echo "10. Show Status"
    echo "11. Exit"
    echo ""
}

# Main script logic
main() {
    if [ $# -eq 0 ]; then
        # Interactive mode
        while true; do
            show_menu
            read -p "Choose an option (1-11): " choice
            
            case $choice in
                1) setup_environment ;;
                2) start_dev_server ;;
                3) start_prod_server ;;
                4) run_tests ;;
                5) run_linting ;;
                6) format_code ;;
                7) build_project ;;
                8) 
                    read -p "Enter server host (or press Enter for default): " host
                    read -p "Enter server user (or press Enter for default): " user
                    deploy_project "$host" "$user"
                    ;;
                9) clean_project ;;
                10) show_status ;;
                11) 
                    print_success "Goodbye!"
                    exit 0
                    ;;
                *)
                    print_error "Invalid option! Please choose 1-11."
                    ;;
            esac
            
            echo ""
            read -p "Press Enter to continue..."
        done
    else
        # Command line mode
        case $1 in
            setup) setup_environment ;;
            dev) start_dev_server ;;
            start) start_prod_server ;;
            test) run_tests ;;
            lint) run_linting ;;
            format) format_code ;;
            build) build_project ;;
            deploy) deploy_project "$2" "$3" ;;
            clean) clean_project ;;
            status) show_status ;;
            *)
                echo "Usage: $0 {setup|dev|start|test|lint|format|build|deploy|clean|status}"
                exit 1
                ;;
        esac
    fi
}

# Run main function with all arguments
main "$@"
```

---

## 4. Homework Assignment

### Requirements:
Build a **Books Library API** using raw Node.js with the following specifications:

#### Required Endpoints:
1. `GET /books` - Get all books (with filtering, pagination, sorting)
2. `POST /books` - Add a new book
3. `GET /books/:id` - Get book by ID
4. `PUT /books/:id` - Update book by ID
5. `DELETE /books/:id` - Delete book by ID
6. `GET /books/search` - Search books by title/author
7. `GET /authors` - Get all unique authors
8. `GET /stats` - Get library statistics

#### Book Data Structure:
```javascript
{
  id: 1,
  title: "The Great Gatsby",
  author: "F. Scott Fitzgerald",
  isbn: "978-0-7432-7356-5",
  publicationYear: 1925,
  genre: "Fiction",
  pages: 180,
  isAvailable: true,
  rating: 4.2,
  createdAt: "2023-01-15T10:30:00.000Z",
  updatedAt: "2023-01-15T10:30:00.000Z"
}
```

#### Advanced Features to Implement:
1. **Input Validation**: Validate all book data
2. **Error Handling**: Proper error responses
3. **Filtering**: By genre, availability, publication year range
4. **Pagination**: Support page and limit parameters
5. **Sorting**: By title, author, rating, publication year
6. **Search**: Full-text search in title and author
7. **Statistics**: Total books, available books, genres count, etc.
8. **Environment Configuration**: Use .env file
9. **Bash Scripts**: Create management script
10. **Logging**: Request logging with timestamps

### Bonus Challenges:
1. Add book borrowing functionality (checkout/checkin)
2. Implement rate limiting
3. Add request caching
4. Create API documentation endpoint
5. Add data export functionality (JSON format)

---

## 5. Testing Your Knowledge

### Quick Quiz:

#### Question 1: Variable Declarations
What will be the output of this code?
```javascript
for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log(`var: ${i}`), 100);
}

for (let j = 0; j < 3; j++) {
    setTimeout(() => console.log(`let: ${j}`), 200);
}
```

#### Question 2: Arrow Functions
Fix this code to make it work properly:
```javascript
const user = {
    name: "Alice",
    greet: () => {
        console.log(`Hello, I'm ${this.name}`);
    }
};
user.greet(); // Should print "Hello, I'm Alice"
```

#### Question 3: Destructuring
Simplify this code using destructuring:
```javascript
const response = {
    data: {
        user: {
            profile: {
                name: "John",
                email: "john@example.com"
            }
        }
    },
    status: 200
};

const name = response.data.user.profile.name;
const email = response.data.user.profile.email;
const status = response.status;
```

#### Question 4: Async/Await
Convert this Promise chain to async/await:
```javascript
fetchUser(1)
    .then(user => fetchUserPosts(user.id))
    .then(posts => posts.filter(post => post.published))
    .then(publishedPosts => console.log(publishedPosts))
    .catch(error => console.error(error));
```

### Answers:

#### Answer 1:
```
var: 3
var: 3  
var: 3
let: 0
let: 1
let: 2
```
Explanation: `var` is function-scoped, so all timeouts reference the same `i` which becomes 3 after the loop. `let` is block-scoped, creating a new `j` for each iteration.

#### Answer 2:
```javascript
const user = {
    name: "Alice",
    greet: function() {  // Use regular function, not arrow function
        console.log(`Hello, I'm ${this.name}`);
    }
};
// OR
const user = {
    name: "Alice",
    greet() {  // ES6 method syntax
        console.log(`Hello, I'm ${this.name}`);
    }
};
```

#### Answer 3:
```javascript
const {
    data: {
        user: {
            profile: { name, email }
        }
    },
    status
} = response;
```

#### Answer 4:
```javascript
const fetchUserData = async () => {
    try {
        const user = await fetchUser(1);
        const posts = await fetchUserPosts(user.id);
        const publishedPosts = posts.filter(post => post.published);
        console.log(publishedPosts);
    } catch (error) {
        console.error(error);
    }
};
```

---

## Next Lesson Preview

In **Lesson 3**, we'll cover:
- **Express.js Framework**: Building APIs faster and cleaner
- **Middleware**: Understanding the middleware pattern
- **Advanced Routing**: Route parameters, route handlers, route organization
- **Request/Response Handling**: Body parsing, validation, error handling
- **Project Structure**: Professional Express.js project organization
- **Authentication Basics**: JWT tokens and session management

## Additional Resources

### Essential Reading:
- [Node.js Official Documentation](https://nodejs.org/en/docs/)
- [MDN JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)
- [ES6 Features Guide](https://github.com/lukehoban/es6features)

### Practice Platforms:
- [JavaScript30](https://javascript30.com/) - 30 Day JavaScript Challenge
- [Node School](https://nodeschool.io/) - Interactive Node.js tutorials
- [freeCodeCamp](https://www.freecodecamp.org/) - Full stack development curriculum

### Tools to Install:
- [Postman](https://www.postman.com/) - API testing
- [VS Code](https://code.visualstudio.com/) - Code editor with great Node.js support
- [Git](https://git-scm.com/) - Version control

Remember: Practice is key! Try to build small projects and experiment with the concepts you've learned. Don't just read the code - type it out and run it yourself!
