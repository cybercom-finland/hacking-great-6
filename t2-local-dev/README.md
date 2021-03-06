# \#HackingGreat 6 - Serverless with AWS - @Cybercom

## Tech Track 2 - Develop your Serverless Application  Locally

### Summary

Serverless applications can be developed on a laptop offline, either by running 
the code locally or running a serverless platform locally. Special interest is 
in Open Source serverless frameworks and platforms. 

The local development is also an easy way to get started without any cloud 
credentials.

### Instructions

#### Serverless Framework <https://serverless.com>

##### AWS-credentials

To run the AWS-examples, even the offline ones, you need to have AWS credentials configured.

To configure the credentials to profile, do the following with AWS-cli

    aws configure --profile hg6

The profile name will be saved to environment variable later.

##### Serverless binary

Pull the serverless client locally to the repo

    (cd .. && npm install serverless)

It is going to be referred locally using shell alias later.

##### Simple example using AWS Lambda

The first example is an introduction to Serverless Framework using AWS Lambda 
Free Tier.

    # Configure the environment
    export AWS_PROFILE=hg6
    # node_modules is in the root of git clone
    alias serverless="$(git rev-parse --show-toplevel)"/node_modules/serverless/bin/serverless

    # The example is generated using serverless --template aws-nodejs
    cd serverless/

    # Run test, test and remove
    serverless deploy -v 
    serverless invoke -f hello -l
    serverless remove

##### Example using serverless-offline features

The next more complete example is from Serverless Framework Examples 
repository.

You need place where to check out the examples repository. `~/git` is used here 
as an example.

    # Configure the environment
    export AWS_PROFILE=hg6
    # node_modules is in the root of git clone
    alias serverless="$(git rev-parse --show-toplevel)"/node_modules/serverless/bin/serverless

    cd ~/git
    git clone https://github.com/serverless/examples.git serverless-examples
    cd serverless-examples/aws-node-rest-api-with-dynamodb-and-offline/

    # Summarised startup from the README.md
    npm install
    serverless dynamodb install
    serverless offline start

    # And then in another unrelated terminal test the local installation
    # POST
    curl -X POST -H "Content-Type:application/json" http://localhost:3000/todos --data '{ "text": "Learn Serverless" }'
    # And GET
    curl -H "Content-Type:application/json" http://localhost:3000/todos

#### Kubeless

Kubeless runs functions inside Kubernetes. The example here uses OpenShift as 
Kubernetes platform.

Download OpenShift client <https://github.com/openshift/origin/releases> and 
kubeless client <https://github.com/kubeless/kubeless/releases>.

    oc cluster up
    oc login -u system:admin
    oc new-project kubeless
    export RELEASE=v0.5.0
    oc create -f https://github.com/kubeless/kubeless/releases/download/$RELEASE/kubeless-openshift-$RELEASE.yaml
    # see that plugin is running
    oc get pods -n kubeless

First simple test function from the docs

    oc new-project ftest
    kubeless function deploy get-python --runtime python2.7 --from-file kubeless/test.py --handler test.foobar
    kubeless function call get-python --data '{"echo": "hello hacking great"}'

#### Other options

-  <https://open.iron.io> Open Source Serverless Computing - Docker based application images, platform is easy to run

#### Requirements

Laptop with preferably a Linux installed.

- Node.JS
