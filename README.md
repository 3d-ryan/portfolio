# Playwright
While I have more years working with Selenium Webdriver I'm very excited about Playwright as a testing framework. It provides a lot of what we need out of the box including browsers and drivers, parallelization, retry logic, screenshot, and debug tools. It's also easy to include Allure as a reporting option, which is my preferred reporting tool. However, there are several areas for improvment.

## Multi Tab/Window Testing
We can capture screenshots for mulitple tabs with Playwright by default. Unfortunately all screenshots will simply be named 'screenshot'. It is much more helpful for debugging test failures to have named screenshots for each tab we are working with.

