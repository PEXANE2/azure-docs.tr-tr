---
title: SAP NetWeaver için Azure VM HA mimarisi ve senaryoları | Microsoft Dokümanlar
description: Azure Sanal Makinelerde SAP NetWeaver için yüksek kullanılabilirlik mimarisi ve senaryoları
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: rdeltcheva
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 887caaec-02ba-4711-bd4d-204a7d16b32b
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 02/26/2020
ms.author: radeltch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 08f770ced6cb1ec1102159788e1583d481436b08
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79279916"
---
# <a name="high-availability-architecture-and-scenarios-for-sap-netweaver"></a>SAP NetWeaver için yüksek kullanılabilirlik mimarisi ve senaryoları

[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2243692]:https://launchpad.support.sap.com/#/notes/2243692

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png


[sap-installation-guides]:http://service.sap.com/instguides

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
[sap-ascs-ha-multi-sid-wsfc-file-share]:sap-ascs-ha-multi-sid-wsfc-file-share.md
[sap-ascs-ha-multi-sid-wsfc-shared-disk]:sap-ascs-ha-multi-sid-wsfc-shared-disk.md
[sap-hana-ha]:sap-hana-high-availability.md
[sap-suse-ascs-ha]:high-availability-guide-suse.md
[sap-suse-ascs-ha-anf]:high-availability-guide-suse-netapp-files.md
[sap-higher-availability]:sap-higher-availability-architecture-scenarios.md

[planning-guide]:planning-guide.md  
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff
[planning-guide-11]:planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058
[planning-guide-11.4.1]:planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77
[planning-guide-11.5]:planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10
[planning-guide-3.1]:planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a
[planning-guide-3.2.1]:planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358
[planning-guide-3.2.2]:planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560
[planning-guide-3.2.3]:planning-guide.md#18810088-f9be-4c97-958a-27996255c665
[planning-guide-3.2]:planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77
[planning-guide-3.3.2]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92
[planning-guide-5.1.1]:planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53
[planning-guide-5.1.2]:planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c
[planning-guide-5.2.1]:planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef
[planning-guide-5.2.2]:planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3
[planning-guide-5.2]:planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7
[planning-guide-5.3.1]:planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79
[planning-guide-5.3.2]:planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a
[planning-guide-5.4.2]:planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1
[planning-guide-5.5.1]:planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646
[planning-guide-5.5.3]:planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d
[planning-guide-7.1]:planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30
[planning-guide-7]:planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14
[planning-guide-9.1]:planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3
[planning-guide-azure-premium-storage]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92

[virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]:../../windows/sql/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md
[virtual-machines-windows-portal-sql-alwayson-int-listener]:../../windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md

[sap-ha-bc-virtual-env-hyperv-vmware-white-paper]:https://scn.sap.com/docs/DOC-44415
[sap-ha-partner-information]:https://scn.sap.com/docs/DOC-8541
[azure-sla]:https://azure.microsoft.com/support/legal/sla/
[azure-virtual-machines-manage-availability]:https://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability
[azure-storage-redundancy]:https://azure.microsoft.com/documentation/articles/storage-redundancy/
[azure-storage-managed-disks-overview]:https://docs.microsoft.com/azure/storage/storage-managed-disks-overview

