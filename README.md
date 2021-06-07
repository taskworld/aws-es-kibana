[![npm version](https://badge.fury.io/js/aws-es-kibana.svg)](https://badge.fury.io/js/aws-es-kibana) ![dependencies](https://david-dm.org/santthosh/aws-es-kibana.svg)
[![Docker Stars](https://img.shields.io/docker/stars/santthosh/aws-es-kibana.svg)](https://registry.hub.docker.com/v2/repositories/santthosh/aws-es-kibana/stars/count/)

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/santthosh/aws-es-kibana)

# AWS ES/Kibana Proxy

AWS ElasticSearch/Kibana Proxy to access your [AWS ES](https://aws.amazon.com/elasticsearch-service/) cluster. 

This is the solution for accessing your cluster if you have [configured access policies](http://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-createupdatedomains.html#es-createdomain-configure-access-policies) for your ES domain

## Changes in this fork

- Added integration with OpenID Connect

## Usage

Install the npm module 

    npm install -g aws-es-kibana
    
Set AWS credentials
                          
    export AWS_ACCESS_KEY_ID=XXXXXXXXXXXXXXXXXXX
    export AWS_SECRET_ACCESS_KEY=XXXXXXXXXXXXXXXXXXX

Run the proxy (do not include the `http` or `https` from your `cluster-endpoint` or the proxy won't function)

    aws-es-kibana <cluster-endpoint>

Where cluster-endpoint can be either a URL (i.e. https://search-xxxxx.us-west-2.es.amazonaws.com) or a hostname (i.e. search-xxxxx.us-west-2.es.amazonaws.com). 
Alternatively, you can set the _AWS_PROFILE_ environment variable

    AWS_PROFILE=myprofile aws-es-kibana <cluster-endpoint>
    
Example with hostname as cluster-endpoint:

![aws-es-kibana](https://raw.githubusercontent.com/santthosh/aws-es-kibana/master/aws-es-kibana.png)

### OpenID Connect

Using OpenID Connect-compatible IdPs you can allow people to access Elasticsearch with their credentials without using shared passwords. To use, set the following environment variables:

```
# See: https://github.com/auth0/express-openid-connect
ISSUER_BASE_URL=https://accounts.google.com
CLIENT_ID=1234567890-abcdefghijklmnopqrstuvwxyz1234567.apps.googleusercontent.com
BASE_URL=https://aws-ws-kibana-abcdefghij-uc.a.run.app
SECRET=eae9671c6692e1e13f4716cf7230e3f9d131299e577ed6a388c9aae08124da6e

# For basic auth
USER=user
PASSWORD=123fa9770f3c02331fab5f4af52bed568607a7e5c54b8aa9813ab3cd534ae0a1
```

If your user has an active session via OpenID connect, then Basic Auth is bypassed. To login with OpenID Connect, go to `/login`.

⚠️ **You MUST enable Basic authentication even if you do not use it.** Since OIDC support only bypasses Basic authentication, this means if Basic authentication is not set up, anyone can access your instance.

### Run within docker container

To build, use [`pack`](https://buildpacks.io/):

    pack build aws-es-kibana --builder heroku/buildpacks:20

Run the container (do not forget to pass the required environment variables)

	docker run --init --rm -ti --env-file=.env -p 9200:9200 aws-es-kibana


## Credits

Adopted from this [gist](https://gist.github.com/nakedible-p/ad95dfb1c16e75af1ad5). Thanks [@nakedible-p](https://github.com/nakedible-p)
