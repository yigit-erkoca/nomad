## 1.8.3 (August 13, 2024)

SECURITY:

* security: Fix symlink escape during unarchiving by removing existing paths within the same allocdir. Compromising the Nomad client agent at the source allocation first is a prerequisite for leveraging this issue. [[GH-23738](https://github.com/hashicorp/nomad/issues/23738)]

IMPROVEMENTS:

* acl: Submitting a policy with a leading `/` in a variable path will now return an error to prevent improperly working policies. [[GH-23757](https://github.com/hashicorp/nomad/issues/23757)]
* cli: Added option to return original HCL in `job inspect` command [[GH-23699](https://github.com/hashicorp/nomad/issues/23699)]
* cli: Added support for updating the roles for an ACL token [[GH-18532](https://github.com/hashicorp/nomad/issues/18532)]
* cli: `acl token create` will now emit a warning if the token has a policy that does not yet exist [[GH-16437](https://github.com/hashicorp/nomad/issues/16437)]
* keyring: Added support for encrypting the keyring via Vault transit or external KMS [[GH-23580](https://github.com/hashicorp/nomad/issues/23580)]
* keyring: Added support for prepublishing keys [[GH-23577](https://github.com/hashicorp/nomad/issues/23577)]
* identity: Added support for server-configured additional claims on the Vault default_identity block [[GH-23675](https://github.com/hashicorp/nomad/issues/23675)]
* metrics: Added `client.tasks` metrics to track task states [[GH-23773](https://github.com/hashicorp/nomad/issues/23773)]
* resources: Added `resources.secrets` field to configure size of secrets directory on Linux [[GH-23696](https://github.com/hashicorp/nomad/issues/23696)]
* tls: Allow setting the `tls_min_version` field to `"tls13"` [[GH-23713](https://github.com/hashicorp/nomad/issues/23713)]
* ui: added a Pack badge to the jobs index page for jobs run via Nomad Pack [[GH-23404](https://github.com/hashicorp/nomad/issues/23404)]

BUG FIXES:

* api: Fixed a bug where an `api.Config` targeting a unix domain socket could not be reused between clients [[GH-23785](https://github.com/hashicorp/nomad/issues/23785)]
* cni: .conf and .json config files are now parsed properly [[GH-23629](https://github.com/hashicorp/nomad/issues/23629)]
* cni: network.cni jobspec updates now replace allocs to apply the new network config [[GH-23764](https://github.com/hashicorp/nomad/issues/23764)]
* docker: Fixed a bug where plugin SELinux labels would conflict with read-only `volume` options [[GH-23750](https://github.com/hashicorp/nomad/issues/23750)]
* identity: Fixed a bug where a missing default task identity could panic the leader [[GH-23763](https://github.com/hashicorp/nomad/issues/23763)]
* keyring: Fixed a bug where keys could be garbage collected before workload identities expire [[GH-23577](https://github.com/hashicorp/nomad/issues/23577)]
* keyring: Fixed a bug where keys would never exit the "rekeying" state after a rotation with the `-full` flag [[GH-23577](https://github.com/hashicorp/nomad/issues/23577)]
* keyring: Fixed a bug where periodic key rotation would not occur [[GH-23577](https://github.com/hashicorp/nomad/issues/23577)]
* networking: The same static port can now be used more than once on host networks with multiple IPs [[GH-23693](https://github.com/hashicorp/nomad/issues/23693)]
* scaling: Fixed a bug where state store corruption could occur when writing scaling events [[GH-23673](https://github.com/hashicorp/nomad/issues/23673)]
* template: Fixed a bug where change_mode = "script" would not execute after a client restart [[GH-23663](https://github.com/hashicorp/nomad/issues/23663)]
* ui: Fixed storage/plugin 404s by unescaping a slash character in the request URL [[GH-23625](https://github.com/hashicorp/nomad/issues/23625)]
* windows: Fix bug with containers capabilities on Docker CE [[GH-23599](https://github.com/hashicorp/nomad/issues/23599)]

## 1.8.2 (July 16, 2024)

BREAKING CHANGES:

* docker: default to hyper-v isolation mode on Windows [[GH-23452](https://github.com/hashicorp/nomad/issues/23452)]

SECURITY:

* build: Updated Go to 1.22.5 to address CVE-2024-24791 [[GH-23498](https://github.com/hashicorp/nomad/issues/23498)]
* migration: Added a check for relative paths escaping the allocation directory when unpacking archive during migration, to harden clients against compromised peer clients sending malicious archives [[GH-23319](https://github.com/hashicorp/nomad/issues/23319)]
* security: Removed insecure TLS cipher suites: `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`, `TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA25` and `TLS_RSA_WITH_AES_128_CBC_SHA256`. [[GH-23551](https://github.com/hashicorp/nomad/issues/23551)]

IMPROVEMENTS:

* client: add a preferred_address_family config to prefer ipv4 or ipv6 when deducing IP from network interface [[GH-23389](https://github.com/hashicorp/nomad/issues/23389)]
* cni: allow users to input CNI args in job specification [[GH-23538](https://github.com/hashicorp/nomad/issues/23538)]
* deps: Updated Consul API to 1.29.1. [[GH-23436](https://github.com/hashicorp/nomad/issues/23436)]
* deps: Updated consul-template to 0.39 to allow admin partition and sameness groups queries. [[GH-23436](https://github.com/hashicorp/nomad/issues/23436)]
* docker: Validate that unprivileged containers aren't running as ContainerAdmin on Windows [[GH-23443](https://github.com/hashicorp/nomad/issues/23443)]
* namespaces: Added warnings if deleting namespaces that have existing objects associated with them [[GH-23499](https://github.com/hashicorp/nomad/issues/23499)]
* quota (Enterprise): Allow CPU cores to be configured within a quota [[GH-23543](https://github.com/hashicorp/nomad/issues/23543)]
* scaling: Added `-check-index` support to `job scale` command [[GH-23457](https://github.com/hashicorp/nomad/issues/23457)]
* ui: Allow users to create Global ACL tokens from the Administration UI [[GH-23506](https://github.com/hashicorp/nomad/issues/23506)]
* ui: Update headers in the Admin section to use the HashiCorp Design System [[GH-23366](https://github.com/hashicorp/nomad/issues/23366)]
* ui: allow for multiple namespaces in jobs index filters [[GH-23468](https://github.com/hashicorp/nomad/issues/23468)]

BUG FIXES:

* api: Fixed bug where newlines in JobSubmission vars weren't encoded correctly [[GH-23560](https://github.com/hashicorp/nomad/issues/23560)]
* cli: Fixed bug where the `plugin status` command would fail if the plugin ID was a prefix of another plugin ID [[GH-23502](https://github.com/hashicorp/nomad/issues/23502)]
* cli: Fixed bug where the `quota status` and `quota inspect` commands would fail if the quota name was a prefix of another quota name [[GH-23502](https://github.com/hashicorp/nomad/issues/23502)]
* cli: Fixed bug where the `scaling policy info` command would fail if the policy ID was a prefix of another policy ID [[GH-23502](https://github.com/hashicorp/nomad/issues/23502)]
* cli: Fixed bug where the `service info` command would fail if the service name was a prefix of another service name in the same namespace [[GH-23502](https://github.com/hashicorp/nomad/issues/23502)]
* cli: Fixed bug where the `volume deregister`, `volume detach`, and `volume status` commands would fail if the volume ID was a prefix of another volume ID in the same namespace [[GH-23502](https://github.com/hashicorp/nomad/issues/23502)]
* consul: Fixed a bug where service registration and Envoy bootstrap would not wait for Consul ACL tokens and services to be replicated to the local agent [[GH-23381](https://github.com/hashicorp/nomad/issues/23381)]
* plugins: Fix panic on systems that don't support NUMA [[GH-23399](https://github.com/hashicorp/nomad/issues/23399)]
* qemu: Fixed a bug that prevented `qemu` tasks from running on Linux [[GH-23466](https://github.com/hashicorp/nomad/issues/23466)]
* quota (Enterprise): Fixed a bug where a task's resource core count was not translated to CPU MHz and checked against its quota when performing a job plan [[GH-18876](https://github.com/hashicorp/nomad/issues/18876)]
* scheduler: Fix a bug where reserved resources are not calculated correctly [[GH-23386](https://github.com/hashicorp/nomad/issues/23386)]
* server: Fixed a bug where expiring heartbeats for garbage collected nodes could panic the server [[GH-23383](https://github.com/hashicorp/nomad/issues/23383)]
* template: Fix template rendering on Windows [[GH-23432](https://github.com/hashicorp/nomad/issues/23432)]
* ui: Actions run from jobs with explicit name properties now work from the web UI [[GH-23553](https://github.com/hashicorp/nomad/issues/23553)]
* ui: Dont show keyboard nav hints when taking a screenshot [[GH-23365](https://github.com/hashicorp/nomad/issues/23365)]
* ui: Fix an issue where a remotely purged job would prevent redirect from taking place in the web UI [[GH-23492](https://github.com/hashicorp/nomad/issues/23492)]
* ui: Fix an issue where access to Job Templates in the UI was restricted to variable.write access [[GH-23458](https://github.com/hashicorp/nomad/issues/23458)]
* ui: Fix the Upload Jobspec button on the Run Job page [[GH-23548](https://github.com/hashicorp/nomad/issues/23548)]
* ui: Fixed support for namespace parameter on job statuses API [[GH-23456](https://github.com/hashicorp/nomad/issues/23456)]
* ui: fix an issue where gateway timeouts would cause the jobs list to revert to null, gives users a Pause Fetch option [[GH-23427](https://github.com/hashicorp/nomad/issues/23427)]
* vault: Fixed a bug where requests to derive or renew tokens could be sent to the wrong namespace [[GH-23491](https://github.com/hashicorp/nomad/issues/23491)]

## 1.8.1 (June 19, 2024)

SECURITY:

* build: Updated Go to 1.22.4 to address Go stdlib vulnerabilities CVE-2024-24789 and CVE-2024-24790 [[GH-23172](https://github.com/hashicorp/nomad/issues/23172)]

IMPROVEMENTS:

* api: Add support for setting Notes field for Consul health checks [[GH-22397](https://github.com/hashicorp/nomad/issues/22397)]
* cli: `operator snapshot inspect` now includes details of data in snapshot [[GH-18372](https://github.com/hashicorp/nomad/issues/18372)]
* docker: Added container_exists_attempts plugin configuration variable [[GH-22419](https://github.com/hashicorp/nomad/issues/22419)]
* docker: Added support for oom_score_adj [[GH-23297](https://github.com/hashicorp/nomad/issues/23297)]
* exec: Fixed a bug where `exec` driver tasks would fail on older versions of glibc [[GH-23331](https://github.com/hashicorp/nomad/issues/23331)]
* metrics (Enterprise): Publish quota utilization as metrics [[GH-22912](https://github.com/hashicorp/nomad/issues/22912)]
* raw_exec: Added support for oom_score_adj [[GH-23308](https://github.com/hashicorp/nomad/issues/23308)]
* ui: adds a Stopped label for jobs that a user has manually stopped [[GH-23328](https://github.com/hashicorp/nomad/issues/23328)]
* ui: namespace dropdown gets a search field and supports many namespaces [[GH-20626](https://github.com/hashicorp/nomad/issues/20626)]
* ui: shorten client/node metadata/attributes display and make parent-terminal attributes show up [[GH-23290](https://github.com/hashicorp/nomad/issues/23290)]

BUG FIXES:

* acl: Fix plugin policy validation when checking write permissions [[GH-23274](https://github.com/hashicorp/nomad/issues/23274)]
* api: (Enterprise) fixed Allocations.GetPauseState method discarding the task argument [[GH-23377](https://github.com/hashicorp/nomad/issues/23377)]
* client: Fixed a bug where empty task directories would be left behind [[GH-23237](https://github.com/hashicorp/nomad/issues/23237)]
* connect: fix validation with multiple socket paths [[GH-22312](https://github.com/hashicorp/nomad/issues/22312)]
* consul: (Enterprise) Fixed a bug where gateway config entries were written before Sentinel policies were enforced [[GH-22228](https://github.com/hashicorp/nomad/issues/22228)]
* consul: Fixed a bug where Consul admin partition was not used to login via Consul JWT auth method [[GH-22226](https://github.com/hashicorp/nomad/issues/22226)]
* consul: Fixed a bug where gateway config entries were written to the Nomad server agent's Consul partition and not the client's partition [[GH-22228](https://github.com/hashicorp/nomad/issues/22228)]
* driver: Fixed a bug where the exec, java, and raw_exec drivers would not configure cgroups to allow access to devices provided by device plugins [[GH-22518](https://github.com/hashicorp/nomad/issues/22518)]
* scheduler: Fixed a bug where rescheduled allocations that could not be placed would later ignore their reschedule policy limits [[GH-12319](https://github.com/hashicorp/nomad/issues/12319)]
* task schedule: Fixed a bug where schedules wrongly errored as invalid on the last day of the month [[GH-23329](https://github.com/hashicorp/nomad/issues/23329)]
* ui: unbind job detail running allocations count from job-summary endpoint [[GH-23306](https://github.com/hashicorp/nomad/issues/23306)]

## 1.8.0 (May 28, 2024)

IMPROVEMENTS:

* agent: Added support for systemd readiness notifications [[GH-20528](https://github.com/hashicorp/nomad/issues/20528)]
* api: new /v1/jobs/statuses endpoint collates details about jobs' allocs and latest deployment, intended for use in the updated UI jobs index page [[GH-20130](https://github.com/hashicorp/nomad/issues/20130)]
* artifact: Added support for downloading artifacts without validating the TLS certificate [[GH-20126](https://github.com/hashicorp/nomad/issues/20126)]
* autopilot: Added `operator autopilot health` command to review Autopilot health data [[GH-20156](https://github.com/hashicorp/nomad/issues/20156)]
* cli: Add `-jwks-ca-file` argument to `setup consul/vault` commands [[GH-20518](https://github.com/hashicorp/nomad/issues/20518)]
* client/volumes: Add a mount volume level option for selinux tags on volumes [[GH-19839](https://github.com/hashicorp/nomad/issues/19839)]
* client: expose network namespace bridge/cni configuration values as task env vars [[GH-11810](https://github.com/hashicorp/nomad/issues/11810)]
* connect: Added support for `volume_mount` blocks on sidecar task overrides [[GH-20575](https://github.com/hashicorp/nomad/issues/20575)]
* consul/connect: Attempt autodetection of podman task driver for Connect gateways [[GH-20611](https://github.com/hashicorp/nomad/issues/20611)]
* consul: provide tasks that have Consul tokens the CONSUL_HTTP_TOKEN environment variable [[GH-20519](https://github.com/hashicorp/nomad/issues/20519)]
* core: Do not create evaluations within batch deregister endpoint during job garbage collection [[GH-20510](https://github.com/hashicorp/nomad/issues/20510)]
* csi: Added support for wildcard namespace to `plugin status` command [[GH-20551](https://github.com/hashicorp/nomad/issues/20551)]
* deps: Update msgpack to v2 [[GH-20173](https://github.com/hashicorp/nomad/issues/20173)]
* deps: Updated `docker` dependency to 26.0.1 [[GH-20389](https://github.com/hashicorp/nomad/issues/20389)]
* driver/rawexec: Allow specifying custom cgroups [[GH-20481](https://github.com/hashicorp/nomad/issues/20481)]
* func: Allow custom paths to be added the the getter landlock [[GH-20315](https://github.com/hashicorp/nomad/issues/20315)]
* jobspec: Add a schedule{} block for time based task execution (Enterprise) [[GH-22201](https://github.com/hashicorp/nomad/issues/22201)]
* metrics: Added tracking of enqueue and dequeue times of evaluations to the broker [[GH-20329](https://github.com/hashicorp/nomad/issues/20329)]
* networking: Inject constraints on CNI plugins when using bridge networking [[GH-15473](https://github.com/hashicorp/nomad/issues/15473)]
* scheduler: Added a new configuration to avoid rescheduling allocations if a nodes misses one or more heartbits [[GH-19101](https://github.com/hashicorp/nomad/issues/19101)]
* server: Add new options for reconcilation in case of disconnected nodes [[GH-20029](https://github.com/hashicorp/nomad/issues/20029)]
* ui: Added a UI for creating, editing and deleting Sentinel Policies [[GH-20483](https://github.com/hashicorp/nomad/issues/20483)]
* ui: Added a copy button on Action output [[GH-19496](https://github.com/hashicorp/nomad/issues/19496)]
* ui: Added a new UI block to job spec in order to provide description and links in the Web UI [[GH-18292](https://github.com/hashicorp/nomad/issues/18292)]
* ui: Added token.name information to the top nav for ease of operator debugging [[GH-20539](https://github.com/hashicorp/nomad/issues/20539)]
* ui: Improve error and warning messages for invalid variable and job template paths/names [[GH-19989](https://github.com/hashicorp/nomad/issues/19989)]
* ui: Overhaul of the Jobs Index list page, with live updates, more informative statuses, filter expressions, and pagination [[GH-20452](https://github.com/hashicorp/nomad/issues/20452)]
* ui: Prompt a user before they close an exec window to prevent accidental close-browser-tab shortcuts that overlap with terminal ones [[GH-19985](https://github.com/hashicorp/nomad/issues/19985)]
* ui: Replaced single-line variable value fields with multi-line textarea blocks [[GH-19544](https://github.com/hashicorp/nomad/issues/19544)]
* ui: Updated the style of components in the Variables web ui [[GH-19544](https://github.com/hashicorp/nomad/issues/19544)]
* ui: change the State filter on clients page to split out eligibility and drain status [[GH-18607](https://github.com/hashicorp/nomad/issues/18607)]

BUG FIXES:

* cli: Fix handling of scaling jobs which don't generate evals [[GH-20479](https://github.com/hashicorp/nomad/issues/20479)]
* client: Fix unallocated CPU metric calculation when client reserved CPU is set [[GH-20543](https://github.com/hashicorp/nomad/issues/20543)]
* client: terminate old exec task processes before starting new ones, to avoid accidentally leaving running processes in case of an error [[GH-20500](https://github.com/hashicorp/nomad/issues/20500)]
* config: Fixed a panic triggered by registering a job specifying a Vault cluster that has not been configured within the server [[GH-22227](https://github.com/hashicorp/nomad/issues/22227)]
* core: Fix multiple incorrect type conversion for potential overflows [[GH-20553](https://github.com/hashicorp/nomad/issues/20553)]
* csi: Fixed a bug where concurrent mount and unmount operations could unstage volumes needed by another allocation [[GH-20550](https://github.com/hashicorp/nomad/issues/20550)]
* csi: Fixed a bug where plugins would not be deleted on GC if their job updated the plugin ID [[GH-20555](https://github.com/hashicorp/nomad/issues/20555)]
* csi: Fixed a bug where volumes in different namespaces but the same ID would fail to stage on the same client [[GH-20532](https://github.com/hashicorp/nomad/issues/20532)]
* job endpoint: fix implicit constraint mutation for task-level services [[GH-22229](https://github.com/hashicorp/nomad/issues/22229)]
* quota (Enterprise): Fixed a bug where quota usage would not be freed if a job was purged
* services: Added retry to Nomad service deregistration RPCs during alloc stop [[GH-20596](https://github.com/hashicorp/nomad/issues/20596)]
* services: Fixed bug where Nomad services might not be deregistered when nodes are marked down or allocations are terminal [[GH-20590](https://github.com/hashicorp/nomad/issues/20590)]
* structs: Fix job canonicalization for array type fields [[GH-20522](https://github.com/hashicorp/nomad/issues/20522)]
* ui: Fix a bug where the UI would prompt a user to promote a deployment with unplaced canaries [[GH-20408](https://github.com/hashicorp/nomad/issues/20408)]
* ui: Fixed an issue where keynav would not trigger evaluation sidebar expand [[GH-20047](https://github.com/hashicorp/nomad/issues/20047)]
* ui: Show the namespace in the web UI exec command hint [[GH-20218](https://github.com/hashicorp/nomad/issues/20218)]
* windows: Fixed a regression where scanning task processes was inefficient [[GH-20619](https://github.com/hashicorp/nomad/issues/20619)]

## 1.7.11 Enterprise (August 13, 2024)

SECURITY:

* security: Fix symlink escape during unarchiving by removing existing paths within the same allocdir. Compromising the Nomad client agent at the source allocation first is a prerequisite for leveraging this issue. [[GH-23738](https://github.com/hashicorp/nomad/issues/23738)]

IMPROVEMENTS:

* keyring: Added support for prepublishing keys [[GH-23577](https://github.com/hashicorp/nomad/issues/23577)]

BUG FIXES:

* api: Fixed a bug where an `api.Config` targeting a unix domain socket could not be reused between clients [[GH-23785](https://github.com/hashicorp/nomad/issues/23785)]
* cni: .conf and .json config files are now parsed properly [[GH-23629](https://github.com/hashicorp/nomad/issues/23629)]
* docker: Fixed a bug where plugin SELinux labels would conflict with read-only `volume` options [[GH-23750](https://github.com/hashicorp/nomad/issues/23750)]
* identity: Fixed a bug where a missing default task identity could panic the leader [[GH-23763](https://github.com/hashicorp/nomad/issues/23763)]
* keyring: Fixed a bug where keys could be garbage collected before workload identities expire [[GH-23577](https://github.com/hashicorp/nomad/issues/23577)]
* keyring: Fixed a bug where keys would never exit the "rekeying" state after a rotation with the `-full` flag [[GH-23577](https://github.com/hashicorp/nomad/issues/23577)]
* keyring: Fixed a bug where periodic key rotation would not occur [[GH-23577](https://github.com/hashicorp/nomad/issues/23577)]
* networking: The same static port can now be used more than once on host networks with multiple IPs [[GH-23693](https://github.com/hashicorp/nomad/issues/23693)]
* scaling: Fixed a bug where state store corruption could occur when writing scaling events [[GH-23673](https://github.com/hashicorp/nomad/issues/23673)]
* template: Fixed a bug where change_mode = "script" would not execute after a client restart [[GH-23663](https://github.com/hashicorp/nomad/issues/23663)]
* windows: Fix bug with containers capabilities on Docker CE [[GH-23599](https://github.com/hashicorp/nomad/issues/23599)]

## 1.7.10 Enterprise (July 16, 2024)

BREAKING CHANGES:

* docker: default to hyper-v isolation mode on Windows [[GH-23452](https://github.com/hashicorp/nomad/issues/23452)]

SECURITY:

* build: Updated Go to 1.22.5 to address CVE-2024-24791 [[GH-23498](https://github.com/hashicorp/nomad/issues/23498)]
* migration: Added a check for relative paths escaping the allocation directory when unpacking archive during migration, to harden clients against compromised peer clients sending malicious archives [[GH-23319](https://github.com/hashicorp/nomad/issues/23319)]
* security: Removed insecure TLS cipher suites: `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`, `TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA25` and `TLS_RSA_WITH_AES_128_CBC_SHA256`. [[GH-23551](https://github.com/hashicorp/nomad/issues/23551)]

IMPROVEMENTS:

* deps: Updated Consul API to 1.29.1. [[GH-23436](https://github.com/hashicorp/nomad/issues/23436)]
* deps: Updated consul-template to 0.39 to allow admin partition and sameness groups queries. [[GH-23436](https://github.com/hashicorp/nomad/issues/23436)]
* docker: Validate that unprivileged containers aren't running as ContainerAdmin on Windows [[GH-23443](https://github.com/hashicorp/nomad/issues/23443)]

BUG FIXES:

* api: Fixed bug where newlines in JobSubmission vars weren't encoded correctly [[GH-23560](https://github.com/hashicorp/nomad/issues/23560)]
* cli: Fixed bug where the `plugin status` command would fail if the plugin ID was a prefix of another plugin ID [[GH-23502](https://github.com/hashicorp/nomad/issues/23502)]
* cli: Fixed bug where the `quota status` and `quota inspect` commands would fail if the quota name was a prefix of another quota name [[GH-23502](https://github.com/hashicorp/nomad/issues/23502)]
* cli: Fixed bug where the `scaling policy info` command would fail if the policy ID was a prefix of another policy ID [[GH-23502](https://github.com/hashicorp/nomad/issues/23502)]
* cli: Fixed bug where the `service info` command would fail if the service name was a prefix of another service name in the same namespace [[GH-23502](https://github.com/hashicorp/nomad/issues/23502)]
* cli: Fixed bug where the `volume deregister`, `volume detach`, and `volume status` commands would fail if the volume ID was a prefix of another volume ID in the same namespace [[GH-23502](https://github.com/hashicorp/nomad/issues/23502)]
* consul: Fixed a bug where service registration and Envoy bootstrap would not wait for Consul ACL tokens and services to be replicated to the local agent [[GH-23381](https://github.com/hashicorp/nomad/issues/23381)]
* qemu: Fixed a bug that prevented `qemu` tasks from running on Linux [[GH-23466](https://github.com/hashicorp/nomad/issues/23466)]
* quota (Enterprise): Fixed a bug where a task's resource core count was not translated to CPU MHz and checked against its quota when performing a job plan [[GH-18876](https://github.com/hashicorp/nomad/issues/18876)]
* scheduler: Fix a bug where reserved resources are not calculated correctly [[GH-23386](https://github.com/hashicorp/nomad/issues/23386)]
* server: Fixed a bug where expiring heartbeats for garbage collected nodes could panic the server [[GH-23383](https://github.com/hashicorp/nomad/issues/23383)]
* template: Fix template rendering on Windows [[GH-23432](https://github.com/hashicorp/nomad/issues/23432)]

## 1.7.9 Enterprise (June 19, 2024)

SECURITY:

* build: Updated Go to 1.22.4 to address Go stdlib vulnerabilities CVE-2024-24789 and CVE-2024-24790 [[GH-23172](https://github.com/hashicorp/nomad/issues/23172)]

IMPROVEMENTS:

* cli: `operator snapshot inspect` now includes details of data in snapshot [[GH-18372](https://github.com/hashicorp/nomad/issues/18372)]
* docker: Added container_exists_attempts plugin configuration variable [[GH-22419](https://github.com/hashicorp/nomad/issues/22419)]
* exec: Fixed a bug where `exec` driver tasks would fail on older versions of glibc [[GH-23331](https://github.com/hashicorp/nomad/issues/23331)]

BUG FIXES:

* acl: Fix plugin policy validation when checking write permissions [[GH-23274](https://github.com/hashicorp/nomad/issues/23274)]
* connect: fix validation with multiple socket paths [[GH-22312](https://github.com/hashicorp/nomad/issues/22312)]
* consul: (Enterprise) Fixed a bug where gateway config entries were written before Sentinel policies were enforced [[GH-22228](https://github.com/hashicorp/nomad/issues/22228)]
* consul: Fixed a bug where Consul admin partition was not used to login via Consul JWT auth method [[GH-22226](https://github.com/hashicorp/nomad/issues/22226)]
* consul: Fixed a bug where gateway config entries were written to the Nomad server agent's Consul partition and not the client's partition [[GH-22228](https://github.com/hashicorp/nomad/issues/22228)]
* driver: Fixed a bug where the exec, java, and raw_exec drivers would not configure cgroups to allow access to devices provided by device plugins [[GH-22518](https://github.com/hashicorp/nomad/issues/22518)]
* scheduler: Fixed a bug where rescheduled allocations that could not be placed would later ignore their reschedule policy limits [[GH-12319](https://github.com/hashicorp/nomad/issues/12319)]

## 1.7.8 Enterprise (May 28, 2024)

SECURITY:

* deps: Updated `docker` dependency to 25.0.5 [[GH-20171](https://github.com/hashicorp/nomad/issues/20171)]

IMPROVEMENTS:

* auth: Add support for authenticating via Workload Identity to the quota and sentinel APIs
* autopilot: Added `operator autopilot health` command to review Autopilot health data [[GH-20156](https://github.com/hashicorp/nomad/issues/20156)]
* cli: Add `-jwks-ca-file` argument to `setup consul/vault` commands [[GH-20518](https://github.com/hashicorp/nomad/issues/20518)]
* client/volumes: Add a mount volume level option for selinux tags on volumes [[GH-19839](https://github.com/hashicorp/nomad/issues/19839)]
* consul: provide tasks that have Consul tokens the CONSUL_HTTP_TOKEN environment variable [[GH-20519](https://github.com/hashicorp/nomad/issues/20519)]
* ui: Improve error and warning messages for invalid variable and job template paths/names [[GH-19989](https://github.com/hashicorp/nomad/issues/19989)]
* ui: Prompt a user before they close an exec window to prevent accidental close-browser-tab shortcuts that overlap with terminal ones [[GH-19985](https://github.com/hashicorp/nomad/issues/19985)]

BUG FIXES:

* cli: Fix handling of scaling jobs which don't generate evals [[GH-20479](https://github.com/hashicorp/nomad/issues/20479)]
* client: Fix unallocated CPU metric calculation when client reserved CPU is set [[GH-20543](https://github.com/hashicorp/nomad/issues/20543)]
* client: terminate old exec task processes before starting new ones, to avoid accidentally leaving running processes in case of an error [[GH-20500](https://github.com/hashicorp/nomad/issues/20500)]
* config: Fixed a panic triggered by registering a job specifying a Vault cluster that has not been configured within the server [[GH-22227](https://github.com/hashicorp/nomad/issues/22227)]
* core: Fix multiple incorrect type conversion for potential overflows [[GH-20553](https://github.com/hashicorp/nomad/issues/20553)]
* csi: Fixed a bug where concurrent mount and unmount operations could unstage volumes needed by another allocation [[GH-20550](https://github.com/hashicorp/nomad/issues/20550)]
* csi: Fixed a bug where plugins would not be deleted on GC if their job updated the plugin ID [[GH-20555](https://github.com/hashicorp/nomad/issues/20555)]
* csi: Fixed a bug where volumes in different namespaces but the same ID would fail to stage on the same client [[GH-20532](https://github.com/hashicorp/nomad/issues/20532)]
* job endpoint: fix implicit constraint mutation for task-level services [[GH-22229](https://github.com/hashicorp/nomad/issues/22229)]
* quota (Enterprise): Fixed a bug where quota usage would not be freed if a job was purged
* services: Added retry to Nomad service deregistration RPCs during alloc stop [[GH-20596](https://github.com/hashicorp/nomad/issues/20596)]
* services: Fixed bug where Nomad services might not be deregistered when nodes are marked down or allocations are terminal [[GH-20590](https://github.com/hashicorp/nomad/issues/20590)]
* structs: Fix job canonicalization for array type fields [[GH-20522](https://github.com/hashicorp/nomad/issues/20522)]
* ui: Fix a bug where the UI would prompt a user to promote a deployment with unplaced canaries [[GH-20408](https://github.com/hashicorp/nomad/issues/20408)]
* ui: Fixed an issue where keynav would not trigger evaluation sidebar expand [[GH-20047](https://github.com/hashicorp/nomad/issues/20047)]
* ui: Show the namespace in the web UI exec command hint [[GH-20218](https://github.com/hashicorp/nomad/issues/20218)]
* windows: Fixed a regression where scanning task processes was inefficient [[GH-20619](https://github.com/hashicorp/nomad/issues/20619)]

## 1.7.7 (April 16, 2024)

SECURITY:

* artifact: Updated `go-getter` dependency to v1.7.4 to address CVE-2024-3817 [[GH-20391](https://github.com/hashicorp/nomad/issues/20391)]

IMPROVEMENTS:

* autopilot: add Enterprise health information to autopilot API [[GH-20153](https://github.com/hashicorp/nomad/issues/20153)]
* cli: Collect only one heap profile per `operator debug` interval [[GH-20219](https://github.com/hashicorp/nomad/issues/20219)]
* consul/connect: Added support for TLS configuration, headers configuration, and request limit configuration to ingress service block [[GH-16753](https://github.com/hashicorp/nomad/issues/16753)]
* consul/connect: Added support for destination partition in `upstream` block [[GH-20167](https://github.com/hashicorp/nomad/issues/20167)]
* scheduler: Record exhausted node metrics for devices when preemption fails to find an allocation to evict [[GH-20346](https://github.com/hashicorp/nomad/issues/20346)]
* ui: When you re-bind keyboard shortcuts they now correctly show up in shift-held hints [[GH-20235](https://github.com/hashicorp/nomad/issues/20235)]

BUG FIXES:

* agent: allow configuration of in-memory telemetry sink [[GH-20166](https://github.com/hashicorp/nomad/issues/20166)]
* api: Fixed a bug where `AllocDirStats` field was missing from Read Stats client API [[GH-20261](https://github.com/hashicorp/nomad/issues/20261)]
* cli: Fixed a bug where `operator debug` did not respect the `-pprof-interval` flag and would take only one profile [[GH-20206](https://github.com/hashicorp/nomad/issues/20206)]
* cni: Fixed a regression where default DNS set by `dockerd` or other task drivers was not respected [[GH-20189](https://github.com/hashicorp/nomad/issues/20189)]
* config: Fixed a bug where IPv6 addresses were not accepted without ports for `client.servers` blocks [[GH-20324](https://github.com/hashicorp/nomad/issues/20324)]
* consul: Fixed a bug where services with interpolation would not get correctly signed Workload Identities [[GH-20344](https://github.com/hashicorp/nomad/issues/20344)]
* deployments: Fixed a goroutine leak when jobs are purged [[GH-20348](https://github.com/hashicorp/nomad/issues/20348)]
* deps: Updated consul-template dependency to 0.37.4 to fix a resource leak [[GH-20234](https://github.com/hashicorp/nomad/issues/20234)]
* docker: Fixed a bug where cpuset cgroup would not be updated on cgroup v1 systems [[GH-20294](https://github.com/hashicorp/nomad/issues/20294)]
* docker: Fixed a bug where cpuset would not be updated on cgroup v2 systems using cgroupfs [[GH-20276](https://github.com/hashicorp/nomad/issues/20276)]
* drain: Fixed a bug where Workload Identity tokens could not be used to drain a node [[GH-20317](https://github.com/hashicorp/nomad/issues/20317)]
* namespace/node pool: Fixed a bug where the `-region` flag would not be respected for namespace and node pool updates if ACLs were disabled [[GH-20220](https://github.com/hashicorp/nomad/issues/20220)]
* state: Fixed a bug where restarting a server could fail if the Raft logs include a drain update that used a now-expired token [[GH-20317](https://github.com/hashicorp/nomad/issues/20317)]
* template: Fixed a bug where a partial `client.template` block would cause defaults for unspecified fields to be ignored [[GH-20165](https://github.com/hashicorp/nomad/issues/20165)]
* ui: Fix an issue where the job status box would error if an allocation had no task events [[GH-20383](https://github.com/hashicorp/nomad/issues/20383)]

## 1.7.6 (March 12, 2024)

SECURITY:

* build: Update to go1.22 to address Go standard library vulnerabilities CVE-2024-24783, CVE-2023-45290, and CVE-2024-24785. [[GH-20066](https://github.com/hashicorp/nomad/issues/20066)]
* deps: Upgrade protobuf library to 1.33.0 to avoid scan alerts for CVE-2024-24786, which Nomad is not vulnerable to [[GH-20100](https://github.com/hashicorp/nomad/issues/20100)]

IMPROVEMENTS:

* cli: Added -json option on job status command [[GH-18925](https://github.com/hashicorp/nomad/issues/18925)]
* fingerprint: Added a fingerprint for Consul DNS address and port [[GH-19969](https://github.com/hashicorp/nomad/issues/19969)]

BUG FIXES:

* cli: Fixed a bug where the `nomad job restart` command could crash if the job type was not present in a response from the server [[GH-20049](https://github.com/hashicorp/nomad/issues/20049)]
* client: Fixed a bug where corrupt client state could panic the client [[GH-19972](https://github.com/hashicorp/nomad/issues/19972)]
* cni: Fixed a bug where DNS set by CNI plugins was not provided to task drivers [[GH-20007](https://github.com/hashicorp/nomad/issues/20007)]
* connect: Fixed a bug where `expose` blocks would not appear in `job plan` diff output [[GH-19990](https://github.com/hashicorp/nomad/issues/19990)]
* server: Prevent NPE when service lacks identity [[GH-19986](https://github.com/hashicorp/nomad/issues/19986)]

## 1.7.5 (February 13, 2024)

SECURITY:

* windows: Remove `LazyDLL` calls for system modules to harden Nomad against attacks from the host [[GH-19925](https://github.com/hashicorp/nomad/issues/19925)]

IMPROVEMENTS:

* api: emit `JobDeregistered` event when job is deregistered with `purge` [[GH-19903](https://github.com/hashicorp/nomad/issues/19903)]

BUG FIXES:

* cli: Fix return code when `nomad job run` succeeds after a blocked eval [[GH-19876](https://github.com/hashicorp/nomad/issues/19876)]
* cli: Fixed a bug where the `nomad tls ca create` command failed when the `-domain` was used without other values [[GH-19892](https://github.com/hashicorp/nomad/issues/19892)]
* client: Ensure the value for CPU shares are within the allowed range [[GH-19935](https://github.com/hashicorp/nomad/issues/19935)]
* client: Prevent client from starting if cgroup initialization fails [[GH-19915](https://github.com/hashicorp/nomad/issues/19915)]
* connect: Fixed envoy sidecars being unable to restart after node reboots [[GH-19787](https://github.com/hashicorp/nomad/issues/19787)]
* driver/java: Ensure the OOM killed response is populated when the task exits [[GH-19818](https://github.com/hashicorp/nomad/issues/19818)]
* driver/qemu: Ensure the OOM killed response is populated when the task exits [[GH-19830](https://github.com/hashicorp/nomad/issues/19830)]
* driver/rawexec: Ensure the OOM killed response is populated when the task exits [[GH-19829](https://github.com/hashicorp/nomad/issues/19829)]
* exec: Fixed a bug in `alloc exec` where closing websocket streams could cause a panic [[GH-19932](https://github.com/hashicorp/nomad/issues/19932)]
* scheduler: Fixed a bug that caused blocked evaluations due to port conflict to not have a reason explaining why the evaluation was blocked [[GH-19933](https://github.com/hashicorp/nomad/issues/19933)]
* ui: Fix an issue where a same-named task from a different group could be selected when the user clicks Exec from a task group page where multiple allocations would be valid [[GH-19878](https://github.com/hashicorp/nomad/issues/19878)]

## 1.7.4 (February 08, 2024)

SECURITY:

* deps: Updated runc to 1.1.12 to address CVE-2024-21626 [[GH-19851](https://github.com/hashicorp/nomad/issues/19851)]
* migration: Fixed a bug where archives used for migration were not checked for symlinks that escaped the allocation directory [[GH-19887](https://github.com/hashicorp/nomad/issues/19887)]
* template: Fixed a bug where symlinks could force templates to read and write to arbitrary locations (CVE-2024-1329) [[GH-19888](https://github.com/hashicorp/nomad/issues/19888)]

## 1.7.3 (January 15, 2024)

IMPROVEMENTS:

* build: update to go 1.21.6 [[GH-19709](https://github.com/hashicorp/nomad/issues/19709)]
* cgroupslib: Consider CGroups OFF when essential controllers are missing [[GH-19176](https://github.com/hashicorp/nomad/issues/19176)]
* cli: Add new option `nomad setup vault -check` to help cluster operators migrate to workload identities for Vault [[GH-19720](https://github.com/hashicorp/nomad/issues/19720)]
* consul: Add fingerprint for Consul Enterprise admin partitions [[GH-19485](https://github.com/hashicorp/nomad/issues/19485)]
* consul: Added support for Consul Enterprise admin partitions [[GH-19665](https://github.com/hashicorp/nomad/issues/19665)]
* consul: Added support for failures_before_warning and failures_before_critical in Nomad agent services [[GH-19336](https://github.com/hashicorp/nomad/issues/19336)]
* consul: Added support for failures_before_warning in Consul service checks [[GH-19336](https://github.com/hashicorp/nomad/issues/19336)]
* drivers/exec: Added support for OOM detection in exec driver [[GH-19563](https://github.com/hashicorp/nomad/issues/19563)]
* drivers: Enable configuring a raw_exec task to not have an upper memory limit [[GH-19670](https://github.com/hashicorp/nomad/issues/19670)]
* identity: Added vault_role to JWT workload identity claims if specified in jobspec [[GH-19535](https://github.com/hashicorp/nomad/issues/19535)]
* ui: Added group name to allocation tooltips on job status panel [[GH-19601](https://github.com/hashicorp/nomad/issues/19601)]
* ui: Adds a warning message to pages in the Web UI when logs are disabled [[GH-18823](https://github.com/hashicorp/nomad/issues/18823)]
* ui: Hide token secret upon successful login [[GH-19529](https://github.com/hashicorp/nomad/issues/19529)]
* ui: when an Action has long output, anchor to the latest messages [[GH-19452](https://github.com/hashicorp/nomad/issues/19452)]
* vault: Add `allow_token_expiration` field to allow Vault tokens to expire without renewal for short-lived tasks [[GH-19691](https://github.com/hashicorp/nomad/issues/19691)]
* vault: Nomad clients will no longer attempt to renew Vault tokens that cannot be renewed [[GH-19691](https://github.com/hashicorp/nomad/issues/19691)]

BUG FIXES:

* acl: Fixed a bug where 1.5 and 1.6 clients could not access Nomad Variables and Services via templates [[GH-19578](https://github.com/hashicorp/nomad/issues/19578)]
* acl: Fixed auth method hashing which meant changing some fields would be silently ignored [[GH-19677](https://github.com/hashicorp/nomad/issues/19677)]
* auth: Added new optional OIDCDisableUserInfo setting for OIDC auth provider [[GH-19566](https://github.com/hashicorp/nomad/issues/19566)]
* client: Fixed a bug where where the environment variable / file for the Consul token weren't written. [[GH-19490](https://github.com/hashicorp/nomad/issues/19490)]
* consul (Enterprise): Fixed a bug where the group/task Consul cluster was assigned "default" when unset instead of the namespace-governed value
* core: Ensure job HCL submission data is persisted and restored during the FSM snapshot process [[GH-19605](https://github.com/hashicorp/nomad/issues/19605)]
* namespaces: Failed delete calls no longer return success codes [[GH-19483](https://github.com/hashicorp/nomad/issues/19483)]
* rawexec: Fixed a bug where oom_score_adj would be inherited from Nomad client [[GH-19515](https://github.com/hashicorp/nomad/issues/19515)]
* server: Fix panic when validating non-service reschedule block [[GH-19652](https://github.com/hashicorp/nomad/issues/19652)]
* server: Fix server not waiting for workers to submit nacks for dequeued evaluations before shutting down [[GH-19560](https://github.com/hashicorp/nomad/issues/19560)]
* state: Fixed a bug where purged jobs would not get new deployments [[GH-19609](https://github.com/hashicorp/nomad/issues/19609)]
* ui: Fix rendering of allocations table for jobs that don't have actions [[GH-19505](https://github.com/hashicorp/nomad/issues/19505)]
* vault: Fixed a bug that could cause errors during leadership transition when migrating to the new JWT and workload identity authentication workflow [[GH-19689](https://github.com/hashicorp/nomad/issues/19689)]
* vault: Fixed a bug where `allow_unauthenticated` was enforced when a `default_identity` was set [[GH-19585](https://github.com/hashicorp/nomad/issues/19585)]

## 1.7.2 (December 13, 2023)

FEATURES:

* **Reschedule on Lost**: Adds the ability to prevent tasks on down nodes from being rescheduled [[GH-16867](https://github.com/hashicorp/nomad/issues/16867)]

IMPROVEMENTS:

* audit (Enterprise): Added ACL token role links to audit log auth objects [[GH-19415](https://github.com/hashicorp/nomad/issues/19415)]
* ui: Added a new example template with Task Actions [[GH-19153](https://github.com/hashicorp/nomad/issues/19153)]
* ui: dont allow new jobspec download until template is populated, and remove group count from jobs index [[GH-19377](https://github.com/hashicorp/nomad/issues/19377)]
* ui: make the exec window look nicer on mobile screens [[GH-19332](https://github.com/hashicorp/nomad/issues/19332)]

BUG FIXES:

* auth: Fixed a bug where `tls.verify_server_hostname=false` was not respected, leading to authentication failures between Nomad agents [[GH-19425](https://github.com/hashicorp/nomad/issues/19425)]
* cli: Fix a bug in the `var put` command which prevented combining items as CLI arguments and other parameters as flags [[GH-19423](https://github.com/hashicorp/nomad/issues/19423)]
* client: Fix a panic in building CPU topology when inaccurate CPU data is provided [[GH-19383](https://github.com/hashicorp/nomad/issues/19383)]
* client: Fixed a bug where clients are unable to detect CPU topology in certain conditions [[GH-19457](https://github.com/hashicorp/nomad/issues/19457)]
* consul (Enterprise): Fixed a bug where implicit Consul constraints were not specific to non-default Consul clusters [[GH-19449](https://github.com/hashicorp/nomad/issues/19449)]
* consul: uses token namespace to fetch policies for verification [[GH-18516](https://github.com/hashicorp/nomad/issues/18516)]
* core: Fixed a bug where linux nodes with no reservable cores would panic the scheduler [[GH-19458](https://github.com/hashicorp/nomad/issues/19458)]
* csi: Added validation to `csi_plugin` blocks to prevent `stage_publish_base_dir` from being a subdirectory of `mount_dir` [[GH-19441](https://github.com/hashicorp/nomad/issues/19441)]
* metrics: Revert upgrade of `go-metrics` to fix an issue where metrics from dependencies, such as raft, were no longer emitted [[GH-19374](https://github.com/hashicorp/nomad/issues/19374)]
* ui: Fixed an issue where Accessor ID was masked by default when editing a token [[GH-19432](https://github.com/hashicorp/nomad/issues/19432)]
* vault: Fixed a bug that caused `template` blocks to ignore Nomad configuration for Vault and use the default address of `https://127.0.0.1:8200` when the job does not have a `vault` block defined [[GH-19439](https://github.com/hashicorp/nomad/issues/19439)]

## 1.7.1 (December 08, 2023)

BUG FIXES:

* cli: Fixed a bug that caused the `nomad agent` command to ignore the `VAULT_TOKEN` and `VAULT_NAMESPACE` environment variables [[GH-19349](https://github.com/hashicorp/nomad/issues/19349)]
* client: remove incomplete allocation entries from client state database during client restarts [[GH-16638](https://github.com/hashicorp/nomad/issues/16638)]
* connect: Fixed a bug where deployments would not wait for Connect sidecar task health checks to pass [[GH-19334](https://github.com/hashicorp/nomad/issues/19334)]
* keyring: Fixed a bug where RSA keys were not replicated to followers [[GH-19350](https://github.com/hashicorp/nomad/issues/19350)]

## 1.7.0 (December 07, 2023)

FEATURES:

* **Job Actions**: Introduces the action concept to jobspecs, the web UI, CLI and API. Operators can now define actions that Nomad users can execute against running allocations. [[GH-18794](https://github.com/hashicorp/nomad/issues/18794)]
* **Multiple Vault and Consul Clusters:** Nomad Enterprise can now use multiple Vault or Consul clusters. Each task or service can be registered with a different Consul cluster and each task can obtain secrets from a different Vault cluster. [[GH-5311](https://github.com/hashicorp/nomad/issues/5311)]
* **NUMA aware scheduling**: Nomad Enterprise now supports optimized scheduling on NUMA hardware [[GH-18681](https://github.com/hashicorp/nomad/issues/18681)]
* **Workload Identity IDP:** Nomad's workload identities may now be used with third parties that support JWT or OIDC IDPs such as the AWS IAM OIDC Provider. [[GH-18691](https://github.com/hashicorp/nomad/issues/18691)]
* **Workload Identity for Consul:** Jobs can now use workload identity to authenticate to Consul. [[GH-15618](https://github.com/hashicorp/nomad/issues/15618)]
* **Workload Identity for Vault:** Jobs can now use workload identity to authenticate to Vault. [[GH-15617](https://github.com/hashicorp/nomad/issues/15617)]

BREAKING CHANGES:

* client/fingerprint: The `cpu.numcores.power` node attribute has been renamed to `cpu.numcores.performance` on Apple Silicon nodes [[GH-18843](https://github.com/hashicorp/nomad/issues/18843)]
* client: the `unique.cgroup.mountpoint` node attribute has been removed [[GH-18371](https://github.com/hashicorp/nomad/issues/18371)]
* client: the `unique.cgroup.version` node attribute has been renamed to `os.cgroups.version` [[GH-18371](https://github.com/hashicorp/nomad/issues/18371)]
* core: Honor job's namespace when checking `distinct_hosts` feasibility [[GH-19004](https://github.com/hashicorp/nomad/issues/19004)]

SECURITY:

* build: Update to go1.21.4 to resolve Windows path validation CVE in Go [[GH-19013](https://github.com/hashicorp/nomad/issues/19013)]
* build: Update to go1.21.5 to resolve Windows path validation CVE in Go [[GH-19320](https://github.com/hashicorp/nomad/issues/19320)]

IMPROVEMENTS:

* api: Add JWKS HTTP API endpoint [[GH-18035](https://github.com/hashicorp/nomad/issues/18035)]
* api: Added support for Unix domain sockets [[GH-16872](https://github.com/hashicorp/nomad/issues/16872)]
* build (Enterprise): Support building s390x binaries. [[GH-18069](https://github.com/hashicorp/nomad/issues/18069)]
* cli: Add file prediction for operator raft/snapshot commands [[GH-18901](https://github.com/hashicorp/nomad/issues/18901)]
* cli: Added help text to `acl bootstrap` about reading the initial token from a file [[GH-18961](https://github.com/hashicorp/nomad/issues/18961)]
* cli: Added identities, networks, and volumes to the output of the `operator client-state` command [[GH-18996](https://github.com/hashicorp/nomad/issues/18996)]
* cli: Added support for prefix ID matching and wildcard namespaces to `service info` command [[GH-18836](https://github.com/hashicorp/nomad/issues/18836)]
* client: add support for NetBSD clients [[GH-18562](https://github.com/hashicorp/nomad/issues/18562)]
* client: enable detection of numa topology [[GH-18146](https://github.com/hashicorp/nomad/issues/18146)]
* config: Add `go-netaddrs` support to `server_join.retry_join` [[GH-18745](https://github.com/hashicorp/nomad/issues/18745)]
* consul: constraint for minimum version of Consul increased to 1.8.0 [[GH-19104](https://github.com/hashicorp/nomad/issues/19104)]
* deps: bumped `shirou/gopsutil` to v3.23.9 [[GH-18562](https://github.com/hashicorp/nomad/issues/18562)]
* fingerprint: clients now backoff after successfully fingerprinting Consul [[GH-18426](https://github.com/hashicorp/nomad/issues/18426)]
* identity: Add support for multiple workload identities [[GH-18123](https://github.com/hashicorp/nomad/issues/18123)]
* identity: Implement `change_mode` and `change_signal` for workload identities [[GH-18943](https://github.com/hashicorp/nomad/issues/18943)]
* identity: Support jwt expiration and rotation [[GH-18262](https://github.com/hashicorp/nomad/issues/18262)]
* identity: default to RS256 for new workload ids [[GH-18882](https://github.com/hashicorp/nomad/issues/18882)]
* sentinel (Enterprise): Add existing job information to Sentinel when available. [[GH-18553](https://github.com/hashicorp/nomad/issues/18553)]
* server: Added transfer-leadership API and CLI [[GH-17383](https://github.com/hashicorp/nomad/issues/17383)]
* sso: Allow adding a token name format to auth methods which can be used to generate token names when signing in via SSO [[GH-19135](https://github.com/hashicorp/nomad/issues/19135)]
* ui: color-code node and server status cells [[GH-18318](https://github.com/hashicorp/nomad/issues/18318)]
* ui: for system and sysbatch jobs, now show client name on hover in job panel [[GH-19051](https://github.com/hashicorp/nomad/issues/19051)]
* ui: nicer comment styles in UI example jobs [[GH-19037](https://github.com/hashicorp/nomad/issues/19037)]
* ui: show plan output warnings alongside placement failures and dry-run info when running a job through the web ui [[GH-19225](https://github.com/hashicorp/nomad/issues/19225)]
* ui: simplify presentation of task event times (10m2.230948s bceomes 10m2s etc.) [[GH-18595](https://github.com/hashicorp/nomad/issues/18595)]
* vars: Added a locking feature for Nomad Variables [[GH-18520](https://github.com/hashicorp/nomad/issues/18520)]

DEPRECATIONS:

* config: Loading plugins from `plugin_dir` without a `plugin` configuration block is deprecated [[GH-19189](https://github.com/hashicorp/nomad/issues/19189)]

BUG FIXES:

* agent: Correct websocket status code handling [[GH-19172](https://github.com/hashicorp/nomad/issues/19172)]
* api: Fix panic in `Allocation.Stub` method when `Job` is unset [[GH-19115](https://github.com/hashicorp/nomad/issues/19115)]
* cli: Fixed a bug that caused the `nomad job restart` command to miscount the allocations to restart [[GH-19155](https://github.com/hashicorp/nomad/issues/19155)]
* cli: Fixed a bug where the `operator client-state` command would crash if it reads an allocation without a task state [[GH-18996](https://github.com/hashicorp/nomad/issues/18996)]
* cli: Fixed a panic when the `nomad job restart` command received an interrupt signal while waiting for an answer [[GH-19154](https://github.com/hashicorp/nomad/issues/19154)]
* cli: Fixed the `nomad job restart` command to create replacements for batch and system jobs and to prevent sysbatch jobs from being rescheduled since they never create replacements [[GH-19147](https://github.com/hashicorp/nomad/issues/19147)]
* client: Fixed a bug where client API calls would fail incorrectly with permission denied errors when using ACL tokens with dangling policies [[GH-18972](https://github.com/hashicorp/nomad/issues/18972)]
* core: Fix incorrect submit time for stopped jobs [[GH-18967](https://github.com/hashicorp/nomad/issues/18967)]
* ui: Fixed an issue where purging a job with a namespace did not process correctly [[GH-19139](https://github.com/hashicorp/nomad/issues/19139)]
* ui: fix an issue where starting a stopped job with default-less variables would not retain those variables when done via the job page start button in the web ui [[GH-19220](https://github.com/hashicorp/nomad/issues/19220)]
* ui: fix the job auto-linked variable path name when user lacks variable write permissions [[GH-18598](https://github.com/hashicorp/nomad/issues/18598)]
* variables: Fixed a bug where poststop tasks were not allowed access to Variables [[GH-18754](https://github.com/hashicorp/nomad/issues/18754)]
* vault: Fixed a bug where poststop tasks would not get a Vault token [[GH-19268](https://github.com/hashicorp/nomad/issues/19268)]
* vault: Fixed an issue that could cause Nomad to attempt to renew a Vault token that is already expired [[GH-18985](https://github.com/hashicorp/nomad/issues/18985)]

## 1.6.14 Enterprise (August 13, 2024)

SECURITY:

* security: Fix symlink escape during unarchiving by removing existing paths within the same allocdir. Compromising the Nomad client agent at the source allocation first is a prerequisite for leveraging this issue. [[GH-23738](https://github.com/hashicorp/nomad/issues/23738)]

IMPROVEMENTS:

* keyring: Added support for prepublishing keys [[GH-23577](https://github.com/hashicorp/nomad/issues/23577)]

BUG FIXES:

* cni: .conf and .json config files are now parsed properly [[GH-23629](https://github.com/hashicorp/nomad/issues/23629)]
* docker: Fixed a bug where plugin SELinux labels would conflict with read-only `volume` options [[GH-23750](https://github.com/hashicorp/nomad/issues/23750)]
* keyring: Fixed a bug where keys could be garbage collected before workload identities expire [[GH-23577](https://github.com/hashicorp/nomad/issues/23577)]
* keyring: Fixed a bug where keys would never exit the "rekeying" state after a rotation with the `-full` flag [[GH-23577](https://github.com/hashicorp/nomad/issues/23577)]
* keyring: Fixed a bug where periodic key rotation would not occur [[GH-23577](https://github.com/hashicorp/nomad/issues/23577)]
* networking: The same static port can now be used more than once on host networks with multiple IPs [[GH-23693](https://github.com/hashicorp/nomad/issues/23693)]
* scaling: Fixed a bug where state store corruption could occur when writing scaling events [[GH-23673](https://github.com/hashicorp/nomad/issues/23673)]
* template: Fixed a bug where change_mode = "script" would not execute after a client restart [[GH-23663](https://github.com/hashicorp/nomad/issues/23663)]
* windows: Fix bug with containers capabilities on Docker CE [[GH-23599](https://github.com/hashicorp/nomad/issues/23599)]

## 1.6.13 Enterprise (July 16, 2024)

BREAKING CHANGES:

* docker: default to hyper-v isolation mode on Windows [[GH-23452](https://github.com/hashicorp/nomad/issues/23452)]

SECURITY:

* build: Updated Go to 1.22.5 to address CVE-2024-24791 [[GH-23498](https://github.com/hashicorp/nomad/issues/23498)]
* migration: Added a check for relative paths escaping the allocation directory when unpacking archive during migration, to harden clients against compromised peer clients sending malicious archives [[GH-23319](https://github.com/hashicorp/nomad/issues/23319)]
* security: Removed insecure TLS cipher suites: `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`, `TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA25` and `TLS_RSA_WITH_AES_128_CBC_SHA256`. [[GH-23551](https://github.com/hashicorp/nomad/issues/23551)]

IMPROVEMENTS:

* deps: Updated Consul API to 1.29.1. [[GH-23436](https://github.com/hashicorp/nomad/issues/23436)]
* deps: Updated consul-template to 0.39 to allow admin partition and sameness groups queries. [[GH-23436](https://github.com/hashicorp/nomad/issues/23436)]
* docker: Validate that unprivileged containers aren't running as ContainerAdmin on Windows [[GH-23443](https://github.com/hashicorp/nomad/issues/23443)]

BUG FIXES:

* api: Fixed bug where newlines in JobSubmission vars weren't encoded correctly [[GH-23560](https://github.com/hashicorp/nomad/issues/23560)]
* cli: Fixed bug where the `plugin status` command would fail if the plugin ID was a prefix of another plugin ID [[GH-23502](https://github.com/hashicorp/nomad/issues/23502)]
* cli: Fixed bug where the `quota status` and `quota inspect` commands would fail if the quota name was a prefix of another quota name [[GH-23502](https://github.com/hashicorp/nomad/issues/23502)]
* cli: Fixed bug where the `scaling policy info` command would fail if the policy ID was a prefix of another policy ID [[GH-23502](https://github.com/hashicorp/nomad/issues/23502)]
* cli: Fixed bug where the `service info` command would fail if the service name was a prefix of another service name in the same namespace [[GH-23502](https://github.com/hashicorp/nomad/issues/23502)]
* cli: Fixed bug where the `volume deregister`, `volume detach`, and `volume status` commands would fail if the volume ID was a prefix of another volume ID in the same namespace [[GH-23502](https://github.com/hashicorp/nomad/issues/23502)]
* quota (Enterprise): Fixed a bug where a task's resource core count was not translated to CPU MHz and checked against its quota when performing a job plan [[GH-18876](https://github.com/hashicorp/nomad/issues/18876)]
* scheduler: Fix a bug where reserved resources are not calculated correctly [[GH-23386](https://github.com/hashicorp/nomad/issues/23386)]
* server: Fixed a bug where expiring heartbeats for garbage collected nodes could panic the server [[GH-23383](https://github.com/hashicorp/nomad/issues/23383)]
* template: Fix template rendering on Windows [[GH-23432](https://github.com/hashicorp/nomad/issues/23432)]

## 1.6.12 Enterprise (June 19, 2024)

SECURITY:

* build: Updated Go to 1.22.4 to address Go stdlib vulnerabilities CVE-2024-24789 and CVE-2024-24790 [[GH-23172](https://github.com/hashicorp/nomad/issues/23172)]

IMPROVEMENTS:

* cli: `operator snapshot inspect` now includes details of data in snapshot [[GH-18372](https://github.com/hashicorp/nomad/issues/18372)]
* docker: Added container_exists_attempts plugin configuration variable [[GH-22419](https://github.com/hashicorp/nomad/issues/22419)]
* exec: Fixed a bug where `exec` driver tasks would fail on older versions of glibc [[GH-23331](https://github.com/hashicorp/nomad/issues/23331)]

BUG FIXES:

* acl: Fix plugin policy validation when checking write permissions [[GH-23274](https://github.com/hashicorp/nomad/issues/23274)]
* connect: fix validation with multiple socket paths [[GH-22312](https://github.com/hashicorp/nomad/issues/22312)]
* driver: Fixed a bug where the exec, java, and raw_exec drivers would not configure cgroups to allow access to devices provided by device plugins [[GH-22518](https://github.com/hashicorp/nomad/issues/22518)]
* scheduler: Fixed a bug where rescheduled allocations that could not be placed would later ignore their reschedule policy limits [[GH-12319](https://github.com/hashicorp/nomad/issues/12319)]

## 1.6.11 Enterprise (May 28, 2024)

SECURITY:

* deps: Updated `docker` dependency to 25.0.5 [[GH-20171](https://github.com/hashicorp/nomad/issues/20171)]

BUG FIXES:

* cli: Fix handling of scaling jobs which don't generate evals [[GH-20479](https://github.com/hashicorp/nomad/issues/20479)]
* client: terminate old exec task processes before starting new ones, to avoid accidentally leaving running processes in case of an error [[GH-20500](https://github.com/hashicorp/nomad/issues/20500)]
* core: Fix multiple incorrect type conversion for potential overflows [[GH-20553](https://github.com/hashicorp/nomad/issues/20553)]
* csi: Fixed a bug where concurrent mount and unmount operations could unstage volumes needed by another allocation [[GH-20550](https://github.com/hashicorp/nomad/issues/20550)]
* csi: Fixed a bug where plugins would not be deleted on GC if their job updated the plugin ID [[GH-20555](https://github.com/hashicorp/nomad/issues/20555)]
* csi: Fixed a bug where volumes in different namespaces but the same ID would fail to stage on the same client [[GH-20532](https://github.com/hashicorp/nomad/issues/20532)]
* quota (Enterprise): Fixed a bug where quota usage would not be freed if a job was purged
* services: Added retry to Nomad service deregistration RPCs during alloc stop [[GH-20596](https://github.com/hashicorp/nomad/issues/20596)]
* services: Fixed bug where Nomad services might not be deregistered when nodes are marked down or allocations are terminal [[GH-20590](https://github.com/hashicorp/nomad/issues/20590)]
* structs: Fix job canonicalization for array type fields [[GH-20522](https://github.com/hashicorp/nomad/issues/20522)]
* ui: Show the namespace in the web UI exec command hint [[GH-20218](https://github.com/hashicorp/nomad/issues/20218)]

## 1.6.10 (April 16, 2024)

SECURITY:

* artifact: Updated `go-getter` dependency to v1.7.4 to address CVE-2024-3817 [[GH-20391](https://github.com/hashicorp/nomad/issues/20391)]

BUG FIXES:

* api: Fixed a bug where `AllocDirStats` field was missing from Read Stats client API [[GH-20261](https://github.com/hashicorp/nomad/issues/20261)]
* cli: Fixed a bug where `operator debug` did not respect the `-pprof-interval` flag and would take only one profile [[GH-20206](https://github.com/hashicorp/nomad/issues/20206)]
* cni: Fixed a regression where default DNS set by `dockerd` or other task drivers was not respected [[GH-20189](https://github.com/hashicorp/nomad/issues/20189)]
* config: Fixed a bug where IPv6 addresses were not accepted without ports for `client.servers` blocks [[GH-20324](https://github.com/hashicorp/nomad/issues/20324)]
* deployments: Fixed a goroutine leak when jobs are purged [[GH-20348](https://github.com/hashicorp/nomad/issues/20348)]
* deps: Updated consul-template dependency to 0.37.4 to fix a resource leak [[GH-20234](https://github.com/hashicorp/nomad/issues/20234)]
* drain: Fixed a bug where Workload Identity tokens could not be used to drain a node [[GH-20317](https://github.com/hashicorp/nomad/issues/20317)]
* namespace/node pool: Fixed a bug where the `-region` flag would not be respected for namespace and node pool updates if ACLs were disabled [[GH-20220](https://github.com/hashicorp/nomad/issues/20220)]
* state: Fixed a bug where restarting a server could fail if the Raft logs include a drain update that used a now-expired token [[GH-20317](https://github.com/hashicorp/nomad/issues/20317)]
* template: Fixed a bug where a partial `client.template` block would cause defaults for unspecified fields to be ignored [[GH-20165](https://github.com/hashicorp/nomad/issues/20165)]
* ui: Fix an issue where the job status box would error if an allocation had no task events [[GH-20383](https://github.com/hashicorp/nomad/issues/20383)]

## 1.6.9 (March 12, 2024)

SECURITY:

* build: Update to go1.22 to address Go standard library vulnerabilities CVE-2024-24783, CVE-2023-45290, and CVE-2024-24785. [[GH-20066](https://github.com/hashicorp/nomad/issues/20066)]
* deps: Upgrade protobuf library to 1.33.0 to avoid scan alerts for CVE-2024-24786, which Nomad is not vulnerable to [[GH-20100](https://github.com/hashicorp/nomad/issues/20100)]

BUG FIXES:

* cli: Fixed a bug where the `nomad job restart` command could crash if the job type was not present in a response from the server [[GH-20049](https://github.com/hashicorp/nomad/issues/20049)]
* client: Fixed a bug where corrupt client state could panic the client [[GH-19972](https://github.com/hashicorp/nomad/issues/19972)]
* cni: Fixed a bug where DNS set by CNI plugins was not provided to task drivers [[GH-20007](https://github.com/hashicorp/nomad/issues/20007)]
* connect: Fixed a bug where `expose` blocks would not appear in `job plan` diff output [[GH-19990](https://github.com/hashicorp/nomad/issues/19990)]

## 1.6.8 (February 13, 2024)

SECURITY:

* windows: Remove `LazyDLL` calls for system modules to harden Nomad against attacks from the host [[GH-19925](https://github.com/hashicorp/nomad/issues/19925)]

BUG FIXES:

* cli: Fix return code when `nomad job run` succeeds after a blocked eval [[GH-19876](https://github.com/hashicorp/nomad/issues/19876)]
* cli: Fixed a bug where the `nomad tls ca create` command failed when the `-domain` was used without other values [[GH-19892](https://github.com/hashicorp/nomad/issues/19892)]
* connect: Fixed envoy sidecars being unable to restart after node reboots [[GH-19787](https://github.com/hashicorp/nomad/issues/19787)]
* exec: Fixed a bug in `alloc exec` where closing websocket streams could cause a panic [[GH-19932](https://github.com/hashicorp/nomad/issues/19932)]
* scheduler: Fixed a bug that caused blocked evaluations due to port conflict to not have a reason explaining why the evaluation was blocked [[GH-19933](https://github.com/hashicorp/nomad/issues/19933)]
* ui: Fix an issue where a same-named task from a different group could be selected when the user clicks Exec from a task group page where multiple allocations would be valid [[GH-19878](https://github.com/hashicorp/nomad/issues/19878)]

## 1.6.7 (February 08, 2024)

SECURITY:

* deps: Updated runc to 1.1.12 to address CVE-2024-21626 [[GH-19851](https://github.com/hashicorp/nomad/issues/19851)]
* migration: Fixed a bug where archives used for migration were not checked for symlinks that escaped the allocation directory [[GH-19887](https://github.com/hashicorp/nomad/issues/19887)]
* template: Fixed a bug where symlinks could force templates to read and write to arbitrary locations (CVE-2024-1329) [[GH-19888](https://github.com/hashicorp/nomad/issues/19888)]

## 1.6.6 (January 15, 2024)

IMPROVEMENTS:

* build: update to go 1.21.6 [[GH-19709](https://github.com/hashicorp/nomad/issues/19709)]

BUG FIXES:

* acl: Fixed auth method hashing which meant changing some fields would be silently ignored [[GH-19677](https://github.com/hashicorp/nomad/issues/19677)]
* auth: Added new optional OIDCDisableUserInfo setting for OIDC auth provider [[GH-19566](https://github.com/hashicorp/nomad/issues/19566)]
* core: Ensure job HCL submission data is persisted and restored during the FSM snapshot process [[GH-19605](https://github.com/hashicorp/nomad/issues/19605)]
* namespaces: Failed delete calls no longer return success codes [[GH-19483](https://github.com/hashicorp/nomad/issues/19483)]
* server: Fix server not waiting for workers to submit nacks for dequeued evaluations before shutting down [[GH-19560](https://github.com/hashicorp/nomad/issues/19560)]
* state: Fixed a bug where purged jobs would not get new deployments [[GH-19609](https://github.com/hashicorp/nomad/issues/19609)]

## 1.6.5 (December 13, 2023)

BUG FIXES:

* cli: Fix a bug in the `var put` command which prevented combining items as CLI arguments and other parameters as flags [[GH-19423](https://github.com/hashicorp/nomad/issues/19423)]
* client: remove incomplete allocation entries from client state database during client restarts [[GH-16638](https://github.com/hashicorp/nomad/issues/16638)]
* connect: Fixed a bug where deployments would not wait for Connect sidecar task health checks to pass [[GH-19334](https://github.com/hashicorp/nomad/issues/19334)]
* consul: uses token namespace to fetch policies for verification [[GH-18516](https://github.com/hashicorp/nomad/issues/18516)]
* csi: Added validation to `csi_plugin` blocks to prevent `stage_publish_base_dir` from being a subdirectory of `mount_dir` [[GH-19441](https://github.com/hashicorp/nomad/issues/19441)]
* metrics: Revert upgrade of `go-metrics` to fix an issue where metrics from dependencies, such as raft, were no longer emitted [[GH-19375](https://github.com/hashicorp/nomad/issues/19375)]

## 1.6.4 (December 07, 2023)

BREAKING CHANGES:

* core: Honor job's namespace when checking `distinct_hosts` feasibility [[GH-19004](https://github.com/hashicorp/nomad/issues/19004)]

SECURITY:

* build: Update to go1.21.4 to resolve Windows path validation CVE in Go [[GH-19013](https://github.com/hashicorp/nomad/issues/19013)]
* build: Update to go1.21.5 to resolve Windows path validation CVE in Go [[GH-19320](https://github.com/hashicorp/nomad/issues/19320)]

IMPROVEMENTS:

* cli: Add file prediction for operator raft/snapshot commands [[GH-18901](https://github.com/hashicorp/nomad/issues/18901)]
* ui: color-code node and server status cells [[GH-18318](https://github.com/hashicorp/nomad/issues/18318)]
* ui: show plan output warnings alongside placement failures and dry-run info when running a job through the web ui [[GH-19225](https://github.com/hashicorp/nomad/issues/19225)]

BUG FIXES:

* agent: Correct websocket status code handling [[GH-19172](https://github.com/hashicorp/nomad/issues/19172)]
* api: Fix panic in `Allocation.Stub` method when `Job` is unset [[GH-19115](https://github.com/hashicorp/nomad/issues/19115)]
* cli: Fixed a bug that caused the `nomad job restart` command to miscount the allocations to restart [[GH-19155](https://github.com/hashicorp/nomad/issues/19155)]
* cli: Fixed a panic when the `nomad job restart` command received an interrupt signal while waiting for an answer [[GH-19154](https://github.com/hashicorp/nomad/issues/19154)]
* cli: Fixed the `nomad job restart` command to create replacements for batch and system jobs and to prevent sysbatch jobs from being rescheduled since they never create replacements [[GH-19147](https://github.com/hashicorp/nomad/issues/19147)]
* client: Fixed a bug where client API calls would fail incorrectly with permission denied errors when using ACL tokens with dangling policies [[GH-18972](https://github.com/hashicorp/nomad/issues/18972)]
* core: Fix incorrect submit time for stopped jobs [[GH-18967](https://github.com/hashicorp/nomad/issues/18967)]
* ui: Fixed an issue where purging a job with a namespace did not process correctly [[GH-19139](https://github.com/hashicorp/nomad/issues/19139)]
* ui: fix an issue where starting a stopped job with default-less variables would not retain those variables when done via the job page start button in the web ui [[GH-19220](https://github.com/hashicorp/nomad/issues/19220)]
* ui: fix the job auto-linked variable path name when user lacks variable write permissions [[GH-18598](https://github.com/hashicorp/nomad/issues/18598)]
* variables: Fixed a bug where poststop tasks were not allowed access to Variables [[GH-19270](https://github.com/hashicorp/nomad/issues/19270)]
* vault: Fixed a bug where poststop tasks would not get a Vault token [[GH-19268](https://github.com/hashicorp/nomad/issues/19268)]
* vault: Fixed an issue that could cause Nomad to attempt to renew a Vault token that is already expired [[GH-18985](https://github.com/hashicorp/nomad/issues/18985)]

## 1.6.3 (October 30, 2023)

SECURITY:

* build: Update to Go 1.21.3 [[GH-18717](https://github.com/hashicorp/nomad/issues/18717)]

IMPROVEMENTS:

* agent: Added config option to enable file and line log detail [[GH-18768](https://github.com/hashicorp/nomad/issues/18768)]
* api: Added support for the `log_include_location` query parameter within the
`/v1/agent/monitor` HTTP endpoint [[GH-18795](https://github.com/hashicorp/nomad/issues/18795)]
* cli: Add `-prune` flag to `nomad operator force-leave` command [[GH-18463](https://github.com/hashicorp/nomad/issues/18463)]
* cli: Added `log-include-location` flag to the `monitor` command [[GH-18795](https://github.com/hashicorp/nomad/issues/18795)]
* cli: Added `log-include-location` flag to the `operator debug` command [[GH-18795](https://github.com/hashicorp/nomad/issues/18795)]
* csi: add ability to expand the size of volumes for plugins that support it [[GH-18359](https://github.com/hashicorp/nomad/issues/18359)]
* template: reduce memory usage associated with communicating with the Nomad API [[GH-18524](https://github.com/hashicorp/nomad/issues/18524)]
* ui: observe a token's roles' rules in the UI and add an interface for managing tokens, roles, and policies [[GH-17770](https://github.com/hashicorp/nomad/issues/17770)]

BUG FIXES:

* build: Add `timetzdata` Go build tag on Windows binaries to embed time zone data so periodic jobs are able to specify a time zone value on Windows environments [[GH-18676](https://github.com/hashicorp/nomad/issues/18676)]
* cli: Fixed an unexpected behavior of the `nomad acl token update` command that could cause a management token to be downgraded to client on update [[GH-18689](https://github.com/hashicorp/nomad/issues/18689)]
* cli: Use same offset when following single or multiple alloc logs [[GH-18604](https://github.com/hashicorp/nomad/issues/18604)]
* cli: ensure HCL env vars are added to the job submission object in the `job run` command [[GH-18832](https://github.com/hashicorp/nomad/issues/18832)]
* client: ensure null dynamic node metadata values are removed from memory [[GH-18664](https://github.com/hashicorp/nomad/issues/18664)]
* client: prevent tasks from starting without the prestart hooks running [[GH-18662](https://github.com/hashicorp/nomad/issues/18662)]
* metrics: Fixed a bug where CPU counters could report errors for negative values [[GH-18835](https://github.com/hashicorp/nomad/issues/18835)]
* scaling: Unblock blocking queries to /v1/job/{job-id}/scale if the job goes away [[GH-18637](https://github.com/hashicorp/nomad/issues/18637)]
* scheduler (Enterprise): auto-unblock evals with associated quotas when node resources are freed up [[GH-18838](https://github.com/hashicorp/nomad/issues/18838)]
* scheduler: Ensure duplicate allocation indexes are tracked and fixed when performing job updates [[GH-18873](https://github.com/hashicorp/nomad/issues/18873)]
* server: Fixed a bug where Raft server configuration parameters were not correctly merged [[GH-18494](https://github.com/hashicorp/nomad/issues/18494)]
* services: use interpolated address when performing nomad service health checks [[GH-18584](https://github.com/hashicorp/nomad/issues/18584)]
* ui: using start/stop from the job page in the UI will no longer fail when the job lacks HCL submission data [[GH-18621](https://github.com/hashicorp/nomad/issues/18621)]

## 1.6.2 (September 13, 2023)

IMPROVEMENTS:

* build: Update to Go 1.21.0 [[GH-18184](https://github.com/hashicorp/nomad/issues/18184)]
* cli: support wildcard namespaces in alloc subcommands when the `-job` flag is used [[GH-18095](https://github.com/hashicorp/nomad/issues/18095)]
* config: Added an option to configure how many historic versions of jobs are retained in the state store [[GH-17939](https://github.com/hashicorp/nomad/issues/17939)]
* consul/connect: Added support for `DestinationPeer`, `DestinationType`, `LocalBindSocketPath`, and `LocalBindSocketMode` in upstream block [[GH-16745](https://github.com/hashicorp/nomad/issues/16745)]
* jobspec: Add 'crons' field for multiple `cron` expressions [[GH-17858](https://github.com/hashicorp/nomad/issues/17858)]
* jobspec: Add new parameter `render_templates` for `restart` block to allow explicit re-render of templates on task restart. The default value is `false` and is fully backward compatible [[GH-18054](https://github.com/hashicorp/nomad/issues/18054)]
* jobspec: add `node_pool` as a valid field [[GH-18366](https://github.com/hashicorp/nomad/issues/18366)]
* raft: remove use of deprecated Leader func [[GH-18352](https://github.com/hashicorp/nomad/issues/18352)]
* status: go-getter failure reason now shown in `alloc status` [[GH-18444](https://github.com/hashicorp/nomad/issues/18444)]
* ui: Added configurable content security policy header [[GH-18085](https://github.com/hashicorp/nomad/issues/18085)]
* ui: adds a new Variables page to all job pages [[GH-17964](https://github.com/hashicorp/nomad/issues/17964)]
* ui: adds keyboard commands for pagination on lists using [[ and ]] [[GH-18210](https://github.com/hashicorp/nomad/issues/18210)]
* ui: sort variable key/values alphabetically by key when editing [[GH-18051](https://github.com/hashicorp/nomad/issues/18051)]
* ui: trim variable path names before saving [[GH-18198](https://github.com/hashicorp/nomad/issues/18198)]

BUG FIXES:

* acl: Fixed a bug where ACL tokens linked to ACL roles containing duplicate policies would cause erronous permission denined responses [[GH-18419](https://github.com/hashicorp/nomad/issues/18419)]
* cli: Add missing help message for the `-consul-namespace` flag in the `nomad job run` command [[GH-18081](https://github.com/hashicorp/nomad/issues/18081)]
* cli: Fix panic in `alloc logs` command when receiving empty stdout or stderr log frames [[GH-17815](https://github.com/hashicorp/nomad/issues/17815)]
* cli: Fixed a bug that prevented CSI volumes in namespaces other than `default` from being displayed in the `nomad node status -verbose` output [[GH-17925](https://github.com/hashicorp/nomad/issues/17925)]
* cli: Snapshot name is required in `volume snapshot create` command [[GH-17958](https://github.com/hashicorp/nomad/issues/17958)]
* client: Fixed a bug where the state of poststop tasks could be corrupted by client gc [[GH-17971](https://github.com/hashicorp/nomad/issues/17971)]
* client: Ignore stale server updates to prevent GCing allocations that should be running [[GH-18269](https://github.com/hashicorp/nomad/issues/18269)]
* client: return 404 instead of 500 when trying to access logs and files from allocations that have been garbage collected [[GH-18232](https://github.com/hashicorp/nomad/issues/18232)]
* core: Fixed a bug where exponential backoff could result in excessive CPU usage [[GH-18200](https://github.com/hashicorp/nomad/issues/18200)]
* csi: fixed a bug that could case a panic when deleting volumes [[GH-18234](https://github.com/hashicorp/nomad/issues/18234)]
* fingerprint: fix 'default' alias not being added to interface specified by network_interface [[GH-18096](https://github.com/hashicorp/nomad/issues/18096)]
* jobspec: Add diff for Task Group scaling block [[GH-18332](https://github.com/hashicorp/nomad/issues/18332)]
* migration: Fixed a bug where previous alloc logs were destroyed when migrating ephemeral_disk on the same client [[GH-18108](https://github.com/hashicorp/nomad/issues/18108)]
* scheduler: Fixed a bug where device IDs were not correctly filtered in constraints [[GH-18141](https://github.com/hashicorp/nomad/issues/18141)]
* services: Add validation message when `tls_skip_verify` is set to `true` on a Nomad service [[GH-18333](https://github.com/hashicorp/nomad/issues/18333)]
* ui: maintain HCL2 jobspec when using Start Job in the web ui [[GH-18120](https://github.com/hashicorp/nomad/issues/18120)]
* ui: search results are no longer overridden by sorting preferences on the jobs index page [[GH-18053](https://github.com/hashicorp/nomad/issues/18053)]

## 1.6.1 (July 21, 2023)

IMPROVEMENTS:

* cli: Display volume namespace on `nomad volume status` and `nomad node status` output [[GH-17911](https://github.com/hashicorp/nomad/issues/17911)]
* cpustats: Use config "cpu_total_compute" (if set) for all CPU statistics [[GH-17628](https://github.com/hashicorp/nomad/issues/17628)]
* metrics: Add `allocs.memory.max_allocated` to report the value of tasks' `memory_max` resource value [[GH-17938](https://github.com/hashicorp/nomad/issues/17938)]
* ui: added a button to copy variable path to clipboard [[GH-17935](https://github.com/hashicorp/nomad/issues/17935)]
* ui: adds a keyboard shortcut for Create Variable [[GH-17932](https://github.com/hashicorp/nomad/issues/17932)]
* ui: if a job is remotely purged while you're actively on it, it will let you know and re-route you to the index page [[GH-17915](https://github.com/hashicorp/nomad/issues/17915)]
* ui: indicate that nomad/jobs as a variable path is auto-accessible by all nomad jobs [[GH-17933](https://github.com/hashicorp/nomad/issues/17933)]

BUG FIXES:

* core: Fixed a bug where namespaces were not canonicalized on snapshot restore, resulting in potential nil access panic [[GH-18017](https://github.com/hashicorp/nomad/issues/18017)]
* csi: Fixed a bug in sending concurrent requests to CSI controller plugins by serializing them per plugin [[GH-17996](https://github.com/hashicorp/nomad/issues/17996)]
* csi: Fixed a bug where CSI controller requests could be sent to unhealthy plugins [[GH-17996](https://github.com/hashicorp/nomad/issues/17996)]
* csi: Fixed a bug where CSI controller requests could not be sent to controllers on nodes ineligible for scheduling [[GH-17996](https://github.com/hashicorp/nomad/issues/17996)]
* services: Fixed a bug that prevented passing query parameters in Nomad native service discovery HTTP health check paths [[GH-17936](https://github.com/hashicorp/nomad/issues/17936)]
* ui: Fixed a bug that could cause an error when accessing a region running versions of Nomad prior to 1.6.0 [[GH-18021](https://github.com/hashicorp/nomad/issues/18021)]
* ui: Fixed a bug that prevented nodes from being filtered by the "Ineligible" and "Draining" state filters [[GH-17940](https://github.com/hashicorp/nomad/issues/17940)]
* ui: Fixed error handling for cross-region requests when the receiving region does not implement the endpoint being requested [[GH-18020](https://github.com/hashicorp/nomad/issues/18020)]

## 1.6.0 (July 18, 2023)

FEATURES:

* **Node Pools**: Allow cluster operators to partition Nomad clients and control which jobs are allowed to run in each pool. [[GH-11041](https://github.com/hashicorp/nomad/issues/11041)]

BREAKING CHANGES:

* acl: Job evaluate endpoint now requires `submit-job` instead of `read-job` capability [[GH-16463](https://github.com/hashicorp/nomad/issues/16463)]

SECURITY:

* acl: Fixed a bug where a namespace ACL policy without label was applied to an unexpected namespace. [CVE-2023-3072](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2023-3072) [[GH-17908](https://github.com/hashicorp/nomad/issues/17908)]
* search: Fixed a bug where ACL did not filter plugin and variable names in search endpoint. [CVE-2023-3300](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2023-3300) [[GH-17906](https://github.com/hashicorp/nomad/issues/17906)]
* sentinel (Enterprise): Fixed a bug where ACL tokens could be exfiltrated via Sentinel logs [CVE-2023-3299](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2023-3299) [[GH-17907](https://github.com/hashicorp/nomad/issues/17907)]

IMPROVEMENTS:

* agent: Display server node ID in agent configuration at startup [[GH-17084](https://github.com/hashicorp/nomad/issues/17084)]
* api: enable support for storing original job source [[GH-16763](https://github.com/hashicorp/nomad/issues/16763)]
* api: return a structured error for unexpected responses [[GH-16743](https://github.com/hashicorp/nomad/issues/16743)]
* build: Publish official Docker images with the Nomad CLI [[GH-17017](https://github.com/hashicorp/nomad/issues/17017)]
* checks: Added support for Consul check field tls_server_name [[GH-17334](https://github.com/hashicorp/nomad/issues/17334)]
* cli: Add `-quiet` flag to `nomad var init` command [[GH-17526](https://github.com/hashicorp/nomad/issues/17526)]
* cli: Add check for missing host volume `path` in `nomad config validate` command [[GH-17393](https://github.com/hashicorp/nomad/issues/17393)]
* cli: Add leader status to output of `nomad server members -json` [[GH-17138](https://github.com/hashicorp/nomad/issues/17138)]
* cli: Add the ability to customize the details of the CA when running `nomad tls ca create` [[GH-17309](https://github.com/hashicorp/nomad/issues/17309)]
* cli: Sort output by Node name of the command `nomad operator raft list-peers` [[GH-16221](https://github.com/hashicorp/nomad/issues/16221)]
* cli: `job plan` help text for running the plan now includes the `-namespace` flag [[GH-16243](https://github.com/hashicorp/nomad/issues/16243)]
* client: check kernel module in `/sys/module` to help with WSL2 bridge networking [[GH-17306](https://github.com/hashicorp/nomad/issues/17306)]
* client: de-duplicate allocation client status updates and prevent allocation client status updates from being sent until clients have first synchronized with the server [[GH-17074](https://github.com/hashicorp/nomad/issues/17074)]
* client: prioritize allocation updates to reduce Raft and RPC load [[GH-17354](https://github.com/hashicorp/nomad/issues/17354)]
* cni: Ensure to setup CNI addresses in deterministic order [[GH-17766](https://github.com/hashicorp/nomad/issues/17766)]
* connect: Auto detect when to use podman for connect sidecar proxies [[GH-17065](https://github.com/hashicorp/nomad/issues/17065)]
* connect: do not restrict automatic envoy versioning to docker driver [[GH-17041](https://github.com/hashicorp/nomad/issues/17041)]
* connect: use full docker.io prefixed name for envoy image references [[GH-17045](https://github.com/hashicorp/nomad/issues/17045)]
* deploymentwatcher: Allow deployments to fail early when running out of reschedule attempts [[GH-17341](https://github.com/hashicorp/nomad/issues/17341)]
* deps: Updated Vault SDK to 0.9.0 [[GH-17281](https://github.com/hashicorp/nomad/issues/17281)]
* deps: Updated consul-template to v0.31.0 [[GH-16908](https://github.com/hashicorp/nomad/issues/16908)]
* deps: update docker to 23.0.3 [[GH-16862](https://github.com/hashicorp/nomad/issues/16862)]
* deps: update github.com/hashicorp/raft from 1.3.11 to 1.5.0 [[GH-17421](https://github.com/hashicorp/nomad/issues/17421)]
* deps: update go.etcd.io/bbolt from 1.3.6 to 1.3.7 [[GH-16228](https://github.com/hashicorp/nomad/issues/16228)]
* docker: Add `group_add` configuration [[GH-17313](https://github.com/hashicorp/nomad/issues/17313)]
* docker: Added option for labeling container with parent job ID of periodic/dispatch jobs [[GH-17843](https://github.com/hashicorp/nomad/issues/17843)]
* drivers: Add `DisableLogCollection` to task driver capabilities interface [[GH-17196](https://github.com/hashicorp/nomad/issues/17196)]
* metrics: add "total_ticks_count" counter for allocs/host CPU usage [[GH-17579](https://github.com/hashicorp/nomad/issues/17579)]
* runtime: Added 'os.build' attribute to node fingerprint on windows os [[GH-17576](https://github.com/hashicorp/nomad/issues/17576)]
* ui: Added a new Job Status Panel that helps show allocation status throughout a deployment and in steady state [[GH-16134](https://github.com/hashicorp/nomad/issues/16134)]
* ui: Adds a Download as .nomad.hcl button to jobspec editing in the UI [[GH-17752](https://github.com/hashicorp/nomad/issues/17752)]
* ui: Job status and deployment redesign [[GH-16932](https://github.com/hashicorp/nomad/issues/16932)]
* ui: Restyles "toast" notifications in the web UI with the Helios Design System [[GH-16099](https://github.com/hashicorp/nomad/issues/16099)]
* ui: add tooltips to the node and datacenter labels in the Topology page [[GH-17647](https://github.com/hashicorp/nomad/issues/17647)]
* ui: adds a toggle and localStorage property to Word Wrap logs and job definitions [[GH-17754](https://github.com/hashicorp/nomad/issues/17754)]
* ui: adds keyboard nav for switching between regions by pressing "r 1", "r 2", etc. [[GH-17169](https://github.com/hashicorp/nomad/issues/17169)]
* ui: affix page header to the top of the browser window to handle browser extension push-down gracefully [[GH-17783](https://github.com/hashicorp/nomad/issues/17783)]
* ui: change token input type from text to password [[GH-17345](https://github.com/hashicorp/nomad/issues/17345)]
* ui: remove namespace, type, and priority columns from child job table [[GH-17645](https://github.com/hashicorp/nomad/issues/17645)]
* vault: Add new configuration `disable_file` to prevent access to the Vault token by tasks that use `image` filesystem isolation [[GH-13343](https://github.com/hashicorp/nomad/issues/13343)]

DEPRECATIONS:

* envoy: remove support for envoy fallback image [[GH-17044](https://github.com/hashicorp/nomad/issues/17044)]

BUG FIXES:

* api: Fixed a bug that caused a panic when calling the `Jobs().Plan()` function with a job missing an ID [[GH-17689](https://github.com/hashicorp/nomad/issues/17689)]
* api: add missing constant for unknown allocation status [[GH-17726](https://github.com/hashicorp/nomad/issues/17726)]
* api: add missing field NetworkStatus for Allocation [[GH-17280](https://github.com/hashicorp/nomad/issues/17280)]
* cgroups: Fixed a bug removing all DevicesSets when alloc is created/removed [[GH-17535](https://github.com/hashicorp/nomad/issues/17535)]
* cli: Fix a panic in the `nomad job restart` command when monitoring replacement allocations [[GH-17346](https://github.com/hashicorp/nomad/issues/17346)]
* cli: Output error messages during deployment monitoring [[GH-17348](https://github.com/hashicorp/nomad/issues/17348)]
* client: Fixed a bug where Nomad incorrectly wrote to memory swappiness cgroup on old kernels [[GH-17625](https://github.com/hashicorp/nomad/issues/17625)]
* client: Fixed a bug where agent would panic during drain incurred by shutdown [[GH-17450](https://github.com/hashicorp/nomad/issues/17450)]
* client: fixed a bug that prevented Nomad from fingerprinting Consul 1.13.8 correctly [[GH-17349](https://github.com/hashicorp/nomad/issues/17349)]
* consul: Fixed a bug where Nomad would repeatedly try to revoke successfully revoked SI tokens [[GH-17847](https://github.com/hashicorp/nomad/issues/17847)]
* core: Fix panic around client deregistration and pending heartbeats [[GH-17316](https://github.com/hashicorp/nomad/issues/17316)]
* core: fixed a bug that caused job validation to fail when a task with `kill_timeout` was placed inside a group with `update.progress_deadline` set to 0 [[GH-17342](https://github.com/hashicorp/nomad/issues/17342)]
* csi: Fixed a bug where CSI volumes would fail to restore during client restarts [[GH-17840](https://github.com/hashicorp/nomad/issues/17840)]
* docker: Fixed a bug where network pause container would not be removed after node restart [[GH-17455](https://github.com/hashicorp/nomad/issues/17455)]
* drivers/docker: Fixed a bug where long-running docker operations would incorrectly timeout [[GH-17731](https://github.com/hashicorp/nomad/issues/17731)]
* identity: Fixed a bug where workload identities for periodic and dispatch jobs would not have access to their parent job's ACL policy [[GH-17018](https://github.com/hashicorp/nomad/issues/17018)]
* replication: Fix a potential panic when a non-authoritative region is upgraded and a server with the new version becomes the leader. [[GH-17476](https://github.com/hashicorp/nomad/issues/17476)]
* scheduler: Fixed a panic when a node has only one configured dynamic port [[GH-17619](https://github.com/hashicorp/nomad/issues/17619)]
* tls: Fixed a bug where the `nomad tls cert` command did not create certificates with the correct SANs for them to work with non default domain and region names. [[GH-16959](https://github.com/hashicorp/nomad/issues/16959)]
* ui: dont show a service as healthy when its parent allocation stops running [[GH-17465](https://github.com/hashicorp/nomad/issues/17465)]
* ui: fix a mirage-only issue where our mock token logs repeated unnecessarily [[GH-17010](https://github.com/hashicorp/nomad/issues/17010)]
* ui: fixed a handful of UX-related bugs during variable editing [[GH-17319](https://github.com/hashicorp/nomad/issues/17319)]
* ui: fixes an issue where the allocations table on child (periodic, parameterized) job pages wouldn't update when accessed via their parent [[GH-17214](https://github.com/hashicorp/nomad/issues/17214)]
* ui: preserve newlines when displaying shown variables in non-json mode [[GH-17343](https://github.com/hashicorp/nomad/issues/17343)]

## Unsupported Versions

Versions of Nomad before 1.6.0 are no longer supported. See [CHANGELOG-unsupported.md](./CHANGELOG-unsupported.md) for their changelogs.
