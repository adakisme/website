---
title: "Create Workspaces, Projects, Accounts and Roles"
keywords: 'KubeSphere, Kubernetes, Multi-tenant, Workspace, Account, Role, Project'
description: 'Create Workspaces, Projects, Accounts and Roles'
linkTitle: "Create Workspaces, Projects, Accounts and Roles"
weight: 2300
---

This quickstart demonstrates how to create workspaces, roles and user accounts which are required for the other tutorials. Meanwhile, you will learn how to create projects and DevOps projects within your workspace where your workloads are running. After this tutorial, you will become familiar with the multi-tenant management system of KubeSphere.

## Prerequisites

KubeSphere needs to be installed in your machine.

## Estimated Time

About 15 minutes.

## Architecture

The multi-tenant system of KubeSphere features **three** levels of hierarchical structure which are cluster, workspace and project. A project in KubeSphere is a Kubernetes [namespace](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/).

You are required to create a new [workspace](../../workspace-administration/what-is-workspace/) to work with instead of using the system workspace where system resources are running and most of them are viewable only. In addition, it is strongly recommended different tenants work with corresponding roles in a workspace for security considerations.

You can create multiple workspaces within a KubeSphere cluster. Under each workspace, you can also create multiple projects. Each level has multiple built-in roles. Besides, KubeSphere allows you to create roles with customized authorization as well. The KubeSphere hierarchy is applicable for enterprise users with different teams or groups, and different roles within each team.

## Hands-on Lab

### Step 1: Create an account

After KubeSphere is installed, you need to add different users with varied roles to the platform so that they can work at different levels on various resources. Initially, you only have one default account, which is `admin`, granted the role `platform-admin`. In the first step, you create an account `user-manager` and further create more accounts as `user-manager`.

1. Log in to the web console as `admin` with the default account and password (`admin/P@88w0rd`).

   {{< notice tip >}}
For account security, it is highly recommended that you change your password the first time you log in to the console. To change your password, select **User Settings** in the drop-down menu in the top right corner. In **Password Setting**, set a new password. You also can change the console language in **User Settings**.
   {{</ notice >}}

   ![chang-password](/images/docs/quickstart/create-workspaces-projects-accounts/chang-password.jpg)

2. After you log in to the console, click **Platform** in the top-left corner and select **Access Control**.

   ![access-control](/images/docs/quickstart/create-workspaces-projects-accounts/access-control.png)

   In **Account Roles**, there are four available built-in roles as shown below. The account to be created next will be assigned the role `users-manager`.

   | Built-in Roles       | Description                                                  |
   | -------------------- | ------------------------------------------------------------ |
   | `workspaces-manager` | The workspace manager on the platform who manages all workspaces on the platform. |
   | `users-manager`      | The user manager on the platform who manages all users.      |
   | `platform-regular`   | The normal user on the platform who has no access to any resources before joining a workspace or cluster. |
   | `platform-admin`     | The platform administrator who can manage all resources on the platform. |

   {{< notice note >}}
Built-in roles are created automatically by KubeSphere and cannot be edited or deleted.
   {{</ notice >}}

3. In **Accounts**, click **Create**. In the pop-up window, provide all the necessary information (marked with *) and select `users-manager`  for **Role**. Refer to the image below as an example.

   ![create-account](/images/docs/quickstart/create-workspaces-projects-accounts/create-account.jpg)

   Click **OK** after you finish. A newly-created account will display in **Accounts**.

4. Log out of the console and log back in with the account `user-manager` to create four accounts that will be used in other tutorials.

   {{< notice tip >}}
   To log out, click your username in the top-right corner and select **Log Out**.
   {{</ notice >}}

   | Account           | Role                 | Description                                                  |
   | ----------------- | -------------------- | ------------------------------------------------------------ |
   | `ws-manager`      | `workspaces-manager` | Create and manage all workspaces.                            |
   | `ws-admin`        | `platform-regular`   | Manage all resources in a specified workspace (This account is used to invite new members to a workspace in this example). |
   | `project-admin`   | `platform-regular`   | Create and manage projects and DevOps projects, and invite new members into the projects. |
   | `project-regular` | `platform-regular`   | `project-regular` will be invited to a project or DevOps project by `project-admin`. This account will be used to create workloads, pipelines and other resources in a specified project. |

5. Verify the four accounts created.

   ![account-list](/images/docs/quickstart/create-workspaces-projects-accounts/account-list.png)

