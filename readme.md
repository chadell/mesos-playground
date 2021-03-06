# Mesos-Marathon Playground

Simple API to interact with Marathon/Mesos and deploy docker containers using docker-compose

## Run it
Run the demo script to deploy the Mesos and API environment.

```bash
$ ./script/demo
```

## How to interact with the API
A basic authentication method is in place requesting the token 12345678, so authentication is mnadatory before interacting with the API

```bash
curl -X GET localhost:5000
{
    "message": "The server could not verify that you are authorized to access the URL requested.  You either supplied the wrong credentials (e.g. a bad password), or your browser doesn't understand how to supply the credentials required."
}
```

```bash
curl -X POST  -H "Content-Type: application/json" localhost:5000/auth/12345678 -i
HTTP/1.0 204 NO CONTENT
Content-Type: application/json
Content-Length: 14
Set-Cookie: session=.eJwdjj1vwjAQQP9KdTNDvrpEYqAyjRjuLCPH1t2CVJqSGHsJoKRB_PdG3d_Te084_YzdrYf6Pj66DZyGb6if8PYFNUj4GHjhX_KmpHSMoi7vHNpSq32Gyy5jjyUmLjnsFq14poKSblyv1bUSazIqsJJwyLWVRE2bs-ccbR9JnScMJhP1GSVhyWllGnflZCq0ZhJ_7NHuC_QuUjhnksyEvp0puEF7l0j1Uezat5eZrKzeYQuvDTxu3fj_Dzm8_gDEUUZe.DT0Wfw.qGRZnvne8ssQ9IsPni6tItcEtWk; HttpOnly; Path=/
Server: Werkzeug/0.14.1 Python/2.7.14
Date: Sun, 14 Jan 2018 16:27:43 GMT
```

Use this cookie from now on:
```bash
COOKIE="session=.eJwdjj1vwjAQQP9KdTNDvrpEYqAyjRjuLCPH1t2CVJqSGHsJoKRB_PdG3d_Te084_YzdrYf6Pj66DZyGb6if8PYFNUj4GHjhX_KmpHSMoi7vHNpSq32Gyy5jjyUmLjnsFq14poKSblyv1bUSazIqsJJwyLWVRE2bs-ccbR9JnScMJhP1GSVhyWllGnflZCq0ZhJ_7NHuC_QuUjhnksyEvp0puEF7l0j1Uezat5eZrKzeYQuvDTxu3fj_Dzm8_gDEUUZe.DT0Wfw.qGRZnvne8ssQ9IsPni6tItcEtWk"
```

