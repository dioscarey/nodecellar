## ism-cli

install:

    npm i -g @a3labs/injectsharedmodules-cli

ism.yaml file must be in the root of the project.

`yml 
    projectPath: ./
    sharedModulesPath: _shared_modules/
    functionsPath: ./
`

then run

    ims-cli init

## Creating a shared module

Your shared module package needs to have the next script:
`json 
    ...,
    "scripts": {
    	...,
    	"postinstall":  "ism-cli --internal --target=${PWD##*/};",
    	...,
    },
    ....,
`

If your shared module depends on another shared module, add the "sharedModulesDependencies" param in your package.json.
Example:

      ...
        sharedModulesDependencies: ["my-shared-module-name", "another-sm"],
      ...

## Install your Shared Module in the Cloud Function

All Cloud Functions need to have the next script in the package.json file

    ...
    "scripts": {
    	....
    	"preinstall":  "if [[ ${PWD} =~ hm-cloud-functions ]]; then ism-cli --target=${PWD##*/}; fi",
    	....
    },
    ....

## Shared Modules dependencies in your Cloud Function

To add your shared modules in the project, you must have the param "sharedModulesDependencies" as an array in the cloud function package.

 Example:

    ...
    sharedModulesDependencies: ["my-shared-module-name", "another-sm"],
    ...

## How it works

Cloud function  
  
When ism-cli starts the process in your Cloud Function, it creates a folder with the name  of _shared_module, then, it compress all the shared modules dependencies as a tarball file and install them into the _shared_module path.

Note: ism-cli uses `npm pack`to compress a shared module.

If a Shared Module dependes on another Shared module, it creates a _INTERNAL_DEPENDENCIES folder into the _shared_module path.

Finally, it updates the package dependencies with the paths to the shared modules tarballs generated previously.

Shared Module
