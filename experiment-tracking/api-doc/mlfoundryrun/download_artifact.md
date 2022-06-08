# download\_artifact

Download recursively an artifact file or directory from a run to a local directory if applicable, and return a local path for it

| Parameters  | Description                                                                                                                                              |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| path | <p>(str) Relative source path to the desired artifact |
| dest_path | <p>(str) Absolute path of the local filesystem destination directory to which to download the specified artifacts. This directory must already exist. If unspecified, the artifacts will either be downloaded to a new uniquely-named directory on the local filesystem or will be returned directly in the case of the LocalArtifactRepository |

### Returns

Local path of desired artifact.
