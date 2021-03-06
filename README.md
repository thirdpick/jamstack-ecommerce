God:~/environment/jamstack-ecommerce (master) $ git add .
God:~/environment/jamstack-ecommerce (master) $ git remote add origin https://github.com/thirdpick/jamstack-ecommerce.git
God:~/environment/jamstack-ecommerce (master) $ git push -u origin master
error: src refspec master does not match any.
error: failed to push some refs to 'https://github.com/thirdpick/jamstack-ecommerce.git'
God:~/environment/jamstack-ecommerce (master) $ git push origin master                                                                                                 
error: src refspec master does not match any.
error: failed to push some refs to 'https://github.com/thirdpick/jamstack-ecommerce.git'
God:~/environment/jamstack-ecommerce (master) $ git pull -f origin master
remote: Enumerating objects: 105, done.
remote: Total 105 (delta 0), reused 0 (delta 0), pack-reused 105
Receiving objects: 100% (105/105), 13.10 MiB | 36.56 MiB/s, done.
Resolving deltas: 100% (7/7), done.
From https://github.com/thirdpick/jamstack-ecommerce
 * branch            master     -> FETCH_HEAD
 * [new branch]      master     -> origin/master
git push origin master
Username for 'https://github.com/thirdpick/jamstack-ecommerce.git': thirdpick
Password for 'https://thirdpick@github.com/thirdpick/jamstack-ecommerce.git': 
Everything up-to-date

God:~/environment/jamstack-ecommerce (master) $ git add .
God:~/environment/jamstack-ecommerce (master) $ git commit -m "secondary"
[master d4735bc] secondary
 Committer: Ubuntu <ubuntu@ip-172-31-44-37.ec2.internal>
Your name and email address were configured automatically based
on your username and hostname. Please check that they are accurate.
You can suppress this message by setting them explicitly:

    git config --global user.name "Your Name"
    git config --global user.email you@example.com

After doing this, you may fix the identity used for this commit with:

    git commit --amend --reset-author

 1 file changed, 22 insertions(+)
God:~/environment/jamstack-ecommerce (master) $ git push origin master
Username for 'https://github.com/thirdpick/jamstack-ecommerce.git': thirdpick
Password for 'https://thirdpick@github.com/thirdpick/jamstack-ecommerce.git': 
Counting objects: 3, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 746 bytes | 746.00 KiB/s, done.
Total 3 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To https://github.com/thirdpick/jamstack-ecommerce.git
   268c29a..d4735bc  master -> master
God:~/environment/jamstack-ecommerce (master) $ 


npm install
gatsby develop

npm install -g @aws-amplify/cli
amplify
amplify configure
amplify init

God:~/environment/jamstack-ecommerce (master) $ amplify init
Note: It is recommended to run this command from the root of your app directory
? Enter a name for the project ecorestoreweb
? Enter a name for the environment dev
? Choose your default editor: Visual Studio Code
? Choose the type of app that you're building javascript
Please tell us about your project
? What javascript framework are you using react
? Source Directory Path:  src
? Distribution Directory Path: build
? Build Command:  npm run-script build
? Start Command: npm run-script start
Using default provider  awscloudformation
For more information on AWS Profiles, see:
https://docs.aws.amazon.com/cli/latest/userguide/cli-multiple-profiles.html

? Do you want to use an AWS profile? (Y/n) 

CREATE_COMPLETE AuthRole   AWS::IAM::Role Thu Aug 06 2020 02:00:21 GMT+0000 (Coordinated Universal Time) 
⠇ Initializing project in the cloud...

CREATE_COMPLETE DeploymentBucket                AWS::S3::Bucket            Thu Aug 06 2020 02:00:29 GMT+0000 (Coordinated Universal Time) 
CREATE_COMPLETE amplify-ecorestoreweb-dev-15958 AWS::CloudFormation::Stack Thu Aug 06 2020 02:00:32 GMT+0000 (Coordinated Universal Time) 
✔ Successfully created initial AWS cloud resources for deployments.
✔ Initialized provider successfully.
Initialized your environment successfully.

Your project has been successfully initialized and connected to the cloud!

Some next steps:
"amplify status" will show you what you've added already and if it's locally configured or deployed
"amplify add <category>" will allow you to add features like user login or a backend API
"amplify push" will build all your local backend resources and provision it in the cloud
"amplify console" to open the Amplify Console and view your project status
"amplify publish" will build all your local backend and frontend resources (if you have hosting category added) and provision it in the cloud

