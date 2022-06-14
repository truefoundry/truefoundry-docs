### Setting up MLFoundry

You can install and login to mlfoundry on your device by following the steps below.

1. [Sign-up](https://app.truefoundry.com/signup) and login into your account.

2. Install mlfoundry library on your system.

3. Login to the mlfoundry library on your system. The CLI will prompt for an API key. Find your API key [here](https://app.truefoundry.com/settings).

```
pip install mlfoundry
mlfoundry login
```
On a notebook environment you can use the `login` function.

```python
import mlfoundry
mlfoundry.login()
```

#### Can I force relogin if I am already logged in on my device?

Yes. If you want to overwrite the existing API key, use the `--relogin` flag with the CLI command.

```
mlfoundry login --relogin
```
If you are on notebook,

```python
import mlfoundry
mlfoundry.login(relogin=True)
```

#### How can I login to mlfoundry on a device in a non-interactive mode?

If you are running automated training jobs, you can use the `MLF_API_KEY` environment variable to set the API key. You do not need to use the `mlfoundry login` command if you are using the environment variable.

The API key passed via the environment variable takes precedence over the API key passed via the `login` command.

```
export MLF_API_KEY="your-api-key"
```

#### Troubleshooting for Mac OS users with M1 processors

Users can face issues while installing MLFoundry packages on Mac OS computers with M1 chip. Here are a few links that will help you troubleshoot.

* Error in installing numpy. Follow this [link](https://stackoverflow.com/questions/65336789/numpy-build-fail-in-m1-big-sur-11-1) to fix the issue.

* While installing MLFoundry user might face the following error.
    ```
    SetuptoolsDeprecationWarning: setuptools.installer is deprecated. Requirements should be satisfied by a PEP 517 installer.
    ```
    Execute the following command and try installing MLFoundry again.
    ```
    SYSTEM_VERSION_COMPAT=1 pip install mlfoundry
    ```

    If the error still persists, follow the [link](https://stackoverflow.com/questions/64038673/could-not-build-wheels-for-which-use-pep-517-and-cannot-be-installed-directly) to solve the issue.
