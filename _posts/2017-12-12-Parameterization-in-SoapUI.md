---
title: Parameterization in SoapUI
author: Piyumi Dissanayake  
linkedin: https://lk.linkedin.com/in/piyumi-dissanayake-0241002b
---

There are many ways of handling parameters in SoapUI API Tests. This
article is based on how to handle Project wide global variables for REST
requests, when it comes to API Testing using SoapUI tool.

Following is a step-by-step guide on how to add Request Level parameters
which can be used by the test cases inside of the project for a simple
HTTP REST API. This API needs an API key to use the API - this is a
unique key that identifies your requests.

Therefore, the Authentication is handled with an API key.

There are three ways of attaching the API key to a request described as
follows.

-   Via the ApiKey query string parameter.

-   Via the X-Api-Key HTTP header.

-   Via the Authorization HTTP header.

Therefore, this API Key is one of the mandatory parameters for the API
test we are going to write, since it must be used in each and every
request within the API.

In that case, it is recommended to use the API Key as a global Parameter
for the whole project. Because that approach allows us to reuse it in
all the test cases, we are going to write.

When you are creating the REST Project, You can add REST Requests for
each and Every Endpoint in your API. For each of those Requests to can
define what are the parameters involved as shown in the following image.

If you have a parameter that has a unique value which should be reused
in all your test cases you can populate the Value field of the query
string parameter with that value so that it will get pre-populated in
each and every test case within the project. In addition, if you have
parameters, which should be dynamically populated at the test case
Level, you should leave those parameters Empty.

<img src="/img/piyumi1.PNG" width="640" height="364" />

When you are creating the test cases, you simply have to choose what
Resource/ Endpoint you are using for the specific test case. Then it
will populate all the parameters and values involved. If you need to
change any Parameter value, you can do that as well.

<img src="/img/piyumi2.PNG" width="624" height="355" />

By following this approach, parameters can be easily reused within the
project and will save time spent on defining parameters at the test step
level as well.
