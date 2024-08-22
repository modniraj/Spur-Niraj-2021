# Spur-Niraj-2021


# Automated Testing with Selenium and HTMLTestRunner

## Table of Contents
1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Environment Setup](#environment-setup)
4. [Project Structure](#project-structure)
5. [Understanding the Test Script](#understanding-the-test-script)
6. [Running the Tests](#running-the-tests)
7. [Interpreting the HTML Report](#interpreting-the-html-report)
8. [Troubleshooting](#troubleshooting)
9. [Additional Resources](#additional-resources)

---

## Introduction
This guide explains how to set up and execute Selenium-based automated testing using Python. The tests are designed to interact with a specific web application, perform a series of actions, and generate a detailed HTML report using `HTMLTestRunner`.

## Prerequisites
Before you begin, ensure that the following prerequisites are met:

1. **Operating System**: This guide assumes you are using Windows, but the steps should be similar for macOS or Linux.
2. **Python**: Python 3.6 or higher should be installed on your machine.
3. **WebDriver**: Microsoft Edge WebDriver must be installed and accessible via PATH.

## Environment Setup

### 1. Install Python
- **Download**: Get the latest version of Python from [python.org](https://www.python.org/downloads/).
- **Install**: Follow the installation instructions, ensuring you check the option to add Python to your system PATH.

### 2. Install Required Python Packages
Open a terminal or command prompt and run the following commands to install the necessary packages:

```bash
pip install selenium
pip install html-testRunner
```

### 3. Download and Set Up Edge WebDriver
- **Download**: Visit the [Microsoft Edge WebDriver download page](https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/) and download the version that matches your installed Edge browser version.
- **Set Up**: Unzip the WebDriver and place it in a directory included in your system PATH, or specify the path directly in your scripts.

### 4. Verify Installation
Run the following command to verify that everything is set up correctly:

```bash
python --version
```

You should see the Python version. Also, test the WebDriver installation:

```bash
msedgedriver --version
```

This should print the version of the installed WebDriver.

## Project Structure

You can organize your project as follows:

```
/your-project-directory
|-- /report                 # Directory where HTML reports will be generated
|-- v1.py                 # Your main test script
|-- requirements.txt        # List of dependencies (optional)
```

The `v1.py` script contains the Selenium tests, and the `report` directory will hold the generated HTML reports.

## Understanding the Test Script

### Overview
The script `v1.py` is designed to interact with a web application hosted on a specific URL. The script performs the following steps:

1. **Navigate to the website** and wait for the page to load.
2. **Switch to the correct iframe** on the webpage.
3. **Locate and click elements** like "Testing XYZ," "Open CAPEX," and "Submit for approval" using Selenium.
4. **Capture any failures** and generate an HTML report with the status of each step.

### Structure
The script uses Python’s `unittest` framework to structure the tests and `HTMLTestRunner` to generate an HTML report. Each significant action is encapsulated in its own test method for better modularity and error handling.

#### Example Structure:

```python
import unittest
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
import time
from HtmlTestRunner import HTMLTestRunner

class V2TestSuite(unittest.TestCase):
    # setUp method to initialize the WebDriver
    def setUp(self):
        # Setup WebDriver, maximize window, and navigate to the target URL
        pass

    # Individual test methods for each test step
    def test_step_3(self):
        pass

    def test_step_4(self):
        pass

    def test_step_5(self):
        pass

    def test_step_6(self):
        pass

    # tearDown method to close the WebDriver
    def tearDown(self):
        pass

# Run the tests with HTMLTestRunner
if __name__ == "__main__":
    unittest.main(testRunner=HTMLTestRunner(output='report', report_name='V5.1TestReport'))
```

### Key Methods:
- **`setUp(self)`**: Initializes the WebDriver and navigates to the URL.
- **`test_step_3(self)`**: Attempts to click the "Testing XYZ" element.
- **`test_step_4(self)`**: Clicks another element identified as "Testing XYZ."
- **`test_step_5(self)`**: Interacts with the "Open CAPEX" button.
- **`test_step_6(self)`**: Attempts to click the "Submit for approval" button.
- **`tearDown(self)`**: Closes the WebDriver after tests.

## Running the Tests

1. **Navigate to your project directory**:
   ```bash
   cd /your-project-directory
   ```

2. **Run the script**:
   ```bash
   python v1.py
   ```

3. **Review the Output**:
   - The tests will execute, and you'll see output in your terminal or command prompt.
   - After execution, an HTML report will be generated in the `report` directory.

## Interpreting the HTML Report

- **Location**: The HTML report will be in the `report` directory with a filename like `V5.1TestReport.html`.
- **Open the Report**: Open the HTML file in your web browser to view the test results.
- **Contents**:
  - Each test method will be listed with its status (Pass/Fail).
  - If a test failed, details about the failure, including stack trace information, will be provided.

## Troubleshooting

### Common Issues:
1. **WebDriver Issues**: Ensure the WebDriver version matches your browser version.
2. **Element Not Found**: Increase the wait time or verify the XPath used to locate elements.
3. **Tests Always Pass/Fail**: Double-check your exception handling and ensure that exceptions are raised properly to reflect the actual test status.

### Debugging Tips:
- **Use `print` statements** in your code to output debugging information.
- **Check WebDriver Logs**: Sometimes WebDriver provides detailed logs that can help diagnose issues.
- **Run Tests Individually**: You can comment out other tests and run them one by one to isolate issues.

## Additional Resources

- **Selenium Documentation**: [Selenium WebDriver](https://www.selenium.dev/documentation/en/webdriver/)
- **HTMLTestRunner Documentation**: [HTMLTestRunner](https://pypi.org/project/html-testRunner/)
- **Python Unittest Documentation**: [unittest](https://docs.python.org/3/library/unittest.html)

---

This guide should provide a comprehensive overview of setting up and running your Selenium tests with HTMLTestRunner. If you follow these steps and use the provided code structure, you should be able to execute the tests and generate detailed reports successfully. If any issues arise, refer to the troubleshooting section for potential solutions.
