# Issue description:

This is sample code to support a issue report to SAM project https://github.com/awslabs/aws-sam-cli/pull/1180


## Scenario:
- template.yaml and package.json at the root directory of the node aplicaiton.
- More than 1 lambda function declared at template.yaml.

## Issue:

Altough `sam build` completes sucessfully

```bash
lucasam$ sam build
Building resource 'FunctionOne'
Running NodejsNpmBuilder:NpmPack
Running NodejsNpmBuilder:CopyNpmrc
Running NodejsNpmBuilder:CopySource
Running NodejsNpmBuilder:NpmInstall
Running NodejsNpmBuilder:CleanUpNpmrc
Building resource 'FunctionTwo'
Running NodejsNpmBuilder:NpmPack
Running NodejsNpmBuilder:CopyNpmrc
Running NodejsNpmBuilder:CopySource
Running NodejsNpmBuilder:NpmInstall
Running NodejsNpmBuilder:CleanUpNpmrc

Build Succeeded

Built Artifacts  : .aws-sam/build
Built Template   : .aws-sam/build/template.yaml

Commands you can use next
=========================
[*] Invoke Function: sam local invoke
[*] Package: sam package --s3-bucket <yourbucket>
````

When you inspect generated files the output of the first `sam build` is packaged at the second function

```bash
lucasam$ cd .aws-sam/build/

lucasam$ du -hs *
 43M    FunctionOne
 86M    FunctionTwo               // SAM CODEBASE, DOUBLE THE SIZE
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