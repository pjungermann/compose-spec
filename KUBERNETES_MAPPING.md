# Compose - Kubernetes mapping

This document outlines mapping between `docker-compose.yaml` files and corresponding Kubernetes equivalents. 

__Glossary:__

- __✓:__ Converts
- __n:__ Not yet implemented
- __x:__ Not applicable / no 1-1 conversion

| Keys                           | Map|  Kubernetes                                                  | Notes                                                                                                          |
|--------------------------------|----|--------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
| __Service__                    | ✓  |                                                              |                                                                                                                |
| service.service.build          | ✓  |                                                              | Builds/Pushes to Docker repository. See `--build` parameter                                                    |
| service.build.context          | ✓  |                                                              |                                                                                                                |
| service.build.dockerfile       | ✓  |                                                              |                                                                                                                |
| service.build.args             | n  |                                                              |                                                                                                                |
| service.build.cache_from       | n  |                                                              |                                                                                                                |
| service.cap_add, cap_drop      | ✓  |  Pod.Spec.Container.SecurityContext.Capabilities.Add/Drop    |                                                                                                                |
| service.command                | ✓  |  Pod.Spec.Container.Command                                  |                                                                                                                |
| service.configs                | ✓  |                                                              |                                                                                                                |
| service.cgroup_parent          | x  |                                                              | Not supported within Kubernetes. See issue https://github.com/kubernetes/kubernetes/issues/11986               |
| service.container_name         | ✓  |  Metadata.Name + Deployment.Spec.Containers.Name             |                                                                                                                |
| service.credential_spec        | x  |                                                              | Only applicable to Windows containers                                                                          |
| service.deploy                 | ✓  |                                                              |                                                                                                                |
| service.deploy.mode            | ✓  |                                                              |                                                                                                                |
| service.deploy.replicas        | ✓  |  Deployment.Spec.Replicas / DeploymentConfig.Spec.Replicas   |                                                                                                                |
| service.deploy.placement       | ✓  |  Pod.Spec.NodeSelector                                       |                                                                                                                |
| service.deploy.update_config   | ✓  |  Workload.Spec.Strategy                                      | Deployment / DeploymentConfig                                                                                  |
| service.deploy.resources       | ✓  |  Containers.Resources.Limits.Memory / Containers.Resources.Limits.CPU | Support for memory as well as cpu                                                                     |
| service.deploy.restart_policy  | ✓  |  Pod generation                                              | This generated a Pod, see the [user guide on restart](http://kompose.io/user-guide/#restart)                   |
| service.deploy.labels          | ✓  |  Workload.Metadata.Labels                                    | Only applied to workload resource                       |                                                      |
| service.devices                | x  |                                                              | Not supported within Kubernetes, See issue https://github.com/kubernetes/kubernetes/issues/5607                |
| service.depends_on             | x  |                                                              |                                                                                                                |
| service.dns                    | x  |                                                              | Not used within Kubernetes. Kubernetes uses a managed DNS server                                               |
| service.dns_search             | x  |                                                              | See `dns` key                                                                                                  |
| service.domainname             | ✓  |  Pod.Spec.SubDomain                                          |                                                                                                                | 
| service.tmpfs                  | ✓  |  Pod.Spec.Containers.Volumes.EmptyDir                        | Creates emptyDirvolume with medium set to Memory & mounts given directory inside container                     |
| service.entrypoint             | ✓  |  Pod.Spec.Container.Command                                  | Same as command                                                                                                |
| service.env_file               | ✓  |                                                              |                                                                                                                |
| service.environment            | ✓  |  Pod.Spec.Container.Env                                      |                                                                                                                |
| service.expose                 | ✓  |  Service.Spec.Ports                                                                                                                                                           | 
| service.endpoint_mode          | ✓  |                                                              | If endpoint_mode=vip, the created Service will be forced to set to NodePort type                               |
| service.extends                | ✓  |                                                              | Extends by utilizing the same image supplied                                                                   |
| service.external_links         | x  |                                                              | Kubernetes uses a flat-structure for all containers and thus external_links does not have a 1-1 conversion     |
| service.extra_hosts            | n  |                                                              |                                                                                                                |
| service.group_add              | ✓  |                                                              |                                                                                                                |
| service.healthcheck            | ✓  |                                                              |                                                                                                                |
| service.hostname               | ✓  |  Pod.Spec.HostName                                           |                                                                                                                |
| service.image                  | ✓  |  Deployment.Spec.Containers.Image                            |                                                                                                                |
| service.isolation              | x  |                                                              | Not applicable as this applies to Windows with HyperV support                                                  |
| service.labels                 | ✓  |  Metadata.Annotations                                        |                                                                                                                |
| service.links                  | x  |                                                              | All containers in the same pod are accessible in Kubernetes                                                    |
| service.logging                | x  |                                                              | Kubernetes has built-in logging support at the node-level                                                      |
| service.network_mode           | x  |                                                              | Kubernetes uses its own cluster networking                                                                     |
| service.networks               | ✓  |                                                              | See `networks` key                                                                                             |
| service.networks.aliases       | x  |                                                              | See `networks` key                                                                                             |
| service.networks.addresses     | x  |                                                              | See `networks` key                                                                                             |
| service.pid                    | ✓  |  Pod.Spec.HostPID                                            |                                                                                                                |
| service.ports                  | ✓  |  See #ports-exposure                                                |                                                                                                         |
| service.secrets                | ✓  |  See #secrets                                                |                                                                                                                |
| service.security_opt           | x  |                                                              | Kubernetes uses its own container naming scheme                                                                |
| service.stop_grace_period      | ✓  |  Pod.Spec.TerminationGracePeriodSeconds                      |                                                                                                                |
| service.stop_signal            | x  |                                                              | Not supported within Kubernetes. See issue https://github.com/kubernetes/kubernetes/issues/30051               |
| service.sysctls                | n  |                                                              |                                                                                                                |
| service.ulimits                | x  |                                                              | Not supported within Kubernetes. See issue https://github.com/kubernetes/kubernetes/issues/3595                |
| service.userns_mode            | x  |                                                              | Not supported within Kubernetes and ignored in Docker Compose Version 3                                        |
| service.volumes                | ✓  |  See #persistent-volume-claim                                | Creates a PersistentVolumeClaim. Can only be created if there is already a PersistentVolume within the cluster |
| service.restart                | ✓  |                                                              |                                                                                                                |
|                                |    |                                                              |                                                                                                                |
| __Volume__                     | x  |                                                              |                                                                                                                |
| driver                         | x  |                                                              |                                                                                                                |
| driver_opts                    | x  |                                                              |                                                                                                                |
| external                       | x  |                                                              |                                                                                                                |
| labels                         | x  |                                                              |                                                                                                                |
|                                |    |                                                              |                                                                                                                |
| __Network__                    | x  |                                                              |                                                                                                                |
| driver                         | x  |                                                              |                                                                                                                |
| driver_opts                    | x  |                                                              |                                                                                                                |
| enable_ipv6                    | x  |                                                              |                                                                                                                |
| ipam                           | x  |                                                              |                                                                                                                |
| internal                       | x  |                                                              |                                                                                                                |
| labels                         | x  |                                                              |                                                                                                                |
| external                       | x  |                                                              |                                                                                                                |
|                                |    |                                                              |                                                                                                                |
| __Secret__                     | x  |                                                              |                                                                                                                |
| TBD                            | x  |                                                              |                                                                                                                |
|                                |    |                                                              |                                                                                                                |
| __Config__                     | x  |                                                              |                                                                                                                |
| TBD                            | x  |                                                              |                                                                                                                |
|                                |    |                                                              |                                                                                                                |
              
## Persistent Volumes

service.volumes items will be mapped to a Persistent Volume Claim. 
Such a volume can be bind-mounted, or can be a named volume (described separately in the "volumes" section of the compose file).

Bind-mounted volumes are mapped to hostPath mounts in Compose on Kubernetes. in Kompose, they are mapped to a PVC but there is no link to the actual path defined as source. 
 
Named volumes are mapped to a Persistent Volume Claim, with a unique generated name used to reference the PVC from the deployment file.

| Compose                        |  Kubernetes                                                  | Notes                                                                       |
|--------------------------------|--------------------------------------------------------------|-----------------------------------------------------------------------------|
| unique volume name             | Deployment.Spec.Containers.VolumeMounts.name, Deployment.Spec.Volumes.Name, Deployment.Spec.Volumes.PersistentVolumeClaim.name, PersistentVolumeClaim.metadata.name  |  | 
| source                         | used to derive the unique volume name in Kubernetes          | 
| target                         | Deployment.Spec.Containers.VolumeMounts.mountPath            |                                                                             |
| read_only                      | Deployment.Spec.Containers.VolumeMounts.readOnly (false by default) |                                                                             | 

Note : Compose on Kube also defines Kubernetes service as a StatefuleSet if it uses a volume. This changes the way the service is linked to the PVC. There are various options where impacts need to be detailed for a well defined mapping of compose volumes to Kubernetes PV.
 
Note : Compose on Kubernetes currently uses the volume name provided in the compose file, and does not try to transform it. This name must be a valid volume name in Kubernetes. Kompose performs some transformations if necessary to make the name compliant (for example, transforms "test_volume" to "test-volume").

## Secrets

Secrets in Swarm are a simple key-value pair. In Kubernetes, a secret has a name and then a map of keys to values. 
Compose file secrets transform into a Kubernetes secret with only one key. 
The name of the compose secret (eg. `mysecret`) is used for the mount path of the Kubernetes secret : mountPath: `/run/secrets/mysecret`. The secret single key will be mapped to a file with the same name `mysecret`.  

Within the pod, one can access the value of the secret with:  
`# cat /run/secrets/mysecret/mysecret`.

If the secret is defined in the compose file as a file secret, then it is mapped to a secret in the Kubernetes cluster (created or updated during application deployment). This new secret will be named after the  named with the same secret name. 
This new Kubernetes secret includes a single key and the value is taken from the local file when deploying the compose file.

Note : currently Compose on Kubernetes uses Kubernetes volume subPath to make the secret available with `# cat /run/secrets/mysecret` (one less subfolder). However, using subPath will prevent pods from receving updates on the secret, so it's probably better to not continue this way for the mapping definition. 

## Port Exposure

compose services exposing ports will be mapped to a Kubernetes service associated with the related pod.

| Compose                        |  Kubernetes                                                  | Notes                                                                       |
|--------------------------------|--------------------------------------------------------------|-----------------------------------------------------------------------------|
| ports.source                   | Deployment.Spec.Containers.port.containerPort, Service.specs.ports.port          | 
| ports.target                   | Service.specs.ports.targetPort                               |                                                                             |

Compose on Kubernetes automatically creates a LoadBalanced typed service, in order to expose the port to the outside world. 
Kompose by default creates a ClusterId service, this can be changed if we use specific labels in the compose file to create a LoadBalanced service. 

Additionally, for pods that don't expose ports, Compose on Kubernetes will automatically create a Kubernetes Service (type ClusterIP) so that various pods can discover each other and communicate using the service names. 
Kompose requires that compose service expose ports for the creation of a service that will make pods discoverable within the cluster.  