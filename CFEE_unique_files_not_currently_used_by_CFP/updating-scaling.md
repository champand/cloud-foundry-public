---

copyright:

  years: 2018, 2019
lastupdated: "2019-08-12"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# Updating and scaling your {{site.data.keyword.cfee_full_notm}} instances
{: #update-scale}

Update the {{site.data.keyword.cfee_full_notm}} service instance to the latest version to get the latest CFEE functions and fixes. Updates of CFEE can include new versions of Cloud Foundry and CFEE supporting services (Kubernetes, Cloud Object Storage or Compose for PostgreSQL).  However, not every CFEE update will include a new version of Cloud Foundry and CFEE supporting services.

The CFEE version updates take place on the control plane containing the CFEE components and on the cells. You can also scale the capacity of your CFEE instance by adding or deleting application cells.

**Note:** During a version update or while cells are being added or deleted, some metrics (e.g., Memory and CPU) shown in the _Overview_, _Resource Usage_ and _Update and Scaling_ pages may not be available.

## Updating the version
{: #update}

A CFEE update sequentially updates the CFEE cluster's control plane and cell nodes.

Users need the following permissions to be able to update a CFEE instance to a new version:
   * _Editor_ role or higher to a CFEE instance.
   * _Operator_ role or higher to the Kubernetes cluster into which the CFEE is provisioned.

To update the CFEE version of your CFEE instance:
1. Go to the [{{site.data.keyword.cfee_full_notm}} Environments list](https://cloud.ibm.com/cloudfoundry/environments) and open the {{site.data.keyword.cfee_full_notm}} that you want to update.
2. Go to the **Updates and Scaling** tag under the _Operations_ page in the left navigation pane.
3. If an update is available, an **Update** button will appear in the _Control Plane_ table. Click **Update**.
4. In the Update dialog, select one of the available CFEE versions and click **Update**. The version description details the versions of the components included in the selected CFEE version package, along with a link to the _What's New_ document describing the content delivered in that version.
5. The _Nodes Status_ column shows the progress of the update. Once the update is complete, the _Version_ column reflects the new CFEE version.

### Updating the CFEE cluster's Kubernetes version

New Kubernetes versions are validated to ensure compatibility with a particular version of CFEE. Once a Kuberenetes version has been validated, it is included as part of a CFEE version. During an update to a CFEE version with a newer version of Kubernetes, the cluster's master version, control plane nodes, and cell nodes are each sequentially updated to the new Kubernetes version. The sequential update process for CFEE components begins once all nodes have been updated to the specified Kubernetes version.

## Disruptions during version update
{: #update-disruption}

Updating the Cloud Foundry control plane is not disruptive.  However, updating Cloud Foundry cells to a new CFEE version may disrupt the operation of the CFEE environment.  The update is performed one cell at a time, so that application instances running in a cell are brought back up once the update is completed while the other cells are down during their update. Nevertheless, some applications and services running in the CFEE may become unavailable under some circumstances. For example, if the CFEE has two Cloud Foundry cells running applications at full capacity, an application with only one instance will be down during the version update because the application instance cannot be moved to another cell while the cell is being updated and, consequently, the application will be unavailable.  Even with three cells and three instances per application, there can be transient disruption in application availability during a version update.

During the version update there may be a discrepancy in the Memory and CPU metrics reported in the Cell Nodes gauges shown in the CFEE _Overview_ page (which are generated by the Kubernetes cluster) and the same metrics shown in the _Nodes_ dashboard in Grafana (which are generated a the operating system level).

The status of CFEE components while the updating is in progress will be reflected in the Health Check page.  The restart takes approximately 45 minutes.

## Version cycle and support policy
{: #version-cycle}

The Cloud Foundry Enterprise Environment service typically releases a new version on a regulary basis. The service's version follows the semantic versioning format _**Major.Minor.Patch**_ (https://semver.org/)

The _minor_ version is incremented regularly (typically, monthly). The _major_ version is also incremented also, but less frequently, typically when there is a significant change.  Updating to a _Major_ version may result in some disruption during upgrade. _Patches_ deliver a fix and are applied to the latest available version. 

To prevent system downtime, CFEE instances are only allowed to update up to 2 _major_ versions ahead of its current version. Using the previous example, if the CFEE instance current version is 3.1.2, and the target version is 7.0.0, the CFEE instance would need to be updated first to version 5.x.x, and then to the target version 7.0.0 .

New capabilities and enhancements to existing capabilities delivered regularly become available only in the latest version. Critical fixes are delivered in the latest release within only the latest 3 _major_ versions (the latest version and the two previous versions). For example, if versions 4.x.x, 3.x.x and 2.x.x are available, versions 1.x.x will not be supported (i.e.,  patches will not be delivered with fixes for versions 1.x.x).  

Patches are delivered on the latest available version of a given _major_. For example, if a CFEE instance is running version 3.1.2 but the latest available version in the 3.x.x _major_ is version 3.3.4, any patches will be delivered on the code base of version 3.3.x, which means that the patch will be delivered in a (new) version 3.3.5. The CFEE instance with version 3.1.2 would need to be updated to version 3.3.5 in order to receive the new patch (i.e., udating just to version 3.3.4 will not solve the provlem fixed in the patch, which is delivered only in version 3.3.5).

## Scaling the infrastructure
{: #scale}

Users need the following permissions to be able to add or remove Cloud Foundry cells to a CFEE instance:
* _Administrator_ role or higher to a CFEE instance.
* _Operator_ role or higher to the Kubernetes cluster into which the CFEE is provisioned.

To add application cells in your CFEE instance:
1. Go to the [{{site.data.keyword.cloud_notm}} dashboard](https://cloud.ibm.com/dashboard/apps/) and open the {{site.data.keyword.cfee_full_notm}} where you want add cells.
2. Click **Change Cloud Foundry cell count** near the _Cells_ table, which opens the _Add Cloud Foundry Cells_ page.
3. In the _Change Cloud Foundry cell count page_, select the total number of cells in the Target cell count field. For a non-VPC {{site.data.keyword.cfee_short}}, the page also shows the Geography and Location of the {{site.data.keyword.cfee_short}} instance where the cells will be added. It also shows the current number of cells in the _Current Cell count_ field and the capacity of the cells to be added (which is the same as the capacity of the current cells) in the Node size filed. The page's right pane shows the estimated cost of the added cells along with the new total estimated cost of the environment. For a VPC {{site.data.keyword.cfee_short}}, there must be enough unallocated IPs in each of the VPC subnets in the {{site.data.keyword.cfee_short}}.
4. Click **Add cells**.  In the _Add Cells_ dialog, click **Add**
5. Go back to the _Updates and Scaling_ page. A new row is added in the _Cell Nodes_ table for each new Cell node. The Node Status column indicates the progress of adding and deploying the cell to your CFEE environment.