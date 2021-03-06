# caddy-s3-proxy

caddy-s3-proxy allows you to proxy requests directly from S3.

S3 does have the website option, in which case, a normal reverse proxy could be used to display S3 data.
However, it is sometimes inconvient to do that.  This module lets you access S3 data even if website access
is not configured on your bucket.

## Making a version of caddy with this plugin

With caddy 2 you can use [xcaddy](https://github.com/caddyserver/xcaddy) to build a version of caddy
with this plugin installed.  To install xcaddy do:
```
go get -u github.com/caddyserver/xcaddy/cmd/xcaddy
```

This repo has a Makefile to make it easier to build a new version of caddy with this plugin.  Just type:
```
make build
```

You can run ```make docker``` do build a local image you can test with.

## Configuration
The Caddyfile directive would look something like this:
```
	s3proxy [<matcher>] {
		bucket <bucket_name>
		region <region_name>
		index  <list of index file names>
                endpoint <alternative S3 endpoint>
		root   <key prefix>
	}
```

|  option   |  type  |  required | default | help |
|-----------|:------:|-----------|---------|------|
| bucket              | string   | yes |                          | S3 bucket name |
| region              | string   | no  |  env AWS_REGION          | S3 region - if not give in the Caddyfile then AWS_REGION env var must be set.|
| endpoint            | string   | no  |  aws default             | S3 hostname |
| index               | string[] | no  |  [index.html, index.txt] | Index files to look up for dir path |
| root                | string   | no  |    | Set a "prefix" to be added to key |

## Credentials

This module uses the default providor chain to get credentials for access to S3.  This provides several more
secure options to provide credentials for accessing S3 without putting the credentials in the Caddyfile.
The methods include (and are looked for in this order):

1) Environment variables.  I.e. AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY

2) Shared credentials file.  (Located at ~/.aws/credentials)

3) If your application uses an ECS task definition or RunTask API operation, IAM role for tasks.

4) If your application is running on an Amazon EC2 instance, IAM role for Amazon EC2.

For much more detail on the various options for setting AWS credentials see here:
https://docs.aws.amazon.com/sdk-for-go/v1/developer-guide/configuring-sdk.html

## Examples you can play with

In the examples directory is an example of using the s3proxy with localstack.
Localstack contains a working version of S3 you can use for local development.

Check out the examples [here](example/LOCALSTACK_EXAMPLE.md).