Pro tip:
Try "amplify add api" to create a backend API and then "amplify publish" to deploy everything









## JAMstack ECommerce Professional (Beta)

JAMstack ECommerce Professional provides a way to quickly get up and running with a fully configurable JAMstack E Commerce site.

Out of the box, the site uses completely static data coming from a provider at `providers/inventoryProvider.js`. You can update this provider to fetch data from any real API by changing the call in the `getInventory` function.

![](design.jpg)

> This project is still in Beta.

### Getting started

1. Clone the project

```sh
$ git clone https://github.com/jamstack-cms/jamstack-ecommerce.git
```

2. Install the dependencies:

```sh
$ yarn

# or

$ npm install
```

3. Run the project

```sh
$ gatsby develop

# or to build

$ gatsby build
```

## About the project

### Tailwind

This project is styled using Tailwind. To learn more how this works, check out the Tailwind documentation [here](https://tailwindcss.com/docs).

### Components

The main files, components, and images you may want to change / modify are:

__Logo__ - src/images/logo.png   
__Buttons, Nav, Header__ - src/components   
__Form components__ - src/components/formComponents   
__Context (state)__ - src/context/mainContext.js   
__Pages (admin, cart, checkout, index)__ - src/pages   
__Templates (category view, single item view, inventory views)__ - src/templates   

### How it works

As it is set up, inventory is fetched from a local hard coded array of inventory items. This can easily be configured to instead be fetched from a remote source like Shopify or another CMS or data source by changing the inventory provider.

#### Configuring inventory provider

Update __providers/inventoryProvider.js__ with your own inventory provider.

#### Download images at build time

If you change the provider to fetch images from a remote source, you may choose to also download the images locally at build time to improve performance. Here is an example of some code that should work for this use case:

```javascript
import fs from 'fs'
import axios from 'axios'
import path from 'path'

function getImageKey(url) {
  const split = url.split('/')
  const key = split[split.length - 1]
  const keyItems = key.split('?')
  const imageKey = keyItems[0]
  return imageKey
}

function getPathName(url, pathName = 'downloads') {
  let reqPath = path.join(__dirname, '..')
  let key = getImageKey(url)
  key = key.replace(/%/g, "")
  const rawPath = `${reqPath}/public/${pathName}/${key}`
  return rawPath
}

async function downloadImage (url) {
  return new Promise(async (resolve, reject) => {
    const path = getPathName(url)
    const writer = fs.createWriteStream(path)
    const response = await axios({
      url,
      method: 'GET',
      responseType: 'stream'
    })
    response.data.pipe(writer)
    writer.on('finish', resolve)
    writer.on('error', reject)
  })
}

export default downloadImage
```

You can use this function in __gatsby-node.esm.js__, map over the inventory data after fetching and replace the image paths with a reference to the location of the downloaded images, probably would look something like this:

```javascript
await Promise.all(
  inventory.map(async (item, index) => {
    try {
      const relativeUrl = `../downloads/${item.image}`
      if (!fs.existsSync(`${__dirname}/public/downloads/${item.image}`)) {
        await downloadImage(image)
      }
      inventory[index].image = relativeUrl
    } catch (err) {
      console.log('error downloading image: ', err)
    }
  })
)
```

### Updating with Auth / Admin panel

1. Update __src/pages/admin.js__ with sign up, sign, in, sign out, and confirm sign in methods.

2. Update __src/templates/ViewInventory.js__ with methods to interact with the actual inventory API.

3. Update __src/components/formComponents/AddInventory.js__ with methods to add item to actual inventory API.

### Roadmap for V1

- Auto dropdown navigation for large number of categories
- Ability to add more / more configurable metadata to item details
- Themeing + dark mode
- Better image support out of the box
- Optional user account / profiles out of the box
- Have an idea or a request? Submit [an issue](https://github.com/jamstack-cms/jamstack-ecommerce/issues) or [a pull request](https://github.com/jamstack-cms/jamstack-ecommerce/pulls)!

### Other considerations

#### Server-side processing of payments

To see an example of how to process payments server-side with stripe, check out the [Lambda function in the snippets folder](https://github.com/jamstack-cms/jamstack-ecommerce/blob/master/snippets/lambda.js).

Also, consider verifying totals by passing in an array of IDs into the function, calculating the total on the server, then comparing the totals to check and make sure they match.