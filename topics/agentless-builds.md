[//]: # (title: Agentless Builds)
[//]: # (auxiliary-id: Agentless Builds)

In terms of 2020.2 EAP, TeamCity introduces _agentless builds_ – builds that can run virtually, without an [agent](build-agent.md).

Under normal conditions, a build agent is required to run a build from start to finish. Now, your build can send an instruction to "release" the currently occupied agent. This agent becomes available to other builds, and the current build finishes on its own.  
This case is optimal for configurations that use third-party tools for finalizing a build: for example, to deploy a project. In previous versions of TeamCity, an agent would be waiting while the external tool performs its tasks which might take a lot of time. Since this version, if there are no build operations that require an agent, the TeamCity server can handle the final stage of the build.

## Detaching build agent

To release its current build agent, the build needs to send the `##teamcity[buildDetachedFromAgent]` [service message](service-messages.md). We highly recommend detaching the agent only before the final step of the build and only if this step does not require any software installed on the agent.

After being released, the agent becomes available to other builds. The server will detect and log all remaining build status information received from the third-party tool.

## Logging build data

Agentless builds can receive logs from external tools via REST API.

To perform requests, you need to use [build-level authentication](artifact-dependencies.md#Build-level+authentication) credentials:
* `%system.teamcity.auth.userId%`
* `%system.teamcity.auth.password%`

You also need to specify the [build ID](working-with-build-results.md#Internal+Build+ID) and server URL:
* `teamcity.build.id`
* `teamcity.serverUrl`

Use the following call to send the [service message](#Detaching+build+agent) as `<message>`:

```shell script
POST /app/rest/runningBuilds/id:<build_id>/log 
(curl -v --basic --user <username>:<password> --request POST http://<teamcity.url>/app/rest/runningBuilds/id:<build_id>/log --data <message> --header "Content-Type: text/plain")
```

Use the following call to send the log information:

```shell script
PUT /app/rest/runningBuilds/id:<build_id>/finishDate
(curl -v --basic --user <username>:<password> --request PUT http://<teamcity.url>/app/rest/runningBuilds/id:<build_id>/finishDate --data '' --header "Content-Type: text/plain")
```

In `--data ''`, you can send the current timestamp in the `yyyyMMdd'T'HHmmssZ` format.