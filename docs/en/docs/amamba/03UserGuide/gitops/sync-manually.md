---
hide:
  - toc
---

# Manually sync the app

This page demonstrates how to manually sync a continuously deployed application.

1. On the `Application Workbench` -> `Continuous Publishing` page, click the name of an application whose synchronization status is `Not Synchronized`.

    ![Not synced](../../images/sync01.png)

1. On the application details page, click the `Sync` button to enter the synchronization page:

    ![Sync](../../images/sync02.png)

1. On the `Sync Application` page, configure the following parameters:

    - Resource name: read-only status, does not support secondary editing
    - Branch/Tag: Set the branch for synchronization. If the synchronization method is automatic synchronization, an error will be reported synchronously after the change
        - Cleanup: After checking, if some resources are deleted in the manifest file, the related resources will also be cleaned up during synchronization, otherwise the related resources will be skipped and cleaned up
        - Test run: Simulates the synchronization process
        - Apply only: After checking, resources will be deployed through kubectl apply
        - Mandatory application: After checking, if a conflict is encountered when deploying a resource, the resource will be deleted and recreated immediately
    - Synchronization settings: The parameters of the synchronization settings are the same as those at the time of creation. For specific parameter descriptions, please refer to creating an application

    In the Synchronization Resources area, select at least one application to be synchronized. For example, some resources have already been synchronized, and only unsynchronized resources need to be synchronized.

    ![Sync](../../images/sync03.png)

1. Click `OK`, wait for the synchronization to be successful, and then check the synchronization result.