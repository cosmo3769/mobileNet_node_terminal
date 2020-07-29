# image classification with modelNet and node js

Integrating machine learning/deep learning with client side makes it complete so the users can see the actual thing happening with the model trained. 

As usually the model is trained in python so showing it in browser with the use of **FLASK**(a minimal web framework of python) is easy. Doing it with node js as server is a real deal.
But it has also been made easy with the emergence of **TENSORFLOWJS**. Here is an image classification mobileNet model. First I wanted to see the predictions on terminal before showing it in the browser.

## Requirements 

###### node js

Node.js is an open-source, cross-platform, JavaScript run-time environment that executes JavaScript code outside of a browser, built on Chrome's V8 JavaScript engine.

###### Tensorflowjs

TensorFlow.js, an open-source library we can use to define, train, and run machine learning models entirely in the browser, using JavaScript and a high-level layers API.

###### MobileNet model

MobileNets are small, low-latency, low-power models parameterized to meet the resource constraints of a variety of use cases.i.e., classification, segmentation, detection etc.

## install node and npm

[node](https://nodejs.org/en/)

[npm install](https://docs.npmjs.com/cli/install)

## project directory and npm init

1.Go to your local directory and make a new folder with **mobileNet_node**(You can give any name to it)

2.Open your favourite code editor(in my case, i opened my vscode).

3.Make a local folder **server**.

4.**cd server/** - In your terminal write this to get inside server folder.

5.**npm init -y** - This will create a package.json file where all the dependencies of project exists.

###### package.json

```
{
  "name": "server",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
}
```

## Installing the libraries

**npm install @tensorflow/tfjs @tensorflow/tfjs-node @tensorflow-models/mobilenet --save** - This will create node modules(where all the packages are stored) and package.lock(where all the information is given about the packages installed) in server folder

package.json created at first will also change with added libraries

###### package.json

```
{
  "name": "server",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "@tensorflow-models/mobilenet": "^2.0.4",
    "@tensorflow/tfjs": "^2.0.1",
    "@tensorflow/tfjs-node": "^2.0.1",
    "express": "^4.17.1"
  }
}
```

## creating a file and loading the libraries

1.create a file named **classify.js**.

2.start adding code in classify.js.

3.This code here is to add the libraries.

```
const tf = require('@tensorflow/tfjs');
const mobilenet = require('@tensorflow-models/mobilenet');
const tfnode = require('@tensorflow/tfjs-node');
const fs = require('fs');
```

## image reading

```
const readImage = path => {
  const imageBuffer = fs.readFileSync(path);
  const tfimage = tfnode.node.decodeImage(imageBuffer);
  return tfimage;
}
```

This code is to import the image, turn into 3D or 4D tensor and then only passing it to image classification model

**fs** - This module is to interact with the file system.

**readFileSync(path)** - It reads the entire contents of file in a synchronous way(execution not blocked until it is read)

**tfnode.node.decodeImage(imageBuffer)** - It decodes the image into 3D or 4D tensor which is necessary before passing it to the image classification model. It only supports PNG, BMP, GIF, JPEG.

## function for image classification

```
const imageClassification = async path => {
  const image = readImage(path);
  const mobilenetModel = await mobilenet.load();
  const predictions = await mobilenetModel.classify(image);
  console.log('Classification Results:', predictions);
}
```

This code is to load the model, classify the image and show it in terminal with **console.log**

**readImage(path)** - To read the image 3D or 4D tensor.

**mobilenet.load()** - To load the model.

**mobilenetModel.classify(image)** - To classify the image into top classes and their probabilities.

***async and await is used to make this function asynchronous(making it go with sequential flow).***

## calling the function

Without making a call to the function, it will not work.

```
if (process.argv.length !== 3) throw new Error('Incorrect arguments: node classify.js <IMAGE_FILE>');
imageClassification(process.argv[2]);
```

If the condition is not satisfied it will not process.

## Finally testing

1.Take the image of cat, dog or rabbit etc. to test the project.(Here I took cats.jpg)

2.**cd server/** - in treminal to get into server folder.

3.**node classify.js cats.jpg** - This will console log out the predictions made on image(here the cat image) with className and probability.

## This is the image of classification

![image_classification_model](https://github.com/piyush-cosmo/mobileNet_node/server/master/mobileNet.png?raw=true)
