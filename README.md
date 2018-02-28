# Overview

Soul is a tool to run automated tasks from Splunk alerts

# Config

Edit username and password in the configsplunk.yaml and actions.yaml

# Usage

Using a fetcher, for example email:

`python get_splunk_email`

Or send your alerts to the server and run the listener.py:

`python listener`

# Example

~~~
→ python get_splunk_email
"'Splunk Alert: Sev2: Prod Api Upstream Server'" 10.20.25.176 cluster01-an1-control.prod.cloud.io
../actions/run_actions -n 'Splunk Alert: Sev2: Prod Capcom Upstream Server' -p HOST=10.20.25.176 -p SERVER=cluster01-an1-control.prod.cloud.io
echo Restarting Api
Restarting Api
../coral/coral -s cluster01-an1-control.prod.cloud.io -t api.313e52ba-0786-11e8-adbd-a2d36eddbd49 -k
echo Finished cluster01-an1-control.prod.cloud.io
Finished cluster01-an1-control.prod.cloud.io
~~~

# Actions

Actions.yaml example:

~~~
actions:
  - name: "Restart API Gateway"
    alert: "Splunk Alert: Sev2: Prod API Upstream Server"
    # action: "echo Restarting API"
    # action: "coral -s SERVER -t APP -k"
    action:
      - name: "echo Restarting API"
        params:
          - param: "MESSAGE"
            value: "Restarting API"
      - name: "../coral/coral -s SERVER -t APP" #$(../coral/coral -s SERVER -t APP | grep HOST | awk '{ print $2 }') -k"
        params:
          - param: "SERVER"
            value: ""
          - param: "APP"
            value: "api"
          - param: "HOST"
            value: ""
      - name: "echo Finished SERVER"
        params:
          - param: "SERVER"
            value: "cluster01-an1-control.prod.cloud.io"
    status: "Pending"
    customer: "Adobe"
~~~

# Author

Alejandro Sanchez Acosta <sancheza@adobe.com>
