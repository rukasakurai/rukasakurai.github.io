*Under construction*
# Context
Testing 


# Who/what does the testing
Same as most other task, testing can be automated by machines (e.g., Azure virtual machines). However, just because it can be automated by machines doesn't automatically mean that it should be. In some/many cases humans are capable of performing testing faster, cheaper, and with higher quality.

Types of testing is usually performed by humans include
- Testing where acceptance criteria includes subjectivity
  - User acceptance testing
  - Exploratory testing
  - Stakeholder feedback
- Testing of experimental features/products

On the other hand, when the acceptance critieria has no subjectivity, and when the feature being tested is expected to continue to exist, then one should seriously consider the automation of the regression test (i.e., testing that existing features work). Even for such regression tests, automating it and maintaining it might be more effort than, continuing the manual testing, but tools such as Selenium could help reduce thie barrier.

This article also talks about manual testing verus automated testing

# Testing and devices
All software runs on some sort of hardware. A lot of software these days runs on web browers or on other applications on laptops or smartphones. However, there is software that runs on other devices such as vehicles and home appliances, which in many cases run a lighter operating system than say Windows 10 (about 15GB) or iOS (about 2GB), which may not have Internet/intranet connectivity, and which include a wider range of sensors and actuators. When the device does not have Internet/intranet connectivity, and when it has more sensors and actuators, this adds constraints to the testing process.

If a device does not have Internet/intranet connectivity, feedback needs to be communicated in a different way. In some cases, while a device may not have persistent Internet/intranet connectivity, it may be possible to communicate it's data, in a more infrequent manner. If so, then while feedback will be less frequent, it can be communicated back to the development teams. I assume (should check assumption) that it is becoming easier and cheaper to connect devices to the Internet/intranet, and if so, more frequent communication of user feedback should be possible, even for products/services that are on devices other than laptops and smartphones.

Devices that traditionally do no thave Internet/intranet connectivity, are tested in a ways that are different from say software that run on web browsers. The automotive industry uses a v-cycle software development process, where acceptance criteria are communicated in phases. For example, this [article](https://x-engineer.org/graduate-engineering/modeling-simulation/model-based-design/essential-aspects-of-the-v-cycle-software-development-process/) contains a process where acceptance criteria is first communicated in PDF format, and then in xcos. A hardware supplier manufactures a device (e.g., HVAC, ECU) according to the specifications. Before shipping the product, the supplier tests that the device functions according to the acceptance criteria, and ships the device to the vehicle manufacturer. The vehicle manufacturer integrates various devices together, and tests the vehicle.

# Regression test automation
Step 1

# Azure Test Plans
[Azure Test Plans](https://docs.microsoft.com/en-us/azure/devops/test/overview) which is part of Azure DevOps is a tool to support manual testing.

# Using Azure Test Plans
Using Azure Test Plans occurs mainly in two steps. The first is to create the tests, and the second is to run the tests. Test cases are created as Test Cases, and Test Cases are grouped into Test Plans or Test Suites. Test Cases in Test Plans or Test Suites can be executed manually by 'Run'ning them, and the results will appear under Progress report and Runs. 
[Best practices for creating Test Cases](https://daveklloyd.medium.com/azure-test-plans-test-cases-39aa1fb1e9b7)
[Best practices for creating and executing Test Plans](https://daveklloyd.medium.com/azure-test-plans-test-plans-212fbaaa68e3)