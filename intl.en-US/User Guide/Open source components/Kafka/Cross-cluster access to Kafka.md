# Cross-cluster access to Kafka {#concept_zkm_mcx_y2b .concept}

An independent Kafka cluster is deployed to provide the Kafka service. Therefore, you may need to access this service across clusters.

## Cross-cluster access to Kafka {#section_qm5_zcx_y2b .section}

Cross-cluster access to Kafka consists of two common types:

-   Access E-MapReduce Kakfa clusters from Alibaba Cloud intranet network.
-   Access E-MapReduce Kakfa clusters from the public network.

Different solutions are prepared for different E-MapReduce versions.

## EMR-3.11.x and Later Versions {#section_c4v_bdx_y2b .section}

-   Access Kafka from the Alibaba Cloud intranet network

    You can access Kafka service directly by using the intranet IP address of a Kakfa cluster node. Use port 9092 to access Kafka from the intranet network.

    Make sure that the networks are accessible before you access Kafka service:

    -   For more information about how to access a VPC from a classic network, see [Access between classic network and VPC](intl.en-US/User Guide/Configure clusters/Access between classic network and VPC.md#) here.
    -   For more information about how to access a VPC from another VPC, see [here](../../../../intl.en-US/IPsec-VPN Quick Start/Configure a VPC-to-VPC connection.md#).
-   Access Kafka in the public network

    The Core node of the Kafka cluster is unable to access to the public network by default. To access the Kafka cluster in the public network, perform the following steps:

    1.  Interconnect Kafka clusters with the public network.
        -   If Kafka clusters are deployed in a VPC environment, there are two ways:
            -   Deploy Express Connect to interconnect the VPC with the public network. For details, see [Express Connect](../../../../intl.en-US/Product Introduction/What is Express Connect?.md#) document.
            -   Bind EIPs to cluster core nodes. For details, see [EIP documents](../../../../intl.en-US/Product Introduction/What is Elastic IP Address?.md#)\). Perform the following steps to bind the EIP to the ECS:
        -   If Kafka is deployed in a classic network, there are two ways:
            -   To create a Pay-As-You-Go cluster, use ECS APIs. For details, see [API documents](../../../../intl.en-US/API Reference/Networks/AllocatePublicIpAddress.md#).
            -   To create a cluster through the Subscription method, you can directly assign a public IP address to relevant host in the ECS console.
    2.  Create an EIP in the [VPC console](https://vpcnext.console.aliyun.com/eip), and purchase relevant EIPs based on the number of Core nodes in the Kafka cluster.
    3.  Configure security group rules for the Kafka cluster to control public network access to the IP addresses of the Kafka cluster. The objective is to improve the security of the Kafka cluster exposed in the public network. You can view the security group to which the cluster belongs in the EMR console, and configure security group rules based on security group IDs. For more information, see [here](../../../../intl.en-US/User Guide/Security groups/Typical applications of security group rules.md#).
    4.  On the **Cluster Management** page of the E-MapReduce console, click **Manage** after the specified cluster, select **Cluster Overview** on the left side of the page, and then click the **Sync Cluster Host Info** button in the upper right corner.
    5.  Restart the cluster Kafka service.
    6.  Use the EIP of the Kafka cluster node to access Kafka service in the public network. Use port 9093 to access Kafka service from the public network.

## Versions earlier than EMR-3.11.x {#section_lnz_hdx_y2b .section}

-   Access Kafka from the Alibaba Cloud intranet network

    In this case, you must configure the host information of the Kafka cluster node on the client host. **Long domain** of the Kafka cluster node must be configured. Example:

    ```
    ／etc/hosts
    # kafka cluster
    10.0.1.23 emr-header-1.cluster-48742
    10.0.1.24 emr-worker-1.cluster-48742
    10.0.1.25 emr-worker-2.cluster-48742
    10.0.1.26 emr-worker-3.cluster-48742
    ```

-   Access Kafka in the public network

    The Core node of the Kafka cluster is unable to access to the public network by default. To access the Kafka cluster in the public network, perform the following steps:

    1.  Interconnect Kafka clusters with the public network.
        -   If Kafka clusters are deployed in a VPC environment, there are two ways:
            -   Deploy Express Connect to interconnect the VPC with the public network. For details, see [Express Connect](http://icms.alibaba-inc.com/document/content/2675?version=477&topic=13811) document.
            -   Bind EIPs to cluster Core nodes. For details, [EIP documents](../../../../intl.en-US/Product Introduction/What is Elastic IP Address?.md#). Perform the following steps to bind the EIP to the ECS.
        -   If Kafka is deployed in a classic network, there are two ways:
            -   To create a Pay-As-You-Go cluster, use ECS APIs. For details, see [API documents](../../../../intl.en-US/API Reference/Networks/AllocatePublicIpAddress.md#).
            -   To create a cluster through the Subscription method, you can directly assign a public IP address to relevant host in the ECS console.
    2.  Create an EIP in the [VPC console](https://vpcnext.console.aliyun.com/eip), and purchase relevant EIPs based on the number of Core nodes in the Kafka cluster.
    3.  Configure security group rules for the Kafka cluster to control public network access to the IP addresses of the Kafka cluster. The objective is to improve the security of the Kafka cluster exposed in the public network. You can view the security group to which the cluster belongs in the EMR console, and configure security group rules based on security group IDs. For more information, see [here](../../../../intl.en-US/User Guide/Security groups/Typical applications of security group rules.md#).
    4.  Modify the software configuration of the Kafka cluster listeners.address.principal to HOST, and restart the Kafka cluster.
    5.  Configure the hosts file on the local client host.

