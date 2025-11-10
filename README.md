# Python Behave BDD Practical for Beginners

## Introduction to BDD and Behave

**Behavior-Driven Development (BDD)** is a software development approach that allows technical and non-technical team members to collaborate using simple, human-readable language to describe how software should behave[1][4].

**Behave** is a BDD framework for Python that allows you to write tests in plain English using a language called Gherkin, which uses keywords like `Given`, `When`, `Then`, `And`[1][4].

---

## Prerequisites

Before starting this practical, ensure you have:
- Python 3.7 or higher installed
- Visual Studio Code installed
- Basic understanding of Python syntax
- pip (Python package manager) installed

---

## Part 1: Setting Up Your Environment

### Step 1: Install Behave

Open your terminal or command prompt and run:

```bash
pip install behave
```

To verify the installation:

```bash
behave --version
```

You should see the version number displayed (e.g., `behave 1.2.6`)[37].

### Step 2: Install VS Code Extension

1. Open Visual Studio Code
2. Go to Extensions (Ctrl+Shift+X or Cmd+Shift+X on Mac)
3. Search for "Behave VSC" by jimasp
4. Click Install[8]

This extension provides:
- Syntax highlighting for Gherkin feature files
- Test explorer integration
- Step navigation between feature files and step definitions
- Ability to run tests directly from feature files[8]

---

## Part 2: Project Structure

### Step 1: Create Project Folder

Create a new folder for your project called `calculator_bdd`:

```
calculator_bdd/

mkdir calculator_bdd
```

### Step 2: Open in VS Code

1. Open Visual Studio Code
2. Go to File > Open Folder
3. Select your `calculator_bdd` folder

### Step 3: Create Required Folders and Files

Your project structure should look like this[21][28]:

```
calculator_bdd/
├── features/
│   ├── calculator.feature
│   └── steps/
│       └── calculator_steps.py
```

