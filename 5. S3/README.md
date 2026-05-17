# Practical 5: S3 static website and Python upload

## Goal

Create an S3 bucket, host a static one-page website, and upload files using a Python script.

## Prerequisites

- AWS account
- AWS CLI installed on Windows
- Python 3.x installed
- Internet access

## Part 1: Create S3 bucket (AWS Console)

1. Open AWS Console -> S3 -> Create bucket.
2. Bucket name: globally unique (example: my-static-website-bucket123).
3. Uncheck Block all public access.
4. Acknowledge the warning and create the bucket.

## Part 2: Enable static website hosting

1. Open the bucket -> Properties.
2. Scroll to Static website hosting -> Edit -> Enable.
3. Index document: index.html.
4. Save changes.

## Part 3: Add bucket policy (public read)

Open Permissions -> Bucket policy and paste:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": ["s3:GetObject"],
      "Resource": ["arn:aws:s3:::my-static-website-bucket123/*"]
    }
  ]
}
```

Replace the bucket name in Resource.

## Part 4: Create the static page

Create index.html:

```
<!DOCTYPE html>
<html>
  <head>
    <title>My First S3 Website</title>
  </head>
  <body>
    <h1>Welcome to My Static Website</h1>
    <p>This website is hosted on AWS S3.</p>
  </body>
</html>
```

## Part 5: Upload manually

1. Open the bucket -> Upload.
2. Add index.html and upload.

## Part 6: Access the website

- In S3 -> Properties -> Static website hosting, copy the Endpoint URL.
- Open the URL in a browser.

Done

## Notes

- Do not hardcode AWS keys in code. Use aws configure or environment variables.
- Delete the bucket when done if you no longer need it.
