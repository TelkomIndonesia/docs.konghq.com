---
name: AWS Lambda
publisher: Kong Inc.
version: 3.5.x

desc: Invoke and manage AWS Lambda functions from Kong
description: |
  Invoke an [AWS Lambda](https://aws.amazon.com/lambda/) function from Kong. It
  can be used in combination with other request plugins to secure, manage, or
  extend the function.

  <div class="alert alert-warning">
    <strong>Note:</strong> The functionality of this plugin as bundled
    with versions of Kong prior to 0.14.0 and Kong Enterprise prior to 0.34
    differs from what is documented herein. Refer to the
    <a href="https://github.com/Kong/kong/blob/master/CHANGELOG.md">CHANGELOG</a>
    for details.
  </div>

type: plugin
categories:
  - serverless

kong_version_compatibility:
    community_edition:
      compatible:
        - 2.2.x
        - 2.1.x
        - 2.0.x
        - 1.5.x
        - 1.4.x
        - 1.3.x
        - 1.2.x
        - 1.1.x
        - 1.0.x
        - 0.14.x
        - 0.13.x
        - 0.12.x
        - 0.11.x
        - 0.10.x
    enterprise_edition:
      compatible:
        - 2.2.x      
        - 2.1.x
        - 1.5.x
        - 1.3-x
        - 0.36-x
        - 0.35-x
        - 0.34-x
        - 0.33-x
        - 0.32-x
        - 0.31-x

params:

  name: aws-lambda
  service_id: true
  route_id: true
  consumer_id: true
  protocols: ["http", "https"]
  dbless_compatible: yes
  config:
    - name: aws_key
      required: semi
      value_in_examples: AWS_KEY
      urlencode_in_examples: true
      default:
      description: The AWS key credential to be used when invoking the function. This value is required if `aws_secret` is defined. If `aws_key` and `aws_secret` are not set, the plugin uses an IAM role inherited from the instance running Kong to authenticate.
    - name: aws_secret
      required: semi
      value_in_examples: AWS_SECRET
      urlencode_in_examples: true
      default:
      description: The AWS secret credential to be used when invoking the function. This value is required if `aws_key` is defined. If `aws_key` and `aws_secret` are not set, the plugin uses an IAM role inherited from the instance running Kong to authenticate.
    - name: aws_region
      required: semi
      default:
      value_in_examples: AWS_REGION
      description: |
        The AWS region where the Lambda function is located. The plugin does not
        attempt to validate the supplied region name; if an invalid region name
        is provided, the plugin will respond with an HTTP `500 Internal Server Error`
        at run-time and log a DNS resolution failure. Either `aws_region` or `host`
        must be provided.
    - name: host
      required: semi
      default:
      value_in_examples:
      description: |
        The host where the Lambda function is located. This value can point to a
        local Lambda server, allowing for easier debugging. Either `host` or
        `aws_region` must be provided.
    - name: function_name
      required: true
      default:
      value_in_examples: LAMBDA_FUNCTION_NAME
      description: The AWS Lambda function name to invoke.
    - name: qualifier
      required: false
      default:
      description: |
        The [`Qualifier`](http://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html#API_Invoke_RequestSyntax) to use when invoking the function.
    - name: invocation_type
      required: false
      default: "`RequestResponse`"
      description: |
        The [`InvocationType`](http://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html#API_Invoke_RequestSyntax) to use when invoking the function. Available types are `RequestResponse`, `Event`, `DryRun`.
    - name: log_type
      required: false
      default: "`Tail`"
      description: |
        The [`LogType`](http://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html#API_Invoke_RequestSyntax) to use when invoking the function. By default, `None` and `Tail` are supported.
    - name: timeout
      required: true
      default: "`60000`"
      description: An optional timeout in milliseconds when invoking the function.
    - name: port
      required: false
      default: "`443`"
      description: |
        The TCP port that this plugin will use to connect to the server.
    - name: keepalive
      required: true
      default: "`60000`"
      description: |
        An optional value in milliseconds that defines how long an idle connection will live before being closed.
    - name: unhandled_status
      required: false
      default: "`200`, `202`, or `204`"
      description: |
        The response status code to use (instead of the default `200`, `202`, or `204`) in the case of an [`Unhandled` Function Error](https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html#API_Invoke_ResponseSyntax).
    - name: forward_request_body
      required: false
      default: "`false`"
      description: |
        An optional value that defines whether the request body is to be sent in the `request_body` field of the JSON-encoded request. If the body arguments can be parsed, they will be sent in the separate `request_body_args` field of the request. The body arguments can be parsed for `application/json`, `application/x-www-form-urlencoded`, and `multipart/form-data` content types.
    - name: forward_request_headers
      required: false
      default: "`false`"
      description: |
        An optional value that defines whether the original HTTP request headers are to be sent as a map in the `request_headers` field of the JSON-encoded request.
    - name: forward_request_method
      required: false
      default: "`false`"
      description: |
        An optional value that defines whether the original HTTP request method verb is to be sent in the `request_method` field of the JSON-encoded request.
    - name: forward_request_uri
      required: false
      default: "`false`"
      description: |
        An optional value that defines whether the original HTTP request URI is to be sent in the `request_uri` field of the JSON-encoded request. Request URI arguments (if any) will be sent in the separate `request_uri_args` field of the JSON body.
    - name: is_proxy_integration
      required: false
      default: "`false`"
      description: |
        An optional value that defines whether the response format to receive from the Lambda to [this format](https://docs.aws.amazon.com/apigateway/latest/developerguide/set-up-lambda-proxy-integrations.html#api-gateway-simple-proxy-for-lambda-output-format).
    - name: awsgateway_compatible
      required: false
      default: "`false`"
      description: |
        An optional value that defines whether the plugin should wrap requests into the Amazon API gateway.
    - name: proxy_url
      required: semi
      default:
      description: |
        An optional value that defines whether the plugin should connect through the given proxy server URL. This value is required if `proxy_scheme` is defined.
    - name: proxy_scheme
      required: semi
      default:
      description: |
        An optional value that defines which HTTP scheme to use for connecting through the proxy server. The schemes supported are: `http` and `https`. This value is required if `proxy_url` is defined.
    - name: skip_large_bodies
      required: false
      default: "`true`"
      description: |
        An optional value that defines whether Kong should send large bodies that are buffered to disk. To define the threshold for the body size, use the [client_body_buffer_size](https://docs.konghq.com/latest/configuration/#client_body_buffer_size) property. Note that sending large bodies will have an impact on system memory.

  extra: |
    **Reminder**: By default, cURL sends payloads with an
    `application/x-www-form-urlencoded` MIME type, which will naturally be URL-
    decoded by Kong. To ensure special characters that are likely to appear in
    your AWS key or secret (like `+`) are correctly decoded, you must
    URL-encode them with `--data-urlencode`.
    Alternatives to this approach would be to send your payload with a
    different MIME type (like `application/json`), or to use a different HTTP client.

---

### Sending parameters

Any form parameter sent along with the request will be also sent as an
argument to the AWS Lambda function.

---
### Notes

If you do not provide an `aws_key` and `aws_secret`, the plugin uses an IAM role inherited from the instance running Kong.

First, the plugin will try ECS metadata to get the role. If no ECS metadata is available, the plugin will fall back on EC2 metadata.

---
### Step-By-Step Guide

Prerequisite: You must have access to the AWS Console as a user who is
allowed to operate with lambda functions, and create users and roles.

1. First, create an execution role called `LambdaExecutor` for your
lambda function.

    In the IAM Console, create a new Role choosing the AWS Lambda service. There
    will be no policies because the function in this example will simply execute
    itself, returning a hardcoded JSON response without accessing other
    AWS resources.

2. Create a user named `KongInvoker`, used by the Kong API gateway
to invoke the function.

    In the IAM Console, create a new user. Programmatic access must be provided to the user via Access and Secret keys. Then, attach existing policies directly, particularly the predefined `AWSLambdaRole`. After the user creation is confirmed, store the Access Key and Secret Key in a safe place.

3. Next, create the lambda function itself in the N. Virginia Region
(code `us-east-1`).

    In Lambda Management, create a new function `MyLambda`. There will be no blueprint because you are going to paste the code below (which is an example code snippet). For the execution role, choose the `LambdaExecutor` created previously.

    **Note**: The following code snippet is only an example. The Kong AWS Lambda plugin supports all runtimes provided by AWS. See the list of runtimes in the **AWS Lambda** > **Functions** > **Create function** dialog.

    ```python
    import json
    def lambda_handler(event, context):
        """
          If is_proxy_integration is set to true :
          jsonbody='''{"statusCode": 200, "body": {"response": "yes"}}'''
        """
        jsonbody='''{"response": "yes"}'''
        return json.loads(jsonbody)
    ```

    Test the lambda function from the AWS console and make sure the execution succeeds.

4. Set up a Route in Kong and link it to the `MyLambda` function you just created.

{% navtabs %}
{% navtab With a database %}

Create the Route:

```bash
curl -i -X POST http://{kong_hostname}:8001/routes \
--data 'name=lambda1' \
--data 'paths[1]=/lambda1'
```

Add the plugin:

```bash
curl -i -X POST http://{kong_hostname}:8001/routes/lambda1/plugins \
--data 'name=aws-lambda' \
--data-urlencode 'config.aws_key={KongInvoker user key}' \
--data-urlencode 'config.aws_secret={KongInvoker user secret}' \
--data 'config.aws_region=us-east-1' \
--data 'config.function_name=MyLambda'
```

{% endnavtab %}
{% navtab Without a database %}

Add a Route and Plugin to the declarative config file:

``` yaml
routes:
- name: lambda1
  paths: [ "/lambda1" ]

plugins:
- route: lambda1
  name: aws-lambda
  config:
    aws_key: {KongInvoker user key}
    aws_secret: {KongInvoker user secret}
    aws_region: us-east-1
    function_name: MyLambda
```

{% endnavtab %}
{% endnavtabs %}

#### Test your Lambda with Kong

After everything is created, make the http request and verify the correct
invocation, execution, and response:

```bash
curl http://{kong_hostname}:8000/lambda1
```

Additional headers:

```
x-amzn-Remapped-Content-Length, X-Amzn-Trace-Id, x-amzn-RequestId
```

JSON response:

```json
{"response": "yes"}
```

Have fun leveraging the power of AWS Lambda in Kong!
