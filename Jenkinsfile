#!groovy
@Library('jenkins-shared-library') _

def configMap = [
    component = "catalogue",
    application = "nodejsVM"
]

if (! env.BRANCH_NAME.equalsIgnoreCase('main')) {
    pipelineDecision.decidePipeline(configMap)
else 
    echo "This is not a featiure branch, Deal with CR process."
}