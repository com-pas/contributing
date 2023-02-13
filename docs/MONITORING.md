# Monitoring
## Table of contents
* [Frontend monitoring](#introduction)
  1. [Getting your APM Server URL](#1-getting-your-apm-server-url)
  2. [Using the /public/init-js script](#2-using-the-publicinit-js-script)
  3. [References](#3-references)


## Frontend monitoring<a name="introduction"></a>
This section suggests an approach for using Elastic's Real User Monitoring (RUM) in order to capture user interactions with our client-side application CoMPAS-OpenSCD. The following instructions assume you/your organization already count on a hosted Elasticsearch Service deployment or an Elastic Cloud organization account with Kibana as the frontend of your monitoring stack and a URL to access it. Also it is assumed that you deploy `compas-open-scd` by using Kubernetes (and [the compas-open-scd docker public docker image](https://hub.docker.com/r/lfenergy/compas-open-scd) or your own generated image). 

### 1. Getting your APM Server URL<a name="server-url"></a>
Navigate to your Kibana URL and select the space that you would like to associate with your frontend app (default if you don't have several spaces in Kibana). Then open the drawer menu and navigate to `APM`:

![Navigation instructions to APM in Kibana](/docs/public/kibana-screenshot-1.png)

In the top right corner of this page click on `Add data`:

![Navigation instructions to the Add data button in APM](/docs/public/kibana-screenshot-2.png)

Then scroll down to APM Agents and select `RUM (JS)`:

![Navigation instructions to the RUM Agent](/docs/public/kibana-screenshot-3.png)

There are two suggested code blocks for setting up the RUM Agent, find in any of them the `serverUrl` param and copy/paste it somewhere in your notes, we will use this URL later in a posterior step.

*Note: You have two options for getting your init script, you either install the `@elastic/apm-rum` dependency in your project or you set up the agent with `<script>` tags. In this document we will describe an approach for the latter.*

### 2. Using the /public/init-js script<a name="init-js"></a>

The __compas-open-scd__ project features a reference to an "empty" javascript resource at `index.html` (line:42)
```html
  <script src="./public/init-js/init.js"></script>
```

This init javascript file has the purpose to allow for dynamic configuration of each compas-open-scd deployment. 

Make sure to include in your `init.js` file the following code:

```js
const script = document.createElement('script');
script.type = 'text/javascript';
// download and host your preffered RUM Agent version minified from https://github.com/elastic/apm-agent-rum-js/releases
script.src = 'https://your-cdn-host.com/path/to/elastic-apm-rum.umd.min.js'; 
script.async = true;
script.crossorigin = "anonymous"; // provides support for CORS
script.onload = function () {
    elasticApm.init({
        serviceName: 'compas-open-scd', // or preferred name, this will be used to filter out results in Kibana
        serverUrl: 'https://your-own-serverUrl.with.a.specific:port', // replace with serverUrl found in Step 1
        environment: 'test', // The environment where the service being monitored is deployed, e.g. 'production', 'development', 'test', etc. Default: ''
    });
}
document.querySelector('head').appendChild(script);
```

### 3. References<a name="references"></a>

* [Full documentation about APM Real User Monitoring JavaScript Agent](https://www.elastic.co/guide/en/apm/agent/rum-js/5.x/intro.html)

* [Full APM Guide](https://www.elastic.co/guide/en/apm/guide/8.6/apm-quick-start.html)

* [@stefvnf's medium blog post about cloning git repos using Kubernetes initContainers and Secrets](https://stefvnf.medium.com/cloning-git-repos-using-kubernetes-initcontainers-and-secrets-8609e3b2d238)