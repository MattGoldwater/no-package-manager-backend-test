My first goal is to serve an HTML file that says Hello World. First I created my index.html file and I decided to use the popular package express to serve the html file.

Out of habit, I manually created a folder titled node_modules to store my packages. Then I rethought things and briefly considered some sort of system to store each library in the folder containing the files where they were used. However, I determined this system wouldn't work well if any library was used in multiple files that wouldn't otherwise be grouped together in a folder.

Additionally, I realized package managers could theoretically help establish naming conventions. In this case, [node rather than npm encourages the use of the term node_modules](https://stackoverflow.com/questions/21818701/can-a-custom-directory-name-be-used-instead-of-node-modules-when-installing-no). Consequentially, whenever I look at a new JavaScript codebase, I don't have to check if packages are in the lib folder, packages folder or whatever someone else would've decided to name the folder. 99% of the time I can find packages in the node_modules folder.

Also, without a package manager there's no way to connect to the npm package registry. As a result, I installed packages on the back-end by cloning the packages git repository into my node_modules folder. Cloning the express GitHub repo took 1 minute 8.75 seconds. Plus, express's dependencies aren't installed yet. To test an npm installation, I created an identical repo and installed express with npm. It only took 18.305 seconds.

It takes longer to download the express GitHub repo because its a much bigger folder (10.6mb) than its npm package (1.8mb including Express's dependencies) . The express repo's git history alone is almost 10mb. (I removed git tracking before committing this repo to github, but you can confirm this by downloading express from github and inspecting its .git folder. To do that on a Mac right click on the folder and select "Get Info") The express git repo also includes other files used for development such as files related to linting, test, benchmarks, continuous integration, examples and various markdown files.

Express GitHub Repo Storage Info: 

![Express GitHub directory picture](https://res.cloudinary.com/dyr8j9g6m/image/upload/v1572286292/Screen_Shot_2019-09-25_at_9.31.09_AM_b8fflk.png "Express GitHub directory picture") 

.git Folder in Express GitHub Repo: 

![picture of .git folder in Express GitHub Repo ](https://res.cloudinary.com/dyr8j9g6m/image/upload/v1572286208/Screen_Shot_2019-09-27_at_9.28.52_AM_x17fmk.png "picture of .git folder in Express GitHub Repo")

Then I added the following code to index.js in the server directory:

```
const express = require('express');
const app = express();
```

It didn't work because body-parser wasn't installed.  Now I had to decide where to install body-parser. I knew that npm would install it in the root node_modules folder because [it tries to keep the dependency tree as flat as possible](https://npm.github.io/how-npm-works-docs/npm3/how-npm3-works.html).

The other option that stood out was to install it within my express folder's node_modules sub-folder. [Npm would've done it this way in version 1 and 2](https://npm.github.io/how-npm-works-docs/npm2/how-npm2-works.html).

I also considered installing packages globally. That seems like a horrible idea because I'd affect multiple projects at the same time when changing my node_modules structure.

I'm not sure if npm's current system is the best way to manage packages, but its definitely better than not using a  package manager on the back-end. It takes too much time to manage packages yourself! Plus there are [many other benefits of package managers](https://softwareengineering.stackexchange.com/questions/372444/why-prefer-a-package-manager-over-a-library-folder).