[planning-guide-figure-100]:media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:media/virtual-machines-shared-sap-planning-guide/planning-monitoring-overview-2502.png
[planning-guide-figure-2600]:media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-2901]:media/virtual-machines-shared-sap-planning-guide/2901-azure-ha-sap-ha-md.png
[planning-guide-figure-300]:media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-3201]:media/virtual-machines-shared-sap-planning-guide/3201-sap-ha-with-sql-md.png
[planning-guide-figure-400]:media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[sap-ha-guide]:sap-high-availability-guide.md
[sap-ha-guide-2]:#42b8f600-7ba3-4606-b8a5-53c4f026da08
[sap-ha-guide-4]:#8ecf3ba0-67c0-4495-9c14-feec1a2255b7
[sap-ha-guide-8]:#78092dbe-165b-454c-92f5-4972bdbef9bf
[sap-ha-guide-8.1]:#c87a8d3f-b1dc-4d2f-b23c-da4b72977489
[sap-ha-guide-8.9]:#fe0bd8b5-2b43-45e3-8295-80bee5415716
[sap-ha-guide-8.11]:#661035b2-4d0f-4d31-86f8-dc0a50d78158
[sap-ha-guide-8.12]:#0d67f090-7928-43e0-8772-5ccbf8f59aab
[sap-ha-guide-8.12.1]:#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79
[sap-ha-guide-8.12.3]:#5c8e5482-841e-45e1-a89d-a05c0907c868
[sap-ha-guide-8.12.3.1]:#1c2788c3-3648-4e82-9e0d-e058e475e2a3
[sap-ha-guide-8.12.3.2]:#dd41d5a2-8083-415b-9878-839652812102
[sap-ha-guide-8.12.3.3]:#d9c1fc8e-8710-4dff-bec2-1f535db7b006
[sap-ha-guide-9]:#a06f0b49-8a7a-42bf-8b0d-c12026c5746b
[sap-ha-guide-9.1]:#31c6bd4f-51df-4057-9fdf-3fcbc619c170
[sap-ha-guide-9.1.1]:#a97ad604-9094-44fe-a364-f89cb39bf097

[sap-ha-multi-sid-guide]:sap-high-availability-multi-sid.md (SAP multi-SID yüksek kullanılabilirlik yapılandırması)


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


## <a name="terminology-definitions"></a>Terminoloji tanımları

**Yüksek kullanılabilirlik**: *Aynı* veri merkezi içinde gereksiz, hataya dayanıklı veya başarısız korumalı bileşenler aracılığıyla BT hizmetlerinin iş sürekliliğini sağlayarak BT kesintilerini en aza indiren bir dizi teknolojiyi ifade eder. Bizim durumumuzda, veri merkezi tek bir Azure bölgesinde bulunur.

**Olağanüstü durum kurtarma**: Bt hizmetlerinin aksamasının ve geri kazanımlarının en aza indirilmesi anlamına gelir, ancak birbirinden yüzlerce mil uzakta olabilecek *çeşitli* veri merkezlerinde. Bizim durumumuzda, veri merkezleri aynı jeopolitik bölge içinde çeşitli Azure bölgelerinde veya müşteri olarak sizin tarafınızda kurulan konumlarda olabilir.


## <a name="overview-of-high-availability"></a>Yüksek kullanılabilirlik genel bakış
Azure'daki SAP yüksek kullanılabilirliği üç türe ayrılabilir:

* **Azure altyapısı yüksek kullanılabilirlik:** 

    Örneğin, yüksek kullanılabilirlik, bilgi işlem (VM), ağ veya depolama ve SAP uygulamalarının kullanılabilirliğini artırmak için sağladığı yararları içerebilir.

* **SAP uygulamalarının *daha yüksek kullanılabilirliğini* elde etmek için Azure altyapısı VM yeniden başlatma kullanma:** 

    Linux'ta Windows Server Failover Clustering (WSFC) veya Pacemaker gibi işlevleri kullanmamaya karar verirseniz, Azure VM yeniden başlatma sı kullanılır. SAP sistemlerini Azure fiziksel sunucu altyapısının planlı ve planlanmamış kapalı kalma sürelerine ve genel olarak temel altında yatan Azure platformuna karşı korur.

* **SAP uygulaması yüksek kullanılabilirlik**: 

    Tam SAP sistemi yüksek kullanılabilirlik elde etmek için, tüm kritik SAP sistem bileşenlerini korumanız gerekir. Örnek:
    * Gereksiz SAP uygulama sunucuları.
    * Benzersiz bileşenler. Sap ASCS/SCS örneği veya veritabanı yönetim sistemi (DBMS) gibi tek bir hata noktası (SPOF) bileşeni örnek olabilir.

Azure'daki SAP yüksek kullanılabilirliği, şirket içi fiziksel veya sanal ortamda SAP yüksek kullanılabilirliğinden farklıdır. [Microsoft Windows'daki VMware ve Hyper-V ile sanal ortamlarda sap NetWeaver yüksek kullanılabilirlik ve iş sürekliliği][sap-ha-bc-virtual-env-hyperv-vmware-white-paper] aşağıdaki kağıt Windows'da sanallaştırılmış ortamlarda standart SAP yüksek kullanılabilirlik yapılandırmaları açıklar.

