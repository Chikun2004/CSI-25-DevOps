
#  Using Pipeline Variables in Azure DevOps Pipelines

This guide walks through the step-by-step use of pipeline variables in Azure DevOps pipelines using YAML syntax.

---

## 1.  What Are Pipeline Variables?

Pipeline variables are key-value pairs used to:
- Pass data between steps/stages
- Configure behavior conditionally
- Avoid hardcoding secrets/config values

---

## 2.  Define Variables in YAML

### 2.1 Static Variable
```yaml
variables:
  buildConfiguration: 'Release'
  environmentName: 'Dev'
```

### 2.2 Multi-line Variables
```yaml
variables:
  longText: |
    Line 1
    Line 2
    Line 3
```

---

## 3.  Use Variables in Scripts

```yaml
steps:
- script: echo "Building in $(buildConfiguration) mode for $(environmentName)"
```

Or with template expression:
```yaml
- script: echo "Environment: ${{ variables.environmentName }}"
```

---

## 4.  Set Variables at Runtime

Use `##vso[task.setvariable]` logging command:
```yaml
- script: |
    echo "##vso[task.setvariable variable=dynamicVar]SomeValue"
- script: echo "Dynamic: $(dynamicVar)"
```

---

## 5.  Variable Groups (Centralized Management)

### 5.1 Create in Azure DevOps UI:
- Pipelines > Library > +Variable Group > Add variables

### 5.2 Link to YAML Pipeline:
```yaml
variables:
- group: MyVariableGroup
```

---

## 6.  Secrets & Secure Variables

- Secret variables are encrypted and can't be echoed in logs.
- Define in UI or pipeline settings.

```yaml
variables:
  - name: mySecret
    value: $(mySecret)
```

Use:
```yaml
- script: echo "$(mySecret)"
  displayName: 'Use secret'
```
>  Secrets won't show in logs even if echoed.

---

## 7.  Environment-Specific Configuration

```yaml
parameters:
  - name: environment
    default: 'dev'

variables:
  - name: apiUrl
    ${{ if eq(parameters.environment, 'dev') }}:
      value: 'https://dev.example.com'
    ${{ if eq(parameters.environment, 'prod') }}:
      value: 'https://prod.example.com'

steps:
- script: echo "Deploying to $(apiUrl)"
```

---

## 8.  Looping with Variables (Arrays)

```yaml
variables:
  targets: "frontend backend api"

steps:
- ${{ each target in variables.targets.split(' ') }}:
  - script: echo "Building ${{ target }}"
```

---

## 9.  Output Variables Between Jobs

```yaml
jobs:
- job: JobA
  steps:
    - script: echo "##vso[task.setvariable variable=outVar;isOutput=true]fromJobA"
      name: setVarStep

- job: JobB
  dependsOn: JobA
  variables:
    outVarFromA: $[ dependencies.JobA.outputs['setVarStep.outVar'] ]
  steps:
    - script: echo "Output variable: $(outVarFromA)"
```

---

## 10.  Best Practices

| Practice                         | Recommendation                                     |
|----------------------------------|----------------------------------------------------|
| Centralize config                | Use variable groups for shared values             |
| Keep secrets secure              | Use secret variables via UI or key vault          |
| Dynamic pipelines                | Use runtime variables + conditions                |
| Stage/job communication          | Use output variables                              |
| Avoid repetition                 | Use templates with parameters and variables       |

---

##  Summary

 Use `variables:` block to define static or dynamic variables  
 Access them via `$(varName)` or `${{ variables.varName }}`  
 Use variable groups for shared configurations  
 Use secret variables for sensitive data  
 Use runtime/output variables for dynamic workflows

---
