# Repository Management

> [!WARNING]
> The following features are experimental and may cause unexpected behavior. Use them carefully and report any issues you encounter.

The **Repository Management** page under **Settings** allows you to configure various ways to manage your wiki repository, including HTTP access for users, automated pushes on every wiki change, and automatic pulls when external changes occur.


## Git Web Server

Enabling Git Web Server allows users to access wiki contents as a regular git repository via HTTP. Users with `READ` permission can pull changes, while users with `UPLOAD` permission can push changes as well.


## Pushing to and Pulling from SSH Remote

You can enable automatic pushing to or pulling from a pre-defined git remote whenever changes occur on either side. Both settings are not connected to each other in any way and only one or both could be enabled when needed.

In both cases, you must define the git remote address at minimum. Note that only SSH remotes are currently supported.

For authorization, An Otter Wiki supports SSH keys as the most secure and straightforward method. Alternatively, you can configure authorization externally (on your host system if running An Otter Wiki natively, or inside the container), make sure that `ssh` command can authenticate with the remote server.

When setting up automated pulls, note the generated webhook URL - it's unique to each remote URL. If you change your remote URL, get the new webhook URL as well.

Both push and pull configurations add buttons at the bottom right of the page, allowing you to test your settings.

[![](./repository-management-actions.png?thumbnail=400)](./repository-management-actions.png)

### GitHub Example

This example shows how to set up automatic pushing and pulling between your wiki and a GitHub repository `user/wiki`, including SSH key authorization.

First, get the remote URL from GitHub, for this example it will be `git@github.com:user/wiki.git`.

#### Authorization with SSH Key

1. Generate a new SSH key using `ssh-keygen -t ed25519 -f mywiki`
2. You'll get two files after that: `mywiki` (private key) and `mywiki.pub` (public key)
3. Go to your GitHub repository > **Settings** > **Deploy keys** > **Add deploy key**
4. Paste your public key (`cat mywiki.pub`) into the **Key** field
5. Check **Allow write access** if you plan to enable automatic pushes
6. Click **Add key** to save

[![](./github-add-deploy-key.png?thumbnail=640)](./github-add-deploy-key.png)

> [!TIP]
> For security reasons, you can safely delete the local key files (`mywiki` and `mywiki.pub`) after completing this setup, since An Otter Wiki stores the private key and GitHub stores the public key.

#### Automatic Pushing

1. Check **Enable pushing to SSH remote**
2. Paste your GitHub remote URL in the **SSH Remote URL** field
3. Paste your private key (`cat mywiki`) in the **SSH Private Key (Optional)** field
4. Click **Save Preferences**
5. Click **Push** in the bottom right to test pushing existing changes to the remote
6. If successful, try editing or creating a page in your wiki - changes should immediately appear on GitHub

#### Automatic Pulling

1. Check **Enable pulling from SSH remote**
2. Paste your GitHub remote URL in the **SSH Remote URL** field
3. Paste your private key (`cat mywiki`) in the **SSH Private Key (Optional)** field
4. Click **Save Preferences**
5. Click **Pull** in the bottom right to test pulling existing changes from the remote
6. If successful, copy the webhook URL from the **Pull webhook URL** field
7. Go to your GitHub repository > **Settings** > **Webhooks** > **Add webhook**
8. Paste the webhook URL into the **Payload URL** field and select `application/json` as the **Content type**
9. Click **Add webhook** to save
10. Test by editing or creating a file on the GitHub side - changes should be immediately pulled into the wiki

[![](./github-add-webhook.png?thumbnail=640)](./github-add-webhook.png)
