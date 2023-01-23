# Monitoring

## Frontend monitoring 
This section suggests an approach for using Elastic's Real User Monitoring (RUM) in order to capture user interactions with our client-side application CoMPAS-OpenSCD. The following instructions assume you/your organization already count on a hosted Elasticsearch Service deployment or an Elastic Cloud organization account with Kibana as the frontend of your monitoring stack and a URL to access it. Also it is assumed that you deploy `compas-open-scd` by using Kubernetes (and [the compas-open-scd docker public docker image](https://hub.docker.com/r/lfenergy/compas-open-scd) or your own generated image). 

### 1. Getting your serverUrl
Navigate to your Kibana URL and select the space that you would like to associate with your frontend app (default if you don't have several spaces in Kibana). Then open the drawer menu and navigate to `APM`:

![Navigation instructions to APM in Kibana](/docs/public/kibana-screenshot-1.png)

In the top right corner of this page click on `Add data`:

![Navigation instructions to the Add data button in APM](/docs/public/kibana-screenshot-2.png)

Then scroll down to APM Agents and select `RUM (JS)`:

![Navigation instructions to the RUM Agent](/docs/public/kibana-screenshot-3.png)

There are two suggested code blocks for setting up the RUM Agent, find in any of them the `serverUrl` param and copy/paste it somewhere in your notes, we will use this URL later in a posterior step.

*Note: You have two options for getting your init script, you either install the `@elastic/apm-rum` dependency in your project or you set up the agent with `<script>` tags. In this document we will describe an approach for the latter.*

### 2. Using the /public/init-js script

The __compas-open-scd__ project features a reference to an "empty" javascript resource at `index.html` (line:42)
```html
  <script src="./public/init-js/init.js"></script>
```

This init javascript file has the purpose to allow for dynamic configuration of each compas-open-scd deployment. 

In a private repository create a copy of `init.js` (or edit if you already have your own copy of the file with custom init scripts) in a folder, let's call it `path-to-init-js` and add the following code:

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

### 3. Cloning private git repo using Kubernetes initContainers and Kubernetes Secrets
Start by generating a personal acccess token in Github and make sure you authorize your token to access your private repo (configure SSO if needed), your token must have checked the write:packages scope checkbox.

Then you must create your Kubernetes Secret:
```sh
kubectl create secret generic test-secret --from-literal=username='foo' --from-literal=password='ghp_12345yourgithubtoken'
```

Modify your YAML deployment template to include an initContainers section:
```yaml
  containers:
    (...)
    volumeMounts:
      - mountPath: /app/public/init-js
        name: "data-volume"
        subPath: init-js
  (...)
  initContainers:
    - name: "init-clone-repo"
      image: alpine/git
      imagePullPolicy: Always
      args:
        - clone
        - --single-branch
        - --branch
        - --
        - 'https://$(GIT_USERNAME):$(GIT_PASSWORD)@gitlab.company.com>/path/to/repo.git'
        - '/path-to-init-js/'
      env:
        - name: GIT_USERNAME
          valueFrom:
            secretKeyRef:
              key: username
              name: test-secret
        - name: GIT_PASSWORD
          valueFrom:
            secretKeyRef:
              key: password
              name: test-secret
      volumeMounts:
        - mountPath: /path-to-init-js
          name: "data-volume"
  volumes:
    - name: "data-volume"
      emptyDir: {}
```

Now apply changes to deployment with the new persistent volume init:

```sh
kubectl apply -f your-yaml-template.yaml
```

Your pod should be up now with an initContainer that clones your private repo and copies the contents of `/path-to-init-js/` to compas-open-scd's `/app/public/init-js/`.

### 4. Web server configuration
// TODO: Asking Pascal to help me describe the problem with outgoing POST requests from Elastic RUM Agent in compas-open-scd and how to approach the solution for it. 

### 5. References

[Full documentation about APM Real User Monitoring JavaScript Agent](https://www.elastic.co/guide/en/apm/agent/rum-js/5.x/intro.html)

[Full APM Guide](https://www.elastic.co/guide/en/apm/guide/8.6/apm-quick-start.html)

[@stefvnf's medium blog post about cloning git repos using Kubernetes initContainers and Secrets](https://stefvnf.medium.com/cloning-git-repos-using-kubernetes-initcontainers-and-secrets-8609e3b2d238)