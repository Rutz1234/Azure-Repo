# single-html-repo

Deployed this site using github actions deployment method available in azure static web apps.
As there is no option to directly upload html files or related code in web app, I first did it using storage account manually uploading html file and code in it.

But then I thought this will be hard task if I have to upload every single code configuation file like .html, .css .js etc in it. So tried deploying it in static web app using deployment method as github. In this if choose configuration github as deployment details and deployment authorization policy it will be automatically creating workflow that is yaml file (Need to deep dive into this more, surface level knowledge is it takes code from github and uses it to create resource or modify it in azure). 

But even so consitent error kept coming first this Error: Could not find either 'build' or 'build:azure' node under 'scripts' in package.json. Could not find value for custom run build command using the environment variable key 'RUN_BUILD_COMMAND'. and if I updated workflow code then invalid api error. Due to second error I tried changing the api token in code itself as in first time error it was copy pasted code so api token was old one i replaced it with new one still no progess.

Then I tried changing code of workflow sometimes and manually put value of deployment token taking it from azure and changing it in github secrets for that particular repo still no progress.

Then next day I started anew, whole new code , alongwith .css and .js files and did every step critically, I put all these files in a folder first in repo and then deployed. So the configuration code files were in folder in the repo i.e. 

my-static-file/
└── index.html

like that.
It got deployed I was able to see the live site. Surprised i saw workflow file as i don't know much about it I didnt get much so I tried to deploy using just html file and not .css and .js. As it was single file I put it in root folder of repo no other folder.
It didnt deploy, then I put it under a folder and then it got deployed.

What I learned from this is so much:
1. Github based deployment creates yaml file and workflow file on its on for static web app. Although Im thinking of a way where we will be the one making it.
2. On surface level I got to know about api keys, secrets and how authentication happens between github and azure for that web app to go live . Ill dive deeper into this more to understand more.
3. In github repo we've section called envirionemnt and secrets where we can create our own too putting value of deployment token in it if we dont want one made by azure.
4. We need to put code files in  a folder for it to be configured otherwise Azure Static Web Apps sometimes auto-detects it as a Node.js app (because there’s no folder structure to indicate it’s a static site)

It then expected a package.json with a build or build:azure script → caused the Oryx build error.

When you moved it into a folder (e.g., my-static-file) and set App location = ./my-static-file, Azure now knows:

“Ah, there’s a folder with static files — no Node.js build needed.”

It deploys your index.html directly → no errors.

<img width="1061" height="263" alt="image" src="https://github.com/user-attachments/assets/21f54a02-d161-4394-9cde-02e4d377ccb2" />

<img width="958" height="264" alt="image" src="https://github.com/user-attachments/assets/6ba387ca-6aa3-4fa4-91bd-bcc7c852b2e6" />

What I'll be doing next:
1. Learn about yaml
2. Learn about api tokens
3. Try this with custom workflows rather than automated from azure.
4. Also try to incorporate terraform in this, and in deployment of web app.
5. Also try to incorporate python to deploy web app.
6. Go for security part of this service. Like what security concerns might be there in that resource.