### Step 2: Create a workspace

In this step, you create a workspace using the account `ws-manager` created in the previous step. As the basic logic unit for the management of projects, DevOps projects and organization members, workspaces underpin the multi-tenant system of KubeSphere.

1. Log in to KubeSphere as `ws-manager` which has the permission to manage all workspaces on the platform. Click **Platform** in the top-left corner and select **Access Control**. In **Workspaces**, you can see there is only one default workspace `system-workspace` listed, where system-related components and services run. You are not allowed to delete this workspace.

   ![create-workspace](/images/docs/quickstart/create-workspaces-projects-accounts/create-workspace.jpg)

2. Click **Create** on the right, name the new workspace `demo-workspace` and set the user `ws-admin` as the workspace manager shown in the screenshot below:

   ![create-workspace](/images/docs/quickstart/create-workspaces-projects-accounts/create-workspace.png)

   Click **Create** after you finish.

   {{< notice note >}}

   If you have enabled the [multi-cluster feature](../../multicluster-management/), you need to [assign an available cluster](../../cluster-administration/cluster-settings/cluster-visibility-and-authorization/#select-available-clusters-when-you-create-a-workspace) (or multiple clusters) to the workspace so that projects can be created on the cluster(s) later.

   {{</ notice >}} 

3. Log out of the console and log back in as `ws-admin`. In **Workspace Settings**, select **Workspace Members** and click **Invite Member**.

   ![invite-member](/images/docs/quickstart/create-workspaces-projects-accounts/invite-member.png)

4. Invite both `project-admin` and `project-regular` to the workspace. Grant them the role `workspace-self-provisioner` and `workspace-viewer` respectively. 

   {{< notice note >}}
The actual role name follows a naming convention: `<workspace name>-<role name>`. For example, in this workspace named `demo-workspace`, the actual role name of the role `viewer` is `demo-workspace-viewer`.
   {{</ notice >}}

   ![invite-member-and-grant-role](/images/docs/quickstart/create-workspaces-projects-accounts/invite-member-and-grant-role.png)

5. After you add both `project-admin` and `project-regular` to the workspace, click **OK**. In **Workspace Members**, you can see three members listed.

   | Account           | Role                         | Description                                                  |
   | ----------------- | ---------------------------- | ------------------------------------------------------------ |
   | `ws-admin`        | `workspace-admin`            | Manage all resources under the workspace (Use this account to invite new members to the workspace). |
   | `project-admin`   | `workspace-self-provisioner` | Create and manage projects and DevOps projects, and invite new members to join the projects. |
   | `project-regular` | `workspace-viewer`           | `project-regular` will be invited by `project-admin` to join a project or DevOps project. The account can be used to create workloads, pipelines, etc. |

### Step 3: Create a project

In this step, you create a project using the account `project-admin` created in the previous step. A project in KubeSphere is the same as a namespace in Kubernetes, which provides virtual isolation for resources. For more information, see [Namespaces](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/).

1. Log in to KubeSphere as `project-admin`. In **Projects**, click **Create**.

   ![kubesphere-projects](/images/docs/quickstart/create-workspaces-projects-accounts/kubesphere-projects.png)

2. Enter the project name (e.g. `demo-project`) and click **OK** to finish. You can also add an alias and description for the project.

   ![demo-project](/images/docs/quickstart/create-workspaces-projects-accounts/demo-project.png)

3. In **Projects**, click the project created just now to view its detailed information.

   ![click-demo-project](/images/docs/quickstart/create-workspaces-projects-accounts/click-demo-project.png)

4. On the **Overview** page of the project, the project quota remains unset by default. You can click **Set** and specify [resource requests and limits](../../workspace-administration/project-quotas/) as needed (e.g. 1 core for CPU and 1000Gi for memory).

   ![quota](/images/docs/quickstart/create-workspaces-projects-accounts/quota.png)

   ![specify-project-quota](/images/docs/quickstart/create-workspaces-projects-accounts/specify-project-quota.png)

5. Invite `project-regular` to this project and grant this user the role `operator`. Refer to the image below for specific steps.

   ![invite-project-member](/images/docs/quickstart/create-workspaces-projects-accounts/invite-project-member.png)

   {{< notice info >}}
The user granted the role `operator` will be a project maintainer who can manage resources other than users and roles in the project.
   {{</ notice >}}

6. Before creating a [Route](../../project-user-guide/application-workloads/ingress/) which is [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/) in Kubernetes, you need to enable a gateway for this project. The gateway is an [NGINX Ingress controller](https://github.com/kubernetes/ingress-nginx) running in the project. To set a gateway, go to **Advanced Settings** in **Project Settings** and click **Set Gateway**. The account `project-admin` is still used in this step.

   ![set-gateway](/images/docs/quickstart/create-workspaces-projects-accounts/set-gateway.png)

7. Choose the access method **NodePort** and click **Save**.

   ![nodeport](/images/docs/quickstart/create-workspaces-projects-accounts/nodeport.png)

8. Under **Internet Access**, it can be seen that the Gateway Address and the NodePort of http and https all display on the page.

   {{< notice note >}}
If you want to expose services using the type `LoadBalancer`, you need to use the LoadBalancer plugin of cloud providers. If your Kubernetes cluster is running in a bare metal environment, it is recommended that you use [PorterLB](https://github.com/kubesphere/porter) as the LoadBalancer plugin.
   {{</ notice >}}

   ![nodeport-setting](/images/docs/quickstart/create-workspaces-projects-accounts/nodeport-setting.png)

### Step 4: Create a role

After you finish the above steps, you know that users can be granted different roles at different levels. The roles used in previous steps are all built-in ones created by KubeSphere itself. In this step, you will learn how to define a customized role to meet the needs in your work.

1. Log in to the console as `admin` again and go to **Access Control**.

2. In **Account Roles**, there are four system roles listed which cannot be deleted or edited. Click **Create** and set a **Role Identifier**. In this example, a role named `roles-manager` will be created.

   ![customized-role](/images/docs/quickstart/create-workspaces-projects-accounts/customized-role.png)

   {{< notice note >}}
It is recommended you enter a description for the role as it explains what the role is used for. The role created here will be responsible for role management only, including adding and deleting roles.
   {{</ notice >}}

   Click **Edit Authorization** to continue.

3. In **Access Control**, select the authorization that you want this role to contain. For example, **Users View**, **Roles Management** and **Roles View** are selected for this role. Click **OK** to finish.

   ![edit-authorization](/images/docs/quickstart/create-workspaces-projects-accounts/edit-authorization.png)

   {{< notice note >}}
   **Depend on** means the major authorization (the one listed after **Depend on**) needs to be selected first so that the affiliated authorization can be assigned.
   {{</ notice >}}

4. Newly-created roles will be listed in **Account Roles**. You can click the three dots on the right to edit it.

   ![roles-manager](/images/docs/quickstart/create-workspaces-projects-accounts/roles-manager.png)

5. In **Accounts**, you can add a new account and grant it the role `roles-manager` or change the role of an existing account to `roles-manager` by editing it.

   ![edit-role](/images/docs/quickstart/create-workspaces-projects-accounts/edit-role.png)

   {{< notice note >}}
   The role of `roles-manager` overlaps with `users-manager` while the latter is also capable of user management. This example is only for demonstration purposes. You can create customized roles based on your needs.
   {{</ notice >}}

### Step 5: Create a DevOps project (Optional)

{{< notice note >}}

To create a DevOps project, you must install the KubeSphere DevOps system in advance, which is a pluggable component providing CI/CD pipelines, Binary-to-image, Source-to-image, and more. For more information about how to enable DevOps, see [KubeSphere DevOps System](../../pluggable-components/devops/).

{{</ notice >}}

1. Log in to the console as `project-admin`. In **DevOps Projects**, click **Create**.

   ![devops](/images/docs/quickstart/create-workspaces-projects-accounts/devops.png)

2. Enter the DevOps project name (e.g. `demo-devops`) and click **OK**. You can also add an alias and description for the project.

   ![devops-project](/images/docs/quickstart/create-workspaces-projects-accounts/devops-project.png)

3. In **DevOps Projects**, click the project created just now to view its detailed information.

   ![new-devops-project](/images/docs/quickstart/create-workspaces-projects-accounts/new-devops-project.png)

4. Go to **Project Management** and select **Project Members**. Click **Invite Member** to grant `project-regular` the role of `operator`, who is allowed to create pipelines and credentials.

   ![devops-invite-member](/images/docs/quickstart/create-workspaces-projects-accounts/devops-invite-member.png)

You are now familiar with the multi-tenant management system of KubeSphere. In other tutorials, the account `project-regular` will also be used to demonstrate how to create applications and resources in a project or DevOps project.
