---
layout: post
title:  "Setting Up Paperclip + AWS S3 + Heroku"
date:   2017-03-12 05:18:35 +0000
---


This past week I have been working on deploying my Rails app, Venture, I ran into several issues. However, DEFINITELY the most frustrating thing was when I discovered that none of the photos uploaded with the paperclip gem were persisting! I was so confused as to how it could work perfectly in development, but fail so miserably in production. I guess I was forgetting that the application is configured differently in production than it is in development. 

![](http://i.giphy.com/zPOErRpLtHWbm.gif)

Finally, thanks to help from many, many Stack Overflow posts, I was able to figure out how to get it to work. Here’s how:

## Step One
After setting up your application on Heroku, the next step is creating an account on AWS and finding your credentials.  Just go to the AWS site (https://aws.amazon.com/) and sign up. Next, click on the dropdown with your name on it in the navigation bar and click on “My Security Credentials” from the dropdown. Then you can click on the “Access Keys” section to find the Access Key and Secret Access Key, which you will need to hold onto for the next few steps. ** Be sure not to share these keys with anyone or to ever publish them on a public repository.
**
## Step Two
The best way to keep track of your credentials within your application is to create a .env file in the root directory of your application. As I mentioned above, you don’t want anyone else to have access to your credentials, so make sure that .env is included in your .gitignore file. The .gitignore file is a place to include any files that you don’t want github to track when you add your code.

Once you have created this file, you will need to create environment variables to hold your credentials. These environment variables will be able to be accessed throughout the application through ENV[‘VARIABLE_NAME].

```
AWS_ACCESS_KEY_ID=your_access_key_id
AWS_SECRET_ACCESS_KEY=your_secret_access_key
AWS _REGION=your_region
BUCKET_NAME=your_bucket_name
```

## Step Three
Now that we have the Environment Variables set up, it’s time to put them to use! We need to tell paperclip to use them in production. Luckily, there is an easy way to do this. The configuration for production mode is located in config/environments/production.rb and this is where we can add our paperclip configurations. This is as easy as adding this code to the configure block:

```
config.paperclip_defaults = {
	storage: :s3,
	s3_region: ENV["AWS_REGION"],
	s3_credentials: {
		bucket: ENV['BUCKET_NAME'],
		access_key_id: ENV['AWS_ACCESS_KEY_ID'],
		secret_access_key: ENV['AWS_SECRET_ACCESS_KEY']
	}
}
```

This is telling paperclip that we are connecting it to s3 and providing it with your credentials and the information about the s3 bucket that you want your files to be stored.

## Step Four

Commit and push your code to Github and then to Heroku so that our paperclip configuration is actually used. Push code to Heroku after Github using
`$ git push heroku master`

Now that our code is pushed, we need to tell Heroku about our credentials. We can do this by running the following commands in the terminal:

```
$ heroku config:set AWS_ACCESS_KEY_ID=your_access_key_id
$ heroku config:set AWS_SECRET_ACCESS_KEY=your_secret_access_key
$ heroku config:set AWS _REGION=your_region
$ heroku config:set BUCKET_NAME=your_bucket_name
```

You may need to restart Terminal after this step.


## Step Five

Run `heroku open` and try adding an image to your application. Hopefully it works and you can give yourself a pat on the back! If everything doesn’t work as expected, I would recommend checking the heroku logs to see what went wrong. You can do this by running `heroku logs --tail` in the terminal.

![](http://i.giphy.com/O1ibc4PRxKwrS.gif)


