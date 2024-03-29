# Adding version control integration

Adding version control services like Github and Bitbucket to truefoundry installation allows a user to directly access public/private repositories of the linked account for deployment.
To allow these integrations in your app, following steps need to be followed.

## Github
To enable Github integration in your app, you need to create a github app using your github account. This github app can then be installed on the users's account through our platform.

Follow these steps to create the github app and integrate it with truefoundry:
- In Github, select your avatar (Your profile and settings) from the navigation bar at the top of the screen. If you want to create app for personal account, select Settings. If you want to create it for an organization, select Your organizations > Settings.
- On the sidebar, select Developer Settings.
- On the sidebar, select Github apps.
- Click the New Github App button and add the following settings:
    - GitHub App name : Truefoundry-Example
    - Homepage URL : https://app.example-org.truefoundry.com
    
    ![Github app settings](../assets/vcs-integration-github-settings-1.png)

    - Identifying and authorizing users 
        - Callback URL : https://app.example-org.truefoundry.com/api/svc/v1/vcs/github/callback
        - Check : Expire user authorization tokens
        - Check : Request user authorization (OAuth) during installation

    ![Github app settings](../assets/vcs-integration-github-settings-2.png)

    - Post installation
        - Check : Redirect on update
    - Webhook
        - Check : Active
        - Webhook URL : https://app.example-org.truefoundry.com/api/svc/v1/vcs/github/webhook-callback

    ![Github app settings](../assets/vcs-integration-github-settings-3.png)

    - Permissions
        - Repository Permissions:
            - Administration: read and write
            - Commit statuses : read and write
            - Contents : read and write
            - Metadata : read-only
            - Pull request: read and write
            - Webhooks: rad and write

            ![Github app permissions](../assets/vcs-integration-github-settings-4.png)

    - Where can this GitHub App be installed? : Any account

    ![Github app settings](../assets/vcs-integration-github-settings-5.png)

- Once the app is created, store the GitHub App name and App id.

![Github app properties](../assets/vcs-integration-github-settings-6.png)

- Navigate to Private Keys section and generate a private key. A PEM file containing private key will be downloaded.

![Github app private key](../assets/vcs-integration-github-settings-7.png)

- Base 64 encode the entire content of the file. Please make sure to omit any whitelines before or after the text in the file. To base64 encode, use following commands on the command line:
    - Linux : `base64 private_key.pem > private_key.b64`
    - Windows : `certutil -encode private_key.pem tmp.b64 && findstr /v /c:- tmp.b64 > private_key.b64 && del tmp.b64`
    - Mac : `base64 -i private_key.pem -o private_key.b64`
Please make sure to omit any whitelines before or after the text in the file. Store the file content of `private_key.b64` as private_key.
- Set environment variables using the [docs](https://docs.truefoundry.com/documentation/deploying-on-your-own-cloud/production-installation) :
    - GITHUB_INSTALLATION_URL=https://github.com/apps/${app_name}/installations/new
    - GITHUB_PRIVATE_KEY=${private_key}
    - GITHUB_APP_ID=${app_id}


## Bitbucket
To enable Bitbucket integration in your app, you need to create a Bitbucket consumer using your Bitbucket account. This consumer can then be added to the users's account through our platform.
Follow these steps to create the Bitbucket app and integrate it with truefoundry:
- In Bitbucket, Select your avatar (Your profile and settings) from the navigation bar at the top of the screen. Under Recent workspaces, select a workspace.
- On the sidebar, select Settings to open the Workspace settings.
- On the sidebar, under Apps and features, select OAuth consumers.
- Click the Add consumer button. Add following settings:
    - Name : Truefoundry-Example
    - Callback URL : https://app.example-org.truefoundry.com/api/svc/v1/vcs/bitbucket/callback
    - URL : https://app.example-org.truefoundry.com

    ![Bitbucket app settings](../assets/vcs-integration-bitbucket-settings-1.png)

    - Permissions: 
        - Account : email, read
        - Repositories : read, write
        - Workspace membership : read
        - Pull requests : read, write

    ![Bitbucket app permissions](../assets/vcs-integration-bitbucket-settings-2.png)

- Once the consumer is created, click on the consumer and store the key and secret for later use.

![Bitbucket app properties](../assets/vcs-integration-bitbucket-settings-3.png)

- Set environment variables using the [docs](https://docs.truefoundry.com/documentation/deploying-on-your-own-cloud/production-installation) :
    - BITBUCKET_CLIENT_ID=${key}
    - BITBUCKET_CLIENT_SECRET=${secret}
