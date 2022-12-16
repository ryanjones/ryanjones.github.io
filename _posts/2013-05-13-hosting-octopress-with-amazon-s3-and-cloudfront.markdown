---
title: "Hosting Octopress with Amazon S3 and Cloudfront"
date: 2013-05-13 20:29
categories: [AWS]
tags: ['Amazon S3', 'Amazon Cloudfront']
---

I recently set up this blog and I figured as my first "real" post I could go through the steps it took to set this up.
I gleaned quite a bit of information from quite a few sites, but I still felt there could be some improvements on this topic. I've made sure to include all of my references in the bottom of the post.

#### Creating the site 

This assumes you have a version of ruby loaded. More information on the setup can be found here: [Octopress setup](http://octopress.org/docs/setup/). Run this through your console:

```bash
git clone git://github.com/imathis/octopress.git octopress
cd octopress
bundle exec rake install
bundle exec rake new_post["first post"]

# and start up the server
bundle exec rake preview
```

You should see the blog up and running if you punch in http://localhost:4000 in your browser:

![][1]

 [1]: /images/posts/first_post.png

#### Setting up the S3 bucket (as a static site)

Amazon is able to host static sites directly through the S3 buckets they provide. You're going to want to add a bucket with the name of
'**www.yourdomain.com**'. Then click on the properties, enable website static hosting, and set the index document to 'index.html'.

![][2]

 [2]: /images/posts/EC2_settings.png

#### Setting up s3cmd

I'm going to use s3cmd to upload our site to S3, this will allow us to push the site up through a rake task (and do some heavy lifting for us). I would normally
suggest installing s3cmd through brew, but in this case we need the ability to invalidate our cloudfront cache, and the brew version doesn't seem to support that flag currently. 
You can install s3cmd directly through source by running this:

```bash
git clone https://github.com/s3tools/s3cmd s3cmd
cd s3cmd
sudo python setup.py install
s3cmd --configure
```

You'll have to enter your S3 API keys when prompted (your Access Key ID and Secret Access Key), you can find them here: 

![][3]

 [3]: /images/posts/amazon_credentials_location.png
 
It should end up writing a file in ~/.s3cfg which contains your amazon credentials. This will allow you to use s3cmd from the deploy rake task that we'll build.

#### Deploy to Amazon S3 static site

There's a great rake task located [here](http://www.jerome-bernard.com/blog/2011/08/20/quick-tip-for-easily-deploying-octopress-blog-on-amazon-cloudfront/) that we'll leverage.
Paste this at the bottom of your Rakefile:

```ruby
desc "Deploy website via s3cmd with CloudFront cache invalidation"
task :s3 do
  puts "## Deploying website via s3cmd"
  ok_failed system("s3cmd sync --acl-public --reduced-redundancy --cf-invalidate public/* s3://#{s3_bucket}/")
end
```

Within the Rakefile you'll need to setup a few variables:

```ruby
# find deploy_default = "rsync" and replace it with 
deploy_default = "s3" 

# and then add this line underneath it
s3_bucket = "www.yourdomain.com"
```

This will set our default deploy method to the s3 task that we added to the bottom, and define the bucket that the rake task needs.

Let's deploy!

```bash
bundle exec rake generate
bundle exec rake deploy
```

All of your files should be pushed up to your Amazon S3 bucket and you should be able to visit the endpoint that was defined on your bucket.

![www.ryanonrails.com.s3-website-us-east-1.amazonaws.com would be my endpoint][2]

#### Configuring Cloudfront

We want this site to be very fast, by pushing out our static items to CloudFront we can gaurantee a faster load time than our regular S3 bucket.
I saw improvements anywhere from .5-1s speed increases (considering the site takes 2s to load on the regular S3 bucket, it's quite a large percentage). It can speed up your site from 25-50%.

Let's head back over to Amazon CloudFront and create a distribution. Click **Create Distribution**, Choose **Download** for the next step. Now here comes the tricky part.
In the Domain Name box, make sure to enter your **Endpoint URL** (IE: www.ryanonrails.com.s3-website-us-east-1.amazonaws.com). The box will auto complete to your bucket, but not the actual custom origin that you have setup with your static site. 
In the Alternate Domain Names(CNAMEs) box, enter in 'www.yourdomain.com'. You can leave everything else as default. 

Here's what it should look like once you've created your distribution:

![][4]

 [4]: /images/posts/cloudfront_dashboard.png

And if you select it and click edit, the General and Origins tab:

![][5]
 [5]: /images/posts/cloudfront_general.png

![][6]
 [6]: /images/posts/cloudfront_origins.png

We add the CNAME so when a request comes in for www.ryanonrails.com it can match the url's up properly. This will make more sense when we setup our DNS.

It will take about 10-15 minutes to distribute across the globe. Once it's finished you should be able to visit your site through your **CloudFront Domain Name** (not to be mistaken with 'www.yourdomain.com'). 
IE: http://d231akc98dz4lc.cloudfront.net/index.html (which will show you the index page of my blog) or http://d231akc98dz4lc.cloudfront.net/blog/archives/ (my archive).

#### Updating your DNS settings

We'll need to change up our DNS settings to make sure that www.yourdomain.com now points to the CloudFront Domain Name. You'll need to set your A Record to 174.129.25.170
and create a www CNAME that points to your Cloudfront Domain Name. Here's my setup on GoDaddy:

![][7]
 [7]: /images/posts/dns_settings.png

By pointing our A record to 174.129.25.170 it actually leverages a service called WWWizer which takes our domain 'yourdomain.com' and redirects it to 'www.yourdomain.com'. 
The reason we have to do this is because you can't actually point an A record at a URL like you can a CNAME, but we need a way that if you hit 'yourdomain.com' it takes you to the www version of the site.
More information on this [here](http://stackoverflow.com/questions/8312162/static-hosting-on-amazon-s3-dns-configuration).

At this point if someone requests www.ryanonrails.com/index.html it sends it to clou

#### Cloudfront cache invalidation

By default in our rake task we pass in ```--cf-invalidate``` to s3cmd. This will invalidate any of the files that are out of date on our blog.
This is useful since we generally want to see our blog post up and live once we post. You can monitor the invalidation job in the Invalidations tab in CloudFront:

![Example invalidation][8]

 [8]: /images/posts/invalidation_cf.png

Once it finishes you should be able to reload 'wwww.yourdomain.com' and see your latest blog post (or typo fix ;)).

#### Potential problems

I ran into a few problems while setting this all up. The first was that I named my bucket 'ryanonrails.com' without the www., 
this caused problems once I implemented the WWWizer service and it ended up redirecting to a bucket that didn't exist ('www.ryanonrails.com' at the time).
Another problem was that I set a Default Root Object on my CloudFront distribution. 
This broke any sub directories (such as 'www.ryanonrails.com/blog/archives'). From what I understand, this would be used on an S3 bucket that wasn't set up as a static site.


References:  

-  http://www.snikt.net/blog/2012/11/03/moving-octopress-to-amazon-s3-and-cloudfront/ (general information on the setup, speedtest chart)
-  http://stackoverflow.com/questions/15309113/amazon-cloudfront-doesnt-respect-my-s3-website-buckets-index-html-rules (specify custom origin in cloudfront)
-  http://stackoverflow.com/questions/1268158/force-cloudfront-distribution-file-update (CloudFront invalidation)
-  http://populationjim.com/2011/02/21/install-and-setup-s3cmd-on-a-mac/ (build s3cmd on a mac)