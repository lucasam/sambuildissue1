# Issue description:

This is sample code to support a issue report to SAM project https://github.com/awslabs/aws-sam-cli/pull/1180


## Scenario:
- template.yaml and package.json at the root directory of the node aplicaiton.
- More than 1 lambda function declared at template.yaml.

## Issue:

Altough `sam build` completes sucessfully

```bash
lucasam$ sam build --debug
Using SAM Template at /Users/lucasam/Documents/src/sambuildissue1/.aws-sam/build/FunctionTwo/template.yaml
Telemetry endpoint configured to be https://aws-serverless-tools-telemetry.us-west-2.amazonaws.com/metrics
'build' command is called
No Parameters detected in the template
2 resources found in the template
Found Serverless function with name='FunctionOne' and CodeUri='.'
Found Serverless function with name='FunctionTwo' and CodeUri='.'
Building resource 'FunctionOne'
Loading workflow module 'aws_lambda_builders.workflows'
Registering workflow 'PythonPipBuilder' with capability 'Capability(language='python', dependency_manager='pip', application_framework=None)'
Registering workflow 'NodejsNpmBuilder' with capability 'Capability(language='nodejs', dependency_manager='npm', application_framework=None)'
Registering workflow 'RubyBundlerBuilder' with capability 'Capability(language='ruby', dependency_manager='bundler', application_framework=None)'
Registering workflow 'GoDepBuilder' with capability 'Capability(language='go', dependency_manager='dep', application_framework=None)'
Registering workflow 'GoModulesBuilder' with capability 'Capability(language='go', dependency_manager='modules', application_framework=None)'
Registering workflow 'JavaGradleWorkflow' with capability 'Capability(language='java', dependency_manager='gradle', application_framework=None)'
Registering workflow 'JavaMavenWorkflow' with capability 'Capability(language='java', dependency_manager='maven', application_framework=None)'
Registering workflow 'DotnetCliPackageBuilder' with capability 'Capability(language='dotnet', dependency_manager='cli-package', application_framework=None)'
Found workflow 'NodejsNpmBuilder' to support capabilities 'Capability(language='nodejs', dependency_manager='npm', application_framework=None)'
Running workflow 'NodejsNpmBuilder'
Running NodejsNpmBuilder:NpmPack
NODEJS packaging file:/Users/lucasam/Documents/src/sambuildissue1/.aws-sam/build/FunctionTwo to /var/folders/sj/60s2bzdn7c52vwbq64d8tr9d64r349/T/tmph7xegid2
executing NPM: ['npm', 'pack', '-q', 'file:/Users/lucasam/Documents/src/sambuildissue1/.aws-sam/build/FunctionTwo']
NODEJS packed to build-issue-0.0.1.tgz
NODEJS extracting to /var/folders/sj/60s2bzdn7c52vwbq64d8tr9d64r349/T/tmph7xegid2/unpacked
NodejsNpmBuilder:NpmPack succeeded
Running NodejsNpmBuilder:CopyNpmrc
NodejsNpmBuilder:CopyNpmrc succeeded
Running NodejsNpmBuilder:CopySource
NodejsNpmBuilder:CopySource succeeded
Running NodejsNpmBuilder:NpmInstall
NODEJS installing in: /Users/lucasam/Documents/src/sambuildissue1/.aws-sam/build/FunctionTwo/.aws-sam/build/FunctionOne
executing NPM: ['npm', 'install', '-q', '--no-audit', '--no-save', '--production']
NodejsNpmBuilder:NpmInstall succeeded
Running NodejsNpmBuilder:CleanUpNpmrc
NodejsNpmBuilder:CleanUpNpmrc succeeded
Building resource 'FunctionTwo'
Loading workflow module 'aws_lambda_builders.workflows'
Found workflow 'NodejsNpmBuilder' to support capabilities 'Capability(language='nodejs', dependency_manager='npm', application_framework=None)'
Running workflow 'NodejsNpmBuilder'
Running NodejsNpmBuilder:NpmPack
NODEJS packaging file:/Users/lucasam/Documents/src/sambuildissue1/.aws-sam/build/FunctionTwo to /var/folders/sj/60s2bzdn7c52vwbq64d8tr9d64r349/T/tmpzvnf85j5
executing NPM: ['npm', 'pack', '-q', 'file:/Users/lucasam/Documents/src/sambuildissue1/.aws-sam/build/FunctionTwo']
NODEJS packed to build-issue-0.0.1.tgz
NODEJS extracting to /var/folders/sj/60s2bzdn7c52vwbq64d8tr9d64r349/T/tmpzvnf85j5/unpacked
NodejsNpmBuilder:NpmPack succeeded
Running NodejsNpmBuilder:CopyNpmrc
NodejsNpmBuilder:CopyNpmrc succeeded
Running NodejsNpmBuilder:CopySource
NodejsNpmBuilder:CopySource succeeded
Running NodejsNpmBuilder:NpmInstall
NODEJS installing in: /Users/lucasam/Documents/src/sambuildissue1/.aws-sam/build/FunctionTwo/.aws-sam/build/FunctionTwo
executing NPM: ['npm', 'install', '-q', '--no-audit', '--no-save', '--production']
NodejsNpmBuilder:NpmInstall succeeded
Running NodejsNpmBuilder:CleanUpNpmrc
NodejsNpmBuilder:CleanUpNpmrc succeeded

Build Succeeded

Built Artifacts  : .aws-sam/build
Built Template   : .aws-sam/build/template.yaml

Commands you can use next
=========================
[*] Invoke Function: sam local invoke
[*] Package: sam package --s3-bucket <yourbucket>
    
Sending Telemetry: {'metrics': [{'commandRun': {'awsProfileProvided': False, 'debugFlagProvided': True, 'region': '', 'commandName': 'sam build', 'duration': 10815, 'exitReason': 'success', 'exitCode': 0, 'requestId': '794b5afe-5db2-4c2e-94f1-46c908e8a0ad', 'installationId': '893c5aa8-d107-4b3b-8f82-e7cb5c9ce2fe', 'sessionId': 'bb97b105-48f6-4dde-92ac-4b421ec6aca1', 'executionEnvironment': 'CLI', 'pyversion': '3.7.4', 'samcliVersion': '0.22.0'}}]}
HTTPSConnectionPool(host='aws-serverless-tools-telemetry.us-west-2.amazonaws.com', port=443): Read timed out. (read timeout=0.1)
````

When you inspect generated files the output of the first `sam build` is packaged at the second function

```bash
lucasam$ cd .aws-sam/build/