```bash
$ curl -X GET http://localhost:5000 --cookie $COOKIE
{
    "Delete a service": "DELETE /services/<service_name>",
    "Deploy a service": "POST /services",
    "Get service/instance info": "GET /services/<service_name>/<instance_id>",
    "Kill an instance": "DELETE /services/<service_name>/<instance_id>",
    "List of services": "GET /services",
    "List service instances": "GET /services/<service_name>",
    "Update a service": "PUT /services/<service_name>?action=<scaleup, scaledown, restart>"
}

$ curl -X GET http://localhost:5000/services --cookie $COOKIE
[]

$ curl -X POST -H "Accept: application/json" -H "Content-Type: application/json" localhost:5000/services -d '{"service": "dumb-app"}' --cookie $COOKIE
"MarathonApp::{'user': None, 'tasks': [], 'labels': {}, 'networks': None, 'cpus': 0.2, 'instances': 1, 'readiness_checks': [], 'disk': 0, 'id': u'/dumb-app', 'tasks_running': 0, 'container': MarathonContainer::{\"docker\": {\"forcePullImage\": false, \"image\": \"dumb-app\", \"parameters\": [], \"portMappings\": [], \"privileged\": false}, \"type\": \"DOCKER\", \"volumes\": []}, 'readiness_check_results': [], 'max_launch_delay_seconds': 3600, 'tasks_staged': 0, 'deployments': [MarathonDeployment::84b251a7-8a79-4234-b6ab-3a9bd04ed2d9], 'version': u'2018-01-13T07:57:01.197Z', 'env': {}, 'upgrade_strategy': MarathonUpgradeStrategy::{\"maximumOverCapacity\": 1, \"minimumHealthCapacity\": 1}, 'uris': [], 'kill_selection': None, 'port_definitions': [PortDefinition::{\"labels\": {}, \"name\": null, \"port\": 0, \"protocol\": \"tcp\"}], 'version_info': None, 'mem': 32, 'task_kill_grace_period_seconds': None, 'args': None, 'backoff_factor': 1.15, 'task_stats': None, 'accepted_resource_roles': None, 'backoff_seconds': 1, 'health_checks': [], 'tasks_healthy': 0, 'tasks_unhealthy': 0, 'gpus': 0, 'task_rate_limit': None, 'secrets': {}, 'fetch': [], 'cmd': None, 'require_ports': False, 'dependencies': [], 'residency': None, 'last_task_failure': None, 'executor': u'', 'store_urls': [], 'ports': [0], 'unreachable_strategy': None, 'constraints': []}"

$ curl -X GET http://localhost:5000/services --cookie $COOKIE
[
    "MarathonApp::{'user': None, 'tasks': [], 'labels': {}, 'networks': None, 'cpus': 0.2, 'instances': 1, 'readiness_checks': [], 'disk': 0, 'id': u'/dumb-app', 'tasks_running': 1, 'container': MarathonContainer::{\"docker\": {\"forcePullImage\": false, \"image\": \"dumb-app\", \"parameters\": [], \"portMappings\": [], \"privileged\": false}, \"type\": \"DOCKER\", \"volumes\": []}, 'readiness_check_results': [], 'max_launch_delay_seconds': 3600, 'tasks_staged': 0, 'deployments': [], 'version': u'2018-01-13T07:57:01.197Z', 'env': {}, 'upgrade_strategy': MarathonUpgradeStrategy::{\"maximumOverCapacity\": 1, \"minimumHealthCapacity\": 1}, 'uris': [], 'kill_selection': None, 'port_definitions': [PortDefinition::{\"labels\": {}, \"name\": null, \"port\": 10000, \"protocol\": \"tcp\"}], 'version_info': MarathonAppVersionInfo::{\"lastConfigChangeAt\": \"2018-01-13T07:57:01.197000Z\", \"lastScalingAt\": \"2018-01-13T07:57:01.197000Z\"}, 'mem': 32, 'task_kill_grace_period_seconds': None, 'args': None, 'backoff_factor': 1.15, 'task_stats': None, 'accepted_resource_roles': None, 'backoff_seconds': 1, 'health_checks': [], 'tasks_healthy': 0, 'tasks_unhealthy': 0, 'gpus': 0, 'task_rate_limit': None, 'secrets': {}, 'fetch': [], 'cmd': None, 'require_ports': False, 'dependencies': [], 'residency': None, 'last_task_failure': None, 'executor': u'', 'store_urls': [], 'ports': [10000], 'unreachable_strategy': None, 'constraints': []}"
]

$ curl -X GET http://localhost:5000/services/dumb-app --cookie $COOKIE
"[MarathonTask::dumb-app.56eece03-f837-11e7-abbc-0242ac110005]"

$ curl -X GET http://localhost:5000/services/dumb-app/dumb-app.56eece03-f837-11e7-abbc-0242ac110005 --cookie $COOKIE
"MarathonTask::{'started_at': datetime.datetime(2018, 1, 13, 7, 57, 3, 989000), 'staged_at': datetime.datetime(2018, 1, 13, 7, 57, 2, 465000), 'state': u'TASK_RUNNING', 'ip_addresses': [MarathonIpAddress::{\"ipAddress\": \"172.17.0.4\", \"protocol\": \"IPv4\"}], 'app_id': u'/dumb-app', 'id': u'dumb-app.56eece03-f837-11e7-abbc-0242ac110005', 'local_volumes': [], 'service_ports': [], 'host': u'master', 'version': u'2018-01-13T07:57:01.197Z', 'health_check_results': [], 'slave_id': u'd0a7cba6-cf2c-407c-a130-c8a3211c337c-S0', 'ports': [11650]}"

$ curl -X PUT http://localhost:5000/services/dumb-app\?action\=scaleup --cookie $COOKIE
{
    "deploymentId": "d31baf05-892a-413c-b900-5e21e11929be",
    "version": "2018-01-13T07:59:11.855Z"
}

$ curl -X GET http://localhost:5000/services/dumb-app --cookie $COOKIE
"[MarathonTask::dumb-app.56eece03-f837-11e7-abbc-0242ac110005, MarathonTask::dumb-app.a452f814-f837-11e7-abbc-0242ac110005]"

$ curl -X PUT http://localhost:5000/services/dumb-app\?action\=scaledown --cookie $COOKIE
{
    "deploymentId": "2d84f4b8-0604-461e-98f5-f8be63372f6e",
    "version": "2018-01-13T07:59:48.744Z"
}

$ curl -X GET http://localhost:5000/services/dumb-app --cookie $COOKIE
"[MarathonTask::dumb-app.56eece03-f837-11e7-abbc-0242ac110005]"

$ curl -X DELETE http://localhost:5000/services/dumb-app --cookie $COOKIE
"{u'deploymentId': u'85eb2add-bca9-4197-b3a0-531ddb2a8c97', u'version': u'2018-01-13T08:00:54.475Z'}"

$ curl -X GET http://localhost:5000/services/dumb-app --cookie $COOKIE
"Marathon API error: MarathonHttpError: HTTP 404 returned with message, \"App '/dumb-app' does not exist\""
```
