## configure git

## install cli Katacoda
`$ npm install katacoda-cli --global`

## create senarion
```
 $ katacoda scenarios:create
 $ git add .
 $ git commit -m "New scenario"
 $ git push origin master
````

## VS code extention
[to Marketplace](https://marketplace.visualstudio.com/items?itemName=Katacoda.vscode)

## Webhooks
[Guide](https://www.katacoda.community/author-profile.html#github) Usually instaled automatically while creating katacoda-account

## Commands
`echo "Run in Terminal"`{{execute}} --> runs command

`ls test-scenario*` --> checks scenario created

`[on Markdown extensions](https://kataco)`--> links as on github

`*test-username* ` --> cursive

`**Friendly URL:** ` ---> bold

## Links
[Layouts](https://katacoda.com/scenario-examples/courses/uilayouts)


[Upploading files to scenario](https://katacoda.com/scenario-examples/scenarios/upload-assets)

The options are:

1) file: The name of the file within the Assets directory.

2) target: The directory where the file should be uploaded to.

3) chmod: Optional, automatically set a chmod flag after uploading the file.

Example from *json*

```
"details": {
    "assets": {
        "host01": [
            {"file": "wait.sh", "target": "/usr/local/bin/", "chmod": "+x"},
            {"file": "deploy.sh", "target": "/usr/local/bin/", "chmod": "+x"}
        ]
    }
},
```

[Adding progress spinner](https://katacoda.com/scenario-examples/scenarios/displaying-progress-spinner)

Imported as [asset](https://github.com/katacoda/scenario-examples/blob/master/displaying-progress-spinner/assets/wait.sh) example [code](https://github.com/katacoda/scenario-examples/tree/master/displaying-progress-spinner)

[Running command in background/foreground](https://katacoda.com/scenario-examples/scenarios/run-commands-automatically)

 index.json file contains the scenario structure. Within the intro block and for each step, two files can be defined under *courseData* and *code*
 
 *courseData* defines a script which runs in the background.

 *code* defines the commands to run in the foreground.
 
For intro 
```
 "intro": {
    "text": "intro.md",
    "courseData": "background.sh",
    "code": "foreground.sh",
    "credits": ""
} 
```
 
 For each step  
 ```
 {
    "title": "Step 2",
    "courseData": "step2-background.sh",
    "code": "step2-foreground.sh",
    "text": "step2.md"
} 
```
**When need to wait**
Background:
```
#!/bin/bash
echo "This is a background script with a long running process"
sleep 10
echo "done" >> /opt/.backgroundfinished
```
 Foreground:
```
echo "Waiting to complete"; while [ ! -f /opt/.backgroundfinished ] ; do sleep 2; done; echo "Done"
```
 


[First scenario](https://katacoda.com/scenario-examples/scenarios/create-scenario-101)

## Index.json example
```
{
  "title": "Creating Your First Katacoda Scenario",
  "description": "Learn how to create your first Katacoda scenario",
  "details": {
    "steps": [
      {
        "title": "Step 1 - Scenario Structure",
        "text": "step1.md"
      },
      {
        "title": "Step 2 - Scenario Syntax",
        "text": "step2.md"
      },
      {
        "title": "Step 3 - CLI",
        "text": "step3.md"
      },
      {
        "title": "Step 4 - VS Code Extension",
        "text": "step4.md"
      },
      {
        "title": "Step 5 - WebHooks",
        "text": "step5.md"
      }
    ],
    "intro": {
      "text": "intro.md"
    },
    "finish": {
      "text": "finish.md"
    }
  },
  "environment": {
    "uilayout": "editor-terminal"
  },
  "backend": {
    "imageid": "nodejs:12"
  }
}
```


