---
hide:
  - toc
---

# 通过界面安装 HwameiStor

本文介绍如何通过平台界面安装 HwameiStor。

## 前提条件

- 待使用节点上已准备空闲 HDD、SSD 磁盘
- 已完成[准备工作](prereq.md)中事项
- 如需要使用高可用数据卷，请提前[安装好 DRBD](drbdinstall.md)
- 如部署环境为生产环境，请提前阅读[生产环境资源要求](proresource.md)
- 如果您的 Kubernetes 发行版使用不同的 `kubelet` 目录，请提前确认 `kubeletRootDir`。
  详细信息请参考[自定义 kubelet 根目录](customized-kubelet.md)。

## 安装步骤

请确认您的集群已成功接入 __容器管理__ 平台，然后执行以下步骤安装 HwameiStor。

1. 在左侧导航栏点击 __容器管理__ —> __集群列表__ ，然后找到准备安装 HwameiStor 的集群名称。

2. 在左侧导航栏中选择 __Helm 应用__ -> __Helm 模板__，找到并点击 __HwameiStor__ 。

    ![UI Install01](https://docs.daocloud.io/daocloud-docs-images/docs/storage/images/hwameistorUI01.jpg)

3. 在 __版本__ 中选择希望安装的版本，点击 __安装__ 。

4. 在安装界面，填写所需的安装参数。如需要部署生产环境，建议调整资源配置：[生产环境资源要求](proresource.md)。

    ![HwameistorUI02](https://docs.daocloud.io/daocloud-docs-images/docs/storage/images/hwameistorUI02.jpg)

    ![HwameistorUI03](https://docs.daocloud.io/daocloud-docs-images/docs/storage/images/HwameistorUI03.jpg)

    - `Global Setting` —> `global image Registry`：
    
        设置所有镜像的仓库地址，默认已经填写了可用的在线仓库。
        如果是私有化环境，可修改为私有仓库地址。
        
    - `Global Setting` —> `K8s image Registry`：
    
        设置 K8s 镜像仓库地址，默认已经填写可用在线仓库。
        如果私有化环境，可修改为私有仓库地址。
        
    - `Global Setting` —> `kubelet Root Dir`：
    
        默认的 `kubelet` 目录为 `/var/lib/kubelet`。
        如果您的 Kubernetes 发行版使用不同的 `kubelet` 目录，必须设置参数 `kubeletRootDir`。
        详细信息请参考[自定义 kubelet 根目录](customized-kubelet.md)。
        
    - `Config Settings` —> `DRBDStartPort`：
    
        默认以 43001 开始，当开启 `DRBD` 时，每创建一个高可用数据卷，需要占用主副本数据卷所在节点的一组端口。
        请[先安装好 DRBD](drbdinstall.md)。
        
    - **StorageClass 配置**
    
        HwameiStor 部署后，会自动创建 StorageClass。
    
        - `AllowVolumeExpansion`：默认为关闭状态，开启后，基于 StorageClass 创建的数据卷可以扩容。
        - `DiskType`：创建的存储池（StorageClass）的磁盘类型，支持类型有：HDD、SSD。默认为 HDD。
        - `Enable HA`：默认关闭 `HA`, 即创建的数据卷为`非高可用`，当开启后，使用该 `StorageClass`
          创建的数据卷可以设置为`高可用数据卷`。请在开启前完成 [DRBD 安装](drbdinstall.md)。
        - `Replicas`：非 `HA` 模式下，`Replicas` 数量为 `1`；当开启 `HA` 模式后，`Replicas` 数量可以为 `1` 或者 `2`，并且数量为 `1` 时，可以转换为 `2`。
        - `ReclaimPolicy`: 数据卷删除时，数据的保留策略默认为 `delete`。
        
            1. `Delete`：当删除数据卷时，数据一并删除。
            2. `Retain`：当删除数据卷时，保留数据。
    
5. 参数输入完成后，点击 __确定__ 完成创建，完成创建后可点击 __Helm 应用__ 查看 __HwameiStor__ 安装状态。

    ![HwameistorUI04](https://docs.daocloud.io/daocloud-docs-images/docs/storage/images/HwameistorUI04.jpg)
    
6. 安装完成！要验证安装效果，请参见下一章[安装后检查](./post-check.md)。
