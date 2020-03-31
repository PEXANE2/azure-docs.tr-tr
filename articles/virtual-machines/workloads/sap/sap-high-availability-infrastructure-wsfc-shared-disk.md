---
title: WSFC&paylaşılan diskile SAP ASCS/SCS için azure altyapısı | Microsoft Dokümanlar
description: Bir SAP ASCS/SCS örneği için Windows failover kümesi ve paylaşılan disk kullanarak Azure altyapısını SAP HA için nasıl hazırlayacağınızı öğrenin.
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: rdeltcheva
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: ec976257-396b-42a0-8ea1-01c97f820fa6
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: radeltch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8a49bc979923bf52d099e30615910c5bdb0601b6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79279864"
---
# <a name="prepare-the-azure-infrastructure-for-sap-ha-by-using-a-windows-failover-cluster-and-shared-disk-for-sap-ascsscs"></a>SAP ASCS/SCS için Windows failover kümesi ve paylaşılan disk kullanarak Azure altyapısını SAP HA için hazırlayın

[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2243692]:https://launchpad.support.sap.com/#/notes/2243692

[sap-installation-guides]:http://service.sap.com/instguides
[tuning-failover-cluster-network-thresholds]:https://techcommunity.microsoft.com/t5/Failover-Clustering/Tuning-Failover-Cluster-Network-Thresholds/ba-p/371834

[azure-resource-manager/management/azure-subscription-service-limits]:../../../azure-resource-manager/management/azure-subscription-service-limits.md
[azure-resource-manager/management/azure-subscription-service-limits-subscription]:../../../azure-resource-manager/management/azure-subscription-service-limits.md

[dbms-guide]:../../virtual-machines-windows-sap-dbms-guide.md

[deployment-guide]:deployment-guide.md

[dr-guide-classic]:https://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md