**Explanation:**
- **features/**: Main directory containing all feature files
- **calculator.feature**: Feature file written in Gherkin language
- **steps/**: Directory containing Python step definitions
- **calculator_steps.py**: Python code that implements the steps

---

## Part 3: Writing Your First Feature File

### Step 1: Create the Feature File

In the `features` folder, create a file called `calculator.feature` and add the following content[21][42]:

```gherkin
Feature: Simple Calculator
    As a user
    I want to use a simple calculator
    So that I can perform basic arithmetic operations

    Scenario: Add two numbers
        Given I have a calculator
        When I add 5 and 3
        Then the result should be 8

    Scenario: Subtract two numbers
        Given I have a calculator
        When I subtract 3 from 10
        Then the result should be 7

    Scenario: Multiply two numbers
        Given I have a calculator
        When I multiply 4 by 6
        Then the result should be 24
```

**Understanding Gherkin Keywords:**
- **Feature**: Describes the high-level functionality being tested[3][9]
- **Scenario**: Describes a specific test case[3][9]
- **Given**: Sets up the initial context or preconditions[3][15]
- **When**: Describes the action being performed[3][15]
- **Then**: States the expected outcome[3][15]
- **And/But**: Adds additional steps to make scenarios more readable[3][15]

---

## Part 4: Creating the Calculator Class

### Step 1: Create Calculator Implementation

In your project root (not in features folder), create a file called `calculator.py`:

```python
class Calculator:
    """A simple calculator class for basic arithmetic operations"""
    
    def __init__(self):
        self.result = 0
    
    def add(self, a, b):
        """Add two numbers and return the result"""
        self.result = a + b
        return self.result
    
    def subtract(self, a, b):
        """Subtract b from a and return the result"""
        self.result = a - b
        return self.result
    
    def multiply(self, a, b):
        """Multiply two numbers and return the result"""
        self.result = a * b
        return self.result
    
    def divide(self, a, b):
        """Divide a by b and return the result"""
        if b == 0:
            raise ValueError("Cannot divide by zero")
        self.result = a / b
        return self.result
```

---

## Part 5: Implementing Step Definitions

### Step 1: Create Step Definitions File

In the `features/steps` folder, create `calculator_steps.py` with the following content[21][22]:

```python
from behave import given, when, then
from calculator import Calculator

@given('I have a calculator')
def step_given_calculator(context):
    """Initialize the calculator and store it in context"""
    context.calculator = Calculator()

@when('I add {num1:d} and {num2:d}')
def step_when_add(context, num1, num2):
    """Add two numbers using the calculator"""
    context.result = context.calculator.add(num1, num2)

@when('I subtract {num2:d} from {num1:d}')
def step_when_subtract(context, num1, num2):
    """Subtract num2 from num1 using the calculator"""
    context.result = context.calculator.subtract(num1, num2)

@when('I multiply {num1:d} by {num2:d}')
def step_when_multiply(context, num1, num2):
    """Multiply two numbers using the calculator"""
    context.result = context.calculator.multiply(num1, num2)

@then('the result should be {expected:d}')
def step_then_result(context, expected):
    """Verify the result matches the expected value"""
    assert context.result == expected, f"Expected {expected}, but got {context.result}"
```

**Understanding the Code:**

1. **Imports**: We import the decorators (`given`, `when`, `then`) from behave and our Calculator class[21]

2. **Context Object**: The `context` variable is a special object that Behave uses to share data between steps. It's automatically managed by Behave[21][13]

3. **Step Decorators**: Each function is decorated with `@given`, `@when`, or `@then` matching the keywords in the feature file[21]

4. **Step Parameters**: The `{num1:d}` syntax extracts integers from the step text. The `:d` means "parse as integer"[21]

---

## Part 6: Running Your Tests

### Method 1: Using Terminal

Open the terminal in VS Code (Ctrl+` or View > Terminal) and run:

```bash
behave
```

You should see output like this[21]:

```
Feature: Simple Calculator

  Scenario: Add two numbers
    Given I have a calculator
    When I add 5 and 3
    Then the result should be 8

  Scenario: Subtract two numbers
    Given I have a calculator
    When I subtract 3 from 10
    Then the result should be 7

  Scenario: Multiply two numbers
    Given I have a calculator
    When I multiply 4 by 6
    Then the result should be 24

3 scenarios passed, 0 failed, 0 skipped
9 steps passed, 0 failed, 0 skipped, 0 undefined
```

### Method 2: Using VS Code Debugger

To debug your tests in VS Code, create a launch configuration[2][5]:

1. Click on the Debug icon in the left sidebar (or press Ctrl+Shift+D)
2. Click "create a launch.json file"
3. Add this configuration:

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Behave Current File",
            "type": "python",
            "request": "launch",
            "module": "behave",
            "console": "integratedTerminal",
            "args": [
                "${file}"
            ]
        }
    ]
}
```

Now you can:
- Open any `.feature` file
- Press F5 to run/debug that specific feature
- Set breakpoints in your step definition files[2][5]

### Method 3: Using Behave VSC Extension

With the Behave VSC extension installed, you can:
- Click the play button next to scenarios in feature files
- View test results in the Test Explorer panel
- Navigate between steps and their definitions[8]

---

## Part 7: Adding More Scenarios with Data Tables

### Step 1: Enhance Feature File with Scenario Outline

Add this to your `calculator.feature` file[21][42]:

```gherkin
    Scenario Outline: Add multiple number combinations
        Given I have a calculator
        When I add <num1> and <num2>
        Then the result should be <expected>

        Examples:
            | num1 | num2 | expected |
            | 1    | 1    | 2        |
            | 5    | 5    | 10       |
            | 100  | 200  | 300      |
            | 0    | 0    | 0        |
```

**Scenario Outline** allows you to run the same scenario multiple times with different data. The `Examples` table provides the test data[42].

### Step 2: Run Tests Again

```bash
behave
```

The scenario outline will run once for each row in the Examples table, giving you 4 additional test cases automatically!

---

## Part 8: Understanding Test Results

When you run `behave`, the output shows:

- **Green**: Passed steps
- **Red**: Failed steps  
- **Yellow**: Skipped steps
- **Cyan**: Undefined steps (steps without implementation)[21]

If a test fails, you'll see:

```
Failing scenarios:
  features/calculator.feature:10  Add two numbers

0 features passed, 1 failed, 0 skipped
1 scenario passed, 1 failed, 0 skipped
```

---

## Part 9: Common Behave Commands

Run all tests:
```bash
behave
```

Run a specific feature file:
```bash
behave features/calculator.feature
```

Run with verbose output:
```bash
behave -v
```

Run and stop at first failure:
```bash
behave --stop
```

Show all step definitions:
```bash
behave --dry-run
```

---

## Part 10: COMPREHENSIVE BEGINNER EXERCISE

### Exercise: Create a Temperature Converter

In this exercise, you'll create a complete Behave test project for a **Temperature Converter** that converts between Celsius, Fahrenheit, and Kelvin.

---

### ✏️ Your Task

Create a complete working Behave project with the following requirements:

#### 1. Create Feature File

Create `features/temperature_converter.feature` with **at least 4 scenarios** covering:
- Convert Celsius to Fahrenheit
- Convert Fahrenheit to Celsius
- Convert Celsius to Kelvin
- At least one edge case (e.g., absolute zero, negative temperatures)

**Example format to guide you** (but expand on this):
```gherkin
Feature: Temperature Converter
    As a user
    I want to convert between temperature scales
    So that I can work with different temperature units

    Scenario: Convert 0 Celsius to Fahrenheit
        Given I have a temperature converter
        When I convert 0 degrees Celsius to Fahrenheit
        Then the result should be 32 degrees Fahrenheit
```

#### 2. Create Implementation Class

Create `temperature_converter.py` with a class that:
- Has methods to convert between Celsius, Fahrenheit, and Kelvin
- Handles edge cases appropriately
- Stores the last conversion result

**Example structure**:
```python
class TemperatureConverter:
    def celsius_to_fahrenheit(self, celsius):
        # Implementation here
        pass
    
    def fahrenheit_to_celsius(self, fahrenheit):
        # Implementation here
        pass
    
    def celsius_to_kelvin(self, celsius):
        # Implementation here
        pass
```

#### 3. Create Step Definitions

Create `features/steps/temperature_steps.py` with step implementations for:
- Initializing the converter (`@given`)
- Performing conversions (`@when`)
- Verifying results (`@then`)

#### 4. Run Your Tests

Execute your test suite:
```bash
behave
```

**All scenarios must pass** before submission.

---

### 📋 Submission Checklist

Please verify your work against these criteria before submitting:

#### ✅ Project Structure
- [ ] Project folder name is appropriate (e.g., `temperature_converter_bdd`)
- [ ] Folder structure exactly matches requirements:
  ```
  project/
  ├── temperature_converter.py
  └── features/
      ├── temperature_converter.feature
      └── steps/
          └── temperature_steps.py
  ```
- [ ] No extra unnecessary files or folders in the project root

#### ✅ Feature File Quality
- [ ] Feature file uses proper Gherkin syntax
- [ ] At least 4 distinct scenarios are present
- [ ] Each scenario has Given-When-Then structure in correct order[6]
- [ ] Scenarios test different use cases (not just variations of same test)
- [ ] Scenario titles are descriptive and clear
- [ ] No procedure-driven imperative steps (use declarative language)[6]
- [ ] Feature description explains the "As a..., I want..., So that..." perspective[6][79]

#### ✅ Calculator/Converter Class
- [ ] Class is properly defined with clear method names
- [ ] All conversion methods are implemented
- [ ] Methods return correct numerical results
- [ ] Edge cases are handled appropriately (negative numbers, extreme values)
- [ ] No syntax errors when importing the class

#### ✅ Step Definitions
- [ ] All step functions use correct decorators (@given, @when, @then)
- [ ] Step function names match the feature file text exactly
- [ ] Steps are parameterized using `{value:type}` syntax correctly
- [ ] Context object is used properly to share data between steps[21]
- [ ] Assertions use clear, meaningful error messages
- [ ] All undefined steps are implemented (no cyan/blue steps in output)

#### ✅ Test Execution
- [ ] Running `behave` shows all scenarios passed
- [ ] Output shows "0 failed" results
- [ ] No errors or exceptions during execution
- [ ] Test output is clean and shows proper formatting

#### ✅ Code Quality
- [ ] Python code follows basic style guidelines
- [ ] Variable names are meaningful and consistent
- [ ] Comments explain complex logic (if needed)
- [ ] No hardcoded test data (use parameters instead)
- [ ] Code is readable and maintainable

#### ✅ BDD Best Practices[6][79][81]
- [ ] Scenarios are concise and focused on single behaviors
- [ ] No unnecessary implementation details in feature file
- [ ] Steps are reusable (not too specific or too generic)
- [ ] Language is user-focused, not technical
- [ ] Scenarios could be read by non-technical stakeholders

---

### 🎯 Submission Requirements

Submit the following:

1. **Complete project folder** with all files properly organized
2. **Output screenshot** showing all tests passing
3. **Feature file contents** (paste the feature file text)
4. **Implementation class** (paste the converter class code)
5. **Step definitions** (paste the step definitions code)

OR

Submit as a **ZIP file** containing the complete working project.

---

### ❓ Common Questions

**Q: Can I use an online calculator or existing code?**
A: This is a learning exercise. You should write the converter yourself, but you can verify calculations with an online tool.

**Q: How many scenarios are enough?**
A: Minimum 4, but more shows deeper understanding. Aim for 5-6 covering various cases.

**Q: Do I need to add Scenario Outlines?**
A: Not required for this exercise, but using them shows advanced understanding.

**Q: What if my conversion math is slightly off?**
A: Use rounding in your assertions: `assert abs(result - expected) < 0.01`

**Q: Can I share code with classmates?**
A: No, this is individual assessment. However, discussing the approach is fine.

---

### 🚀 How to Submit

Prepare your submission by:

1. **Verify all tests pass**: Run `behave -v` and capture the output
2. **Collect all files**: Feature file, implementation class, step definitions
3. **Create ZIP file** with complete project structure
4. **Include README** with instructions to run the tests
5. **Submit** through Brightspace

---

## Part 11: Troubleshooting

**Problem: "ImportError: No module named behave"**
- Solution: Make sure you installed behave with `pip install behave`

**Problem: "No steps directory"**
- Solution: Ensure your folder structure matches the required format with `features/steps/` directory[21]

**Problem: Steps showing as "undefined"**
- Solution: Check that your step definitions exactly match the text in your feature file[21]

**Problem: "ModuleNotFoundError: No module named 'temperature_converter'"**
- Solution: Make sure `temperature_converter.py` is in your project root directory, not inside the `features` folder

**Problem: "AssertionError: Expected X but got Y"**
- Solution: Check your conversion formulas. Verify calculations manually first.

**Problem: Steps not being recognized by Behave**
- Solution: Ensure your step definition file is in `features/steps/` and the decorator text matches your feature file exactly

---

## Summary: What You've Learned

✅ What BDD and Behave are and why they're valuable
✅ How to install and configure Behave in VS Code
✅ The required project structure for Behave projects
✅ How to write feature files using Gherkin syntax
✅ How to implement step definitions in Python
✅ How to run tests using multiple methods
✅ How to use Scenario Outlines for data-driven testing
✅ How to debug Behave tests in VS Code
✅ **How to build a complete working Behave project from scratch**

---

## Next Steps After Completing the Exercise

Once you've successfully completed the temperature converter exercise:

1. **Review feedback** and understand areas for improvement
2. **Explore the intermediate practical** for advanced concepts
3. **Try adding more features** like error handling and rounding options
4. **Integrate with a real application** - create a CLI tool that uses your converter
5. **Share your project** with others and get code review feedback
6. **Read the official documentation** at https://behave.readthedocs.io/

---

## References

[1] https://behave.readthedocs.io/
[2] https://qxf2.com/ - VS Code Behave execution
[3] https://browserstack.com/ - Gherkin guide
[4] https://techlistic.com/ - BDD with Behave
[6] https://automationpanda.com/ - Writing Good Gherkin
[8] VS Code Behave Extension
[13] https://lambdatest.com/ - Behave tutorial
[15] https://accelq.com/ - Format and syntax
[21] https://behave.readthedocs.io/en/latest/ - Tutorial
[22] https://automationpanda.com/ - Python Testing 101
[28] Behave examples
[37] https://behave.readthedocs.io/en/latest/install.html
[42] https://behave.readthedocs.io/en/latest/step_matchers.html
[79] https://lambdatest.com/ - BDD Complete Guide
[81] https://accelq.com/ - BDD Testing

Happy Learning and Testing! 🎓✅
