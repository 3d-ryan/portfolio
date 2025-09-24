# Playwright
While I have more years working with Selenium Webdriver I'm very excited about Playwright as a testing framework. It
provides a lot of what we need out of the box including browsers and drivers, parallelization, retry logic, screenshot,
and debug tools. It's also easy to include Allure as a reporting option, which is my preferred reporting tool. However,
there are several areas for improvement.

## Multi Tab/Window Testing
We can capture screenshots for multiple tabs with Playwright by default. Unfortunately all screenshots will be named 
'screenshot'. This makes the report less clear, and debugging more difficult. Having each tab or window named improves
the report:

![Multi Tab Screenshots Image](/resources/multiTabScreenshot.png)
