---
layout: post
title:  "Deploying an Ember-CLI App"
image: "../../../../../../../images/deploy-ember.jpeg"
date:   2015-04-15 20:51:32
categories: ember
---

One of the big wins for using Ember-CLI is that you can deploy your frontend totally independent of your backend. This allows you to make a
quick style change and re-deploy your frontend, without having to do a complete deploy of the entire service. I do this for all of my Ember apps now
and currently use Amazon Web Services(AWS) Simple Storage Service (S3) for the hosting.

AWS can be daunting when you first start trying to learn it. There are a lot of services and on the welcome screen you're greeted by a bunch of buttons
and no clue what any of them do. The two we're going to be using and learning about today are S3 and Identity and Access Management (IAM). S3 allows
you to store data, or host a static site which is what we will be doing today. IAM lets you define users and assign policies (permissions) to that user,
that way you don't end up with a user that has complete access to every service. Amazon actually recommends that you never use the root user in anything, always
make sure to have an IAM user set up.

## Create and Configure a Bucket

First thing we have to do is create a bucket to host our static site in. Something to keep in mind is that bucket names are global across all AWS accounts.
Another thing that can trip you up is that if you are routing your static site's DNS through Route 53, it is import that your bucket name match what your
domain is going to be. So if you plan to host the site at mygreatblog.com the bucket name must be 'mygreatblog.com'.

To set up the bucket for hosting, click the `Properties` button in the top right, then click `Static Website Hosting` in the accordian menu below.
This opens another menu with 3 options, you'll want to click the middle option, `Enable website hosting`. Enter `index.html` inside the `Index Document` input
box and click save.

Next click the accordian one above, `Permissions`, and there will be a button near the bottom called `Add bucket policy`. We're going to add a policy that basically
opens up permissions for anyone to `get` the contents of the bucket. This way anyone can visit your site.
Here is an example of what you want to enter (substituting 'mygreatblog.com' for your bucket name):

{% highlight ruby %}
{
  "Version": "2008-10-17",
  "Statement": [
    {
      "Sid": "PublicReadForGetBucketObjects",
      "Effect": "Allow",
      "Principal": {
        "AWS": "*"
      },
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::mygreatblog.com/*"
    }
  ]
}
{% endhighlight %}

## Upload Site

After getting your bucket configured, click on your bucket name to enter it. You should see `The bucket 'mygreatblog.com' is empty`. Great, we're about to fix that.
Click the `Actions` dropdown and then select `Upload`. From here you'll want to drag the entire contents of your Ember app's dist/ folder.
After the files finish uploading you'll be good to go. Click the `Properties` button again and navigate to the `Static Website Hosting` menu, there should be a link
that looks something like `mygreatblog.com.s3-website-us-east-1.amazonaws.com`, this is the link to your static site and what you'll want to Alias your DNS to.

## Setting Up an Automatic Deploy

Congratulations! You have a static site hosted on S3. Next is the step to setting up an automatic deploy so you don't have to do the drag and drop everytime you want to redeploy your site.

First you need to set up a user and attach a policy with the proper access rights to your bucket.
From the 'dashboard' select Identity and Access Management then in the left navigation click `Users`. In the top left there will be a blue button `Create New Users`, click that and enter the
username. From there you'll be redirected to a screen with credentials, make sure you take note of these or download them. You will use these to access the bucket from the API.
Click close and then select `Policies` from the left navigation, `Get Started` and then `Create Policy` from the top left. Enter the policy name, and then you can use the example policy
I have below:
{% highlight ruby %}
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Stmt1428973152000",
            "Effect": "Allow",
            "Action": [
                "s3:*"
            ],
            "Resource": [
                "arn:aws:s3:::mygreatblog.com"
            ]
        }
    ]
}
{% endhighlight %}

Navigate back to the user list and select the user you just made. Near the bottom of the screen select `Attach Policy`, then in the top left change the filter to be `Customer Managed Policies`.
Select the policy we just created and then you're done. Using the credentials you received when you created the user you can now upload using the API.

There are numerous ways to automatically upload to S3. For Ember-CLI apps specifically there is [ember-cli-deploy](https://github.com/ember-cli/ember-cli-deploy), if Java is your
preference you can use [s3\_website](https://github.com/laurilehmijoki/s3_website).

I also have a Python package I wrote to deploy sites to S3, [S3D](https://github.com/TayHobbs/S3D).

Now you have all the tools and knowledge needed to deploy a static site to Amazon S3, so go and show off your great site.
