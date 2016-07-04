#Continuous testing in the world of APIs

This talk was run in the format of talking about the evolution of testing Demond's company had been through. 

This evolution went across 3 streams:

1. Ways of creating tests and test data

They had simple static tests with sample test data. Problem when new people joined the team and changed the test data meant that tests were failing, and therefore disregarded. Then started using external APIs for creating test data, that failed too because the external APIs went down regularly. Last try was to spin the entire environment with a set of data in it - that was much more successful. 

2. Process of executing, maintaining tests and agreements surrounding it

Initially, the tester wrote the tests on paper, then developer automated and maintained the tests. New process is for devs to make PR, tester then tests the tests (inception.gif!)

3. Applicability, evaluation of the tool stack we use. 

Lots of different tools used initially, by they then created a new tool (using cucumber) - [Minosse](https://github.com/icemobilelab/minosse). 
Human readable - generic set of common steps for testing API. 

They tag all the tests according to the service name / API test. They also have a Jenkins dashboard to figure out wwhich test is failing. 



 
