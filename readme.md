# PublishTemplate (1.5.0)
This template would be used to upload your own reusable templates from your scm repositories to artifactory.
You can read more about templates [here](https://www.jfrog.com/confluence/display/JFROG/Global+Templates).

### Compatibility
- This version is only supported with Pipelines version 1.40.x and above

### Important changes from v1.5.0 
- Introducing an optional parameter `publishTemplate.runtime`, allowing users to configure the runtime object as per their requirements. This includes customization options such as language selection and version specification.

## Features
- validates the template
- uploads the template to artifactory

## Resources
Two resources are defined in this pipeline. One is a [GitRepo](https://www.jfrog.com/confluence/display/JFROG/GitRepo) resource, which is designed to contain the repository path where your templates are present and another one is a [PropertyBag](https://jfrog.com/help/r/jfrog-pipelines-documentation/propertybag) resource which gets updated with deployed template details. This resource can optionally be used to extend the publishing flow to trigger other pipelines. 
You are expected to provide a values.yml containing the repository path of your templates.
[See documentation on defining resources](https://www.jfrog.com/confluence/display/JFROG/Pipelines+Resources)

### values.yml
```
templateRepository:
  # scm integration name
  gitProvider: myGitIntegration

  # repository path where your templates are present
  path: someOrg/someRepo

  # branch name or pattern where your templates are present in the repository (optional)
  branch: master

  # template folder location, where your templateDefinition.yml file is present
  templateFolder: templates/templateName
 
  # when set to true and branch is also defined, then auto triggers the publish pipeline upon commit. (optional)
  autoPublishOnTemplateChange: true
  
publishTemplate:
  # Template namespace
  namespace: jfrog_name_space

  # Template name
  name: myPipelineTemplate

  # Template version
  version: 0.0.1

  # name of the pipeline (optional)
  pipelineName: PublishTemplate

  # template repository resource name (optional)
  templateRepositoryResourceName: templateRepository

  # upload status resource name (optional)
  templateUploadStatusResourceName: templateUploadStatus

  # JFrog platform access token integration name
  jfrogAccessTokenIntegration: myJfrogAccessToken
  
  # Flag to control the version override behavior, if set to false then run will fail, if detects the template is already present. 
  allowOverride: true
  
  # Flag to control the runtime language and versions (optional)
  runtime:
    type: image
    image:
        auto:
            language: 'node'
            versions:
                - "18"
```

## Steps
This pipeline contains only one step, a [Bash](https://www.jfrog.com/confluence/display/JFROG/Bash) step, which validates the input template and uploads to artifactory.
