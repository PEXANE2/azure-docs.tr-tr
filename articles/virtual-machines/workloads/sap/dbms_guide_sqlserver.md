---
title: SAP iş yükü için SQL Server Azure Sanal Makineler DBMS dağıtımı | Microsoft Dokümanlar
description: SAP iş yükü için SQL Server Azure Sanal Makineler DBMS dağıtımı
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/26/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a0fbed1f4dd62b2d75d39f475d2fe124c55a2b97
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75645812"
---
# <a name="sql-server-azure-virtual-machines-dbms-deployment-for-sap-netweaver"></a>SAP NetWeaver için SQL Server Azure Sanal Makineler DBMS dağıtımı

[767598]:https://launchpad.support.sap.com/#/notes/767598
[773830]:https://launchpad.support.sap.com/#/notes/773830
[826037]:https://launchpad.support.sap.com/#/notes/826037
[965908]:https://launchpad.support.sap.com/#/notes/965908
[1031096]:https://launchpad.support.sap.com/#/notes/1031096
[1114181]:https://launchpad.support.sap.com/#/notes/1114181
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
[2171857]:https://launchpad.support.sap.com/#/notes/2171857
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

[dbms-guide]:dbms-guide_general.md 
[dbms-guide-2.1]:dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f 
[dbms-guide-2.2]:dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 
[dbms-guide-2.3]:dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 
[dbms-guide-2]:dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 
[dbms-guide-3]:dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 
[dbms-guide-5.5.1]:dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268 
[dbms-guide-5.5.2]:dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b 
[dbms-guide-5.6]:dbms_guide_sqlserver.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 
[dbms-guide-5.8]:dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 
[dbms-guide-5]:dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 
[dbms-guide-8.4.1]:dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 
[dbms-guide-8.4.2]:dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d 
[dbms-guide-8.4.3]:dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c 
[dbms-guide-8.4.4]:dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 
[dbms-guide-900-sap-cache-server-on-premises]:dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8
[dbms-guide-managed-disks]:dbms-guide.md#f42c6cb5-d563-484d-9667-b07ae51bce29

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

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

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
[planning-guide-figure-2500]:media/virtual-machines-shared-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]:media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-300]:media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-400]:media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd 
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f 

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
[storage-powershell-guide-full-copy-vhd]:../../../storage/common/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]:../../windows/disks-types.md
[storage-redundancy]:../../../storage/common/storage-redundancy.md
[storage-scalability-targets]:../../../storage/common/scalability-targets-standard-accounts.md
[storage-use-azcopy]:../../../storage/common/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:../../linux/attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../../../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]:../../../resource-manager-deployment-model.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md 
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md 
[virtual-machines-linux-agent-user-guide]:../../linux/agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../linux/agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager-capture]:../../linux/capture-image.md#step-2-capture-the-vm
[virtual-machines-linux-configure-raid]:../../linux/configure-raid.md
[virtual-machines-linux-configure-lvm]:../../linux/configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../../linux/suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../../linux/update-agent.md
[virtual-machines-manage-availability-linux]:../../linux/manage-availability.md
[virtual-machines-manage-availability-windows]:../../windows/manage-availability.md
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
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/eresources/templates/sql-server-2014-alwayson-existing-vnet-and-ad/
[virtual-network-deploy-multinic-arm-cli]:../linux/multiple-nics.md
[virtual-network-deploy-multinic-arm-ps]:../windows/multiple-nics.md
[virtual-network-deploy-multinic-arm-template]:../../../virtual-network/template-samples.md
[virtual-networks-configure-vnet-to-vnet-connection]:../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../../../virtual-network/manage-virtual-network.md#create-a-virtual-network
[virtual-networks-manage-dns-in-vnet]:../../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics]:../../../virtual-network/virtual-network-deploy-multinic-classic-ps.md
[virtual-networks-nsg]:../../../virtual-network/security-overview.md
[virtual-networks-reserved-private-ip]:../../../virtual-network/virtual-networks-static-private-ip-arm-ps.md
[virtual-networks-static-private-ip-arm-pportal]:../../../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-udr-overview]:../../../virtual-network/virtual-networks-udr-overview.md
[vpn-gateway-about-vpn-devices]:../../../vpn-gateway/vpn-gateway-about-vpn-devices.md
[vpn-gateway-create-site-to-site-rm-powershell]:../../../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]:../../../vpn-gateway/vpn-gateway-plan-design.md
[vpn-gateway-site-to-site-create]:../../../vpn-gateway/vpn-gateway-site-to-site-create.md
[vpn-gateway-vpn-faq]:../../../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../../../cli-install-nodejs.md
[xplat-cli-azure-resource-manager]:../../../xplat-cli-azure-resource-manager.md




Bu belge, Azure IaaS'da SAP iş yükü için SQL Server dağıtılırken göz önünde bulundurulması gereken birkaç farklı alanı kapsamaktadır. Bu belgenin ön koşulu olarak, SAP [iş yükü için Azure Sanal Makineler DBMS dağıtımı için gereken hususları](dbms_guide_general.md) ve [Azure belgelerindeki SAP iş yükündeki](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started)diğer kılavuzları okumuş olmalısınız. 



> [!IMPORTANT]
> Bu belgenin kapsamı SQL Server'daki Windows sürümüdür. SAP, SQL Server'ın Linux sürümünü SAP yazılımıyla desteklemiyor. Belgede, Microsoft Azure Platformu'nun Hizmet teklifi olarak bir Platform olan Microsoft Azure SQL Veritabanı tartışMıyor. Bu makaledeki tartışma, Azure Sanal Makineleri'ndeki şirket içi dağıtımlar olarak bilinen SQL Server ürününün çalıştırılması ve Altyapı'nın Azure'un Hizmet yeteneği olarak kullanımıyla ilgilidir. Bu iki teklif arasındaki veritabanı özellikleri ve işlevleri farklıdır ve birbiriyle karıştırılmamalıdır. Ayrıca bakınız:<https://azure.microsoft.com/services/sql-database/>
> 
>

Genel olarak, Azure IaaS'da SAP iş yükünü çalıştırmak için en son SQL Server sürümlerini kullanmayı düşünmelisiniz. En son SQL Server sürümleri, Azure hizmetlerinin ve işlevlerinin bazılarına daha iyi entegrasyon sunar. Veya bir Azure IaaS altyapısında işlemleri en iyi duruma getiren değişiklikler var.

Devam etmeden önce [bu][virtual-machines-sql-server-infrastructure-services] belgeleri gözden geçirmen önerilir.

Aşağıdaki bölümlerde, yukarıdaki bağlantı altında belgelerin parçaları toplanır ve belirtilmiştir. SAP'nin özellikleri de belirtilmiş ve bazı kavramlar daha ayrıntılı olarak açıklanmıştır. Ancak, SQL Server'a özgü belgeleri okumadan önce yukarıdaki belgeler üzerinde çalışma önerilir.