lucasam$ du -hs *
 43M    FunctionOne
 86M    FunctionTwo               // SAME CODEBASE, DOUBLE THE SIZE
4.0K    template.yaml

lucasam$ cd FunctionTwo/
lucasam$ ls -lia
total 16
34492672 drwxr-xr-x   7 lucasam  ANT\Domain Users  224 Oct 17 10:31 .
34489527 drwxr-xr-x   5 lucasam  ANT\Domain Users  160 Oct 17 10:31 ..
34492678 drwxr-xr-x   3 lucasam  ANT\Domain Users   96 Oct 17 10:31 .aws-sam     // THIS FOLDER SHOULD NOT BE HERE
34492675 drwxr-xr-x   4 lucasam  ANT\Domain Users  128 Oct 17 10:31 lib
34494271 drwxr-xr-x  17 lucasam  ANT\Domain Users  544 Oct 17 10:31 node_modules
34492674 -rw-r--r--   1 lucasam  ANT\Domain Users  291 Oct 26  1985 package.json
34492673 -rw-r--r--   1 lucasam  ANT\Domain Users  469 Oct 26  1985 template.yaml
````

Observe that:
- Function Two contains `.aws-sam` directory built during function one build.
- The size of FunctionTwo package is the double of the first one.
- Similiar behavior does not apply to java projects

Note: https://github.com/awslabs/aws-sam-cli/issues/1236 / https://github.com/awslabs/aws-sam-cli/pull/1180 might fix this this issue.