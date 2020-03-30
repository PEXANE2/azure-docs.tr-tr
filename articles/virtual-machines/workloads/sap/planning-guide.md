---
title: "Azure'da SAP: Planlama ve Uygulama Kılavuzu"
description: SAP NetWeaver için Azure Sanal Makineler planlaması ve uygulaması
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: MSSedusch
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: d7c59cc1-b2d0-4d90-9126-628f9c7a5538
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/11/2020
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7ddcc5165f5588ff9015d7fafbc2b822268ffea7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80337156"
---
# <a name="azure-virtual-machines-planning-and-implementation-for-sap-netweaver"></a>SAP NetWeaver için Azure Sanal Makineler planlaması ve uygulaması

[767598]:https://launchpad.support.sap.com/#/notes/767598
[773830]:https://launchpad.support.sap.com/#/notes/773830
[826037]:https://launchpad.support.sap.com/#/notes/826037
[965908]:https://launchpad.support.sap.com/#/notes/965908
[1031096]:https://launchpad.support.sap.com/#/notes/1031096
[1139904]:https://launchpad.support.sap.com/#/notes/1139904
[1173395]:https://launchpad.support.sap.com/#/notes/1173395
[1245200]:https://launchpad.support.sap.com/#/notes/1245200
[1409604]:https://launchpad.support.sap.com/#/notes/1409604
[1558958]:https://launchpad.support.sap.com/#/notes/1558958
[1585981]:https://launchpad.support.sap.com/#/notes/1585981
[1588316]:https://launchpad.support.sap.com/#/notes/1588316
[1590719]:https://launchpad.support.sap.com/#/notes/1590719
[1597355]:https://launchpad.support.sap.com/#/notes/1597355
[1605680]:https://launchpad.support.sap.com/#/notes/1605680
[1619720]:https://launchpad.support.sap.com/#/notes/1619720
[1619726]:https://launchpad.support.sap.com/#/notes/1619726
[1619967]:https://launchpad.support.sap.com/#/notes/1619967
[1750510]:https://launchpad.support.sap.com/#/notes/1750510
[1752266]:https://launchpad.support.sap.com/#/notes/1752266
[1757924]:https://launchpad.support.sap.com/#/notes/1757924
[1757928]:https://launchpad.support.sap.com/#/notes/1757928
[1758182]:https://launchpad.support.sap.com/#/notes/1758182
[1758496]:https://launchpad.support.sap.com/#/notes/1758496
[1772688]:https://launchpad.support.sap.com/#/notes/1772688
[1814258]:https://launchpad.support.sap.com/#/notes/1814258
[1882376]:https://launchpad.support.sap.com/#/notes/1882376
[1909114]:https://launchpad.support.sap.com/#/notes/1909114
[1922555]:https://launchpad.support.sap.com/#/notes/1922555
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1941500]:https://launchpad.support.sap.com/#/notes/1941500
[1956005]:https://launchpad.support.sap.com/#/notes/1956005
[1973241]:https://launchpad.support.sap.com/#/notes/1973241
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2039619]:https://launchpad.support.sap.com/#/notes/2039619
[2069760]:https://launchpad.support.sap.com/#/notes/2069760
[2121797]:https://launchpad.support.sap.com/#/notes/2121797
[2134316]:https://launchpad.support.sap.com/#/notes/2134316
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
[2243692]:https://launchpad.support.sap.com/#/notes/2243692

[azure-cli]:../../../cli-install-nodejs.md
[azure-portal]:https://portal.azure.com
[azure-ps]:/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-resource-manager/management/azure-subscription-service-limits]:../../../azure-resource-manager/management/azure-subscription-service-limits.md
[azure-resource-manager/management/azure-subscription-service-limits-subscription]:../../../azure-resource-manager/management/azure-subscription-service-limits.md#subscription-limits

[dbms-guide]:dbms-guide.md
[dbms-guide-2.1]:dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f
[dbms-guide-2.2]:dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91
[dbms-guide-2.3]:dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152
[dbms-guide-2]:dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64
[dbms-guide-3]:dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3
[dbms-guide-5.5.1]:dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268
[dbms-guide-5.5.2]:dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b
[dbms-guide-5.6]:dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8
[dbms-guide-5.8]:dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30
[dbms-guide-5]:dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737
[dbms-guide-8.4.1]:dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573
[dbms-guide-8.4.2]:dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d
[dbms-guide-8.4.3]:dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c
[dbms-guide-8.4.4]:dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818
[dbms-guide-900-sap-cache-server-on-premises]:dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:deployment-guide.md
[deployment-guide-2.2]:deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94
[deployment-guide-3.1.2]:deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab
[deployment-guide-3.2]:deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281
[deployment-guide-3.3]:deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2
[deployment-guide-3.4]:deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1
[deployment-guide-3]:deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e
[deployment-guide-4.1]:deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7
[deployment-guide-4.2]:deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e
[deployment-guide-4.3]:deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc
[deployment-guide-4.4.2]:deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542
[deployment-guide-4.4]:deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d
[deployment-guide-4.5.1]:deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4
[deployment-guide-4.5.2]:deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f
[deployment-guide-4.5]:deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca
[deployment-guide-5.1]:deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2
[deployment-guide-5.2]:deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1
[deployment-guide-5.3]:deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8

[deployment-guide-configure-monitoring-scenario-1]:deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b
[deployment-guide-configure-proxy]:deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d
[deployment-guide-figure-100]:media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:deployment-guide.md#figure-11
[deployment-guide-figure-1100]:media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:deployment-guide.md#figure-14
[deployment-guide-figure-1400]:media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:deployment-guide.md#figure-5
[deployment-guide-figure-50]:media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:deployment-guide.md#figure-6
[deployment-guide-figure-600]:media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:deployment-guide.md#figure-7
[deployment-guide-figure-700]:media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b

[deploy-template-cli]:../../../resource-group-template-deploy-cli.md
[deploy-template-portal]:../../../resource-group-template-deploy-portal.md
[deploy-template-powershell]:../../../resource-group-template-deploy.md

[dr-guide-classic]:https://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md
[getting-started-dbms]:get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:../../virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:../../virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:../../virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:../../virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:../../virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:../../virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:https://go.microsoft.com/fwlink/?LinkId=613056

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-Azvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:planning-guide.md  
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff
[planning-guide-11]:planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058
[planning-guide-11.4.1]:planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77
[planning-guide-11.5]:planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f
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

[powershell-install-configure]:https://docs.microsoft.com/powershell/azure/install-az-ps
[resource-group-authoring-templates]:../../../resource-group-authoring-templates.md
[resource-group-overview]:../../../azure-resource-manager/management/overview.md
[resource-groups-networking]:../../../networking/networking-overview.md
[sap-pam]:https://support.sap.com/pam
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[storage-azure-cli]:../../../storage/common/storage-azure-cli.md
[storage-azure-cli-copy-blobs]:../../../storage/common/storage-azure-cli.md#copy-blobs
[storage-introduction]:../../../storage/common/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]:../../../storage/common/storage-powershell-guide-full.md
[storage-premium-storage-preview-portal]:../../windows/disks-types.md
[storage-redundancy]:../../../storage/common/storage-redundancy.md
[storage-scalability-targets]:../../../storage/common/scalability-targets-standard-accounts.md
[storage-use-azcopy]:../../../storage/common/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:../../linux/attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../../../resource-manager-deployment-model.md
[virtual-machines-Az-versus-azuresm]:virtual-machines-linux-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md
[virtual-machines-linux-agent-user-guide]:../../extensions/agent-linux.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../extensions/agent-windows.md#command-line-options
[virtual-machines-linux-capture-image]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager-capture]:../../linux/capture-image.md#step-2-capture-the-vm
[virtual-machines-windows-capture-image-resource-manager]:../../windows/capture-image.md
[virtual-machines-windows-capture-image]:../../virtual-machines-windows-generalize-vhd.md
[virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture]:../../virtual-machines-windows-generalize-vhd.md
[virtual-machines-linux-configure-raid]:../../linux/configure-raid.md
[virtual-machines-linux-configure-lvm]:../../linux/configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../../linux/suse-create-upload-vhd.md
[virtual-machines-linux-create-upload-vhd-oracle]:../../linux/oracle-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../../linux/update-agent.md
[virtual-machines-manage-availability]:../../linux/manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-create-powershell.md
[virtual-machines-sizes-linux]:../../linux/sizes.md
[virtual-machines-sizes-windows]:../../windows/sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]:./../../windows/sqlclassic/virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]:./../../windows/sqlclassic/virtual-machines-windows-classic-ps-sql-int-listener.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]:./../../windows/sql/virtual-machines-windows-sql-high-availability-dr.md
[virtual-machines-sql-server-infrastructure-services]:./../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md
[virtual-machines-sql-server-performance-best-practices]:./../../windows/sql/virtual-machines-windows-sql-performance.md
[virtual-machines-upload-image-windows-resource-manager]:../../virtual-machines-windows-upload-image.md
[virtual-machines-windows-tutorial]:../../virtual-machines-windows-hero-tutorial.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/documentation/templates/sql-server-2014-alwayson-dsc/
[virtual-network-deploy-multinic-arm-cli]:../../linux/multiple-nics.md
[virtual-network-deploy-multinic-arm-ps]:../../windows/multiple-nics.md
[virtual-network-deploy-multinic-arm-template]:../../../virtual-network/template-samples.md
[virtual-networks-configure-vnet-to-vnet-connection]:../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../../../virtual-network/manage-virtual-network.md#create-a-virtual-network
[virtual-networks-manage-dns-in-vnet]:../../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics-windows]:../../windows/multiple-nics.md
[virtual-networks-multiple-nics-linux]:../../linux/multiple-nics.md
[virtual-networks-nsg]:../../../virtual-network/security-overview.md
[virtual-networks-reserved-private-ip]:../../../virtual-network/virtual-networks-static-private-ip-arm-ps.md
[virtual-networks-static-private-ip-arm-pportal]:../../../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-udr-overview]:../../../virtual-network/virtual-networks-udr-overview.md
[vpn-gateway-about-vpn-devices]:../../../vpn-gateway/vpn-gateway-about-vpn-devices.md
[vpn-gateway-create-site-to-site-rm-powershell]:../../../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]:../../../vpn-gateway/vpn-gateway-about-vpngateways.md
[vpn-gateway-site-to-site-create]:../../../vpn-gateway/vpn-gateway-site-to-site-create.md
[vpn-gateway-vpn-faq]:../../../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../../../cli-install-nodejs.md
[xplat-cli-azure-resource-manager]:../../../xplat-cli-azure-resource-manager.md
[capture-image-linux-step-2-create-vm-image]:../../linux/capture-image.md#step-2-create-vm-image



Microsoft Azure, şirketlerin uzun satın alma döngüleri olmadan en az sürede bilgi işlem ve depolama kaynakları edinmelerine olanak tanır. Azure Sanal Makine hizmeti, şirketlerin SAP NetWeaver tabanlı uygulamalar gibi klasik uygulamaları Azure'a dağıtmasına ve şirket içinde daha fazla kaynak elde etmeden güvenilirliklerini ve kullanılabilirliklerini genişletmesine olanak tanır. Azure Sanal Makine Hizmetleri, şirketlerin Azure Sanal Makinelerini şirket içi etki alanları, Özel Bulutları ve SAP Sistem Ortamına etkin bir şekilde entegre etmelerine olanak tanıyan tesisler arası bağlantıyı da destekler.
Bu teknik inceleme, Microsoft Azure Sanal Makine'nin temellerini açıklar ve Azure'daki SAP NetWeaver yüklemeleri için planlama ve uygulama konularının gözden geçirilmesini sağlar ve bu nedenle gerçek başlatmadan önce okunacak belge olmalıdır SAP NetWeaver'ın Azure'daki dağıtımları.
Makale, SAP yazılımının belirli platformlardaki yüklemeleri ve dağıtımları için birincil kaynakları temsil eden SAP Yükleme Belgeleri ve SAP Notlarını tamamlar.

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

## <a name="summary"></a>Özet
Cloud Computing, küçük şirketlerden büyük ve çok uluslu şirketlere kadar BT sektöründe giderek daha fazla önem kazanan yaygın olarak kullanılan bir terimdir.

Microsoft Azure, çok çeşitli yeni olanaklar sunan Microsoft'un Bulut Hizmetleri Platformudur. Artık müşteriler bulutta bir hizmet olarak uygulamaları hızla temin ve sağlamamayı mümkün kılabilir, bu nedenle teknik veya bütçe kısıtlamalarıyla sınırlı değildir. Şirketler, donanım altyapısına zaman ve bütçe yatırımı yapmak yerine, uygulama, iş süreçleri ve müşteriler ve kullanıcılar için faydalarına odaklanabilir.

Microsoft, Microsoft Azure Sanal Makine Hizmetleri ile kapsamlı bir Hizmet Olarak Altyapı (IaaS) platformu sunmaktadır. SAP NetWeaver tabanlı uygulamalar, Azure Sanal Makinelerinde (IaaS) desteklenmektedir. Bu teknik incelemede, Microsoft Azure'daki SAP NetWeaver tabanlı uygulamaların tercih edilen platform olarak nasıl planlanıp uygulanacağı açıklanmaktadır.

Makalenin kendisi iki ana unsura odaklanır:

* İlk bölümde, Azure'daki SAP NetWeaver tabanlı uygulamalar için desteklenen iki dağıtım desenini açıklanır. Ayrıca, SAP dağıtımları göz önünde bulundurularak Azure'un genel kullanımını da açıklar.
* İkinci bölümde, ilk bölümde açıklanan farklı senaryoları uygulayarak ayrıntılar.

Ek kaynaklar için bu belgedeki [kaynaklar][planning-guide-1.2] bölümüne bakın.

### <a name="definitions-upfront"></a>Tanımlar peşin
Belge boyunca aşağıdaki terimleri kullanırız:

* IaaS: Hizmet Olarak Altyapı
* PaaS: Hizmet Olarak Platform
* SaaS: Hizmet Olarak Yazılım
* SAP Bileşeni: ECC, BW, Solution Manager veya S/4HANA gibi tek bir SAP uygulaması.  SAP bileşenleri geleneksel ABAP veya Java teknolojilerine veya İş Nesneleri gibi NetWeaver tabanlı olmayan bir uygulamaya dayalı olabilir.
* SAP Ortamı: Geliştirme, QAS, Eğitim, DR veya Üretim gibi bir iş işlevini gerçekleştirmek için mantıksal olarak gruplanmış bir veya daha fazla SAP bileşeni.
* SAP Yatayı: Bu terim, müşterinin BT ortamındaki tüm SAP varlıklarını ifade eder. SAP ortamı tüm üretim ve üretim dışı ortamları içerir.
* SAP Sistemi: Örneğin, bir SAP ERP geliştirme sistemi, SAP BW test sistemi, SAP CRM üretim sistemi, vb. DBMS katmanı ve uygulama katmanının kombinasyonu Azure dağıtımlarında, bu iki katmanı şirket içi ve Azure arasında bölmek desteklenmez. Sap sistemi şirket içinde veya Azure'da dağıtılır anlamına gelir. Ancak, SAP ortamının farklı sistemlerini Azure'a veya şirket içinde dağıtabilirsiniz. Örneğin, SAP CRM geliştirme ve test sistemlerini Azure'da, ancak SAP CRM üretim sistemini şirket içinde dağıtabilirsiniz.
* Binalar arası veya karma: Sanal Müzik'lerin yerinde, çok siteye veya şirket içi veri merkezi(ler) ile Azure arasında ExpressRoute bağlantısı olan bir Azure aboneliğine dağıtıldığı bir senaryoyu açıklar. Ortak Azure belgelerinde, bu tür dağıtımlar binalar arası veya karma senaryolar olarak da tanımlanır. Bağlantının nedeni, şirket içi etki alanlarını, şirket içi Active Directory/OpenLDAP'ı ve şirket içi DNS'yi Azure'a genişletmektir. Şirket içi manzara, aboneliğin Azure varlıklarına genişletilir. Bu uzantıya sahip olan VM'ler şirket içi etki alanının bir parçası olabilir. Şirket içi etki alanı kullanıcıları sunuculara erişebilir ve bu VM'lerde (DBMS hizmetleri gibi) hizmetler çalıştırabilir. Şirket içinde dağıtılan VM'ler ile Azure'un dağıtılmış VM'leri arasında iletişim ve ad çözümlemesi mümkündür. Bu, SAP varlıklarını Azure'a dağıtan en yaygın ve neredeyse münhasır durumdur. Daha fazla bilgi için [bu][vpn-gateway-cross-premises-options] makaleye ve [bu makaleye][vpn-gateway-site-to-site-create]bakın.
* SAP için Azure İzleme Uzantısı, Gelişmiş İzleme ve Azure Uzantısı: Bir ve aynı öğeyi açıklayın. SAP Ana Bilgisayar Aracısı'na Azure altyapısı yla ilgili bazı temel verileri sağlamak için sizin yeriniz tarafından dağıtılması gereken bir VM uzantısını açıklar. SAP notlarında SAP, bunu İzleme Uzantısı veya Gelişmiş izleme olarak adlandırabilir. Azure'da, bunu **SAP için Azure Uzantısı**olarak anlıyoruz.

> [!NOTE]
> SAP sistemlerini çalıştıran Azure Sanal Makinelerin in adak alan adı olan SAP sistemlerinin şirket içi veya karma dağıtımları, üretim SAP sistemleri için desteklenir. Parçaları dağıtmak veya SAP manzaralarını Azure'a dağıtmak için tesisler arası veya karma yapılandırmalar desteklenir. Azure'da tüm SAP ortamını çalıştırmak bile bu VM'lerin şirket içi etki alanının ve ADS/OpenLDAP'ın bir parçası olmasını gerektirir. 
>
>



### <a name="resources"></a><a name="e55d1e22-c2c8-460b-9897-64622a34fdff"></a>Kaynaklar
Azure belgelerindeki SAP iş yükünün giriş noktası [burada](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started)bulunur. Bu giriş noktasından başlayarak aşağıdaki konuları kapsayan birçok makale bulabilirsiniz:

- SAP NetWeaver ve İş Bir Azure'da
- Azure'daki çeşitli DBMS sistemleri için SAP DBMS kılavuzları
- Azure'da SAP iş yükü için yüksek kullanılabilirlik ve olağanüstü durum kurtarma
- Azure'da SAP HANA çalıştırmak için özel kılavuz
- SAP HANA DBMS için Azure HANA Büyük Örneklerine özel kılavuz 


> [!IMPORTANT]
> Başvurulan SAP Yükleme Kılavuzları'na veya diğer SAP belgelerine mümkün olan her yerde <http://service.sap.com/instguides>bir bağlantı kullanılır (Reference InstGuide-01, bkz. ). Microsoft belgeleri yalnızca SAP yazılımının yüklenmesi ve işletilmesi için belirli görevleri kapsadığından, belirli SAP işlevlerinin ön koşulları, kurulum süreci veya ayrıntıları söz konusu olduğunda, SAP belgeleri ve kılavuzları her zaman dikkatle okunmalıdır. Microsoft Azure Sanal Makine.
>
>

Aşağıdaki SAP Notları Azure'daki SAP konusuyla ilgilidir:

| Not numarası | Başlık |
| --- | --- |
| [1928533] |Azure'da SAP Uygulamaları: Desteklenen Ürünler ve Boyutlandırma |
| [2015553] |Microsoft Azure'da SAP: Ön Koşulları Destekle |
| [1999351] |SAP için Gelişmiş Azure İzleme Sorun Giderme |
| [2178632] |Microsoft Azure'da SAP için Temel İzleme Ölçümleri |
| [1409604] |Windows'da Sanallaştırma: Gelişmiş İzleme |
| [2191498] |Azure ile Linux'ta SAP: Gelişmiş İzleme |
| [2243692] |Microsoft Azure (IaaS) VM üzerinde Linux: SAP lisans sorunları |
| [1984787] |SUSE LINUX Enterprise Server 12: Kurulum notları |
| [2002167] |Red Hat Enterprise Linux 7.x: Kurulum ve Yükseltme |
| [2069760] |Oracle Linux 7.x SAP Kurulumu ve Yükseltmesi |
| [1597355] |Linux için takas alanı önerisi |

