My goal for this project is to serve an HTML file that says Hello World without using a package manager.

First, I created my index.html file. I decided to use the popular package [express](https://expressjs.com/) to serve the html file.

Out of habit, I created a folder titled node_modules to store my packages. Then I rethought things and briefly considered storing each library in the folder containing the files where they were used. I determined this theoretical system wouldn't be straightforward. That's because I would need to determine how to handle any library used in multiple files that wouldn't otherwise be in the same folder.

Additionally, I realized package managers could theoretically help establish naming conventions. In this case, [node rather than npm encourages the use of the term node_modules](https://stackoverflow.com/questions/21818701/can-a-custom-directory-name-be-used-instead-of-node-modules-when-installing-no). Consequentially, whenever I look at a new JavaScript codebase, I don't have to check if packages are in the lib folder, packages folder or whatever someone else would've decided to name the folder. 99% of the time I can find packages in the node_modules folder.

Also, without a package manager there's no way to connect to the [npm package registry](https://docs.npmjs.com/misc/registry). As a result, I installed express by cloning its [git repository](https://github.com/expressjs/express) into my node_modules folder. Cloning the express GitHub repo took 1 minute 8.75 seconds. Plus, express's dependencies aren't installed yet. To test an npm installation, I created an identical repo and installed express with npm. It only took 18.305 seconds.

It takes longer to download the express GitHub repo because its a much bigger folder (10.6mb) than its npm package (1.8mb including Express's dependencies) . The express repo's git history alone is almost 10mb. 

Later on, I realized that I could download a package by using the [npm package registry api](https://github.com/npm/registry/blob/master/docs/REGISTRY-API.md). I was able to install the express tar file and unzip its contents in less than a second with the following command. 
```
curl https://registry.npmjs.org/express/-/express-4.17.1.tgz | tar -xz && mv package express
``` 

While this command is fast, it doesn't install a packages sub-dependencies. I could continue to try to figure out a way to automate this process, but at that point I'd basically be making my own package manager. So I'll move on for now.

Next, I added the following code to index.js in the server directory:

```javascript
const express = require('express');
const app = express();
```

I ran the index.js file in my server folder and it threw an error because express's dependency [body-parser](https://www.npmjs.com/package/body-parser) wasn't installed.  Now I had to decide where to install body-parser. My first thought was to install it in the root node_modules folder as npm would because [it tries to keep the dependency tree as flat as possible](https://npm.github.io/how-npm-works-docs/npm3/how-npm3-works.html).

The other option that stood out was to install it within my express folder's node_modules sub-folder. [Npm would've done it this way in version 1 and 2](https://npm.github.io/how-npm-works-docs/npm2/how-npm2-works.html).

I also considered installing packages globally to save disk space by only installing each package version once. I couldn't think of a simple way to do this without creating a single node_modules structure for all of my projects. That would mean I'd alter multiple projects at once when changing my node_modules structure.

Later on, as I learned about [pnpm](https://pnpm.js.org/) and [yarn pnp](https://yarnpkg.com/lang/en/docs/pnp/) it turns out modern package managers have figured out better methods to make a dependency graph that allows you to reduce installation times and save hard drive space. They're covered in [my blog post](https://medium.com/@MattGoldwater/the-evolution-of-javascript-package-managers-f9797be7cf0e).

At this point, I didn't want to take the time to install body-parser, any other dependencies needed to serve the index.html file or figure out my dependency tree. I'm convinced that its worthwhile to use a package manager on the back-end! Plus, [package managers have many other benefits](https://softwareengineering.stackexchange.com/questions/372444/why-prefer-a-package-manager-over-a-library-folder).

I also tried [creating a react project without using a package manager for front-end packages](https://github.com/MattGoldwater/no-package-manager-frontend-test). Feel free to check it out!
