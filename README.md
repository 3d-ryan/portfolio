# Welcome
Welcome to my portfolio. My goal is to create a public facing document that helps me demonstrate my test and development
skills. If you're interested in my work, or think I would be a good fit for a QA position at your company please [reach
out](www.linkedin.com/in/david-ryan-447a23169).

# Table of Contents
1. [Playwright](#playwright)
   1. [Detailed Test Steps](#detailed-test-steps)
      1. [Parameter and Return Logging](#parameter-and-return-logging)
      2. [Logging Complex Types](#logging-complex-types)
      3. [Anonymize Sensitive Data](#anonymize-sensitive-data)
      4. [Nested Steps and Helper Functions](#nested-steps-and-helper-functions)
   2. [BDD Format](#bdd-format)
   3. [Expected Errors](#expected-errors)
   4. [In Step Screenshots](#in-step-screenshots)
   5. [Multi Tab/Window Testing](#multi-tabwindow-testing)
   6. [Soft Assert Screenshots](#soft-assert-screenshots)
   7. [Element Screenshots](#element-screenshots)
2. [Selenium Java](#selenium-java)
3. [PHP Web Game](#php-web-game-laravel)

# Playwright
While I have more years working with Selenium Webdriver I'm very excited about Playwright as a testing framework. It
provides a lot of what we need out of the box including browsers and drivers, parallelization, retry logic, screenshot,
and debug tools. It's also easy to include Allure as a reporting option, which is my preferred reporting tool. However,
there are several areas for improvement.

## Detailed Test Steps
One of my first objectives with any test project is to ensure the report provides detailed steps. Our automated test
report should read like a manual test case, or in the case of failure the 'steps to reproduce' of a bug report. This
has several advantages:
- People do not need to look at the test code to understand the scope or steps of the test.
- Anyone investigating a failure has steps to reproduce, which they can perform manually if needed.
- The test report can act as steps to reproduce for bug reports.
- Detailed reports make it easier to debug and fix broken tests.
- Test reports can be used as internal product documentation for onboarding or training.
- When written into the Page Object Model new tests become self documenting.
![Detailed report](/resources/detailedReport.png)

### Parameter and Return Logging
My reports include parameters and return values from Page Object Functions so we can better trace values when looking
at the report.
![Step with parameters](/resources/logParameters.png "Step Parameters")
![Step with return](/resources/logReturnValues.png "Step Return")

### Logging Complex Types
I've designed the additional logging to handle complex types such as objects and arrays. The report will allow us to
drill down into the fine details of any complex variables.
![Array return drill down screenshot](/resources/returnArray.png)

### Anonymize Sensitive Data
With this much detail we also need to ensure that any sensitive values are hidden.
![Step with hidden value](/resources/hideSensitiveStrings.png "Hidden Value")

### Nested Steps and Helper Functions
All of these steps also nest as appropriate. This allows us to read a high level overview of the test, or dig deep into
the implementation.
![Many sub steps](/resources/manySubSteps.png)

We can see above an example of a helper function. This is a pattern I use to improve logs and reduce code duplication.
I've seen many page classes with repeated functionality like:
- fillForm()
- completeForm()
- fillFormRequired()
- uploadPhotoAndSubmitForm()
- fillFormAndCancel()
- fillFormWithInvalidDataClickSubmitAndWaitForError()

The last one is an exaggeration, but only by a bit 😬

Writing functions like this into a page object is an example of
test logic creeping into page logic. These functions clog up our page object model and create a bunch of duplicate code.
Providing a helper class to pages that have complex sequences of actions helps keep the page class cleaner, while still
providing helpful sequences for our tests.

## BDD Format
I have also built in logging utilities for writing test cases in BDD format. This can give us the desired output format
without having to depend on a keyword driven framework.
![BDD test steps](/resources/bddFormat.png)

## Expected Errors
Another common issue I have seen is related to negative or edge case testing. Our Page Object Model often includes
validating steps to ensure our actions are complete and correct. For example, after clicking the 'Log In' button on
a login page, we expect the login button to no longer be visible. This is important for test stability because we want
to ensure our test is in a known and stable state before we do the next action.

Issues arise when we have to test scenarios that do not fit our expected behaviour. I have seen many page functions like
clickLogInAndWaitForFailure(). Functions like this clog up our page object model and often have poor performance as we
wait for operations to time out.

Providing a generic utility function expectError() allows us to test edge cases on any page without writing additional
page object functions. We can also control our timeouts in this context to improve test performance. I've reduced test
suite execution time by up to 20% using this approach.
![Expected error](/resources/expectError.png)

My framework has a default timeout of 5 seconds, but we can see the failing step executes in about 1 second.
Additionally, we make an assertion that an exception was actually thrown. This creates an implicit check for the
correctness of our Page Object Model. Login should fail when we enter the wrong credentials, and our page object should
throw an error in that situation.

## In Step Screenshots
Any screenshots captured by Playwright will appear as part of the main body of the Allure report. This gives the
screenshot less context than if it appears within the failing step.
![Nested screenshot](/resources/nestedScreenshot.png)

## Multi Tab/Window Testing
We can capture screenshots for multiple tabs with Playwright by default. Unfortunately all screenshots will be named 
'screenshot'. This makes the report less clear, and debugging more difficult. Having each tab or window named improves
the report.
![Multi tab screenshot](/resources/multiTabScreenshot.png)

## Soft Assert Screenshots
Playwright provides the ability to make soft assertions. If a soft assertion fails the test will continue to execute and
report any failures at the end. By default, we will only have a screenshot at the end of the test. We want to be able
to see a screenshot of our application at the time the failure occurred.
![Soft asserts with screenshots](/resources/screenshotSoftExpectFailures.png)

## Element Screenshots
With Playwright, we can screenshot individual elements and make image comparisons. The issue with the default behaviour
is that images are treated as unique to each individual environment and browser. This means that our developers, and our
CI will require their own versions of any snapshot we generate. The reason for this is clear, different browsers or OS
will often generate a slightly different image of a given element. However, these differences are often only a few
pixels of white space.

By implementing my own screenshot comparison we can have a single image act as the source of truth for all browsers and
environments. My algorithm will resize images (if necessary) to meet the expected image size, then perform the
comparison using the same underlying libraries Playwright uses.

If we must deal with elements that have significant differences between browsers or environments we can provide
different expected images given the current test settings.

In my example report if a failure occurs Allure will show the expected screenshot, the actual screenshot, and the diff
between the two.
![Allure report with failing screenshot comparison](/resources/screenshotMatchExample.png)

# Selenium Java
ToDo.

# PHP Web Game (Laravel)
ToDo.