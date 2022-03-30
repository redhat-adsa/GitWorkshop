# Basic Git

## Authentication

Most repositories are disabling username and password as an auth mechanism for security reason so you will either need to set up a personal access token if using https authentication, or set up an ssh key and add it to your git server. Your Team will need to decide what is a best practice going forward. for this exercise you only need to do the one you wish to use, so please choose your own adventure [SSH](#ssh) or [Personal Access Token](#personal-access-token)

## SSH

Generate an 4096 bit rsa key with a comment to make sure you know what it is for later.

  ```shell
  ssh-keygen -t rsa -b 4096 -C "sshkeyforgit"
  cat ~/.ssh/id_rsa.pub
  ```

  `NOTE:` Never share your private key. It is the one without the `.pub` extension.

## Add the public key to Azure DevOps

Associate the public key generated in the previous step with your user ID.

1. Open your security settings by browsing to the web portal and selecting your avatar in the upper right of the
   user interface. Select **Security** in the menu that appears.

   ![Accessing User Profile in Azure DevOps Services](../images/ssh_profile_access.png)

2. Select **+ New Key**.

   ![Accessing Security Configuration in Azure DevOps Services](../images/ado_ssh_accessing_security_key.png)

3. Copy the contents of the public key (for example, id_rsa.pub) that you generated into the **Public Key Data** field.

   *IMPORTANT*
   >Avoid adding whitespace or new lines into the **Key Data** field, as they can cause Azure DevOps Services to use an invalid public key. When pasting in the key, a newline often is added at the end. Be sure to remove this newline if it occurs.

    ![Configuring Public Key in Azure DevOps Services](../images/ado_ssh_key_input.png)

4. Give the key a useful description (this description will be displayed on the **SSH public keys** page for your profile) so that you can remember it later. Select **Save** to store the public key. Once saved, you cannot change the key. You can delete the key or create a new entry for another key. There are no restrictions on how many keys you can add to your user profile.

5. Test the connection by running the following command: `ssh -T git@ssh.dev.azure.com`.
If everything is working correctly, you'll receive a response which says: `remote: Shell access is not supported.`

## Personal Access Token

1. Sign in to your organization in Azure DevOps (```https://dev.azure.com/{yourorganization}```)
  
2. From your home page, open your user settings, and then select **Personal access tokens**.

   ![Select Personal Access Tokens](../images/ado-select-personal-access-tokens.jpg)

3. And then select **+ New Token**.

   ![Select New Token to create](../images/ado-select-new-token.png)

4. Name your token, select the organization where you want to use the token, and then choose a lifespan for your token.

   ![Enter basic token information](../images/ado-create-new-pat.png)

5. Select the scopes for this token to authorize for *your specific tasks*.

   For example, to create a token to enable a build and release agent to authenticate to Azure DevOps Services, limit your token's scope to **Agent Pools (Read & manage)**. To read audit log events, and manage and delete streams, select **Read Audit Log**, and then select **Create**.

   ![Select scopes for your PAT](../images/ado-select-pat-scopes-preview.png)

6. When you're done, make sure to copy the token. For your security, it won't be shown again. Use this token as your password.

---

## Forking

Most public repositories don't let everyone push to them so you will need to fork the repository to your own namespace in your git server before you can clone it.

Here is how to fork in ADO:

1. Navigate to the repository to fork, and choose **Fork**.

2. Specify a name, and choose the project where you want the fork to be created. If the repository contains a lot of topic branches, we recommend you fork only the default branch.

3. Choose **Fork** to create the fork.

![Create new fork](../images/ado-create-new-fork.png)

> [!NOTE]
> You must have the [Create Repository](https://docs.microsoft.com/en-us/azure/devops/organizations/security/permissions?view=azure-devops#git-repository-object-level) permission in your chosen project to create a fork.
> We recommend you create a dedicated project for forks where all contributors have the Create Repository permission. For an example of granting this permission, see [Set Git repository permissions](https://docs.microsoft.com/en-us/azure/devops/repos/git/set-git-repository-permissions?view=azure-devops#set-git-repository-permissions).

## Cloning a repository

Cloning is making a local copy of code in a local copy of the remote repository. Most of the time you will be cloning someone elses repository so you can use it or make a change and then push in for a request to have it merged into the upstream codebase. Remember unless you have access to the upstream repository you will need to either request access or fork the code to push into it.

**For this exercise** fork this repository using the instructions above then clone it (depending on if you created a PAT (public access token) or a SSH key follow the correct instruction below:

## SSH Clone

```shell
git clone git@azuredevops.com:username/GitWorkshop.git
```

## HTTP Clone

Example:

```shell
git clone https://<pat>@azuredevops.com/username/GitWorkshop.git
```

`NOTE:` Replace `<pat>` with the token you created in the earlier exercise.

Output:

```shell
Cloning into 'GitWorkshop'...
remote: Enumerating objects: 283, done.
remote: Counting objects: 100% (283/283), done.
remote: Compressing objects: 100% (173/173), done.
remote: Total 283 (delta 77), reused 276 (delta 74), pack-reused 0
Receiving objects: 100% (283/283), 81.03 KiB | 1.72 MiB/s, done.
Resolving deltas: 100% (77/77), done.
cd GitWorkshop
tree
.
├── LICENSE
├── README.md
└── workshops
    └── how_we_enable
        ├── 01-Getting_Started.md
        ├── 02-Logistics.md
        ├── 03-Strategy.md
        └── 04-Practice_Scenarios.md
```

## Branching

It is a best practice to not commit code directly to the main (legacy master) branch. In order to void this we create a feature branch to work from with the `git branch` or `git checkout` command. First please go to this interactive training for branching: [https://learngitbranching.js.org/](https://learngitbranching.js.org/)

To create a branch and check it out (ie move to it as your working branch) you can do this with a single command `git checkout -b <branch-name>`.

```shell
git checkout -b test
```

Output:

```shell
Switched to a new branch 'test'
```

You should see that you are now in the test branch by executing the command `git status`

```shell
git status
```

Output:

```shell
On branch test
nothing to commit, working tree clean
```

Now any changes you make are in a separate branch from main called test.

## Commiting Code

1. In the GitWorkshop directory create a new file and look at the status of it in git.

    ```shell
    touch mynewfile.txt
    git status
    ```

    Output of git status:

    ```shell
    Untracked files:
    (use "git add <file>..." to include in what will be committed)
        mynewfile.txt
    ```

2. Now let's add the file so it is tracked by git going forward `git add <filename>`, and commit it so that the new file is ready to be pushed to the remote repository `git commit -m 'meaningful commit message that defines what changed, not how it changed the logs will show the how'`.

    ```shell
    git add mynewfile.txt
    git status
    ```

    Output of git status:

    ```shell
    Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
            new file:   mynewfile.txt
    ```

3. Finally lets commit the code:

    ```shell
    git commit -m 'adding mynewfile.txt'
    ```

    Output:

    ```shell
    [main 60cdc2b] adding mynewfile.txt
     1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 mynewfile.txt
    ```

## Pushing Commit to Repository Server

Just like it sounds, `git push` pushes the commits to the central repository. A best practice is to specify the remote you are pushing to (usually origin) and the branch for this example we will use test. `Note:` Generally upstream repositories will not allow pushing directly to main, so you will always need to either fork the repository, or create a branch.

```shell
git push origin test
```

`Note:` 'Origin' in the command above denotes the location being pushed to.  Each repository will have an origin and a remote location (as stated before).

Output:

```shell
Enumerating objects: 1, done.
Counting objects: 100% (1/1), done.
Delta compression using up to 8 threads
Compressing objects: 100% (1/1), done.
Writing objects: 100% (1/1), 1018 bytes | 339.00 KiB/s, done.
Total 9 (delta 5), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local objects.
To github.com:yourusername/GitWorkshop.git
   c0db901..6eaaf6d  test -> test
```

## Merging Code

If you are the owner and want to merge a feature branch into `main` you can do so via `git merge <branchname>` from the main branch, however this is not a best practice. Best practice is to perform a pull request from the central server and have someone review the changes before they are merged into main.

From the Azure DevOps project website, you can create a new PR from:

- [The Pull requests page](#from-the-pull-requests-page).
- [A feature branch pushed to your repo](#from-a-pushed-branch).
- [An existing PR, by using cherry-pick](#add-updates-with-cherry-pick).
- [The Development control in a linked Azure Boards work item](#from-a-linked-work-item).

### Create a PR from the Pull requests page

You can create PRs for any branch from your project's **Pull requests** page on the web.

1. On the **Repos** > **Pull requests** page, select **New pull request** at upper right.

   ![Screenshot of the New pull request button.](../images/ado-new-pr-button.png)

2. Select the branch with the changes and the branch you want to merge the changes into, such as the main branch.

   ![Screenshot of source and target branches for a P R in Azure Repos.](../images/ado-pr-branch-targets.png)

3. Enter your PR details and create the PR.

### Create a PR from a pushed branch

After you push or update a feature branch, Azure Repos displays a prompt to create a PR.

- On the **Pull requests** page:

  ![Screenshot that shows the prompt to create a P R on the Pull Requests tab in Azure Repos.](../images/ado-create-pr-from-push-new-nav.png)

- On the **Files** page:

  ![Screenshot that shows the prompt to create a P R on the Files tab in Azure Repos.](../images/ado-create-pr-from-push-files-tab-new-nav.png)

Select **Create a pull request** to go to a page where you can enter your PR details and create the PR.

After you push or update a feature branch, Azure Repos prompts you to create a PR in the **Code** view on the web. This prompt is displayed on **Pull Requests** and **Files**.

![Screenshot that shows the prompt to create a P R on the Pull Requests tab in Azure Repos.](../images/ado-create-pr-from-push.png)

![Screenshot that shows the prompt to create a P R on the Files tab in Azure Repos.](../images/ado-create-pr-from-push-files-tab.png)

Select **Create a pull request** to go to a page where you can enter your PR details and create the PR.

### Create a PR from a linked work item

You can create a PR directly from an Azure Boards work item linked to the branch.

1. In Azure Boards, from **Backlogs** or **Queries** in the **Work** view, open a work item that's linked to the branch.
1. In the **Development** area of the work item, select **Create a pull request**.

   ![Screenshot of creating a PR from the Development area of a work item with a linked branch.](../images/ado-create-pr-from-work-item.png)

The link takes you to a page where you can enter your PR details and create the PR.

Please create a pull request following above documentation to your repository you created above from the `test` branch into the `main` branch of your fork.

---

[Next Advanced Git](02-advanced.md)

> Note: all information here was sourced from [Microsoft Docs](https://docs.microsoft.com/en-us/azure/devops/repos/git/)