Devam etmeden önce bilmeniz gereken iaaS özel bilgiler bazı SQL Server vardır:

* **SQL Sürüm Desteği**: SAP müşterileri için SQL Server 2008 R2 ve üzeri Microsoft Azure Virtual Machine'de desteklenir. Önceki sürümler desteklenmez. Daha fazla ayrıntı için bu genel [Destek Bildirimini](https://support.microsoft.com/kb/956893) gözden geçirin. Genel olarak, SQL Server 2008 de Microsoft tarafından desteklenir. Ancak, SQL Server 2008 R2 ile tanıtılan SAP için önemli işlevsellik nedeniyle, SQL Server 2008 R2 SAP için minimum sürümdür. Genel olarak, Azure IaaS'da SAP iş yükünü çalıştırmak için en son SQL Server sürümlerini kullanmayı düşünmelisiniz. En son SQL Server sürümleri, Azure hizmetlerinin ve işlevlerinin bazılarına daha iyi entegrasyon sunar. Veya bir Azure IaaS altyapısında işlemleri en iyi duruma getiren değişiklikler var. Bu nedenle, kağıt SQL Server 2016 ve SQL Server 2017 ile sınırlıdır.
* **SQL Performance**: Microsoft Azure barındırılan Sanal Makineler, diğer genel bulut sanallaştırma tekliflerine kıyasla iyi performans gösterir, ancak bireysel sonuçlar değişebilir. Azure Sanal [Makineler'de SQL Server için en iyi uygulamaları gerçekleştirin makalesine](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-performance)göz atın.
* **Azure Marketindeki Görselleri Kullanma**: Yeni bir Microsoft Azure VM dağıtmanın en hızlı yolu Azure Marketi'nden bir resim kullanmaktır. Azure Marketi'nde en son SQL Server sürümlerini içeren görüntüler vardır. SQL Server'ın yüklü olduğu görüntüler SAP NetWeaver uygulamaları için hemen kullanılamaz. Bunun nedeni, varsayılan SQL Server harmanlama sap NetWeaver sistemleri tarafından gerekli değil, bu görüntüler içinde yüklü olmasıdır. Bu tür görüntüleri kullanmak için, [Microsoft Azure Marketi'nin dışında bir SQL Server görüntüsünü kullanarak][dbms-guide-5.6]bölümünde belgelenen adımları denetleyin. 


## <a name="recommendations-on-vmvhd-structure-for-sap-related-sql-server-deployments"></a>SAP ile ilgili SQL Server dağıtımları için VM/VHD yapısı na ilişkin öneriler
Genel açıklamaya uygun olarak, SQL Server yürütülebilir leri VM'nin işletim sistemi diskinin sistem sürücüsüne\)yerleştirilmeli veya kurulmalıdır (sürücü C: .  Genellikle, SQL Server sistem veritabanlarının çoğu SAP NetWeaver iş yükü tarafından yüksek düzeyde kullanılmaz. Sonuç olarak SQL Server sistem veritabanları (master, msdb ve model) C:\ üzerinde kalabilir de sürücü. SAP iş yüklerinde daha yüksek veri hacmi veya G/Ç işlem hacmi gerektirebilecek bir özel durum tempdb olmalıdır. OS VHD'ye uygulanmaması gereken G/Ç iş yükü. Bu tür sistemler için aşağıdaki adımlar gerçekleştirilmelidir:


* Tüm SAP sertifikalı VM türleri ile (bkz. SAP Note [1928533),]A-Serisi VM'ler hariç, tempdb verileri ve günlük dosyaları kalıcı olmayan D:\ Sürücü. 
* Yine de, birden çok tempdb veri dosyaları kullanılması önerilir. D farkında olun:\ sürücü hacimleri VM türüne göre farklıdır. D tam boyutları için:\ farklı Sanal M'lerin sürücüsü, [Azure'daki Windows sanal makineleri için](https://docs.microsoft.com/azure/virtual-machines/windows/sizes)makale Boyutlarını kontrol edin.

Bu yapılandırmalar tempdb'nin sistem sürücüsünün sağlayabileceğinden daha fazla alan tüketmesini sağlar. Kalıcı olmayan D:\ sürücü ayrıca daha iyi G/Ç gecikmesi ve iş artışı sunar (A-Serisi VM'ler hariç). Uygun tempdb boyutunu belirlemek için, mevcut sistemlerde tempdb boyutlarını kontrol edebilirsiniz. 