Windows için olduğu gibi Linux için sapinst-integrated SAP yüksek kullanılabilirlik yapılandırması yoktur. Linux için SAP yüksek kullanılabilirliği hakkında bilgi için yüksek [kullanılabilirlik iş ortağı bilgilerine][sap-ha-partner-information]bakın.

## <a name="azure-infrastructure-high-availability"></a>Azure altyapısı yüksek kullanılabilirlik

### <a name="sla-for-single-instance-virtual-machines"></a>Tek örnekli sanal makineler için SLA

Şu anda premium depolama ile% 99,9 tek VM SLA vardır. Tek bir VM'nin kullanılabilirliği hakkında bir fikir edinmek için, çeşitli kullanılabilir [Azure Hizmet Düzeyi Anlaşmalarının][azure-sla]ürünlerini oluşturabilirsiniz.

Hesaplamanın temeli ayda 30 gün veya 43.200 dakikadır. Örneğin, %0,05'lik bir kapalı kalma süresi 21,6 dakikaya karşılık gelir. Her zamanki gibi, çeşitli hizmetlerin kullanılabilirliği aşağıdaki şekilde hesaplanır:

(Kullanılabilirlik Hizmeti #1/100) * (Kullanılabilirlik Hizmeti #2/100) * \*(Kullanılabilirlik Hizmeti #3/100) ...

Örnek:

(99.95/100) * (99.9/100) * (99.9/100) = 0.9975 veya genel durumu %99.75.

### <a name="multiple-instances-of-virtual-machines-in-the-same-availability-set"></a>Aynı kullanılabilirlik kümesinde birden çok sanal makine örneği
Aynı *kullanılabilirlik kümesinde*dağıtılan iki veya daha fazla örneği olan tüm sanal makineler için, en az bir örneğine en az %99,95 oranında sanal makine bağlantısına sahip olacağınızı garanti ediyoruz.

İki veya daha fazla Sanal Makine aynı kullanılabilirlik kümesinin parçası olduğunda, kullanılabilirlik kümesindeki her sanal makineye, temel Azure platformu tarafından bir *güncelleştirme etki alanı* ve bir hata etki *alanı* atanır.

* **Alan adlarını güncelleştirme,** azure altyapısının planlı bakımı sırasında birden çok VM'nin aynı anda yeniden başlatılmadığını garanti eder. Aynı anda yalnızca bir VM yeniden başlatılır.

* **Hata etki alanları,** VM'lerin ortak bir güç kaynağını ve ağ anahtarını paylaşmayan donanım bileşenlerine dağıtılmış olduğunu garanti eder. Sunucular, bir ağ anahtarı veya bir güç kaynağı planlanmamış bir kapalı kalma süresinden geçtiğinde, yalnızca bir VM etkilenir.

Daha fazla bilgi için bkz: [Azure'daki Windows sanal makinelerinin kullanılabilirliğini yönetin.][azure-virtual-machines-manage-availability]

Yüksek kullanılabilirlik elde etmek için kullanılabilirlik kümesi kullanılır:

* Gereksiz SAP uygulama sunucuları.  
* SAP ASCS/SCS örneği veya DBMS gibi SPOF'ları koruyan iki veya daha fazla düğüme (örneğin VM) sahip kümeler.


### <a name="azure-availability-zones"></a>Azure Kullanılabilirlik Alanları
Azure, farklı [Azure Bölgeleri'nde](https://azure.microsoft.com/global-infrastructure/regions/) [Azure Kullanılabilirlik Bölgeleri](https://docs.microsoft.com/azure/availability-zones/az-overview) kavramlarını kullanıma alma sürecindedir. Kullanılabilirlik Bölgelerinin sunulduğu Azure bölgelerinde, Azure bölgeleri güç kaynağı, soğutma ve ağ tedarikinde bağımsız olan birden çok veri merkezine sahiptir. Tek bir Azure bölgesinde farklı bölgeler sunmanın nedeni, uygulamaları sunulan iki veya üç Kullanılabilirlik Bölgesi'ne dağıtmanızı sağlamaktır. Güç kaynakları ve/veya ağındaki sorunların yalnızca bir Kullanılabilirlik Bölgesi altyapılarını etkileyeceğini varsayarsak, bir Azure bölgesindeki uygulama dağıtımınız hala tam olarak işlevseldir. Bir bölgede bazı VM'ler kaybolabileceğinden, sonunda kapasite de azalır. Ama diğer iki bölgede VM hala çalışır durumda. Teklif bölgeleri sunan Azure bölgeleri [Azure Kullanılabilirlik Bölgeleri'nde](https://docs.microsoft.com/azure/availability-zones/az-overview)listelenir.

Kullanılabilirlik Bölgeleri'ni kullanarak göz önünde bulundurulması gereken bazı şeyler vardır. Hususlar gibi listesi:

- Kullanılabilirlik Bölgesinde Azure Kullanılabilirlik Kümelerini dağıtamazsınız. VM için dağıtım çerçevesi olarak Kullanılabilirlik Bölgesi veya Kullanılabilirlik Kümesi'ni seçmeniz gerekir.
- Windows Failover Cluster Services veya Linux Pacemaker'ı temel alan başarısız küme çözümleri oluşturmak için [Temel Yük Dengeleyicisini](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) kullanamazsınız. Bunun yerine Azure [Standart Yük Dengeleyicisi SKU'yu](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones) kullanmanız gerekir
- Azure Kullanılabilirlik Bölgeleri, bir bölgedeki farklı bölgeler arasında belirli bir mesafe garantisi vermiyor
- Farklı Azure bölgelerindeki farklı Azure Kullanılabilirlik Bölgeleri arasındaki ağ gecikmesi, Azure bölgesinden bölgeye farklı olabilir. Bir bölgeden etkin DBMS VM'ye ağ gecikmesi iş süreci etkisinden hala kabul edilebilir olduğundan, müşteri olarak farklı bölgeler arasında dağıtılan SAP uygulama katmanını makul bir şekilde çalıştırabileceğiniz durumlar olacaktır. Oysa bir bölgede etkin DBMS VM ile başka bir bölgede bir VM'deki SAP uygulama örneği arasındaki gecikmenin SAP iş süreçleri için çok müdahaleci ve kabul edilemez olabileceği müşteri senaryoları olacaktır. Sonuç olarak, gecikme süresi çok yüksekse uygulama için etkin/etkin mimari veya etkin/pasif mimari ile dağıtım mimarilerinin farklı olması gerekir.
- [Azure yönetilen disklerin](https://azure.microsoft.com/services/managed-disks/) kullanılması, Azure Kullanılabilirlik Bölgelerine dağıtmak için zorunludur 


### <a name="planned-and-unplanned-maintenance-of-virtual-machines"></a>Sanal makinelerin planlı ve plansız bakımı

İki tür Azure platform olayı sanal makinelerinizin kullanılabilirliğini etkileyebilir:

* **Planlanan bakım** olayları, Microsoft tarafından temel Azure platformuna yapılan periyodik güncelleştirmelerdir. Güncelleştirmeler, sanal makinelerinizin çalıştırdığı platform altyapısının genel güvenilirliğini, performansını ve güvenliğini artırır.

* Sanal makinenizin altında yatan donanım veya fiziksel altyapı bir şekilde başarısız olduğunda **planlanmamış bakım** olayları oluşur. Yerel ağ hataları, yerel disk hataları veya diğer raf düzeyi hataları içerebilir. Böyle bir hata algılandığında, Azure platformu sanal makinenizi otomatik olarak sanal makinenizi barındıran sağlıksız fiziksel sunucudan sağlıklı bir fiziksel sunucuya aktarır. Bu tür olaylar nadirdir, ancak sanal makinenizin yeniden başlatılmasına da neden olabilir.

Daha fazla bilgi için bkz: [Azure'daki Windows sanal makinelerinin kullanılabilirliğini yönetin.][azure-virtual-machines-manage-availability]

### <a name="azure-storage-redundancy"></a>Azure Depolama artıklığı
Depolama hesabınızdaki veriler, geçici donanım hataları karşısında bile Azure Depolama SLA'sını karşılayacak dayanıklılık ve yüksek kullanılabilirlik sağlamak için her zaman çoğaltılır.

Azure Depolama varsayılan olarak verilerin üç görüntüsünü sakladığından, RAID 5 veya RAID 1'in birden çok Azure diskinde kullanılması gereksizdir.

Daha fazla bilgi için Azure [Depolama çoğaltma'ya][azure-storage-redundancy]bakın.

### <a name="azure-managed-disks"></a>Azure Yönetilen Diskleri
Yönetilen Diskler, Azure depolama hesaplarında depolanan sanal sabit diskler (VHD'ler) yerine kullanılması önerilen Azure Kaynak Yöneticisi'nde bir kaynak türüdür. Yönetilen diskler, bağlı oldukları sanal makinenin Azure kullanılabilirlik kümesiyle otomatik olarak hizalanır. Sanal makinenizin ve üzerinde çalışan hizmetlerin kullanılabilirliğini artırırlar.

Daha fazla bilgi için Azure [Yönetilen Diskler'e genel bakış][azure-storage-managed-disks-overview]bakın.

Sanal makinelerinizin dağıtımını ve yönetimini basitleştirdikleri için yönetilen diskleri kullanmanızı öneririz.



## <a name="utilizing-azure-infrastructure-high-availability-to-achieve-higher-availability-of-sap-applications"></a>SAP uygulamalarında *daha yüksek kullanılabilirlik* elde etmek için Azure altyapısından yüksek kullanılabilirlik kullanma

WsFC veya Pacemaker gibi işlevleri Linux'ta kullanmamaya karar verirseniz (şu anda yalnızca SUSE Linux Enterprise Server [SLES] 12 ve sonrası için desteklenir), Azure VM yeniden başlatması kullanılır. SAP sistemlerini Azure fiziksel sunucu altyapısının planlı ve planlanmamış kapalı kalma sürelerine ve genel olarak temel altında yatan Azure platformuna karşı korur.

Bu yaklaşım hakkında daha fazla bilgi için, [SAP sisteminin daha yüksek kullanılabilirliğini elde etmek için Azure altyapı VM yeniden başlat'ı kullanma][sap-higher-availability]bölümüne bakın.

## <a name="high-availability-of-sap-applications-on-azure-iaas"></a><a name="baed0eb3-c662-4405-b114-24c10a62954e"></a>Azure IaaS'da SAP uygulamalarının yüksek kullanılabilirliği

Tam SAP sistemi yüksek kullanılabilirlik elde etmek için, tüm kritik SAP sistem bileşenlerini korumanız gerekir. Örnek:
  * Gereksiz SAP uygulama sunucuları.
  * Benzersiz bileşenler. Sap ASCS/SCS örneği veya veritabanı yönetim sistemi (DBMS) gibi tek bir hata noktası (SPOF) bileşeni örnek olabilir.

Sonraki bölümlerde üç kritik SAP sistem bileşeni için de yüksek kullanılabilirlik nasıl elde edilecek tartışılır.

### <a name="high-availability-architecture-for-sap-application-servers"></a>SAP uygulama sunucuları için yüksek kullanılabilirlik mimarisi

> Bu bölüm aşağıdakiler için geçerlidir:
>
> ![Windows][Logo_Windows] Windows ve ![Linux][Logo_Linux] Linux
>

Genellikle SAP uygulama sunucusu ve iletişim örnekleri için belirli bir yüksek kullanılabilirlik çözümüne ihtiyacınız yoktur. Fazlalıkla yüksek kullanılabilirlik elde emiş siniz ve Azure sanal makinelerinin çeşitli örneklerinde birden çok iletişim örneğini yapılandırırsınız. Azure sanal makinelerinin iki örneğinde en az iki SAP uygulama örneği yüklenmeliydi.

![Şekil 1: Yüksek kullanılabilirlik SAP uygulama sunucusu][sap-ha-guide-figure-2000]

_**Şekil 1:** Yüksek kullanılabilirlik SAP uygulama sunucusu_

SAP uygulama sunucusu örneklerini barındıran tüm sanal makineleri aynı Azure kullanılabilirlik kümesine yerleştirmeniz gerekir. Azure kullanılabilirlik kümesi şunları sağlar:

* Tüm sanal makineler aynı güncelleştirme alanının bir parçasıdır.  
    Güncelleştirme etki alanı, sanal makinelerin planlanan bakım kapalı kalma süresi sırasında aynı anda güncellenmemesini sağlar.

    Azure ölçeğindeki bir birimdeki farklı güncelleştirme ve hata etki alanları üzerinde oluşturulan temel işlevsellik, [güncelleştirme etki alanları][planning-guide-3.2.2] bölümünde zaten tanıtıldı.

* Tüm sanal makineler aynı hata etki alanının bir parçasıdır.  
    Hata etki alanı, tek bir hata noktasının tüm sanal makinelerin kullanılabilirliğini etkilememesi için sanal makinelerin dağıtılmasını sağlar.

Azure ölçeği birimi içinde ayarlanan bir Azure kullanılabilirliği kümesi tarafından kullanılabilecek güncelleştirme ve hata etki alanı sayısı sınırlıdır. Tek bir kullanılabilirlik kümesine VM eklemeye devam ederseniz, iki veya daha fazla VM sonunda aynı hata veya güncelleştirme etki alanı sona erer.

Beş güncelleştirme etki alanımız olduğunu varsayarak, özel VM'lerine birkaç SAP uygulama sunucusu örneği dağıtırsanız, aşağıdaki resim ortaya çıkar. Kullanılabilirlik kümesindeki gerçek maksimum güncelleştirme ve hata etki alanı sayısı gelecekte değişebilir:

![Şekil 2: Bir Azure kullanılabilirlik kümesindeki][planning-guide-figure-3000]
SAP uygulama sunucularının yüksek kullanılabilirliği_**Şekil 2:** Bir Azure kullanılabilirlik kümesinde SAP uygulama sunucularının yüksek kullanılabilirliği_

Daha fazla bilgi için bkz: [Azure'daki Windows sanal makinelerinin kullanılabilirliğini yönetin.][azure-virtual-machines-manage-availability]

Daha fazla bilgi için, SAP NetWeaver belgesi için Azure sanal makinelerin planlanması ve uygulanmasının [Azure kullanılabilirlik kümeleri][planning-guide-3.2.3] bölümüne bakın.

**Yalnızca yönetilmeyen diskler:** Azure depolama hesabı olası tek bir hata noktası olduğundan, en az iki sanal makinenin dağıtıldığı en az iki Azure depolama hesabına sahip olmak önemlidir. İdeal bir kurulumda, SAP iletişim örneği çalıştıran her sanal makinenin diskleri farklı bir depolama hesabında dağıtılır.

> [!IMPORTANT]
> SAP yüksek kullanılabilirlik yüklemeleriniz için Azure yönetilen diskleri kullanmanızı şiddetle öneririz. Yönetilen diskler bağlı oldukları sanal makinenin kullanılabilirlik kümesiyle otomatik olarak hizalandığından, sanal makinenizin kullanılabilirliğini ve üzerinde çalışan hizmetleri artırır.  
>

### <a name="high-availability-architecture-for-an-sap-ascsscs-instance-on-windows"></a>Windows'daki BIR SAP ASCS/SCS örneği için yüksek kullanılabilirlik mimarisi

> ![Windows][Logo_Windows] Windows
>

SAP ASCS/SCS örneğini korumak için bir WSFC çözümü kullanabilirsiniz. Çözümün iki çeşidi vardır:

* **Kümelenmiş paylaşılan diskler kullanarak SAP ASCS/SCS örneğini kümeleyin**: Bu mimari hakkında daha fazla bilgi için, [paylaşılan bir küme kullanarak Windows failover kümesinde SAP ASCS/SCS örneğini kümeleme][sap-high-availability-guide-wsfc-shared-disk]bölümüne bakın.   

* **Dosya paylaşımını kullanarak SAP ASCS/SCS örneğini kümeleyin**: Bu mimari hakkında daha fazla bilgi için [bkz.][sap-high-availability-guide-wsfc-file-share]

* **ANF SMB paylaşımını kullanarak SAP ASCS/SCS örneğini kümeleyin**: Bu mimari hakkında daha fazla bilgi için, [ANF SMB dosya paylaşımını kullanarak Windows failover kümesinde Bir SAP ASCS/SCS örneğine](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-windows-netapp-files-smb)bakın.

### <a name="high-availability-architecture-for-an-sap-ascsscs-instance-on-linux"></a>Linux'taki BIR SAP ASCS/SCS örneği için yüksek kullanılabilirlik mimarisi

> ![Linux][Logo_Linux] Linux
> 
> SLES küme çerçevesini kullanarak SAP ASCS/SCS örneğini kümeleme hakkında daha fazla bilgi için, [SAP uygulamaları için SUSE Linux Enterprise Server'daki Azure VM'lerde SAP NetWeaver için yüksek kullanılabilirlik][sap-suse-ascs-ha]bilgisine bakın. Yüksek kullanılabilirLik gerektirmeyen SLES'teki alternatif HA mimarisi [için, SAP uygulamaları için Azure NetApp Dosyaları ile SUSE Linux Enterprise Server'da SAP NetWeaver için yüksek kullanılabilirlik kılavuzuna][sap-suse-ascs-ha-anf]bakın.

Red Hat küme çerçevesini kullanarak SAP ASCS/SCS örneğini kümeleme hakkında daha fazla bilgi için [Red Hat Enterprise Linux'ta SAP NetWeaver için Azure Sanal Makineler yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel)


### <a name="sap-netweaver-multi-sid-configuration-for-a-clustered-sap-ascsscs-instance"></a>Kümelenmiş SAP ASCS/SCS örneği için SAP NetWeaver multi-SID yapılandırması

> ![Windows][Logo_Windows] Windows
> 
> Multi-SID, dosya paylaşımı ve paylaşılan disk kullanılarak WSFC ile desteklenir.
> 
> Windows'da çoklu SID yüksek kullanılabilirlik mimarisi hakkında daha fazla bilgi için bkz:

* [SAP ASCS/SCS örneği windows server failover kümeleme ve dosya paylaşımı için çok SID yüksek kullanılabilirlik][sap-ascs-ha-multi-sid-wsfc-file-share]

* [Sap ASCS/SCS örneği windows server failover kümeleme ve paylaşılan disk için çoklu SID yüksek kullanılabilirlik][sap-ascs-ha-multi-sid-wsfc-shared-disk]

> ![Linux][Logo_Linux] Linux
> 
> Multi-SID kümeleme, SAP ASCS/ERS için Linux Pacemaker kümelerinde, aynı kümedeki **beş** SAP SID ile sınırlı olarak desteklenir.
> Linux'ta çoklu SID yüksek kullanılabilirlik mimarisi hakkında daha fazla bilgi için bkz:

* [SAP uygulamaları multi-SID kılavuzu için SLES'te Azure VM'lerde SAP NW için HA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-multi-sid)
* [SAP uygulamaları multi-SID kılavuzu için RHEL'de Azure VM'lerde SAP NW için HA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-multi-sid)

### <a name="high-availability-dbms-instance"></a>Yüksek kullanılabilirlik dbms örneği

DBMS aynı zamanda sap sisteminde tek bir temas noktasıdır. Yüksek kullanılabilirlik çözüm kullanarak onu korumak gerekir. Aşağıdaki şekilde, Windows Server Failover Clustering ve Azure dahili yük dengeleyicisi ile Azure'da yüksek kullanılabilirlik çözümünü sql server alwayson gösterir. SQL Server AlwaysOn, kendi DBMS çoğaltmasını kullanarak DBMS verilerini ve günlük dosyalarını çoğaltır. Bu durumda, tüm kurulumu basitleştiren küme paylaşılan diske ihtiyacınız yoktur.

![Şekil 3: SQL Server AlwaysOn ile yüksek kullanılabilirlik sap DBMS örneği][sap-ha-guide-figure-2003]

_**Şekil 3:** SQL Server AlwaysOn ile yüksek kullanılabilirlik sap DBMS örneği_

Azure Kaynak Yöneticisi dağıtım modelini kullanarak Azure'da SQL Server DBMS kümeleme hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Kaynak Yöneticisi'ni kullanarak Azure sanal makinelerdeki Bir AlwaysOn kullanılabilirlik grubunu el ile yapılandırın][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]

* [Azure'daki AlwaysOn kullanılabilirlik grubu için Azure dahili yük dengeleyicisi yapılandırma][virtual-machines-windows-portal-sql-alwayson-int-listener]

Azure Kaynak Yöneticisi dağıtım modelini kullanarak AZURE'da SAP HANA DBMS kümeleme hakkında daha fazla bilgi için, [Azure sanal makinelerde (VM) SAP HANA'nın yüksek kullanılabilirliği][sap-hana-ha]bölümüne bakın.
