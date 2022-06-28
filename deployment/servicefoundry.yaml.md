# `servicefoundry.yaml` Reference

* [servicefoundry.yaml Reference](#servicefoundryyaml-reference)
   * [version](#version)
   * [build](#build)
      * [sfy_build_pack](#sfy_build_pack)
      * [options](#options)
   * [service](#service)
      * [name](#name)
      * [workspace](#workspace)
      * [cpu](#cpu)
      * [memory](#memory)
      * [replicas](#replicas)
      * [ports](#ports)
      * [env](#env)
      * [inject_truefoundry_api_key](#inject_truefoundry_api_key)
      * [command](#command)
      * [args](#args)
* [Examples](#examples)

## `version`

**type: `string`**

**required: false**

**default: `"v1"`**

The servicefoundry definition version.

**Example:**

```yaml
version: v1
build:
  ...
service:
  ...
```

## `build`

**type: `object`**

**required: false**

**default: TODO**

The build section specifies how to package your code and generate a container image out of it.

### `sfy_build_pack`

**type: `string`**

**required: true**

TODO

**Example:**

```yaml
build:
  ...
```

### `options`

**type: `object`**

**required: false**

**default: `{}`**

TODO

**Example:**

```yaml
build:
  ...
```

## `service`

The service section defines parameters for deploying the container image built from your code. You can configure name, resource requirements and limits, ports to expose, environment variables, number of replicas.

### `name`

**type: `string`**

**required: true**

The name of the service. Must be minimum 5 characters and maximum 60 characters. It can only contain alphanumeric characters (`A-Za-z0-9`) and `-` (hypen)

**Example:**

```yaml
service:
  name: my-service-1
  ...
```

### `workspace`

**type: `string`**

**required: true**

The Fully Qualified Name (FQN) of the TrueFoundry workspace to deploy the service to. You can find your workspaces at https://app.truefoundry.com/workspace 

**Example:**

```yaml
service:
  workspace: v1:tfy-dub-euwe1-production:my-workspace-1
  ...
```

### `cpu`

**type: `object`**

**required: true**

The amount of cpu to assign to 1 replica of the service. The amount is specified in the number of virtual cores. It can be fractional but the minimum allowed value is 0.001. See [Kubernetes - CPU units](https://kubernetes.io/docs/tasks/configure-pod-container/assign-cpu-resource/#cpu-units) for additional reference

The object has support for two keys:

**`required`**: The minimum amount of CPU required for each replica. A replica cannot be started if the available amount of available CPU resources is below this.

**`limit`**: The maximum amount of CPU allowed for each replica.

At least one of these must be specified. See [Kubernetes - Specify a CPU request and a CPU limit](https://kubernetes.io/docs/tasks/configure-pod-container/assign-cpu-resource/#specify-a-cpu-request-and-a-cpu-limit) for additional reference.

**Example:**

```yaml
service:
  cpu:
    required: 0.5
    limit: 1.5
  ...
```

### `memory`

**type: `object`**

**required: true**

TODO

**Example:**

```yaml
service:
  memory:
    required: 500000000
    limit: 1500000000
```

### `replicas`

**type: `integer`**

**required: false**

**default**: `1`

The number of replicas for the service. All requests will be evenly distributed among available replicas. More than `1` replica ensures fault tolerance and availability when updating a service with new code.

**Example:**

```yaml
service:
  replicas: 2
```

### `ports`

**type: `array`**

**required: false**

**default**: `[]`

TODO

**Example:**

```yaml
service:
  ports:
    - 8080
    - container_port: 8000
      protocol: TCP
```

### `env`

**type: `array`**

**required: false**

**default**: `[]`

TODO

**Example:**

```yaml
service:
  env:
    - name: MY_ENV_1
      value: MY_VALUE_1
    - name: MY_ENV_2
      value: MY_VALUE_2
```

### `inject_truefoundry_api_key`

**type: `boolean`**

**required: false**

**default**: `true`

When set to `true` this adds `TFY_API_KEY` to the environment in the container. This can be useful to interact with TrueFoundry APIs.

**Example:**

```yaml
service:
  inject_truefoundry_api_key: true
```

### `command`

**type: `string | array`**

**required: false**

**default**: `[]`

TODO

**Example:**

**Example:**

`string` value

```yaml
service:
  command: python /app/my_script.py
```

`array` value

```yaml
service:
  command:
    - python
    - /app/my_script.py
```

### `args`

**type: `string | array`**

**required: false**

**default**: `[]`

TODO

**Example:**

`string` value

```yaml
service:
  args: arg1 arg2
```

`array` value

```yaml
service:
  args:
    - arg1
    - arg2
```

# Examples

```yaml
version: v1
build:
  sfy_build_pack: TODO
  options: TODO
service:
  name: my-service-1
  cpu:
  	required: 0.5
  	limit: 1.5
  memory:
    required: 500000000
    limit: 1500000000
  replicas: 2
  env:
    - name: MY_ENV_1
      value: MY_VALUE_1
    - name: MY_ENV_2
      value: MY_VALUE_2
  inject_truefoundry_api_key: true
  ports:
    - 8080
    - container_port: 8000
      protocol: TCP
  command: python /app/my_script.py
  args:
    - arg1
    - arg2
  workspace: v1:tfy-dub-euwe1-production:my-workspace-1
```