Ayrıca Linux için tüm SAP Notlarını içeren [SCN Wiki'yi](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) okuyun.

Azure aboneliklerinin genel varsayılan sınırlamaları ve maksimum sınırlamaları [bu makalede][azure-resource-manager/management/azure-subscription-service-limits-subscription]bulunabilir.

## <a name="possible-scenarios"></a>Olası Senaryolar
SAP genellikle işletmeler içinde en kritik görev uygulamaları biri olarak görülüyor. Bu uygulamaların mimarisi ve işlemleri çoğunlukla karmaşıktır ve kullanılabilirlik ve performans gereksinimlerini karşılamanızı sağlamak önemlidir.

Bu nedenle işletmeler, bu tür iş açısından kritik iş süreçlerini yürütmek için hangi bulut sağlayıcısını seçeceklerini dikkatlice düşünmek zorunda kalmıştır. Azure, iş açısından kritik SAP uygulamaları ve iş süreçleri için ideal bir genel bulut platformudur. Çok çeşitli Azure altyapısı göz önüne alındığında, mevcut SAP NetWeaver ve S/4HANA sistemlerinin neredeyse tamamı bugün Azure'da barındırılabilir. Azure, VM'lere birçok Terabayt bellek ve 200'den fazla CPU sağlar. Bunun ötesinde Azure, 24 TB'a kadar ÖLÇEKlendirilebilen HANA dağıtımlarına ve 120 TB'a kadar SAP HANA ölçekli dağıtımlara olanak tanıyan [HANA Büyük Örnekleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)sunar. Bugün, şirket içinde neredeyse tüm SAP senaryolarının Azure'da da çalıştırılabildiği belirtilebilir. 

Senaryoların ve bazı desteklenmeyen senaryoların kaba bir açıklaması için, [Azure sanal makine destekli senaryolarda](./sap-planning-supported-configurations.md)belge SAP iş yükünü görün.

Azure'da dağıtmak istediğiniz mimarinizin planlaması ve geliştirilmesi boyunca başvurulan belgelerde desteklenmeyen olarak adlandırılan bu senaryoları ve bazı koşulları denetleyin.

Genel olarak en yaygın dağıtım deseni, görüntülenen gibi bir binalar arası senaryodur

![Siteden Siteye Bağlantı (tesisler arası) ile VPN][planning-guide-figure-300]

Birçok müşterinin bir şirket içi dağıtım deseni uygulamasının nedeni, tüm uygulamaların Azure ExpressRoute kullanarak şirket içinde Azure'a genişlemesi ve Azure'u sanal veri merkezi olarak ele alabilmek için en şeffaf olmasıdır. Giderek daha fazla varlık Azure'a taşındıkça, Azure'da dağıtılan altyapı ve ağ altyapısı büyüyecek ve şirket içi varlıklar buna göre azalacak. Her şey kullanıcılar ve uygulamalar için şeffaf.

SAP sistemlerini genel olarak Azure IaaS veya IaaS'ye başarılı bir şekilde dağıtmak için, geleneksel dış kaynak veya ev sahibi ve IaaS teklifleri arasındaki önemli farkları anlamak önemlidir. Geleneksel ana kaynak veya dış kaynak altyapıyı (ağ, depolama ve sunucu türü) müşterinin barındırmak istediği iş yüküne uyarlarken, iş yükünü karakterize etmek ve doğru Azure'u seçmek müşterinin veya iş ortağının sorumluluğudur IaaS dağıtımları için VM, depolama ve ağ bileşenleri.

Azure'a dağıtımınızın planlanması için veri toplamak için şunları yapmak önemlidir:

- Azure VM'lerde çalışan SAP ürünlerinin desteklenmelerini değerlendirin
- Bu SAP ürünleri için belirli İşletim Sistemi sürümlerinin belirli Azure VM'leriyle desteklenmeyi değerlendirin
- Belirli Azure VM'leri ile SAP ürünleriniz için hangi DBMS sürümlerinin desteklenmelerini değerlendirin
- Desteklenen bir yapılandırmaya ulaşmak için gerekli os/DBMS sürümlerinden bazılarının SAP sürümü, Destek Paketi yükseltmesi ve çekirdek yükseltmeleri gerçekleştirmenizi gerektiretip gerektirmediğini değerlendirin
- Azure'da dağıtmak için farklı işletim sistemlerine geçmeniz gerekip gerekmediğini değerlendirin.

Azure'da desteklenen SAP bileşenleri, desteklenen Azure altyapı birimleri ve ilgili işletim sistemi sürümleri ve DBMS sürümleri hakkındaki ayrıntılar makalede [Azure dağıtımları için hangi SAP yazılımının desteklendirildigünü](./sap-supported-product-on-azure.md)açıklanmıştır. Geçerli SAP sürümlerinin, işletim sisteminin ve DBMS sürümlerinin değerlendirilmelerinden elde edilen sonuçlar, SAP sistemlerini Azure'a taşıma çabaları üzerinde büyük bir etkiye sahiptir. Bu değerlendirmenin sonuçları, SAP sürüm yükseltmelerinin veya işletim sistemlerinin değiştirilmesinin gerekli olduğu durumlarda önemli hazırlık çabaları olup olmadığını belirleyecektir.


## <a name="azure-regions"></a><a name="be80d1b9-a463-4845-bd35-f4cebdb5424a"></a>Azure Bölgeleri
Microsoft'un Azure hizmetleri Azure bölgelerinde toplanır. Azure bölgesi, farklı Azure hizmetlerini çalıştıran ve barındıran donanım ve altyapıyı içeren veri merkezlerinden oluşan bir veya koleksiyondur. Bu altyapı, işlem düğümleri veya depolama düğümleri olarak işlev gören veya ağ işlevselliğini çalıştıran çok sayıda düğüm içerir. 

Farklı Azure bölgelerinin listesi için [Azure coğrafyaları](https://azure.microsoft.com/global-infrastructure/geographies/)makalesine bakın. Tüm Azure bölgeleri aynı hizmetleri sunmuyor. Çalıştırmak istediğiniz SAP ürününe ve bununla ilgili işletim sistemi ve DBMS'ye bağlı olarak, belirli bir bölgenin gereksinim duyduğunuz VM türlerini sunamayacağı bir duruma rastlayabilirsiniz. Bu, özellikle genellikle M/Mv2 VM serisinin VM'leri gereken SAP HANA'yı çalıştırmak için geçerlidir. Bu VM aileleri yalnızca bölgelerin bir alt kümesinde dağıtılır. Bölgelere göre kullanılabilen site Ürünleri'nin yardımıyla hangi bölgelerde vm, türleri, Azure depolama türleri veya diğer Azure Hizmetlerinin tam olarak hangi bölgelerde [kullanılabildiği](https://azure.microsoft.com/global-infrastructure/services/)öğrenebilirsiniz. Planlamanıza başladığınızda ve birincil bölge ve nihayetinde ikincil bölge olarak belirli bölgeleri göz önünde bulundurdığınızda, öncelikle bu bölgelerde gerekli hizmetlerin mevcut olup olmadığını araştırmanız gerekir. 

### <a name="availability-zones"></a>Kullanılabilirlik Alanları
Azure bölgelerinden bazıları Kullanılabilirlik Bölgeleri adlı bir kavram uyguladı. Kullanılabilirlik Bölgeleri, Azure bölgesindeki fiziksel olarak ayrı konumlardır. Her Kullanılabilirlik Alanı bağımsız enerji, soğutma ve ağ kaynaklarıyla donatılmış bir veya daha fazla veri merkezinden oluşur. Örneğin, Azure'un iki Kullanılabilirlik Bölgesinde iki VM dağıtmak ve SAP DBMS sisteminiz veya SAP Merkezi Hizmetleri için yüksek kullanılabilirlik çerçevesi uygulamak, Azure'daki en iyi SLA'yı sağlar. Azure'daki bu sanal makine SLA için [Sanal Makine SLA'larının](https://azure.microsoft.com/support/legal/sla/virtual-machines/)en son sürümünü kontrol edin. Azure bölgeleri son yıllarda hızla geliştiği ve genişlediğinden, Azure bölgelerinin topolojisi, fiziksel veri merkezi sayısı, bu veri merkezleri arasındaki mesafe ve Azure Kullanılabilirlik Bölgeleri arasındaki mesafe farklı olabilir. Ve bu ağ gecikmesi ile.

Kullanılabilirlik Bölgeleri ilkesi [HANA Büyük Örnekleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)HANA özel hizmet için geçerli değildir. HANA Büyük Örnekleri için Hizmet Düzeyi [anlaşmaları, Azure Büyük Örnekleri'ndeki SAP HANA için SLA makalesinde](https://azure.microsoft.com/support/legal/sla/sap-hana-large/) bulunabilir 


### <a name="fault-domains"></a><a name="df49dc09-141b-4f34-a4a2-990913b30358"></a>Hata Etki Alanları
Hata Etki Alanları, veri merkezlerinde bulunan fiziksel altyapıyla yakından ilişkili fiziksel bir hata birimini temsil eder ve fiziksel bir bıçak veya raf Hata Etki Alanı olarak kabul edilebilirken, ikisi arasında doğrudan bire bir eşleme yoktur.

Microsoft Azure Sanal Makine Hizmetlerinde bir SAP sisteminin parçası olarak birden fazla Sanal Makine dağıttığınızda, uygulamanızı farklı Fay Etki Alanları'na dağıtmak için Azure Kumaş Denetleyicisi'ni etkileyerek daha yüksek gereksinimleri karşılayabilirsiniz kullanılabilirlik SLA'ları. Ancak, Hata Etki Alanları'nın bir Azure Ölçeği Birimi (yüzlerce Bilgi İşlem düğümü veya Depolama düğümü ve ağ koleksiyonu) üzerinden dağıtılması veya VM'lerin belirli bir Hata Etki Alanına atanması, üzerinde doğrudan denetimiolmayan bir şeydir. Azure kumaş denetleyicisini farklı Hata Etki Alanları üzerinde bir vm kümesi dağıtmaya yönlendirmek için, dağıtım sırasında VM'lere bir Azure kullanılabilirliği kümesi atamanız gerekir. Azure kullanılabilirlik kümeleri hakkında daha fazla bilgi için, bu belgede [Azure kullanılabilirlik kümeleri][planning-guide-3.2.3] bölümüne bakın.


### <a name="upgrade-domains"></a><a name="fc1ac8b2-e54a-487c-8581-d3cc6625e560"></a>Etki Alanlarını Yükseltme
Yükseltme Etki Alanları, birden çok VM'de çalışan SAP örneklerinden oluşan SAP sistemindeki bir VM'nin nasıl güncelleştirilebildiğini belirlemeye yardımcı olan mantıksal bir birimi temsil eder. Yükseltme gerçekleştiğinde, Microsoft Azure bu Yükseltme Etki Alanlarını tek tek güncelleştirme işleminden geçer. Dağıtım zamanında KiVM'leri farklı Yükseltme Etki Alanları'na yayarak SAP sisteminizi kısmen olası kapalı kalma sürelerinden koruyabilirsiniz. Azure'u farklı Yükseltme Etki Alanları'na yayılmış bir SAP sisteminin VM'lerini dağıtmaya zorlamak için, her VM'nin dağıtım süresine belirli bir öznitelik ayarlamanız gerekir. Hata Etki Alanları'na benzer şekilde, Azure Ölçeği Birimi birden çok Yükseltme Etki Alanına ayrılır. Azure kumaş denetleyicisini farklı Yükseltme Etki Alanları üzerinde bir vm kümesi dağıtmak üzere yönlendirmek için, dağıtım sırasında VM'lere bir Azure Kullanılabilirlik Kümesi atamanız gerekir. Azure kullanılabilirlik kümeleri hakkında daha fazla bilgi için aşağıdaki bölüm [Azure kullanılabilirlik kümeleri][planning-guide-3.2.3] bölümüne bakın.


### <a name="azure-availability-sets"></a><a name="18810088-f9be-4c97-958a-27996255c665"></a>Azure kullanılabilirlik kümeleri
Azure Sanal Makineler, tek bir Azure kullanılabilirlik kümesindeki Azure Kumaş Denetleyicisi tarafından farklı Hata ve Yükseltme Etki Alanları üzerinden dağıtılır. Farklı Hata ve Yükseltme Etki Alanları üzerinden dağıtımAmacı, altyapı bakımı veya bir Hata Etki Alanı içinde bir hata durumunda bir SAP sisteminin tüm VM'lerinin kapatılmasını önlemektir. Varsayılan olarak, VM'ler kullanılabilirlik kümesinin bir parçası değildir. Bir VM'nin kullanılabilirlik kümesine katılımı, dağıtım sırasında veya daha sonra bir VM'nin yeniden yapılandırması ve yeniden dağıtılmasıyla tanımlanır.

Azure kullanılabilirlik kümeleri kavramını ve kullanılabilirlik kümelerinin Hata ve Yükseltme Etki Alanları ile ilgili biçimini anlamak için [bu makaleyi][virtual-machines-manage-availability]okuyun. 

Kullanılabilirlik kümelerini tanımladıkça ve farklı VM ailelerinin çeşitli VM'lerini tek bir kullanılabilirlik kümesi içinde karıştırmaya çalışırken, belirli bir VM türünü böyle bir kullanılabilirlik kümesine eklemenizi engelleyen sorunlarla karşılaşabilirsiniz. Bunun nedeni, kullanılabilirlik kümesinin belirli bir tür işlem ana bilgisayarları içeren birimi ölçeklendirmeye bağlı olmasıdır. Ve belirli bir tür bilgi işlem ana bilgisayar yalnızca belirli türde VM ailelerini çalıştırabilir. Örneğin, bir kullanılabilirlik kümesi oluşturur ve ilk VM'yi kullanılabilirlik kümesine dağıtırsanız ve Esv3 ailesinin bir VM türünü seçerseniz ve sonra M ailesinin ikinci VM vm'si olarak dağıtmaya çalışırsanız, ikinci ayırmada reddedilirsiniz. Bunun nedeni, Esv3 ailesi VM'lerinin M ailesinin sanal makineleriyle aynı ana bilgisayar donanımında çalışmamasıdır. Aynı sorun, VM'leri yeniden boyutlandırmaya çalıştığınızda ve Bir VM'yi Esv3 ailesinden M ailesinin VM türüne taşımaya çalıştığınızda da oluşabilir. Aynı ana bilgisayar donanımında barındırılamayan bir VM ailesine yeniden boyutlandırma durumunda, kullanılabilirlik kümenizdeki tüm VM'leri kapatmanız ve diğer ana bilgisayar makine türünde çalıştırabilmek için yeniden boyutlandırmanız gerekir. Kullanılabilirlik kümesi içinde dağıtılan VM'lerin [SLA'ları için Sanal Makine SLA'ları](https://azure.microsoft.com/support/legal/sla/virtual-machines/)makalesini kontrol edin. 

Kullanılabilirlik kümesi ve ilgili güncelleme ve hata etki alanı ilkesi [HANA Büyük Örnekleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)HANA özel hizmet için geçerli değildir. HANA Büyük Örnekleri için Hizmet Düzeyi [anlaşmaları, Azure Büyük Örnekleri'ndeki SAP HANA için SLA makalesinde](https://azure.microsoft.com/support/legal/sla/sap-hana-large/)bulunabilir. 

> [!IMPORTANT]
> Azure Kullanılabilirlik Bölgeleri ve Azure kullanılabilirlik kümeleri kavramları birbirini dışlar. Bu, belirli bir Kullanılabilirlik Bölgesine veya Azure kullanılabilirlik kümesine bir çift veya birden çok VM dağıtabileceğiniz anlamına gelir. Ama ikisi birden değil.


## <a name="azure-virtual-machine-services"></a>Azure sanal makine hizmetleri
Azure, dağıtmak için seçebileceğiniz çok çeşitli sanal makineler sunar. Ön teknoloji ve altyapı satın alımları için gerek yoktur. Azure VM hizmeti, web uygulamalarını ve bağlı uygulamaları barındırmak, ölçeklendirmek ve yönetmek için isteğe bağlı bilgi işlem ve depolama sağlayarak uygulamaları korumayı ve çalıştırmayı kolaylaştırır. Altyapı yönetimi, kullanım gereksinimlerini birkaç farklı fiyatlandırma modeli seçeneğiyle karşılamak için yüksek kullanılabilirlik ve dinamik ölçeklendirme için tasarlanmış bir platform ile otomatikleştirilir.

![Microsoft Azure Sanal Makine Hizmetlerinin Konumlandırılması][planning-guide-figure-400]

Azure sanal makineleriyle Microsoft, IaaS örnekleri olarak azure'a özel sunucu görüntüleri dağıtmanızı sağlar. Ya da Azure resim galerisinden zengin bir sarf malzemesi işletim sistemi görsel seçkisi arasından seçim yapabilirsiniz.

Azure Sanal Makine Hizmeti, operasyonel açıdan, şirket içinde dağıtılan sanal makinelere benzer deneyimler sunar. Azure VM'de çalışan belirli işletim sisteminin ve bu VM'deki uygulamalarının yönetiminden, işlemlerinden ve yamalarından siz sorumlusunuz. Microsoft, bu VM'yi Azure altyapısında (Hizmet Olarak Altyapı - IaaS) barındırmanın ötesinde başka bir hizmet sunmuyor. Müşteri olarak dağıttığınız SAP iş yükü için, Microsoft'un IaaS teklifleri dışında hiçbir teklifi yoktur. 

Microsoft Azure platformu çok kiracılı bir platformdur. Sonuç olarak, Azure VM'leri barındıran depolama, ağ ve işlem kaynakları, birkaç istisna dışında kiracılar arasında paylaşılır. Akıllı azaltma ve kota mantığı, bir kiracının başka bir kiracının (gürültülü komşu) performansını ciddi bir şekilde etkilemesini önlemek için kullanılır. Özellikle SAP HANA için Azure platformlarını onaylamak için Microsoft'un, birden çok VM'nin SAP'ye düzenli olarak aynı ana bilgisayarda çalıştırılabildiği durumlar için kaynak yalıtımını kanıtlaması gerekir. Azure'daki mantık, bant genişliğindeki varyansları küçük, yüksek oranda paylaşılan platformlarda müşterilerin şirket içi dağıtımlarında karşılaşabileceğinden daha büyük varyanslar sunmaya çalışsa da, büyük oranda paylaşılan platformlar da kaynak/bant genişliği kullanılabilirliği açısından daha büyük farklar ortaya koyma eğilimindedir. Azure'daki bir SAP sisteminin şirket içi sisteme göre daha büyük farklılıklar yaşama olasılığı dikkate alınması gerekir.

### <a name="azure-virtual-machines-for-sap-workload"></a>SAP iş yükü için azure sanal makineler

SAP iş yükü için seçimi, SAP iş yüküne ve SAP HANA iş yüküne daha özel olarak uygun farklı VM ailelerine daralttın. Doğru VM türünü bulma biçiminiz ve SAP iş yükü yle çalışma yeteneğiniz belgede Azure [dağıtımları için hangi SAP yazılımının desteklenir](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-supported-product-on-azure)olduğu açıklanmıştır. 

> [!NOTE]
> SAP iş yükü için sertifikalı VM türleri, CPU ve bellek kaynaklarının aşırı sağlanması yoktur.

Tamamen desteklenen VM'lerin seçiminin ötesinde, bunların [bölgeye göre kullanılabilen](https://azure.microsoft.com/global-infrastructure/services/)site Ürünlerine dayalı olarak belirli bir bölgede kullanılabilir olup olmadığını da kontrol etmeniz gerekir. Ama daha da önemlisi, olup olmadığını değerlendirmek gerekir:

- Farklı VM türlerinin işlemci ve bellek kaynakları 
- Farklı VM türlerinin IOPS bant genişliği
- Farklı VM türlerinin ağ özellikleri
- Eklenebilecek disk sayısı
- Belirli Azure depolama türlerinden yararlanma becerisi

ihtiyacınızı karşılar. Bu verilerin çoğu [burada (Linux)][virtual-machines-sizes-linux] ve [burada (Windows)][virtual-machines-sizes-windows] belirli bir VM türü için bulunabilir.

Fiyatlandırma modeli olarak, şu gibi listeyi içeren birkaç farklı fiyatlandırma seçeneğiniz var:

- Kullandıkça öde
- Bir yıl ayrılmıştır
- Üç yıl ayrılmıştır
- Spot fiyatlandırma

İşletim sistemleri ve farklı bölgeler etrafında farklı hizmet teklifleri ile farklı tekliflerin her fiyatlandırma sitede [Linux Sanal Makineler Fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) ve Windows Sanal Makineler [Fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-machines/windows/)mevcuttur. Bir yıl ve üç yıl ayrılmış örneklerin ayrıntıları ve esnekliği için şu makaleleri kontrol edin:

- [Azure Rezervasyonları nedir?](https://docs.microsoft.com/azure/cost-management-billing/reservations/save-compute-costs-reservations)
- [Ayrılmış VM Örnekleriyle sanal makine boyutu esnekliği](https://docs.microsoft.com/azure/virtual-machines/windows/reserved-vm-instance-size-flexibility)
- [Azure rezervasyon indirimini sanal makinelere uygulama](https://docs.microsoft.com/azure/cost-management-billing/manage/understand-vm-reservation-charges) 

Spot fiyatlandırma hakkında daha fazla bilgi için [Azure Spot Sanal Makineler](https://azure.microsoft.com/pricing/spot/)makalesini okuyun. Aynı VM türünün fiyatlandırması, farklı Azure bölgeleri arasında da farklı olabilir. Bazı müşteriler için daha ucuz bir Azure bölgesine dağıtmaya değerdi.

Ayrıca, Azure özel bir ana bilgisayar kavramlarını sunar. Özel ana bilgisayar konsepti, Azure tarafından yapılan yama döngüleri üzerinde daha fazla denetim sağlar. Yama süresini kendi zamanlamalarınıza göre zamanlayabilirsiniz. Bu teklif, özellikle normal iş yükü döngüsünü izlemeyen iş yüküne sahip müşterileri hedeflemektedir. Azure'a özel ana bilgisayar tekliflerinin kavramlarını okumak için [Azure Özel Ana Bilgisayar](https://docs.microsoft.com/azure/virtual-machines/windows/dedicated-hosts)makalesini okuyun. Bu teklifi kullanmak SAP iş yükü için desteklenir ve Microsoft'un altyapı ve nihai bakım planlarının yamalanması üzerinde daha fazla denetime sahip olmak isteyen birkaç SAP müşterisi tarafından kullanılır. Microsoft'un sanal makineleri barındıran Azure altyapısını nasıl koruduğu ve yamaları hakkında daha fazla bilgi için [Azure'daki sanal makineler için Bakım makalesini](https://docs.microsoft.com/azure/virtual-machines/maintenance-and-updates)okuyun.

#### <a name="generation-1-and-generation-2-virtual-machines"></a>Nesil 1 ve Nesil 2 sanal makineler
Microsoft'un hipervizör sanal makinelerin iki farklı nesil işleyebilir. Bu biçimlere **Nesil 1** ve **Nesil 2**denir. **Generation 2,** 2012 yılında Windows Server 2012 hypervisor ile tanıtıldı. Azure, Generation 1 sanal makinelerini kullanmaya başladı. Azure sanal makinelerini dağıttığınızda varsayılan değer, Nesil 1 biçimini kullanmaya devam etmektir. Bu arada Nesil 2 VM biçimlerini de dağıtabilirsiniz. [Azure'daki nesil 2 VM'ler için destek](https://docs.microsoft.com/azure/virtual-machines/windows/generation-2) makalesi, Nesil 2 VM olarak dağıtılabilen Azure VM ailelerini listeler. Bu makalede, Hyper-V özel bulut ve Azure üzerinde çalıştırabildikleri için Generation 2 sanal makinelerin çok önemli işlevsel farklılıkları da listelenir. Daha da önemlisi bu makalede, Nesil 1 sanal makineleri ile Nesil 2 VM'ler arasındaki işlevsel farklar Azure'da çalıştırılarken de listelenir. 

> [!NOTE]
> Azure'da çalışan Nesil 1 ve Nesil 2 VM'lerin işlevsel farklılıkları vardır. Bu farklılıkların listesini görmek [için Azure'daki nesil 2 VM'ler için Destek](https://docs.microsoft.com/azure/virtual-machines/windows/generation-2) makalesini okuyun.  
 
Varolan bir VM'yi bir nesilden diğerine taşımak mümkün değildir. Sanal makine üretimini değiştirmek için, istediğiniz neslin yeni bir VM'ini dağıtmanız ve çalıştırdığınız yazılımı nesildeki sanal makinede yeniden yüklemeniz gerekir. Bu yalnızca VM'nin temel VHD görüntüsünü etkiler ve veri diskleri veya ekli NFS veya Kobİ paylaşımları üzerinde hiçbir etkisi yoktur. Örneğin, Bir Nesil 1 VM'de başlangıçta atanan veri diskleri, NFS veya Kobİ paylaşımları

Şu anda, özellikle Azure M Serisi VM'ler ve Mv2 Serisi VM'ler arasında bu sorunla karşılaşacaksınız. Generation 1 VM formatındaki sınırlamalar nedeniyle, Mv2 ailesinin büyük VM'leri Generation 1 formatında sunulamadı, ancak yalnızca Generation 2'de sunulması gerekiyordu. Diğer tarafta, M-Serisi VM ailesi Nesil 2'de konuşlandırılmak üzere henüz etkinleştirilemedi. Sonuç olarak, M serisi ve Mv2 serisi sanal makineler arasında yeniden boyutlandırma, yazılımın diğer VM ailesini hedeflediğiniz sanal bir makineye yeniden yüklemesini gerektirir. Microsoft, Nesil 2 dağıtımları için M serisi VM'leri dağıtmanızı sağlamak için çalışmaktadır. Gelecekte Nesil 2 VM olarak M-serisi VM'ler dağıtmak, M serisi ve Mv2 serisi sanal makineler arasında daha az yeniden boyutlandırma sağlayacak. Her iki yönde de, M-Serisi'nden daha büyük Mv2 serisi sanal makinelere veya daha büyük Mv2 serisi VM'lerden daha küçük M serisi VM'lere kadar aşağı boyutlandırma. Belgeler, M serisi VM'ler Generation 2 VM olarak dağıtılabildiği anda güncellenecektir.    

 

### <a name="storage-microsoft-azure-storage-and-data-disks"></a><a name="a72afa26-4bf4-4a25-8cf7-855d6032157f"></a>Depolama: Microsoft Azure Depolama ve Veri Diskleri
Microsoft Azure Sanal Makineler farklı depolama türleri kullanır. Azure Sanal Makine Hizmetleri'nde SAP'yi uygularken, bu iki ana depolama türü arasındaki farkları anlamak önemlidir:

* Kalıcı olmayan, geçici depolama.
* Kalıcı depolama.

Azure VM'ler, bir VM dağıtıldıktan sonra kalıcı olmayan diskler sunar. VM'nin yeniden başlatılması durumunda, bu sürücülerdeki tüm içerik silinir. Bu nedenle, veri dosyaları ve veritabanlarının günlük/redo dosyalarının hiçbir koşulda bu kalıcı olmayan sürücülerde bulunması gerektiği belirtilmiş olur. Bu kalıcı olmayan sürücülerin tempdb ve geçici tablo alanları için uygun olabileceği bazı veritabanları için özel durumlar olabilir. Ancak, bu kalıcı olmayan sürücüler bu VM ailesiyle sınırlı olduğundan, bu sürücüleri A Serisi VM'ler için kullanmaktan kaçının. Daha fazla ayrıntı için, [Azure'daki Windows VM'lerde geçici sürücüyü anlama makalesini](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/) okuyun

---
> ![Windows][Logo_Windows] Windows
> 
> Sürücü D:\ Azure VM'de, Azure işlem düğümündeki bazı yerel diskler tarafından desteklenen kalıcı olmayan bir sürücüdür. Kalıcı olmadığından, D:\ üzerindeki içerikte yapılan herhangi bir değişikliğin VM yeniden başlatıldığında sürücü kaybolur. Depolanan dosyalar, oluşturulan dizinler, yüklenen uygulamalar vb. gibi "herhangi bir değişiklik" ile
> 
> ![Linux][Logo_Linux] Linux
> 
> Linux Azure VM'ler, Azure bilgi işlem düğümündeki yerel diskler tarafından desteklenen kalıcı olmayan bir sürücü olan /mnt/kaynağa otomatik olarak bir sürücü monte eder. Bu, kalıcı olmadığından, VM yeniden başlatıldığında /mnt/resource'daki içerikte yapılan değişikliklerin kaybolduğu anlamına gelir. Depolanan dosyalar, oluşturulan dizinler, yüklenen uygulamalar vb. gibi değişikliklerde.
> 
> 

---

Microsoft Azure Depolama kalıcı depolama ve SAN depolama görülen tipik koruma ve artıklık düzeyleri sağlar. Azure Depolama'ya dayalı diskler, Azure Depolama Hizmetleri'nde bulunan sanal sabit disktir (VHD'lerdir). Yerel Işletim Sistemi-Diski\, (Windows C: Linux /dev/sda1) Azure Depolama'da depolanır ve VM'ye monte edilen ek Birimler/Diskler de burada depolanır.

Varolan bir VHD'yi şirket içinden yüklemek veya Azure içinden boş hd'ler oluşturmak ve bu VHD'leri dağıtılan VM'lere eklemek mümkündür.

Azure Depolama'ya bir VHD oluşturduktan veya yükledikten sonra, bunları varolan bir Sanal Makineye monte edip takmak ve varolan (monte edilmemiş) VHD'yi kopyalamak mümkündür.

Bu VHD'ler kalıcı olduğundan, sanal makine örneğini yeniden başlatıp yeniden oluştururken bu VH'ler içindeki veriler ve değişiklikler güvenlidir. Bir örnek silinse bile, bu VHD'ler güvende kalır ve yeniden dağıtılabilir veya işletim sistemi dışındaki disklerin diğer VM'lere monte edilebiliyor.

Azure Depolama hakkında daha fazla bilgiyi burada bulabilirsiniz:

* <https://azure.microsoft.com/documentation/services/storage/>
* <https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>
* <https://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/azure-disk-encryption-for-linux-and-windows-virtual-machines-public-preview.aspx>

#### <a name="azure-standard-storage"></a>Azure Standart Depolama
Azure Standart depolama, Azure IaaS yayımlandığında kullanılabilen depolama türüydü. Tek bir disk başına uygulanan IOPS kotaları vardı. Yaşanan gecikme gecikmesi, genellikle şirket içinde barındırılan üst düzey SAP sistemleri için dağıtılan SAN/NAS aygıtları ile aynı sınıfta değildi. Bununla birlikte, Azure Standart Depolama, bu arada Azure'da dağıtılan yüzlerce SAP sistemi için yeterli olduğunu kanıtladı.

Azure Standart Depolama Hesapları'nda depolanan diskler, depolanan gerçek verilere, depolama işlemlerinin hacmine, giden veri aktarımlarına ve seçilen artıklık seçeneğine göre ücretlendirilir. Birçok disk boyutu nda maksimum 1 TB oluşturulabilir, ancak bu boş kaldığı sürece hiçbir ücret yoktur. Daha sonra her biri 100 GB ile bir VHD doldurursanız, VHD ile oluşturulan var nominal boyutu için değil, 100GB depolamak için ücretlendirilir.

#### <a name="azure-premium-storage"></a><a name="ff5ad0f9-f7f4-4022-9102-af07aef3bc92"></a>Azure Premium Depolama
Azure Premium Depolama, aşağıdakileri sağlama hedefiyle tanıtıldı:

* Daha iyi G /O gecikmesi.
* Daha iyi iş artışı.
* G/Ç gecikmesinde daha az değişkenlik.

Bu amaçla, birçok değişiklik hangi iki en önemli tanıtıldı:

* Azure Depolama düğümlerinde SSD disklerinin kullanımı
* Azure işlem düğümünün yerel SSD'si tarafından desteklenen yeni bir okuma önbelleği

Yeteneklerin diskin (veya VHD)'nin boyutuna bağlı olarak değişmediği Standart depolamaalanının karşısında, Premium Depolama'nın şu anda bu makalede gösterilen üç farklı disk kategorisi vardır:<https://azure.microsoft.com/pricing/details/storage/unmanaged-disks/>

IOPS/disk ve disk iş bilgili/diskin disklerin boyut kategorisine bağlı olduğunu görüyorsunuz

Premium Depolama durumunda maliyet temeli, bu tür disklerde depolanan gerçek veri hacmi değil, diskiçinde depolanan veri miktarından bağımsız olarak, böyle bir diskin boyut kategorisidir.

Ayrıca, Premium Depolama Alanı'nda gösterilen boyut kategorilerine doğrudan eşlemeyen diskler de oluşturabilirsiniz. Bu durum, özellikle Diskleri Standart Depolama'dan Premium Depolama alanına kopyalarken olabilir. Bu gibi durumlarda, sonraki en büyük Premium Depolama disk için bir eşleme gerçekleştirilir.

SAP sertifikalı Azure VM ailelerinin çoğu Premium Depolama ile çalışabilir ve azure standardı ile Premium Depolama arasında bir karışım la çalışabilir.

[Bu makalede (Linux)][virtual-machines-sizes-linux] ve [bu makalede (Windows)][virtual-machines-sizes-windows]DS serisi VM'lerin bir bölümünü kontrol ediyorsanız, VM düzeyinin parçalı lık üzerinde Premium Depolama diskleri için veri hacmi sınırlamaları olduğunu fark edeyim. Farklı DS serisi veya GS serisi VM'ler de monte edilebilir veri disklerinin sayısında farklı sınırlamalar vardır. Bu sınırlar yukarıda belirtilen makalede de belgelenmiştir. Ama özünde, örneğin, tek bir DS14 VM 32 x P30 diskler monte eğer 32 x bir P30 disk maksimum verim ilerken olamaz anlamına gelir. Bunun yerine makalede belgelenen VM düzeyinde maksimum iş verimi veri veri elde ini sınırlar.

Premium Depolama hakkında daha fazla bilgiyi burada bulabilirsiniz:<https://azure.microsoft.com/blog/2015/04/16/azure-premium-storage-now-generally-available-2>

#### <a name="azure-storage-accounts"></a>Azure Depolama Hesapları

Azure'da hizmet veya VM dağıtılırken, Azure Depolama Hesapları adı verilen birimlerde VHD ve VM Görüntülerinin dağıtımı düzenlenebilir. Azure dağıtımı planlarken, Azure'un kısıtlamalarını dikkatlice göz önünde bulundurmanız gerekir. Bir tarafta, Azure aboneliği başına sınırlı sayıda Depolama Hesabı vardır. Her Azure Depolama Hesabı çok sayıda VHD dosyası tutabilse de, Depolama Hesabı başına toplam IOPS için sabit bir sınır vardır. Önemli IO çağrıları oluşturan Yüzlerce SAP VM'yi DBMS sistemleriyle dağıtırken, yüksek IOPS DBMS VM'lerinin birden çok Azure Depolama Hesabı arasında dağıtılması önerilir. Abonelik başına Geçerli Azure Depolama Hesapları sınırını aşmamaya özen gerekir. Depolama, bir SAP sistemi için veritabanı dağıtımının önemli bir parçası olduğundan, bu kavram zaten başvurulan [DBMS Dağıtım Kılavuzu'nda][dbms-guide]daha ayrıntılı olarak tartışılır.

Azure Depolama Hesapları hakkında daha fazla bilgiyi [standart depolama hesapları için Ölçeklenebilirlik hedeflerinde](../../../storage/common/scalability-targets-standard-account.md) ve premium sayfa [blob depolama hesapları için Ölçeklenebilirlik hedeflerinde](../../../storage/blobs/scalability-targets-premium-page-blobs.md)bulunabilir. Bu makaleleri okurken, Azure Standart Depolama Hesapları ile Premium Depolama Hesapları arasındaki sınırlamalar arasında farklılıklar olduğunu fark esiniz. Büyük farklar, söz konusu Depolama Hesabı'nda depolanabilecek veri hacmidir. Standart Depolama'da, hacim Premium Depolama'dan daha büyüktür. Diğer taraftan, Standart Depolama Hesabı IOPS'de ciddi şekilde sınırlıdır (bkz. sütun **Toplam İstek Oranı),** ancak Azure Premium Depolama Hesabı'nda böyle bir sınırlama yoktur. SAP sistemlerinin, özellikle de DBMS sunucularının dağıtımlarını tartışırken bu farklılıkların ayrıntılarını ve sonuçlarını tartışacağız.

Depolama Hesabı içinde, farklı VHD'leri düzenlemek ve kategorilere ayırmak amacıyla farklı kapsayıcılar oluşturma olanağınız vardır. Bu kapsayıcılar, örneğin, farklı VM'lerin ayrı VHD'leri için kullanılır. Tek bir Azure Depolama Hesabı'nın altında tek bir kapsayıcı veya birden çok kapsayıcı kullanmanın hiçbir performans sonucu yoktur.

Azure'da bir VHD adı, Azure içindeki VHD için benzersiz bir ad sağlaması gereken aşağıdaki adlandırma bağlantısını izler:

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

Yukarıdaki dize, Azure Depolama'da depolanan VHD'yi benzersiz olarak tanımlamalı.

#### <a name="managed-disks"></a><a name="c55b2c6e-3ca1-4476-be16-16c81927550f"></a>Yönetilen Diskler

Yönetilen Diskler, Azure Depolama Hesapları'nda depolanan VHD'ler yerine kullanılabilen Azure Kaynak Yöneticisi'nde yeni bir kaynak türüdür. Yönetilen Diskler, bağlı oldukları sanal makinenin kullanılabilirlik kümesiyle otomatik olarak hizalanır ve bu nedenle sanal makinenizin kullanılabilirliğini ve sanal makinede çalışan hizmetleri artırır. Daha fazla bilgi için [genel bakış makalesini](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview)okuyun.

Sanal makinelerinizin dağıtımını ve yönetimini basitleştirdikleri için Yönetilen diski kullanmanızı öneririz.
SAP şu anda yalnızca Premium Yönetilen Diskleri destekler. Daha fazla bilgi için SAP Note [1928533'ü]okuyun.

#### <a name="microsoft-azure-storage-resiliency"></a>Microsoft Azure Depolama esnekliği

Microsoft Azure Depolama, temel VHD'yi (işletim sistemi ile) ve ekli diskleri veya BLOB'ları en az üç ayrı depolama düğümünde depolar. Bu gerçeğe Yerel Yedekli Depolama (LRS) adı verilir. LRS, Azure'daki tüm depolama türleri için varsayılandır. 

Azure [Depolama çoğaltma](https://docs.microsoft.com/azure/storage/common/storage-redundancy?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)makalesinde açıklanan birkaç fazlalık yöntemi daha vardır.

> [!NOTE]
>DBMS VM'leri ve veritabanlarını depolayan ve dosyaları yeniden günlüğe kaydetme/yeniden oluşturma yı sağlayan diskler için önerilen depolama türü olan Azure Premium Depolama olarak, kullanılabilir tek yöntem LRS'dir. Sonuç olarak, veritabanı verilerinin başka bir Azure Bölgesi'nde veya başka bir Azure Kullanılabilirlik Bölgesi'nde çoğaltılmasını etkinleştirmek için SQL Server Always Open, Oracle Data Guard veya HANA System Replication gibi veritabanı yöntemlerini yapılandırmanız gerekir.


> [!NOTE]
> DBMS dağıtımları için, Azure Standart Depolama'da kullanılabilen Geo Redundant Depolama'nın kullanımı, ciddi performans etkisi olduğundan ve VM'ye bağlı farklı VHD'lerde yazma sırasını yerine getirmediği için önerilmez. Veritabanı ve günlük/redo dosyaları kaynak VM tarafında birden fazla VHD'ye (çoğunlukla olduğu gibi) yayılırsa, farklı VHD'ler arasında yazma sırasını onurlandırmamak, çoğaltma hedef tarafında tutarsız veritabanlarına son verme potansiyeli taşır.


### <a name="microsoft-azure-networking"></a><a name="61678387-8868-435d-9f8c-450b2424f5bd"></a>Microsoft Azure Ağ

Microsoft Azure, SAP yazılımıyla gerçekleştirmek istediğimiz tüm senaryoların eşlemesine olanak tanıyan bir ağ altyapısı sağlar. Yetenekleri şunlardır:

* Dışarıdan, doğrudan Windows Terminal Hizmetleri veya ssh/VNC üzerinden VM'lere erişim
* VM'ler içindeki uygulamalar tarafından kullanılan hizmetlere ve belirli bağlantı noktalarına erişim
* Azure VM olarak dağıtılan bir grup VM arasında Dahili İletişim ve Ad Çözümü
* Müşterinin şirket içi ağı yla Azure ağı arasında binalar arası bağlantı
* Azure Bölgesi'ni veya Azure siteleri arasında veri merkezi bağlantısı arasında çapraz

Buradan daha fazla bilgi bulabilirsiniz: <https://azure.microsoft.com/documentation/services/virtual-network/>

Azure'da ad ve IP çözünürlüğünü yapılandırmak için birçok farklı olasılık vardır. Ayrıca, kendi DNS sunucunuzu kurmak yerine kullanılabilecek bir Azure DNS hizmeti de vardır. Daha fazla bilgi [bu makalede][virtual-networks-manage-dns-in-vnet] ve [bu sayfada](https://azure.microsoft.com/services/dns/)bulunabilir.

Binalar arası veya karma senaryolar için, şirket içi AD/OpenLDAP/DNS'nin VPN veya Azure'a özel bağlantı yoluyla genişletilmiş olmasına güveniyoruz. Burada belgelenen belirli senaryolar için Azure'da bir AD/OpenLDAP yinelemesi yüklü olması gerekebilir.

Ağ ve ad çözümlemesi bir SAP sistemi için veritabanı dağıtımının önemli bir parçası olduğundan, bu kavram [DBMS Dağıtım Kılavuzu'nda][dbms-guide]daha ayrıntılı olarak ele alınmıştır.

##### <a name="azure-virtual-networks"></a>Azure Sanal Ağları

Bir Azure Sanal Ağı oluşturarak, Azure DHCP işlevselliği tarafından ayrılan özel IP adreslerinin adres aralığını tanımlayabilirsiniz. Binalar arası senaryolarda, tanımlanan IP adresi aralığı yine de Azure tarafından DHCP kullanılarak tahsis edilir. Ancak, Alan Adı çözümü şirket içinde yapılır (VM'lerin şirket içi bir etki alanının parçası olduğunu varsayarsak) ve bu nedenle farklı Azure Bulut Hizmetleri dışındaki adresleri çözebilir.

Azure'daki her Sanal Makinenin bir Sanal Ağa bağlanması gerekir.

Daha fazla bilgi [bu makalede][resource-groups-networking] ve [bu sayfada](https://azure.microsoft.com/documentation/services/virtual-network/)bulunabilir.


> [!NOTE]
> Varsayılan olarak, bir VM dağıtıldıktan sonra Sanal Ağ yapılandırmasını değiştiremezsiniz. TCP/IP ayarları Azure DHCP sunucusuna bırakılmalıdır. Varsayılan davranış Dinamik IP atamasıdır.
>
>

Sanal ağ kartının MAC adresi değişebilir, örneğin yeniden boyutlandıktan ve Windows veya Linux konuk işletim sistemi yeni ağ kartını aldıktan sonra ve bu durumda IP ve DNS adreslerini atamak için otomatik olarak DHCP'yi kullanır.

##### <a name="static-ip-assignment"></a>Statik IP Atama
Bir Azure Sanal Ağı'nda Sanal M'lere sabit veya ayrılmış IP adresleri atamak mümkündür. Sanal Sanal Ağ'da VM'lerin çalıştırılması, bazı senaryolar için gerektiğinde veya gerekirse bu işlevsellihattan yararlanmak için büyük bir olasılık açar. IP ataması, VM'nin çalışıp çalışmadığına veya kapanmasından bağımsız olarak VM'nin varlığı boyunca geçerli liğini korur. Sonuç olarak, Sanal Ağ için IP adresleri aralığını tanımlarken toplam VM sayısını (çalışan ve durduran VM'ler) hesaba katmanız gerekir. IP adresi, VM ve Ağ Arabirimi silinene veya IP adresi yeniden atanana kadar atanmadan atanmıştır. Daha fazla bilgi için [bu makaleyi][virtual-networks-static-private-ip-arm-pportal]okuyun.

> [!NOTE]
> Statik IP adreslerini Azure araçları yla tek tek vNIC'lere atamalısınız. Konuk işletim sistemi içindeki statik IP adreslerini bir vNIC'e atamamalısınız. Azure Yedekleme Hizmeti gibi bazı Azure hizmetleri, en azından birincil vNIC'nin statik IP adreslerine göre değil, DHCP olarak ayarlanmış olmasına dayanır. Ayrıca bkz. belge [Sorun Giderme Azure sanal makine](https://docs.microsoft.com/azure/backup/backup-azure-vms-troubleshoot#networking)yedeklemesi.
>
>

##### <a name="multiple-nics-per-vm"></a>VM başına birden fazla NIC

Azure Sanal Makine için birden çok sanal ağ arabirimi kartı (vNIC) tanımlayabilirsiniz. Birden çok vNIC'ye sahip olma özelliği sayesinde, örneğin istemci trafiğinin bir vNIC üzerinden yönlendirildiği ve arka uç trafiğinin ikinci bir vNIC üzerinden yönlendirildiği ağ trafiği ayrımını ayarlamaya başlayabilirsiniz. VM türüne bağlı olarak bir VM atanmış olabilir vNICs sayısı için farklı sınırlamalar vardır. Tam ayrıntılar, işlevsellik ve kısıtlamalar bu makalelerde bulunabilir:

* [Birden çok NIC içeren bir Windows VM oluşturma][virtual-networks-multiple-nics-windows]
* [Birden fazla NIC içeren bir Linux VM oluşturma][virtual-networks-multiple-nics-linux]
* [Şablon kullanarak çoklu NIC VM'leri dağıtma][virtual-network-deploy-multinic-arm-template]
* [PowerShell'i kullanarak çoklu NIC VM'leri dağıtma][virtual-network-deploy-multinic-arm-ps]
* [Azure CLI'yi kullanarak çoklu NIC VM'leri dağıtma][virtual-network-deploy-multinic-arm-cli]

#### <a name="site-to-site-connectivity"></a>Siteden Siteye Bağlantı

Çapraz tesisler, saydam ve kalıcı BIR VPN bağlantısıyla bağlantılı Azure VM'leri ve Şirket İçi'dir. Azure'daki en yaygın SAP dağıtım deseni olması bekleniyor. Varsayım, Azure'daki SAP örnekleriyle birlikte çalışma yordamlarının ve işlemlerinin saydam çalışması gerektiğidir. Bu, azure'daki bir geliştirme sistemindeki değişiklikleri şirket içinde dağıtılan bir test sistemine taşımak için sap Aktarım Yönetim Sistemi'ni (TMS) kullanmanın yanı sıra bu sistemlerden çıktı alabileceğiniz anlamına gelir. Siteden siteye daha fazla belge [bu makalede][vpn-gateway-create-site-to-site-rm-powershell] bulunabilir

##### <a name="vpn-tunnel-device"></a>VPN Tünel Cihazı

Siteden siteye bağlantı oluşturmak için (Azure veri merkezine şirket içi veri merkezi), bir VPN aygıtı edinmeniz ve yapılandırmanız veya Windows Server 2012 ile yazılım bileşeni olarak tanıtılan Yönlendirme ve Uzaktan Erişim Hizmeti'ni (RRAS) kullanmanız gerekir.

* [PowerShell'i kullanarak siteden siteye VPN bağlantısı olan sanal ağ oluşturma][vpn-gateway-create-site-to-site-rm-powershell]
* [Siteden Siteye VPN Gateway bağlantıları için VPN cihazları hakkında][vpn-gateway-about-vpn-devices]
* [VPN Gateway SSS][vpn-gateway-vpn-faq]

![Şirket içi ve Azure arasında siteden siteye bağlantı][planning-guide-figure-600]

Yukarıdaki Şekil, iki Azure aboneliğinin Azure'daki Sanal Ağlar'da kullanılmak için ayrılmış IP adresi alt aralıklarına sahip olduğunu gösterir. Şirket içi ağdan Azure'a bağlantı VPN üzerinden kurulur.

#### <a name="point-to-site-vpn"></a>Noktadan Siteye VPN

Noktadan siteye VPN, her istemci makinenin azure'a kendi VPN'i ile bağlanmasını gerektirir. SAP senaryoları için, baktığımız noktadan siteye bağlantı pratik değildir. Bu nedenle, daha fazla referans lar noktaya-to-site VPN bağlantısı verilir.

Daha fazla bilgiyi burada bulabilirsiniz
* [Azure portal’ı kullanarak bir sanal ağa yönelik Noktadan Siteye bağlantı yapılandırma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)
* [PowerShell'i kullanarak bir sanal ağa yönelik bir Noktadan Siteye bağlantı yapılandırma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps)

#### <a name="multi-site-vpn"></a>Çok Siteli VPN

Azure, günümüzde tek bir Azure aboneliği için Çok Siteli VPN bağlantısı oluşturma olanağı da sunmaktadır. Daha önce tek bir abonelik siteden siteye VPN bağlantısıyla sınırlıydı. Bu sınırlama, tek bir abonelik için Çok Siteli VPN bağlantıları ile gitti. Bu, tesisler arası yapılandırmalar aracılığıyla belirli bir abonelik için birden fazla Azure Bölgesi'nden yararlanmayı mümkün kılar.

Daha fazla belge için [bu makaleye][vpn-gateway-create-site-to-site-rm-powershell] bakın

#### <a name="vnet-to-vnet-connection"></a>VNet - VNet Bağlantısı

Çok Siteli VPN'i kullanarak, her bölgede ayrı bir Azure Sanal Ağı yapılandırmanız gerekir. Ancak, genellikle farklı bölgelerdeki yazılım bileşenlerinin birbirleriyle iletişim kurması gereksinimine sahipsiniz. İdeal olarak, bu iletişim bir Azure Bölgesinden şirket içi ve oradan da diğer Azure Bölgesi'ne yönlendirilmemelidir. Azure, kısayol için bir bölgedeki bir Azure Sanal Ağı'ndan başka bir bölgede barındırılan başka bir Azure Sanal Ağına bağlantı yapılandırma olanağı sunar. Bu işlevsellik VNet-to-VNet bağlantısı olarak adlandırılır. Bu işlevsellik hakkında daha fazla <https://azure.microsoft.com/documentation/articles/vpn-gateway-vnet-vnet-rm-ps/>bilgi burada bulabilirsiniz: .

#### <a name="private-connection-to-azure-expressroute"></a>Azure ExpressRoute'a Özel Bağlantı

Microsoft Azure ExpressRoute, Azure veri merkezleri ile müşterinin şirket içi altyapısı veya ortak konum ortamında özel bağlantılar oluşturulmasına olanak tanır. ExpressRoute çeşitli MPLS (paket anahtarlı) VPN sağlayıcıları veya diğer Ağ Servis Sağlayıcıları tarafından sunulmaktadır. ExpressRoute bağlantıları ortak İnternet üzerinden geçmemektedir. ExpressRoute bağlantıları, birden çok paralel devre, daha hızlı hızlar ve Internet üzerindeki tipik bağlantılara göre daha düşük gecikme süreleri ile daha yüksek güvenlik, daha fazla güvenilirlik sunar.

Azure ExpressRoute ve teklifleri hakkında daha fazla ayrıntıyı burada bulabilirsiniz:

* <https://azure.microsoft.com/documentation/services/expressroute/>
* <https://azure.microsoft.com/pricing/details/expressroute/>
* <https://azure.microsoft.com/documentation/articles/expressroute-faqs/>

Express Route, burada belgelenen tek bir ExpressRoute devresi üzerinden birden fazla Azure aboneliği sağlar

* <https://azure.microsoft.com/documentation/articles/expressroute-howto-linkvnet-arm/>
* <https://azure.microsoft.com/documentation/articles/expressroute-howto-circuit-arm/>

#### <a name="forced-tunneling-in-case-of-cross-premises"></a>Zorunlu tünel ler, tesisler arası
Siteden siteye, noktadan siteye veya ExpressRoute aracılığıyla şirket içi etki alanlarını birleştiren VM'ler için, Internet proxy ayarlarının bu VM'lerdeki tüm kullanıcılar için de dağıtıldıklarından emin olmanız gerekir. Varsayılan olarak, bu VM'lerde veya internete erişmek için tarayıcı kullanan kullanıcılarda çalışan yazılımlar şirket proxy'si üzerinden gitmez, ancak Azure üzerinden doğrudan Internet'e bağlanır. Ama hatta proxy ayarı yazılım ve hizmetlerin sorumluluğu proxy için kontrol etmek için olduğundan şirket proxy üzerinden trafik yönlendirmek için% 100 çözüm değildir. VM'de çalışan yazılım bunu yapmıyorsa veya bir yönetici ayarları manipüle ediyorsa, Internet trafiği doğrudan Azure üzerinden Internet'e yönlendirilebilir.

Böyle doğrudan bir internet bağlantısını önlemek için, Zorunlu Tünel'i şirket içi ve Azure arasında siteden siteye bağlantıyla yapılandırabilirsiniz. Zorunlu Tünel özelliğinin ayrıntılı açıklaması burada yayınlanmıştır<https://azure.microsoft.com/documentation/articles/vpn-gateway-forced-tunneling-rm/>

ExpressRoute ile Zorunlu Tünelleme, ExpressRoute BGP eşleme oturumları aracılığıyla varsayılan bir rotanın reklamını yaparak etkinleştirilir.

#### <a name="summary-of-azure-networking"></a>Azure Ağ Özeti

Bu bölümde Azure Ağı hakkında birçok önemli nokta yer almıştır. Burada ana noktaların bir özeti:

* Azure Sanal Ağlar, Azure dağıtımınıza bir ağ yapısı koymanızı sağlar. VNet'ler birbirine karşı izole edilebilir veya VNets arasındaki Ağ Güvenlik Grupları trafiği yardımıyla kontrol edilebilir.
* Azure Sanal Ağlar, IP adresi aralıklarını VM'lere atamak veya Sanal M'lere sabit IP adresleri atamak için kullanılabilir
* Site-To-Site veya Site'ye Bağlantı bağlantısı kurmak için önce bir Azure Sanal Ağı oluşturmanız gerekir
* Sanal bir makine dağıtıldıktan sonra, VM'ye atanan Sanal Ağı değiştirmek artık mümkün değildir

### <a name="quotas-in-azure-virtual-machine-services"></a>Azure Sanal Makine Hizmetlerinde Kotalar
Depolama ve ağ altyapısının Azure altyapısında çeşitli hizmetler çalıştıran VM'ler arasında paylaşılması konusunda açık olmamız gerekir. Ve tıpkı müşterinin kendi veri merkezlerinde olduğu gibi, bazı altyapı kaynaklarının aşırı sağlanması bir dereceye kadar gerçekleşir. Microsoft Azure Platformu, kaynak tüketimini sınırlamak ve tutarlı ve deterministik performansı korumak için disk, CPU, ağ ve diğer kotaları kullanır.  Farklı VM türleri (A5, A6, vb) disk, CPU, RAM ve Ağ sayısı için farklı kotalara sahiptir.

> [!NOTE]
> SAP tarafından desteklenen VM türlerinin CPU ve bellek kaynakları ana bilgisayar düğümlerine önceden tahsis edilir. Bu, VM dağıtıldıktan sonra, ana bilgisayardaki kaynakların VM türütarafından tanımlandığı şekilde kullanılabilir olduğu anlamına gelir.
>
>

Azure çözümlerinde SAP'yi planlarken ve boyutlandırırken, her sanal makine boyutuna yönelik kotalar göz önünde bulundurulmalıdır. VM kotaları [burada (Linux)][virtual-machines-sizes-linux] ve [burada (Windows)][virtual-machines-sizes-windows]açıklanmıştır.

Açıklanan kotalar teorik maksimum değerleri temsil eder.  Disk başına IOPS sınırı küçük IOs (8kb) ile elde edilebilir, ancak büyük iOs (1Mb) ile elde edilemez.  IOPS sınırı tek diskin tanecikliliğine uygulanır.

Sap sisteminin Azure Sanal Makine Hizmetleri'ne ve yeteneklerine uygun olup olmadığına veya varolan bir sistemin Sistemi Azure'da dağıtmak için farklı şekilde yapılandırılması gerekip gerekmediğine karar vermek için kaba bir karar ağacı olarak aşağıdaki karar ağacı kullanılabilir:

![SAP'yi Azure'da dağıtma ya da karar verme özelliği ne karar verecek][planning-guide-figure-700]

**Adım 1**: Başlamak için en önemli bilgi, belirli bir SAP sistemi için SAPS gereksinimidir. SAP sistemi 2 katmanlı bir yapılandırmada şirket içinde dağıtılmış olsa bile, SAPS gereksinimlerinin DBMS bölümüne ve SAP uygulama kısmına ayrılması gerekir. Varolan sistemler için, kullanılan donanımla ilgili SAPS genellikle varolan SAP kriterlerine göre belirlenebilir veya tahmin edilebilir. Sonuçlar burada bulunabilir: <https://sap.com/about/benchmark.html>.
Yeni dağıtılan SAP sistemleri için, sistemin SAPS gereksinimlerini belirlemesi gereken bir boyutlandırma alıştırması yapmış olmalısınız.
Azure'da SAP boyutlandırması için bu bloga ve eklenmiş belgeye bakın:<https://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

**Adım 2**: Mevcut sistemleriçin DBMS sunucusundaki saniyede G/Ç hacmi ve G/Ç işlemleri ölçülmelidir. Yeni planlanan sistemler için, yeni sistem için boyutlandırma egzersizi de DBMS tarafında G/Ç gereksinimleri kaba fikirler vermelidir. Emin değilseniz, sonunda kavram bir Kanıt yürütmek gerekir.

**Adım 3**: DBMS sunucusu için SAPS gereksinimini Azure'un sağlayabileceği farklı VM türlerinin sağladığı SAPS ile karşılaştırın. Farklı Azure VM türlerinin SAPS'si hakkındaki bilgiler SAP Note [1928533'te]belgelenmiştir. Veritabanı katmanı dağıtımların çoğunda ölçeklendirmez bir SAP NetWeaver sisteminde katman olduğundan odak dbms VM ilk olmalıdır. Buna karşılık, SAP uygulama katmanı ölçeklenebilir. SAP desteklenen Azure VM türlerinden hiçbiri gerekli SAPS'yi teslim edemezse, planlanan SAP sisteminin iş yükü Azure'da çalıştırılamaz. Sistemi şirket içinde dağıtmanız veya sistemin iş yükü hacmini değiştirmeniz gerekir.

**Adım 4**: [Burada (Linux)][virtual-machines-sizes-linux] ve [burada (Windows)][virtual-machines-sizes-windows]belgelenen Azure, Standart Depolama veya Premium Depolama'yı kullanıp kullanmadığınızdan bağımsız olarak disk başına bir IOPS kotası uygular. VM türüne bağlı olarak monte edilebilen veri disklerinin sayısı değişir. Sonuç olarak, farklı VM türlerinin her biri ile elde edilebilir bir maksimum IOPS numarası hesaplayabilirsiniz. Veritabanı dosya düzenine bağlı olarak, diskleri konuk işletim sistemi içinde tek bir birim olacak şekilde çizgili yapabilirsiniz. Ancak, dağıtılan bir SAP sisteminin geçerli IOPS hacmi en büyük VM azure türünün hesaplanan sınırlarını aşarsa ve daha fazla bellekle telafi etme şansı yoksa, SAP sisteminin iş yükü ciddi şekilde etkilenebilir. Bu gibi durumlarda, sistemi Azure'da dağıtmamanız gereken bir noktaya ulaşabilirsiniz.

**Adım 5**: Özellikle 2 Katmanlı yapılandırmalarda şirket içinde dağıtılan SAP sistemlerinde, sistemin Azure'da 3 Katmanlı yapılandırmada yapılandırılması gerekebilir. Bu adımda, SAP uygulama katmanında ölçeklendirilemeyecek ve farklı Azure VM türlerinin sunduğu CPU ve bellek kaynaklarına sığmayan bir bileşen olup olmadığını denetlemeniz gerekir. Böyle bir bileşen gerçekten varsa, SAP sistemi ve iş yükü Azure'a dağıtılamaz. Ancak SAP uygulama bileşenlerini birden çok Azure Sanal'ına ölçeklendirebilirseniz, sistem Azure'a dağıtılabilir.

**Adım 6**: DBMS ve SAP uygulama katmanı bileşenleri Azure VM'lerde çalıştırılabiliyorsa, yapılandırmanın aşağıdakilerle ilgili olarak tanımlanması gerekir:

* Azure VM sayısı
* Tek tek bileşenler için VM türleri
* Yeterli IOPS sağlamak için DBMS VM'deki VHD sayısı

## <a name="managing-azure-assets"></a>Azure Varlıklarını Yönetme

### <a name="azure-portal"></a>Azure portalında

Azure portalı, Azure VM dağıtımlarını yöneten üç arabirimden biridir. Görüntülerden VM'ler dağıtmak gibi temel yönetim görevleri Azure portalı üzerinden yapılabilir. Ayrıca, Depolama Hesapları, Sanal Ağlar ve diğer Azure bileşenlerinin oluşturulması da Azure portalının iyi işleyebilir görevleridir. Ancak, şirket içinden Azure'a VHD yükleme veya Azure'da bir VHD kopyalama gibi işlevler, PowerShell veya CLI aracılığıyla üçüncü taraf araçları veya yönetim gerektiren görevlerdir.

![Microsoft Azure portalı - Virtual Machine'e genel bakış][planning-guide-figure-800]


Sanal Makine örneği için yönetim ve yapılandırma görevleri Azure portalı içinden mümkündür.

Sanal Makine'yi yeniden başlatmanın ve kapatmanın yanı sıra görüntü hazırlama örneğini yakalamak ve Sanal Makine örneğinin boyutunu yapılandırmak için Sanal Makine örneği için veri diskleri ekleyebilir, ayırabilir ve oluşturabilirsiniz.

Azure portalı, VM'leri ve diğer birçok Azure hizmetlerini dağıtmak ve yapılandırmak için temel işlevsellik sağlar. Ancak kullanılabilir tüm işlevler Azure portalı tarafından karşılanmaz. Azure portalında aşağıdaki gibi görevleri gerçekleştirmek mümkün değildir:

* Azure'a VHD yükleme
* VM'leri Kopyalama


### <a name="management-via-microsoft-azure-powershell-cmdlets"></a>Microsoft Azure PowerShell cmdlets ile yönetim

Windows PowerShell, Azure'da daha fazla sayıda sistem dağıtan müşteriler tarafından yaygın olarak benimsenen güçlü ve genişletilebilir bir çerçevedir. PowerShell cmdlets'in masaüstü, dizüstü bilgisayar veya özel yönetim istasyonuna kurulmasından sonra PowerShell cmdlets'leri uzaktan çalıştırılabilir.

Azure PowerShell cmdlets kullanımı için yerel bir masaüstü/ dizüstü bilgisayar etkinleştirme işlemi ve Azure aboneliği (ler) ile kullanım için bunları yapılandırmak için [nasıl bu makalede][powershell-install-configure]açıklanmıştır.

Azure PowerShell cmdlets'in nasıl yüklenir, güncellenir ve yapılandırılıncaya ilişkin daha ayrıntılı adımlar da [Dağıtım Kılavuzu'nun bu bölümünde][deployment-guide-4.1]bulunabilir.

Müşteri deneyimi şimdiye kadar PowerShell (PS) kesinlikle vms dağıtmak ve VMs dağıtımında özel adımlar oluşturmak için daha güçlü bir araç olmuştur. Azure'da SAP örnekleri çalıştıran tüm müşteriler, Azure portalında yaptıkları yönetim görevlerini tamamlamak için PS cmdlet'leri kullanır ve hatta yalnızca Azure'daki dağıtımlarını yönetmek için PS cmdlet'leri kullanır. Azure'a özel cmdlets, Windows ile ilgili 2000'den fazla cmdlet ile aynı adlandırma kuralını paylaştığından, Windows yöneticilerinin bu cmdlets'ten yararlanması kolay bir iştir.

Burada örnek bakın:<https://blogs.technet.com/b/keithmayer/archive/2015/07/07/18-steps-for-end-to-end-iaas-provisioning-in-the-cloud-with-azure-resource-manager-arm-powershell-and-desired-state-configuration-dsc.aspx>


SAP için Azure Uzantısı'nın dağıtımı (bu belgedeki [SAP için][planning-guide-9.1] Azure Uzantısı bölümüne bakın) yalnızca PowerShell veya CLI üzerinden mümkündür. Bu nedenle, Azure'da bir SAP NetWeaver sistemini dağıtırken veya yönetirken PowerShell veya CLI'yi kurmak ve yapılandırmak zorunludur.  

Azure daha fazla işlevsellik sağladığından, cmdletlerin güncellenir yeni PS cmdletleri eklenecektir. Bu nedenle, cmdlets'in yeni bir <https://azure.microsoft.com/downloads/> sürümü için ayda en az bir kez Azure İndirme sitesini kontrol etmek mantıklıdır. Yeni sürüm eski sürümün üstüne yüklenir.

Azure ile ilgili PowerShell komutlarının genel listesi <https://docs.microsoft.com/powershell/azure/overview>için buraya bakın: .

### <a name="management-via-microsoft-azure-cli-commands"></a>Microsoft Azure CLI komutları ile yönetim

Linux kullanan ve Azure kaynaklarını yönetmek isteyen müşteriler için PowerShell bir seçenek olmayabilir. Microsoft, azure CLI'yi alternatif olarak sunar.
Azure CLI, Azure Platformu ile çalışmak için bir dizi açık kaynak, çapraz platform komutu sağlar. Azure CLI, Azure portalında bulunan işlevselliğin çoğunu sağlar.

Yükleme, yapılandırma ve Azure görevlerini gerçekleştirmek için CLI komutlarının nasıl kullanılacağı hakkında bilgi için bkz.

* [Azure klasik CLI'yi yükleme][xplat-cli]
* [Azure Kaynak Yöneticisi şablonlarını ve Azure CLI şablonlarını kullanarak sanal makineleri dağıtın ve yönetin] [.. /.. /linux/create-ssh-secureed-vm-from-template.md]
* [Azure Kaynak Yöneticisi ile Mac, Linux ve Windows için Azure klasik CLI'yi kullanma][xplat-cli-azure-resource-manager]

Ayrıca, [Dağıtım Kılavuzu'nda][planning-guide] [Linux VM'leri için][deployment-guide-4.5.2] Azure CLI bölümünü sap için Azure Uzantısı'nı dağıtmak için Azure CLI'nin nasıl kullanılacağını da okuyun.


## <a name="first-steps-planning-a-deployment"></a>Dağıtım planlamanın ilk adımları
Dağıtım planlamasındaki ilk adım, SAP'yi çalıştırmak için kullanılabilen VM'leri denetlemek değildir. İlk adım zaman alan bir adım olabilir, ancak en önemlisi, şirketinizdeki uyumluluk ve güvenlik ekipleriyle, hangi SAP iş yükünü veya iş sürecini genel buluta dağıtmak için sınır koşullarının ne olduğu konusunda çalışmaktır. Şirketiniz azure'a daha önce başka yazılımlar dağıttıysa, işlem kolay olabilir. Şirketiniz yolculuğun başında daha fazlaysa, belirli SAP verilerinin ve SAP iş süreçlerinin genel bulutta barındırılmasına olanak tanıyan sınır koşullarını, güvenlik koşullarını belirlemek için daha büyük tartışmalar gerekebilir.

Yararlı bir yardım olarak, Microsoft'un sağlayabileceği uyumluluk teklifleri listesi için [Microsoft uyumluluk tekliflerine](https://docs.microsoft.com/microsoft-365/compliance/offering-home) işaret edebilirsiniz. 

Azure hizmetinde veri şifreleme si veya Azure hizmetindeki diğer şifreleme gibi diğer endişe alanları [Azure şifrelemeye genel bakış](https://docs.microsoft.com/azure/security/fundamentals/encryption-overview)olarak belgelenmiştir.

Planlamanızda projenin bu aşamasını hafife almayın. Yalnızca bu konu yla ilgili anlaşmanız ve kurallarınız olduğunda, Azure'da dağıttığınız ağ mimarisinin planlaması olan bir sonraki adıma gitmeniz gerekir.


## <a name="different-ways-to-deploy-vms-for-sap-in-azure"></a>Azure'da SAP için VM dağıtmanın farklı yolları

Bu bölümde, Azure'da vm dağıtmanın farklı yollarını öğrenirsiniz. Ek hazırlık yordamları ve Azure'daki VHD'lerin ve VM'lerin kullanımı bu bölümde ele alınmıştır.

### <a name="deployment-of-vms-for-sap"></a>SAP için VM dağıtımı

Microsoft Azure, VM'leri ve ilişkili diskleri dağıtmak için birden çok yol sunar. Bu nedenle, VM'lerin preparatları dağıtım yöntemine bağlı olarak farklılık gösterebilir. Genel olarak, aşağıdaki senaryolara bir göz atalım:

#### <a name="moving-a-vm-from-on-premises-to-azure-with-a-non-generalized-disk"></a><a name="4d175f1b-7353-4137-9d2f-817683c26e53"></a>Genelleştirilmeyen bir diskle bir VM'yi şirket içinde Azure'a taşıma

Belirli bir SAP sistemini şirket içinde Azure'a taşımayı planlıyorsunuz. Bu, Işletim Sistemi, SAP İkilileri ve DBMS ikilileri ile DBMS'nin veri ve günlük dosyalarını içeren VHD'yi Azure'a yükleyerek yapılabilir. [Aşağıdaki senaryo #2][planning-guide-5.1.2]aksine, ana bilgisayar adını, SAP SID'yi ve SAP kullanıcı hesaplarını şirket içi ortamda yapılandırırken Azure VM'de saklarsınız. Bu nedenle, görüntüyü genelleme gerekli değildir. Şirket içi hazırlık adımları ve genelleştirilmiş VM'lerin veya VH'lerin Azure'a yüklenmesi için bu belgenin [genellenmemiş bir diskiyle bir VM'yi şirket içinde Azure'a taşıma][planning-guide-5.2.1] ya da yükleme bölüm lerine bakın. Bölüm Senaryo 3'ü okuyun: Azure'da böyle bir görüntüyü dağıtmanın ayrıntılı adımları için [Dağıtım Kılavuzu'nda][deployment-guide] [SAP ile genelleştirilmeyen bir Azure VHD kullanarak bir VM'yi şirket içinde taşıma.][deployment-guide-3.4]

#### <a name="deploying-a-vm-with-a-customer-specific-image"></a><a name="e18f7839-c0e2-4385-b1e6-4538453a285c"></a>Müşteriye özel bir görüntüyle VM dağıtma

İşletim sistemi veya DBMS sürümünüzün belirli yama gereksinimleri nedeniyle, Azure Marketi'nde sağlanan görüntüler gereksinimlerinize uymayabilir. Bu nedenle, kendi özel Işletim Sistemi/DBMS VM görüntünüz kullanılarak bir VM oluşturmanız gerekebilir ve bu görüntü daha sonra birkaç kez dağıtılabilir. Çoğaltma için böyle özel bir görüntü hazırlamak için aşağıdaki öğelerin dikkate alınması gerekir:

---
> ![Windows][Logo_Windows] Windows
>
> Daha fazla ayrıntıya bakın: <https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed> Windows ayarları (Windows SID ve ana bilgisayar adı gibi) sysprep komutu aracılığıyla şirket içi VM'de soyutlanmalıdır/genelleştirilmelidir.
>
>
> ![Linux][Logo_Linux] Linux
>
> Azure'a yüklenecek bir VHD hazırlamak için [SUSE,][virtual-machines-linux-create-upload-vhd-suse] [Red Hat][virtual-machines-linux-redhat-create-upload-vhd]veya [Oracle Linux][virtual-machines-linux-create-upload-vhd-oracle]için bu makalelerde açıklanan adımları izleyin.
>
>

---
Şirket içi VM'nize SAP içeriğini zaten yüklediyseniz (özellikle 2 Katmanlı sistemler için), Azure VM'nin dağıtımından sonra SAP Sistem Ayarları'nı SAP Yazılım Sağlama Yöneticisi (SAP Note [1619720)]tarafından desteklenen örnek yeniden adlandırma yordamı aracılığıyla uyarlayabilirsiniz. Bölümlere bakın [SAP için müşteriye özel bir görüntüyle vm dağıtma][planning-guide-5.2.2] ya da şirket içi hazırlık adımları ve genelleştirilmiş bir VM'yi Azure'a yüklemek için şirket içi bu belgenin bir [VHD'sini Azure'a yükleme.][planning-guide-5.3.2] Bölüm [Senaryo 2:Böyle bir görüntüyü][deployment-guide-3.3] Azure'da dağıtmanın ayrıntılı adımları için [Dağıtım Kılavuzu'nda][deployment-guide] SAP için özel bir görüntüiçeren bir VM dağıtma.

#### <a name="deploying-a-vm-out-of-the-azure-marketplace"></a>Azure Marketi'nden Bir VM dağıtma

VM'nizi dağıtmak için Azure Marketi'nden Microsoft veya üçüncü taraf tarafından sağlanan bir VM görüntüsünü kullanmak istiyorsunuz. VM'nizi Azure'da dağıttıktan sonra, SAP yazılımını ve/veya DBMS'yi şirket içi ortamda yaptığınız gibi VM'nize yüklemek için aynı yönergeleri ve araçları izlersiniz. Daha ayrıntılı dağıtım açıklaması [Scenario 1: Deploying a VM out of the Azure Marketplace for SAP][deployment-guide-3.2] için bkz. [Deployment Guide][deployment-guide]

### <a name="preparing-vms-with-sap-for-azure"></a><a name="6ffb9f41-a292-40bf-9e70-8204448559e7"></a>Azure için SAP ile VM'ler hazırlama

Azure'a VM yüklemeden önce, VM'lerin ve VHD'lerin belirli gereksinimleri yerine getirdiğinden emin olmanız gerekir. Kullanılan dağıtım yöntemine bağlı olarak küçük farklılıklar vardır.

#### <a name="preparation-for-moving-a-vm-from-on-premises-to-azure-with-a-non-generalized-disk"></a><a name="1b287330-944b-495d-9ea7-94b83aff73ef"></a>Genelleştirilmeyen bir diskle bir VM'yi şirket içinde Azure'a taşımaya hazırlık

Yaygın bir dağıtım yöntemi, sap sistemini şirket içinde çalıştıran varolan bir VM'yi şirket içinde Azure'a taşımaktır. VM'deki VM ve SAP sistemi, aynı ana bilgisayar adını ve büyük olasılıkla aynı SAP SID'yi kullanarak Azure'da çalışmalıdır. Bu durumda, VM'nin konuk işletim sistemi birden çok dağıtım için genelleştirilmemelidir. Şirket içi ağ Azure'a genişletildiyse, aynı etki alanı hesapları bile şirket içinde daha önce kullanılanlarla VM içinde kullanılabilir.

Kendi Azure VM Diskinizi hazırlarken gerekenler şunlardır:

* Başlangıçta işletim sistemi içeren VHD sadece 127GB maksimum boyutu olabilir. Bu sınırlama Mart 2015 sonunda ortadan kaldırıldı. Artık işletim sistemini içeren VHD, diğer Azure Depolama'nın vhd barındırılan boyutu nda 1 TB'a kadar olabilir.
* Sabit VHD formatında olması gerekir. VHDx formatındaki dinamik VHD'ler veya VHD'ler henüz Azure'da desteklenmedi. PowerShell komut ları veya CLI ile VHD yüklediğinizde dinamik VHD'ler statik VHD'lere dönüştürülür
* VM'ye monte edilen ve VM'ye azure'a yeniden monte edilmesi gereken VHD'lerin de sabit bir VHD formatında olması gerekir. Veri disklerinin boyut sınırları için [bu makaleyi (Linux)](../../linux/managed-disks-overview.md) ve [bu makaleyi (Windows)](../../windows/managed-disks-overview.md)okuyun. PowerShell komut ları veya CLI ile VHD yüklediğinizde dinamik VHD'ler statik VHD'lere dönüştürülür
* Microsoft desteği tarafından kullanılabilen veya VM dağıtılana ve daha uygun kullanıcılar kullanılabilene kadar çalışacak hizmetler ve uygulamalar için bağlam olarak atanabilen yönetici ayrıcalıklarına sahip başka bir yerel hesap ekleyin.
* Belirli dağıtım senaryosu için gerekli olabilecekdiğer yerel hesapları ekleyin.

---
> ![Windows][Logo_Windows] Windows
>
> Bu senaryoda VM'nin Azure'a yüklenmesi ve dağıtılması için VM'nin genellemesi (sysprep) gerekmez.
> Sürücü D:\ olduğundan emin olun kullanılmaz.
> Bu belgede ekli [diskler için otomatik başlatma][planning-guide-5.5.3] yı bölüm olarak açıklandığı gibi ekli diskler için disk otomatik montajını ayarlayın.
>
> ![Linux][Logo_Linux] Linux
>
> Bu senaryoda VM'yi Azure'a yüklemek ve dağıtmak için VM'nin genellemesi (waagent-deprovision) gerekmez.
> /mnt/resource'ın kullanılmadığından ve TÜM disklerin uuid üzerinden monte edilmiş olduğundan emin olun. Os diski için, bootloader girişinin uuid tabanlı bineci de yansıttığından emin olun.
>
>

---
#### <a name="preparation-for-deploying-a-vm-with-a-customer-specific-image-for-sap"></a><a name="57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3"></a>SAP için müşteriye özel bir görüntüyle VM dağıtmaya hazırlık

Genelleştirilmiş işletim sistemi içeren VHD dosyaları Azure Depolama Hesapları'ndaki kapsayıcılarda veya Yönetilen Disk görüntüleri olarak depolanır. Bölüm Senaryo 2'de açıklandığı gibi, VHD veya Yönetilen Disk görüntüsünü dağıtım şablondosyalarınızda kaynak olarak göstererek böyle bir görüntüden yeni bir VM dağıtabilirsiniz: [Dağıtım Kılavuzu'nun][deployment-guide]SAP'si [için özel bir görüntüyle bir VM dağıtma.][deployment-guide-3.3]

Kendi Azure VM Resminizi hazırlarken gerekenler şunlardır:

* Başlangıçta işletim sistemi içeren VHD sadece 127GB maksimum boyutu olabilir. Bu sınırlama Mart 2015 sonunda ortadan kaldırıldı. Artık işletim sistemini içeren VHD, diğer Azure Depolama'nın vhd barındırılan boyutu nda 1 TB'a kadar olabilir.
* Sabit VHD formatında olması gerekir. VHDx formatındaki dinamik VHD'ler veya VHD'ler henüz Azure'da desteklenmedi. PowerShell komut ları veya CLI ile VHD yüklediğinizde dinamik VHD'ler statik VHD'lere dönüştürülür
* VM'ye monte edilen ve VM'ye azure'a yeniden monte edilmesi gereken VHD'lerin de sabit bir VHD formatında olması gerekir. Veri disklerinin boyut sınırları için lütfen [bu makaleyi (Linux)](../../windows/managed-disks-overview.md) ve [bu makaleyi (Windows)](../../linux/managed-disks-overview.md) okuyun. PowerShell komut ları veya CLI ile VHD yüklediğinizde dinamik VHD'ler statik VHD'lere dönüştürülür
* Belirli dağıtım senaryosu için gerekli olabilecekdiğer yerel hesapları ekleyin.
* Görüntü SAP NetWeaver'ın bir yüklemesini içeriyorsa ve azure dağıtımı noktasında ki ana bilgisayar adının orijinal adından yeniden adlandırma olasılığı yüksekse, SAP Yazılım Sağlama Yöneticisi DVD'sinin en son sürümlerini şablona kopyalamak önerilir. Bu, değiştirilen ana bilgisayar adını uyarlamak ve/veya yeni bir kopya başlatılır başlatılır başlamaz dağıtılan VM görüntüsündeki SAP sisteminin SID'sini değiştirmek için SAP sağlanan yeniden adlandırma işlevini kolayca kullanmanıza olanak tanır.

---
> ![Windows][Logo_Windows] Windows
>
> Sürücü D:\ olduğundan emin olun bu belgede [ekli diskler için][planning-guide-5.5.3] otomatik montaj ayarlama bölümünde açıklandığı gibi ekli diskler için set disk otomatik montaj kullanılmaz.
>
> ![Linux][Logo_Linux] Linux
>
> /mnt/resource'ın kullanılmadığından ve TÜM disklerin uuid üzerinden monte edilmiş olduğundan emin olun. Os diski için bootloader girişinin uuid tabanlı bineci de yansıttığından emin olun.
>
>

---
* SAP GUI (yönetim ve kurulum amacıyla) böyle bir şablona önceden yüklenebilir.
* Bu yazılım VM'nin yeniden adı ile çalışabildiği sürece, vm'leri binalar arası senaryolarda başarılı bir şekilde çalıştırmak için gereken diğer yazılımlar yüklenebilir.

VM genel olacak kadar hazırlanırsa ve hedeflenen Azure dağıtım senaryosunda bulunmayan hesaplardan/kullanıcılardan bağımsız olarak, böyle bir görüntüyü genellemenin son hazırlık adımı gerçekleştirilir.

##### <a name="generalizing-a-vm"></a>VM genelleme
---
> ![Windows][Logo_Windows] Windows
>
> Son adım, Yönetici hesabı olan bir VM'de oturum açmadır. Windows komut pencereni *yönetici*olarak açın. %windir%\windows\system32\sysprep'e gidin ve sysprep.exe'yi çalıştırın.
> Küçük bir pencere görüntülenir. **Genelleme** seçeneğini (varsayılan işaretlenmemiştir) denetlemek ve Kapatma Seçeneğini 'Yeniden Başlatma' varsayılanından 'kapatma' olarak değiştirmek önemlidir. Bu yordam, sysprep işleminin bir VM'nin Konuk İşletim Sistemi'nde şirket içinde yürütüldünü varsayar.
> Yordamı Azure'da zaten çalışan bir VM ile gerçekleştirmek istiyorsanız, [bu makalede](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource)açıklanan adımları izleyin.
>
> ![Linux][Logo_Linux] Linux
>
> [Kaynak Yöneticisi şablonu olarak kullanmak üzere Linux sanal makinesi yakalama][capture-image-linux-step-2-create-vm-image]
>
>

---
### <a name="transferring-vms-and-vhds-between-on-premises-to-azure"></a>Şirket içi VM'leri ve VHD'leri Azure'a aktarma
Azure portalı üzerinden Azure'a VM görüntüleri ve diskler yüklemek mümkün olmadığından, Azure PowerShell cmdlets veya CLI kullanmanız gerekir. Başka bir olasılık aracı 'AzCopy' kullanımıdır. Araç, şirket içi ve Azure arasında (her iki yönde de) VHD'leri kopyalayabilir. Ayrıca Azure Bölgeleri arasında VHD'leri kopyalayabilir. AzCopy'nin indirimi ve kullanımı için lütfen [bu belgelere][storage-use-azcopy] başvurun.

Üçüncü bir alternatif çeşitli üçüncü taraf GUI odaklı araçlar kullanmak olacaktır. Ancak, bu araçların Azure Sayfa Blobs'u desteklediğinden emin olun. Amaçlarımız için Azure Page Blob deposunu kullanmamız gerekir (farklar burada açıklanmıştır: <https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>). Ayrıca Azure tarafından sağlanan araçlar, yüklenmesi gereken VM'leri ve VH'leri sıkıştırmada da etkilidir. Sıkıştırmadaki bu verimlilik yükleme süresini kısalttığı için bu önemlidir (bu da şirket içi tesisten ve hedeflenen Azure dağıtım bölgesinden internete yükleme bağlantısına bağlı olarak değişir). Avrupa'nın konumundan ABD merkezli Azure veri merkezlerine VM veya VHD yüklemenin, aynı VM/VH'leri Avrupa Azure veri merkezlerine yüklemekten daha uzun süreceğinin adil bir varsayımıdır.

#### <a name="uploading-a-vhd-from-on-premises-to-azure"></a><a name="a43e40e6-1acc-4633-9816-8f095d5a7b6a"></a>Şirket içinde Azure'a VHD yükleme
Varolan bir VM veya VHD'yi şirket içi ağdan yüklemek için böyle bir VM veya VHD'nin, [bir VM'yi şirket içinde azure'a taşıma][planning-guide-5.2.1] bölümünde listelenen gereksinimleri bu belgenin genellenmemiş bir diskiyle karşılaması gerekir.

Böyle bir VM genelleştirilmiş olması gerekmez ve durum ve şekil şirket içi tarafında kapatıldıktan sonra sahip yüklenebilir. Aynı durum, herhangi bir işletim sistemi içermeyen ek VHD'ler için de geçerlidir.

##### <a name="uploading-a-vhd-and-making-it-an-azure-disk"></a>VHD yükleme ve Azure Diski yapma
Bu durumda, içinde işletim sistemi olan veya olmayan bir VHD yüklemek ve veri diski olarak VM'ye monte etmek veya işletim sistemi diski olarak kullanmak istiyoruz. Bu çok aşamalı bir süreçtir

**Powershell**

* *Connect-AzAccount* ile aboneliğinizde oturum açın
* Bağlamınızın aboneliğini *Set-AzContext* ve parametre SubscriptionId veya SubscriptionName ile ayarlayın - bkz.<https://docs.microsoft.com/powershell/module/az.accounts/set-Azcontext>
* *Ekle-AzVhd* ile VHD'yi Azure Depolama Hesabına yükleyin - bkz.<https://docs.microsoft.com/powershell/module/az.compute/add-Azvhd>
* (İsteğe bağlı) *New-AzDisk* ile VHD'den Yönetilen Disk Oluşturun - bkz.<https://docs.microsoft.com/powershell/module/az.compute/new-Azdisk>
* Yeni bir VM config'in işletim sistemi diskini *Set-AzVMOSDisk* ile VHD veya Yönetilen Disk'e ayarlayın - bkz.<https://docs.microsoft.com/powershell/module/az.compute/set-Azvmosdisk>
* *New-AzVM* ile VM config yeni bir VM oluşturun - bkz.<https://docs.microsoft.com/powershell/module/az.compute/new-Azvm>
* *Add-AzVMDataDisk* ile yeni bir VM'ye veri diski ekleyin - bkz.<https://docs.microsoft.com/powershell/module/az.compute/add-Azvmdatadisk>

**Azure CLI**

* *az giriş* ile aboneliğinizde oturum açın
* Az hesap kümesi yle aboneliğinizi seçin *--abonelik `<subscription name or id` *
* *Az depolama blob yüklemeile* VHD'yi yükleyin - Azure [Depolama ile Azure CLI'yi kullanma][storage-azure-cli]
* (İsteğe bağlı) *Az disk oluşturma* ile VHD'den Yönetilen Disk oluşturma - bkz.https://docs.microsoft.com/cli/azure/disk
* Yüklenen VHD veya Yönetilen Diski *az vm oluşturma* ve parametre *-ekle-os-disk* ile OS diski olarak belirten yeni bir VM oluşturma
* *Az vm disk ekleme* ve parametre ile yeni bir VM'ye veri diski ekleme *--yeni*

**Şablon**

* PowerShell veya Azure CLI ile VHD'yi yükleyin
* (İsteğe bağlı) PowerShell, Azure CLI veya Azure portalı ile VHD'den Yönetilen Disk oluşturma
* VM'yi, vhd'yi bu örnekte gösterildiği gibi başvuran bir [JSON şablonuyla](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-specialized-vhd-new-or-existing-vnet/azuredeploy.json) veya [bu örnekteki JSON şablonunda](https://github.com/Azure/azure-quickstart-templates/blob/master/sap-2-tier-user-image-md/azuredeploy.json)gösterildiği gibi Yönetilen Diskler'i kullanarak dağıtın.

#### <a name="deployment-of-a-vm-image"></a>VM Görüntüsünün Dağıtımı
Varolan bir VM veya VHD'yi şirket içi ağdan yüklemek için, azure VM görüntüsü olarak kullanmak için böyle bir VM veya VHD'nin, bu belgenin SAP'si [için müşteriye özel bir görüntüyle VM dağıtma][planning-guide-5.2.2] bölümünde listelenen gereksinimleri karşılaması gerekir.

* VM'nizi genelleştirmek için Windows'ta *sysprep* veya *Linux'ta waagent deprovision'ı* kullanın - Bkz. Windows için [Sysprep Teknik Başvurusu](https://technet.microsoft.com/library/cc766049.aspx) veya Linux için Kaynak Yöneticisi [şablonu olarak kullanmak üzere bir Linux sanal makinesi nasıl yakalanabilir][capture-image-linux-step-2-create-vm-image]
* *Connect-AzAccount* ile aboneliğinizde oturum açın
* Bağlamınızın aboneliğini *Set-AzContext* ve parametre SubscriptionId veya SubscriptionName ile ayarlayın - bkz.<https://docs.microsoft.com/powershell/module/az.accounts/set-Azcontext>
* *Ekle-AzVhd* ile VHD'yi Azure Depolama Hesabına yükleyin - bkz.<https://docs.microsoft.com/powershell/module/az.compute/add-Azvhd>
* (İsteğe bağlı) *New-AzImage* ile VHD'den Yönetilen Disk Görüntüsü Oluşturma - bkz.<https://docs.microsoft.com/powershell/module/az.compute/new-Azimage>
* Yeni bir VM config işletim sistemi diskini
  * *Set-AzVMOSDisk -SourceImageUri -CreateOption fromImage* ile VHD - bkz.<https://docs.microsoft.com/powershell/module/az.compute/set-Azvmosdisk>
  * Yönetilen Disk Görüntü *Seti-AzVMSourceImage* - bkz.<https://docs.microsoft.com/powershell/module/az.compute/set-Azvmsourceimage>
* *New-AzVM* ile VM config yeni bir VM oluşturun - bkz.<https://docs.microsoft.com/powershell/module/az.compute/new-Azvm>

**Azure CLI**

* VM'nizi genelleştirmek için Windows'ta *sysprep* veya *Linux'ta waagent deprovision'ı* kullanın - Bkz. Windows için [Sysprep Teknik Başvurusu](https://technet.microsoft.com/library/cc766049.aspx) veya Linux için Kaynak Yöneticisi [şablonu olarak kullanmak üzere bir Linux sanal makinesi nasıl yakalanabilir][capture-image-linux-step-2-create-vm-image]
* *az giriş* ile aboneliğinizde oturum açın
* Az hesap kümesi yle aboneliğinizi seçin *--abonelik `<subscription name or id` *
* *Az depolama blob yüklemeile* VHD'yi yükleyin - Azure [Depolama ile Azure CLI'yi kullanma][storage-azure-cli]
* (İsteğe bağlı) *Az görüntü oluşturma* ile VHD'den Yönetilen Disk Görüntüsü oluşturma - bkz.https://docs.microsoft.com/cli/azure/image
* Yüklenen VHD veya Yönetilen Disk Görüntüsünü *az vm oluşturma* ve parametre *-görüntü* ile işletim sistemi diski olarak belirten yeni bir VM oluşturma

**Şablon**

* VM'nizi genelleştirmek için Windows'ta *sysprep* veya *Linux'ta waagent deprovision'ı* kullanın - Bkz. Windows için [Sysprep Teknik Başvurusu](https://technet.microsoft.com/library/cc766049.aspx) veya Linux için Kaynak Yöneticisi [şablonu olarak kullanmak üzere bir Linux sanal makinesi nasıl yakalanabilir][capture-image-linux-step-2-create-vm-image]
* PowerShell veya Azure CLI ile VHD'yi yükleyin
* (İsteğe bağlı) PowerShell, Azure CLI veya Azure portalı ile VHD'den Yönetilen Disk Görüntüsü oluşturma
* VM'yi, [bu örnekteki JSON şablonunda](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-specialized-vhd-new-or-existing-vnet/azuredeploy.json) gösterildiği gibi görüntü VHD'ye atıfta bulunan bir JSON [şablonuyla veya bu örnekteki JSON şablonunda](https://github.com/Azure/azure-quickstart-templates/blob/master/sap-2-tier-user-image-md/azuredeploy.json)gösterildiği gibi Yönetilen Disk Görüntüsünü kullanarak dağıtın.

#### <a name="downloading-vhds-or-managed-disks-to-on-premises"></a>Şirket içi VHD'ler veya Yönetilen Diskler indirme
Hizmet Olarak Azure Altyapısı, yalnızca VH'ler ve SAP sistemleri yükleyebilen tek yönlü bir yol değildir. SAP sistemlerini Azure'dan şirket içi dünyaya da taşıyabilirsiniz.

İndirme sırasında VHD'ler veya Yönetilen Diskler etkin olamaz. VM'lere monte edilen diskleri indirirken bile VM'nin kapatılması ve ayrılması gerekir. Yalnızca veritabanı içeriğini indirmek istiyorsanız, bu sistem şirket içinde yeni bir sistem kurmak için kullanılmalıdır ve indirme sırasında ve yeni sistemin kurulumu sırasında Azure'daki sistemin hala çalışır durumda olması kabul edilebilirse , sıkıştırılmış bir veritabanı yedeklemesini bir diske gerçekleştirerek uzun bir kapalı kalma süresini önleyebilir ve işletim sistemi temel VM'sini indirmek yerine bu diski indirebilirsiniz.

#### <a name="powershell"></a>PowerShell

* Yönetilen Disk Indirme  
  Öncelikle Yönetilen Disk'in altında yatan blob'a erişmeniz gerekir. Ardından, altta yatan blob'u yeni bir depolama hesabına kopyalayabilir ve bu depolama hesabından blob'u indirebilirsiniz.

  ```powershell
  $access = Grant-AzDiskAccess -ResourceGroupName <resource group> -DiskName <disk name> -Access Read -DurationInSecond 3600
  $key = (Get-AzStorageAccountKey -ResourceGroupName <resource group> -Name <storage account name>)[0].Value
  $destContext = (New-AzStorageContext -StorageAccountName <storage account name -StorageAccountKey $key)
  Start-AzStorageBlobCopy -AbsoluteUri $access.AccessSAS -DestContainer <container name> -DestBlob <blob name> -DestContext $destContext
  # Wait for blob copy to finish
  Get-AzStorageBlobCopyState -Container <container name> -Blob <blob name> -Context $destContext
  Save-AzVhd -SourceUri <blob in new storage account> -LocalFilePath <local file path> -StorageKey $key
  # Wait for download to finish
  Revoke-AzDiskAccess -ResourceGroupName <resource group> -DiskName <disk name>
  ```

* VHD indirme  
  SAP sistemi durdurulduktan ve VM kapatıldıktan sonra, VHD disklerini şirket içi dünyaya geri indirmek için şirket içi hedefte PowerShell cmdlet Save-AzVhd'yi kullanabilirsiniz. Bunu yapmak için, Azure portalının 'depolama Bölümü'nde bulabileceğiniz VHD'nin URL'sine (Depolama Hesabına ve VHD'nin oluşturulduğu depolama kabına gidin) ve VHD'nin nerede kopyalanması gerektiğini bilmeniz gerekir.

  Daha sonra, SourceUri parametresini vhd'nin URL'si olarak, LocalFilePath'i ise VHD'nin fiziksel konumu (adı da dahil) olarak tanımlayarak komutu kaldırabilirsiniz. Komut aşağıdaki gibi görünebilir:

  ```powerhell
  Save-AzVhd -ResourceGroupName <resource group name of storage account> -SourceUri http://<storage account name>.blob.core.windows.net/<container name>/sapidedata.vhd -LocalFilePath E:\Azure_downloads\sapidesdata.vhd
  ```

  Save-AzVhd cmdlet hakkında daha fazla <https://docs.microsoft.com/powershell/module/az.compute/save-Azvhd>bilgi için buraya bakın.

#### <a name="azure-cli"></a>Azure CLI
* Yönetilen Disk Indirme  
  Öncelikle Yönetilen Disk'in altında yatan blob'a erişmeniz gerekir. Ardından, altta yatan blob'u yeni bir depolama hesabına kopyalayabilir ve bu depolama hesabından blob'u indirebilirsiniz.

  ```azurecli
  az disk grant-access --ids "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>" --duration-in-seconds 3600
  az storage blob download --sas-token "<sas token>" --account-name <account name> --container-name <container name> --name <blob name> --file <local file>
  az disk revoke-access --ids "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>"
  ```

* VHD indirme   
  SAP sistemi durdurulduktan ve VM kapatıldıktan sonra, VHD disklerini şirket içi dünyaya geri indirmek için şirket içi hedefteki Azure CLI _komutu azure depolama blob indirmesini_ kullanabilirsiniz. Bunu yapmak için, Azure portalının 'Depolama Bölümü'nde bulabileceğiniz VHD'nin adını ve kapsayıcısını (VHD'nin oluşturulduğu Depolama Hesabına ve depolama konteynerine gidin) ve VHD'nin nerede kopyalanması gerektiğini bilmeniz gerekir Hedef.

  Daha sonra vhd parametreleri blob ve karşıdan yükleme ve hedef VHD fiziksel hedef konumu (adı da dahil olmak üzere) olarak tanımlayarak komut kaldıraç olabilir. Komut aşağıdaki gibi görünebilir:

  ```azurecli
  az storage blob download --name <name of the VHD to download> --container-name <container of the VHD to download> --account-name <storage account name of the VHD to download> --account-key <storage account key> --file <destination of the VHD to download>
  ```

### <a name="transferring-vms-and-disks-within-azure"></a>Azure'da VM'ler ve diskler aktarma

#### <a name="copying-sap-systems-within-azure"></a>Azure içinde SAP sistemlerini kopyalama

Sap sistemi veya sap uygulama katmanını destekleyen özel bir DBMS sunucusu büyük olasılıkla ikili işletim sistemi veya SAP veritabanının veri ve günlük dosyası(lar) içeren birkaç diskten oluşacaktır. Ne disk kopyalamanın Azure işlevi ne de diskleri yerel bir diske kaydetmenin Azure işlevi, birden çok diski tutarlı bir şekilde görüntüleyen bir eşitleme mekanizmasına sahip değildir. Bu nedenle, aynı VM'ye karşı monte edilmiş olsa lar bile, kopyalanan veya kaydedilen disklerin durumu farklı olacaktır. Bu, farklı disklerde bulunan farklı veri ve logfile(ler) sahip somut durumda, sonunda veritabanı tutarsız olacağı anlamına gelir.

**Sonuç: SAP sistem yapılandırmasının bir parçası olan diskleri kopyalamak veya kaydetmek için SAP sistemini durdurmanız ve ayrıca dağıtılan VM'yi kapatmanız gerekir. Ancak o zaman, Azure'da veya şirket içinde SAP sisteminin bir kopyasını oluşturmak için disk kümesini kopyalayabilir veya indirebilirsiniz.**

Veri diskleri Bir Azure Depolama Hesabı'nda VHD dosyaları olarak depolanabilir ve doğrudan sanal bir makineye bağlanabilir veya görüntü olarak kullanılabilir. Bu durumda, VHD sanal makineye bağlanmadan önce başka bir konuma kopyalanır. Azure'daki VHD dosyasının tam adı Azure içinde benzersiz olmalıdır. Daha önce de belirtildiği gibi, isim gibi görünen üç bölümlü bir ad tür:

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

Veri diskleri yönetilen diskler de olabilir. Bu durumda, Yönetilen Disk sanal makineye bağlanmadan önce yeni bir Yönetilen Disk oluşturmak için kullanılır. Yönetilen Disk'in adı bir kaynak grubu içinde benzersiz olmalıdır.

##### <a name="powershell"></a>PowerShell

[Bu makalede][storage-powershell-guide-full-copy-vhd]gösterildiği gibi bir VHD kopyalamak için Azure PowerShell cmdlets kullanabilirsiniz. Yeni bir Yönetilen Disk oluşturmak için aşağıdaki örnekte gösterildiği gibi New-AzDiskConfig ve New-AzDisk kullanın.

```powershell
$config = New-AzDiskConfig -CreateOption Copy -SourceUri "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>" -Location <location>
New-AzDisk -ResourceGroupName <resource group name> -DiskName <disk name> -Disk $config
```

##### <a name="azure-cli"></a>Azure CLI

Bir VHD'yi kopyalamak için Azure CLI'yi kullanabilirsiniz. Yeni bir Yönetilen Disk oluşturmak için aşağıdaki örnekte gösterildiği gibi *az disk oluşturma* kullanın.

```azurecli
az disk create --source "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>" --name <disk name> --resource-group <resource group name> --location <location>
```

##### <a name="azure-storage-tools"></a>Azure Depolama araçları

* <https://storageexplorer.com/>

Azure Depolama Kaşiflerinin profesyonel sürümlerini burada bulabilirsiniz:

* <https://www.cerebrata.com/>
* <https://clumsyleaf.com/products/cloudxplorer>

Bir depolama hesabı içinde bir VHD kendisi kopyası sadece birkaç saniye sürer bir işlemdir (SAN donanım yazma üzerinde tembel kopya ve kopya ile anlık görüntü oluşturma benzer). VHD dosyasının bir kopyasını aldıktan sonra, sanal bir makineye takabilir veya sanal makinelere VHD kopyalarını eklemek için bir görüntü olarak kullanabilirsiniz.

##### <a name="powershell"></a>PowerShell

```powershell
# attach a vhd to a vm
$vm = Get-AzVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzVMDataDisk -VM $vm -Name newdatadisk -VhdUri <path to vhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun, for example 0> -CreateOption attach
$vm | Update-AzVM

# attach a managed disk to a vm
$vm = Get-AzVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzVMDataDisk -VM $vm -Name newdatadisk -ManagedDiskId <managed disk id> -Caching <caching option> -DiskSizeInGB $null -Lun <lun, for example 0> -CreateOption attach
$vm | Update-AzVM

# attach a copy of the vhd to a vm
$vm = Get-AzVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzVMDataDisk -VM $vm -Name <disk name> -VhdUri <new path of vhd> -SourceImageUri <path to image vhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun, for example 0> -CreateOption fromImage
$vm | Update-AzVM

# attach a copy of the managed disk to a vm
$vm = Get-AzVM -ResourceGroupName <resource group name> -Name <vm name>
$diskConfig = New-AzDiskConfig -Location $vm.Location -CreateOption Copy -SourceUri <source managed disk id>
$disk = New-AzDisk -DiskName <disk name> -Disk $diskConfig -ResourceGroupName <resource group name>
$vm = Add-AzVMDataDisk -VM $vm -Caching <caching option> -Lun <lun, for example 0> -CreateOption attach -ManagedDiskId $disk.Id
$vm | Update-AzVM
```

##### <a name="azure-cli"></a>Azure CLI

```azurecli

# attach a vhd to a vm
az vm unmanaged-disk attach --resource-group <resource group name> --vm-name <vm name> --vhd-uri <path to vhd>

# attach a managed disk to a vm
az vm disk attach --resource-group <resource group name> --vm-name <vm name> --disk <managed disk id>

# attach a copy of the vhd to a vm
# this scenario is currently not possible with Azure CLI. A workaround is to manually copy the vhd to the destination.

# attach a copy of a managed disk to a vm
az disk create --name <new disk name> --resource-group <resource group name> --location <location of target virtual machine> --source <source managed disk id>
az vm disk attach --disk <new disk name or managed disk id> --resource-group <resource group name> --vm-name <vm name> --caching <caching option> --lun <lun, for example 0>
```

#### <a name="copying-disks-between-azure-storage-accounts"></a><a name="9789b076-2011-4afa-b2fe-b07a8aba58a1"></a>Azure Depolama Hesapları arasında diskleri kopyalama
Bu görev Azure portalında gerçekleştirilebilir. Azure PowerShell cmdlets, Azure CLI veya bir üçüncü taraf depolama tarayıcısı kullanabilirsiniz. PowerShell cmdlets veya CLI komutları, Azure aboneliği içindeki Depolama Hesapları ve bölgeler arasında blob'ları eş senkronize bir şekilde kopyalama olanağı nı da içeren lekeler oluşturabilir ve yönetebilir.

##### <a name="powershell"></a>PowerShell
VHD'leri abonelikler arasında da kopyalayabilirsiniz. Daha fazla bilgi için [bu makaleyi][storage-powershell-guide-full-copy-vhd]okuyun.

PS cmdlet mantığının temel akışı şu na benzer:

* *New-AzStorageContext* ile **kaynak** depolama hesabı için bir depolama hesabı bağlamı oluşturma - bkz.<https://docs.microsoft.com/powershell/module/az.storage/new-AzStoragecontext>
* *New-AzStorageContext* ile **hedef** depolama hesabı için bir depolama hesabı bağlamı oluşturma - bkz.<https://docs.microsoft.com/powershell/module/az.storage/new-AzStoragecontext>
* Kopyayı

```powershell
Start-AzStorageBlobCopy -SrcBlob <source blob name> -SrcContainer <source container name> -SrcContext <variable containing context of source storage account> -DestBlob <target blob name> -DestContainer <target container name> -DestContext <variable containing context of target storage account>
```

* Bir döngüiçinde kopyanın durumunu kontrol edin

```powershell
Get-AzStorageBlobCopyState -Blob <target blob name> -Container <target container name> -Context <variable containing context of target storage account>
```

* Yeni VHD'yi yukarıda açıklandığı gibi sanal bir makineye takın.

Örnekler için [bu makaleye][storage-powershell-guide-full-copy-vhd]bakın.

##### <a name="azure-cli"></a>Azure CLI
* Kopyayı

```azurecli
az storage blob copy start --source-blob <source blob name> --source-container <source container name> --source-account-name <source storage account name> --source-account-key <source storage account key> --destination-container <target container name> --destination-blob <target blob name> --account-name <target storage account name> --account-key <target storage account name>
```

* Kopya hala bir döngü içinde yse durumu kontrol edin

```azurecli
az storage blob show --name <target blob name> --container <target container name> --account-name <target storage account name> --account-key <target storage account name>
```

* Yeni VHD'yi yukarıda açıklandığı gibi sanal bir makineye takın.

### <a name="disk-handling"></a>Disk Taşıma

#### <a name="vmdisk-structure-for-sap-deployments"></a><a name="4efec401-91e0-40c0-8e64-f2dceadff646"></a>SAP dağıtımları için VM/disk yapısı

İdeal olarak bir VM yapısı nın ve ilişkili disklerin işlenmesi basit olmalıdır. Şirket içi yüklemelerde, müşteriler sunucu yüklemesini yapılandırmanın birçok yolunu geliştirdiler.

* Işletim sistemi ve DBMS ve/veya SAP'nin tüm ikililerini içeren bir temel disk. Mart 2015'ten bu yana, bu diski 127 GB ile sınırlayan önceki kısıtlamalar yerine 1 TB boyutuna kadar olabilir.
* SAP veritabanının DBMS günlük dosyasını ve DBMS geçici depolama alanının günlük dosyasını içeren bir veya birden çok disk (DBMS bunu destekliyorsa). Veritabanı günlüğü IOPS gereksinimleri yüksekse, gerekli IOPS hacmine ulaşmak için birden çok diski şeritlemeniz gerekir.
* SAP veritabanının bir veya iki veritabanı dosyasını ve DBMS geçici veri dosyalarını içeren bir dizi disk de (DBMS bunu destekliyorsa).

![SAP için Azure IaaS VM Başvuru Yapılandırması][planning-guide-figure-1300]


---
> ![Windows][Logo_Windows] Windows
>
> Birçok müşteri ile biz yapılandırmaları gördüm, örneğin, SAP ve DBMS ikilic yüklü değildi:\ işletim sistemi yüklü olduğu sürücü. Bunun çeşitli nedenleri vardı, ama biz kök geri döndü, genellikle sürücüler küçük ve işletim sistemi yükseltmeleri 10-15 yıl önce ek alan gerekli olduğunu. Her iki durum da artık bu günlerde çok sık geçerli değil. Bugün c:\ sürücü büyük hacimli disklerde veya VM'lerde eşlenebilir. Dağıtımları yapılarında basit tutmak için, Azure'daki SAP NetWeaver sistemleri için aşağıdaki dağıtım modelini izlemenizi önerilir
>
> Windows işletim sistemi pagefile D olmalıdır: sürücü (kalıcı olmayan disk)
>
> ![Linux][Logo_Linux] Linux
>
> Linux swapfilesini [bu makalede][virtual-machines-linux-agent-user-guide]açıklandığı şekilde Linux'a /mnt/mnt/kaynak altına yerleştirin. Takas dosyası Linux Agent /etc/waagent.conf yapılandırma dosyasında yapılandırılabilir. Aşağıdaki ayarları ekleyin veya değiştirin:
>
>

```console
ResourceDisk.EnableSwap=y
ResourceDisk.SwapSizeMB=30720
```

Değişiklikleri etkinleştirmek için Linux Agent'ı

```console
sudo service waagent restart
```

Önerilen takas dosya boyutu hakkında daha fazla bilgi için LÜTFEN SAP Note [1597355'i] okuyun

---
DBMS veri dosyaları için kullanılan disk sayısı ve bu disklerin barındırılan Azure Depolama türü IOPS gereksinimlerine ve gereken gecikme ye göre belirlenmelidir. Tam kotalar [bu makalede (Linux)][virtual-machines-sizes-linux] ve [bu makalede (Windows)][virtual-machines-sizes-windows]açıklanmıştır.

Sap dağıtımlarının son iki yıldaki deneyimi bize şu şekilde özetlenebilir bazı dersler öğretti:

* Varolan müşteri sistemleri SAP veritabanı(lar) temsil eden farklı boyutlu veri dosyaları olabilir, çünkü farklı veri dosyalarına IOPS trafiği her zaman aynı değildir. Sonuç olarak, LUN'lerin bunlardan oyulmuş veri dosyalarını yerleştirmek için birden çok disk üzerinden RAID yapılandırması kullanımı daha iyi oldu. Özellikle Azure Standart Depolama'da, IOPS oranının DBMS işlem günlüğüne karşı tek bir diskin kotasına çarptığı durumlar vardı. Bu tür senaryolarda, Premium Depolama kullanımı önerilir veya alternatif olarak bir yazılım şeridi ile birden çok Standart Depolama diski biraraya.

---
> ![Windows][Logo_Windows] Windows
>
> * [Azure Sanal Makinelerde SQL Server için performansa yönelik en iyi yöntemler][virtual-machines-sql-server-performance-best-practices]
>
> ![Linux][Logo_Linux] Linux
>
> * [Linux'ta Yazılım RAID'i yapılandırın][virtual-machines-linux-configure-raid]
> * [AZURE'da BIR Linux VM üzerinde LVM yapılandırma][virtual-machines-linux-configure-lvm]
> * [Azure Depolama sırları ve Linux G/Ç optimizasyonları](https://blogs.msdn.com/b/igorpag/archive/2014/10/23/azure-storage-secrets-and-linux-i-o-optimizations.aspx)
>
>

---
* Premium Depolama, özellikle kritik işlem günlüğü yazmaları için önemli ölçüde daha iyi performans gösteriyor. Performans gibi üretim sağlaması beklenen SAP senaryolarında, Azure Premium Depolama'dan yararlanabilen VM-Serisi'nin kullanılması önerilir.

İşletim sistemi içeren ve tavsiye ettiğimiz gibi SAP'nin ikililerinin ve veritabanının (temel VM) artık 127 GB ile sınırlı olmadığını unutmayın. Şimdi boyutu 1 TB kadar olabilir. Bu, örneğin SAP toplu iş günlükleri de dahil olmak üzere gerekli tüm dosyayı tutmak için yeterli alan olmalıdır.

Özellikle DBMS VM'ler için daha fazla öneri ve daha fazla ayrıntı için [DBMS Dağıtım Kılavuzu'na][dbms-guide] başvurun

#### <a name="disk-handling"></a>Disk Taşıma

Çoğu senaryoda, SAP veritabanını VM'ye dağıtmak için ek diskler oluşturmanız gerekir. Bu belgenin SAP dağıtımları için Bölüm [VM/disk yapısındaki][planning-guide-5.5.1] disk sayısıyla ilgili hususlarhakkında konuştuk. Azure portalı, temel VM dağıtıldıktan sonra diskleri eklemeye ve ayırmaya olanak tanır. VM dolduğunda ve çalışırken ve durdurulduğunda diskler eklenebilir/ayrılabilir. Azure portalı bir disk iliştirirken, şu anda başka bir VM'ye bağlı olmayan boş bir disk veya varolan bir disk eklemeyi teklif ediyor.

**Not**: Diskler herhangi bir anda yalnızca bir VM'ye takılabilir.

![Azure Standart Depolama ile diskleri ekleme/ ayırma][planning-guide-figure-1400]

Yeni bir sanal makinenin dağıtımı sırasında Yönetilen Diskleri kullanmak veya disklerinizi Azure Depolama Hesapları'na yerleştirmek isteyip istemediğinize karar verebilirsiniz. Premium Depolama'yı kullanmak istiyorsanız Yönetilen Diskler'i kullanmanızı öneririz.

Ardından, yeni ve boş bir disk oluşturmak isteyip istemediğiniz veya daha önce yüklenen ve şimdi VM'ye eklenmesi gereken varolan bir diski seçmek isteyip istemediğiniz konusunda karar vermeniz gerekir.

**ÖNEMLİ**: Azure Standart Depolama ile Ana Bilgisayar Önbelleğe Alma'yı kullanmak **istemiyorsun.** Host Önbellek tercihini NONE varsayılanında bırakmalısınız. Azure Premium Depolama ile, G/Ç özelliği çoğunlukla veritabanı veri dosyalarına karşı tipik G/Ç trafiği gibi okunuyorsa, Önbelleğe Alma'yı etkinleştirmelisiniz. Veritabanı hareket günlüğü dosyası durumunda, önbelleğe alma önerilir.

---
> ![Windows][Logo_Windows] Windows
>
> [Azure portalına veri diski ekleme][virtual-machines-linux-attach-disk-portal]
>
> Diskler bağlıysa, Windows Disk Yöneticisi'ni açmak için VM'de oturum açmanız gerekir. Bağlı [diskler için otomatik montaj ayarlama][planning-guide-5.5.3]bölümünde önerildiği gibi otomatik montaj etkinleştirilmezse, yeni eklenen birimin çevrimiçi olarak alınması ve başlatılması gerekir.
>
> ![Linux][Logo_Linux] Linux
>
> Diskler bağlıysa, VM'de oturum açmanız ve [bu makalede][virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux] açıklandığı şekilde diskleri başlatmanız gerekir
>
>

---
Yeni disk boş bir diskse, diski de biçimlendirmeniz gerekir. Biçimlendirme için, özellikle DBMS verileri ve günlük dosyaları için DBMS'nin çıplak metal dağıtımları için aynı öneriler geçerlidir.

Azure Depolama hesabı, G/Ç hacmi, IOPS ve veri hacmi açısından sonsuz kaynak sağlamaz. Genellikle DBMS VM'ler bundan en çok etkilenir. Azure Depolama Hesabı hacminin sınırında kalmak için dağıtmak için dağıtmak için az sayıda yüksek G/Ç hacimli VM'niz varsa, her VM için ayrı bir Depolama Hesabı kullanmak en iyisi olabilir. Aksi takdirde, her bir Depolama Hesabının sınırına basmadan bu VM'leri farklı Depolama hesapları arasında nasıl dengeleyebilirsiniz görmeniz gerekir. Daha fazla ayrıntı [DBMS Dağıtım Kılavuzu'nda][dbms-guide]ele alınmıştır. Ayrıca saf SAP uygulama sunucusu VM'ler veya diğer VM'ler için bu sınırlamaları göz önünde bulundurmanız gerekir, bu da sonunda ek VHD'ler gerektirebilir. Yönetilen Disk kullanıyorsanız, bu kısıtlamalar geçerli değildir. Premium Depolama'yı kullanmayı planlıyorsanız, Yönetilen Disk'i kullanmanızı öneririz.

Depolama Hesapları ile ilgili bir diğer konu da, Depolama Hesabındaki VHD'lerin Coğrafi olarak çoğaltılıp çoğaltılmadığıdır. Coğrafi çoğaltma, VM düzeyinde değil, Depolama Hesabı düzeyinde etkin veya devre dışı bırakılır. Coğrafi çoğaltma etkinleştirilirse, Depolama Hesabı'ndaki VHD'ler aynı bölge içindeki başka bir Azure veri merkezine çoğaltılır. Bu konuda karar vermeden önce, aşağıdaki kısıtlama hakkında düşünmelisiniz:

Azure Coğrafi çoğaltma, bir VM'deki her VHD'de yerel olarak çalışır ve Bir VM'deki birden çok VHD'de iOs'ları kronolojik sırada çoğaltmaz. Bu nedenle, vm tabanını temsil eden VHD ve VM'ye bağlı ek VH'ler birbirinden bağımsız olarak çoğaltılır. Bu, farklı VHD'lerde değişiklikler arasında eşitleme olmadığı anlamına gelir. IOs'ların yazıldıkları sırayla bağımsız olarak çoğaltılması, veritabanlarıbirden fazla VHD'ye dağıtılan veritabanı sunucuları için coğrafi çoğaltmanın değerli olmadığı anlamına gelir. DBMS'ye ek olarak, süreçlerin farklı VHD'lerde veri yazdığı veya işlediği ve değişikliklerin sırasını korumanın önemli olduğu başka uygulamalar da olabilir. Bu bir gereksinimse, Azure'da coğrafi çoğaltma etkinleştirilmemelidir. Bir vm kümesi için coğrafi çoğaltmaya ihtiyacınız olup olmadığına bağlı olarak, ancak başka bir küme için değil, VM'leri ve bunların ilgili VHD'lerini coğrafi çoğaltma etkin leştirilmiş veya devre dışı bırakılmış farklı Depolama Hesaplarına kategorilere ayırabilirsiniz.

#### <a name="setting-automount-for-attached-disks"></a><a name="17e0d543-7e8c-4160-a7da-dd7117a1ad9d"></a>Ekli diskler için otomatik montaj ayarlama
---
> ![Windows][Logo_Windows] Windows
>
> Kendi Görüntülerden veya Disklerden oluşturulan VM'ler için, otomatik montaj parametresini denetlemek ve ayarlamak gerekir. Bu parametrenin ayarlanması, Azure'da yeniden başlatma veya yeniden dağıtımdan sonra VM'nin bağlı/monte edilmiş sürücüleri otomatik olarak yeniden takmasına olanak tanır.
> Parametre, Microsoft tarafından Azure Marketi'nde sağlanan görüntüler için ayarlanır.
>
> Automount ayarlamak için, komut satırı yürütülebilir diskpart.exe burada belgeleri kontrol edin:
>
> * [DiskPart Komut Satırı Seçenekleri](https://technet.microsoft.com/library/bb490893.aspx)
> * [Otomatik montaj](https://technet.microsoft.com/library/cc753703.aspx)
>
> Windows komut satırı penceresi yönetici olarak açılmalıdır.
>
> Diskler bağlıysa, Windows Disk Yöneticisi'ni açmak için VM'de oturum açmanız gerekir. Bağlı [diskler için otomatik montaj ayarlama][planning-guide-5.5.3]bölümünde önerildiği gibi otomatik montaj etkinleştirilmezse, yeni eklenen birim >çevrimiçi olarak alınması ve başlatılması gerekir.
>
> ![Linux][Logo_Linux] Linux
>
> [Bu makalede][virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]açıklandığı gibi yeni eklenen boş bir diski başlatmanız gerekir.
> Ayrıca / etc / fstab yeni diskler eklemeniz gerekir.
>
>

---
### <a name="final-deployment"></a>Son Dağıtım

Özellikle SAP için Azure Uzantısı'nın dağıtımıyla ilgili olarak, son dağıtım ve kesin adımlar için [Dağıtım Kılavuzu'na][deployment-guide]bakın.

## <a name="accessing-sap-systems-running-within-azure-vms"></a>Azure VM'ler içinde çalışan SAP sistemlerine erişim

SAP GUI kullanarak bu SAP sistemlerine genel internet üzerinden bağlanmak istediğiniz senaryolar için aşağıdaki yordamların uygulanması gerekir.

Belgenin ilerleyen saatlerinde, şirket içi sistemler le Azure sistemleri arasında sitebağlantısı (VPN tüneli) veya Azure ExpressRoute bağlantısı olan, tesisler arası dağıtımlarda SAP sistemlerine bağlanan diğer ana senaryoyu tartışacağız.

### <a name="remote-access-to-sap-systems"></a>SAP sistemlerine Uzaktan Erişim

Azure Kaynak Yöneticisi ile artık eski klasik modeldeki gibi varsayılan uç nokta yok. Azure Kaynak Yöneticisi VM'nin tüm bağlantı noktaları şu sürece açıktır:

1. Alt ağ veya ağ arabirimi için Ağ Güvenlik Grubu tanımlanmamıştır. Azure VM'lere olan ağ trafiği sözde "Ağ Güvenlik Grupları" aracılığıyla güvence altına alınabilir. Daha fazla bilgi için bkz. [Ağ Güvenlik Grubu (NSG) nedir?][virtual-networks-nsg]
2. Ağ arabirimi için Azure Yük Dengeleyicisi tanımlanmadı   

[Bu makalede][virtual-machines-azure-resource-manager-architecture]açıklandığı gibi klasik model ve ARM arasındaki mimari farkı görün.

#### <a name="configuration-of-the-sap-system-and-sap-gui-connectivity-over-the-internet"></a>SAP Sistemi ve SAP GUI bağlantısının internet üzerinden yapılandırması

Lütfen bu konuya ayrıntıları açıklayan bu makaleye bakın:<https://blogs.msdn.com/b/saponsqlserver/archive/2014/06/24/sap-gui-connection-closed-when-connecting-to-sap-system-in-azure.aspx>

#### <a name="changing-firewall-settings-within-vm"></a>VM içinde Güvenlik Duvarı Ayarlarını Değiştirme

SAP sisteminize gelen trafiğe izin verecek şekilde sanal makinelerinizdeki güvenlik duvarını yapılandırmanız gerekebilir.

---
> ![Windows][Logo_Windows] Windows
>
> Varsayılan olarak, Azure'da dağıtılan bir VM içindeki Windows Güvenlik Duvarı açık. Artık SAP Bağlantı Noktası'nın açılmasına izin verebilirsiniz, aksi takdirde SAP GUI bağlanamaz.
> Bunu yapmak için:
>
> * Denetim Masası\Sistem ve Güvenlik\Windows Güvenlik **Duvar'ı Gelişmiş Ayarlara**Açın.
> * Şimdi Gelen Kurallar'a sağ tıklayın ve **Yeni Kural'ı**seçti.
> * Aşağıdaki Sihirbaz yeni bir **Bağlantı Noktası** kuralı oluşturmayı seçti.
> * Sihirbazın bir sonraki adımında, ayarı TCP'de bırakın ve açmak istediğiniz bağlantı noktası numarasını yazın. SAP örnek kimliğimiz 00 olduğundan, 3200 aldık. Örneğinizin farklı bir örnek numarası varsa, örnek numarasına göre daha önce tanımladığınız bağlantı noktası açılmalıdır.
> * Sihirbazın bir sonraki bölümünde, **Bağlantıya İzin Ver** öğesini işaretli bırakmanız gerekir.
> * Sihirbazın bir sonraki adımında kuralın Etki Alanı, Özel ve Genel ağ için geçerli olup olmadığını tanımlamanız gerekir. İhtiyaçlarınıza göre gerekirse ayarlayın. Ancak, genel ağ üzerinden dışarıdan SAP GUI ile bağlantı kurarak, kuralın ortak ağa uygulanması gerekir.
> * Sihirbazın son adımında, kuralı adlandırın ve **Finish**tuşuna basarak kaydedin.
>
> Kural hemen yürürlüğe girer.
>
> ![Bağlantı noktası kuralı tanımı][planning-guide-figure-1600]
>
> ![Linux][Logo_Linux] Linux
>
> Azure Marketi'ndeki Linux görüntüleri varsayılan olarak iptables güvenlik duvarını etkinleştirmez ve SAP sisteminize bağlantı çalışmalıdır. Iptables'ı veya başka bir güvenlik duvarını etkinleştirdiyseniz, gelen tcp trafiğinin bağlantı noktası 32xx'e (xx SAP sisteminizin sistem numarasıdır) izin vermek için iptables belgelerine veya kullanılan güvenlik duvarının belgelerine bakın.
>
>

---
#### <a name="security-recommendations"></a>Güvenlik önerileri

SAP GUI, çalışan SAP örneklerinin (bağlantı noktası 32xx) hemen hiçbirine bağlanmaz, ancak ilk olarak SAP ileti sunucusu işlemine (bağlantı noktası 36xx) açılan bağlantı noktası üzerinden bağlanır. Geçmişte, aynı bağlantı noktası uygulama örnekleriiçin iç iletişim için ileti sunucusu tarafından kullanılmıştır. Şirket içi uygulama sunucuların Azure'daki bir ileti sunucusuyla yanlışlıkla iletişim kurmasını önlemek için dahili iletişim bağlantı noktaları değiştirilebilir. SAP ileti sunucusu ve uygulama örnekleri arasındaki iç iletişimin, proje testi için geliştirme klonu gibi şirket içi sistemlerden klonlanmış sistemlerde farklı bir bağlantı noktası numarasıyla değiştirilmesi önerilir. Bu varsayılan profil parametresi ile yapılabilir:

> rdisp/msserv_internal
>
>

SAP İleti [Sunucusu için Güvenlik Ayarları'nda](https://help.sap.com/saphelp_nwpi71/helpdata/en/47/c56a6938fb2d65e10000000a42189c/content.htm) belgelenmiş gibi


### <a name="single-vm-with-sap-netweaver-demotraining-scenario"></a><a name="3e9c3690-da67-421a-bc3f-12c520d99a30"></a>SAP NetWeaver demo/eğitim senaryosuna sahip tek VM

![Azure Bulut Hizmetleri'nde yalıtılmış aynı VM adlarıyla tek vm SAP demo sistemleri çalıştırma][planning-guide-figure-1700]

Bu senaryoda, tam eğitim/demo senaryosunun tek bir VM'de bulunduğu tipik bir eğitim/demo sistemi senaryosu uyguluyoruz. Dağıtımın VM görüntü şablonları aracılığıyla yapıldığını varsayıyoruz. Ayrıca, bu demo/eğitimlerin birden fazlasının aynı ada sahip VM'lerle dağıtılması gerektiğini varsayıyoruz. Tüm eğitim sistemleri şirket içi varlıklarınız için bağlantı yada değildir ve karma dağıtımın tam tersidir.

Varsayım, bu belgede [Azure için SAP ile VM'ler hazırlama][planning-guide-5.2] bölümünün bazı bölümlerinde açıklandığı gibi bir VM Görüntüsü oluşturduğunuzdur.

Senaryoyu uygulamak için olayların sırası aşağıdaki gibi görünür:

##### <a name="powershell"></a>PowerShell

* Her eğitim/demo ortamı için yeni bir kaynak grubu oluşturun

```powershell
$rgName = "SAPERPDemo1"
New-AzResourceGroup -Name $rgName -Location "North Europe"
```

* Yönetilen Diskleri kullanmak istemiyorsanız yeni bir depolama hesabı oluşturma

```powershell
$suffix = Get-Random -Minimum 100000 -Maximum 999999
$account = New-AzStorageAccount -ResourceGroupName $rgName -Name "saperpdemo$suffix" -SkuName Standard_LRS -Kind "Storage" -Location "North Europe"
```

* Aynı ana bilgisayar adı ve IP adreslerinin kullanımını etkinleştirmek için her eğitim/demo ortamı için yeni bir sanal ağ oluşturun. Sanal ağ, Uzak Masaüstü erişimine ve SSH için 22 bağlantı noktasına olanak sağlamak için yalnızca 3389 bağlantı noktasına gelen trafiğin izin verdiği bir Ağ Güvenlik Grubu tarafından korunur.

```powershell
# Create a new Virtual Network
$rdpRule = New-AzNetworkSecurityRuleConfig -Name SAPERPDemoNSGRDP -Protocol * -SourcePortRange * -DestinationPortRange 3389 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 100
$sshRule = New-AzNetworkSecurityRuleConfig -Name SAPERPDemoNSGSSH -Protocol * -SourcePortRange * -DestinationPortRange 22 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 101
$nsg = New-AzNetworkSecurityGroup -Name SAPERPDemoNSG -ResourceGroupName $rgName -Location  "North Europe" -SecurityRules $rdpRule,$sshRule

$subnetConfig = New-AzVirtualNetworkSubnetConfig -Name Subnet1 -AddressPrefix  10.0.1.0/24 -NetworkSecurityGroup $nsg
$vnet = New-AzVirtualNetwork -Name SAPERPDemoVNet -ResourceGroupName $rgName -Location "North Europe"  -AddressPrefix 10.0.1.0/24 -Subnet $subnetConfig
```

* Sanal makineye internetten erişmek için kullanılabilecek yeni bir genel IP adresi oluşturma

```powershell
# Create a public IP address with a DNS name
$pip = New-AzPublicIpAddress -Name SAPERPDemoPIP -ResourceGroupName $rgName -Location "North Europe" -DomainNameLabel $rgName.ToLower() -AllocationMethod Dynamic
```

* Sanal makine için yeni bir ağ arabirimi oluşturma

```powershell
# Create a new Network Interface
$nic = New-AzNetworkInterface -Name SAPERPDemoNIC -ResourceGroupName $rgName -Location "North Europe" -Subnet $vnet.Subnets[0] -PublicIpAddress $pip
```

* Sanal makine oluşturur. Bu senaryo için, her VM aynı ada sahip olacaktır. Bu VM'lerde SAP NetWeaver örneklerinin SAP SID'si de aynı olacaktır. Azure Kaynak Grubu içinde, VM adının benzersiz olması gerekir, ancak farklı Azure Kaynak Gruplarında aynı adla VM'ler çalıştırabilirsiniz. Windows'un varsayılan 'Yönetici' hesabı veya Linux için 'root' geçerli değildir. Bu nedenle, yeni bir yönetici kullanıcı adı bir parola ile birlikte tanımlanması gerekir. VM boyutunu da tanımlanması gerekir.

```powershell
#####
# Create a new virtual machine with an official image from the Azure Marketplace
#####
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vmconfig = New-AzVMConfig -VMName SAPERPDemo -VMSize Standard_D11

# select image
$vmconfig = Set-AzVMSourceImage -VM $vmconfig -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
$vmconfig = Set-AzVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
# $vmconfig = Set-AzVMSourceImage -VM $vmconfig -PublisherName "SUSE" -Offer "SLES-SAP" -Skus "12-SP1" -Version "latest"
# $vmconfig = Set-AzVMSourceImage -VM $vmconfig -PublisherName "RedHat" -Offer "RHEL" -Skus "7.2" -Version "latest"
# $vmconfig = Set-AzVMSourceImage -VM $vmconfig -PublisherName "Oracle" -Offer "Oracle-Linux" -Skus "7.2" -Version "latest"
# $vmconfig = Set-AzVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Add-AzVMNetworkInterface -VM $vmconfig -Id $nic.Id

$vmconfig = Set-AzVMBootDiagnostics -Disable -VM $vmconfig
$vm = New-AzVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

```powershell
#####
# Create a new virtual machine with a VHD that contains the private image that you want to use
#####
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vmconfig = New-AzVMConfig -VMName SAPERPDemo -VMSize Standard_D11

$vmconfig = Add-AzVMNetworkInterface -VM $vmconfig -Id $nic.Id

$diskName="osfromimage"
$osDiskUri=$account.PrimaryEndpoints.Blob.ToString() + "vhds/" + $diskName  + ".vhd"

$vmconfig = Set-AzVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri <path to VHD that contains the OS image> -Windows
$vmconfig = Set-AzVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred
#$vmconfig = Set-AzVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri <path to VHD that contains the OS image> -Linux
#$vmconfig = Set-AzVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Set-AzVMBootDiagnostics -Disable -VM $vmconfig
$vm = New-AzVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

```powershell
#####
# Create a new virtual machine with a Managed Disk Image
#####
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vmconfig = New-AzVMConfig -VMName SAPERPDemo -VMSize Standard_D11

$vmconfig = Add-AzVMNetworkInterface -VM $vmconfig -Id $nic.Id

$vmconfig = Set-AzVMSourceImage -VM $vmconfig -Id <Id of Managed Disk Image>
$vmconfig = Set-AzVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred
#$vmconfig = Set-AzVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Set-AzVMBootDiagnostics -Disable -VM $vmconfig
$vm = New-AzVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

* İsteğe bağlı olarak ek diskler ekleyin ve gerekli içeriği geri yükleyin. Tüm blob adları (blobs URL'leri) Azure içinde benzersiz olmalıdır.

```powershell
# Optional: Attach additional VHD data disks
$vm = Get-AzVM -ResourceGroupName $rgName -Name SAPERPDemo
$dataDiskUri = $account.PrimaryEndpoints.Blob.ToString() + "vhds/datadisk.vhd"
Add-AzVMDataDisk -VM $vm -Name datadisk -VhdUri $dataDiskUri -DiskSizeInGB 1023 -CreateOption empty | Update-AzVM

# Optional: Attach additional Managed Disks
$vm = Get-AzVM -ResourceGroupName $rgName -Name SAPERPDemo
Add-AzVMDataDisk -VM $vm -Name datadisk -DiskSizeInGB 1023 -CreateOption empty -Lun 0 | Update-AzVM
```

##### <a name="cli"></a>CLI

Aşağıdaki örnek kod Linux'ta kullanılabilir. Windows için, yukarıda açıklandığı gibi PowerShell'i kullanın veya $rgName yerine %rgName kullanmak için örneği uyarlayın ve Windows komut *kümesini*kullanarak ortam değişkenini ayarlayın.

* Her eğitim/demo ortamı için yeni bir kaynak grubu oluşturun

```azurecli
rgName=SAPERPDemo1
rgNameLower=saperpdemo1
az group create --name $rgName --location "North Europe"
```

* Yeni depolama hesabı oluşturma

```azurecli
az storage account create --resource-group $rgName --location "North Europe" --kind Storage --sku Standard_LRS --name $rgNameLower
```

* Aynı ana bilgisayar adı ve IP adreslerinin kullanımını etkinleştirmek için her eğitim/demo ortamı için yeni bir sanal ağ oluşturun. Sanal ağ, Uzak Masaüstü erişimine ve SSH için 22 bağlantı noktasına olanak sağlamak için yalnızca 3389 bağlantı noktasına gelen trafiğin izin verdiği bir Ağ Güvenlik Grubu tarafından korunur.

```azurecli
az network nsg create --resource-group $rgName --location "North Europe" --name SAPERPDemoNSG
az network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGRDP --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 3389 --access Allow --priority 100 --direction Inbound
az network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGSSH --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 22 --access Allow --priority 101 --direction Inbound

az network vnet create --resource-group $rgName --name SAPERPDemoVNet --location "North Europe" --address-prefixes 10.0.1.0/24
az network vnet subnet create --resource-group $rgName --vnet-name SAPERPDemoVNet --name Subnet1 --address-prefix 10.0.1.0/24 --network-security-group SAPERPDemoNSG
```

* Sanal makineye internetten erişmek için kullanılabilecek yeni bir genel IP adresi oluşturma

```azurecli
az network public-ip create --resource-group $rgName --name SAPERPDemoPIP --location "North Europe" --dns-name $rgNameLower --allocation-method Dynamic
```

* Sanal makine için yeni bir ağ arabirimi oluşturma

```azurecli
az network nic create --resource-group $rgName --location "North Europe" --name SAPERPDemoNIC --public-ip-address SAPERPDemoPIP --subnet Subnet1 --vnet-name SAPERPDemoVNet
```

* Sanal makine oluşturur. Bu senaryo için, her VM aynı ada sahip olacaktır. Bu VM'lerde SAP NetWeaver örneklerinin SAP SID'si de aynı olacaktır. Azure Kaynak Grubu içinde, VM adının benzersiz olması gerekir, ancak farklı Azure Kaynak Gruplarında aynı adla VM'ler çalıştırabilirsiniz. Windows'un varsayılan 'Yönetici' hesabı veya Linux için 'root' geçerli değildir. Bu nedenle, yeni bir yönetici kullanıcı adı bir parola ile birlikte tanımlanması gerekir. VM boyutunu da tanımlanması gerekir.

```azurecli
#####
# Create virtual machines using storage accounts
#####
az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image SUSE:SLES-SAP:12-SP1:latest --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --authentication-type password
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image RedHat:RHEL:7.2:latest --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --authentication-type password
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image "Oracle:Oracle-Linux:7.2:latest" --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --authentication-type password

#####
# Create virtual machines using Managed Disks
#####
az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image SUSE:SLES-SAP:12-SP1:latest --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --authentication-type password
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image RedHat:RHEL:7.2:latest --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --authentication-type password
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image "Oracle:Oracle-Linux:7.2:latest" --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --authentication-type password
```

```azurecli
#####
# Create a new virtual machine with a VHD that contains the private image that you want to use
#####
az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --os-type Windows --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --image <path to image vhd>
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --os-type Linux --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --image <path to image vhd> --authentication-type password

#####
# Create a new virtual machine with a Managed Disk Image
#####
az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --image <managed disk image id>
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --image <managed disk image id> --authentication-type password
```

* İsteğe bağlı olarak ek diskler ekleyin ve gerekli içeriği geri yükleyin. Tüm blob adları (blobs URL'leri) Azure içinde benzersiz olmalıdır.

```azurecli
# Optional: Attach additional VHD data disks
az vm unmanaged-disk attach --resource-group $rgName --vm-name SAPERPDemo --size-gb 1023 --vhd-uri https://$rgNameLower.blob.core.windows.net/vhds/data.vhd  --new

# Optional: Attach additional Managed Disks
az vm disk attach --resource-group $rgName --vm-name SAPERPDemo --size-gb 1023 --disk datadisk --new
```

##### <a name="template"></a>Şablon

Örnek şablonları GitHub'daki azure-quickstart şablonları deposunda kullanabilirsiniz.

* [Basit Linux VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
* [Basit Windows VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
* [Görüntüden VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

### <a name="implement-a-set-of-vms-that-communicate-within-azure"></a>Azure içinde iletişim kuran bir vm kümesi uygulama

Bu hibrit olmayan senaryo, demo/eğitim senaryosunu temsil eden yazılımın birden çok VM'ye yayıldığı eğitim ve demo amaçları için tipik bir senaryodur. Farklı VM'lere yüklenen farklı bileşenlerin birbirleriyle iletişim kurması gerekir. Yine, bu senaryoda şirket içi ağ iletişimi veya şirket içi senaryo gerekmez.

Bu senaryo, bu belgenin [SAP NetWeaver demo/eğitim senaryosu ile][planning-guide-7.1] bölüm Tek VM açıklanan yükleme bir uzantısıdır. Bu durumda, varolan bir kaynak grubuna daha fazla sanal makine eklenir. Aşağıdaki örnekte, eğitim ortamı SAP ASCS/SCS VM, DBMS çalıştıran bir VM ve SAP Application Server örneği VM'den oluşur.

Bu senaryoyu oluşturmadan önce, daha önce senaryoda zaten uygulanmış olan temel ayarları düşünmeniz gerekir.

#### <a name="resource-group-and-virtual-machine-naming"></a>Kaynak Grubu ve Sanal Makine adlandırma

Tüm kaynak grubu adları benzersiz olmalıdır. kaynaklarınızın>-soneki gibi `<rg-name` kendi adlandırma düzeninizi geliştirin.

Sanal makine adı kaynak grubu içinde benzersiz olmalıdır.

#### <a name="set-up-network-for-communication-between-the-different-vms"></a>Farklı VM'ler arasındaki iletişim için Ağ ayarlama

![Azure Sanal Ağı'nda VM kümesi][planning-guide-figure-1900]

Aynı eğitim/demo manzaranın klonları ile çarpışmaları önlemek için, her manzara için bir Azure Sanal Ağı oluşturmanız gerekir. DNS ad çözünürlüğü Azure tarafından sağlanacaktır veya Azure dışında kendi DNS sunucunuzu yapılandırabilirsiniz (burada daha fazla tartışılamaz). Bu senaryoda, kendi DNS'mizi yapılandırmayız. Bir Azure Sanal Ağı içindeki tüm sanal makinelerde, ana bilgisayar adları üzerinden iletişim etkinleştirilir.

Eğitim veya demo manzaraları sanal ağlar ve yalnızca kaynak grupları tarafından ayırmak için nedenler aşağıdakiler olabilir:

* Sap manzara olarak kurmak kendi AD / OpenLDAP ve bir Etki Alanı Sunucusu her manzara bir parçası olması gerekir gerekir.  
* Ayarlanan SAP ortamı, sabit IP adresleriyle çalışması gereken bileşenlere sahiptir.

Azure Sanal Ağlar ve bunların nasıl tanımlanabileceği hakkında daha fazla ayrıntı [bu makalede][virtual-networks-create-vnet-arm-pportal]bulunabilir.

## <a name="deploying-sap-vms-with-corporate-network-connectivity-cross-premises"></a>SAP VM'leri Kurumsal Ağ Bağlantısıyla Dağıtma (Binalar Arası)

Bir SAP ortamı çalıştırırsınız ve dağıtımı üst düzey DBMS sunucuları için yalın metal, uygulama katmanları için şirket içi sanallaştırılmış ortamlar ve daha küçük 2 Katmanlı yapılandırılmış SAP sistemleri ve Azure IaaS arasında bölmek istiyorsunuz. Temel varsayım, bir SAP ortamındaki SAP sistemlerinin dağıtım formlarından bağımsız olarak, şirkette dağıtılan diğer yazılım bileşenleriyle ve birbirleriyle iletişim kurması gerektiğidir. Sap GUI veya diğer arabirimlere bağlanan son kullanıcı için dağıtım formu nda da herhangi bir fark olmamalıdır. Bu koşullar ancak siteden siteye/çok siteye bağlantı veya Azure ExpressRoute gibi özel bağlantılar aracılığıyla Azure sistemlerine genişletilen şirket içi Active Directory/OpenLDAP ve DNS hizmetlerini aldığımızda karşılanabilir.



### <a name="scenario-of-an-sap-landscape"></a>SAP manzara senaryosu

Çapraz veya karma senaryo kabaca aşağıdaki grafikte olduğu gibi açıklanabilir:

![Şirket içi ve Azure varlıkları arasında siteden siteye bağlantı][planning-guide-figure-2100]

Minimum gereksinim, tarayıcı erişimi için SSL/TLS veya Azure hizmetlerine sistem erişimi için VPN tabanlı bağlantılar gibi güvenli iletişim protokollerinin kullanılmasıdır. Varsayım, şirketlerin kurumsal ağları ile Azure arasındaki VPN bağlantısını farklı şekilde ele almalarıdır. Bazı şirketler tüm bağlantı noktalarını boş boş açabilir. Bazı diğer şirketler, hangi bağlantı noktalarını açmaları gerektiği vb. kesin olarak bilmek isteyebilir.

Aşağıdaki tabloda tipik SAP iletişim bağlantı noktaları listelenmiştir. Temelde SAP ağ geçidi bağlantı noktasını açmak için yeterlidir.

<!-- sapms is prefix of a SAP service name and not a spelling error -->

| Hizmet | Bağlantı Noktası Adı | Örnek `<nn`> = 01 | Varsayılan Aralık (min-max) | Açıklama |
| --- | --- | --- | --- | --- |
| Dağıtıcı |sapdp`<nn>` bakınız * |3201 |3200 - 3299 |SAP Dispatcher, SAP GUI tarafından Windows ve Java için kullanılır |
| İleti sunucusu |sapms`<sid`> görmek ** |3600 |ücretsiz sapms`<anySID`> |sid = SAP-Sistem-Id |
| Ağ geçidi |sapgw`<nn`> görmek * |3301 |serbest |CPIC ve RFC iletişimi için kullanılan SAP ağ geçidi |
| SAP yönlendirici |sapdp99 |3299 |serbest |Yalnızca CI (merkezi örnek) Hizmet adları yüklemeden sonra /etc/services'da rasgele bir değere yeniden atanabilir. |

*) nn = SAP Örnek Numarası

**) sid = SAP-Sistem-Id

SAP ürünleri tarafından farklı SAP ürünleri veya hizmetleri için gerekli <https://scn.sap.com/docs/DOC-17124>bağlantı noktaları hakkında daha ayrıntılı bilgiye buradan bulabilirsiniz.
Bu belgeyle, belirli SAP ürünleri ve senaryoları için gerekli olan VPN aygıtında özel bağlantı noktalarını açabilmelisin.

Böyle bir senaryoda VM'leri dağıtırken diğer güvenlik önlemleri, erişim kurallarını tanımlamak için bir [Ağ Güvenlik Grubu][virtual-networks-nsg] oluşturmak olabilir.



#### <a name="printing-on-a-local-network-printer-from-sap-instance-in-azure"></a>Azure'daki SAP örneğinden yerel bir ağ yazıcısında yazdırma

##### <a name="printing-over-tcpip-in-cross-premises-scenario"></a>Binalar Arası senaryoda TCP/IP üzerinden yazdırma

Yerleşik TCP/IP tabanlı ağ yazıcılarınızı bir Azure VM'de kurmak, vpn Siteden Siteye tüneliniz veya ExpressRoute bağlantınız olduğunu varsayarsak, şirket ağınızdakiyle genel olarak aynıdır.

---
> ![Windows][Logo_Windows] Windows
>
> Bunu yapmak için:
>
> * Bazı ağ yazıcıları, yazıcınızı azure VM'de ayarlamayı kolaylaştıran bir yapılandırma sihirbazıyla birlikte gelir. Yazıcıyla sihirbaz yazılımı dağıtılmamışsa, yazıcıyı ayarlamanın el ile yolu yeni bir TCP/IP yazıcı bağlantı noktası oluşturmaktır.
> * Açık Kontrol Paneli -> Cihazlar ve Yazıcılar -> Yazıcı Ekleme
> * TCP/IP adresi veya ana bilgisayar adı kullanarak yazıcı eklemeyi seçin
> * Yazıcının IP adresini yazın
> * Yazıcı Bağlantı Noktası standardı 9100
> * Gerekirse uygun yazıcı sürücüsünü el ile takın.
>
> ![Linux][Logo_Linux] Linux
>
> * Windows gibi sadece bir ağ yazıcısı yüklemek için standart yordamı izleyin
> * sadece bir yazıcı eklemek için nasıl [SUSE](https://www.suse.com/documentation/sles-12/book_sle_deployment/data/sec_y2_hw_print.html) veya [Red Hat ve Oracle Linux](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/sec-Printer_Configuration.html) için kamu Linux kılavuzları izleyin.
>
>

---
![Ağ yazdırma][planning-guide-figure-2200]

##### <a name="host-based-printer-over-smb-shared-printer-in-cross-premises-scenario"></a>Binalar Arası senaryoda SMB (paylaşılan yazıcı) üzerinden ana bilgisayar tabanlı yazıcı

Ana bilgisayar tabanlı yazıcılar tasarım ağını uyumlu değildir. Ancak ana bilgisayar tabanlı yazıcı, yazıcı nın açık renkli bir bilgisayara bağlı olması nedeniyle ağdaki bilgisayarlar arasında paylaşılabilir. Şirket ağınızı Site-To-Site veya ExpressRoute'a bağlayın ve yerel yazıcınızı paylaşın. SMB protokolü, ad hizmeti olarak DNS yerine NetBIOS kullanır. NetBIOS ana bilgisayar adı DNS ana bilgisayar adından farklı olabilir. Standart durum, NetBIOS ana bilgisayar adı ve DNS ana bilgisayar adı aynı olmasıdır. DNS etki alanı NetBIOS ad alanında anlamlı değildir. Buna göre, NetBIOS ad alanında DNS ana bilgisayar adı ve DNS etki alanından oluşan tam nitelikli DNS ana bilgisayar adı kullanılmamalıdır.

Yazıcı payı ağdaki benzersiz bir adla tanımlanır:

* Kobİ ana bilgisayar ının ana bilgisayar adı (her zaman gereklidir).
* Payın adı (her zaman gereklidir).
* Yazıcı payı SAP sistemiyle aynı etki alanında değilse etki alanının adı.
* Ayrıca, yazıcı paylaşımına erişmek için bir kullanıcı adı ve parola gerekebilir.

Nasıl yapılır:

---
> ![Windows][Logo_Windows] Windows
>
> Yerel yazıcınızı paylaşın.
> Azure VM'de Windows Gezgini'ni açın ve yazıcının paylaşım adını yazın.
> Bir yazıcı yükleme sihirbazı yükleme işlemi boyunca size rehberlik edecektir.
>
> ![Linux][Logo_Linux] Linux
>
> Aşağıda, Linux'ta ağ yazıcılarının yapılandırılması veya Linux'ta yazdırmayla ilgili bir bölüm dahil olmak üzere bazı belgeler verilmiştir. VM VPN'in bir parçası olduğu sürece Azure Linux VM'de de aynı şekilde çalışacaktır:
>
> * Sles<https://en.opensuse.org/SDB:Printing_via_SMB_(Samba)_Share_or_Windows_Share>
> * RHEL veya Oracle Linux<https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/System_Administrators_Guide/sec-Printer_Configuration.html#s1-printing-smb-printer>
>
>

---
##### <a name="usb-printer-printer-forwarding"></a>USB Yazıcı (yazıcı yönlendirme)

Azure'da Uzak Masaüstü Hizmetleri'nin kullanıcılara uzak bir oturumda yerel yazıcı aygıtlarına erişim sağlama olanağı kullanılamaz.

---
> ![Windows][Logo_Windows] Windows
>
> Windows ile yazdırma hakkında daha fazla <https://technet.microsoft.com/library/jj590748.aspx>bilgi burada bulabilirsiniz: .
>
>

---
#### <a name="integration-of-sap-azure-systems-into-correction-and-transport-system-tms-in-cross-premises"></a>SAP Azure Sistemlerinin Çapraz Tesislerde Düzeltme ve Taşıma Sistemine (TMS) Entegrasyonu

SAP Değişim ve Taşıma Sistemi'nin (TMS) peyzajdaki sistemler arasında taşıma isteğini dışa aktarmak ve almak üzere yapılandırılmalıdır. Bir SAP sisteminin (DEV) geliştirme örneklerinin Azure'da bulunduğunu, kalite güvencesinin (QA) ve üretken sistemlerin (PRD) şirket içinde olduğunu varsayıyoruz. Ayrıca, merkezi bir ulaşım dizini olduğunu varsayıyoruz.

##### <a name="configuring-the-transport-domain"></a>Aktarım Etki Alanını Yapılandırma

Aktarım Etki Alanı Denetleyicisini Yapılandırmada açıklandığı gibi, Aktarım Etki Alanı Denetleyicisi olarak belirlediğiniz sistemde Aktarım Etki [Alanınızı yapılandırın.](https://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm) Bir sistem kullanıcısı TMSADM oluşturulacak ve gerekli RFC hedefi oluşturulacaktır. Bu RFC bağlantılarını SM59 işlemini kullanarak kontrol edebilirsiniz. Ana bilgisayar adı çözümlemesi aktarım etki alanınızda etkinleştirilmelidir.

Nasıl yapılır:

* Senaryomuzda, şirket içi QAS sisteminin CTS etki alanı denetleyicisi olacağına karar verdik. Hareket STMS'yi arayın. TMS iletişim kutusu görüntülenir. Yapıla Aktarım Etki Alanı iletişim kutusu görüntülenir. (Bu iletişim kutusu yalnızca henüz bir aktarım etki alanı yapılandırmadıysanız görüntülenir.)
* Otomatik olarak oluşturulan kullanıcı TMSADM'ın yetki verdiğinden emin olun (SM59 -> ABAP Connection -> TMSADM@E61.DOMAIN_E61 -> Ayrıntılar -> Yardımcı Programlar(M) -> Yetkilendirme Testi). İşlem STMS'nin ilk ekranı, bu SAP Sisteminin artık burada gösterildiği gibi aktarım etki alanının denetleyicisi olarak çalıştığını göstermelidir:

![Etki alanı denetleyicisindeki işlem STMS'sinin ilk ekranı][planning-guide-figure-2300]

#### <a name="including-sap-systems-in-the-transport-domain"></a>Taşıma Alanına SAP Sistemlerini Dahil Etme

Bir aktarım etki alanına sap sistemi dahil etme sırası aşağıdaki gibi görünür:

* Azure'daki DEV sisteminde, aktarım sistemine (İstemci 000) gidin ve hareket STMS'yi arayın. İletişim kutusundan Diğer Yapılandırma'yı seçin ve Etki Alanına Sistemi Dahil et ile devam edin. Etki Alanı Denetleyicisini hedef ana bilgisayar olarak belirtin[(Aktarım Etki Alanı'ndaki SAP Sistemleri dahil).](https://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0c17acc11d1899e0000e829fbbd/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm) Sistem şimdi aktarım etki alanına dahil edilmeyi bekliyor.
* Güvenlik nedenleriyle, isteğinizi onaylamak için etki alanı denetleyicisine geri dönmeniz gerekir. Sistem Genel Bakış'ı seçin ve bekleme sistemini onaylayın. Ardından komut istemini onaylayın ve yapılandırma dağıtılacaktır.

Bu SAP sistemi artık aktarım etki alanında ki diğer tüm SAP sistemleri hakkında gerekli bilgileri içerir. Aynı zamanda, yeni SAP sisteminin adres verileri diğer tüm SAP sistemlerine gönderilir ve SAP sistemi taşıma denetim programının aktarım profiline girilir. RFC'lerin ve etki alanı çalışmasının aktarım dizinine erişim olup olmadığını denetleyin.

[Değişim ve Taşıma Sistemi](https://help.sap.com/saphelp_nw70ehp3/helpdata/en/48/c4300fca5d581ce10000000a42189c/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm)belgelerinde açıklandığı gibi her zamanki gibi taşıma sisteminizin yapılandırması ile devam edin.

Nasıl yapılır:

* Şirket teki STMS'lerinizin doğru şekilde yapılandırıldığından emin olun.
* Transport Domain Controller'ın ana bilgisayar adının Azure'daki sanal makineniz ve yardımcı vize ile çözülediğinden emin olun.
* Çağrı hareketi STMS -> Diğer Yapılandırma -> Etki Alanına Sistemi dahil edin.
* Şirket içi TMS sistemindeki bağlantıyı doğrulayın.
* Aktarım rotalarını, gruplarını ve katmanlarını her zamanki gibi yapılandırın.

Siteden siteye bağlı binalar arası senaryolarda, şirket içi ve Azure arasındaki gecikme hala önemli olabilir. Nesneleri geliştirme ve test sistemleri üzerinden üretime taşıma sırasını takip edersek veya farklı sistemlere taşıma veya destek paketleri uygulamayı düşünürsek, merkezi taşıma dizininin konumuna bağlı olarak, bazı sistemler, merkezi taşıma dizininde yüksek gecikmeli okuma veya yazma verileriyle karşılaşır. Durum, farklı sistemlerin veri merkezleri arasında önemli bir mesafe olan farklı veri merkezleri aracılığıyla yayıldığı SAP peyzaj yapılandırmalarına benzer.

Bu tür gecikme sürelerini aşmak ve sistemlerin taşıma dizinine veya taşıma dizininden okuma veya yazma da hızlı çalışmasını sağlamak için, iki STMS aktarım etki alanı (biri şirket içi, diğeri Azure'daki sistemler için ve aktarım etki alanlarını birbirine bağlamak) ayarlayabilirsiniz. Sap TMS'de bu kavramın arkasındaki ilkeleri açıklayan <https://help.sap.com/saphelp_me60/helpdata/en/c4/6045377b52253de10000009b38f889/content.htm?frameset=/en/57/38dd924eb711d182bf0000e829fbfe/frameset.htm>bu dokümantasyonu lütfen kontrol edin: .

Nasıl yapılır:

* İşlem STMS'lerini kullanarak her konumda (şirket içi ve Azure) bir aktarım etki alanı ayarlama<https://help.sap.com/saphelp_nw70ehp3/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm>
* Etki alanlarını bir etki alanı bağlantısıyla bağlayın ve iki etki alanı arasındaki bağlantıyı onaylayın.
  <https://help.sap.com/saphelp_nw73ehp1/helpdata/en/a3/139838280c4f18e10000009b38f8cf/content.htm>
* Yapılandırmayı bağlantılı sisteme dağıtın.

#### <a name="rfc-traffic-between-sap-instances-located-in-azure-and-on-premises-cross-premises"></a>Azure'da bulunan SAP örnekleri ile şirket içi (Binalar Arası) arasındaki RFC trafiği

Şirket içinde ve Azure'da bulunan sistemler arasındaki RFC trafiğinin çalışması gerekir. Hedef sisteme doğru bir RFC bağlantısı tanımlamanız gereken bir kaynak sistemde bir bağlantı arama hareketi SM59 kurmak için. Yapılandırma, bir RFC Bağlantısının standart kurulumuna benzer.

Tesisler arası senaryoda, birbirleriyle iletişim kurması gereken SAP sistemlerini çalıştıran VM'lerin aynı etki alanında olduğunu varsayıyoruz. Bu nedenle, SAP sistemleri arasında bir RFC bağlantısının kurulumu, şirket içi senaryolarda kurulum adımlarından ve girişlerden farklı değildir.

#### <a name="accessing-local-fileshares-from-sap-instances-located-in-azure-or-vice-versa"></a>Azure'da bulunan SAP örneklerinden yerel dosya paylaşımlarına erişme veya bunun tersi

Azure'da bulunan SAP örneklerinin, şirket bünyesinde bulunan dosya paylaşımlarına erişmeleri gerekir. Ayrıca, şirket içi SAP örneklerinin Azure'da bulunan dosya paylaşımlarına erişmeleri gerekir. Dosya paylaşımlarını etkinleştirmek için, izinleri ve yerel sistemdeki paylaşım seçeneklerini yapılandırmanız gerekir. Azure ve veri merkeziniz arasındaki VPN veya ExpressRoute bağlantısındaki bağlantı noktalarını açtığınızdan emin olun.

## <a name="supportability"></a>Desteklenebilirlik

### <a name="azure-extension-for-sap"></a><a name="6f0a47f3-a289-4090-a053-2521618a28c3"></a>SAP için Azure Uzantısı

Görev açısından kritik SAP sistemlerinin Azure altyapı bilgilerinin bir kısmını VM'lerde yüklü olan SAP Ana Bilgisayar Aracısı örneklerine beslemek için, SAP için bir Azure (VM) Uzantısı'nın dağıtılan VM'ler için yüklenmesi gerekir. SAP'nin talepleri SAP uygulamalarına özgü olduğundan, Microsoft gerekli işlevselliği Azure'a genel olarak uygulamamaya, müşterilerin gerekli VM uzantısını ve yapılandırmalarını sanal makinelerine dağıtmasına bırakmaya karar verdi. Azure'da. Ancak, SAP için Azure VM Uzantısı'nın dağıtımı ve yaşam döngüsü yönetimi çoğunlukla Azure tarafından otomatikleştirilir.

#### <a name="solution-design"></a>Çözüm tasarımı

SAP Host Agent'ın gerekli bilgileri alabilmesi için geliştirilen çözüm, Azure VM Aracısı ve Uzantı çerçevesinin mimarisine dayanır. Azure VM Aracısı ve Uzantı çerçevesinin fikri, Azure VM Uzantı galerisinde bulunan yazılım uygulamalarının(lar) bir VM içinde yüklenmesine izin vermektir. Bu kavramın arkasındaki temel fikir, (SAP için Azure Uzantısı gibi durumlarda), özel işlevlerin VM'ye dağıtılmasına ve dağıtım sırasında bu yazılımın yapılandırmasına izin vermektir.

VM içindeki belirli Azure VM Uzantılarının işlenmesini sağlayan 'Azure VM Aracısı', Azure portalında VM oluşturma da varsayılan olarak Windows VM'lere enjekte edilir. SUSE, Red Hat veya Oracle Linux durumunda, VM aracısı zaten Azure Marketi görüntüsünün bir parçasıdır. Durumda, bir Linux VM'yi şirket içi Azure'a yükler, VM aracısının el ile yüklenmesi gerekir.

Azure'daki SAP Ana Bilgisayar aracısına Azure altyapı bilgileri sağlamak için çözümün temel yapı taşları aşağıdaki gibi görünür:

![Microsoft Azure Uzantısı bileşenleri][planning-guide-figure-2400]

Yukarıdaki blok diyagramında gösterildiği gibi, çözümün bir bölümü Azure İşlemleri tarafından yönetilen genel olarak çoğaltılan bir depo olan Azure VM Image ve Azure Uzantısı Galerisi'nde barındırılır. SAP için Azure Uzantısı'nın yeni sürümlerini yayımlamak için Azure İşlemleri ile çalışmak SAP'nin Azure uygulaması üzerinde çalışan ortak SAP/MS ekibinin sorumluluğundadır.

Yeni bir Windows VM dağıttığınızda, Azure VM Aracısı otomatik olarak VM'ye eklenir. Bu aracının işlevi, VM'lerin Azure Uzantılarının yüklenmesi ve yapılandırmasını koordine etmektir. Azure VM Aracısı, Linux VM'leri için Azure Marketi işletim sistemi imajının zaten bir parçasıdır.

Ancak, yine de müşteri tarafından yürütülmesi gereken bir adım vardır. Bu, performans koleksiyonunun etkinleştirme ve yapılandırmasıdır. Yapılandırmayla ilgili işlem PowerShell komut dosyası veya CLI komutu yla otomatikleştirilir. PowerShell komut [dosyası, Dağıtım Kılavuzu'nda][deployment-guide]açıklandığı gibi Microsoft Azure Komut Dosyası Merkezi'nden indirilebilir.

SAP için Azure uzantısının genel Mimarisi aşağıdaki gibi görünür:

![SAP için Azure uzantısı ][planning-guide-figure-2500]

**Dağıtımlar sırasında bu PowerShell cmdlets veya CLI komutunu kullanmanın tam nasıl ve ayrıntılı adımları için [Dağıtım Kılavuzu'nda][deployment-guide]verilen yönergeleri izleyin.**

### <a name="integration-of-azure-located-sap-instance-into-saprouter"></a>Azure'un sap örneğinin SAProuter'a entegrasyonu

Azure'da çalışan SAP örneklerine SAProuter'dan da erişilebiliyor olması gerekir.

![SAP-Router Ağ Bağlantısı][planning-guide-figure-2600]

Doğrudan IP bağlantısı yoksa, SAProuter katılımcı sistemler arasında TCP/IP iletişimini sağlar. Bu, iletişim ortakları arasında ağ düzeyinde uçlardan uca bağlantı nın gerekli olmadığı avantajını sağlar. SAProuter varsayılan olarak port 3299 dinliyor.
SAP örneklerini bir SAProuter üzerinden bağlamak için, saprouter dizesini ve ana bilgisayar adını bağlanma ya da bağlanma girişimiyle vermeniz gerekir.

## <a name="sap-netweaver-as-java"></a>SAP NetWeaver AS Java

Şimdiye kadar belgenin odak sap NetWeaver genel olarak veya SAP NetWeaver ABAP yığını olmuştur. Bu küçük bölümde, SAP Java yığını için belirli hususlar listelenir. En önemli SAP NetWeaver Java sadece tabanlı uygulamalardan biri SAP Enterprise Portal olduğunu. SAP PI ve SAP Solution Manager gibi diğer SAP NetWeaver tabanlı uygulamalar hem SAP NetWeaver ABAP hem de Java yığınlarını kullanır. Bu nedenle, kesinlikle sap NetWeaver Java yığını ile ilgili belirli yönlerini de dikkate almak için bir ihtiyaç vardır.

### <a name="sap-enterprise-portal"></a>SAP Kurumsal Portalı

Bir Azure Sanal Makine'de sap Portalı'nın kurulumu, binalar arası senaryolarda dağıtım yapıyorsanız şirket içi yüklemeden farklı değildir. DNS şirket içinde yapıldığından, tek tek örneklerin bağlantı noktası ayarları şirket içinde yapılandırıldıkça yapılabilir. Bu belgede şimdiye kadar açıklanan öneriler ve kısıtlamalar SAP Enterprise Portal veya genel olarak SAP NetWeaver Java yığını gibi bir uygulama için geçerlidir.

![Maruz SAP Portalı][planning-guide-figure-2700]

Sanal makine ana bilgisayar siteden siteye VPN tüneli veya ExpressRoute üzerinden şirket ağına bağlı iken bazı müşteriler tarafından özel bir dağıtım senaryosu Internet'e SAP Enterprise Portal doğrudan maruz olduğunu. Böyle bir senaryo için, belirli bağlantı noktalarının açık olduğundan ve güvenlik duvarı veya ağ güvenlik grubu tarafından engellenmediğinden emin olabilirsiniz. 

İlk portal URI http(ler):`<Portalserver`>:5XX00/irj bağlantı noktası SAP tarafından <https://help.sap.com/saphelp_nw70ehp1/helpdata/de/a2/f9d7fed2adc340ab462ae159d19509/frameset.htm>belgelenmiş olarak oluşur.

![Bitiş noktası yapılandırması][planning-guide-figure-2800]

SAP Kurumsal Portalınızın URL'sini ve/veya bağlantı noktalarını özelleştirmek istiyorsanız, aşağıdaki belgeleri kontrol edin:

* [Portal URL'sini Değiştir](https://wiki.scn.sap.com/wiki/display/EP/Change+Portal+URL)
* [Varsayılan bağlantı noktası numaralarını, Portal bağlantı noktası numaralarını değiştirme](https://wiki.scn.sap.com/wiki/display/NWTech/Change+Default++port+numbers%2C+Portal+port+numbers)

## <a name="high-availability-ha-and-disaster-recovery-dr-for-sap-netweaver-running-on-azure-virtual-machines"></a>Azure Sanal Makinelerde çalışan SAP NetWeaver için Yüksek Kullanılabilirlik (HA) ve Olağanüstü Durum Kurtarma (DR)

### <a name="definition-of-terminologies"></a>Terminolojilerin tanımı

Yüksek **kullanılabilirlik (HA)** terimi genellikle, **aynı** veri merkezi içinde gereksiz, hataya dayanıklı veya başarısız korumalı bileşenler aracılığıyla BT hizmetlerinin iş sürekliliğini sağlayarak BT kesintilerini en aza indiren bir dizi teknolojiyle ilişkilidir. Bizim durumumuzda, tek bir Azure Bölgesi içinde.

**Olağanüstü durum kurtarma (DR)** da BT hizmetleri kesinti en aza indirmek ve kurtarma ama **farklı** veri merkezleri arasında, genellikle yüzlerce kilometre uzaklıkta bulunan hedefliyor. Bizim durumumuzda genellikle aynı jeopolitik bölge içinde veya müşteri olarak sizin oluşturduğunuz farklı Azure Bölgeleri arasında.

### <a name="overview-of-high-availability"></a>Yüksek Kullanılabilirlik Genel Bakış

Azure'daki SAP yüksek kullanılabilirliği hakkındaki tartışmayı iki bölüme ayırabiliriz:

* **Azure altyapısı yüksek kullanılabilirlik**, örneğin HA bilgi işlem (VM), ağ, depolama vb ve SAP uygulama kullanılabilirliğini artırmak için faydaları.
* **SAP uygulaması yüksek kullanılabilirlik**, örneğin SAP yazılım bileşenlerinin HA:
  * SAP uygulama sunucuları
  * SAP ASCS/SCS örneği
  * DB sunucusu

ve nasıl Azure altyapı HA ile kombine edilebilir.

Azure'daki SAP Yüksek Kullanılabilirlik, şirket içi fiziksel veya sanal ortamda SAP Yüksek Kullanılabilirliği ile karşılaştırıldığında bazı farklılıklara sahiptir. SAP'nin aşağıdaki makalesinde, Windows'daki sanallaştırılmış ortamlardaki <https://scn.sap.com/docs/DOC-44415>standart SAP Yüksek Kullanılabilirlik yapılandırmaları açıklanmaktadır: . Linux için Windows için var olduğu gibi sapinst-integrated SAP-HA yapılandırması yoktur. Linux için SAP HA on-premises ile <https://scn.sap.com/docs/DOC-8541>ilgili olarak burada daha fazla bilgi bulabilirsiniz: .

### <a name="azure-infrastructure-high-availability"></a>Azure Altyapısı Yüksek Kullanılabilirlik

Şu anda %99,9'luk tek VM SLA bulunmaktadır. Tek bir VM'nin kullanılabilirliğinin nasıl görünebileceği hakkında bir fikir edinmek için, <https://azure.microsoft.com/support/legal/sla/>kullanılabilir farklı Azure SLA'larının ürünlerini oluşturabilirsiniz: .

Hesaplamanın temeli ayda 30 gün veya 43200 dakikadır. Bu nedenle, %0,05'lik kapalı kalma süresi 21,6 dakikaya karşılık gelir. Her zamanki gibi, farklı hizmetlerin kullanılabilirliği aşağıdaki şekilde çarpacaktır:

(Kullanılabilirlik Hizmeti #1/100) * (Kullanılabilirlik Hizmeti #2/100) * (Kullanılabilirlik Hizmeti #3/100) 

Gibi:

(99.95/100) * (99.9/100) * (99.9/100) = 0.9975 veya genel durumu %99.75.

#### <a name="virtual-machine-vm-high-availability"></a>Sanal Makine (VM) Yüksek Kullanılabilirlik

Sanal makinelerinizin kullanılabilirliğini etkileyebilecek iki tür Azure platformu olayı vardır: planlı bakım ve planlanmamış bakım.

* Planlanan bakım etkinlikleri, Microsoft tarafından temel Azure platformunda sanal makinelerinizin çalıştırdığı platform altyapısının genel güvenilirliğini, performansını ve güvenliğini artırmak için yapılan periyodik güncelleştirmelerdir.
* Plansız bakım olayları, sanal makinenizin altında yatan donanım ya da fiziksel altyapı bir şekilde arıza yaptığında gerçekleştirilir. Buna yerel ağ hataları, yerel disk hataları veya raf düzeyinde diğer hatalar dahildir. Böyle bir hata algılandığında, Azure platformu sanal makinenizi barındıran sağlıksız fiziksel sunucudan sanal makinenizi otomatik olarak sağlıklı bir fiziksel sunucuya aktaracaktır. Bu tür olaylar nadirdir, ancak sanal makinenizin yeniden başlatılmasına da neden olabilir.

Bu belgelerde daha fazla ayrıntı bulunabilir:<https://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>

#### <a name="azure-storage-redundancy"></a>Azure Depolama Artıklığı

Microsoft Azure Depolama Hesabınızdaki veriler, geçici donanım hataları karşısında bile Azure Depolama SLA'sını karşılayacak dayanıklılık ve yüksek kullanılabilirlik sağlamak için her zaman çoğaltılır.

Azure Depolama varsayılan olarak verilerin üç görüntüsünü tuttuğundan, raid5 veya RAID1 birden çok Azure diskinde gerekli değildir.

Daha fazla ayrıntı bu makalede bulunabilir:<https://azure.microsoft.com/documentation/articles/storage-redundancy/>

#### <a name="utilizing-azure-infrastructure-vm-restart-to-achieve-higher-availability-of-sap-applications"></a>SAP Uygulamalarının Daha Yüksek Kullanılabilirliğini Elde Etmek Için Azure Altyapısı VM Yeniden Başlatma'yı Kullanma

Linux'ta Windows Server Failover Clustering (WSFC) veya Pacemaker gibi işlevleri kullanmamaya karar verirseniz (şu anda yalnızca SLES 12 ve üzeri için desteklenir), Azure VM Restart bir SAP Sistemini Azure'un planlı ve planlanmamış kapalı kalma sürelerine karşı korumak için kullanılır fiziksel sunucu altyapısı ve genel temel Azure platformu.

> [!NOTE]
> Azure VM Restart'ın öncelikle VM'leri ve NOT uygulamalarını koruduğundan bahsetmek önemlidir. VM Restart SAP uygulamaları için yüksek kullanılabilirlik sunmaz, ancak belirli bir düzeyde altyapı kullanılabilirliği ve bu nedenle sap sistemlerinin dolaylı olarak daha yüksek kullanılabilirliğini sunar. Planlı veya planlanmamış bir ana bilgisayar kesintisinden sonra VM'yi yeniden başlatmak için gereken süre için sla da yoktur. Bu nedenle, bu yüksek kullanılabilirlik yöntemi (A)SCS veya DBMS gibi bir SAP sisteminin kritik bileşenleri için uygun değildir.
>
>

Yüksek kullanılabilirlik için bir diğer önemli altyapı öğesi depolamadır. Örneğin Azure Depolama SLA% 99,9 kullanılabilirlik olduğunu. Biri tüm Sanal Sanal Bilgileri diskleriyle tek bir Azure Depolama Hesabına dağıtırsa, potansiyel Azure Depolama kullanılabilirliği, o Azure Depolama Hesabı'na yerleştirilen tüm VM'lerin ve bu Sanal M'lerin içinde çalışan tüm SAP bileşenlerinin kullanılamamasına neden olur.  

Tüm VM'leri tek bir Azure Depolama Hesabına koymak yerine, her VM için özel depolama hesaplarını da kullanabilirsiniz ve bu şekilde birden çok bağımsız Azure Depolama Hesabı kullanarak genel VM ve SAP uygulama kullanılabilirliğini artırabilirsiniz.

Azure Yönetilen Diskler, bağlı oldukları sanal makinenin Hata Etki Alanı'na otomatik olarak yerleştirilir. İki sanal makineyi bir kullanılabilirlik kümesine yerve Yönetilen Diskler kullanırsanız, platform Yönetilen Diskleri farklı Fay Etki Alanları'na dağıtmayı da halleder. Premium Depolama'yı kullanmayı planlıyorsanız, Diskleri Yönet'i de kullanmanızı şiddetle öneririz.

Azure altyapı HA ve depolama hesaplarını kullanan bir SAP NetWeaver sisteminin örnek mimarisi şu na benzer:

![SAP uygulamasına daha fazla kullanılabilirlik sağlamak için Azure altyapısı HA'sını kullanma][planning-guide-figure-2900]

Azure altyapısı HA ve Yönetilen Diskler kullanan bir SAP NetWeaver sisteminin örnek mimarisi şu na benzer:

![SAP uygulamasına daha fazla kullanılabilirlik sağlamak için Azure altyapısı HA'sını kullanma][planning-guide-figure-2901]

Kritik SAP bileşenleri için şimdiye kadar aşağıdakileri elde ettik:

* SAP Uygulama Sunucularının Yüksek Kullanılabilirliği (AS)

  SAP uygulama sunucusu örnekleri gereksiz bileşenlerdir. Her SAP AS örneği, farklı bir Azure Hatası ve Yükseltme Etki Alanında çalışan kendi VM'sinde dağıtılır (bkz. bölüm [Hata Etki Alanları][planning-guide-3.2.1] ve Yükseltme Etki [Alanları).][planning-guide-3.2.2] Bu, Azure kullanılabilirlik kümeleri kullanılarak sağlanır (bkz. bölüm [Azure Kullanılabilirlik Kümeleri).][planning-guide-3.2.3] Bir Azure Hatası veya Yükseltme Etki Alanının planlı veya planlanmamış kullanılabilirliği, SAP AS örnekleriyle sınırlı sayıda VM'nin kullanılamamasına neden olur.

  Her SAP AS örneği kendi Azure Depolama hesabına yerleştirilir - bir Azure Depolama Hesabının kullanılabilirliği olası değildir, SAP AS örneğinde yalnızca bir VM'nin kullanılamamasına neden olur. Ancak, tek bir Azure aboneliğinde Azure Depolama Hesapları'nın bir sınırı olduğunu unutmayın. VM yeniden başlatmadan sonra (A)SCS örneğinin otomatik olarak başlatılmasını sağlamak [için, SAP örnekleri için Otomatik Başlatma'yı kullanma][planning-guide-11.5]bölümünde açıklanan (A)SCS örnek başlangıç profilinde Otomatik başlatma parametresini ayarladığınızdan emin olun.
  Daha fazla bilgi [için lütfen SAP Uygulama Sunucuları için Yüksek Kullanılabilirlik][planning-guide-11.4.1] bölümünü okuyun.

  Yönetilen Diskler kullanıyor olsanız bile, bu diskler bir Azure Depolama Hesabında da depolanır ve depolama kesintisi durumunda kullanılamaz.

* *Daha yüksek* SAP (A)SCS örneğinin kullanılabilirliği

  Burada Yüklü SAP (A)SCS örneği ile VM'yi korumak için Azure VM Restart'ı kullanıyoruz. Azure severs'in planlı veya planlanmamış kapalı kalma süresi söz konusu olduğunda, VM'ler kullanılabilir başka bir sunucuda yeniden başlatılır. Daha önce de belirtildiği gibi, Azure VM Restart öncelikle VM'leri ve NOT uygulamalarını korur, bu durumda (A)SCS örneğini. VM Restart sayesinde, SAP (A)SCS örneğinin dolaylı olarak daha yüksek kullanılabilirliğine ulaşacağız. VM yeniden başlatmadan sonra (A)SCS örneğinin otomatik olarak başlatılmasını sigortalamak [için, SAP örnekleri için Otomatik Başlatma'yı kullanma][planning-guide-11.5]bölümünde açıklanan (A)SCS örnek başlangıç profilinde Otomatik başlatma parametresini ayarladığınızdan emin olun. Bu, tek bir VM'de çalışan Tek Bir Hata Noktası (SPOF) olarak (A)SCS örneğinin tüm SAP ortamının kullanılabilirliği için belirleyici faktör olacağı anlamına gelir.

* *Daha yüksek* DBMS Sunucusunun Kullanılabilirliği

  Burada, SAP (A)SCS örnek kullanım örneğine benzer şekilde, VM'yi yüklü DBMS yazılımıyla korumak için Azure VM Restart'ı kullanıyoruz ve VM Restart aracılığıyla DBMS yazılımının daha yüksek kullanılabilirliğini elde ediyoruz.
  Tek bir VM'de çalışan DBMS aynı zamanda bir SPOF'tur ve tüm SAP manzarasının kullanılabilirliği için belirleyici bir faktördür.

### <a name="sap-application-high-availability-on-azure-iaas"></a>Azure IaaS'ta SAP Uygulaması Yüksek Kullanılabilirlik

Tam SAP sistemi yüksek kullanılabilirlik elde etmek için, tüm kritik SAP sistem bileşenlerini (örneğin gereksiz SAP uygulama sunucuları ve SAP (A)SCS örneği ve DBMS gibi benzersiz hatalar (Örneğin Tek Hata Noktası) korumamız gerekir.

#### <a name="high-availability-for-sap-application-servers"></a><a name="5d9d36f9-9058-435d-8367-5ad05f00de77"></a>SAP Uygulama Sunucuları için Yüksek Kullanılabilirlik

SAP uygulama sunucuları/iletişim örnekleri için belirli bir yüksek kullanılabilirlik çözümü hakkında düşünmenız gerekmez. Yüksek kullanılabilirlik artıklık ve böyle farklı sanal makinelerde yeterli sahip elde edilir. Bunların tümü, vm'lerin planlanan bakım kapalı kalma süresi sırasında aynı anda güncelleştirilemelerini önlemek için aynı Azure kullanılabilirliği kümesine yerleştirilmelidir. Azure Ölçeği Birimi içindeki farklı Yükseltme ve Hata Etki Alanları'nı temel alan temel işlev, Yükseltme [Etki Alanları][planning-guide-3.2.2]bölümünde zaten tanıtıldı. Azure kullanılabilirlik kümeleri, bu belgenin Azure [Kullanılabilirlik Kümeleri][planning-guide-3.2.3] bölümünde sunulmuştur.

Azure Ölçeği Birimi içinde ayarlanan bir Azure kullanılabilirliği tarafından kullanılabilecek sonsuz sayıda Hata ve Yükseltme Etki Alanı yoktur. Bu, er ya da geç birden fazla VM aynı Hata veya Yükseltme Etki Alanı biter, bir kullanılabilirlik kümesi içine VM bir dizi koyarak anlamına gelir.

Özel VM'lerinde birkaç SAP uygulama sunucusu örneği dağıtılır ve beş Yükseltme Etki Alanı aldığımızı varsayarsak, sonunda aşağıdaki resim ortaya çıkar. Kullanılabilirlik kümesindeki gerçek maksimum hata sayısı ve güncelleştiretki alanları gelecekte değişebilir:

![Azure'daki SAP Uygulama Sunucularının HA'sı][planning-guide-figure-3000]

Bu belgelerde daha fazla ayrıntı bulunabilir:<https://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>

#### <a name="high-availability-for-sap-central-services-on-azure"></a>Azure'da SAP Merkezi Hizmetleri için Yüksek Kullanılabilirlik

Azure'daki SAP Central Services'ın yüksek kullanılabilirlik mimarisi için, giriş bilgileri olarak [SAP NetWeaver makalesiyüksek kullanılabilirlik mimarisini ve senaryoları](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-architecture-scenarios) kontrol edin. Makale, belirli işletim sistemleri için daha ayrıntılı açıklamalara işaret eder.

#### <a name="high-availability-for-the-sap-database-instance"></a>SAP veritabanı örneği için yüksek kullanılabilirlik

Tipik SAP DBMS HA kurulumu, DBMS yüksek kullanılabilirlik işlevinin etkin DBMS örneğinden ikinci VM'ye kadar olan verileri pasif bir DBMS örneğine çoğaltmak için kullanıldığı iki DBMS VM'sine dayanır.

Genel olarak DBMS için Yüksek Kullanılabilirlik ve Olağanüstü Durum kurtarma işlevleri nin yanı sıra belirli DBMS' ler [DBMS Dağıtım Kılavuzu'nda][dbms-guide]açıklanmıştır.

#### <a name="end-to-end-high-availability-for-the-complete-sap-system"></a>Komple SAP Sistemi için Uçdan Uca Yüksek Kullanılabilirlik

Aşağıda Azure'daki tam bir SAP NetWeaver HA mimarisine iki örnek verilmiştir - biri Windows için, diğeri Linux için.

Yalnızca yönetilmeyen diskler: Birçok SAP sistemi dağıttığınızda ve dağıtılan VM sayısı abonelik başına maksimum Depolama Hesabı sınırını aştığında, aşağıda açıklandığı gibi kavramların biraz tehlikeye alınması gerekebilir. Bu gibi durumlarda, VM'lerin VHD'lerinin tek bir Depolama Hesabı içinde birleştirilmesi gerekir. Genellikle bunu, farklı SAP sistemlerinin SAP uygulama katmanı VM'lerinin VHD'lerini birleştirerek yaparsınız.  Ayrıca, farklı SAP sistemlerinin farklı DBMS VM'lerinin farklı VHD'lerini bir Azure Depolama Hesabında birleştirdik. Bu şekilde Azure Depolama Hesaplarının IOPS<https://azure.microsoft.com/documentation/articles/storage-scalability-targets>sınırlarını göz önünde bulundurun ( )


##### <a name="windowslogo_windows-ha-on-windows"></a>![Windows][Logo_Windows] Windows'da HA

![Azure IaaS'da SQL Server ile SAP NetWeaver Uygulaması HA Mimarisi][planning-guide-figure-3200]

Aşağıdaki Azure yapıları, altyapı sorunları ve ana bilgisayar düzeltme eki etkisini en aza indirmek için SAP NetWeaver sistemi için kullanılır:

* Tüm sistem Azure'da dağıtılır (gerekli - DBMS katmanı, (A)SCS örneği ve tam uygulama katmanının aynı konumda çalışması gerekir).
* Tüm sistem tek bir Azure aboneliği içinde çalışır (gereklidir).
* Tüm sistem tek bir Azure Sanal Ağı içinde çalışır (gereklidir).
* Bir SAP sisteminin VM'lerinin üç kullanılabilirlik kümesine ayrılması, aynı Sanal Ağa ait tüm VM'lerde bile mümkündür.
* Her katman (örneğin DBMS, ASCS, Uygulama Sunucuları) özel bir kullanılabilirlik kümesi kullanmalıdır.
* Bir SAP sisteminin DBMS örneklerini çalıştıran tüm VM'ler tek bir kullanılabilirlik kümesindedir. SQL Server AlwaysOn veya Oracle Data Guard gibi yerel DBMS yüksek kullanılabilirlik özellikleri kullanıldığından, sistem başına birden fazla VM çalıştıran DBMS örneği olduğunu varsayıyoruz.
* DBMS örneklerini çalıştıran tüm VM'ler kendi depolama hesaplarını kullanır. DBMS verileri ve günlük dosyaları, verileri senkronize eden DBMS yüksek kullanılabilirlik işlevleri kullanılarak bir depolama hesabından başka bir depolama hesabına kopyalanır. Bir depolama hesabının kullanılamaması, bir SQL Windows küme düğümünün kullanılamamasına neden olur, ancak tüm SQL Server hizmetine neden olmaz.
* Bir SAP sisteminin (A)SCS örneğini çalıştıran tüm VM'ler tek bir kullanılabilirlik kümesindedir. Windows Server Failover Cluster (WSFC), (A)SCS örneğini korumak için bu VM'lerin içinde yapılandırılır.
* (A)SCS örneklerini çalıştıran tüm VM'ler kendi depolama hesaplarını kullanır. (A) SCS örnek dosyaları ve SAP global klasörü, SIOS DataKeeper çoğaltma kullanılarak bir depolama hesabından başka bir depolama hesabına kopyalanır. Bir depolama hesabının kullanılamaması, bir (A)SCS Windows küme düğümünün kullanılamamasına, ancak (A)SCS hizmetinin tamamının olmamasına neden olur.
* SAP uygulama sunucusu katmanını temsil eden TÜM VM'ler üçüncü kullanılabilirlik kümesindedir.
* SAP uygulama sunucularını çalıştıran TÜM VM'ler kendi depolama hesaplarını kullanır. Bir depolama hesabının kullanılamaması, diğer SAP uygulama sunucularının çalışmaya devam ettiği bir SAP uygulama sunucusunun kullanılamamasına neden olur.

Aşağıdaki şekil Yönetilen Diskler kullanarak aynı manzara yı gösterilmiştir.

![Azure IaaS'da SQL Server ile SAP NetWeaver Uygulaması HA Mimarisi][planning-guide-figure-3201]

##### <a name="linuxlogo_linux-ha-on-linux"></a>![Linux][Logo_Linux] Linux üzerinde HA

Azure'da Linux'ta SAP HA mimarisi temelde yukarıda açıklandığı gibi Windows için aynıdır. Desteklenen yüksek kullanılabilirlik çözümleri listesi için SAP Note [1928533'e] bakın.

### <a name="using-autostart-for-sap-instances"></a><a name="4e165b58-74ca-474f-a7f4-5e695a93204f"></a>SAP örnekleri için Otomatik Başlatma'yı kullanma

SAP, VM içinde işletim sistemi başladıktan hemen sonra SAP örneklerini başlatmak için işlevsellik sundu. Kesin adımlar SAP Bilgi Bankası Madde [1909114]belgelenmiştir. Ancak, sap, birden fazla VM'nin etkilendiğini veya VM başına birden çok örneğin çalıştırıldığı varsayarak, örnek yeniden başlatma sırasına göre denetim olmadığı için ayarı artık kullanmanızı önermez. VM'de bir SAP uygulama sunucusu örneğinin tipik bir Azure senaryosu ve sonunda tek bir VM'nin yeniden başlatılması durumunda, Otomatik Başlatma kritik değildir ve bu parametre eklenerek etkinleştirilebilir:

    Autostart = 1

SAP ABAP ve/veya Java örneğinin başlangıç profiline.

> [!NOTE]
> Autostart parametresi de bazı düşüşler olabilir. Daha ayrıntılı olarak, parametre, örneğin ilgili Windows/Linux hizmeti başlatıldığında bir SAP ABAP veya Java örneğinin başlatılmasını tetikler. Bu kesinlikle işletim sistemi kadar önyükleme durumdur. Ancak, SAP hizmetlerinin yeniden başlatılması, SUM veya diğer güncelleştirmeler veya yükseltmeler gibi SAP Yazılım Yaşam Döngüsü Yönetimi işlevleri için de yaygın bir durumdur. Bu işlevler, bir örneğin otomatik olarak yeniden başlatılmasını beklemiyor. Bu nedenle, otomatik başlatma parametresi bu tür görevleri çalıştırmadan önce devre dışı bırakılmalıdır. Otomatik başlatma parametresi, ASCS/SCS/CI gibi kümelenmiş SAP örnekleri için de kullanılmamalıdır.
>
>

SAP örnekleri için otomatik başlatma ile ilgili ek bilgilere buradan bakın:

* [Unix Server Start/Stop'unizle birlikte SAP'yi başlat/Durdur](https://scn.sap.com/community/unix/blog/2012/08/07/startstop-sap-along-with-your-unix-server-startstop)
* [SAP NetWeaver Yönetim Aracılarının Başlatılması ve Durdurulması](https://help.sap.com/saphelp_nwpi711/helpdata/en/49/9a15525b20423ee10000000a421938/content.htm)
* [HANA Veritabanıotomatik Başlatma nasıl etkinleştirilir](http://sapbasisinfo.com/blog/2016/08/15/enabling-autostart-of-sap-hana-database-on-server-boot-situation/)

### <a name="larger-3-tier-sap-systems"></a>Daha büyük 3 Katmanlı SAP sistemleri
3 Katmanlı SAP yapılandırmalarının yüksek kullanılabilirlik yönleri daha önceki bölümlerde tartışıldı. Peki ya DBMS sunucu gereksinimlerinin Azure'da bulunamayacak kadar büyük olduğu, ancak SAP uygulama katmanının Azure'a dağıtılabileceği sistemlere ne denene ne olacak?

#### <a name="location-of-3-tier-sap-configurations"></a>3 Katmanlı SAP yapılandırmalarının konumu
Uygulama katmanının kendisini veya uygulama ve DBMS katmanını şirket içi ve Azure arasında bölmek desteklenmez. Bir SAP sistemi şirket içinde veya Azure'da tamamen dağıtılır. Ayrıca, bazı uygulama sunucularının şirket içinde, bazılarının da Azure'da çalıştırılması desteklenmez. Tartışmanın başlangıç noktası bu. Ayrıca, bir SAP sisteminin DBMS bileşenlerinin ve SAP uygulama sunucusu katmanının iki farklı Azure Bölgesinde dağıtılmasını da desteklemeyiz. Örneğin, Batı ABD'deki DBMS ve Orta ABD'deki SAP uygulama katmanı. Bu tür yapılandırmaları desteklememe nedeni SAP NetWeaver mimarisinin gecikme duyarlılığıdır.

Ancak, geçen yıl boyunca veri merkezi ortakları Azure Bölgeleri için ortak konumlar geliştirdi. Bu ortak konumlar genellikle bir Azure Bölgesi'ndeki fiziksel Azure veri merkezlerine yakındır. ExpressRoute aracılığıyla ortak konumdaki varlıkların kısa mesafe ve Azure'a bağlantısı, 2 m'den kısa bir gecikmeye neden olabilir. Bu gibi durumlarda, DBMS katmanını (depolama SAN/NAS dahil) bu ortak konumda bulmak ve Azure'daki SAP uygulama katmanı mümkündür. [HANA Büyük Örnekleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture). 

### <a name="offline-backup-of-sap-systems"></a>SAP sistemlerinin Çevrimdışı Yedeklemesi
Seçilen SAP yapılandırmasına bağlı olarak (2 Katmanlı veya 3 Katmanlı) yedeklemeye gerek olabilir. VM kendisi içeriği artı veritabanının bir yedekleme olması. DBMS ile ilgili yedeklemelerin veritabanı yöntemleriyle yapılması beklenmektedir. Farklı veritabanları için ayrıntılı bir açıklama, [DBMS Kılavuzu'nda][dbms-guide]bulunabilir. Diğer taraftan, SAP verileri, bir sonraki bölümde açıklandığı şekilde bu bölümde veya çevrimiçi olarak açıklandığı şekilde çevrimdışı bir şekilde (veritabanı içeriği de dahil olmak üzere) yedeklenebilir.

Çevrimdışı yedekleme temelde Azure portalı üzerinden VM kapatılması ve temel VM diskinin bir kopyasını niçin VM'ye bağlı tüm diskler gerektirir. Bu VM ve ilişkili disk zaman görüntüsü bir nokta koruyacak. Yedeklerin farklı bir Azure Depolama Hesabına kopyalanması önerilir. Bu nedenle, bu belgenin [Azure Depolama Hesapları arasındaki diskleri][planning-guide-5.4.2] bölüm kopyalama bölümünde açıklanan yordam geçerli olacaktır.
Azure portalını kullanarak kapatmanın yanı sıra, burada açıklandığı gibi Powershell veya CLI üzerinden de yapabilirsiniz:<https://azure.microsoft.com/documentation/articles/virtual-machines-deploy-rmtemplates-powershell/>

Bu durum geri yükleme, temel VM'nin yanı sıra temel VM'nin ve monte edilmiş disklerin orijinal disklerinin silmesi, kaydedilen disklerin yönetilen diskler için özgün Depolama Hesabı veya kaynak grubuna kopyalanması ve ardından sistemin yeniden dağıtılmasından oluşur.
Bu makalede, PowerShell'de bu işlemin nasıl yazılalı yazılanınca yada bir örnek gösterilmektedir:<http://www.westerndevs.com/azure-snapshots/>

Yukarıda açıklandığı gibi bir VM yedeklemesini geri yüklemek yeni bir donanım anahtarı oluşturduğundan lütfen yeni bir SAP lisansı yüklediğinizden emin olun.

### <a name="online-backup-of-an-sap-system"></a>SAP sisteminin çevrimiçi yedeklemesi

DBMS yedekleme [DBMS Kılavuzu'nda][dbms-guide]açıklandığı gibi DBMS özgü yöntemleri ile gerçekleştirilir.

SAP sistemindeki diğer SANAL M'ler Azure Sanal Makine Yedekleme işlevi kullanılarak yedeklenebilir. Azure Sanal Makine Yedekleme, Azure'da tam bir VM yedeklemek için standart bir yöntemdir. Azure Yedekleme yedeklemeleri Azure'da saklar ve yeniden VM geri yüklemesine olanak tanır.

> [!NOTE]
> Aralık 2015 itibariyle VM Backup kullanarak SAP lisanslama için kullanılan benzersiz VM KIMLIĞI TUTMAZ. Bu, vm yedeklemesinden geri yükleme nin yeni bir SAP lisans anahtarının yüklenmesini gerektirdiği anlamına gelir, çünkü geri yüklenen VM yeni bir VM olarak kabul edilir ve kaydedilen eskisinin yerine yenisi olarak kabul edilmez.
>
> ![Windows][Logo_Windows] Windows
>
> Teorik olarak, dbms sistemi Windows VSS 'yi (Volume Shadow Copy Service) <https://msdn.microsoft.com/library/windows/desktop/bb968832(v=vs.85).aspx>örneğin SQL Server'ın desteklediği gibi, veritabanlarını çalıştıran VM'ler de tutarlı bir şekilde yedeklenebilir.
> Ancak, Azure VM yedeklemelerine dayalı olarak veritabanlarının zamanında geri yüklenmelerinin mümkün olmadığını unutmayın. Bu nedenle, öneri, Azure VM Yedekleme'ye güvenmek yerine DBMS işlevselliği olan veritabanlarının yedeklemelerini gerçekleştirmektir.
>
> Azure Sanal Makine Yedekleme'ye alışmak için buradan başlayın: <https://docs.microsoft.com/azure/backup/backup-azure-vms>.
>
> Diğer olasılıklar, veritabanlarını yedeklemek/geri yüklemek için Azure VM ve Azure Yedekleme'de yüklü Microsoft Veri Koruma Yöneticisi'nin bir birleşimini kullanmaktır. Daha fazla bilgiyi burada <https://docs.microsoft.com/azure/backup/backup-azure-dpm-introduction>bulabilirsiniz: .  
>
> ![Linux][Logo_Linux] Linux
>
> Linux'ta Windows VSS'nin eşdeğeri yoktur. Bu nedenle yalnızca dosya tutarlı yedeklemeler mümkündür, ancak uygulama tutarlı yedeklemeler değildir. SAP DBMS yedeklemesi DBMS işlevselliği kullanılarak yapılmalıdır. SAP ile ilgili verileri içeren dosya sistemi, örneğin burada açıklandığı gibi katran kullanılarak kaydedilebilir:<https://help.sap.com/saphelp_nw70ehp2/helpdata/en/d3/c0da3ccbb04d35b186041ba6ac301f/content.htm>
>
>

### <a name="azure-as-dr-site-for-production-sap-landscapes"></a>Üretim SAP manzaraları için DR sitesi olarak Azure

2014'ün ortalarından bu yana, Hyper-V, System Center ve Azure çevresindeki çeşitli bileşenlerin uzantıları, Azure'un Hyper-V'yi temel alan şirket içinde çalışan VM'ler için DR sitesi olarak kullanımını sağlar.

Bu çözümün nasıl dağıtılanbildiğini ayrıntılı <https://blogs.msdn.com/b/saponsqlserver/archive/2014/11/19/protecting-sap-solutions-with-azure-site-recovery.aspx>olarak anlatan bir blog burada belgelenmiştir: .

## <a name="summary"></a>Özet

Azure'daki SAP sistemleri için Yüksek Kullanılabilirlik'in temel noktaları şunlardır:

* Bu noktada, SAP tek hata noktası şirket içi dağıtımlarda yapılabilecek şekilde tam olarak aynı şekilde güvence altına alınamaz. Bunun nedeni, Paylaşılan Disk kümelerinin üçüncü taraf yazılımı kullanılmadan Azure'da henüz oluşturulamamasıdır.
* DBMS katmanı için paylaşılan disk kümeleme teknolojisine güvenmeyen DBMS işlevselliğini kullanmanız gerekir. Ayrıntılar [DBMS Kılavuzu'nda][dbms-guide]belgelenmiştir.
* Azure altyapısındaki veya ana bilgisayar bakımındaki Hata Etki Alanları'ndaki sorunların etkisini en aza indirmek için Azure kullanılabilirlik kümelerini kullanmanız gerekir:
  * SAP uygulama katmanı için bir kullanılabilirlik kümesi olması önerilir.
  * SAP DBMS katmanı için ayrı bir kullanılabilirlik kümesi olması önerilir.
  * Farklı SAP sistemlerinin VM'leri için aynı kullanılabilirlik kümesini uygulamak DEĞİLDİr.
  * Premium Yönetilen Diskler kullanılması önerilir.
* SAP DBMS katmanının yedekleme amacıyla [DBMS Kılavuzu'nu kontrol edin.][dbms-guide]
* Basit iletişim örneklerini yeniden dağıtmak genellikle daha hızlı olduğundan SAP İletişim örneklerini yedeklemek pek mantıklı değildir.
* SAP sisteminin küresel dizinini ve farklı örneklerin tüm profillerini içeren VM'yi yedeklemek mantıklıdır ve Windows Backup veya örneğin Linux'ta katran ile yapılmalıdır. Windows Server 2008 (R2) ve Windows Server 2012 (R2) arasında daha yeni Windows Server sürümlerini kullanarak yedeklemeyi kolaylaştıran farklılıklar olduğundan, Windows Server 2012 'yi (R2) Windows konuk işletim sistemi olarak çalıştırmanızı öneririz.

## <a name="next-steps"></a>Sonraki adımlar
Makaleleri okuyun:

- [SAP NetWeaver için Azure Sanal Makineler dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide)
- [SAP iş yükü için Azure Sanal Makineler DBMS dağıtımında dikkat edilmesi gerekenler](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general)
- [AZURE'da SAP HANA altyapı yapılandırmaları ve işlemleri] (https://docs.microsoft.com/
- azure/sanal makineler/iş yükleri/sap/hana-vm-işlemleri)