>[!NOTE]
> durumda tempdb veri dosyalarını yerleştirin ve dosyayı D klasörüne kaydedin:\ oluşturduğunuz sürücü, bir VM yeniden başlatma sonra klasörün var olduğundan emin olmanız gerekir. Since the D:\ bir VM tüm dosyayı yeniden başlatıldıktan ve dizin yapıları silindikten sonra sürücü yeni olarak başlatılır. D:\ üzerinde nihai dizin yapılarını yeniden oluşturma olasılığı SQL Server hizmeti başlamadan önce sürücü [bu makalede](https://cloudblogs.microsoft.com/sqlserver/2014/09/25/using-ssds-in-azure-vms-to-store-sql-server-tempdb-and-buffer-pool-extensions/)belgelenmiştir.

SAP veritabanı ile SQL Server'ı çalıştıran ve tempdb veri ve tempdb logfilesinin D:\ sürücü gibi görünür:

![SQL Server için basit VM disk yapılandırması diyagramı](./media/dbms_sqlserver_deployment_guide/Simple_disk_structure.PNG)

Yukarıdaki diyagram basit bir durum gösterir. Makalede de belirtildiği [gibi, Azure Sanal Makineler DBMS dağıtımı için SAP iş yükü,](dbms_guide_general.md)numarası ve Premium Depolama disklerinin boyutu için göz önünde bulundurulması gereken hususlar farklı etkenlere bağlıdır. Ama genel olarak biz öneririz:

- SQL Server veri dosyalarını içeren bir veya az sayıda birim oluşturmak için depolama alanlarını kullanma. Bu yapılandırmanın arkasındaki neden, gerçek hayatta farklı G/Ç iş yüküne sahip farklı boyutlu veritabanı dosyalarına sahip çok sayıda SAP veritabanı olmasıdır.
- Yeterli IOPS sağlamak ve SQL Server işlem günlüğü dosyası için Depolama alanlarını kullanma. Potansiyel IOPS iş yükü genellikle işlem günlüğü hacminin boyutlandırılması için yol gösterici çizgidir, SQL Server işlem hacminin potansiyel hacmi değildir
- Performans yeterince iyi olduğu sürece tempdb için D:\sürücü kullanın. Genel iş yükü, D:\ üzerinde bulunan tmepdb tarafından performans sınırlı ise:\ [bu makalede](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-performance)önerbelirtildiği gibi ayrı Premium Depolama diskleri için tempdb taşımak için dikkate almak gerekebilir sürücü .


### <a name="special-for-m-series-vms"></a>M Serisi VM'lere Özel
Azure M Serisi VM'de, Işlem günlüğüne yazma süresi, Azure Yazma Hızlandırıcısı kullanırken Azure Premium Depolama performansına kıyasla etkenler tarafından azaltılabilir. Bu nedenle, SQL Server işlem günlüğü için birim oluşturan VHD(ler) için Azure Yazma Hızlandırıcısı dağıtmanız gerekir. Ayrıntılar belgede okunabilir [Hızlandırıcı Yaz](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator).
  

### <a name="formatting-the-disks"></a>Diskleri biçimlendirme
SQL Server için, SQL Server verileri ve günlük dosyaları içeren diskler için NTFS blok boyutu 64KB olmalıdır. D:\ biçimini vermeye gerek yoktur. Sürücü. Bu sürücü önceden biçimlendirilmiş olarak gelir.

Veritabanlarının geri yükleminin veya oluşturulmasının dosyaların içeriğini sıfırlayarak veri dosyalarını başlatmadığından emin olmak için, SQL Server hizmetinin yürüttüğü kullanıcı bağlamının belirli bir izni olduğundan emin olmalısınız. Genellikle Windows Administrator grubundaki kullanıcılar bu izinlere sahiptir. SQL Server hizmeti Windows Administrator olmayan kullanıcının kullanıcı bağlamında çalıştırılırsa, bu kullanıcıyı Kullanıcı Hakkı **Ses Bakım Görevlerini Gerçekleştir'i**atamanız gerekir.  Bu Microsoft Bilgi Bankası Makalesindeki ayrıntılara bakın:<https://support.microsoft.com/kb/2574695>

### <a name="impact-of-database-compression"></a>Veritabanı sıkıştırma etkisi
G/Ç bant genişliğinin sınırlayıcı bir faktör haline gelebileceği yapılandırmalarda, IOPS'yi azaltan her ölçü, Azure gibi bir IaaS senaryosunda çalıştırılabilecek iş yükünü n için genişletmeye yardımcı olabilir. Bu nedenle, henüz yapılmamışsa, varolan bir SAP veritabanını Azure'a yüklemeden önce hem SAP hem de Microsoft tarafından SQL Server PAGE sıkıştırması önerilir.

Azure'a yüklemeden önce Veritabanı Sıkıştırma'yı gerçekleştirme önerisi iki nedenden dolayı verilmiştir:

* Yüklenecek veri miktarı daha düşüktür.
* Sıkıştırma yürütme süresi, daha fazla CPU veya daha yüksek G/Ç bant genişliği veya daha az G/Ç gecikmesi ile daha güçlü donanım kullanabileceğini varsayarsak daha kısadır.
* Daha küçük veritabanı boyutları, disk ayırma için daha az maliyete neden olabilir

Veritabanı sıkıştırma, bir Azure Sanal Makineleri'nde şirket içinde olduğu gibi çalışır. Varolan SAP NetWeaver SQL Server veritabanlarının nasıl sıkıştırılacağın alabildiğini öğrenmek için geliştirilmiş [SAP sıkıştırma aracı MSSCOMPRESS'ı](https://blogs.msdn.microsoft.com/saponsqlserver/2016/11/25/improved-sap-compression-tool-msscompress/)işaretleyin. 

## <a name="sql-server-2014-and-more-recent---storing-database-files-directly-on-azure-blob-storage"></a>SQL Server 2014 ve daha yeni - Veritabanı Dosyalarını doğrudan Azure Blob Depolama'da depolama
SQL Server 2014 ve daha sonra, veritabanı dosyalarını çevrelerindeki Bir VHD'nin 'sarıcı' olmadan doğrudan Azure Blob Store'da depolama olanağı nı yayımlar. Özellikle Standart Azure Depolama veya daha küçük VM türleri kullanılarak bu tür dağıtımlar, bazı küçük VM türlerine monte edilebilen sınırlı sayıda disk tarafından zorlanacak IOPS sınırlarının üstesinden gelebileceğiniz senaryolar sağlar. Bu dağıtım şekli, sql server sistem veritabanları için değil, kullanıcı veritabanları için çalışır. Ayrıca SQL Server veri ve günlük dosyaları için çalışır. Bir SAP SQL Server veritabanını VHD'lere 'sarma' yerine bu şekilde dağıtmak istiyorsanız, aklınızda bulundurun:

* Kullanılan Depolama Hesabı'nın, VM SQL Server'ı dağıtmak için kullanılan Hesap'ın çalıştığı Azure Bölgesinde olması gerekir.
* VHD'lerin farklı Azure Depolama Hesapları'na dağıtımıyla ilgili daha önce listelenen hususlar, bu dağıtım yöntemi için de geçerlidir. G/Ç işlemleri Azure Depolama Hesabı'nın sınırlarına göre sayılır anlamına gelir.
* VM'nin depolama G/Ç kotasına karşı muhasebe yapmak yerine, SQL Server verilerini ve günlük dosyalarını temsil eden depolama bloblarına karşı trafik, VM'nin belirli VM türündeki ağ bant genişliğine kaydedilir. Belirli bir VM türünün ağ ve depolama bant genişliği için [Azure'daki Windows sanal makineleri için makale Boyutlar'a başvurun.](https://docs.microsoft.com/azure/virtual-machines/windows/sizes)
* Ağ kotası üzerinden dosya G/Ç itme sonucunda, depolama kotasını çoğunlukla mahsur kullanabilirsiniz ve bu da VM'nin genel bant genişliğini yalnızca kısmen kullanır.
* Azure Premium Depolama'nın farklı disk boyutları için sahip olduğu IOPS ve G/Ç iş durumu Performansı hedefleri artık geçerli değildir. Oluşturduğunuz lekeler Azure Premium Depolama'da bulunsa bile. Hedefler makale [Yüksek performanslı Premium Depolama ve VM'ler için yönetilen diskler](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage)belgelenmiştir. SQL Server veri dosyalarının ve günlük dosyalarının doğrudan Azure Premium Depolama'da depolanan blob'lara yerleştirilmesi sonucunda, performans özellikleri Azure Premium Depolama'daki VHD'lere göre farklı olabilir.
* SQL Server veri dosyalarını doğrudan Azure blobs'a yerleştirirken Azure Premium Depolama diskleri için kullanılabilir ana bilgisayar tabanlı önbelleğe alma kullanılamaz.
* M Serisi VM'lerde Azure Yazma Hızlandırıcısı, SQL Server işlem günlüğü dosyasına karşı milisaniye altı yazılarını desteklemek için kullanılamaz. 

Bu işlevselliğin ayrıntılarını [Microsoft Azure'daki SQL Server veri dosyalarında](https://docs.microsoft.com/sql/relational-databases/databases/sql-server-data-files-in-microsoft-azure?view=sql-server-2017) bulabilirsiniz.

Üretim sistemleri için öneri, bu yapılandırmadan kaçınmak ve sql server verilerinin yerleşimlerini seçmek ve dosyaları doğrudan Azure blobs yerine Azure Premium Storage VHD'lerde günlüğe kaydetmektir.


## <a name="sql-server-2014-buffer-pool-extension"></a>SQL Server 2014 Arabellek Havuzu Uzantısı
SQL Server 2014, [Arabellek Havuzu Uzantısı](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension?view=sql-server-2017)adı verilen yeni bir özellik tanıttı. Bu işlevsellik, bir sunucunun veya VM'nin yerel SSD'leri tarafından desteklenen ikinci düzey bir önbellekle bellekte tutulan SQL Server'ın arabellek havuzunu genişletir. Arabellek havuzu uzantısı, daha büyük bir çalışma veri kümesini 'bellekte' tutmayı sağlar. Azure Standart Depolama'ya erişmeyle karşılaştırıldığında, bir Azure VM'nin yerel SSD'lerinde depolanan arabellek havuzunun uzantısına erişim birçok etkenden daha hızlıdır. Arabellek Havuzu Uzantısı'nı SQL Server veri dosyaları için önerilen Azure Premium Depolama Okuma Önbelleği ile karşılaştırmak, Arabellek Havuzu Uzantıları için önemli bir avantaj beklenmesi değildir. Bunun nedeni, her iki önbellek (SQL Server Arabellek Havuzu Uzantısı ve Premium Depolama Okuma Önbelleği) Azure bilgi işlem düğümünün yerel disklerini kullanıyor olmasıdır.

SAP iş yükü ile SQL Server Arabellek Havuzu Uzantısı ile bu arada kazanılan deneyimler karıştırılır ve yine de her durumda kullanıp kullanmamak konusunda net öneriler izin vermez. İdeal durum, SAP uygulamasının gerektirdiği çalışma kümesinin ana belleğe sığdırılmasıdır. Bu arada Azure, 4 TB'a kadar bellekle gelen VM'ler sunarken, çalışma kümesini bellekte tutmak mümkün olmalıdır. Bu nedenle Arabellek Havuzu Uzantısı kullanımı bazı nadir durumlarda sınırlıdır ve bir genel durum olmamalıdır.  

## <a name="backuprecovery-considerations-for-sql-server"></a>SQL Server için Yedekleme/Kurtarma hususları
SQL Server'ı Azure'a dağıtırken yedekleme metodolojiniz gözden geçirilmelidir. Sistem bir üretim sistemi olmasa bile, SQL Server tarafından barındırılan SAP veritabanının düzenli olarak yedeklenmesi gerekir. Azure Depolama üç görüntü tuttuğundan, bir depolama çökmesini telafi etmek açısından yedekleme artık daha az önemlidir. Uygun bir yedekleme ve kurtarma planı korumak için öncelik nedeni zaman kurtarma yetenekleri nokta sağlayarak mantıksal / el ile hataları telafi edebilirsiniz daha fazladır. Bu nedenle amaç, veritabanını belirli bir noktaya geri yüklemek için yedeklemeleri kullanmak veya varolan veritabanını kopyalayarak başka bir sistemi tohumlamak için Azure'daki yedeklemeleri kullanmaktır. 

Azure'daki farklı SQL Server yedekleme olanaklarına bakmak [için Azure Sanal Makinelerde SQL Server için Yedekleme ve Geri Yükleme makalesini](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery)okuyun. Makale birkaç farklı olasılıkları kapsamaktadır.

### <a name="manual-backups"></a>Manuel yedeklemeler
'El ile' yedeklemeleri gerçekleştirmek için çeşitli olanaklara sahipsiniz:

1. Doğrudan bağlı Azure disklerine geleneksel SQL Server yedeklemeleri gerçekleştirme. Bu yöntem, sistem yenilemeleri için hızlı bir şekilde kullanılabilen yedeklemelere sahip olduğunuz ve mevcut SAP sistemlerinin kopyaları olarak yeni sistemler oluşturma avantajına sahiptir
2.  SQL Server 2012 CU4 ve üzeri veritabanlarını Azure depolama URL'sine yedekleyebilir.
3.  Azure Blob Depolamasında Veritabanı Dosyaları için Dosya Anlık Görüntü Yedekleri. Bu yöntem yalnızca SQL Server verileriniz ve günlük dosyalarınız Azure blob depolama da bulunduğunda çalışır

İlk yöntem iyi bilinen ve birçok durumda şirket içi dünyada da uygulanır. Yine de, uzun vadeli yedekleme konumunu çözmek için görev ile bırakır. Yedeklemelerinizi yerel olarak bağlı Azure Depolama'da 30 veya daha fazla gün tutmak istemediğiniz için, Azure Yedekleme Hizmetleri'ni veya yedeklemeleriniz için erişim ve bekletme yönetimini içeren başka bir üçüncü taraf yedekleme/kurtarma aracını kullanmanız gerekir. Veya Windows depolama alanlarını kullanarak Azure'da büyük bir dosya sunucusu oluşturursunuz.

İkinci [yöntem, SQL Server Backup to URL](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url?view=sql-server-2017)makalesinde daha yakın olarak açıklanmıştır. SQL Server'ın farklı sürümlerinde bu işlevsellikte bazı varyasyonlar vardır. Bu nedenle, belirli SQL Server sürüm denetimi için belgeleri kontrol etmelisiniz. Önemli bu makalede kısıtlamalar bir sürü listeler dikkat etmek. Yedeklemeyi aşağıdakilere karşı gerçekleştirme olanağınız vardır:

- Tek bir Azure sayfası blob, daha sonra yedekleme boyutunu 1000 GB ile sınırlar. Bu aynı zamanda elde edebilirsiniz iş geleni sınırlar.
- 12 TB teorik yedekleme boyutu sağlayan birden çok (64'e kadar) Azure blok lekeleri. Ancak, müşteri veritabanları ile yapılan testler, maksimum yedekleme boyutunun teorik sınırından daha küçük olabileceğini ortaya çıkardı. Bu durumda, yedeklemelerin tutulmasını yönetmekten ve o yedeklemelere de erişmekten siz sorumlusunuz.


### <a name="automated-backup-for-sql-server"></a>SQL Server için Otomatik Yedekleme
Otomatik Yedekleme, Azure'da bir Windows VM'de çalışan SQL Server Standard ve Enterprise sürümleri için otomatik yedekleme hizmeti sağlar. Bu hizmet, Azure portalındaki SQL Server Windows sanal makine resimlerine otomatik olarak yüklenen [SQL Server IaaS Agent Extension](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension)tarafından sağlanır. SQL Server yüklü kendi işletim sistemi görüntüleri dağıtıyorsanız, VM uzantılarını ayrı olarak yüklemeniz gerekir. Gerekli adımlar bu [makalede](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension)belgelenmiştir.

Bu yöntemin yetenekleri hakkında daha fazla bilgi bu makalelerde bulunabilir:

- SQL Server 2014: [SQL Server 2014 Sanal Makineler için Otomatik Yedekleme (Kaynak Yöneticisi)](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-backup)
- SQL Server 2016/2017: [Azure Sanal Makineler için Otomatik Yedekleme v2 (Kaynak Yöneticisi)](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-backup-v2)

Belgelere bakıldığında, daha yeni SQL Server sürümleriile işlevselliğin iyileştiğini görebilirsiniz. SQL Server otomatik yedeklemeleri hakkında daha fazla ayrıntı, [SQL Server Managed Backup to Microsoft Azure](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure?view=sql-server-2017)makalesinde yayımlanır. Teorik yedekleme boyutu sınırı 12 TB'dir.  Otomatik yedeklemeler, 12 TB'a kadar yedekleme boyutları için iyi bir yöntem olabilir. Birden çok blob paralel olarak yazıldığından, 100 MB/sn'den daha büyük bir iş bekleyebilirsiniz. 
 

### <a name="azure-backup-for-sql-server-vms"></a>SQL Server VM'ler için Azure Yedekleme
BU yeni SQL Server yedekleme yöntemi, Azure Yedekleme hizmetleri tarafından genel önizleme olarak Haziran 2018 itibarıyla sunulur. SQL Server yedekleme yöntemi diğer üçüncü taraf araçları, yani SQL Server VSS / VDI arabirimi bir hedef konuma yedekleme akışı için kullanıyorsanız aynıdır. Bu durumda hedef konum Azure Kurtarma Hizmeti kasasıdır.

Merkezi yedekleme yapılandırmaları, izleme ve yönetim çok sayıda avantaj ekler bu yedekleme yöntemi, ayrıntılı bir açıklama [daha burada](https://docs.microsoft.com/azure/backup/backup-azure-sql-database)mevcuttur. 


### <a name="third-party-backup-solutions"></a>Üçüncü taraf yedekleme çözümleri
Çok sayıda SAP müşterisi için, yeniden başlayıp, SAP ortamının Azure'da çalışan bölümü için eksiksiz yeni yedekleme çözümleri sunma olasılığı yoktu. Sonuç olarak, varolan yedekleme çözümlerinin kullanılması ve Azure'a genişletilmesi gerekiyordu. Varolan yedekleme çözümlerini Azure'a genişletmek genellikle bu alandaki ana satıcıların çoğunda iyi çalıştı. 


## <a name="using-a-sql-server-image-out-of-the-microsoft-azure-marketplace"></a><a name="1b353e38-21b3-4310-aeb6-a77e7c8e81c8"></a>Microsoft Azure Marketi'nden bir SQL Server görüntüsü kullanma
Microsoft, SQL Server sürümlerini zaten içeren Azure Marketi'nde VM'ler sunar. SQL Server ve Windows için lisans gerektiren SAP müşterileri için, bu görüntüleri kullanmak, zaten yüklü olan SQL Server'a sahip VM'leri döndürerek lisans gereksinimini karşılamak için bir fırsat olabilir. Sap için bu tür görüntüleri kullanabilmek için aşağıdaki hususların yapılması gerekir:

* SQL Server Değerlendirme Dışı sürümleri, Azure Marketi'nden dağıtılan 'yalnızca Windows' VM'den daha yüksek maliyetler kazanır. Fiyatları karşılaştırmak için <https://azure.microsoft.com/pricing/details/virtual-machines/windows/> bu <https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-enterprise/>makalelere bakın: ve . 
* Yalnızca SAP tarafından desteklenen SQL Server sürümlerini kullanabilirsiniz.
* Azure Marketi'nde sunulan VM'lerde yüklenen SQL Server örneğinin harmanlanması, SAP NetWeaver'ın çalıştırmak için SQL Server örneğini gerektirdiği harmanlama değildir. Aşağıdaki bölümde ki yollarla olsa da harmanlama değiştirebilirsiniz.

### <a name="changing-the-sql-server-collation-of-a-microsoft-windowssql-server-vm"></a>Microsoft Windows/SQL Server VM'nin SQL Server Collation'ını değiştirme
Azure Marketi'ndeki SQL Server görüntüleri SAP NetWeaver uygulamalarının gerektirdiği harmanlama yı kullanacak şekilde ayarlanmadığından, dağıtımdan hemen sonra değiştirilmesi gerekir. SQL Server için, bu harmanlama değişikliği VM dağıtılır dağılmaz ve bir yönetici dağıtılan VM'ye giriş yapabilir girmez aşağıdaki adımlarla yapılabilir:

* Yönetici olarak bir Windows Komut Penceresi açın.
* Dizini C:\Program Files\Microsoft SQL Server\110\Setup Bootstrap\SQLServer2012 olarak değiştirin.
* Komutu çalıştırın: Setup.exe /QUIET /ACTION=REBUILDDATABASE /INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS=`<local_admin_account_name`> /SQLCOLLATION=SQL_Latin1_General_Cp850_BIN2   
  * `<local_admin_account_name`>, VM'yi galeri aracılığıyla ilk kez dağıtırken yönetici hesabı olarak tanımlanan hesaptır.

İşlem yalnızca birkaç dakika sürer. Adımın doğru sonuçla sonuçlanıp sonuçlanmadığından emin olmak için aşağıdaki adımları gerçekleştirin:

* SQL Server Management Studio’yu açın.
* Sorgu Penceresi'ni açın.
* SQL Server ana veritabanında sp_helpsort komutunu çalıştırın.

İstenilen sonuç aşağıdaki gibi görünmelidir:

    Latin1-General, binary code point comparison sort for Unicode Data, SQL Server Sort Order 40 on Code Page 850 for non-Unicode Data

Sonuç farklıysa, SAP dağıtımını durdurun ve kurulum komutunun neden beklendiği gibi çalışmadığını araştırın. SAP NetWeaver uygulamalarının, yukarıda belirtilenden farklı SQL Server kod sayfaları ile SQL Server örneğine dağıtılması **DESTEKLENMEZ.**

## <a name="sql-server-high-availability-for-sap-in-azure"></a>SQL Server Azure'da SAP için Yüksek Kullanılabilirlik
SAP için Azure IaaS dağıtımlarında SQL Server'ı kullanarak, kullanılabilir DBMS katmanını dağıtmak için ekleyeceğiniz birkaç farklı olanak vardır. Azure Sanal [Makineler DBMS dağıtımında sap iş yükü için göz önünde bulundurulması](dbms_guide_general.md) gerektiği gibi, Azure tek bir VM için farklı up-time SLA'lar ve bir Azure Kullanılabilirlik Kümesi'nde dağıtılan bir çift VM sağlar. Varsayım, Azure Kullanılabilirlik Kümeleri'nde dağıtım gerektiren üretim dağıtımlarınız için zamanında SLA'ya yönelmenizdir. Böyle bir durumda, böyle bir Kullanılabilirlik Kümesi'ne en az iki VM dağıtmanız gerekir. Bir VM etkin SQL Server Instance çalıştıracaktır. Diğer VM pasif Örnek çalışacak

### <a name="sql-server-clustering-using-windows-scale-out-file-server"></a>Windows Scale-out File Server kullanarak SQL Server Kümeleme
Microsoft, Windows Server 2016 ile [Storage Spaces Direct'i](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)tanıttı. Depolama Alanları Doğrudan Dağıtım'a bağlı olarak, SQL Server FCI kümeleme desteklenir. Ayrıntılar [Azure Sanal Makinelerde SQL Server Failover Cluster Instance yapılandırma](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-create-failover-cluster)makalede bulunabilir. Çözüm, küme kaynaklarının sanal IP adresiyle başa çıkabilmek için bir Azure yük dengeleyicisi gerektirir. SQL Server veritabanı dosyaları Depolama Alanları'nda depolanır. Bu nedenle, Azure Premium Depolama'yı temel alan Windows Depolama Alanları oluşturmanız gerektiği belirtilmektedir. Bu çözüm henüz çok uzun süre desteklenmediğinden, sap üretim senaryolarında bu çözümü kullanan bilinen bir SAP müşterisi yoktur.  

### <a name="sql-server-log-shipping"></a>SQL Server Log Gönderimi
Yüksek kullanılabilirlik yöntemlerinden biri (HA) SQL Server Log Shipping olduğunu. HA yapılandırmasında yer alan VM'lerde çalışan ad çözümlemesi varsa, sorun yoktur ve Azure'daki kurulum şirket içinde yapılan herhangi bir kurulumdan farklı değildir. Log Shipping'in kurulumu ve Log Shipping ile ilgili ilkeler ile ilgili olarak. SQL Server Log Shipping'in ayrıntılarını [Log Shipping (SQL Server) hakkında](https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server?view=sql-server-2017)makalede bulabilirsiniz.

SQL Server günlük gönderim işlevi, azure'da tek bir Azure bölgesinde yüksek kullanılabilirlik elde etmek için pek kullanılmadı. Ancak, aşağıdaki senaryolarda SAP müşterileri Azure ile birlikte günlük gönderimlerini başarıyla kullanıyordu:

- Bir Azure bölgesinden başka bir Azure bölgesine olağanüstü durum kurtarma senaryoları
- Şirket içi bir Azure bölgesine olağanüstü durum kurtarma yapılandırması
- Şirket içi ve Azure'a kesme senaryoları. Bu gibi durumlarda, günlük gönderimi, Azure'daki yeni DBMS dağıtımını şirket içinde devam eden üretim sistemiyle eşitlemek için kullanılır. Kesme sırasında üretim kapatılır ve son ve en son işlem günlüğü yedeklemelerinin Azure DBMS dağıtımına aktarıldığından emin olun. Ardından Azure DBMS dağıtımı üretime açılır.  



### <a name="database-mirroring"></a>Veritabanı Yansıtma
SAP tarafından desteklenen Veritabanı Yansıtma (bkz. SAP Note [965908)]SAP bağlantı dizesinde bir hata oluşturmaya dayanır. Çapraz Önermeler için, iki VM'nin aynı etki alanında olduğunu ve kullanıcı bağlamında iki SQL Server örneğinin de bir etki alanı kullanıcısı altında çalıştığını ve ilgili iki SQL Server örneğinde yeterli ayrıcalıklara sahip olduğunu varsayıyoruz. Bu nedenle, Azure'da Veritabanı Yansıtma'nın kurulumu, tipik bir şirket içi kurulum/yapılandırma arasında farklılık göstermez.

Yalnızca Bulut dağıtımları itibariyle, en kolay yöntem Azure'da bu DBMS VM'leri (ve ideal olarak özel SAP VM'leri) tek bir etki alanında olması için başka bir etki alanı kurulumuna sahip olmaktır.

Bir etki alanı mümkün değilse, burada açıklandığı gibi uç noktaları yansıtan veritabanı için sertifikalar da kullanabilirsiniz:<https://docs.microsoft.com/sql/database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql>

Azure'da Veritabanı Yansıtma'yı kurmak için bir öğreticiye buradan bakabilirsiniz:<https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server> 

### <a name="sql-server-always-on"></a>SQL Server Always On
Her Zaman Sap şirket içi için desteklendikçe (bkz. SAP Note [1772688),]Azure'da SAP ile birlikte desteklenir. Sql Server Kullanılabilirlik Grubu Dinleyicisi'ni dağıtma konusunda bazı özel hususlar vardır (Azure Kullanılabilirlik Kümesi ile karıştırılmamalıdır) çünkü Azure bu noktada şirket içinde mümkün olduğu kadar bir AD/DNS nesnesi oluşturulmasına izin vermez. Bu nedenle, Azure'un belirli davranışlarının üstesinden gelmek için bazı farklı yükleme adımları gereklidir.

Kullanılabilirlik Grubu Dinleyicisi'ni kullanarak dikkat edilmesi gereken bazı noktalar şunlardır:

* Kullanılabilirlik Grubu Dinleyicisi'ni kullanmak yalnızca Windows Server 2012 veya daha yüksek bir zamanda VM'nin konuk işletim sistemi olarak kullanılabilir. Windows Server 2012 için bu düzeltme eki uygulandığından emin olmanız gerekir:<https://support.microsoft.com/kb/2854082> 
* Windows Server 2008 R2 için bu yama yok ve Her Zaman Açık bağlantıları dizebir failover ortağı belirterek Veritabanı Yansıtma aynı şekilde kullanılması gerekir (SAP default.pfl parametre dbs/mss/server üzerinden yapılır - SAP Note [965908]bakın).
* Kullanılabilirlik Grubu Dinleyicisi kullanırken, Veritabanı VM'lerinin özel bir Yük Dengeleyicisine bağlanması gerekir. Azure'un her iki VM'nin de tesadüfen kapatıldığı durumlarda yeni IP adresleri atamasını önlemek için, Her Zaman Açık yapılandırmasında bu VM'lerin ağ arabirimlerine statik IP adresleri atamanız gerekir [(bu][virtual-networks-reserved-private-ip] makalede statik bir IP adresi tanımlama açıklanmıştır)
* WsFC küme yapılandırmasını oluşturulurken kümenin atanmış özel bir IP adresine ihtiyacı olduğu özel adımlar gereklidir, çünkü Azure geçerli işleviyle kümenin oluşturulduğu düğümle aynı IP adresini atar. Bu, kümeye farklı bir IP adresi atamak için el ile bir adım ın gerçekleştirilmesi gerektiği anlamına gelir.
* Kullanılabilirlik Grubu Dinleyicisi, Kullanılabilirlik grubunun birincil ve ikincil yinelemelerini çalıştıran VM'lere atanan TCP/IP uç noktalarıyla Azure'da oluşturulacak.
* Bu uç noktaları ALAK'larla sabitlemek gerekebilir.

Azure VM listelerinde SQL Server ile Her Zaman Açık'ı dağıtmayla ilgili ayrıntılı belgeler:

- [Azure sanal makinelerde SQL Server Always On kullanılabilirlik gruplarına tanıtın.](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview)
- [Farklı bölgelerdeki Azure sanal makinelerinde Her Zaman Açık kullanılabilirlik grubunu yapılandırın.](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-dr)
- [Azure'daki Her Zaman Kullanılabilirlik grubu için bir yük dengeleyicisi yapılandırın.](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener)

>[!NOTE]
> Kullanılabilirlik Grubu dinleyicisinin sanal IP adresi için Azure yük bakiyesini yapılandırıyorsanız, DirectServerReturn'ün yapılandırıldığından emin olun. bu seçeneğin yapılandırılması, SAP uygulama katmanı ile DBMS katmanı arasındaki ağ gidiş-dönüş gecikmesini azaltır. 

SQL Server Always On, SAP iş yükü dağıtımları için Azure'da kullanılan en yaygın yüksek kullanılabilirlik ve olağanüstü durum kurtarma işlevidir. Çoğu müşteri, tek bir Azure Bölgesinde yüksek kullanılabilirlik için Always On'u kullanır. Dağıtım yalnızca iki düğümle sınırlıysa, bağlantı için iki seçeneğiniz vardır:

- Kullanılabilirlik Grubu Dinleyicisini kullanma. Kullanılabilirlik Grubu Dinleyicisi ile bir Azure yük dengeleyicisi dağıtmanız gerekir. Bu genellikle varsayılan dağıtım yöntemidir. SAP uygulamaları, tek bir düğüme karşı değil, Kullanılabilirlik Grubu dinleyicisine karşı bağlanacak şekilde yapılandırılacak
- SQL Server Database Mirroring'in bağlantı parametrelerini kullanma. Bu durumda, SAP uygulamalarının bağlantılarını her iki düğüm adının da adlandırılacak şekilde yapılandırmanız gerekir. Böyle bir SAP yan yapılandırmasının tam ayrıntıları SAP Note [#965908](https://launchpad.support.sap.com/#/notes/965908)belgelenmiştir. Bu seçeneği kullanarak, bir Kullanılabilirlik Grubu dinleyicisi yapılandırmanız gerekmez. Ve bu sql server yüksek kullanılabilirlik için hiçbir Azure yük dengeleyici ile. Sonuç olarak, SQL Server örneğine gelen trafik Azure yük bakiyesi üzerinden yönlendirilemediğinden, SAP uygulama katmanı ile DBMS katmanı arasındaki ağ gecikmesi daha düşüktür. Ancak, bu seçenek yalnızca Kullanılabilirlik Grubunuzu iki örneğine yayacak şekilde kısıtlarsanız çalışır. 

Pek çok müşteri, Azure bölgeleri arasında ek olağanüstü durum kurtarma işlevselliği için SQL Server Always Always On işlevinden yararlanıyor. Bazı müşteriler, ikincil bir yinelemeden yedekleme gerçekleştirme özelliğini de kullanır. 

## <a name="sql-server-transparent-data-encryption"></a>SQL Server Şeffaf Veri Şifreleme
SAP SQL Server veritabanlarını Azure'da dağıtırken SQL Server [Saydam Veri Şifreleme (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) kullanan çok sayıda müşteri vardır. SQL Server TDE işlevi SAP tarafından tam olarak desteklenir (bkz. SAP Note [#1380493).](https://launchpad.support.sap.com/#/notes/1380493) 

### <a name="applying-sql-server-tde"></a>SQL Server TDE Uygulama
Şirket içinde çalışan başka bir DBMS'den Azure'da çalışan Windows/SQL Server'a heterojen bir geçiş gerçekleştirdiğinizde, boş hedef veritabanınızı SQL Server'da önceden oluşturmanız gerekir. Bir sonraki adımda SQL Server TDE işlevini uygulayabilirsiniz. Üretim sisteminizi şirket içinde çalıştırırken. Bu sırada gerçekleştirmek istediğiniz nedeni, boş veritabanı şifreleme işlemi oldukça uzun sürebilir olmasıdır. SAP alma işlemleri daha sonra kapalı kalma aşamasında verileri şifrelenmiş veritabanına aktarır. Şifreli bir veritabanına alma yükü, kapatma süresi aşamasındadışlama aşamasından sonra veritabanını şifrelemekten daha düşük zaman etkisine sahiptir. Veritabanının üstünde çalışan SAP iş yükü ile TDE'yi uygulamaya çalışırken yapılan olumsuz deneyimler. Bu nedenle, öneri, TDE'nin dağıtımını belirli veritabanında SAP iş yükü olmadan yapılması gereken bir etkinlik olarak ele almaktır.

SAP SQL Server veritabanlarını şirket içi Azure'a taşıyacağınız durumlarda, şifrelemeyi en hızlı şekilde uygulayabileceğiniz altyapı üzerinde test yapmanızı öneririz. Bunun için bu gerçekleri aklınızda tutun:

- Veritabanına veri şifreleme uygulamak için kaç iş parçacığı nın kullanıldığını tanımlayamazsınız. İş parçacığı sayısı büyük ölçüde SQL Server veri ve günlük dosyaları üzerinde dağıtılır disk birimleri nin sayısına bağlıdır. Daha farklı birimler (sürücü harfleri) anlamına gelir, daha fazla iş parçacığı şifreleme gerçekleştirmek için paralel meşgul olacak. Böyle bir yapılandırma, Azure VM'lerde SQL Server veritabanı dosyaları için bir veya daha az sayıda depolama alanı oluşturma yla ilgili önceki disk yapılandırma önerisiyle biraz çelişmektedir. Az sayıda birim içeren bir yapılandırma, şifrelemeyi çalıştıran az sayıda iş parçacığına yol açar. Tek bir iş parçacığı şifreleme 64KB ölçüde okuma, şifreler ve daha sonra işlem günlüğü dosyasına bir kayıt yazmak, ölçüde şifrelenmiş var söylüyorum. Sonuç olarak, hareket günlüğündeki yük orta düzeydedir.
- Eski SQL Server sürümlerinde, SQL Server veritabanınızı şifrelediğinizde yedekleme sıkıştırma artık verimlilik elde etmedi. Bu davranış, planınız SQL Server veritabanınızı şirket içinde şifrelemek ve ardından Azure'daki veritabanını geri yüklemek için yedeklemeyi Azure'a kopyalamak olduğunda bir soruna dönüşebilir. SQL Server yedekleme sıkıştırma genellikle faktör 4 bir sıkıştırma oranı elde eder.
- SQL Server 2016 ile SQL Server, şifreli veritabanlarını verimli bir şekilde sıkıştırmaya olanak tanıyan yeni işlevleri kullanıma sundu. Bazı ayrıntılar için [bu bloglara](https://blogs.msdn.microsoft.com/sqlcat/2016/06/20/sqlsweet16-episode-1-backup-compression-for-tde-enabled-databases/) bakın.
 
TDE şifreleme uygulamasını yalnızca küçük SAP iş yükü olmadan ele alarak, TDE'yi şirket içinde SAP veritabanınıza uygulamanın mı yoksa Azure'da mı uygulamanın daha iyi olup olmadığını özel yapılandırmanızda sınamalısınız. Azure'da, aşırı sağlama altyapısı açısından kesinlikle daha fazla esnekliğe sahipsiniz ve TDE uygulandıktan sonra altyapıyı küçültür.

### <a name="using-azure-key-vault"></a>Azure Anahtar Kasası Kullanma
Azure, şifreleme anahtarlarını depolamak için [Bir Key Vault](https://azure.microsoft.com/services/key-vault/) hizmetini sunar. Diğer taraftaki SQL Server, TDE sertifikaları için mağaza olarak Azure Key Vault'u kullanabilecek bir konektör sunar.

SQL Server TDE listeleri için Azure Key Vault'u kullanmak için daha fazla ayrıntı:

- [Azure Key Vault (SQL Server) kullanarak Genişletilebilir Anahtar Yönetimi.](https://docs.microsoft.com/sql/relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server?view=sql-server-2017)
- [SQL Server TDE Genişletilebilir Anahtar Yönetimi Azure Key Vault kullanarak - Kurulum Adımları](https://docs.microsoft.com/sql/relational-databases/security/encryption/setup-steps-for-extensible-key-management-using-the-azure-key-vault?view=sql-server-2017).
- [SQL Server Bağlayıcı Bakım & Sorun Giderme](https://docs.microsoft.com/sql/relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting?view=sql-server-2017).
- [Müşterilerden SQL Server Saydam Veri Şifrelemesi Hakkında Daha Fazla Soru – TDE + Azure Key Vault](https://blogs.msdn.microsoft.com/saponsqlserver/2017/04/04/more-questions-from-customers-about-sql-server-transparent-data-encryption-tde-azure-key-vault/).


>[!IMPORTANT]
>SQL Server TDE'nin, özellikle Azure tuşu Vault ile kullanılması, SQL Server 2014, SQL Server 2016 ve SQL Server 2017'nin en son yamaları kullanılması önerilir. Bunun nedeni, müşteri geri bildirimlerine dayalı olarak, koda optimizasyonlar ve düzeltmelerin uygulanmasıdır. Örnek olarak, [KBA #4058175](https://support.microsoft.com/help/4058175/tde-enabled-backup-and-restore-slow-if-encryption-key-is-stored-in-ekm)denetleyin.
>  

## <a name="general-sql-server-for-sap-on-azure-summary"></a><a name="9053f720-6f3b-4483-904d-15dc54141e30"></a>Azure Özeti'nde SAP için Genel SQL Server
Bu kılavuzda birçok öneri vardır ve Azure dağıtımınızı planlamadan önce bu kılavuzu birden çok kez okumanızı öneririz. Ancak genel olarak, Azure'a özel önerilerle ilgili en iyi genel DBMS'yi takip etmeyi unutmayın:

1. Azure'da en fazla avantaja sahip OLAN SQL Server 2017 gibi en son DBMS sürümükullanın. 
2. Veri dosyası düzenini ve Azure kısıtlamalarını dengelemek için SAP sistem ortamınızı Azure'da dikkatlice planlayın:
   * Çok fazla diskiniz yok, ancak gerekli IOPS'nize ulaşabildiğinizden emin olmak için yeterli diske sahip olun.
   * Yönetilen Diskler kullanmıyorsanız, IOPS'nin Azure Depolama Hesabı başına da sınırlı olduğunu ve Depolama Hesaplarının her Azure aboneliğinde sınırlı olduğunu unutmayın[(daha fazla ayrıntı).][azure-resource-manager/management/azure-subscription-service-limits] 
   * Yalnızca daha yüksek bir iş elde etmeniz gerekiyorsa diskler arasında şerit.
3. Asla yazılım yüklemek veya D kalıcılık gerektiren herhangi bir dosya koymak:\ kalıcı olmayan bir sürücü ve bu sürücüdeki herhangi bir şey Windows yeniden başlatma kaybolur.
4. Azure Standart Depolama için disk önbelleğe alma kullanmayın.
5. Azure coğrafi olarak çoğaltılmış Azure Standart Depolama Hesapları kullanmayın.  DBMS iş yükleri için Yerel Yedek'i kullanın.
6. Veritabanı verilerini çoğaltmak için DBMS satıcınızın HA/DR çözümünü kullanın.
7. Her zaman Ad Çözümlemesi kullanın, IP adreslerine güvenmeyin.
8. SQL Server TDE'yi kullanarak en son SQL Server yamaları uygulayın.
9. Mümkün olan en yüksek veritabanı sıkıştırmasını kullanın. Hangi SQL Server için sayfa sıkıştırma olduğunu.
10. Azure Marketi'ndeki SQL Server görüntülerini kullanırken dikkatli olun. SQL Server'ı kullanıyorsanız, üzerine sap NetWeaver sistemi yüklemeden önce örnek harmanlamasını değiştirmeniz gerekir.
11. [Dağıtım Kılavuzu'nda][deployment-guide]açıklandığı gibi Azure için SAP Ana Bilgisayar İzleme'yi yükleyin ve yapılandırın.
