---
title: Understanding package-lock.json
author: Bhaskar
---

## What are package.json and package-lock.json

package.json and package-lock.json are versioning files which list the dependencies of our node.js project. package.json is created as soon as we initialise npm (Node Package Manager) in our project using the following command:
```
npm init
```
Suppose we have [csvtojson](https://www.npmjs.com/package/csvtojson) installed in our node.js project. In this case, our package.json will list our project dependency as follows:
```
"dependencies": {
"csvtojson": "^2.0.10"
}
```

However, each dependency can have multiple other dependencies as well. Here, the package-lock.json file lists the entire dependency tree along with their versions as follows:

```
...
"dependencies": {
"bluebird": {
"version": "3.7.2",
"resolved": "https://registry.npmjs.org/bluebird/-/bluebird-3.7.2.tgz",
"integrity": "sha512-XpNj6GDQzdfW+r2Wnn7xiSAd7TM3jzkxGXBGTtWKuSXv1xUV+azxAm8jdWZN06QTQk+2N2XB9jRDkvbmQmcRtg=="
},
"csvtojson": {
"version": "2.0.10",
"resolved": "https://registry.npmjs.org/csvtojson/-/csvtojson-2.0.10.tgz",
"integrity": "sha512-lUWFxGKyhraKCW8Qghz6Z0f2l/PqB1W3AO0HKJzGIQ5JRSlR651ekJDiGJbBT4sRNNv5ddnSGVEnsxP9XRCVpQ==",
"requires": {
"bluebird": "^3.5.1",
"lodash": "^4.17.3",
"strip-bom": "^2.0.0"
}
},
"is-utf8": {
"version": "0.2.1",
"resolved": "https://registry.npmjs.org/is-utf8/-/is-utf8-0.2.1.tgz",
"integrity": "sha512-rMYPYvCzsXywIsldgLaSoPlw5PfoB/ssr7hY4pLfcodrA5M/eArza1a9VmTiNIBNMjOGr1Ow9mTyU2o69U6U9Q=="

},
...
```
## Why is package-lock.json needed?

As we can see above, package.json follows semantic versioning and lists the version range for each dependency following semantic versioning. For Example:
```
^2.0.10 //Updates to the latest minor version
~2.0.10 //Updates to the latest patch version
```
Suppose there is no package-lock.json and someone uses our project. In that case, as soon as they enter **npm install**, they will have downloaded the latest minor version or the patch version of the project dependencies following the versioning provided in the package.json file. This may result in inconsistencies with the original project and the user may not be able to run the project. This is due to the fact that the downloaded project may have updated packages which may be incompatible with the original project.

That is why **package-lock.json** becomes important. It was introduced in NPM 5 and has become an integral part of node.js projects. The package-lock.json files contain the exact version number of all the dependencies used in the project. It essentially locks the version number of each of the project dependencies. 

If a project has a package-lock.json file, each user who uses the project will have the same versions of the project dependencies installed in their local systems as used in the original project. In this way, they will have the exact same package versions and there will be no chance of a project not working due to incompatible dependencies. 

Even if the original project uses packages which are older, the other users will have the same packages installed in their systems due to the presence of the package-lock.json file. Thus, the package-lock.json file should always be committed to a repository along with the project.

However, package-lock.json will only install the packages having versions within the range provided for in the package.json file. In case of an inconsistency between the versions specified in package.json and package-lock.json, the package.json version **will overwrite** the version mentioned for a package in the package-lock.json file. For example, if a package has the following versions specified:
```
^3.0.0 // Version in package.json file
 2.9.6 // Version in package-lock.json file
```
In this case, the installed package version will not be 2.9.6 but 3.x.y, hence, overwriting the package-lock.json file.