With AWS Lambda, you upload your code and run it without thinking about servers. Many customers enjoy the way this works, but if you’ve invested in container tooling for your development workflows, it’s not easy to use the same approach to build applications using Lambda.

To help you with that, you can now package and deploy Lambda functions as container images of up to 10 GB in size. In this way, you can also easily build and deploy larger workloads that rely on sizable dependencies, such as machine learning or data intensive workloads. Just like functions packaged as ZIP archives, functions deployed as container images benefit from the same operational simplicity, automatic scaling, high availability, and native integrations with many services.

We are providing base images for all the supported Lambda runtimes (Python, Node.js, Java, .NET, Go, Ruby) so that you can easily add your code and dependencies. We also have base images for custom runtimes based on Amazon Linux that you can extend to include your own runtime implementing the Lambda Runtime API.

You can deploy your own arbitrary base images to Lambda, for example images based on Alpine or Debian Linux. To work with Lambda, these images must implement the Lambda Runtime API. To make it easier to build your own base images, we are releasing Lambda Runtime Interface Clients implementing the Runtime API for all supported runtimes. These implementations are available via native package managers, so that you can easily pick them up in your images, and are being shared with the community using an open source license.

We are also releasing as open source a Lambda Runtime Interface Emulator that enables you to perform local testing of the container image and check that it will run when deployed to Lambda. The Lambda Runtime Interface Emulator is included in all AWS-provided base images and can be used with arbitrary images as well.

Your container images can also use the Lambda Extensions API to integrate monitoring, security and other tools with the Lambda execution environment.


to create a lambda function container on windows you need to have node installed 
1. Create an application directory, set up npm, install the Faker.js package for generating test data:

        mkdir getCustomerFunction
        cd getCustomerFunction
        npm init –y
        npm i faker --save
      
2. Create a file called app.js and paste the following code. This the same Lambda handler code you use in a regular zip-file deployment:

        const faker = require('faker')
        module.exports.lambdaHandler = async (event, context) => {
            return faker.helpers.createCard()
        }
3. Create a file called Dockerfile and paste the following code. This instructs Docker how to build the container, installs any necessary packages, and shows where the Lambda handler is available.

        FROM public.ecr.aws/lambda/nodejs:12
        COPY app.js package*.json ./
        RUN npm install
        CMD [ "app.lambdaHandler" ]