[ha-guide]:sap-high-availability-guide.md
[sap-high-availability-architecture-scenarios]:sap-high-availability-architecture-scenarios.md
[sap-high-availability-guide-wsfc-shared-disk]:sap-high-availability-guide-wsfc-shared-disk.md
[sap-high-availability-guide-wsfc-file-share]:sap-high-availability-guide-wsfc-file-share.md
[sap-ascs-high-availability-multi-sid-wsfc]:sap-ascs-high-availability-multi-sid-wsfc.md
[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md
[sap-high-availability-installation-wsfc-shared-disk]:sap-high-availability-installation-wsfc-shared-disk.md
[sap-ha-guide-9.1.1]:high-availability-guide.md#a97ad604-9094-44fe-a364-f89cb39bf097
[sap-hana-ha]:sap-hana-high-availability.md
[sap-suse-ascs-ha]:high-availability-guide-suse.md

[planning-guide]:planning-guide.md
[planning-guide-11]:planning-guide.md
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10

[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f


[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md
[sap-high-availability-infrastructure-wsfc-shared-disk-vpn]:sap-high-availability-infrastructure-wsfc-shared-disk.md#c87a8d3f-b1dc-4d2f-b23c-da4b72977489
[sap-high-availability-infrastructure-wsfc-shared-disk-change-def-ilb]:sap-high-availability-infrastructure-wsfc-shared-disk.md#fe0bd8b5-2b43-45e3-8295-80bee5415716
[sap-high-availability-infrastructure-wsfc-shared-disk-setup-wsfc]:sap-high-availability-infrastructure-wsfc-shared-disk.md#0d67f090-7928-43e0-8772-5ccbf8f59aab
[sap-high-availability-infrastructure-wsfc-shared-disk-collect-cluster-config]:sap-high-availability-infrastructure-wsfc-shared-disk.md#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79
[sap-high-availability-infrastructure-wsfc-shared-disk-install-sios]:sap-high-availability-infrastructure-wsfc-shared-disk.md#5c8e5482-841e-45e1-a89d-a05c0907c868
[sap-high-availability-infrastructure-wsfc-shared-disk-add-dot-net]:sap-high-availability-infrastructure-wsfc-shared-disk.md#1c2788c3-3648-4e82-9e0d-e058e475e2a3
[sap-high-availability-infrastructure-wsfc-shared-disk-install-sios-both-nodes]:sap-high-availability-infrastructure-wsfc-shared-disk.md#dd41d5a2-8083-415b-9878-839652812102
[sap-high-availability-infrastructure-wsfc-shared-disk-setup-sios]:sap-high-availability-infrastructure-wsfc-shared-disk.md#d9c1fc8e-8710-4dff-bec2-1f535db7b006

[sap-ha-multi-sid-guide]:sap-high-availability-multi-sid.md (SAP multi-SID yüksek kullanılabilirlik yapılandırması)

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png


[sap-ha-guide-figure-1000]:./media/virtual-machines-shared-sap-high-availability-guide/1000-wsfc-for-sap-ascs-on-azure.png
[sap-ha-guide-figure-1001]:./media/virtual-machines-shared-sap-high-availability-guide/1001-wsfc-on-azure-ilb.png
[sap-ha-guide-figure-1002]:./media/virtual-machines-shared-sap-high-availability-guide/1002-wsfc-sios-on-azure-ilb.png
[sap-ha-guide-figure-2000]:./media/virtual-machines-shared-sap-high-availability-guide/2000-wsfc-sap-as-ha-on-azure.png
[sap-ha-guide-figure-2001]:./media/virtual-machines-shared-sap-high-availability-guide/2001-wsfc-sap-ascs-ha-on-azure.png
[sap-ha-guide-figure-2003]:./media/virtual-machines-shared-sap-high-availability-guide/2003-wsfc-sap-dbms-ha-on-azure.png
[sap-ha-guide-figure-2004]:./media/virtual-machines-shared-sap-high-availability-guide/2004-wsfc-sap-ha-e2e-archit-template1-on-azure.png
[sap-ha-guide-figure-2005]:./media/virtual-machines-shared-sap-high-availability-guide/2005-wsfc-sap-ha-e2e-arch-template2-on-azure.png

[sap-ha-guide-figure-3000]:./media/virtual-machines-shared-sap-high-availability-guide/3000-template-parameters-sap-ha-arm-on-azure.png
[sap-ha-guide-figure-3001]:./media/virtual-machines-shared-sap-high-availability-guide/3001-configuring-dns-servers-for-Azure-vnet.png
[sap-ha-guide-figure-3002]:./media/virtual-machines-shared-sap-high-availability-guide/3002-configuring-static-IP-address-for-network-card-of-each-vm.png
[sap-ha-guide-figure-3003]:./media/virtual-machines-shared-sap-high-availability-guide/3003-setup-static-ip-address-ilb-for-ascs-instance.png
[sap-ha-guide-figure-3004]:./media/virtual-machines-shared-sap-high-availability-guide/3004-default-ascs-scs-ilb-balancing-rules-for-azure-ilb.png
[sap-ha-guide-figure-3005]:./media/virtual-machines-shared-sap-high-availability-guide/3005-changing-ascs-scs-default-ilb-rules-for-azure-ilb.png
[sap-ha-guide-figure-3006]:./media/virtual-machines-shared-sap-high-availability-guide/3006-adding-vm-to-domain.png
[sap-ha-guide-figure-3007]:./media/virtual-machines-shared-sap-high-availability-guide/3007-config-wsfc-1.png
[sap-ha-guide-figure-3008]:./media/virtual-machines-shared-sap-high-availability-guide/3008-config-wsfc-2.png
[sap-ha-guide-figure-3009]:./media/virtual-machines-shared-sap-high-availability-guide/3009-config-wsfc-3.png
[sap-ha-guide-figure-3010]:./media/virtual-machines-shared-sap-high-availability-guide/3010-config-wsfc-4.png
[sap-ha-guide-figure-3011]:./media/virtual-machines-shared-sap-high-availability-guide/3011-config-wsfc-5.png
[sap-ha-guide-figure-3012]:./media/virtual-machines-shared-sap-high-availability-guide/3012-config-wsfc-6.png
[sap-ha-guide-figure-3013]:./media/virtual-machines-shared-sap-high-availability-guide/3013-config-wsfc-7.png
[sap-ha-guide-figure-3014]:./media/virtual-machines-shared-sap-high-availability-guide/3014-config-wsfc-8.png
[sap-ha-guide-figure-3015]:./media/virtual-machines-shared-sap-high-availability-guide/3015-config-wsfc-9.png
[sap-ha-guide-figure-3016]:./media/virtual-machines-shared-sap-high-availability-guide/3016-config-wsfc-10.png
[sap-ha-guide-figure-3017]:./media/virtual-machines-shared-sap-high-availability-guide/3017-config-wsfc-11.png
[sap-ha-guide-figure-3018]:./media/virtual-machines-shared-sap-high-availability-guide/3018-config-wsfc-12.png
[sap-ha-guide-figure-3019]:./media/virtual-machines-shared-sap-high-availability-guide/3019-assign-permissions-on-share-for-cluster-name-object.png
[sap-ha-guide-figure-3020]:./media/virtual-machines-shared-sap-high-availability-guide/3020-change-object-type-include-computer-objects.png
[sap-ha-guide-figure-3021]:./media/virtual-machines-shared-sap-high-availability-guide/3021-check-box-for-computer-objects.png
[sap-ha-guide-figure-3022]:./media/virtual-machines-shared-sap-high-availability-guide/3022-set-security-attributes-for-cluster-name-object-on-file-share-quorum.png
[sap-ha-guide-figure-3023]:./media/virtual-machines-shared-sap-high-availability-guide/3023-call-configure-cluster-quorum-setting-wizard.png
[sap-ha-guide-figure-3024]:./media/virtual-machines-shared-sap-high-availability-guide/3024-selection-screen-different-quorum-configurations.png
[sap-ha-guide-figure-3025]:./media/virtual-machines-shared-sap-high-availability-guide/3025-selection-screen-file-share-witness.png
[sap-ha-guide-figure-3026]:./media/virtual-machines-shared-sap-high-availability-guide/3026-define-file-share-location-for-witness-share.png
[sap-ha-guide-figure-3027]:./media/virtual-machines-shared-sap-high-availability-guide/3027-successful-reconfiguration-cluster-file-share-witness.png
[sap-ha-guide-figure-3028]:./media/virtual-machines-shared-sap-high-availability-guide/3028-install-dot-net-framework-35.png
[sap-ha-guide-figure-3029]:./media/virtual-machines-shared-sap-high-availability-guide/3029-install-dot-net-framework-35-progress.png
[sap-ha-guide-figure-3030]:./media/virtual-machines-shared-sap-high-availability-guide/3030-sios-installer.png
[sap-ha-guide-figure-3031]:./media/virtual-machines-shared-sap-high-availability-guide/3031-first-screen-sios-data-keeper-installation.png
[sap-ha-guide-figure-3032]:./media/virtual-machines-shared-sap-high-availability-guide/3032-data-keeper-informs-service-be-disabled.png
[sap-ha-guide-figure-3033]:./media/virtual-machines-shared-sap-high-availability-guide/3033-user-selection-sios-data-keeper.png
[sap-ha-guide-figure-3034]:./media/virtual-machines-shared-sap-high-availability-guide/3034-domain-user-sios-data-keeper.png
[sap-ha-guide-figure-3035]:./media/virtual-machines-shared-sap-high-availability-guide/3035-provide-sios-data-keeper-license.png
[sap-ha-guide-figure-3036]:./media/virtual-machines-shared-sap-high-availability-guide/3036-data-keeper-management-config-tool.png
[sap-ha-guide-figure-3037]:./media/virtual-machines-shared-sap-high-availability-guide/3037-tcp-ip-address-first-node-data-keeper.png
[sap-ha-guide-figure-3038]:./media/virtual-machines-shared-sap-high-availability-guide/3038-create-replication-sios-job.png
[sap-ha-guide-figure-3039]:./media/virtual-machines-shared-sap-high-availability-guide/3039-define-sios-replication-job-name.png
[sap-ha-guide-figure-3040]:./media/virtual-machines-shared-sap-high-availability-guide/3040-define-sios-source-node.png
[sap-ha-guide-figure-3041]:./media/virtual-machines-shared-sap-high-availability-guide/3041-define-sios-target-node.png
[sap-ha-guide-figure-3042]:./media/virtual-machines-shared-sap-high-availability-guide/3042-define-sios-synchronous-replication.png
[sap-ha-guide-figure-3043]:./media/virtual-machines-shared-sap-high-availability-guide/3043-enable-sios-replicated-volume-as-cluster-volume.png
[sap-ha-guide-figure-3044]:./media/virtual-machines-shared-sap-high-availability-guide/3044-data-keeper-synchronous-mirroring-for-SAP-gui.png
[sap-ha-guide-figure-3045]:./media/virtual-machines-shared-sap-high-availability-guide/3045-replicated-disk-by-data-keeper-in-wsfc.png
[sap-ha-guide-figure-3046]:./media/virtual-machines-shared-sap-high-availability-guide/3046-dns-entry-sap-ascs-virtual-name-ip.png
[sap-ha-guide-figure-3047]:./media/virtual-machines-shared-sap-high-availability-guide/3047-dns-manager.png
[sap-ha-guide-figure-3048]:./media/virtual-machines-shared-sap-high-availability-guide/3048-default-cluster-probe-port.png
[sap-ha-guide-figure-3049]:./media/virtual-machines-shared-sap-high-availability-guide/3049-cluster-probe-port-after.png
[sap-ha-guide-figure-3050]:./media/virtual-machines-shared-sap-high-availability-guide/3050-service-type-ers-delayed-automatic.png
[sap-ha-guide-figure-5000]:./media/virtual-machines-shared-sap-high-availability-guide/5000-wsfc-sap-sid-node-a.png
[sap-ha-guide-figure-5001]:./media/virtual-machines-shared-sap-high-availability-guide/5001-sios-replicating-local-volume.png
[sap-ha-guide-figure-5002]:./media/virtual-machines-shared-sap-high-availability-guide/5002-wsfc-sap-sid-node-b.png
[sap-ha-guide-figure-5003]:./media/virtual-machines-shared-sap-high-availability-guide/5003-sios-replicating-local-volume-b-to-a.png

[sap-ha-guide-figure-6003]:./media/virtual-machines-shared-sap-high-availability-guide/6003-sap-multi-sid-full-landscape.png

[sap-templates-3-tier-multisid-xscs-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs%2Fazuredeploy.json
[sap-templates-3-tier-multisid-xscs-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps-md%2Fazuredeploy.json

[virtual-machines-azure-resource-manager-architecture-benefits-arm]:../../../azure-resource-manager/management/overview.md#the-benefits-of-using-resource-manager

[virtual-machines-manage-availability]:../../virtual-machines-windows-manage-availability.md


> ![Windows][Logo_Windows] Windows
>

Bu makalede, bir SAP ASCS örneğini kümelemek için bir seçenek olarak *bir küme paylaşılan disk* kullanarak Windows failover kümesinde yüksek kullanılabilirliksap sap sistemi yüklemek ve yapılandırmak için Azure altyapısını hazırlamak için attığınız adımlar açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Yüklemeye başlamadan önce bu makaleyi inceleyin:

* [Mimari kılavuzu: Bir küme paylaşılan disk kullanarak Windows failover kümesinde sap ASCS/SCS örneğini kümeleme][sap-high-availability-guide-wsfc-shared-disk]

## <a name="prepare-the-infrastructure-for-architectural-template-1"></a>Mimari Şablon 1 için altyapıyı hazırlama
SAP için Azure Kaynak Yöneticisi şablonları, gerekli kaynakların dağıtımını basitleştirmeye yardımcı olur.

Azure Kaynak Yöneticisi'ndeki üç katmanlı şablonlar da yüksek kullanılabilirlik senaryolarını destekler. Örneğin, Mimari Şablon 1'in iki kümesi vardır. Her küme SAP ASCS/SCS ve DBMS için sap tek bir hata noktasıdır.

Bu makalede açıkladığımız örnek senaryo için Azure Kaynak Yöneticisi şablonlarını buradan alabilirsiniz:

* [Azure Marketi görüntüsü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image)  
* [Azure Yönetilen Diskler kullanarak Azure Marketi görüntüsü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-md)  
* [Özel görüntü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image)
* [Yönetilen Diskler kullanarak özel görüntü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-md)

Mimari Şablon 1 için altyapı hazırlamak için:

- Azure portalında, **Parametreler** bölmesinde, **SYSTEMAVAILABILITY** **kutusunda, HA'** yı seçin.

  ![Şekil 1: SAP yüksek kullanılabilirlikAzure Kaynak Yöneticisi parametrelerini ayarlama][sap-ha-guide-figure-3000]

_**Şekil 1:** SAP yüksek kullanılabilirlik Azure Kaynak Yöneticisi parametrelerini ayarlama_


  Şablonlar oluşturur:

  * **Sanal makineler:**
    * SAP Application Server sanal \<makineleri:\>SAPSystemSID -di-\<Sayı\>
    * ASCS/SCS küme sanal \<makineleri:\>SAPSystemSID -ascs-\<Sayı\>
    * DBMS kümesi: \<SAPSystemSID\>-db-\<Sayı\>

  * **İlişkili IP adreslerine sahip tüm sanal makineler için ağ kartları:**
    * \<SAPSystemSID\>-nic-di-\<Sayısı\>
    * \<SAPSystemSID\>-nic-ascs-\<Sayı\>
    * \<SAPSystemSID\>-nic-db-\<Sayı\>

  * **Azure depolama hesapları (yalnızca yönetilmeyen diskler)**:

  * **Kullanılabilirlik grupları:**
    * SAP Application Server sanal \<makineleri:\>SAPSystemSID -avset-di
    * SAP ASCS/SCS küme sanal \<makineleri:\>SAPSystemSID -avset-ascs
    * DBMS küme sanal \<makineleri:\>SAPSystemSID -avset-db

  * **Azure dahili yük dengeleyicisi**:
    * ASCS/SCS örneği ve IP adresi \<SAPSystemSID\>-lb-ascs için tüm bağlantı noktaları ile
    * SQL Server DBMS ve IP adresi \<SAPSystemSID\>-lb-db için tüm bağlantı noktaları ile

  * **Ağ güvenlik** \<grubu :\>SAPSystemSID -nsg-ascs-0  
    * SAPSystemSID \<\>-ascs-0 sanal makineye açık harici Uzak Masaüstü Protokolü (RDP) bağlantı noktası ile

> [!NOTE]
> Ağ kartlarının ve Azure dahili yük dengeleyicilerinin tüm IP adresleri varsayılan olarak dinamiktir. Bunları statik IP adresleriyle değiştirin. Bunu daha sonra makalede nasıl yapacağımızı açıklayacağız.
>
>

## <a name="deploy-virtual-machines-with-corporate-network-connectivity-cross-premises-to-use-in-production"></a><a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a>Üretimde kullanmak üzere kurumsal ağ bağlantısına (tesisler arası) sahip sanal makineleri dağıtma
Üretim SAP sistemleri için Azure VPN Ağ Geçidi veya Azure ExpressRoute kullanarak kurumsal ağ bağlantısına sahip Azure sanal makinelerini dağıtın.

> [!NOTE]
> Azure Sanal Ağ örneğini kullanabilirsiniz. Sanal ağ ve alt ağ zaten oluşturuldu ve hazırlandı.
>
>

1. Azure portalında, **Parametreler** bölmesinde, **NEWOREXISTINGSUBNET** kutusunda **varolan'ı**seçin.
2. **SUBNETID** kutusuna, Azure sanal makinelerinizi dağıtmayı planladığınız hazırlanmış Azure ağ alt net kimliğinin tam dizesini ekleyin.
3. Tüm Azure ağ alt ağlarının listesini almak için bu PowerShell komutunu çalıştırın:

   ```powershell
   (Get-AzVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets
   ```

   **Kimlik** alanı alt net kimliğinin değerini gösterir.
4. Tüm alt net kimlik değerlerinin listesini almak için bu PowerShell komutunu çalıştırın:

   ```powershell
   (Get-AzVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets.Id
   ```

   Alt net kimliği şuna benzer:

   ```
   /subscriptions/<subscription ID>/resourceGroups/<VPN name>/providers/Microsoft.Network/virtualNetworks/azureVnet/subnets/<subnet name>
   ```

## <a name="deploy-cloud-only-sap-instances-for-test-and-demo"></a><a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a>Test ve demo için yalnızca buluta özel SAP örneklerini dağıtma
Yüksek kullanılabilirlik sap sisteminizi yalnızca buluta özel dağıtım modelinde dağıtabilirsiniz. Bu tür bir dağıtım öncelikle demo ve test kullanım örnekleri için yararlıdır. Üretim kullanım kılıfları için uygun değil.

- Azure portalında, **Parametreler** bölmesinde, **NEWOREXISTINGSUBNET** kutusunda **yeni**seçeneğini belirleyin. **SUBNETID** alanını boş bırakın.

  SAP Azure Kaynak Yöneticisi şablonu otomatik olarak Azure sanal ağı ve alt ağı oluşturur.

> [!NOTE]
> Ayrıca, aynı Azure Sanal Ağ örneğinde Active Directory ve DNS hizmeti için en az bir sanal makine dağıtmanız gerekir. Şablon bu sanal makineleri oluşturmaz.
>
>


## <a name="prepare-the-infrastructure-for-architectural-template-2"></a>Mimari Şablon 2 için altyapıyı hazırlama

SAP Mimari Şablon 2 için gerekli altyapı kaynaklarının dağıtımını basitleştirmeye yardımcı olmak için SAP için bu Azure Kaynak Yöneticisi şablonu kullanabilirsiniz.

Bu dağıtım senaryosu için Azure Kaynak Yöneticisi şablonlarını buradan alabilirsiniz:

* [Azure Marketi görüntüsü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged)  
* [Yönetilen Diskler kullanarak Azure Marketi görüntüsü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged-md)  
* [Özel görüntü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged)
* [Yönetilen Diskler kullanarak özel görüntü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged-md)


## <a name="prepare-the-infrastructure-for-architectural-template-3"></a>Mimari Şablon 3 için altyapıyı hazırlama

Altyapıyı hazırlayabilir ve SAP'yi multi-SID için yapılandırabilirsiniz. Örneğin, *varolan* bir küme yapılandırmasına ek bir SAP ASCS/SCS örneği ekleyebilirsiniz. Daha fazla bilgi için bkz: [Azure Kaynak Yöneticisi'nde bir SAP multi-SID yapılandırması oluşturmak için varolan bir küme yapılandırması için ek bir SAP ASCS/SCS örneğini yapılandırın.][sap-ha-multi-sid-guide]

Yeni bir multi-SID kümesi oluşturmak istiyorsanız, [GitHub'da](https://github.com/Azure/azure-quickstart-templates)çoklu SID hızlı başlatma şablonlarını kullanabilirsiniz.

Yeni bir multi-SID kümesi oluşturmak için aşağıdaki üç şablonu dağıtmanız gerekir:

* [ASCS/SCS şablonu](#ASCS-SCS-template)
* [Veritabanı şablonu](#database-template)
* [Uygulama sunucuları şablonu](#application-servers-template)

Aşağıdaki bölümlerde şablonlarda sağlamanız gereken şablonlar ve parametreler hakkında daha fazla ayrıntı vardır.

### <a name="ascsscs-template"></a><a name="ASCS-SCS-template"></a>ASCS/SCS şablonu

ASCS/SCS şablonu, birden çok ASCS/SCS örneğini barındıran bir Windows Server başarısız kümesi oluşturmak için kullanabileceğiniz iki sanal makine dağıtır.

ASCS/SCS multi-SID şablonu kurmak için, [ASCS/SCS multi-SID şablonu][sap-templates-3-tier-multisid-xscs-marketplace-image] veya [ASCS/SCS multi-SID şablonu Yönetilen Diskler kullanarak,][sap-templates-3-tier-multisid-xscs-marketplace-image-md]aşağıdaki parametreler için değerleri girin:

- **Kaynak Önek**: Dağıtım sırasında oluşturulan tüm kaynakları önek için kullanılan kaynak önekisini ayarlayın. Kaynaklar yalnızca bir SAP sistemine ait olmadığından, kaynağın öneki bir SAP sisteminin SID'si değildir.  Önek üç ve altı karakter arasında olmalıdır.
- **Yığın Türü**: SAP sisteminin yığın türünü seçin. Yığın türüne bağlı olarak, Azure Yük Dengeleyici'nin SAP sistemi başına bir (yalnızca ABAP veya Java) veya iki (ABAP+Java) özel IP adresi vardır.
- **İşletim Sistemi Türü**: Sanal makinelerin işletim sistemini seçin.
- **SAP Sistem Sayısı**: Bu kümeye yüklemek istediğiniz SAP sistemlerinin sayısını seçin.
- **Sistem Kullanılabilirliği**: **SELECT HA**.
- **Yönetici Kullanıcı Adı ve Yönetici Şifresi**: Makinede oturum açmada kullanılabilecek yeni bir kullanıcı oluşturun.
- **Yeni veya Varolan Alt Ağ**: Yeni bir sanal ağ ve alt ağ oluşturup oluşturmayacağını veya varolan bir alt ağı kullanıp kullanmayacağını ayarlayın. Şirket içi ağınıza bağlı bir sanal ağınız varsa, **varolan**ağınızı seçin.
- **Subnet Id**: VM'yi, vm'nin atanması gereken tanımlanmış bir alt ağına yerleştirmek istiyorsanız, o alt ağın kimliğini adlandırın. Kimlik genellikle şu na benzer:

  /subscriptions/\<subscription\>id\</resourceGroups/\>kaynak grup adı /providers/Microsoft.Network/virtualNetworks/\<sanal ağ adı\>/subnets/\<subnet adı\>

Şablon, birden çok SAP sistemlerini destekleyen bir Azure Yük Dengeleyici örneğini dağıtır:

- ASCS örnekleri 00, 10, 20...
- SCS örnekleri örneğinde 01, 11, 21...
- ASCS Enqueue Çoğaltma Sunucusu (ERS) (yalnızca Linux) örnekleri 02, 12, 22...
- SCS ERS (yalnızca Linux) örnekleri 03, 13, 23...

Yük dengeleyicisi 1 VIP(ler) (2 Linux için), ASCS/SCS için 1x VIP ve ERS için 1x VIP (yalnızca Linux) içerir.

#### <a name="sap-ascsscs-ports"></a><a name="0f3ee255-b31e-4b8a-a95a-d9ed6200468b"></a>SAP ASCS/SCS bağlantı noktaları
Aşağıdaki liste tüm yük dengeleme kurallarını içerir (x SAP sisteminin sayısıdır, örneğin, 1, 2, 3...):
- Her SAP sistemi için Windows'a özel bağlantı noktaları: 445, 5985
- ASCS bağlantı noktaları (örnek numarası x0): 32x0, 36x0, 39x0, 81x0, 5x013, 5x014, 5x016
- SCS bağlantı noktaları (örnek numarası x1): 32x1, 33x1, 39x1, 81x1, 5x113, 5x114, 5x116
- Linux üzerinde ASCS ERS bağlantı noktaları (örnek numarası x2): 33x2, 5x213, 5x214, 5x216
- Linux'ta SCS ERS bağlantı noktaları (örnek numarası x3): 33x3, 5x313, 5x314, 5x316

Yük dengeleyicisi aşağıdaki sonda bağlantı noktalarını kullanacak şekilde yapılandırılır (x SAP sisteminin sayısıdır, örneğin, 1, 2, 3...):
- ASCS/SCS dahili yük dengeleyici prob bağlantı noktası: 620x0
- ERS dahili yük dengeleyici prob bağlantı noktası (sadece Linux): 621x2

### <a name="database-template"></a><a name="database-template"></a>Veritabanı şablonu

Veritabanı şablonu, bir SAP sistemi için ilişkisel veritabanı yönetim sistemini (RDBMS) yüklemek için kullanabileceğiniz bir veya iki sanal makinedağıdağır. Örneğin, beş SAP sistemi için bir ASCS/SCS şablonu dağıtırsanız, bu şablonu beş kez dağıtmanız gerekir.

Yönetilen Diskler kullanarak [veritabanı multi-SID şablonu][sap-templates-3-tier-multisid-db-marketplace-image] veya [veritabanı multi-SID şablonu,][sap-templates-3-tier-multisid-db-marketplace-image-md]veritabanı multi-SID şablonu kurmak için, aşağıdaki parametreler için değerleri girin:

- **Sap Sistem Kimliği**: Yüklemek istediğiniz SAP sisteminin SAP sistem kimliğini girin. Kimlik, dağıtılan kaynaklar için önek olarak kullanılır.
- **Os Türü**: Sanal makinelerin işletim sistemini seçin.
- **Dbtype**: Kümeye yüklemek istediğiniz veritabanı türünü seçin. Microsoft SQL Server'ı yüklemek istiyorsanız **SQL'i** seçin. SAP HANA'yı sanal makinelere yüklemeyi planlıyorsanız **HANA'yı** seçin. Doğru işletim sistemi türünü seçtiğinizden emin olun. SQL için **Windows'u** seçin ve HANA için bir Linux dağıtımı seçin. Sanal makinelere bağlı Azure Yük Dengeleyicisi, seçili veritabanı türünü destekleyecek şekilde yapılandırılır:
  * **SQL**: Yük bakiyesi yük dengesi bağlantı noktası 1433. SQL Server AlwaysOn kurulumunuz için bu bağlantı noktasını kullandığınızdan emin olun.
  * **HANA**: Yük dengeleyici yük dengesi portları 35015 ve 35017. Sap HANA'yı örnek numarası **50**ile yüklediğinizden emin olun.
  Yük dengeleyicisi prob portu 62550 kullanır.
- **Sap Sistem Boyutu**: Yeni sistemin sağladığı SAPS sayısını ayarlayın. Sistemin kaç SAPS gerektirdiğinden emin değilseniz, SAP Teknoloji Ortağınıza veya Sistem Entegratörünüze danışın.
- **Sistem Kullanılabilirliği**: **SELECT HA**.
- **Yönetici Kullanıcı Adı ve Yönetici Şifresi**: Makinede oturum açmada kullanılabilecek yeni bir kullanıcı oluşturun.
- **Subnet Id**: ASCS/SCS şablonunun dağıtımı sırasında kullandığınız alt netin kimliğini veya ASCS/SCS şablon dağıtımının bir parçası olarak oluşturulan alt netin kimliğini girin.

### <a name="application-servers-template"></a><a name="application-servers-template"></a>Uygulama sunucuları şablonu

Uygulama sunucuları şablonu, bir SAP sistemi için SAP Application Server örnekleri olarak kullanılabilecek iki veya daha fazla sanal makine dağıtmaktadır. Örneğin, beş SAP sistemi için bir ASCS/SCS şablonu dağıtırsanız, bu şablonu beş kez dağıtmanız gerekir.

Uygulama sunucuları multi-SID şablonu kurmak için, [uygulama sunucularında multi-SID şablonu][sap-templates-3-tier-multisid-apps-marketplace-image] veya [uygulama sunucularında yönetilen Diskler kullanarak multi-SID şablonu,][sap-templates-3-tier-multisid-apps-marketplace-image-md]aşağıdaki parametreler için değerleri girin:

  -  **Sap Sistem Kimliği**: Yüklemek istediğiniz SAP sisteminin SAP sistem kimliğini girin. Kimlik, dağıtılan kaynaklar için önek olarak kullanılır.
  -  **Os Türü**: Sanal makinelerin işletim sistemini seçin.
  -  **Sap Sistem Boyutu**: Yeni sistemin sağladığı SAPS sayısıdır. Sistemin kaç SAPS gerektirdiğinden emin değilseniz, SAP Teknoloji Ortağınıza veya Sistem Entegratörünüze danışın.
  -  **Sistem Kullanılabilirliği**: **SELECT HA**.
  -  **Yönetici Kullanıcı Adı ve Yönetici Şifresi**: Makinede oturum açmada kullanılabilecek yeni bir kullanıcı oluşturun.
  -  **Subnet Id**: ASCS/SCS şablonunun dağıtımı sırasında kullandığınız alt netin kimliğini veya ASCS/SCS şablon dağıtımının bir parçası olarak oluşturulan alt netin kimliğini girin.


## <a name="azure-virtual-network"></a><a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a>Azure Sanal Ağ
Örneğimizde, Azure Sanal Ağ örneğinin adres alanı 10.0.0.0/16'dır. Adres aralığı 10.0.0.0/24 olan Subnet adında bir alt ağ vardır. Tüm sanal makineler ve dahili yük dengeleyicileri bu sanal ağda dağıtılır.

> [!IMPORTANT]
> Konuk işletim sistemi içindeki ağ ayarlarında herhangi bir değişiklik yapmayın. Buna IP adresleri, DNS sunucuları ve alt ağ dahildir. Azure'da tüm ağ ayarlarınızı yapılandırın. Dinamik Ana Bilgisayar Yapılandırma Protokolü (DHCP) hizmeti ayarlarınızı yayır.
>
>

## <a name="dns-ip-addresses"></a><a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a>DNS IP adresleri

Gerekli DNS IP adreslerini ayarlamak için aşağıdaki adımları tamamlayın:

1. Azure portalında, **DNS sunucular** bölmesinde, sanal ağ **DNS sunucular** seçeneğinizin **Özel DNS**olarak ayarlandığından emin olun.
2. Ayarlarınızı sahip olduğunuz ağ türüne göre seçin. Daha fazla bilgi için aşağıdaki kaynaklara bakın:
   * Şirket içi DNS sunucularının IP adreslerini ekleyin.  
   Şirket içi DNS sunucularını Azure'da çalışan sanal makinelere genişletebilirsiniz. Bu senaryoda, DNS hizmetini çalıştırdığınız Azure sanal makinelerinin IP adreslerini ekleyebilirsiniz.
   * Azure'da yalıtılmış VM dağıtımları için: DNS sunucusu olarak hizmet veren aynı Sanal Ağ örneğinde ek bir sanal makine dağıtın. DNS hizmetini çalıştırmak için ayarladığınız Azure sanal makinelerinIN IP adreslerini ekleyin.

   ![Şekil 2: Azure Sanal Ağı için DNS sunucularını yapılandırma][sap-ha-guide-figure-3001]

   _**Şekil 2:** Azure Sanal Ağı için DNS sunucularını yapılandırma_

   > [!NOTE]
   > DNS sunucularının IP adreslerini değiştirirseniz, değişikliği uygulamak ve yeni DNS sunucularını yaymak için Azure sanal makinelerini yeniden başlatmanız gerekir.
   >
   >

Örneğimizde, DNS hizmeti bu Windows sanal makinelerde yüklenir ve yapılandırılır:

| Sanal makine rolü | Sanal makine ana bilgisayar adı | Ağ kartı adı | Statik IP adresi |
| --- | --- | --- | --- |
| İlk DNS sunucusu |domcontr-0 |pr1-nic-domcontr-0 |10.0.0.10 |
| İkinci DNS sunucusu |domcontr-1 |pr1-nic-domcontr-1 |10.0.0.11 |

## <a name="host-names-and-static-ip-addresses-for-the-sap-ascsscs-clustered-instance-and-dbms-clustered-instance"></a><a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a>SAP ASCS/SCS kümelenmiş örnek ve DBMS kümelenmiş örnek için ana bilgisayar adları ve statik IP adresleri

Şirket içi dağıtım için bu ayrılmış ana bilgisayar adları ve IP adreslerine ihtiyacınız vardır:

| Sanal ana bilgisayar adı rolü | Sanal ana bilgisayar adı | Sanal statik IP adresi |
| --- | --- | --- |
| SAP ASCS/SCS ilk küme sanal ana bilgisayar adı (küme yönetimi için) |pr1-ascs-vir |10.0.0.42 |
| SAP ASCS/SCS örnek sanal ana bilgisayar adı |pr1-ascs-sap |10.0.0.43 |
| SAP DBMS ikinci küme sanal ana bilgisayar adı (küme yönetimi) |pr1-dbms-vir |10.0.0.32 |

Kümeyi oluşturduğunuzda, sanal ana bilgisayar adlarını pr1-ascs-vir ve pr1-dbms-vir ve kümenin kendisini yöneten ilişkili IP adreslerini oluşturun. Bunun nasıl yapılacağını öğrenmek için bkz. küme [yapılandırmasında küme düğümlerini topla.][sap-high-availability-infrastructure-wsfc-shared-disk-collect-cluster-config]

DNS sunucusunda diğer iki sanal ana bilgisayar adı, pr1-ascs-sap ve pr1-dbms-sap ve ilişkili IP adreslerini el ile oluşturabilirsiniz. Kümelenmiş SAP ASCS/SCS örneği ve kümelenmiş DBMS örneği bu kaynakları kullanır. Bunun nasıl yapılacağını öğrenmek için [bkz.][sap-ha-guide-9.1.1]

## <a name="set-static-ip-addresses-for-the-sap-virtual-machines"></a><a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a>SAP sanal makineleri için statik IP adreslerini ayarlama
Kümenizde kullanmak üzere sanal makineleri dağıttıktan sonra, tüm sanal makineler için statik IP adresleri ayarlamanız gerekir. Bunu konuk işletim sisteminde değil, Azure Sanal Ağ yapılandırmasında yapın.

1. Azure portalında **Kaynak Grubu** > **Ağ Kartı** > **Ayarları** > **IP Adresini**seçin.
2. IP **adresleri** bölmesinde, **Atama**altında **Statik'i**seçin. IP **adres** kutusuna, kullanmak istediğiniz IP adresini girin.

   > [!NOTE]
   > Ağ kartının IP adresini değiştirirseniz, değişikliği uygulamak için Azure sanal makinelerini yeniden başlatmanız gerekir.  
   >
   >

   ![Şekil 3: Her sanal makinenin ağ kartı için statik IP adreslerini ayarlama][sap-ha-guide-figure-3002]

   _**Şekil 3:** Her sanal makinenin ağ kartı için statik IP adreslerini ayarlama_

   Active Directory veya DNS hizmetiniz için kullanmak istediğiniz sanal makineler de dahil olmak üzere tüm sanal makineler için tüm ağ arabirimleri için bu adımı yineleyin.

Örneğimizde, bu sanal makineler ve statik IP adresleri vardır:

| Sanal makine rolü | Sanal makine ana bilgisayar adı | Ağ kartı adı | Statik IP adresi |
| --- | --- | --- | --- |
| İlk SAP uygulama sunucusu örneği |pr1-di-0 |pr1-nic-di-0 |10.0.0.50 |
| İkinci SAP Application Server örneği |pr1-di-1 |pr1-nic-di-1 |10.0.0.51 |
| ... |... |... |... |
| Son SAP uygulama sunucusu örneği |pr1-di-5 |pr1-nic-di-5 |10.0.0.55 |
| ASCS/SCS örneği için ilk küme düğümü |pr1-ascs-0 |pr1-nic-ascs-0 |10.0.0.40 |
| ASCS/SCS örneği için ikinci küme düğümü |pr1-ascs-1 |pr1-nic-ascs-1 |10.0.0.41 |
| DBMS örneği için ilk küme düğümü |pr1-db-0 |pr1-nic-db-0 |10.0.0.30 |
| DBMS örneği için ikinci küme düğümü |pr1-db-1 |pr1-nic-db-1 |10.0.0.31 |

## <a name="set-a-static-ip-address-for-the-azure-internal-load-balancer"></a><a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a>Azure dahili yük dengeleyicisi için statik bir IP adresi ayarlama

SAP Azure Kaynak Yöneticisi şablonu, SAP ASCS/SCS örnek kümesi ve DBMS kümesi için kullanılan bir Azure dahili yük dengeleyicisi oluşturur.

> [!IMPORTANT]
> SAP ASCS/SCS'nin sanal ana bilgisayar adının IP adresi, SAP ASCS/SCS dahili yük dengeleyicisinin IP adresiyle aynıdır: pr1-lb-ascs.
> DBMS'nin sanal adının IP adresi, DBMS dahili yük dengeleyicisinin IP adresiyle aynıdır: pr1-lb-dbms.
>
>

Azure dahili yük dengeleyicisi için statik bir IP adresi ayarlamak için:

1. İlk dağıtım, dahili yük dengeleyiciip IP adresini **Dinamik**olarak ayarlar. Azure portalında, **IP adresleri** bölmesinde, **Atama**altında **Statik'i**seçin.
2. Dahili yük **dengeleyicipr1-lb-ascs** IP adresini SAP ASCS/SCS örneğinin sanal ana bilgisayar adının IP adresine ayarlayın.
3. Dahili yük **dengeleyicipr1-lb-dbms** IP adresini DBMS örneğinin sanal ana bilgisayar adının IP adresine ayarlayın.

   ![Şekil 4: SAP ASCS/SCS örneği için dahili yük dengeleyicisi için statik IP adreslerini ayarlama][sap-ha-guide-figure-3003]

   _**Şekil 4:** SAP ASCS/SCS örneği için dahili yük dengeleyicisinin statik IP adreslerini ayarlama_

Örneğimizde, şu statik IP adreslerine sahip iki Azure dahili yük dengeleyicimiz vardır:

| Azure dahili yük dengeleyicisi rolü | Azure dahili yük dengeleyici adı | Statik IP adresi |
| --- | --- | --- |
| SAP ASCS/SCS örnek dahili yük dengeleyicisi |pr1-lb-ascs |10.0.0.43 |
| SAP DBMS dahili yük dengeleyicisi |pr1-lb-dbms |10.0.0.33 |


## <a name="default-ascsscs-load-balancing-rules-for-the-azure-internal-load-balancer"></a><a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a>Azure dahili yük dengeleyicisi için varsayılan ASCS/SCS yük dengeleme kuralları

SAP Azure Kaynak Yöneticisi şablonu, gereksinim duyduğunuz bağlantı noktalarını oluşturur:
* Varsayılan örnek numarası 00 olan bir ABAP ASCS örneği
* Varsayılan örnek numarası 01 olan bir Java SCS örneği

SAP ASCS/SCS örneğini yüklediğinizde, ABAP ASCS örneğiniz için varsayılan örnek 00 sayısını ve Java SCS örneğiniz için varsayılan örnek 01'i kullanmanız gerekir.

Ardından, SAP NetWeaver bağlantı noktaları için gerekli iç yük dengeleme uç noktalarını oluşturun.

Gerekli iç yük dengeleme uç noktalarını oluşturmak için, ilk olarak, SAP NetWeaver ABAP ASCS bağlantı noktaları için bu yük dengeleme uç noktalarını oluşturun:

| Hizmet/yük dengeleme kural adı | Varsayılan bağlantı noktası numaraları | Beton bağlantı noktaları (00 numaralı ASCS örneğinde) (10 ile ERS) |
| --- | --- | --- |
| Enqueue sunucu / *lbrule3200* |32\<Örnek Numarası\> |3200 |
| ABAP mesaj sunucusu / *lbrule3600* |36\<Örnek Numarası\> |3600 |
| Dahili ABAP iletisi / *lbrule3900* |39\<Örnek Numarası\> |3900 |
| Mesaj sunucusu HTTP / *Lbrule8100* |81\<Örnek Numarası\> |8100 |
| SAP başlangıç hizmeti ASCS HTTP / *Lbrule50013* |5\<Örnek\>Sayı 13 |50013 |
| SAP başlangıç hizmeti ASCS HTTPS / *Lbrule50014* |5\<Örnek\>Sayı 14 |50014 |
| Enqueue çoğaltma / *Lbrule50016* |5\<Örnek\>Sayı 16 |50016 |
| SAP başlangıç servisi ERS HTTP *Lbrule51013* |5\<Örnek\>Sayı 13 |51013 |
| SAP başlangıç servisi ERS HTTP *Lbrule51014* |5\<Örnek\>Sayı 14 |51014 |
| Windows Uzaktan Yönetim (WinRM) *Lbrule5985* | |5985 |
| Dosya paylaşımı *Lbrule445* | |445 |

**Tablo 1:** SAP NetWeaver ABAP ASCS örneklerinin bağlantı noktası numaraları

Ardından, SAP NetWeaver Java SCS bağlantı noktaları için bu yük dengeleme uç noktalarını oluşturun:

| Hizmet/yük dengeleme kural adı | Varsayılan bağlantı noktası numaraları | Beton bağlantı noktaları (01 numaralı SCS örneği) (11 ile ERS) |
| --- | --- | --- |
| Enqueue sunucu / *lbrule3201* |32\<Örnek Numarası\> |3201 |
| Ağ geçidi sunucusu / *lbrule3301* |33\<Örnek Numarası\> |3301 |
| Java mesaj sunucusu / *lbrule3900* |39\<Örnek Numarası\> |3901 |
| Mesaj sunucusu HTTP / *Lbrule8101* |81\<Örnek Numarası\> |8101 |
| SAP başlangıç hizmeti SCS HTTP / *Lbrule50113* |5\<Örnek\>Sayı 13 |50113 |
| SAP başlangıç hizmeti SCS HTTPS / *Lbrule50114* |5\<Örnek\>Sayı 14 |50114 |
| Enqueue çoğaltma / *Lbrule50116* |5\<Örnek\>Sayı 16 |50116 |
| SAP başlangıç servisi ERS HTTP *Lbrule51113* |5\<Örnek\>Sayı 13 |51113 |
| SAP başlangıç servisi ERS HTTP *Lbrule51114* |5\<Örnek\>Sayı 14 |51114 |
| WinRM *Lbrule5985* | |5985 |
| Dosya paylaşımı *Lbrule445* | |445 |

**Tablo 2:** SAP NetWeaver Java SCS örneklerinin bağlantı noktası numaraları

![Şekil 5: Azure dahili yük dengeleyicisi için Varsayılan ASCS/SCS yük dengeleme kuralları][sap-ha-guide-figure-3004]

_**Şekil 5:** Azure dahili yük dengeleyicisi için varsayılan ASCS/SCS yük dengeleme kuralları_

Yük dengeleyici pr1-lb-dbms IP adresini DBMS örneğinin sanal ana bilgisayar adının IP adresine ayarlayın.

### <a name="change-the-ascsscs-default-load-balancing-rules-for-the-azure-internal-load-balancer"></a><a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a>Azure dahili yük dengeleyicisi için ASCS/SCS varsayılan yük dengeleme kurallarını değiştirme

SAP ASCS veya SCS örnekleri için farklı sayılar kullanmak istiyorsanız, bağlantı noktalarının adlarını ve değerlerini varsayılan değerlerden değiştirmeniz gerekir.

1. Azure portalında ** \<SID\>-lb-ascs yük dengeleyici** > **yük dengeleme kurallarını**seçin.
2. SAP ASCS veya SCS örneğine ait tüm yük dengeleme kuralları için aşağıdaki değerleri değiştirin:

   * Adı
   * Bağlantı noktası
   * Arka uç bağlantı noktası

   Örneğin, varsayılan ASCS örnek numarasını 00'dan 31'e değiştirmek istiyorsanız, Tablo 1'de listelenen tüm bağlantı noktaları için değişiklikler yapmanız gerekir.

   Burada bağlantı noktası *lbrule3200*için bir güncelleştirme örneği.

   ![Şekil 6: Azure dahili yük dengeleyicisi için ASCS/SCS varsayılan yük dengeleme kurallarını değiştirme][sap-ha-guide-figure-3005]

   _**Şekil 6:** Azure dahili yük dengeleyicisi için ASCS/SCS varsayılan yük dengeleme kurallarını değiştirme_

## <a name="add-windows-virtual-machines-to-the-domain"></a><a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a>Etki alanına Windows sanal makineleri ekleme

Sanal makinelere statik bir IP adresi atadıktan sonra, sanal makineleri etki alanına ekleyin.

![Şekil 7: Etki alanına sanal makine ekleme][sap-ha-guide-figure-3006]

_**Şekil 7:** Etki alanına sanal makine ekleme_

## <a name="add-registry-entries-on-both-cluster-nodes-of-the-sap-ascsscs-instance"></a><a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a>SAP ASCS/SCS örneğinin her iki küme düğümüne de kayıt defteri girişleri ekleme

Azure Yük Dengeleyicisi, bağlantılar belirli bir süre (boşta zaman acısı) boşta yken bağlantıları kapatan bir dahili yük dengeleyiciye sahiptir. Sap iş süreçlerini iletişim örneklerinde ilk sıra/sıra silme isteğinin gönderilmesi gerekir başlamaz SAP enqueue işlemine açık bağlantılar açar. Bu bağlantılar genellikle çalışma süreci veya enqueue işlemi yeniden başlatılına kadar kurulu kalır. Ancak, bağlantı belirli bir süre için boştaysa, Azure iç yük dengeleyicisi bağlantıları kapatır. SAP iş süreci artık yoksa enqueue işlemine bağlantıyı yeniden oluşturduğundan, bu bir sorun değildir. Bu etkinlikler SAP işlemlerinin geliştirici izlerinde belgelenmiştir, ancak bu izlemelerde büyük miktarda ek içerik oluşturur. TCP/IP'yi `KeepAliveTime` ve `KeepAliveInterval` her iki küme düğümünü değiştirmek iyi bir fikirdir. TCP/IP parametrelerindeki bu değişiklikleri, makalenin ilerleyen saatlerinde açıklanan SAP profil parametreleriyle birleştirin.

SAP ASCS/SCS örneğinin her iki küme düğümüne de kayıt defteri girişleri eklemek için, öncelikle SAP ASCS/SCS için her iki Windows küme düğümüne de bu Windows kayıt defteri girişlerini ekleyin:

| Yol | HKLM\System\CurrentControlSet\Services\Tcpip\Parametreler |
| --- | --- |
| Değişken adı |`KeepAliveTime` |
| Değişken türü |REG_DWORD (Ondalık) |
| Değer |120000 |
| Belgelere bağlantı |[https://technet.microsoft.com/library/cc957549.aspx](https://technet.microsoft.com/library/cc957549.aspx) |

**Tablo 3:** İlk TCP/IP parametresini değiştirme

Ardından, SAP ASCS/SCS için her iki Windows küme düğümüne de bu Windows kayıt defteri girişini ekleyin:

| Yol | HKLM\System\CurrentControlSet\Services\Tcpip\Parametreler |
| --- | --- |
| Değişken adı |`KeepAliveInterval` |
| Değişken türü |REG_DWORD (Ondalık) |
| Değer |120000 |
| Belgelere bağlantı |[https://technet.microsoft.com/library/cc957548.aspx](https://technet.microsoft.com/library/cc957548.aspx) |

**Tablo 4:** İkinci TCP/IP parametresini değiştirme

Değişiklikleri uygulamak için her iki küme düğümlerini yeniden başlatın.

## <a name="set-up-a-windows-server-failover-cluster-for-an-sap-ascsscs-instance"></a><a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a>SAP ASCS/SCS örneği için windows server failover kümesi ni ayarlama

BIR SAP ASCS/SCS örneği için bir Windows Server failover kümesinin ayarlanması şu görevleri içerir:

- Küme düğümlerini küme yapılandırmasında toplayın.
- Küme dosyası paylaşım tanığını yapılandırın.

### <a name="collect-the-cluster-nodes-in-a-cluster-configuration"></a><a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a>Küme konfigürasyonunda küme düğümlerini toplama

1. Rol ve Özellikler Ekle Sihirbazı'nda, her iki küme düğümüne de başarısız kümeleme ekleyin.
2. Failover Cluster Manager'ı kullanarak failover kümesini ayarlayın. Failover Cluster Manager'da **Cluster Oluştur'u**seçin ve ardından yalnızca ilk kümenin (düğüm A) adını ekleyin. İkinci düğümü henüz eklemeyin; daha sonraki bir adımda ikinci düğümü eklersiniz.

   ![Şekil 8: İlk küme düğümünün sunucu veya sanal makine adını ekleme][sap-ha-guide-figure-3007]

   _**Şekil 8:** İlk küme düğümünün sunucu veya sanal makine adını ekleme_

3. Kümenin ağ adını (sanal ana bilgisayar adı) girin.

   ![Şekil 9: Küme adını girin][sap-ha-guide-figure-3008]

   _**Şekil 9:** Küme adını girin_

4. Kümeyi oluşturduktan sonra bir küme doğrulama testi çalıştırın.

   ![Şekil 10: Küme doğrulama denetimini çalıştırın][sap-ha-guide-figure-3009]

   _**Şekil 10:** Küme doğrulama denetimini çalıştırma_

   İşlemin bu noktasında disklerle ilgili uyarıları yok sayabilirsiniz. Daha sonra bir dosya paylaşım tanığı ve SIOS paylaşılan diskler eklersiniz. Bu aşamada, bir çoğunluk olması konusunda endişelenmenize gerek yok.

   ![Şekil 11: Çoğunluk diski bulunamadı][sap-ha-guide-figure-3010]

   _**Şekil 11:** Çoğunluk diski bulunamadı_

   ![Şekil 12: Çekirdek küme kaynağının yeni bir IP adresine ihtiyacı vardır][sap-ha-guide-figure-3011]

   _**Şekil 12:** Çekirdek küme kaynağının yeni bir IP adresine ihtiyacı vardır_

5. Çekirdek küme hizmetinin IP adresini değiştirin. Sunucunun IP adresi sanal makine düğümlerinden birine işaret ettiği için küme, çekirdek küme hizmetinin IP adresini değiştirene kadar başedemez. Bunu çekirdek küme hizmetinin IP kaynağının **Özellikler** sayfasında yapın.

   Örneğin, küme sanal ana bilgisayar adı pr1-ascs-vir için bir IP adresi (örneğimizde, 10.0.0.42) atamamız gerekir.

   ![Şekil 13: Özellikler iletişim kutusunda IP adresini değiştirin][sap-ha-guide-figure-3012]

   _**Şekil 13:** **Özellikler** iletişim kutusunda IP adresini değiştirin_

   ![Şekil 14: Küme için ayrılmış IP adresini atama][sap-ha-guide-figure-3013]

   _**Şekil 14:** Küme için ayrılmış IP adresini atama_

6. Küme sanal ana bilgisayar adını çevrimiçi duruma getirin.

   ![Şekil 15: Küme çekirdeği hizmeti, doğru IP adresiyle çalışır durumda][sap-ha-guide-figure-3014]

   _**Şekil 15:** Küme çekirdeği hizmeti, doğru IP adresiyle çalışır durumda_

7. İkinci küme düğümekleyin.

   Çekirdek küme hizmeti çalışmaya ve çalışmaya devam ettiğine göre, ikinci küme düğümekleyebilirsiniz.

   ![Şekil 16 İkinci küme düğümünü ekleme][sap-ha-guide-figure-3015]

   _**Şekil 16:** İkinci küme düğümü ekleme_

8. İkinci küme düğümü ana bilgisayarı için bir ad girin.

   ![Şekil 17: İkinci küme düğümü ana bilgisayar adını girin][sap-ha-guide-figure-3016]

   _**Şekil 17:** İkinci küme düğümü ana bilgisayar adını girin_

   > [!IMPORTANT]
   > Küme onay **kutusuna tüm uygun depolama alanını ekle'nin** *seçilmediğinden* emin olun.  
   >
   >

   ![Şekil 18: Onay kutusunu seçmeyin][sap-ha-guide-figure-3017]

   _**Şekil 18:** Onay kutusunu *seçmeyin*_

   Çoğunluk ve disklerle ilgili uyarıları yoksayabilirsiniz. [Sap ASCS/SCS küme paylaşım diski için SIOS DataKeeper Cluster Edition'da][sap-high-availability-infrastructure-wsfc-shared-disk-install-sios]açıklandığı gibi, çoğunluğu ayarlayacak ve diski daha sonra paylaşacaksınız.

   ![Şekil 19: Disk çoğunluğu yla ilgili uyarıları yoksay][sap-ha-guide-figure-3018]

   _**Şekil 19:** Disk çoğunluğu yla ilgili uyarıları yoksay_


#### <a name="configure-a-cluster-file-share-witness"></a><a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a>Küme dosyası paylaşım tanığını yapılandırma

Küme dosyası paylaşımı tanığını yapılandırmak şu görevleri içerir:

- Dosya paylaşımı oluşturun.
- Failover Cluster Manager'da dosya paylaşımı tanık çoğunluklarını ayarlayın.

#### <a name="create-a-file-share"></a><a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a>Dosya paylaşımı oluşturma

1. Çoğunluk diski yerine bir dosya paylaşım tanığı seçin. SIOS DataKeeper bu seçeneği destekler.

   Bu makaledeki örneklerde, dosya paylaşımı tanığı Azure'da çalışan Active Directory veya DNS sunucusundadır. Dosya paylaşım tanığına domcontr-0 denir. Azure'a vpn bağlantısı (VPN Ağ Geçidi veya Azure ExpressRoute üzerinden) yapılandırdığınız için, Active Directory veya DNS hizmetiniz şirket içindedir ve dosya paylaşımı tanığıçalıştırmak için uygun değildir.

   > [!NOTE]
   > Etkin Dizin veya DNS hizmetiniz yalnızca şirket içinde çalışıyorsa, dosya paylaşım tanığınızı şirket içinde çalışan Active Directory veya DNS Windows işletim sisteminde yapılandırmayın. Azure'da çalışan küme düğümleri ile Etkin Dizin veya şirket içi DNS arasındaki ağ gecikmesi çok büyük olabilir ve bağlantı sorunlarına neden olabilir. Dosya paylaşım tanığını küme düğümüne yakın çalışan bir Azure sanal makinesinde yapılandırdığından emin olun.  
   >
   >

   Çoğunluk sürücüsünün en az 1.024 MB boş alana ihtiyacı vardır. Çoğunluk sürücüsü için 2.048 MB boş alan öneriyoruz.

2. Küme adı nesnesi ekleyin.

   ![Şekil 20: Küme adı nesnesi için paylaşım üzerindeki izinleri atama][sap-ha-guide-figure-3019]

   _**Şekil 20:** Küme adı nesnesi için paylaşımüzerindeki izinleri atama_

   İzinlerin küme adı nesnesi (örneğimizde pr1-ascs-vir$) için paylaşımdaki verileri değiştirme yetkisini içerdiğinden emin olun.

3. Küme adı nesnesini listeye eklemek için **Ekle'yi**seçin. Şekil 22'de gösterilenlere ek olarak, bilgisayar nesnelerini denetlemek için filtreyi değiştirin.

   ![Şekil 21: Nesne Türlerini bilgisayarları içerecek şekilde değiştirme][sap-ha-guide-figure-3020]

   _**Şekil 21:** **Nesne Türlerini** bilgisayarları içerecek şekilde değiştirme_

   ![Şekil 22: Bilgisayarlar onay kutusunu seçin][sap-ha-guide-figure-3021]

   _**Şekil 22:** **Bilgisayarlar** onay kutusunu seçin_

4. Şekil 21'de gösterildiği gibi küme adı nesnesini girin. Kayıt zaten oluşturulduğundan, Şekil 20'de gösterildiği gibi izinleri değiştirebilirsiniz.

5. Paylaşımın **Güvenlik** sekmesini seçin ve ardından küme adı nesnesi için daha ayrıntılı izinler ayarlayın.

   ![Şekil 23: Dosya paylaşımı çoğunluğundaki küme adı nesnesinin güvenlik özniteliklerini ayarlama][sap-ha-guide-figure-3022]

   _**Şekil 23:** Dosya paylaşımı çoğunluğundaki küme adı nesnesinin güvenlik özniteliklerini ayarlama_

#### <a name="set-the-file-share-witness-quorum-in-failover-cluster-manager"></a><a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a>Failover Cluster Manager'da dosya paylaşımı tanık çoğunluklarını ayarlama

1. Yapılaşı Çoğunluk Ayar Sihirbazı'nı açın.

   ![Şekil 24: Yapılandırma Küme Çoğunluk Ayar Sihirbazı'nı Başlat][sap-ha-guide-figure-3023]

   _**Şekil 24:** Yapılandırma Küme Çoğunluk Ayar Sihirbazı'nı Başlat_

2. Kota **Yapılandırma Seçeneğini Seçin** sayfasında, **çoğunluk tanığını seçin' i**seçin.

   ![Şekil 25: Quorum yapılandırmaları arasından seçim yapabilirsiniz][sap-ha-guide-figure-3024]

   _**Şekil 25:** Aralarından seçim yapabileceğiniz Quorum yapılandırmaları_

3. Çoğunluk **Tanığı** seç sayfasında, **dosya paylaşımı tanığını yapılandır'ı**seçin.

   ![Şekil 26: Dosya paylaşım tanığını seçin][sap-ha-guide-figure-3025]

   _**Şekil 26:** Dosya paylaşımı tanığını seçin_

4. Dosya paylaşımına UNC yolunu girin (örneğimizde, \\domcontr-0\FSW). Yapabileceğiniz değişikliklerin listesini görmek için **İleri'yi**seçin.

   ![Şekil 27: Tanık paylaşımı için dosya paylaşım konumunu tanımlama][sap-ha-guide-figure-3026]

   _**Şekil 27:** Tanık paylaşımı için dosya paylaşım konumunu tanımlama_

5. İstediğiniz değişiklikleri seçin ve sonra **İleri'yi**seçin. Şekil 28'de gösterildiği gibi küme yapılandırmasını başarıyla yeniden yapılandırmanız gerekir:  

   ![Şekil 28: Kümeyi yeniden yapılandırdığınızın doğrulanması][sap-ha-guide-figure-3027]

   _**Şekil 28:** Kümeyi yeniden yapılandırdığınıza dair onay_

Windows failover kümesini başarıyla yükledikten sonra, başarısız algılamayı Azure'daki koşullara uyarlamaları için bazı eşikleri değiştirmeniz gerekir. Değiştirilecek parametreler küme ağ [eşikleri üzerinden Tuning failover][tuning-failover-cluster-network-thresholds]belgelenir. ASCS/SCS için Windows küme yapılandırmasını oluşturan iki VM'nizin aynı alt ağda olduğunu varsayarsak, aşağıdaki parametreleri aşağıdaki değerlerle değiştirin:

- SameSubNetDelay = 2000
- SameSubnetThreshold = 15
- YönlendirmeTarih Boyu = 30

Bu ayarlar müşterilerle test edildi ve iyi bir uzlaşma sunuyor. Onlar yeterince esnek, ama onlar da bir SAP yazılımı veya düğüm veya VM hatası gerçek hata koşullarında yeterince hızlı bir başarısızlık sağlar.

### <a name="install-sios-datakeeper-cluster-edition-for-the-sap-ascsscs-cluster-share-disk"></a><a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a>SAP ASCS/SCS küme paylaşım diski için SIOS DataKeeper Cluster Edition'ı yükleyin

Artık Azure'da çalışan bir Windows Server üzerinde kümeleme yapılandırmanız var. SAP ASCS/SCS örneğini yüklemek için paylaşılan bir disk kaynağına ihtiyacınız vardır. Azure'da ihtiyacınız olan paylaşılan disk kaynaklarını oluşturamazsınız. SIOS DataKeeper Cluster Edition, paylaşılan disk kaynakları oluşturmak için kullanabileceğiniz üçüncü taraf bir çözümdür.

SAP ASCS/SCS küme paylaşım diski için SIOS DataKeeper Cluster Edition'ın yüklenmesi şu görevleri içerir:

- Microsoft .NET Framework 3.5 ekleyin.
- SIOS DataKeeper'ı yükleyin.
- SIOS DataKeeper'ı ayarlayın.

### <a name="add-net-framework-35"></a><a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a>.NET Framework 3.5 Ekle
.NET Framework 3.5, Windows Server 2012 R2'de otomatik olarak etkinleştirilmez veya yüklenmez. SIOS DataKeeper, DataKeeper'ı yüklediğiniz tüm düğümlerde .NET olmasını gerektirdiğinden, kümedeki tüm sanal makinelerin konuk işletim sistemine .NET Framework 3.5 yüklemeniz gerekir.

.NET Framework 3.5 eklemenin iki yolu vardır:

- Şekil 29'da gösterildiği gibi Windows'da Rol ve Özellikler Ekle Sihirbazı'nı kullanın:

  ![Şekil 29: Rol ve Özellikler Ekle Sihirbazı'nı kullanarak .NET Framework 3.5'i yükleyin][sap-ha-guide-figure-3028]

  _**Şekil 29:** Roller ve Özellikler Ekle Sihirbazı'nı kullanarak .NET Framework 3.5'i yükleyin_

  ![Şekil 30: Roller ve Özellikler Ekle Sihirbazı'nı kullanarak .NET Framework 3.5'i yüklediğinizde yükleme ilerleme çubuğu][sap-ha-guide-figure-3029]

  _**Şekil 30:** Roller ve Özellikler Ekle Sihirbazı'nı kullanarak .NET Framework 3.5'i yüklediğinizde yükleme ilerleme çubuğu_

- dism.exe komut satırı aracını kullanın. Bu tür bir yükleme için, Windows yükleme medyasındaki SxS dizinine erişmeniz gerekir. Yükseltilmiş bir komut isteminde şu komutu girin:

  ```
  Dism /online /enable-feature /featurename:NetFx3 /All /Source:installation_media_drive:\sources\sxs /LimitAccess
  ```

### <a name="install-sios-datakeeper"></a><a name="dd41d5a2-8083-415b-9878-839652812102"></a>SIOS DataKeeper'ı yükleyin

Kümedeki her düğüme SIOS DataKeeper Cluster Edition'ı yükleyin. SIOS DataKeeper ile sanal paylaşılan depolama oluşturmak için senkronize bir ayna oluşturun ve ardından küme paylaşılan depolamayı simüle edin.

SIOS yazılımını yüklemeden önce DataKeeperSvc etki alanı kullanıcısını oluşturun.

> [!NOTE]
> Her iki küme düğümünde de Yerel Yönetici grubuna DataKeeperSvc etki alanı kullanıcısını ekleyin.
>
>

SIOS DataKeeper'ı yüklemek için:

1. SIOS yazılımını her iki küme düğümüne de yükleyin.

   ![SIOS yükleyici][sap-ha-guide-figure-3030]

   ![Şekil 31: SIOS DataKeeper kurulumunun ilk sayfası][sap-ha-guide-figure-3031]

   _**Şekil 31:** SIOS DataKeeper kurulumunun ilk sayfası_

2. İletişim kutusunda **Evet'i**seçin.

   ![Şekil 32: DataKeeper bir hizmetin devre dışı bırakıldığını bildirir][sap-ha-guide-figure-3032]

   _**Şekil 32:** DataKeeper, bir hizmetin devre dışı bırakıldığını bildirir_

3. İletişim kutusunda, Etki Alanı veya **Sunucu hesabını**seçmenizi öneririz.

   ![Şekil 33: SIOS DataKeeper için kullanıcı seçimi][sap-ha-guide-figure-3033]

   _**Şekil 33:** SIOS DataKeeper için kullanıcı seçimi_

4. SIOS DataKeeper için oluşturduğunuz etki alanı hesabı kullanıcı adını ve parolasını girin.

   ![Şekil 34: SIOS DataKeeper kurulumu için alan adı kullanıcı adı ve parolasını girin][sap-ha-guide-figure-3034]

   _**Şekil 34:** SIOS DataKeeper kurulumu için alan adı kullanıcı adı ve parolasını girin_

5. Şekil 35'te gösterildiği gibi SIOS DataKeeper örneğinizin lisans anahtarını yükleyin.

   ![Şekil 35: SIOS DataKeeper lisans anahtarınızı girin][sap-ha-guide-figure-3035]

   _**Şekil 35:** SIOS DataKeeper lisans anahtarınızı girin_

6. İstendiğinde, sanal makineyi yeniden başlatın.

### <a name="set-up-sios-datakeeper"></a><a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a>SIOS DataKeeper'ı ayarlama

SIOS DataKeeper'ı her iki düğüme de yükledikten sonra yapılandırmayı başlatın. Yapılandırmanın amacı, sanal makinelerin her birine bağlı ek diskler arasında eşzamanlı veri çoğaltma olmasıdır.

1. DataKeeper Yönetimi ve Yapılandırma aracını başlatın ve ardından **Sunucuyu Bağla'yı**seçin.

   ![Şekil 36: SIOS DataKeeper Yönetimi ve Yapılandırma aracı][sap-ha-guide-figure-3036]

   _**Şekil 36:** SIOS DataKeeper Yönetimi ve Yapılandırma aracı_

2. Yönetim ve Yapılandırma aracının bağlanması gereken ilk düğümün adını veya TCP/IP adresini ve ikinci adımda ikinci düğümü girin.

   ![Şekil 37: Yönetim ve Yapılandırma aracının bağlanması gereken ilk düğümün adını veya TCP/IP adresini ekleyin ve ikinci adımda ikinci düğüm][sap-ha-guide-figure-3037]

   _**Şekil 37:** Yönetim ve Yapılandırma aracının bağlanması gereken ilk düğümün adını veya TCP/IP adresini ekleyin ve ikinci adımda ikinci düğüm_

3. İki düğüm arasındaki çoğaltma işini oluşturun.

   ![Şekil 38: Bir çoğaltma işi oluşturma][sap-ha-guide-figure-3038]

   _**Şekil 38:** Çoğaltma işi oluşturma_

   Sihirbaz, çoğaltma işi oluşturma sürecinde size yol gösteriyor.

4. Çoğaltma işinin adını tanımlayın.

   ![Şekil 39: Çoğaltma işinin adını tanımlayın][sap-ha-guide-figure-3039]

   _**Şekil 39:** Çoğaltma işinin adını tanımlama_

   ![Şekil 40: Geçerli kaynak düğüm olmalıdır düğüm için temel verileri tanımlayın][sap-ha-guide-figure-3040]

   _**Şekil 40:** Geçerli kaynak düğümü olması gereken düğüm için temel verileri tanımlayın_

5. Hedef düğümün adını, TCP/IP adresini ve disk hacmini tanımlayın.

   ![Şekil 41: Geçerli hedef düğümün adını, TCP/IP adresini ve disk hacmini tanımlayın][sap-ha-guide-figure-3041]

   _**Şekil 41:** Geçerli hedef düğümün adını, TCP/IP adresini ve disk hacmini tanımlayın_

6. Sıkıştırma algoritmalarını tanımlayın. Örneğimizde, çoğaltma akışını sıkıştırmanızı öneririz. Özellikle yeniden eşitleme durumlarında, çoğaltma akışının sıkıştırılması yeniden eşitleme süresini önemli ölçüde azaltır. Sıkıştırma, sanal bir makinenin CPU ve RAM kaynaklarını kullanır. Sıkıştırma hızı arttıkça, kullanılan CPU kaynaklarının hacmi de artar. Bu ayarı daha sonra ayarlayabilirsiniz.

7. Denetlemeniz gereken diğer bir ayar da çoğaltmanın eşzamanlı mı yoksa eşzamanlı mı oluştuğudur. SAP ASCS/SCS yapılandırmalarını koruduğunuzda, senkron çoğaltma kullanmanız gerekir.  

   ![Şekil 42: Çoğaltma ayrıntılarını tanımlama][sap-ha-guide-figure-3042]

   _**Şekil 42:** Çoğaltma ayrıntılarını tanımlama_

8. Çoğaltma işi tarafından çoğaltılan birimin paylaşılan disk olarak Windows Server failover küme yapılandırmasına yansıtılıp temsil edilmemesi gerektiğini tanımlayın. SAP ASCS/SCS yapılandırması için, Windows kümesinin çoğaltılan birimi küme birimi olarak kullanabileceği paylaşılan bir disk olarak görmesi için **Evet'i** seçin.

   ![Şekil 43: Yinelenen birimi küme hacmi olarak ayarlamak için Evet'i seçin][sap-ha-guide-figure-3043]

   _**Şekil 43:** Yinelenen birimi küme birimi olarak ayarlamak için **Evet'i** seçin_

   Birim oluşturulduktan sonra, DataKeeper Yönetimi ve Yapılandırma aracı çoğaltma işinin etkin olduğunu gösterir.

   ![Şekil 44: SAP ASCS/SCS paylaşım diski için DataKeeper senkron yansıtma etkin][sap-ha-guide-figure-3044]

   _**Şekil 44:** SAP ASCS/SCS paylaşım diski için DataKeeper senkron yansıtma etkin_

   Failover Cluster Manager şimdi Şekil 45'te gösterildiği gibi diski DataKeeper diski olarak gösterir:

   ![Şekil 45: Failover Cluster Manager, DataKeeper'ın çoğalttettiği diski gösterir][sap-ha-guide-figure-3045]

   _**Şekil 45:** Failover Cluster Manager, DataKeeper'ın çoğalttettiği diski gösterir_

## <a name="next-steps"></a>Sonraki adımlar

* [SAP ASCS/SCS örneği için Windows failover kümesi ve paylaşılan disk kullanarak SAP NetWeaver HA'yı yükleyin][sap-high-availability-installation-wsfc-shared-disk]
