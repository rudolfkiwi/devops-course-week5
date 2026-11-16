# kubectl-stat Plugin

A simple Kubernetes CLI plugin that retrieves CPU and memory usage statistics for Kubernetes resources using `kubectl top`.

The plugin displays resource usage in a comma-separated format, including resource type, namespace, resource name, CPU, and memory.

## Prerequisites

- Kubernetes cluster with Metrics Server installed and running
- `kubectl` installed and configured to access the cluster

## Installation

1. Download or clone the repository containing the plugin script.

2. Rename the script to `kubectl-stat` if it has a different name.

   ```bash
   mv kubeplugin kubectl-stat
   ```

3. Make the script executable:

   ```bash
   chmod +x kubectl-stat
   ```

4. Move the script to a directory included in your `PATH`:

   ```bash
   sudo mv kubectl-stat /usr/local/bin/
   ```

5. Verify that Kubernetes recognizes the plugin:

   ```bash
   kubectl plugin list
   ```

   The output should include `kubectl-stat`.

## Usage

**Syntax:**

```bash
kubectl stat <namespace> <resource>
```

**Example:**

Retrieve CPU and memory usage for Pods in the `default` namespace:

```bash
kubectl stat default pods
```

Expected output:

```text
pods, default, nginx-abc123, 10m, 50Mi
pods, default, redis-xyz456, 25m, 120Mi
```

### Output format

The plugin displays the fields for pods and nodes in a format of 'kubectl top' command output

### Error handling

Both namespace and resource type are required.

If the namespace is missing:

```bash
kubectl stat
```

Output:

```text
Namespace is not provided!
Usage: kubectl stat <namespace> <resource>
```

If the resource type is missing:

```bash
kubectl stat default
```

Output:

```text
Resource is not provided!
Usage: kubectl stat <namespace> <resource>
```

## Limitations

- Requires a functional Kubernetes Metrics API.
- Does not display metrics if they are unavailable from the Kubernetes Metrics API.



## Usage Examples

### 1. Missing Namespace Argument

Running the plugin without specifying a namespace displays an error and usage instructions.

```bash
kubectl stat
```

**Output:**

```text
Namespace is not provided!
Usage: kubectl stat <namespace> <resource>
```

### 2. Node Resource Statistics

Retrieve CPU and memory usage statistics for Kubernetes Nodes.

```bash
kubectl stat default node
```

**Output:**

```text
node, k3d-argo-server-0, 292m, 3%, 3919Mi, 49%
```

Output format: `Resource, Name, CPU, CPU%, Memory, Memory%`

### 3. Pod Resource Statistics

Retrieve CPU and memory usage statistics for Pods in the `default` namespace.

```bash
kubectl stat default pod
```

**Output:**

```text
pod, default, hostnames-57b77f457d-2nhqc, 0m, 6Mi
pod, default, hostnames-57b77f457d-dqrd4, 0m, 5Mi
pod, default, hostnames-57b77f457d-n7qwf, 0m, 5Mi
```
