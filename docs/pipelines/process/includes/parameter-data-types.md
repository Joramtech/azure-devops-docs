---
ms.topic: include
ms.service: azure-devops-pipelines
ms.manager: mijacobs
ms.author: jukullam
author: juliakm
ms.date: 08/30/2024
ai-usage: ai-assisted
---

| Data type | Notes |
|-----------|-------|
| `string` | string
| `number` | may be restricted to `values:`, otherwise any number-like string is accepted
| `boolean` | `true` or `false`
| `object` | any YAML structure
| `step` | a single step
| `stepList` | sequence of [steps](/azure/devops/pipelines/yaml-schema/steps)
| `job` | a single job
| `jobList` | sequence of [jobs](/azure/devops/pipelines/yaml-schema/jobs-job)
| `deployment` | a single deployment job
| `deploymentList` | sequence of deployment jobs
| `stage` | a single stage
| `stageList` | sequence of stages

The step, stepList, job, jobList, deployment, deploymentList, stage, and stageList data types all use standard YAML schema format. This example includes string, number, boolean, object, step, and stepList. 

```yaml
parameters:
- name: myString  # Define a parameter named 'myString'
  type: string  # The parameter type is string
  default: a string  # Default value is 'a string'

- name: myMultiString  # Define a parameter named 'myMultiString'
  type: string  # The parameter type is string
  default: default  # Default value is 'default'
  values:  # Allowed values for 'myMultiString'
  - default  
  - ubuntu  

- name: myNumber  # Define a parameter named 'myNumber'
  type: number  # The parameter type is number
  default: 2  # Default value is 2
  values:  # Allowed values for 'myNumber'
  - 1  
  - 2  
  - 4  
  - 8  
  - 16  

- name: myBoolean  # Define a parameter named 'myBoolean'
  type: boolean  # The parameter type is boolean
  default: true  # Default value is true

- name: myObject  # Define a parameter named 'myObject'
  type: object  # The parameter type is object
  default:  # Default value is an object with nested properties
    foo: FOO  # Property 'foo' with value 'FOO'
    bar: BAR  # Property 'bar' with value 'BAR'
    things:  # Property 'things' is a list
    - one  
    - two  
    - three  
    nested:  # Property 'nested' is an object
      one: apple  # Property 'one' with value 'apple'
      two: pear  # Property 'two' with value 'pear'
      count: 3  # Property 'count' with value 3

- name: myStep  # Define a parameter named 'myStep'
  type: step  # The parameter type is step
  default:  # Default value is a step
    script: echo my step 

- name: mySteplist  # Define a parameter named 'mySteplist'
  type: stepList  # The parameter type is stepList
  default:  # Default value is a list of steps
    - script: echo step one  
    - script: echo step two  

trigger: none  

jobs: 
- job: stepList  # Define a job named 'stepList'
  steps: ${{ parameters.mySteplist }}  # Use the steps from the 'mySteplist' parameter

- job: myStep  # Define a job named 'myStep'
  steps:
    - ${{ parameters.myStep }}  # Use the step from the 'myStep' parameter
```