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

My reports include parameters and return values from Page Object Functions so we can better trace values when looking
at the report.
![Step with parameters](/resources/logParameters.png "Step Parameters")
![Step with return](/resources/logReturnValues.png "Step Return")

With this much detail we also need to ensure that any sensitive values are hidden.
![Step with hidden value](/resources/hideSensitiveStrings.png "Hidden Value")

All of these steps also nest as appropriate. This allows us to read a high level overview of the test, or dig deep into
the implementation.
![Many sub steps](/resources/manySubSteps.png)

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

## In Step Screenshots
Any screenshots captured by Playwright will appear as part of the main body of the Allure report. This gives the
screenshot less context, than if it appears within the failing step.
![Nested screenshot](/resources/nestedScreenshot.png)

## Multi Tab/Window Testing
We can capture screenshots for multiple tabs with Playwright by default. Unfortunately all screenshots will be named 
'screenshot'. This makes the report less clear, and debugging more difficult. Having each tab or window named improves
the report.
![Multi tab screenshot](/resources/multiTabScreenshot.png)
