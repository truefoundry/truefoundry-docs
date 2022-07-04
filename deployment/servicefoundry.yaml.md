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

**default: `{}`**

The build section specifies how to package your code and generate a container image out of it. When not specificied the `build` command automatically tries to detect the correct build pack to use based on the files in your code repository.

### `sfy_build_pack`

**type: `string`**

**required: false**

**default: `sfy_build_pack_python`**

The name of the servicefoundry build pack to use. Should be one of the following:

- `sfy_build_pack_docker`
- `sfy_build_pack_python`

- `sfy_build_pack_fallback`

**Example:**

```yaml
build:
  sfy_build_pack: sfy_build_pack_python
  ...
```

### `options`

**type: `object`**

**required: false**

**default: `{}`**

A set of options for the selected `sfy_build_pack`. For e.g. for `sfy_build_pack_python` one of the options is `python_version` indicating the python version to use as the base.

**Example:**

```yaml
build:
  options:
    python_version: python:3.9
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

The amount of memory (in bytes) to assign to 1 replica of the service. It can only be a positive integer. 

The object has support for two keys:

**`required`**: The minimum amount of memory required for each replica. A replica cannot be started if the available amount of available memory resource is below this.

**`limit`**: The maximum amount of memory consumption allowed for each replica.

**Example:**

```yaml
service:
  memory:
    required: 500000000
    limit: 1500000000
  ...
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
  ...
```

### `ports`

**type: `array`**

**required: false**

**default**: `[]`

The list of ports to expose for the service. The values can be either 

- `integer` E.g. `8080`

- `string` format as `{port_number}/{protocol}` E.g. `"8080/TCP"`

- `object` with keys `container_port` and `protocol` E.g.

  ```yaml
  container_port: 8000
  protocol: TCP
  ```

  when `protocol` is not specified, it is set to `TCP` by default

**Example:**

```yaml
service:
  ports:
    - 8080
    - container_port: 8000
      protocol: TCP
  ...
```

### `env`

**type: `array`**

**required: false**

**default**: `[]`

A list of environment variables to add in the service. Each item in the array is an `object` with keys `name` and `value`

**Example:**

```yaml
service:
  env:
    - name: MY_ENV_1
      value: MY_VALUE_1
    - name: MY_ENV_2
      value: MY_VALUE_2
  ...
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
  ...
```

### `command`

**type: `string | array`**

**required: false**

**default**: `[]`

The command to run when your container is started. You can override the default command by specifying a different command here. When not specified, the default command of the container image/build pack is used. This is equivalent to `ENTRYPOINT` in docker.

**Example:**

`string` value

```yaml
service:
  command: python /app/my_script.py
  ...
```

`array` value

```yaml
service:
  command:
    - python
    - /app/my_script.py
  ...
```

### `args`

**type: `string | array`**

**required: false**

**default**: `[]`

The list of arguments to pass to the `command` of the container image.You can override the default arguments by specifying a different arguments here.

This is equivalent to `CMD` in docker.

If you define args, but do not define a command, the default command is used with your new arguments.

**Example:**

`string` value

```yaml
service:
  args: arg1 arg2
  ...
```

`array` value

```yaml
service:
  args:
    - arg1
    - arg2
  ...
```

# Examples

```yaml
version: v1
build:
  sfy_build_pack: sfy_build_pack_python
  options:
    python_version: python:3.9
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

