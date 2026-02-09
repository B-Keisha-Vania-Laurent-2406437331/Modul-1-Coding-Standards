# Reflection 1

## 1. Clean Code Principles & Secure Coding
In implementing this project, I have applied several Clean Code and Secure Coding principles to ensure the software is maintainable, readable, and secure:

* **Meaningful Names (Penamaan yang Bermakna):** I used clear and descriptive names for variables, methods, and classes (e.g., `ProductRepository`, `create`, `delete`). This practice makes the code **self-documenting**, allowing other developers to understand the logic easily without needing extensive comments.

* **Single Responsibility Principle (SRP):** I adhered to SRP by strictly separating application logic into distinct layers. The **Controller** handles HTTP requests, the **Service** contains business logic, and the **Repository** manages data access. This separation ensures that each class has only one reason to change.

* **Secure Coding (UUID Generation):** Instead of using sequential integers (1, 2, 3...), I utilized **UUIDs** (Universally Unique Identifiers) for generating unique product IDs. This approach significantly enhances security by preventing **ID Enumeration Attacks**, where an attacker could otherwise guess valid IDs by simply incrementing numbers.

## 2. Areas for Improvement
Despite the implementations above, I identified a few critical areas where the code quality and security can be improved:

* **Input Validation & Sanitization:** Currently, the application accepts raw input, which poses risks to data integrity (e.g., creating products with negative quantities) and security vulnerabilities like **Cross-Site Scripting (XSS)**.  
    * *Improvement Plan:* I should implement server-side validation using annotations such as `@NotBlank` and `@Min(0)` to ensure only valid data is processed.

* **File Naming Consistency:** I found inconsistencies between the controller's return values and actual filenames (e.g., returning `"createProduct"` while the file is named `"CreateProduct.html"`).  
    * *Improvement Plan:* I must standardize the naming convention to match exactly (Case-Sensitive). This is crucial to prevent `TemplateInputException` errors when deploying to case-sensitive environments like **Linux** or **GitHub Actions**.
 
# Reflection 2

## 1. Unit Testing & Code Coverage
After writing the unit tests for this project, here are my key takeaways regarding code quality and testing metrics:

* **Confidence in Code Stability:**
    Writing unit tests has significantly increased my confidence in the application's stability. I can verify that core features (Create, Edit, Delete) function correctly. More importantly, these tests act as a safety net during **refactoring**, ensuring that future changes do not break existing functionality.

* **Number of Unit Tests:**
    There is no fixed number of unit tests required for a class. Instead of focusing on quantity, the focus should be on **Scenario Coverage**. A good test suite must cover:
    * **Positive Cases (Happy Path):** Ensures the code works as expected under normal conditions.
    * **Negative Cases (Error Handling):** Ensures the code handles invalid input or exceptions gracefully.
    * **Edge Cases (Boundary Values):** Ensures the code handles limits (e.g., empty lists, maximum values).

* **100% Code Coverage:**
    I learned that **100% Code Coverage does not guarantee bug-free code**. Coverage only measures the percentage of lines executed during tests, but it fails to detect:
    * **Missing Requirements:** Features that were never implemented cannot be caught by coverage tools.
    * **Uncaught Edge Cases:** If a developer forgets to write a test for a specific edge case, the coverage might still be 100% while the bug remains.
    Therefore, code coverage is a useful metric but must be complemented by thoughtful test design.

## 2. Functional Test Cleanliness & Code Quality
Regarding the creation of a new functional test suite to verify the number of items in the product list:

* **Code Quality Issue: Code Duplication**
    If I simply copy-paste the setup procedures (instance variables like `serverPort`, `testBaseUrl`, and `@BeforeEach` setup) from `CreateProductFunctionalTest.java` to a new test class, I would be violating the **DRY (Don't Repeat Yourself)** principle. This creates unnecessary **boilerplate code** in every test file.

* **Maintenance Nightmare:**
    Duplicating setup code drastically reduces maintainability. For example, if the testing configuration changes (e.g., changing how the `baseUrl` is constructed or switching from a random port to a fixed port), I would have to manually update **every single test file**. This increases the risk of inconsistencies and human error.

* **Readability & Focus:**
    A clean test class should focus solely on the **test logic** (verifying the product count), not on **infrastructure setup**. Cluttering the class with repetitive setup code makes it harder for other developers to read and understand what is actually being tested.

* **Proposed Solution: Inheritance (Base Test Class)**
    To improve code cleanliness, I should implement a **Base Functional Test Class** (e.g., `BaseFunctionalTest`).
    * **Encapsulation:** This class would hold the common setup logic (`@LocalServerPort`, `@Value`, `setUp()` method).
    * **Inheritance:** Specific test classes (like `CreateProductFunctionalTest` and the new `CountProductFunctionalTest`) would simply **extend** this base class.
    * **Benefit:** This approach eliminates duplication, centralizes configuration management, and keeps the test classes clean and focused on their specific scenarios.
