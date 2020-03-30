---
title: SAP NetWeaver için Azure Sanal Makineler yüksek kullanılabilirlik | Microsoft Dokümanlar
description: Azure Sanal Makinelerde SAP NetWeaver için yüksek kullanılabilirlik kılavuzu
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: rdeltcheva
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 01/24/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: aa0810818bf7cfea21f925ee639b4b5a50dcb23b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79246129"
---
# <a name="high-availability-for-sap-netweaver-on-azure-vms"></a>Azure VM'lerde SAP NetWeaver için yüksek kullanılabilirlik

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
[2121797]:https://launchpad.support.sap.com/#/notes/2121797
[2134316]:https://launchpad.support.sap.com/#/notes/2134316
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
[2243692]:https://launchpad.support.sap.com/#/notes/2243692

[sap-installation-guides]:http://service.sap.com/instguides

[azure-cli]:../../../cli-install-nodejs.md
[azure-portal]:https://portal.azure.com
[azure-ps]:https://docs.microsoft.com/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-resource-manager/management/azure-subscription-service-limits]:../../../azure-resource-manager/management/azure-subscription-service-limits.md
[azure-resource-manager/management/azure-subscription-service-limits-subscription]:../../../azure-resource-manager/management/azure-subscription-service-limits.md

[dbms-guide]:../../virtual-machines-windows-sap-dbms-guide.md
[dbms-guide-2.1]:../../virtual-machines-windows-sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f
[dbms-guide-2.2]:../../virtual-machines-windows-sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91
[dbms-guide-2.3]:../../virtual-machines-windows-sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152
[dbms-guide-2]:../../virtual-machines-windows-sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64
[dbms-guide-3]:../../virtual-machines-windows-sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3
[dbms-guide-5.5.1]:../../virtual-machines-windows-sap-dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268
[dbms-guide-5.5.2]:../../virtual-machines-windows-sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b
[dbms-guide-5.6]:../../virtual-machines-windows-sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8
[dbms-guide-5.8]:../../virtual-machines-windows-sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30
[dbms-guide-5]:../../virtual-machines-windows-sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737
[dbms-guide-8.4.1]:../../virtual-machines-windows-sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573
[dbms-guide-8.4.2]:../../virtual-machines-windows-sap-dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d
[dbms-guide-8.4.3]:../../virtual-machines-windows-sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c
[dbms-guide-8.4.4]:../../virtual-machines-windows-sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818
[dbms-guide-900-sap-cache-server-on-premises]:../../virtual-machines-windows-sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:../../virtual-machines-windows-sap-deployment-guide.md
[deployment-guide-2.2]:../../virtual-machines-windows-sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94
[deployment-guide-3.1.2]:../../virtual-machines-windows-sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab
[deployment-guide-3.2]:../../virtual-machines-windows-sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281
[deployment-guide-3.3]:../../virtual-machines-windows-sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2
[deployment-guide-3.4]:../../virtual-machines-windows-sap-deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1
[deployment-guide-3]:../../virtual-machines-windows-sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e
[deployment-guide-4.1]:../../virtual-machines-windows-sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7
[deployment-guide-4.2]:../../virtual-machines-windows-sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e
[deployment-guide-4.3]:../../virtual-machines-windows-sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc
[deployment-guide-4.4.2]:../../virtual-machines-windows-sap-deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542
[deployment-guide-4.4]:../../virtual-machines-windows-sap-deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d
[deployment-guide-4.5.1]:../../virtual-machines-windows-sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4
[deployment-guide-4.5.2]:../../virtual-machines-windows-sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f
[deployment-guide-4.5]:../../virtual-machines-windows-sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca
[deployment-guide-5.1]:../../virtual-machines-windows-sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2
[deployment-guide-5.2]:../../virtual-machines-windows-sap-deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1
[deployment-guide-5.3]:../../virtual-machines-windows-sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8

[deployment-guide-configure-monitoring-scenario-1]:../../virtual-machines-windows-sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b
[deployment-guide-configure-proxy]:../../virtual-machines-windows-sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d
[deployment-guide-figure-100]:media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:../../virtual-machines-windows-sap-deployment-guide.md#figure-11
[deployment-guide-figure-1100]:media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:../../virtual-machines-windows-sap-deployment-guide.md#figure-14
[deployment-guide-figure-1400]:media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:../../virtual-machines-windows-sap-deployment-guide.md#figure-5
[deployment-guide-figure-50]:media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:../../virtual-machines-windows-sap-deployment-guide.md#figure-6
[deployment-guide-figure-600]:media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:../../virtual-machines-windows-sap-deployment-guide.md#figure-7
[deployment-guide-figure-700]:media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:../../virtual-machines-windows-sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:../../virtual-machines-windows-sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:../../virtual-machines-windows-sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:../../virtual-machines-windows-sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b

[deploy-template-cli]:../../../resource-group-template-deploy.md#deploy-with-azure-cli-for-mac-linux-and-windows
[deploy-template-portal]:../../../resource-group-template-deploy.md#deploy-with-the-preview-portal
[deploy-template-powershell]:../../../resource-group-template-deploy.md#deploy-with-powershell

[dr-guide-classic]:https://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:../../virtual-machines-windows-sap-get-started.md
[getting-started-dbms]:../../virtual-machines-windows-sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:../../virtual-machines-windows-sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:../../virtual-machines-windows-sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:../../virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:../../virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:../../virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:../../virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:../../virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:../../virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:https://go.microsoft.com/fwlink/?LinkId=613056

[ha-guide]:high-availability-guide.md

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

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

[sap-ha-guide]:high-availability-guide.md
[sap-ha-guide-1]:high-availability-guide.md#217c5479-5595-4cd8-870d-15ab00d4f84c
[sap-ha-guide-2]:high-availability-guide.md#42b8f600-7ba3-4606-b8a5-53c4f026da08
[sap-ha-guide-3]:high-availability-guide.md#42156640c6-01cf-45a9-b225-4baa678b24f1
[sap-ha-guide-3.1]:high-availability-guide.md#f76af273-1993-4d83-b12d-65deeae23686
[sap-ha-guide-3.2]:high-availability-guide.md#3e85fbe0-84b1-4892-87af-d9b65ff91860
[sap-ha-guide-4]:high-availability-guide.md#8ecf3ba0-67c0-4495-9c14-feec1a2255b7
[sap-ha-guide-4.1]:high-availability-guide.md#1a3c5408-b168-46d6-99f5-4219ad1b1ff2
[sap-ha-guide-5]:high-availability-guide.md#fdfee875-6e66-483a-a343-14bbaee33275
[sap-ha-guide-5.1]:high-availability-guide.md#be21cf3e-fb01-402b-9955-54fbecf66592
[sap-ha-guide-5.2]:high-availability-guide.md#ff7a9a06-2bc5-4b20-860a-46cdb44669cd
[sap-ha-guide-6]:high-availability-guide.md#2ddba413-a7f5-4e4e-9a51-87908879c10a
[sap-ha-guide-6.1]:high-availability-guide.md#1a464091-922b-48d7-9d08-7cecf757f341
[sap-ha-guide-6.2]:high-availability-guide.md#44641e18-a94e-431f-95ff-303ab65e0bcb
[sap-ha-guide-7]:high-availability-guide.md#2e3fec50-241e-441b-8708-0b1864f66dfa
[sap-ha-guide-7.1]:high-availability-guide.md#93faa747-907e-440a-b00a-1ae0a89b1c0e
[sap-ha-guide-7.2]:high-availability-guide.md#f559c285-ee68-4eec-add1-f60fe7b978db
[sap-ha-guide-7.2.1]:high-availability-guide.md#b5b1fd0b-1db4-4d49-9162-de07a0132a51
[sap-ha-guide-7.3]:high-availability-guide.md#ddd878a0-9c2f-4b8e-8968-26ce60be1027
[sap-ha-guide-7.4]:high-availability-guide.md#045252ed-0277-4fc8-8f46-c5a29694a816
[sap-ha-guide-8]:high-availability-guide.md#78092dbe-165b-454c-92f5-4972bdbef9bf
[sap-ha-guide-8.1]:high-availability-guide.md#c87a8d3f-b1dc-4d2f-b23c-da4b72977489
[sap-ha-guide-8.2]:high-availability-guide.md#7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310
[sap-ha-guide-8.3]:high-availability-guide.md#47d5300a-a830-41d4-83dd-1a0d1ffdbe6a
[sap-ha-guide-8.4]:high-availability-guide.md#b22d7b3b-4343-40ff-a319-097e13f62f9e
[sap-ha-guide-8.5]:high-availability-guide.md#9fbd43c0-5850-4965-9726-2a921d85d73f
[sap-ha-guide-8.6]:high-availability-guide.md#84c019fe-8c58-4dac-9e54-173efd4b2c30
[sap-ha-guide-8.7]:high-availability-guide.md#7a8f3e9b-0624-4051-9e41-b73fff816a9e
[sap-ha-guide-8.8]:high-availability-guide.md#f19bd997-154d-4583-a46e-7f5a69d0153c
[sap-ha-guide-8.9]:high-availability-guide.md#fe0bd8b5-2b43-45e3-8295-80bee5415716
[sap-ha-guide-8.10]:high-availability-guide.md#e69e9a34-4601-47a3-a41c-d2e11c626c0c
[sap-ha-guide-8.11]:high-availability-guide.md#661035b2-4d0f-4d31-86f8-dc0a50d78158
[sap-ha-guide-8.12]:high-availability-guide.md#0d67f090-7928-43e0-8772-5ccbf8f59aab
[sap-ha-guide-8.12.1]:high-availability-guide.md#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79
[sap-ha-guide-8.12.2]:high-availability-guide.md#e49a4529-50c9-4dcf-bde7-15a0c21d21ca
[sap-ha-guide-8.12.2.1]:high-availability-guide.md#06260b30-d697-4c4d-b1c9-d22c0bd64855
[sap-ha-guide-8.12.2.2]:high-availability-guide.md#4c08c387-78a0-46b1-9d27-b497b08cac3d
[sap-ha-guide-8.12.3]:high-availability-guide.md#5c8e5482-841e-45e1-a89d-a05c0907c868
[sap-ha-guide-8.12.3.1]:high-availability-guide.md#1c2788c3-3648-4e82-9e0d-e058e475e2a3
[sap-ha-guide-8.12.3.2]:high-availability-guide.md#dd41d5a2-8083-415b-9878-839652812102
[sap-ha-guide-8.12.3.3]:high-availability-guide.md#d9c1fc8e-8710-4dff-bec2-1f535db7b006
[sap-ha-guide-9]:high-availability-guide.md#a06f0b49-8a7a-42bf-8b0d-c12026c5746b
[sap-ha-guide-9.1]:high-availability-guide.md#31c6bd4f-51df-4057-9fdf-3fcbc619c170
[sap-ha-guide-9.1.1]:high-availability-guide.md#a97ad604-9094-44fe-a364-f89cb39bf097
[sap-ha-guide-9.1.2]:high-availability-guide.md#eb5af918-b42f-4803-bb50-eff41f84b0b0
[sap-ha-guide-9.1.3]:high-availability-guide.md#e4caaab2-e90f-4f2c-bc84-2cd2e12a9556
[sap-ha-guide-9.1.4]:high-availability-guide.md#10822f4f-32e7-4871-b63a-9b86c76ce761
[sap-ha-guide-9.1.5]:high-availability-guide.md#4498c707-86c0-4cde-9c69-058a7ab8c3ac
[sap-ha-guide-9.2]:high-availability-guide.md#85d78414-b21d-4097-92b6-34d8bcb724b7
[sap-ha-guide-9.3]:high-availability-guide.md#8a276e16-f507-4071-b829-cdc0a4d36748
[sap-ha-guide-9.4]:high-availability-guide.md#094bc895-31d4-4471-91cc-1513b64e406a
[sap-ha-guide-9.5]:high-availability-guide.md#2477e58f-c5a7-4a5d-9ae3-7b91022cafb5
[sap-ha-guide-9.6]:high-availability-guide.md#0ba4a6c1-cc37-4bcf-a8dc-025de4263772
[sap-ha-guide-10]:high-availability-guide.md#18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9
[sap-ha-guide-10.1]:high-availability-guide.md#65fdef0f-9f94-41f9-b314-ea45bbfea445
[sap-ha-guide-10.2]:high-availability-guide.md#5e959fa9-8fcd-49e5-a12c-37f6ba07b916
[sap-ha-guide-10.3]:high-availability-guide.md#755a6b93-0099-4533-9f6d-5c9a613878b5

[sap-ha-multi-sid-guide]:high-availability-multi-sid.md (SAP multi-SID yüksek kullanılabilirlik yapılandırması)


[sap-ha-guide-figure-1000]:media/virtual-machines-shared-sap-high-availability-guide/1000-wsfc-for-sap-ascs-on-azure.png
[sap-ha-guide-figure-1001]:media/virtual-machines-shared-sap-high-availability-guide/1001-wsfc-on-azure-ilb.png
[sap-ha-guide-figure-1002]:media/virtual-machines-shared-sap-high-availability-guide/1002-wsfc-sios-on-azure-ilb.png
[sap-ha-guide-figure-2000]:media/virtual-machines-shared-sap-high-availability-guide/2000-wsfc-sap-as-ha-on-azure.png
[sap-ha-guide-figure-2001]:media/virtual-machines-shared-sap-high-availability-guide/2001-wsfc-sap-ascs-ha-on-azure.png
[sap-ha-guide-figure-2003]:media/virtual-machines-shared-sap-high-availability-guide/2003-wsfc-sap-dbms-ha-on-azure.png
[sap-ha-guide-figure-2004]:media/virtual-machines-shared-sap-high-availability-guide/2004-wsfc-sap-ha-e2e-archit-template1-on-azure.png
[sap-ha-guide-figure-2005]:media/virtual-machines-shared-sap-high-availability-guide/2005-wsfc-sap-ha-e2e-arch-template2-on-azure.png

[sap-ha-guide-figure-3000]:media/virtual-machines-shared-sap-high-availability-guide/3000-template-parameters-sap-ha-arm-on-azure.png
[sap-ha-guide-figure-3001]:media/virtual-machines-shared-sap-high-availability-guide/3001-configuring-dns-servers-for-Azure-vnet.png
[sap-ha-guide-figure-3002]:media/virtual-machines-shared-sap-high-availability-guide/3002-configuring-static-IP-address-for-network-card-of-each-vm.png
[sap-ha-guide-figure-3003]:media/virtual-machines-shared-sap-high-availability-guide/3003-setup-static-ip-address-ilb-for-ascs-instance.png
[sap-ha-guide-figure-3004]:media/virtual-machines-shared-sap-high-availability-guide/3004-default-ascs-scs-ilb-balancing-rules-for-azure-ilb.png
[sap-ha-guide-figure-3005]:media/virtual-machines-shared-sap-high-availability-guide/3005-changing-ascs-scs-default-ilb-rules-for-azure-ilb.png
[sap-ha-guide-figure-3006]:media/virtual-machines-shared-sap-high-availability-guide/3006-adding-vm-to-domain.png
[sap-ha-guide-figure-3007]:media/virtual-machines-shared-sap-high-availability-guide/3007-config-wsfc-1.png
[sap-ha-guide-figure-3008]:media/virtual-machines-shared-sap-high-availability-guide/3008-config-wsfc-2.png
[sap-ha-guide-figure-3009]:media/virtual-machines-shared-sap-high-availability-guide/3009-config-wsfc-3.png
[sap-ha-guide-figure-3010]:media/virtual-machines-shared-sap-high-availability-guide/3010-config-wsfc-4.png
[sap-ha-guide-figure-3011]:media/virtual-machines-shared-sap-high-availability-guide/3011-config-wsfc-5.png
[sap-ha-guide-figure-3012]:media/virtual-machines-shared-sap-high-availability-guide/3012-config-wsfc-6.png
[sap-ha-guide-figure-3013]:media/virtual-machines-shared-sap-high-availability-guide/3013-config-wsfc-7.png
[sap-ha-guide-figure-3014]:media/virtual-machines-shared-sap-high-availability-guide/3014-config-wsfc-8.png
[sap-ha-guide-figure-3015]:media/virtual-machines-shared-sap-high-availability-guide/3015-config-wsfc-9.png
[sap-ha-guide-figure-3016]:media/virtual-machines-shared-sap-high-availability-guide/3016-config-wsfc-10.png
[sap-ha-guide-figure-3017]:media/virtual-machines-shared-sap-high-availability-guide/3017-config-wsfc-11.png
[sap-ha-guide-figure-3018]:media/virtual-machines-shared-sap-high-availability-guide/3018-config-wsfc-12.png
[sap-ha-guide-figure-3019]:media/virtual-machines-shared-sap-high-availability-guide/3019-assign-permissions-on-share-for-cluster-name-object.png
[sap-ha-guide-figure-3020]:media/virtual-machines-shared-sap-high-availability-guide/3020-change-object-type-include-computer-objects.png
[sap-ha-guide-figure-3021]:media/virtual-machines-shared-sap-high-availability-guide/3021-check-box-for-computer-objects.png
[sap-ha-guide-figure-3022]:media/virtual-machines-shared-sap-high-availability-guide/3022-set-security-attributes-for-cluster-name-object-on-file-share-quorum.png
[sap-ha-guide-figure-3023]:media/virtual-machines-shared-sap-high-availability-guide/3023-call-configure-cluster-quorum-setting-wizard.png
[sap-ha-guide-figure-3024]:media/virtual-machines-shared-sap-high-availability-guide/3024-selection-screen-different-quorum-configurations.png
[sap-ha-guide-figure-3025]:media/virtual-machines-shared-sap-high-availability-guide/3025-selection-screen-file-share-witness.png
[sap-ha-guide-figure-3026]:media/virtual-machines-shared-sap-high-availability-guide/3026-define-file-share-location-for-witness-share.png
[sap-ha-guide-figure-3027]:media/virtual-machines-shared-sap-high-availability-guide/3027-successful-reconfiguration-cluster-file-share-witness.png
[sap-ha-guide-figure-3028]:media/virtual-machines-shared-sap-high-availability-guide/3028-install-dot-net-framework-35.png
[sap-ha-guide-figure-3029]:media/virtual-machines-shared-sap-high-availability-guide/3029-install-dot-net-framework-35-progress.png
[sap-ha-guide-figure-3030]:media/virtual-machines-shared-sap-high-availability-guide/3030-sios-installer.png
[sap-ha-guide-figure-3031]:media/virtual-machines-shared-sap-high-availability-guide/3031-first-screen-sios-data-keeper-installation.png
[sap-ha-guide-figure-3032]:media/virtual-machines-shared-sap-high-availability-guide/3032-data-keeper-informs-service-be-disabled.png
[sap-ha-guide-figure-3033]:media/virtual-machines-shared-sap-high-availability-guide/3033-user-selection-sios-data-keeper.png
[sap-ha-guide-figure-3034]:media/virtual-machines-shared-sap-high-availability-guide/3034-domain-user-sios-data-keeper.png
[sap-ha-guide-figure-3035]:media/virtual-machines-shared-sap-high-availability-guide/3035-provide-sios-data-keeper-license.png
[sap-ha-guide-figure-3036]:media/virtual-machines-shared-sap-high-availability-guide/3036-data-keeper-management-config-tool.png
[sap-ha-guide-figure-3037]:media/virtual-machines-shared-sap-high-availability-guide/3037-tcp-ip-address-first-node-data-keeper.png
[sap-ha-guide-figure-3038]:media/virtual-machines-shared-sap-high-availability-guide/3038-create-replication-sios-job.png
[sap-ha-guide-figure-3039]:media/virtual-machines-shared-sap-high-availability-guide/3039-define-sios-replication-job-name.png
[sap-ha-guide-figure-3040]:media/virtual-machines-shared-sap-high-availability-guide/3040-define-sios-source-node.png
[sap-ha-guide-figure-3041]:media/virtual-machines-shared-sap-high-availability-guide/3041-define-sios-target-node.png
[sap-ha-guide-figure-3042]:media/virtual-machines-shared-sap-high-availability-guide/3042-define-sios-synchronous-replication.png
[sap-ha-guide-figure-3043]:media/virtual-machines-shared-sap-high-availability-guide/3043-enable-sios-replicated-volume-as-cluster-volume.png
[sap-ha-guide-figure-3044]:media/virtual-machines-shared-sap-high-availability-guide/3044-data-keeper-synchronous-mirroring-for-SAP-gui.png
[sap-ha-guide-figure-3045]:media/virtual-machines-shared-sap-high-availability-guide/3045-replicated-disk-by-data-keeper-in-wsfc.png
[sap-ha-guide-figure-3046]:media/virtual-machines-shared-sap-high-availability-guide/3046-dns-entry-sap-ascs-virtual-name-ip.png
[sap-ha-guide-figure-3047]:media/virtual-machines-shared-sap-high-availability-guide/3047-dns-manager.png
[sap-ha-guide-figure-3048]:media/virtual-machines-shared-sap-high-availability-guide/3048-default-cluster-probe-port.png
[sap-ha-guide-figure-3049]:media/virtual-machines-shared-sap-high-availability-guide/3049-cluster-probe-port-after.png
[sap-ha-guide-figure-3050]:media/virtual-machines-shared-sap-high-availability-guide/3050-service-type-ers-delayed-automatic.png
[sap-ha-guide-figure-5000]:media/virtual-machines-shared-sap-high-availability-guide/5000-wsfc-sap-sid-node-a.png
[sap-ha-guide-figure-5001]:media/virtual-machines-shared-sap-high-availability-guide/5001-sios-replicating-local-volume.png
[sap-ha-guide-figure-5002]:media/virtual-machines-shared-sap-high-availability-guide/5002-wsfc-sap-sid-node-b.png
[sap-ha-guide-figure-5003]:media/virtual-machines-shared-sap-high-availability-guide/5003-sios-replicating-local-volume-b-to-a.png

[sap-ha-guide-figure-6003]:media/virtual-machines-shared-sap-high-availability-guide/6003-sap-multi-sid-full-landscape.png

[resource-group-authoring-templates]:../../../resource-group-authoring-templates.md
[resource-group-overview]:../../../../../azure-resource-manager/management/overview.md
[resource-groups-networking]:../../../networking/networking-overview.md
[sap-pam]:https://support.sap.com/pam (SAP Ürün Kullanılabilirlik Matrisi)
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-multisid-xscs-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps%2Fazuredeploy.json
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
[virtual-machines-windows-attach-disk-portal]:../../virtual-machines-windows-attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../../../azure-resource-manager/management/overview.md
[virtual-machines-azure-resource-manager-architecture-benefits-arm]:../../../azure-resource-manager/management/overview.md#the-benefits-of-using-resource-manager
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-windows-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md
[virtual-machines-linux-agent-user-guide]:../../linux/agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../linux/agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-capture]:../../linux/capture-image.md#capture-the-vm
[virtual-machines-windows-capture-image]:../../virtual-machines-windows-capture-image.md
[virtual-machines-windows-capture-image-capture]:../../virtual-machines-windows-capture-image.md#capture-the-vm
[virtual-machines-linux-configure-lvm]:../../linux/configure-lvm.md
[virtual-machines-linux-configure-raid]:../../linux/configure-raid.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../../linux/suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../../linux/update-agent.md
[virtual-machines-manage-availability]:../../virtual-machines-windows-manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:../../virtual-machines-windows-ps-create.md
[virtual-machines-sizes]:../../virtual-machines-windows-sizes.md
[virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]:../../windows/sql/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md
[virtual-machines-windows-portal-sql-alwayson-int-listener]:../../windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md
[virtual-machines-upload-image-windows-resource-manager]:../../virtual-machines-windows-upload-image.md
[virtual-machines-windows-tutorial]:../../virtual-machines-windows-hero-tutorial.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/documentation/templates/sql-server-2014-alwayson-dsc/
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
[vpn-gateway-site-to-site-create]:../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md
[vpn-gateway-vpn-faq]:../../../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../../../cli-install-nodejs.md
[xplat-cli-azure-resource-manager]:../../../xplat-cli-azure-resource-manager.md


Azure Sanal Makineler, bilgi işlem, depolama ve ağ kaynaklarına en az sürede ve uzun satın alma döngüleri olmadan ihtiyaç duyan kuruluşlar için çözümdür. SAP NetWeaver tabanlı ABAP, Java ve ABAP+Java yığını gibi klasik uygulamaları dağıtmak için Azure Sanal Makineleri'ni kullanabilirsiniz. Ek şirket içi kaynaklar olmadan güvenilirliği ve kullanılabilirliği artırın. Azure Sanal Makineler, azure sanal makinelerini kuruluşunuzun şirket içi etki alanına, özel bulutlara ve SAP sistem ortamına entegre edebilirsiniz.

Bu makalede, Azure Kaynak Yöneticisi dağıtım modelini kullanarak Azure'da yüksek kullanılabilirlik sap sistemlerini dağıtmak için atabileceğiniz adımları kapsamaktayız. Size şu önemli görevlerde yol sayılacağız:

* [Kaynaklar][sap-ha-guide-2] bölümünde listelenen doğru SAP Notlarını ve yükleme kılavuzlarını bulun. Bu makale, SAP yazılımını belirli platformlara yüklemenize ve dağıtmanıza yardımcı olabilecek birincil kaynaklar olan SAP yükleme belgelerini ve SAP Notlarını tamamlar.
* Azure Kaynak Yöneticisi dağıtım modeli ile Azure klasik dağıtım modeli arasındaki farkları öğrenin.
* Azure dağıtımınız için doğru modeli seçebilmeniz için Windows Server Failover Kümeleme çoğunluk akış modları hakkında bilgi edinin.
* Azure hizmetlerinde Paylaşılan depolama alanı Windows Server Failover Clustering hakkında bilgi edinin.
* Gelişmiş İş Uygulama Programlama (ABAP) SAP Merkezi Hizmetleri (ASCS)/SAP Merkezi Hizmetleri (SCS) ve veritabanı yönetim sistemleri (DBMS) ve SAP Application Server gibi gereksiz bileşenler gibi tek noktalı hata bileşenlerinin Azure'da korunmasına nasıl yardımcı oylamayı öğrenin.
* Azure Kaynak Yöneticisi'ni kullanarak Azure'daki Windows Server Failover Kümeleme kümesinde yüksek kullanılabilirliksap sisteminin kurulumu ve yapılandırması için adım adım bir örnek izleyin.
* Azure'da Windows Server Failover Kümeleme'yi kullanmak için gereken ancak şirket içi dağıtımda gerekli olmayan ek adımlar hakkında bilgi edinin.

Dağıtım ve yapılandırmayı basitleştirmek için, bu makalede SAP üç katmanlı yüksek kullanılabilirlik Kaynak Yöneticisi şablonlarını kullanırız. Şablonlar, yüksek kullanılabilirliksap sistemi için ihtiyacınız olan tüm altyapıdağıtımıotomatikleştirin. Altyapı, SAP sisteminizin SAP Uygulama Performans Standardı (SAPS) boyutlandırmasını da destekler.

## <a name="prerequisites"></a><a name="217c5479-5595-4cd8-870d-15ab00d4f84c"></a>Önkoşullar
Başlamadan önce, aşağıdaki bölümlerde açıklanan ön koşulları karşıladığınızdan emin olun. Ayrıca, [Kaynaklar][sap-ha-guide-2] bölümünde listelenen tüm kaynakları kontrol ettiğinizden emin olun.

Bu makalede, [üç katmanlı SAP NetWeaver](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image/)için Azure Kaynak Yöneticisi şablonlarını kullanıyoruz. Şablonlara yararlı bir genel bakış için SAP [Azure Kaynak Yöneticisi şablonlarına](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/)bakın.

## <a name="resources"></a><a name="42b8f600-7ba3-4606-b8a5-53c4f026da08"></a>Kaynak
Bu makaleler Azure'daki SAP dağıtımlarını kapsar:

* [SAP NetWeaver için Azure Sanal Makineler planlaması ve uygulaması][planning-guide]
* [SAP NetWeaver için Azure Sanal Makineler dağıtımı][deployment-guide]
* [SAP NetWeaver için Azure Sanal Makineler DBMS dağıtımı][dbms-guide]
* [SAP NetWeaver için Azure Sanal Makineler yüksek kullanılabilirlik (bu kılavuz)][sap-ha-guide]

> [!NOTE]
> Mümkün olduğunda, size başvuran SAP yükleme kılavuzuna bir bağlantı veriyoruz [(SAP yükleme kılavuzlarına][sap-installation-guides]bakın). Yükleme işlemi hakkında ön koşullar ve bilgiler için SAP NetWeaver yükleme kılavuzlarını dikkatle okumak iyi bir fikirdir. Bu makalede, Azure Sanal Makineleri ile kullanabileceğiniz SAP NetWeaver tabanlı sistemler için yalnızca belirli görevleri kapsar.
>
>

Bu SAP Notları Azure'daki SAP konusuyla ilgilidir:

| Not numarası | Başlık |
| --- | --- |
| [1928533] |Azure'da SAP Uygulamaları: Desteklenen Ürünler ve Boyutlandırma |
| [2015553] |Microsoft Azure'da SAP: Ön Koşulları Destekle |
| [1999351] |SAP için Geliştirilmiş Azure İzleme |
| [2178632] |Microsoft Azure'da SAP için Temel İzleme Ölçümleri |
| [1999351] |Windows'da Sanallaştırma: Gelişmiş İzleme |
| [2243692] |SAP DBMS Örneği için Azure Premium SSD Depolama Kullanımı |

Genel varsayılan sınırlamalar ve maksimum sınırlamalar da dahil olmak üzere [Azure aboneliklerinin sınırlamaları][azure-resource-manager/management/azure-subscription-service-limits-subscription]hakkında daha fazla bilgi edinin.

## <a name="high-availability-sap-with-azure-resource-manager-vs-the-azure-classic-deployment-model"></a><a name="42156640c6-01cf-45a9-b225-4baa678b24f1"></a>Azure Kaynak Yöneticisi ile yüksek kullanılabilirlik SAP ve Azure klasik dağıtım modeli
Azure Kaynak Yöneticisi ve Azure klasik dağıtım modelleri aşağıdaki alanlarda farklıdır:

- Kaynak grupları
- Azure kaynak grubuna azure dahili yük dengeleyici bağımlılığı
- SAP multi-SID senaryoları için destek

### <a name="resource-groups"></a><a name="f76af273-1993-4d83-b12d-65deeae23686"></a>Kaynak grupları
Azure Kaynak Yöneticisi'nde, Azure aboneliğinizdeki tüm uygulama kaynaklarını yönetmek için kaynak gruplarını kullanabilirsiniz. Tümleşik bir yaklaşım, bir kaynak grubunda, tüm kaynaklar aynı yaşam döngüsüne sahiptir. Örneğin, tüm kaynaklar aynı anda oluşturulur ve aynı anda silinir. [Kaynak grupları](../../../azure-resource-manager/management/overview.md#resource-groups) hakkında daha fazla bilgi edinin.

### <a name="azure-internal-load-balancer-dependency-on-the-azure-resource-group"></a><a name="3e85fbe0-84b1-4892-87af-d9b65ff91860"></a>Azure kaynak grubuna azure dahili yük dengeleyici bağımlılığı

Azure klasik dağıtım modelinde, Azure dahili yük dengeleyicisi (Azure Yük Dengeleyicihizmeti) ile bulut hizmeti grubu arasında bir bağımlılık vardır. Her dahili yük dengeleyicinin bir bulut hizmet grubuna ihtiyacı vardır.

Azure Kaynak Yöneticisi'nde, Azure Yük Bakiyesi'ni kullanmak için bir Azure kaynak grubuna ihtiyacınız yoktur. Ortam daha basit ve esnektir.

### <a name="support-for-sap-multi-sid-scenarios"></a>SAP multi-SID senaryoları için destek

Azure Kaynak Yöneticisi'nde, birden çok SAP sistem tanımlayıcısı (SID) ASCS/SCS örneğini tek bir kümeye yükleyebilirsiniz. Multi-SID örnekleri, her Azure dahili yük dengeleyicisi için birden çok IP adresi desteği sayesinde mümkündür.

Azure klasik dağıtım modelini kullanmak için, [Azure'da SAP NetWeaver'da açıklanan yordamları izleyin: SIOS DataKeeper ile Azure'da Windows Server Failover Clustering'i kullanarak SAP ASCS/SCS örneklerini kümeleme.](https://go.microsoft.com/fwlink/?LinkId=613056)

> [!IMPORTANT]
> SAP yüklemeleriniz için Azure Kaynak Yöneticisi dağıtım modelini kullanmanızı şiddetle öneririz. Klasik dağıtım modelinde bulunmayan birçok avantaj sunar. Azure [dağıtım modelleri][virtual-machines-azure-resource-manager-architecture-benefits-arm] hakkında daha fazla bilgi edinin.   
>
>

## <a name="windows-server-failover-clustering"></a><a name="8ecf3ba0-67c0-4495-9c14-feec1a2255b7"></a>Windows Server Failover Kümeleme
Windows Server Failover Clustering, Windows'da yüksek kullanılabilirlikte SAP ASCS/SCS yüklemesinin ve DBMS'nin temelidir.

Başarısız küme, uygulamaların ve hizmetlerin kullanılabilirliğini artırmak için birlikte çalışan 1+n bağımsız sunuculardan (düğümler) oluşan bir gruptur. Bir düğüm hatası oluşursa, Windows Server Failover Clustering, uygulamaları ve hizmetleri sağlamak için sağlıklı bir küme tutarken oluşabilecek hata sayısını hesaplar. Kümeleme de başarısız olmak için farklı çoğunluk modları arasından seçim yapabilirsiniz.

### <a name="quorum-modes"></a><a name="1a3c5408-b168-46d6-99f5-4219ad1b1ff2"></a>Çoğunluk modları
Windows Server Failover Clustering'i kullandığınızda dört çoğunluk modu arasından seçim yapabilirsiniz:

* **Düğüm Çoğunluk**. Kümenin her düğümü oy kullanabilir. Küme sadece oyların çoğunluğu ile, yani yarısından fazlası ile çalışır. Bu seçeneği, eşit olmayan sayıda düğüme sahip kümeler için öneririz. Örneğin, yedi düğümlü kümedeki üç düğüm başarısız olabilir ve küme hala çoğunluk elde eder ve çalıştırmaya devam eder.  
* **Düğüm ve Disk Çoğunluğu.** Küme depolamasındaki her düğüm ve belirlenmiş bir disk (disk tanığı) kullanılabilir olduklarında ve iletişimde oy kullanabilir. Küme sadece oyların çoğunluğu ile, yani yarısından fazlası ile çalışır. Bu mod, çift düğüm sayısına sahip bir küme ortamında anlamlıdır. Düğümlerin ve diskin yarısı çevrimiçiyse, küme sağlıklı bir durumda kalır.
* **Düğüm ve Dosya Paylaşımı Çoğunluğu.** Her düğüm artı yöneticinin oluşturduğu atanmış bir dosya paylaşımı (dosya paylaşımı tanığı), düğümlerin ve dosya paylaşımının kullanılabilir olup olmadığına bakılmaksızın oy kullanabilir. Küme sadece oyların çoğunluğu ile, yani yarısından fazlası ile çalışır. Bu mod, çift düğüm sayısına sahip bir küme ortamında anlamlıdır. Düğüm ve Disk Çoğunluğu moduna benzer, ancak tanık diski yerine tanık dosyası paylaşımı kullanır. Bu modu uygulamak kolaydır, ancak dosya paylaşımının kendisi çok kullanılamıyorsa, tek bir hata noktası haline gelebilir.
* **Çoğunluk Yok: Yalnızca Disk.** Küme, bir düğüm varsa ve küme depolamasında belirli bir diskle iletişim halindeyse, kümenin bir çoğunluğu vardır. Kümeye yalnızca bu diskle iletişim halinde olan düğümler katılabilir. Bu modu kullanmamanızı öneririz.

## <a name="windows-server-failover-clustering-on-premises"></a><a name="fdfee875-6e66-483a-a343-14bbaee33275"></a>Windows Server Failover Kümeleme şirket içi
Şekil 1 iki düğümden oluşan bir küme gösterir. Düğümler arasındaki ağ bağlantısı başarısız olursa ve her iki düğüm de çalışır durumda kalırsa, çoğunluk diski veya dosya paylaşımı kümenin uygulamalarını ve hizmetlerini sağlamaya devam edecek düğümü belirler. Çoğunluk diskine veya dosya paylaşımına erişimi olan düğüm, hizmetlerin devam etmesini sağlayan düğümdür.

Bu örnekte iki düğümlü küme kullandığından, Düğüm ve Dosya Paylaşımı Çoğunluk çoğunluk modunu kullanırız. Düğüm ve Disk Çoğunluğu da geçerli bir seçenektir. Üretim ortamında, bir çoğunluk diski kullanmanızı öneririz. Ağ ve depolama sistemi teknolojisini kullanılabilir hale getirmek için kullanabilirsiniz.

![Şekil 1: Azure'da SAP ASCS/SCS için Windows Server Failover Kümeleme yapılandırması örneği][sap-ha-guide-figure-1000]

_**Şekil 1:** Azure'da SAP ASCS/SCS için Windows Server Failover Kümeleme yapılandırması örneği_

### <a name="shared-storage"></a><a name="be21cf3e-fb01-402b-9955-54fbecf66592"></a>Paylaşılan depolama
Şekil 1 ayrıca iki düğümlü paylaşılan depolama kümesini de gösterir. Şirket içi paylaşılan depolama kümesinde, kümedeki tüm düğümler paylaşılan depolamayı algılar. Kilitleme mekanizması verileri bozulmadan korur. Başka bir düğüm başarısız olup olmadığını tüm düğümler algılayabilir. Bir düğüm başarısız olursa, kalan düğüm depolama kaynaklarının sahipliğini alır ve hizmetlerin kullanılabilirliğini sağlar.

> [!NOTE]
> SQL Server'da olduğu gibi bazı DBMS uygulamalarında yüksek kullanılabilirlik için paylaşılan disklere ihtiyacınız yoktur. SQL Server Always On, DBMS verilerini ve günlük dosyalarını bir küme düğümünün yerel diskinden başka bir küme düğümünün yerel diskine kopyalar. Bu durumda, Windows küme yapılandırmasının paylaşılan bir diske ihtiyacı yoktur.
>
>

### <a name="networking-and-name-resolution"></a><a name="ff7a9a06-2bc5-4b20-860a-46cdb44669cd"></a>Ağ ve ad çözünürlüğü
İstemci bilgisayarlar kümeye sanal bir IP adresi ve DNS sunucusunun sağladığı sanal ana bilgisayar adı üzerinden ulaşır. Şirket içi düğümler ve DNS sunucusu birden çok IP adresini işleyebilir.

Tipik bir kurulumda, iki veya daha fazla ağ bağlantısı kullanırsınız:

* Depolama ya da depolama alanına özel bir bağlantı
* Sinyal atışını bekleyen küme-iç ağ bağlantısı
* İstemcilerin kümeye bağlanmak için kullandığı ortak ağ

## <a name="windows-server-failover-clustering-in-azure"></a><a name="2ddba413-a7f5-4e4e-9a51-87908879c10a"></a>Azure'da Windows Server Failover Kümeleme
Azure Sanal Makineler, çıplak metal veya özel bulut dağıtımları ile karşılaştırıldığında, Windows Server Failover Kümeleme'yi yapılandırmak için ek adımlar gerektirir. Paylaşılan bir küme diski oluşturduğunuzda, SAP ASCS/SCS örneği için birkaç IP adresi ve sanal ana bilgisayar adları ayarlamanız gerekir.

Bu makalede, Azure'da sap yüksek kullanılabilirlik merkezi hizmetler kümesi oluşturmak için gereken temel kavramları ve ek adımları tartışıyoruz. Size üçüncü taraf aracı SIOS DataKeeper'ı nasıl ayarlayabileceğinizi ve Azure dahili yük bakiyesini nasıl yapılandırabileceğinizi gösteriyoruz. Bu araçları, Azure'da bir dosya paylaşım tanığıyla bir Windows failover kümesi oluşturmak için kullanabilirsiniz.

![Şekil 2: Paylaşılan diskolmadan Azure'da Windows Server Failover Kümeleme yapılandırması][sap-ha-guide-figure-1001]

_**Şekil 2:** Paylaşılan bir disk olmadan Azure'da Windows Server Failover Kümeleme yapılandırması_

### <a name="shared-disk-in-azure-with-sios-datakeeper"></a><a name="1a464091-922b-48d7-9d08-7cecf757f341"></a>SIOS DataKeeper ile Azure'da paylaşılan disk
Yüksek kullanılabilirlik sap ASCS/SCS örneği için küme paylaşılan depolama alanına ihtiyacınız vardır. Azure, Eylül 2016 itibarıyla paylaşılan bir depolama kümesi oluşturmak için kullanabileceğiniz paylaşılan depolama alanı sunmaz. Küme paylaşılan depolama yı taklit eden aynalı bir depolama oluşturmak için üçüncü taraf yazılımı SIOS DataKeeper Cluster Edition'ı kullanabilirsiniz. SIOS çözümü gerçek zamanlı senkron veri çoğaltma sağlar. Küme için paylaşılan bir disk kaynağını şu şekilde oluşturabilirsiniz:

1. Windows küme yapılandırmasındaki sanal makinelerin (VM'ler) her birine ek bir Azure sanal sabit disk (VHD) takın.
2. SIOS DataKeeper Cluster Edition'ı her iki sanal makine düğümünde çalıştırın.
3. SIOS DataKeeper Cluster Edition'ı, kaynak sanal makineden ek VHD ekli ses düzeyinin içeriğini hedef sanal makinenin ek VHD ekli hacmine yansıtacak şekilde yapılandırın. SIOS DataKeeper, kaynağı özetler ve yerel birimleri hedef alır ve bunları paylaşılan tek bir disk olarak Windows Server Failover Clustering'e sunar.

[SIOS DataKeeper](https://us.sios.com/products/datakeeper-cluster/)hakkında daha fazla bilgi alın.

![Şekil 3: SIOS DataKeeper ile Azure'da Windows Server Failover Kümeleme yapılandırması][sap-ha-guide-figure-1002]

_**Şekil 3:** SIOS DataKeeper ile Azure'da Windows Server Failover Kümeleme yapılandırması_

> [!NOTE]
> SQL Server gibi bazı DBMS ürünlerinde yüksek kullanılabilirlik için paylaşılan disklere ihtiyacınız yoktur. SQL Server Always On, DBMS verilerini ve günlük dosyalarını bir küme düğümünün yerel diskinden başka bir küme düğümünün yerel diskine kopyalar. Bu durumda, Windows küme yapılandırmasının paylaşılan bir diske ihtiyacı yoktur.
>
>

### <a name="name-resolution-in-azure"></a><a name="44641e18-a94e-431f-95ff-303ab65e0bcb"></a>Azure'da ad çözümü
Azure bulut platformu, kayan IP adresleri gibi sanal IP adreslerini yapılandırma seçeneği sunmaz. Buluttaki küme kaynağına ulaşmak için sanal bir IP adresi ayarlamak için alternatif bir çözüme ihtiyacınız var.
Azure'un Azure Yük Dengeleyicisi hizmetinde bir dahili yük dengeleyicisi vardır. Dahili yük dengeleyicisi ile istemciler kümesanal IP adresi üzerinden kümeye ulaşır.
Küme düğümlerini içeren kaynak grubunda iç yük dengeleyicisini dağıtmanız gerekir. Ardından, gerekli tüm bağlantı noktası iletme kurallarını dahili yük dengeleyicisinin prob bağlantı noktalarıyla yapılandırın.
İstemciler sanal ana bilgisayar adı üzerinden bağlanabilir. DNS sunucusu küme IP adresini çözer ve iç yük dengeleyicisi kümenin etkin düğümüne iletme bağlantı noktasını işler.

## <a name="sap-netweaver-high-availability-in-azure-infrastructure-as-a-service-iaas"></a><a name="2e3fec50-241e-441b-8708-0b1864f66dfa"></a>SAP NetWeaver, Hizmet Olarak Azure Altyapısında (IaaS) yüksek kullanılabilirlik
SAP uygulamasının yüksek kullanılabilirlik elde etmek için ( SAP yazılım bileşenleri gibi), aşağıdaki bileşenleri korumanız gerekir:

* SAP Application Server örneği
* SAP ASCS/SCS örneği
* DBMS sunucusu

Yüksek kullanılabilirlik senaryolarında SAP bileşenlerinin korunması hakkında daha fazla bilgi [için, SAP NetWeaver için Azure Sanal Makineler planlaması ve uygulamasına](planning-guide.md)bakın.

### <a name="high-availability-sap-application-server"></a><a name="93faa747-907e-440a-b00a-1ae0a89b1c0e"></a>Yüksek kullanılabilirlik SAP Uygulama Sunucusu
Genellikle SAP Application Server ve iletişim örnekleri için belirli bir yüksek kullanılabilirlik çözümüne ihtiyacınız yoktur. Fazlalıkla yüksek kullanılabilirlik elde emiş olursunuz ve Azure Sanal Makineleri'nin farklı örneklerinde birden çok iletişim örneğini yapılandırırsınız. Azure Sanal Makineleri'nin iki örneğinde en az iki SAP uygulama örneği yüklenmeliydi.

![Şekil 4: Yüksek kullanılabilirlik SAP Application Server][sap-ha-guide-figure-2000]

_**Şekil 4:** Yüksek kullanılabilirlik SAP Uygulama Sunucusu_

SAP Application Server örneklerini barındıran tüm sanal makineleri aynı Azure kullanılabilirlik kümesine yerleştirmeniz gerekir. Azure kullanılabilirlik kümesi şunları sağlar:

* Tüm sanal makineler aynı yükseltme alanının bir parçasıdır. Örneğin, yükseltme etki alanı, sanal makinelerin planlanan bakım kapalı kalma süresi sırasında aynı anda güncelleştirilememesini sağlar.
* Tüm sanal makineler aynı hata etki alanının bir parçasıdır. Örneğin, hata etki alanı, tek bir hata noktasının tüm sanal makinelerin kullanılabilirliğini etkilememesi için sanal makinelerin dağıtılmasını sağlar.

[Sanal makinelerin kullanılabilirliğini][virtual-machines-manage-availability]nasıl yönetebilirsiniz hakkında daha fazla bilgi edinin.

Azure depolama hesabı olası tek bir hata noktası olduğundan, en az iki sanal makinenin dağıtıldığı en az iki Azure depolama hesabına sahip olmak önemlidir. İdeal bir kurulumda, SAP iletişim örneği çalıştıran her sanal makinenin diskleri farklı bir depolama hesabında dağıtılır.

### <a name="high-availability-sap-ascsscs-instance"></a><a name="f559c285-ee68-4eec-add1-f60fe7b978db"></a>Yüksek kullanılabilirlik SAP ASCS/SCS örneği
Şekil 5, yüksek kullanılabilirlik sap ASCS/SCS örneğine bir örnektir.

![Şekil 5: Yüksek kullanılabilirlik SAP ASCS/SCS örneği][sap-ha-guide-figure-2001]

_**Şekil 5:** Yüksek kullanılabilirlik SAP ASCS/SCS örneği_

#### <a name="sap-ascsscs-instance-high-availability-with-windows-server-failover-clustering-in-azure"></a><a name="b5b1fd0b-1db4-4d49-9162-de07a0132a51"></a>SAP ASCS/SCS örneğinde Azure'da Windows Server Failover Kümeleme ile yüksek kullanılabilirlik
Azure Sanal Makineler, çıplak metal veya özel bulut dağıtımları ile karşılaştırıldığında, Windows Server Failover Kümeleme'yi yapılandırmak için ek adımlar gerektirir. Windows failover kümesi oluşturmak için paylaşılan bir küme diskine, birkaç IP adresine, birkaç sanal ana bilgisayar adına ve SAP ASCS/SCS örneğini kümelemek için bir Azure dahili yük dengeleyicisine ihtiyacınız vardır. Bunu daha sonra makalede daha ayrıntılı olarak ele alacağız.

![Şekil 6: SIOS DataKeeper kullanarak Azure'da BIR SAP ASCS/SCS yapılandırması için Windows Server Failover Kümeleme][sap-ha-guide-figure-1002]

_**Şekil 6:** SIOS DataKeeper ile Azure'da SAP ASCS/SCS yapılandırması için Windows Server Failover Kümeleme_

### <a name="high-availability-dbms-instance"></a><a name="ddd878a0-9c2f-4b8e-8968-26ce60be1027"></a>Yüksek kullanılabilirlik dbms örneği
DBMS aynı zamanda sap sisteminde tek bir temas noktasıdır. Yüksek kullanılabilirlik çözüm kullanarak onu korumak gerekir. Şekil 7, Windows Server Failover Clustering ve Azure dahili yük dengeleyicisi ile Azure'da her zaman yüksek kullanılabilirlikte bir SQL Server On çözümünü gösterir. SQL Server Always On, kendi DBMS çoğaltmasını kullanarak DBMS verilerini ve günlük dosyalarını çoğaltır. Bu durumda, tüm kurulumu basitleştiren küme paylaşılan disklere ihtiyacınız yoktur.

![Şekil 7: SQL Server Always A'da yüksek kullanılabilirlik sap DBMS örneği][sap-ha-guide-figure-2003]

_**Şekil 7:** SQL Server Always A'sı ile yüksek kullanılabilirliksap DBMS örneği_

Azure Kaynak Yöneticisi dağıtım modelini kullanarak Azure'da SQL Server kümeleme hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Kaynak Yöneticisi'ni kullanarak Azure Sanal Makinelerde Her Zaman Kullanılabilirlik Grubunda YapılandırMa][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]
* [Azure'daki Her Zaman Kullanılabilirlik grubu için Bir Azure dahili yük dengeleyicisi yapılandırma][virtual-machines-windows-portal-sql-alwayson-int-listener]

## <a name="end-to-end-high-availability-deployment-scenarios"></a><a name="045252ed-0277-4fc8-8f46-c5a29694a816"></a>Uçlardan uca yüksek kullanılabilirlik lisi dağıtım senaryoları

### <a name="deployment-scenario-using-architectural-template-1"></a>Mimari Şablon 1'i kullanarak dağıtım senaryosu

Şekil 8, **azure'da bir** SAP Sistemi için sap netweaver yüksek kullanılabilirlik mimarisinin bir örneğini gösterir. Bu senaryo aşağıdaki gibi ayarlanır:

- SAP ASCS/SCS örneği için özel bir küme kullanılır.
- DBMS örneği için özel bir küme kullanılır.
- SAP Application Server örnekleri kendi özel VM'lerinde dağıtılır.

![Şekil 8: ASCS/SCS ve DBMS için özel kümeye sahip SAP yüksek kullanılabilirlikli Mimari Şablon 1][sap-ha-guide-figure-2004]

_**Şekil 8:** SAP yüksek kullanılabilirlikli Mimari Şablon 1, ASCS/SCS ve DBMS için özel kümeler_

### <a name="deployment-scenario-using-architectural-template-2"></a>Mimari Şablon 2'yi kullanarak dağıtım senaryosu

Şekil 9, **azure'da bir** SAP Sistemi için sap netweaver yüksek kullanılabilirlik mimarisinin bir örneğini gösterir. Bu senaryo aşağıdaki gibi ayarlanır:

- Hem SAP ASCS/SCS örneği **hem de** DBMS için özel bir küme kullanılır.
- SAP Application Server örnekleri kendi özel VM'lerde dağıtılır.

![Şekil 9: SAP yüksek kullanılabilirlikLi Mimari Şablon 2, ASCS/SCS için özel bir küme ve DBMS için özel bir küme][sap-ha-guide-figure-2005]

_**Şekil 9:** ASCS/SCS için özel bir küme ve DBMS için özel bir küme ile SAP yüksek kullanılabilirlikli Mimari Şablon 2_

### <a name="deployment-scenario-using-architectural-template-3"></a>Mimari Şablon 3'u kullanarak dağıtım senaryosu

Şekil 10, SID1&gt; ve &lt;SID2 ile **iki** SAP sistemi için &lt;Azure'daki SAP NetWeaver yüksek kullanılabilirlik mimarisinin bir örneğini&gt;gösterir. Bu senaryo aşağıdaki gibi ayarlanır:

- Hem SAP ASCS/SCS SID1 örneği *hem* **de** SAP ASCS/SCS SID2 örneği (bir küme) için özel bir küme kullanılır.
- DBMS SID1 için bir özel küme, DBMS SID2 (iki küme) için başka bir adanmış küme kullanılır.
- SAP sistemi SID1 için SAP Application Server örnekleri kendi özel VMs var.
- SAP sistemi SID2 için SAP Application Server örnekleri kendi özel VMs var.

![Şekil 10: SAP yüksek kullanılabilirlikli Mimari Şablon 3, farklı ASCS/SCS örnekleri için özel bir küme][sap-ha-guide-figure-6003]

_**Şekil 10:** SAP yüksek kullanılabilirlikli Mimari Şablon 3, farklı ASCS/SCS örnekleri için özel bir küme_

## <a name="prepare-the-infrastructure"></a><a name="78092dbe-165b-454c-92f5-4972bdbef9bf"></a>Altyapıyı hazırlama

### <a name="prepare-the-infrastructure-for-architectural-template-1"></a>Mimari Şablon 1 için altyapıyı hazırlama
SAP için Azure Kaynak Yöneticisi şablonları, gerekli kaynakların dağıtımını basitleştirmeye yardımcı olur.

Azure Kaynak Yöneticisi'ndeki üç katmanlı şablonlar, iki kümeye sahip Mimari Şablon 1 gibi yüksek kullanılabilirlik senaryolarını da destekler. Her küme SAP ASCS/SCS ve DBMS için sap tek bir hata noktasıdır.

Bu makalede açıkladığımız örnek senaryo için Azure Kaynak Yöneticisi şablonlarını buradan alabilirsiniz:

* [Azure Marketi görüntüsü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image)  
* [Özel görüntü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image)

Mimari Şablon 1 için altyapı hazırlamak için:

- Azure portalında, **Parametreler** bıçağında, **SYSTEMAVAILABILITY** **kutusunda, HA'yı**seçin.

  ![Şekil 11: SAP yüksek kullanılabilirlikAzure Kaynak Yöneticisi parametrelerini ayarlama][sap-ha-guide-figure-3000]

_**Şekil 11:** SAP yüksek kullanılabilirlik Azure Kaynak Yöneticisi parametrelerini ayarlama_


  Şablonlar oluşturur:

  * **Sanal makineler:**
    * SAP Application Server sanal makineleri: <*SAPSystemSID*>-di-<*Numarası*>
    * ASCS/SCS küme sanal makineleri: <*SAPSystemSID*>-ascs-<*Numarası*>
    * DBMS kümesi: <*SAPSystemSID*>-db-<*Numarası*>

  * **İlişkili IP adreslerine sahip tüm sanal makineler için ağ kartları:**
    * <*SAPSystemSID*>-nic-di-<*Numarası*>
    * <*SAPSystemSID*>-nic-ascs-<*Numarası*>
    * <*SAPSystemSID*>-nic-db-<*Numarası*>

  * **Azure depolama hesapları**

  * **Kullanılabilirlik grupları:**
    * SAP Application Server sanal makineleri: <*SAPSystemSID*>-avset-di
    * SAP ASCS/SCS küme sanal makineleri: *<SAPSystemSID*>-avset-ascs
    * DBMS küme sanal makineleri: <*SAPSystemSID*>-avset-db

  * **Azure dahili yük dengeleyicisi**:
    * *SAPSystemSID*>-lb-ascs <ASCS/SCS örneği ve IP adresi için tüm bağlantı noktalarıyla
    * *SAPSystemSID* <SQL Server DBMS ve IP adresi için tüm bağlantı noktaları ile>-lb-db

  * **Ağ güvenlik grubu**: <*SAPSystemSID*>-nsg-ascs-0  
    * *SAPSystemSID*>-ascs-0 sanal makine <açık harici Uzak Masaüstü Protokolü (RDP) bağlantı noktası ile

> [!NOTE]
> Ağ kartlarının ve Azure dahili yük dengeleyicilerinin tüm IP adresleri varsayılan olarak **dinamiktir.** Bunları **statik** IP adresleriyle değiştirin. Bunu daha sonra makalede nasıl yapacağımızı açıklayacağız.
>
>

### <a name="deploy-virtual-machines-with-corporate-network-connectivity-cross-premises-to-use-in-production"></a><a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a>Üretimde kullanmak üzere kurumsal ağ bağlantısına (tesisler arası) sahip sanal makineleri dağıtma
Üretim SAP sistemleri için, Azure Siteden Siteye VPN veya Azure ExpressRoute'u kullanarak kurumsal ağ bağlantısına sahip Azure sanal makinelerini dağıtın.

> [!NOTE]
> Azure Sanal Ağ örneğini kullanabilirsiniz. Sanal ağ ve alt ağ zaten oluşturuldu ve hazırlandı.
>
>

1. Azure portalında, **Parametreler** bıçağında, **NEWOREXISTINGSUBNET** kutusunda **varolan'ı**seçin.
2. **SUBNETID** kutusuna, Azure sanal makinelerinizi dağıtmayı planladığınız hazırlanmış Azure ağı Nız SubnetID'in tam dizesini ekleyin.
3. Tüm Azure ağ alt ağlarının listesini almak için bu PowerShell komutunu çalıştırın:

   ```PowerShell
   (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets
   ```

   **Kimlik** alanı **SUBNETID'yi**gösterir.
4. Tüm **SUBNETID** değerlerinin listesini almak için bu PowerShell komutunu çalıştırın:

   ```PowerShell
   (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets.Id
   ```

   **SUBNETID** şuna benzer:

   ```
   /subscriptions/<SubscriptionId>/resourceGroups/<VPNName>/providers/Microsoft.Network/virtualNetworks/azureVnet/subnets/<SubnetName>
   ```

### <a name="deploy-cloud-only-sap-instances-for-test-and-demo"></a><a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a>Test ve demo için yalnızca buluta özel SAP örneklerini dağıtma
Yüksek kullanılabilirlik sap sisteminizi yalnızca buluta özel dağıtım modelinde dağıtabilirsiniz. Bu tür bir dağıtım öncelikle demo ve test kullanım örnekleri için yararlıdır. Üretim kullanım kılıfları için uygun değil.

- Azure portalında, **Parametreler** bıçağında, **NEWOREXISTINGSUBNET** kutusunda yeni **'** yi seçin. **SUBNETID** alanını boş bırakın.

  SAP Azure Kaynak Yöneticisi şablonu otomatik olarak Azure sanal ağı ve alt ağı oluşturur.

> [!NOTE]
> Ayrıca, aynı Azure Sanal Ağ örneğinde Active Directory ve DNS için en az bir sanal makine dağıtmanız gerekir. Şablon bu sanal makineleri oluşturmaz.
>
>


### <a name="prepare-the-infrastructure-for-architectural-template-2"></a>Mimari Şablon 2 için altyapıyı hazırlama

SAP Mimari Şablon 2 için gerekli altyapı kaynaklarının dağıtımını basitleştirmeye yardımcı olmak için SAP için bu Azure Kaynak Yöneticisi şablonu kullanabilirsiniz.

Bu dağıtım senaryosu için Azure Kaynak Yöneticisi şablonlarını buradan alabilirsiniz:

* [Azure Marketi görüntüsü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged)  
* [Özel görüntü](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged)


### <a name="prepare-the-infrastructure-for-architectural-template-3"></a>Mimari Şablon 3 için altyapıyı hazırlama

Altyapıyı hazırlayabilir ve SAP'yi **multi-SID**için yapılandırabilirsiniz. Örneğin, *varolan* bir küme yapılandırmasına ek bir SAP ASCS/SCS örneği ekleyebilirsiniz. Daha fazla bilgi için bkz. Azure [Kaynak Yöneticisi'nde BIR SAP multi-SID yapılandırması oluşturmak için ek bir SAP ASCS/SCS örneğini varolan bir küme yapılandırmasında yapılandırın.][sap-ha-multi-sid-guide]

Yeni bir multi-SID kümesi oluşturmak istiyorsanız, [GitHub'da](https://github.com/Azure/azure-quickstart-templates)çoklu SID hızlı başlatma şablonlarını kullanabilirsiniz.
Yeni bir multi-SID kümesi oluşturmak için aşağıdaki üç şablonu dağıtmanız gerekir:

* [ASCS/SCS şablonu](#ASCS-SCS-template)
* [Veritabanı şablonu](#database-template)
* [Uygulama sunucuları şablonu](#application-servers-template)

Aşağıdaki bölümlerde şablonlar ve şablonlarda sağlamanız gereken parametreler hakkında daha fazla ayrıntı vardır.

#### <a name="ascsscs-template"></a><a name="ASCS-SCS-template"></a>ASCS/SCS şablonu

ASCS/SCS şablonu, birden çok ASCS/SCS örneğini barındıran bir Windows Server başarısız kümesi oluşturmak için kullanabileceğiniz iki sanal makine dağıtır.

[ASCS/SCS multi-SID şablonuna ascs/SCS multi-SID şablonu][sap-templates-3-tier-multisid-xscs-marketplace-image]ayarlamak için aşağıdaki parametreler için değerleri girin:

  - **Kaynak Öneki**.  Dağıtım sırasında oluşturulan tüm kaynakları önek için kullanılan kaynak önekini ayarlayın. Kaynaklar yalnızca bir SAP sistemine ait olmadığından, kaynağın öneki bir SAP sisteminin SID'si değildir.  Önek **üç ve altı karakter**arasında olmalıdır.
  - **Yığın Türü**. SAP sisteminin yığın türünü seçin. Yığın türüne bağlı olarak, Azure Yük Dengeleyici'nin SAP sistemi başına bir (yalnızca ABAP veya Java) veya iki (ABAP+Java) özel IP adresi vardır.
  -  **İşletim Sistemi Türü**. Sanal makinelerin işletim sistemini seçin.
  -  **SAP Sistem Sayısı**. Bu kümeye yüklemek istediğiniz SAP sistemlerinin sayısını seçin.
  -  **Sistem Kullanılabilirliği**. **HA'yı**seçin.
  -  **Admin Kullanıcı Adı ve Yönetici Şifresi**. Makinede oturum açmada kullanılabilecek yeni bir kullanıcı oluşturun.
  -  **Yeni veya Varolan Subnet**. Yeni bir sanal ağ ve alt ağ oluşturulup oluşturulmaması veya varolan bir alt ağ kullanılıp kullanılmayacağını ayarlayın. Şirket içi ağınıza bağlı bir sanal ağınız varsa, **varolan**ağınızı seçin.
  -  **Subnet Kimliği**. VM'yi, VM'nin atanması gereken tanımlanmış bir alt ağınız olduğu varolan bir VNet'e dağıtmak istiyorsanız, bu alt ağın kimliğini adlandırın. Kimlik genellikle şu na benzer: */subscriptions/<subscription id*>/resourceGroups/<kaynak grubu *adı*>/sağlayıcılar/Microsoft.Network/virtualNetworks/<sanal *ağ adı*>/subnets/<alt *ağ adı*>

Şablon, birden çok SAP sistemlerini destekleyen bir Azure Yük Dengeleyici örneğini dağıtır.

- ASCS örnekleri 00, 10, 20...
- SCS örnekleri örneğinde 01, 11, 21...
- ASCS Enqueue Çoğaltma Sunucusu (ERS) (yalnızca Linux) örnekleri 02, 12, 22...
- SCS ERS (yalnızca Linux) örnekleri 03, 13, 23...

Yük dengeleyicisi 1 (2 Linux için) VIP(ler), ASCS/SCS için 1x VIP ve ERS için 1x VIP (yalnızca Linux) içerir.

Aşağıdaki liste tüm yük dengeleme kurallarını içerir (x SAP sisteminin sayısıdır, örneğin, 1, 2, 3...):
- Her SAP sistemi için Windows'a özel bağlantı noktaları: 445, 5985
- ASCS bağlantı noktaları (örnek numarası x0): 32x0, 36x0, 39x0, 81x0, 5x013, 5x014, 5x016
- SCS bağlantı noktaları (örnek numarası x1): 32x1, 33x1, 39x1, 81x1, 5x113, 5x114, 5x116
- Linux üzerinde ASCS ERS bağlantı noktaları (örnek numarası x2): 33x2, 5x213, 5x214, 5x216
- Linux'ta SCS ERS bağlantı noktaları (örnek numarası x3): 33x3, 5x313, 5x314, 5x316

Yük dengeleyicisi aşağıdaki sonda bağlantı noktalarını kullanacak şekilde yapılandırılır (x SAP sisteminin sayısıdır, örneğin, 1, 2, 3...):
- ASCS/SCS dahili yük dengeleyici prob bağlantı noktası: 620x0
- ERS dahili yük dengeleyici prob bağlantı noktası (sadece Linux): 621x2

#### <a name="database-template"></a><a name="database-template"></a>Veritabanı şablonu

Veritabanı şablonu, bir SAP sistemi için ilişkisel veritabanı yönetim sistemini (RDBMS) yüklemek için kullanabileceğiniz bir veya iki sanal makinedağıdağır. Örneğin, beş SAP sistemi için bir ASCS/SCS şablonu dağıtırsanız, bu şablonu beş kez dağıtmanız gerekir.

Veritabanı multi-SID şablonu kurmak için, [veritabanı multi-SID şablon,][sap-templates-3-tier-multisid-db-marketplace-image]aşağıdaki parametreler için değerleri girin:

- **Sap Sistem Kimliği**. Yüklemek istediğiniz SAP sisteminin SAP sistem kimliğini girin. Kimlik, dağıtılan kaynaklar için önek olarak kullanılır.
- **Os Tipi**. Sanal makinelerin işletim sistemini seçin.
- **Dbtype**. Kümeye yüklemek istediğiniz veritabanıtürünü seçin. Microsoft SQL Server'ı yüklemek istiyorsanız **SQL'i** seçin. SAP HANA'yı sanal makinelere yüklemeyi planlıyorsanız **HANA'yı** seçin. Doğru işletim sistemi türünü seçtiğinizden emin olun: SQL için **Windows'u** seçin ve HANA için bir Linux dağıtımı seçin. Sanal makinelere bağlı Azure Yük Dengeleyicisi, seçili veritabanı türünü destekleyecek şekilde yapılandırılır:
  * **SQL**. Yük dengeleyici silik 1433 yük dengesi olacaktır. SQL Server Always On kurulumunuz için bu bağlantı noktasını kullandığınızdan emin olun.
  * **HANA**. Yük dengeleyici35015 ve 35017 bağlantı noktalarını yükler. Sap HANA'yı örnek numarası **50**ile yüklediğinizden emin olun.
  Yük dengeleyicisi prob portu 62550 kullanır.
- **Sap Sistem Boyutu**. Yeni sistemin sağlayacağı SAPS sayısını ayarlayın. Sistemin kaç SAPS gerektirdiğinden emin değilseniz, SAP Teknoloji Ortağınıza veya Sistem Entegratörünüze danışın.
- **Sistem Kullanılabilirliği**. **HA'yı**seçin.
- **Admin Kullanıcı Adı ve Yönetici Şifresi**. Makinede oturum açmada kullanılabilecek yeni bir kullanıcı oluşturun.
- **Subnet Kimliği**. ASCS/SCS şablonunun dağıtımı sırasında kullandığınız alt netin kimliğini veya ASCS/SCS şablon dağıtımının bir parçası olarak oluşturulan alt ağın kimliğini girin.

#### <a name="application-servers-template"></a><a name="application-servers-template"></a>Uygulama sunucuları şablonu

Uygulama sunucuları şablonu, bir SAP sistemi için SAP Application Server örnekleri olarak kullanılabilecek iki veya daha fazla sanal makine dağıtmaktadır. Örneğin, beş SAP sistemi için bir ASCS/SCS şablonu dağıtırsanız, bu şablonu beş kez dağıtmanız gerekir.

Uygulama sunucuları multi-SID şablonu kurmak için, [uygulama sunucularında multi-SID şablon,][sap-templates-3-tier-multisid-apps-marketplace-image]aşağıdaki parametreler için değerleri girin:

  -  **Sap Sistem Kimliği**. Yüklemek istediğiniz SAP sisteminin SAP sistem kimliğini girin. Kimlik, dağıtılan kaynaklar için önek olarak kullanılır.
  -  **Os Tipi**. Sanal makinelerin işletim sistemini seçin.
  -  **Sap Sistem Boyutu**. Yeni sistemin sağlayacağı SAPS sayısı. Sistemin kaç SAPS gerektirdiğinden emin değilseniz, SAP Teknoloji Ortağınıza veya Sistem Entegratörünüze danışın.
  -  **Sistem Kullanılabilirliği**. **HA'yı**seçin.
  -  **Admin Kullanıcı Adı ve Yönetici Şifresi**. Makinede oturum açmada kullanılabilecek yeni bir kullanıcı oluşturun.
  -  **Subnet Kimliği**. ASCS/SCS şablonunun dağıtımı sırasında kullandığınız alt netin kimliğini veya ASCS/SCS şablon dağıtımının bir parçası olarak oluşturulan alt ağın kimliğini girin.


### <a name="azure-virtual-network"></a><a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a>Azure sanal ağ
Örneğimizde, Azure sanal ağının adres alanı 10.0.0.0/16'dır. 10.0.0.0/24 adres aralığına sahip **Subnet**adında bir alt ağ vardır. Tüm sanal makineler ve dahili yük dengeleyicileri bu sanal ağda dağıtılır.

> [!IMPORTANT]
> Konuk işletim sistemi içindeki ağ ayarlarında herhangi bir değişiklik yapmayın. Buna IP adresleri, DNS sunucuları ve alt ağ dahildir. Azure'da tüm ağ ayarlarınızı yapılandırın. Dinamik Ana Bilgisayar Yapılandırma Protokolü (DHCP) hizmeti ayarlarınızı yayır.
>
>

### <a name="dns-ip-addresses"></a><a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a>DNS IP adresleri

Gerekli DNS IP adreslerini ayarlamak için aşağıdaki adımları yapın.

1. Azure portalında, **DNS sunucuları** bıçak, sanal ağ **DNS sunucuları** seçeneği **Özel DNS**olarak ayarlandığından emin olun.
2. Ayarlarınızı sahip olduğunuz ağ türüne göre seçin. Daha fazla bilgi için aşağıdaki kaynaklara bakın:
   * Şirket içi DNS sunucularının IP adreslerini ekleyin.  
   Şirket içi DNS sunucularını Azure'da çalışan sanal makinelere genişletebilirsiniz. Bu senaryoda, DNS hizmetini çalıştırdığınız Azure sanal makinelerinin IP adreslerini ekleyebilirsiniz.
   * Azure'da yalıtılmış dağıtımlar için: DNS sunucusu olarak hizmet veren aynı Sanal Ağ örneğinde ek bir sanal makine dağıtın. DNS hizmetini çalıştırmak için ayarladığınız Azure sanal makinelerinIN IP adreslerini ekleyin.

   ![Şekil 12: Azure Sanal Ağı için DNS sunucularını yapılandırma][sap-ha-guide-figure-3001]

   _**Şekil 12:** Azure Sanal Ağı için DNS sunucularını yapılandırma_

   > [!NOTE]
   > DNS sunucularının IP adreslerini değiştirirseniz, değişikliği uygulamak ve yeni DNS sunucularını yaymak için Azure sanal makinelerini yeniden başlatmanız gerekir.
   >
   >

Örneğimizde, DNS hizmeti bu Windows sanal makinelerde yüklenir ve yapılandırılır:

| Sanal makine rolü | Sanal makine ana bilgisayar adı | Ağ kartı adı | Statik IP adresi |
| --- | --- | --- | --- |
| İlk DNS sunucusu |domcontr-0 |pr1-nic-domcontr-0 |10.0.0.10 |
| İkinci DNS sunucusu |domcontr-1 |pr1-nic-domcontr-1 |10.0.0.11 |

### <a name="host-names-and-static-ip-addresses-for-the-sap-ascsscs-clustered-instance-and-dbms-clustered-instance"></a><a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a>SAP ASCS/SCS kümelenmiş örnek ve DBMS kümelenmiş örnek için ana bilgisayar adları ve statik IP adresleri

Şirket içi dağıtım için bu ayrılmış ana bilgisayar adları ve IP adreslerine ihtiyacınız vardır:

| Sanal ana bilgisayar adı rolü | Sanal ana bilgisayar adı | Sanal statik IP adresi |
| --- | --- | --- |
| SAP ASCS/SCS ilk küme sanal ana bilgisayar adı (küme yönetimi için) |pr1-ascs-vir |10.0.0.42 |
| SAP ASCS/SCS örnek sanal ana bilgisayar adı |pr1-ascs-sap |10.0.0.43 |
| SAP DBMS ikinci küme sanal ana bilgisayar adı (küme yönetimi) |pr1-dbms-vir |10.0.0.32 |

Kümeyi oluşturduğunuzda, sanal ana bilgisayar adlarını **pr1-ascs-vir** ve **pr1-dbms-vir** ve kümenin kendisini yöneten ilişkili IP adreslerini oluşturun. Bunun nasıl yapılacağını öğrenmek için bkz. küme [yapılandırmasında küme düğümlerini topla.][sap-ha-guide-8.12.1]

DNS sunucusunda diğer iki sanal ana bilgisayar adı, **pr1-ascs-sap** ve **pr1-dbms-sap**ve ilişkili IP adreslerini el ile oluşturabilirsiniz. Kümelenmiş SAP ASCS/SCS örneği ve kümelenmiş DBMS örneği bu kaynakları kullanır. Bunun nasıl yapılacağını öğrenmek için [bkz.][sap-ha-guide-9.1.1]

### <a name="set-static-ip-addresses-for-the-sap-virtual-machines"></a><a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a>SAP sanal makineleri için statik IP adreslerini ayarlama
Kümenizde kullanmak üzere sanal makineleri dağıttıktan sonra, tüm sanal makineler için statik IP adresleri ayarlamanız gerekir. Bunu konuk işletim sisteminde değil, Azure Sanal Ağ yapılandırmasında yapın.

1. Azure portalında **Kaynak Grubu** > **Ağ Kartı** > **Ayarları** > **IP Adresini**seçin.
2. IP **adresleri** nde, **Atama**altında **Statik'i**seçin. IP **adres** kutusuna, kullanmak istediğiniz IP adresini girin.

   > [!NOTE]
   > Ağ kartının IP adresini değiştirirseniz, değişikliği uygulamak için Azure sanal makinelerini yeniden başlatmanız gerekir.  
   >
   >

   ![Şekil 13: Her sanal makinenin ağ kartı için statik IP adreslerini ayarlama][sap-ha-guide-figure-3002]

   _**Şekil 13:** Her sanal makinenin ağ kartı için statik IP adreslerini ayarlama_

   Active Directory/DNS hizmetiniz için kullanmak istediğiniz sanal makineler de dahil olmak üzere tüm sanal makineler için tüm ağ arabirimleri için bu adımı yineleyin.

Örneğimizde, bu sanal makineler ve statik IP adresleri vardır:

| Sanal makine rolü | Sanal makine ana bilgisayar adı | Ağ kartı adı | Statik IP adresi |
| --- | --- | --- | --- |
| İlk SAP Application Server örneği |pr1-di-0 |pr1-nic-di-0 |10.0.0.50 |
| İkinci SAP Application Server örneği |pr1-di-1 |pr1-nic-di-1 |10.0.0.51 |
| ... |... |... |... |
| Son SAP Application Server örneği |pr1-di-5 |pr1-nic-di-5 |10.0.0.55 |
| ASCS/SCS örneği için ilk küme düğümü |pr1-ascs-0 |pr1-nic-ascs-0 |10.0.0.40 |
| ASCS/SCS örneği için ikinci küme düğümü |pr1-ascs-1 |pr1-nic-ascs-1 |10.0.0.41 |
| DBMS örneği için ilk küme düğümü |pr1-db-0 |pr1-nic-db-0 |10.0.0.30 |
| DBMS örneği için ikinci küme düğümü |pr1-db-1 |pr1-nic-db-1 |10.0.0.31 |

### <a name="set-a-static-ip-address-for-the-azure-internal-load-balancer"></a><a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a>Azure dahili yük dengeleyicisi için statik bir IP adresi ayarlama

SAP Azure Kaynak Yöneticisi şablonu, SAP ASCS/SCS örnek kümesi ve DBMS kümesi için kullanılan bir Azure dahili yük dengeleyicisi oluşturur.

> [!IMPORTANT]
> SAP ASCS/SCS'nin sanal ana bilgisayar adının IP adresi, SAP ASCS/SCS dahili yük dengeleyicisinin IP adresiyle aynıdır: **pr1-lb-ascs.**
> DBMS sanal adının IP adresi DBMS iç yük dengeleyiciip adresi aynıdır: **pr1-lb-dbms**.
>
>

Azure dahili yük dengeleyicisi için statik bir IP adresi ayarlamak için:

1. İlk dağıtım, dahili yük dengeleyiciip IP adresini **Dinamik**olarak ayarlar. Azure portalında, **IP adresleri** nde, **Atama**altında **Statik'i**seçin.
2. Dahili yük **dengeleyicipr1-lb-ascs** IP adresini SAP ASCS/SCS örneğinin sanal ana bilgisayar adının IP adresine ayarlayın.
3. Dahili yük **dengeleyicipr1-lb-dbms** IP adresini DBMS örneğinin sanal ana bilgisayar adının IP adresine ayarlayın.

   ![Şekil 14: SAP ASCS/SCS örneği için dahili yük dengeleyicisi için statik IP adreslerini ayarlama][sap-ha-guide-figure-3003]

   _**Şekil 14:** SAP ASCS/SCS örneği için dahili yük dengeleyicisinin statik IP adreslerini ayarlama_

Örneğimizde, şu statik IP adreslerine sahip iki Azure dahili yük dengeleyicimiz vardır:

| Azure dahili yük dengeleyicisi rolü | Azure dahili yük dengeleyici adı | Statik IP adresi |
| --- | --- | --- |
| SAP ASCS/SCS örnek dahili yük dengeleyicisi |pr1-lb-ascs |10.0.0.43 |
| SAP DBMS dahili yük dengeleyicisi |pr1-lb-dbms |10.0.0.33 |


### <a name="default-ascsscs-load-balancing-rules-for-the-azure-internal-load-balancer"></a><a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a>Azure dahili yük dengeleyicisi için varsayılan ASCS/SCS yük dengeleme kuralları

SAP Azure Kaynak Yöneticisi şablonu, gereksinim duyduğunuz bağlantı noktalarını oluşturur:
* Varsayılan örnek numarası **00** olan bir ABAP ASCS örneği
* Varsayılan örnek numarası **01** olan bir Java SCS örneği

SAP ASCS/SCS örneğini yüklediğinizde, ABAP ASCS örneğiniz için varsayılan örnek **00** sayısını ve Java SCS örneğiniz için varsayılan örnek **01'i** kullanmanız gerekir.

Ardından, SAP NetWeaver bağlantı noktaları için gerekli iç yük dengeleme uç noktalarını oluşturun.

Gerekli iç yük dengeleme uç noktalarını oluşturmak için, ilk olarak, SAP NetWeaver ABAP ASCS bağlantı noktaları için bu yük dengeleme uç noktalarını oluşturun:

| Hizmet/yük dengeleme kural adı | Varsayılan bağlantı noktası numaraları | Beton bağlantı noktaları (00 numaralı ASCS örneğinde) (10 ile ERS) |
| --- | --- | --- |
| Enqueue Sunucu / *lbrule3200* |32<*InstanceNumber*> |3200 |
| ABAP Mesaj Sunucusu / *lbrule3600* |36<*Örnek Numarası*> |3600 |
| Dahili ABAP Mesajı / *lbrule3900* |39<*Örnek Numarası*> |3900 |
| Mesaj Sunucusu HTTP / *Lbrule8100* |81<*InstanceNumber*> |8100 |
| SAP Başlangıç Hizmeti ASCS HTTP / *Lbrule50013* |5<*Örnek Sayısı*>13 |50013 |
| SAP Başlangıç Hizmeti ASCS HTTPS / *Lbrule50014* |5<*Örneği>14* |50014 |
| Enqueue Çoğaltma / *Lbrule50016* |5<*Örnek Sayısı*>16 |50016 |
| SAP Başlangıç Servis ERS HTTP *Lbrule51013* |5<*Örnek Sayısı*>13 |51013 |
| SAP Başlangıç Servis ERS HTTP *Lbrule51014* |5<*Örneği>14* |51014 |
| RM *Lbrule5985* kazanın | |5985 |
| Dosya Paylaşımı *Lbrule445* | |445 |

_**Tablo 1:** SAP NetWeaver ABAP ASCS örneklerinin bağlantı noktası numaraları_

Ardından, SAP NetWeaver Java SCS bağlantı noktaları için bu yük dengeleme uç noktalarını oluşturun:

| Hizmet/yük dengeleme kural adı | Varsayılan bağlantı noktası numaraları | Beton bağlantı noktaları (01 numaralı SCS örneği) (11 ile ERS) |
| --- | --- | --- |
| Enqueue Sunucu / *lbrule3201* |32<*InstanceNumber*> |3201 |
| Ağ Geçidi Sunucusu / *lbrule3301* |33<*InstanceNumber*> |3301 |
| Java Mesaj Sunucusu / *lbrule3900* |39<*Örnek Numarası*> |3901 |
| Mesaj Sunucusu HTTP / *Lbrule8101* |81<*InstanceNumber*> |8101 |
| SAP Başlangıç Hizmeti SCS HTTP / *Lbrule50113* |5<*Örnek Sayısı*>13 |50113 |
| SAP Başlangıç Hizmeti SCS HTTPS / *Lbrule50114* |5<*Örneği>14* |50114 |
| Enqueue Çoğaltma / *Lbrule50116* |5<*Örnek Sayısı*>16 |50116 |
| SAP Başlangıç Servis ERS HTTP *Lbrule51113* |5<*Örnek Sayısı*>13 |51113 |
| SAP Başlangıç Servis ERS HTTP *Lbrule51114* |5<*Örneği>14* |51114 |
| RM *Lbrule5985* kazanın | |5985 |
| Dosya Paylaşımı *Lbrule445* | |445 |

_**Tablo 2:** SAP NetWeaver Java SCS örneklerinin bağlantı noktası numaraları_

![Şekil 15: Azure dahili yük dengeleyicisi için Varsayılan ASCS/SCS yük dengeleme kuralları][sap-ha-guide-figure-3004]

_**Şekil 15:** Azure dahili yük dengeleyicisi için varsayılan ASCS/SCS yük dengeleme kuralları_

Yük dengeleyici **pr1-lb-dbms** IP adresini DBMS örneğinin sanal ana bilgisayar adının IP adresine ayarlayın.

### <a name="change-the-ascsscs-default-load-balancing-rules-for-the-azure-internal-load-balancer"></a><a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a>Azure dahili yük dengeleyicisi için ASCS/SCS varsayılan yük dengeleme kurallarını değiştirme

SAP ASCS veya SCS örnekleri için farklı sayılar kullanmak istiyorsanız, bağlantı noktalarının adlarını ve değerlerini varsayılan değerlerden değiştirmeniz gerekir.

1. Azure portalında ** < *SID*>-lb-ascs yük dengeleyici** > **Yük Dengeleme Kuralları'nı**seçin.
2. SAP ASCS veya SCS örneğine ait tüm yük dengeleme kuralları için aşağıdaki değerleri değiştirin:

   * Adı
   * Bağlantı noktası
   * Arka uç bağlantı noktası

   Örneğin, varsayılan ASCS örnek numarasını 00'dan 31'e değiştirmek istiyorsanız, Tablo 1'de listelenen tüm bağlantı noktaları için değişiklikler yapmanız gerekir.

   Burada bağlantı noktası *lbrule3200*için bir güncelleştirme örneği.

   ![Şekil 16: Azure dahili yük dengeleyicisi için ASCS/SCS varsayılan yük dengeleme kurallarını değiştirme][sap-ha-guide-figure-3005]

   _**Şekil 16:** Azure dahili yük dengeleyicisi için ASCS/SCS varsayılan yük dengeleme kurallarını değiştirme_

### <a name="add-windows-virtual-machines-to-the-domain"></a><a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a>Etki alanına Windows sanal makineleri ekleme

Sanal makinelere statik bir IP adresi atadıktan sonra, sanal makineleri etki alanına ekleyin.

![Şekil 17: Etki alanına sanal makine ekleme][sap-ha-guide-figure-3006]

_**Şekil 17:** Etki alanına sanal makine ekleme_

### <a name="add-registry-entries-on-both-cluster-nodes-of-the-sap-ascsscs-instance"></a><a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a>SAP ASCS/SCS örneğinin her iki küme düğümüne de kayıt defteri girişleri ekleme

Azure Yük Dengeleyicisi, bağlantılar belirli bir süre (boşta zaman acısı) boşta yken bağlantıları kapatan bir dahili yük dengeleyiciye sahiptir. Sap iş süreçlerini iletişim örneklerinde ilk sıra/sıra silme isteğinin gönderilmesi gerekir başlamaz SAP enqueue işlemine açık bağlantılar açar. Bu bağlantılar genellikle çalışma süreci veya enqueue işlemi yeniden başlatılına kadar kurulu kalır. Ancak, bağlantı belirli bir süre için boştaysa, Azure iç yük dengeleyicisi bağlantıları kapatır. SAP iş süreci artık yoksa enqueue işlemine bağlantıyı yeniden oluşturduğundan, bu bir sorun değildir. Bu etkinlikler SAP işlemlerinin geliştirici izlerinde belgelenmiştir, ancak bu izlemelerde büyük miktarda ek içerik oluşturur. TCP/IP'yi `KeepAliveTime` ve `KeepAliveInterval` her iki küme düğümünü değiştirmek iyi bir fikirdir. TCP/IP parametrelerindeki bu değişiklikleri, makalenin ilerleyen saatlerinde açıklanan SAP profil parametreleriyle birleştirin.

SAP ASCS/SCS örneğinin her iki küme düğümüne de kayıt defteri girişleri eklemek için, öncelikle SAP ASCS/SCS için her iki Windows küme düğümüne de bu Windows kayıt defteri girişlerini ekleyin:

| Yol | HKLM\System\CurrentControlSet\Services\Tcpip\Parametreler |
| --- | --- |
| Değişken adı |`KeepAliveTime` |
| Değişken türü |REG_DWORD (Ondalık) |
| Değer |120000 |
| Belgelere bağlantı |[https://technet.microsoft.com/library/cc957549.aspx](https://technet.microsoft.com/library/cc957549.aspx) |

_**Tablo 3:** İlk TCP/IP parametresini değiştirme_

Ardından, SAP ASCS/SCS için her iki Windows küme düğümüne de bu Windows kayıt defteri girişlerini ekleyin:

| Yol | HKLM\System\CurrentControlSet\Services\Tcpip\Parametreler |
| --- | --- |
| Değişken adı |`KeepAliveInterval` |
| Değişken türü |REG_DWORD (Ondalık) |
| Değer |120000 |
| Belgelere bağlantı |[https://technet.microsoft.com/library/cc957548.aspx](https://technet.microsoft.com/library/cc957548.aspx) |

_**Tablo 4:** İkinci TCP/IP parametresini değiştirme_

**Değişiklikleri uygulamak için her iki küme düğümlerini yeniden başlatın.**

### <a name="set-up-a-windows-server-failover-clustering-cluster-for-an-sap-ascsscs-instance"></a><a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a>SAP ASCS/SCS örneği için Windows Server Failover Kümeleme kümesi ayarlama

BIR SAP ASCS/SCS örneği için bir Windows Server Failover Kümeleme kümeleme kurulumu şu görevleri içerir:

- Küme konfigürasyonunda küme düğümlerini toplama
- Küme dosyası paylaşım tanığını yapılandırma

#### <a name="collect-the-cluster-nodes-in-a-cluster-configuration"></a><a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a>Küme konfigürasyonunda küme düğümlerini toplama

1. Rol ve Özellikler Ekle Sihirbazı'nda, her iki küme düğümüne de başarısız kümeleme ekleyin.
2. Failover Cluster Manager'ı kullanarak failover kümesini ayarlayın. Failover Cluster Manager'da **Cluster Oluştur'u**seçin ve ardından yalnızca ilk kümenin adını, a düğümüekleyin. İkinci düğümü henüz eklemeyin; daha sonraki bir adımda ikinci düğümü eklersiniz.

   ![Şekil 18: İlk küme düğümünün sunucu veya sanal makine adını ekleme][sap-ha-guide-figure-3007]

   _**Şekil 18:** İlk küme düğümünün sunucu veya sanal makine adını ekleme_

3. Kümenin ağ adını (sanal ana bilgisayar adı) girin.

   ![Şekil 19: Küme adını girin][sap-ha-guide-figure-3008]

   _**Şekil 19:** Küme adını girin_

4. Kümeyi oluşturduktan sonra bir küme doğrulama testi çalıştırın.

   ![Şekil 20: Küme doğrulama denetimini çalıştırın][sap-ha-guide-figure-3009]

   _**Şekil 20:** Küme doğrulama denetimini çalıştırma_

   İşlemin bu noktasında disklerle ilgili uyarıları yok sayabilirsiniz. Daha sonra bir dosya paylaşım tanığı ve SIOS paylaşılan diskler eklersiniz. Bu aşamada, bir çoğunluk olması konusunda endişelenmenize gerek yok.

   ![Şekil 21: Çoğunluk diski bulunamadı][sap-ha-guide-figure-3010]

   _**Şekil 21:** Çoğunluk diski bulunamadı_

   ![Şekil 22: Çekirdek küme kaynağının yeni bir IP adresine ihtiyacı vardır][sap-ha-guide-figure-3011]

   _**Şekil 22:** Çekirdek küme kaynağının yeni bir IP adresine ihtiyacı var_

5. Çekirdek küme hizmetinin IP adresini değiştirin. Sunucunun IP adresi sanal makine düğümlerinden birine işaret ettiği için küme, çekirdek küme hizmetinin IP adresini değiştirene kadar başedemez. Bunu çekirdek küme hizmetinin IP kaynağının **Özellikler** sayfasında yapın.

   Örneğin, küme sanal ana bilgisayar adı **pr1-ascs-vir**için bir IP adresi (örneğimizde, **10.0.0.42)** atamamız gerekir.

   ![Şekil 23: Özellikler iletişim kutusunda IP adresini değiştirin][sap-ha-guide-figure-3012]

   _**Şekil 23:** **Özellikler** iletişim kutusunda IP adresini değiştirin_

   ![Şekil 24: Küme için ayrılmış IP adresini atama][sap-ha-guide-figure-3013]

   _**Şekil 24:** Küme için ayrılmış IP adresini atama_

6. Küme sanal ana bilgisayar adını çevrimiçi duruma getirin.

   ![Şekil 25: Küme çekirdek hizmeti çalışır durumda ve doğru IP adresiyle][sap-ha-guide-figure-3014]

   _**Şekil 25:** Küme çekirdeği hizmeti çalışır durumda ve doğru IP adresiyle_

7. İkinci küme düğümekleyin.

   Çekirdek küme hizmeti çalışmaya ve çalışmaya devam ettiğine göre, ikinci küme düğümekleyebilirsiniz.

   ![Şekil 26: İkinci küme düğümünü ekleyin][sap-ha-guide-figure-3015]

   _**Şekil 26:** İkinci küme düğümü ekleme_

8. İkinci küme düğümü ana bilgisayarı için bir ad girin.

   ![Şekil 27: İkinci küme düğümü ana bilgisayar adını girin][sap-ha-guide-figure-3016]

   _**Şekil 27:** İkinci küme düğümü ana bilgisayar adını girin_

   > [!IMPORTANT]
   > Küme onay **kutusuna tüm uygun depolama ekle'nin** **seçilmediğinden** emin olun.  
   >
   >

   ![Şekil 28: Onay kutusunu seçmeyin][sap-ha-guide-figure-3017]

   _**Şekil 28:** Onay kutusunu **seçmeyin**_

   Çoğunluk ve disklerle ilgili uyarıları yoksayabilirsiniz. [SAP ASCS/SCS küme paylaşım diski için SIOS DataKeeper Cluster Edition'da][sap-ha-guide-8.12.3]açıklandığı gibi, yeterliliği ayarlayacak ve diski daha sonra paylaşacaksınız.

   ![Şekil 29: Disk çoğunluğu yla ilgili uyarıları yoksay][sap-ha-guide-figure-3018]

   _**Şekil 29:** Disk çoğunluğu yla ilgili uyarıları yoksay_


#### <a name="configure-a-cluster-file-share-witness"></a><a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a>Küme dosyası paylaşım tanığını yapılandırma

Küme dosyası paylaşımı tanığını yapılandırmak şu görevleri içerir:

- Dosya paylaşımı oluşturma
- Failover Cluster Manager'da dosya paylaşımı tanık çoğunluklarını ayarlama

##### <a name="create-a-file-share"></a><a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a>Dosya paylaşımı oluşturma

1. Çoğunluk diski yerine bir dosya paylaşım tanığı seçin. SIOS DataKeeper bu seçeneği destekler.

   Bu makaledeki örneklerde, dosya paylaşımı tanığı Azure'da çalışan Active Directory/DNS sunucusundadır. Dosya paylaşım tanığı **na domcontr-0**denir. Azure'a vpn bağlantısı (Siteden Siteye VPN veya Azure ExpressRoute üzerinden) yapılandırdığınız için, Active Directory/DNS hizmetiniz şirket bünyesindedir ve dosya paylaşımı tanığı çalıştırmak için uygun değildir.

   > [!NOTE]
   > Etkin Dindizi/DNS hizmetiniz yalnızca şirket içinde çalışıyorsa, dosya paylaşım tanığınızı şirket içinde çalışan Active Directory/DNS Windows işletim sisteminde yapılandırmayın. Azure'da çalışan küme düğümleri ile şirket içinde Active Directory/DNS arasında ağ gecikmesi çok büyük olabilir ve bağlantı sorunlarına neden olabilir. Dosya paylaşım tanığını küme düğümüne yakın çalışan bir Azure sanal makinesinde yapılandırdığından emin olun.  
   >
   >

   Çoğunluk sürücüsünün en az 1.024 MB boş alana ihtiyacı vardır. Çoğunluk sürücüsü için 2.048 MB boş alan öneriyoruz.

2. Küme adı nesnesi ekleyin.

   ![Şekil 30: Küme adı nesnesi için paylaşım üzerindeki izinleri atama][sap-ha-guide-figure-3019]

   _**Şekil 30:** Küme adı nesnesi için paylaşımüzerindeki izinleri atama_

   İzinlerin küme adı nesnesi (örneğimizde **pr1-ascs-vir$**) için paylaşımdaki verileri değiştirme yetkisini içerdiğinden emin olun.

3. Küme adı nesnesini listeye eklemek için **Ekle'yi**seçin. Şekil 31'de gösterilenlere ek olarak, bilgisayar nesnelerini denetlemek için filtreyi değiştirin.

   ![Şekil 31: Nesne Türlerini bilgisayarları içerecek şekilde değiştirme][sap-ha-guide-figure-3020]

   _**Şekil 31:** Nesne Türlerini bilgisayarları içerecek şekilde değiştirme_

   ![Şekil 32: Bilgisayarlar onay kutusunu seçin][sap-ha-guide-figure-3021]

   _**Şekil 32:** **Bilgisayarlar** onay kutusunu seçin_

4. Şekil 31'de gösterildiği gibi küme adı nesnesini girin. Kayıt zaten oluşturulduğundan, Şekil 30'da gösterildiği gibi izinleri değiştirebilirsiniz.

5. Paylaşımın **Güvenlik** sekmesini seçin ve ardından küme adı nesnesi için daha ayrıntılı izinler ayarlayın.

   ![Şekil 33: Dosya paylaşımı çoğunluğundaki küme adı nesnesinin güvenlik özniteliklerini ayarlama][sap-ha-guide-figure-3022]

   _**Şekil 33:** Dosya paylaşımı çoğunluğundaki küme adı nesnesinin güvenlik özniteliklerini ayarlama_

##### <a name="set-the-file-share-witness-quorum-in-failover-cluster-manager"></a><a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a>Failover Cluster Manager'da dosya paylaşımı tanık çoğunluklarını ayarlama

1. Yapılaşı Çoğunluk Ayar Sihirbazı'nı açın.

   ![Şekil 34: Yapılandırma Küme Çoğunluk Ayar Sihirbazı'nı Başlat][sap-ha-guide-figure-3023]

   _**Şekil 34:** Yapılandırma Küme Çoğunluk Ayar Sihirbazı'nı Başlat_

2. Çoğunluk **Yapılandırmasını Seç** sayfasında, **çoğunluk tanığını seçin' i**seçin.

   ![Şekil 35: Quorum yapılandırmaları arasından seçim yapabilirsiniz][sap-ha-guide-figure-3024]

   _**Şekil 35:** Aralarından seçim yapabileceğiniz Quorum yapılandırmaları_

3. Çoğunluk **Tanığı** seç sayfasında, **dosya paylaşımı tanığını yapılandır'ı**seçin.

   ![Şekil 36: Dosya paylaşım tanığını seçin][sap-ha-guide-figure-3025]

   _**Şekil 36:** Dosya paylaşımı tanığını seçin_

4. Dosya paylaşımına UNC yolunu girin (örneğimizde, \\domcontr-0\FSW). Yapabileceğiniz değişikliklerin listesini görmek için **İleri'yi**seçin.

   ![Şekil 37: Tanık paylaşımı için dosya paylaşım konumunu tanımlayın][sap-ha-guide-figure-3026]

   _**Şekil 37:** Tanık paylaşımı için dosya paylaşım konumunu tanımlama_

5. İstediğiniz değişiklikleri seçin ve sonra **İleri'yi**seçin. Şekil 38'de gösterildiği gibi küme yapılandırmasını başarıyla yeniden yapılandırmanız gerekir.  

   ![Şekil 38: Kümeyi yeniden yapılandırdığınızın doğrulanması][sap-ha-guide-figure-3027]

   _**Şekil 38:** Kümeyi yeniden yapılandırdığınıza dair onay_

Windows Failover Cluster'ı başarıyla yükledikten sonra, başarısız algılamayı Azure'daki koşullara uyarlamak için bazı eşiklerde değişiklikler yapılması gerekir. Değiştirilecek parametreler bu blogda belgelenmiştir: [https://techcommunity.microsoft.com/t5/Failover-Clustering/Tuning-Failover-Cluster-Network-Thresholds/ba-p/371834](https://techcommunity.microsoft.com/t5/Failover-Clustering/Tuning-Failover-Cluster-Network-Thresholds/ba-p/371834). ASCS/SCS için Windows Cluster Yapılandırması'nı oluşturan iki VM'nizin aynı Alt Net'te olduğunu varsayarsak, aşağıdaki parametrelerin şu değerlerle değiştirilmesi gerekir:  
- SameSubNetDelay = 2000  
- SameSubnetThreshold = 15  
- YönlendirmeTarih Boyu = 30  

Bu ayarlar müşterilerle test edildi ve bir tarafta yeterince esnek olmak için iyi bir uzlaşma sağladı. Öte yandan bu ayarlar SAP yazılımı veya düğüm/VM hatasında gerçek hata koşullarında yeterince hızlı arıza lar sağlıyordu. 

### <a name="install-sios-datakeeper-cluster-edition-for-the-sap-ascsscs-cluster-share-disk"></a><a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a>SAP ASCS/SCS küme paylaşım diski için SIOS DataKeeper Cluster Edition'ı yükleyin

Artık Azure'da çalışan bir Windows Server Failover Kümeleme yapılandırmanız var. Ancak, sap ASCS/SCS örneğini yüklemek için paylaşılan bir disk kaynağına ihtiyacınız vardır. Azure'da ihtiyacınız olan paylaşılan disk kaynaklarını oluşturamazsınız. SIOS DataKeeper Cluster Edition paylaşılan disk kaynakları oluşturmak için kullanabileceğiniz bir üçüncü taraf çözümdür.

SAP ASCS/SCS küme paylaşım diski için SIOS DataKeeper Cluster Edition'ın yüklenmesi şu görevleri içerir:

- .NET Framework 3.5 ekleme
- SIOS DataKeeper'ı yükleme
- SIOS DataKeeper'ı kurma

#### <a name="add-the-net-framework-35"></a><a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a>.NET Framework 3.5 ekle
Microsoft .NET Framework 3.5, Windows Server 2012 R2'de otomatik olarak etkinleştirilmez veya yüklenmez. SIOS DataKeeper, .NET Framework'ün DataKeeper'ı yüklediğiniz tüm düğümlerde olmasını gerektirdiğinden, .NET Framework 3.5'i kümedeki tüm sanal makinelerin konuk işletim sistemine yüklemeniz gerekir.

.NET Framework 3.5'i eklemenin iki yolu vardır:

- Şekil 39'da gösterildiği gibi Windows'da Rol ve Özellikler Ekle Sihirbazı'nı kullanın.

  ![Şekil 39: Rol ve Özellikler Ekle Sihirbazı'nı kullanarak .NET Framework 3.5'i yükleyin][sap-ha-guide-figure-3028]

  _**Şekil 39:** Roller ve Özellikler Ekle Sihirbazı'nı kullanarak .NET Framework 3.5'i yükleyin_

  ![Şekil 40: Roller ve Özellikler Ekle Sihirbazı'nı kullanarak .NET Framework 3.5'i yüklediğinizde yükleme ilerleme çubuğu][sap-ha-guide-figure-3029]

  _**Şekil 40:** Roller ve Özellikler Ekle Sihirbazı'nı kullanarak .NET Framework 3.5'i yüklediğinizde yükleme ilerleme çubuğu_

- Komut satırı aracı dism.exe kullanın. Bu tür bir yükleme için, Windows yükleme medyasındaki SxS dizinine erişmeniz gerekir. Yükseltilmiş bir komut isteminde, yazın:

  ```
  Dism /online /enable-feature /featurename:NetFx3 /All /Source:installation_media_drive:\sources\sxs /LimitAccess
  ```

#### <a name="install-sios-datakeeper"></a><a name="dd41d5a2-8083-415b-9878-839652812102"></a>SIOS DataKeeper'ı yükleyin

Kümedeki her düğüme SIOS DataKeeper Cluster Edition'ı yükleyin. SIOS DataKeeper ile sanal paylaşılan depolama oluşturmak için senkronize bir ayna oluşturun ve ardından küme paylaşılan depolamayı simüle edin.

SIOS yazılımını yüklemeden önce, etki alanı kullanıcısı **DataKeeperSvc'yi**oluşturun.

> [!NOTE]
> Her iki küme düğümünde de Yerel **Yönetici** grubuna **DataKeeperSvc** kullanıcısını ekleyin.
>
>

SIOS DataKeeper'ı yüklemek için:

1. SIOS yazılımını her iki küme düğümüne de yükleyin.

   ![SIOS yükleyici][sap-ha-guide-figure-3030]

   ![Şekil 41: SIOS DataKeeper kurulumunun ilk sayfası][sap-ha-guide-figure-3031]

   _**Şekil 41:** SIOS DataKeeper kurulumunun ilk sayfası_

2. Şekil 42'de gösterilen iletişim kutusunda **Evet'i**seçin.

   ![Şekil 42: DataKeeper bir hizmetin devre dışı bırakıldığını bildirir][sap-ha-guide-figure-3032]

   _**Şekil 42:** DataKeeper, bir hizmetin devre dışı bırakıldığını bildirir_

3. Şekil 43'te gösterilen iletişim kutusunda **Etki Alanı veya Sunucu hesabını**seçmenizi öneririz.

   ![Şekil 43: SIOS DataKeeper için kullanıcı seçimi][sap-ha-guide-figure-3033]

   _**Şekil 43:** SIOS DataKeeper için kullanıcı seçimi_

4. SIOS DataKeeper için oluşturduğunuz etki alanı hesabı kullanıcı adını ve parolalarını girin.

   ![Şekil 44: SIOS DataKeeper kurulumu için alan adı kullanıcı adı ve parolasını girin][sap-ha-guide-figure-3034]

   _**Şekil 44:** SIOS DataKeeper kurulumu için alan adı kullanıcı adı ve parolasını girin_

5. Şekil 45'te gösterildiği gibi SIOS DataKeeper örneğinizin lisans anahtarını yükleyin.

   ![Şekil 45: SIOS DataKeeper lisans anahtarınızı girin][sap-ha-guide-figure-3035]

   _**Şekil 45:** SIOS DataKeeper lisans anahtarınızı girin_

6. İstendiğinde, sanal makineyi yeniden başlatın.

#### <a name="set-up-sios-datakeeper"></a><a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a>SIOS DataKeeper'ı ayarlama

SIOS DataKeeper'ı her iki düğüme de yükledikten sonra yapılandırmayı başlatmanız gerekir. Yapılandırmanın amacı, sanal makinelerin her birine iliştirilen ek VHD'ler arasında eşzamanlı veri çoğaltma olmasıdır.

1. DataKeeper Yönetimi ve Yapılandırma aracını başlatın ve ardından **Sunucuyu Bağla'yı**seçin. (Şekil 46'da bu seçenek kırmızı daire içine alınmış olur.)

   ![Şekil 46: SIOS DataKeeper Yönetimi ve Yapılandırma aracı][sap-ha-guide-figure-3036]

   _**Şekil 46:** SIOS DataKeeper Yönetimi ve Yapılandırma aracı_

2. Yönetim ve Yapılandırma aracının bağlanması gereken ilk düğümün adını veya TCP/IP adresini ve ikinci adımda ikinci düğümü girin.

   ![Şekil 47: Yönetim ve Yapılandırma aracının bağlanması gereken ilk düğümün adını veya TCP/IP adresini ve ikinci adımda ikinci düğümü ekleyin][sap-ha-guide-figure-3037]

   _**Şekil 47:** Yönetim ve Yapılandırma aracının bağlanması gereken ilk düğümün adını veya TCP/IP adresini ekleyin ve ikinci adımda ikinci düğüm_

3. İki düğüm arasındaki çoğaltma işini oluşturun.

   ![Şekil 48: Bir çoğaltma işi oluşturma][sap-ha-guide-figure-3038]

   _**Şekil 48:** Çoğaltma işi oluşturma_

   Sihirbaz, çoğaltma işi oluşturma sürecinde size yol gösteriyor.
4. Kaynak düğümün adını, TCP/IP adresini ve disk hacmini tanımlayın.

   ![Şekil 49: Çoğaltma işinin adını tanımlayın][sap-ha-guide-figure-3039]

   _**Şekil 49:** Çoğaltma işinin adını tanımlama_

   ![Şekil 50: Geçerli kaynak düğüm olmalıdır düğüm için temel verileri tanımlayın][sap-ha-guide-figure-3040]

   _**Şekil 50:** Geçerli kaynak düğümü olması gereken düğüm için temel verileri tanımlayın_

5. Hedef düğümün adını, TCP/IP adresini ve disk hacmini tanımlayın.

   ![Şekil 51: Geçerli hedef düğüm olması gereken düğüm için temel verileri tanımlayın][sap-ha-guide-figure-3041]

   _**Şekil 51:** Geçerli hedef düğüm olması gereken düğüm için temel verileri tanımlayın_

6. Sıkıştırma algoritmalarını tanımlayın. Örneğimizde, çoğaltma akışını sıkıştırmanızı öneririz. Özellikle yeniden eşitleme durumlarında, çoğaltma akışının sıkıştırılması yeniden eşitleme süresini önemli ölçüde azaltır. Sıkıştırmanın sanal bir makinenin CPU ve RAM kaynaklarını kullandığını unutmayın. Sıkıştırma oranı arttıkça, kullanılan CPU kaynaklarının hacmi de artar. Bu ayarı daha sonra da ayarlayabilirsiniz.

7. Denetlemeniz gereken diğer bir ayar da çoğaltmanın eşzamanlı mı yoksa eşzamanlı mı oluştuğudur. *SAP ASCS/SCS yapılandırmalarını koruduğunuzda, senkron çoğaltma kullanmanız gerekir.*  

   ![Şekil 52: Çoğaltma ayrıntılarını tanımlama][sap-ha-guide-figure-3042]

   _**Şekil 52:** Çoğaltma ayrıntılarını tanımlama_

8. Çoğaltma işi tarafından çoğaltılan birimin paylaşılan disk olarak Windows Server Failover Clustercluster yapılandırmasına yansıtılıp temsil edilmemesi gerektiğini tanımlayın. SAP ASCS/SCS yapılandırması için, Windows kümesinin çoğaltılan birimi küme birimi olarak kullanabileceği paylaşılan bir disk olarak görmesi için **Evet'i** seçin.

   ![Şekil 53: Yinelenen hacmi küme hacmi olarak ayarlamak için Evet'i seçin][sap-ha-guide-figure-3043]

   _**Şekil 53:** Yinelenen birimi küme birimi olarak ayarlamak için **Evet'i** seçin_

   Birim oluşturulduktan sonra, DataKeeper Yönetimi ve Yapılandırma aracı çoğaltma işinin etkin olduğunu gösterir.

   ![Şekil 54: SAP ASCS/SCS paylaşım diski için DataKeeper senkron yansıtma etkin][sap-ha-guide-figure-3044]

   _**Şekil 54:** SAP ASCS/SCS paylaşım diski için DataKeeper senkron yansıtma etkin_

   Failover Cluster Manager şimdi Şekil 55'te gösterildiği gibi diski DataKeeper diski olarak gösterir.

   ![Şekil 55: Failover Cluster Manager, DataKeeper'ın çoğalttettiği diski gösterir][sap-ha-guide-figure-3045]

   _**Şekil 55:** Failover Cluster Manager, DataKeeper'ın çoğalttettiği diski gösterir_

## <a name="install-the-sap-netweaver-system"></a><a name="a06f0b49-8a7a-42bf-8b0d-c12026c5746b"></a>SAP NetWeaver sistemini kurun

Kurulumlar kullandığınız DBMS sistemine bağlı olarak değiştiğinden, DBMS kurulumlarını açıklamayacağız. Ancak, DBMS ile ilgili yüksek kullanılabilirlik sorunlarının, farklı DBMS satıcılarının Azure için desteklediği işlevlerle giderildigini varsayıyoruz. Örneğin, SQL Server için Her Zaman veya veritabanı yansıtma ve Oracle veritabanları için Oracle Data Guard. Bu makalede kullandığımız senaryoda, DBMS'ye daha fazla koruma eklemedik.

Farklı DBMS hizmetleri Azure'da bu tür kümelenmiş SAP ASCS/SCS yapılandırmasıyla etkileşimde bulunduğunda özel bir husus yoktur.

> [!NOTE]
> SAP NetWeaver ABAP sistemlerinin, Java sistemlerinin ve ABAP+Java sistemlerinin kurulum prosedürleri hemen hemen aynıdır. En önemli fark, bir SAP ABAP sisteminin bir ASCS örneğine sahip olmasıdır. SAP Java sisteminde bir SCS örneği vardır. SAP ABAP+Java sisteminde aynı Microsoft failover küme grubunda çalışan bir ASCS örneği ve bir SCS örneği vardır. Her SAP NetWeaver yükleme yığını için herhangi bir yükleme farklılıkları açıkça belirtilmiştir. Diğer tüm parçaların aynı olduğunu varsayabilirsiniz.  
>
>

### <a name="install-sap-with-a-high-availability-ascsscs-instance"></a><a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a>SAP'yi yüksek kullanılabilirlikli BIR ASCS/SCS örneğiyle yükleme

> [!IMPORTANT]
> Sayfa dosyanızı DataKeeper yansıtılmış ciltlere yerleştirmeyin. DataKeeper yansıtılmış birimleri desteklemez. Sayfa dosyanızı varsayılan olan bir Azure sanal makinenin geçici sürücü D'sinde bırakabilirsiniz. Zaten orada değilse, Windows sayfa dosyasını Azure sanal makinenizin D'sini sürücüsüne taşıyın.
>
>

SAP'yi yüksek kullanılabilirlikli bir ASCS/SCS örneğiyle yüklemek şu görevleri içerir:

- Kümelenmiş SAP ASCS/SCS örneği için sanal ana bilgisayar adı oluşturma
- SAP ilk küme düğümyükleme
- ASCS/SCS örneğinin SAP profilini değiştirme
- Sonda bağlantı noktası ekleme
- Windows güvenlik duvarı sondası bağlantı noktasını açma

#### <a name="create-a-virtual-host-name-for-the-clustered-sap-ascsscs-instance"></a><a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a>Kümelenmiş SAP ASCS/SCS örneği için sanal bir ana bilgisayar adı oluşturma

1. Windows DNS yöneticisinde, ASCS/SCS örneğinin sanal ana bilgisayar adı için bir DNS girişi oluşturun.

   > [!IMPORTANT]
   > ASCS/SCS örneğinin sanal ana bilgisayar adına atadığınız IP adresi, Azure Yük Dengeleyicisi'ne**<*(SID*>-lb-ascs)** atadığınız IP adresiyle aynı olmalıdır.  
   >
   >

   Sanal SAP ASCS/SCS ana bilgisayar adının IP adresi **(pr1-ascs-sap)** Azure Yük Dengeleyicisinin **(pr1-lb-ascs)** IP adresiyle aynıdır.

   ![Şekil 56: SAP ASCS/SCS küme sanal adı ve TCP/IP adresi için DNS girişini tanımlayın][sap-ha-guide-figure-3046]

   _**Şekil 56:** SAP ASCS/SCS küme sanal adı ve TCP/IP adresi için DNS girişini tanımlayın_

2. Sanal ana bilgisayar adına atanan IP adresini tanımlamak için **DNS Yöneticisi** > **Etki Alanı'nı**seçin.

   ![Şekil 57: SAP ASCS/SCS küme yapılandırması için yeni sanal ad ve TCP/IP adresi][sap-ha-guide-figure-3047]

   _**Şekil 57:** SAP ASCS/SCS küme yapılandırması için yeni sanal ad ve TCP/IP adresi_

#### <a name="install-the-sap-first-cluster-node"></a><a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a>SAP ilk küme düğüm'e yükleme

1. Küme düğümü A'daki ilk küme düğümü seçeneğini çalıştırın. Örneğin, **pr1-ascs-0** ana bilgisayar.
2. Azure dahili yük dengeleyicisinin varsayılan bağlantı noktalarını tutmak için şunları seçin:

   * **ABAP sistemi**: **ASCS** örnek numarası **00**
   * **Java sistemi**: **SCS** örnek numarası **01**
   * **ABAP+Java sistemi**: **ASCS** örnek numarası **00** ve **SCS** örnek numarası **01**

   ABAP ASCS örneği için 00 ve Java SCS örneği için 01 dışındaki örnek numaralarını kullanmak için, öncelikle [Azure dahili yük dengeleyicisi için ASCS/SCS varsayılan yük dengeleme kurallarını değiştir'de][sap-ha-guide-8.9]açıklanan Azure dahili yük dengeleyici varsayılan yük dengeleme kurallarını değiştirmeniz gerekir.

Sonraki birkaç görev standart SAP yükleme belgelerinde açıklanmaz.

> [!NOTE]
> SAP yükleme belgeleri, ilk ASCS/SCS küme düğümünün nasıl yüklenir olduğunu açıklar.
>
>

#### <a name="modify-the-sap-profile-of-the-ascsscs-instance"></a><a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a>ASCS/SCS örneğinin SAP profilini değiştirme

Yeni bir profil parametresi eklemeniz gerekir. Profil parametresi, SAP iş süreçleri ile enqueue sunucusu arasındaki bağlantıların çok uzun süre boşta kaldığında kapanmasını önler. [SAP ASCS/SCS örneğinin her iki küme düğümünde de kayıt defteri ekleme girdilerinde][sap-ha-guide-8.11]sorun senaryosundan bahsettik. Bu bölümde, bazı temel TCP/IP bağlantı parametrelerinde de iki değişiklik yaptık. İkinci adımda, bağlantıların Azure dahili yük `keep_alive` dengeleyicisinin boşta eşiğine çarpmaması için enqueue sunucusunu sinyal gönderecek şekilde ayarlamanız gerekir.

ASCS/SCS örneğinin SAP profilini değiştirmek için:

1. Sap ASCS/SCS örnek profiline bu profil parametresini ekleyin:

   ```
   enque/encni/set_so_keepalive = true
   ```
   Örneğimizde, yol:

   `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_ASCS00_pr1-ascs-sap`

   Örneğin, SAP SCS örnek profiline ve ilgili yola:

   `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_SCS01_pr1-ascs-sap`

2. Değişiklikleri uygulamak için SAP ASCS /SCS örneğini yeniden başlatın.

#### <a name="add-a-probe-port"></a><a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a>Sonda bağlantı noktası ekleme

Tüm küme yapılandırmasının Azure Yük Dengeleyicisi ile çalışmasını sağlamak için dahili yük bakiyeleyicisinin sonda işlevini kullanın. Azure dahili yük dengeleyicisi genellikle gelen iş yükünü katılımcı sanal makineler arasında eşit olarak dağıtır. Ancak, yalnızca bir örnek etkin olduğundan, bu bazı küme yapılandırmalarında çalışmaz. Diğer örnek pasiftir ve iş yükünün hiçbirini kabul edemez. Sonda işlevi, Azure dahili yük dengeleyicisinin yalnızca etkin bir örneğe çalışma atamasına yardımcı olur. Sonda işlevi yle, dahili yük dengeleyicisi hangi örneklerin etkin olduğunu algılayabilir ve ardından yalnızca iş yüküolan örneği hedefleyebilir.

Sonda bağlantı noktası eklemek için:

1. Aşağıdaki PowerShell komutunu çalıştırarak geçerli **ProbePort** ayarını kontrol edin. Küme yapılandırmasındaki sanal makinelerden birinden çalıştırın.

   ```PowerShell
   $SAPSID = "PR1"     # SAP <SID>

   $SAPNetworkIPClusterName = "SAP $SAPSID IP"
   Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter
   ```

2. Bir sonda bağlantı noktası tanımlayın. Varsayılan sonda bağlantı **0**noktası numarası 0'dır. Örneğimizde, biz prob bağlantı noktası **62000**kullanın.

   ![Şekil 58: Küme yapılandırma sondası bağlantı noktası varsayılan olarak 0'dır][sap-ha-guide-figure-3048]

   _**Şekil 58:** Varsayılan küme yapılandırma sondası bağlantı noktası 0_

   Bağlantı noktası numarası SAP Azure Kaynak Yöneticisi şablonlarında tanımlanır. PowerShell'deki bağlantı noktası numarasını atayabilirsiniz.

   **SAP <*SID*> IP** küme kaynağı için yeni bir ProbePort değeri ayarlamak için aşağıdaki PowerShell komut dosyasını çalıştırın. Ortamınız için PowerShell değişkenlerini güncelleştirin. Komut dosyası çalıştırıldıktan sonra, değişiklikleri etkinleştirmek için SAP küme grubunu yeniden başlatmanız istenir.

   ```PowerShell
   $SAPSID = "PR1"      # SAP <SID>
   $ProbePort = 62000   # ProbePort of the Azure Internal Load Balancer

   Clear-Host
   $SAPClusterRoleName = "SAP $SAPSID"
   $SAPIPresourceName = "SAP $SAPSID IP"
   $SAPIPResourceClusterParameters =  Get-ClusterResource $SAPIPresourceName | Get-ClusterParameter
   $IPAddress = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "Address" }).Value
   $NetworkName = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "Network" }).Value
   $SubnetMask = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "SubnetMask" }).Value
   $OverrideAddressMatch = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "OverrideAddressMatch" }).Value
   $EnableDhcp = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "EnableDhcp" }).Value
   $OldProbePort = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "ProbePort" }).Value

   $var = Get-ClusterResource | Where-Object {  $_.name -eq $SAPIPresourceName  }

   Write-Host "Current configuration parameters for SAP IP cluster resource '$SAPIPresourceName' are:" -ForegroundColor Cyan
   Get-ClusterResource -Name $SAPIPresourceName | Get-ClusterParameter

   Write-Host
   Write-Host "Current probe port property of the SAP cluster resource '$SAPIPresourceName' is '$OldProbePort'." -ForegroundColor Cyan
   Write-Host
   Write-Host "Setting the new probe port property of the SAP cluster resource '$SAPIPresourceName' to '$ProbePort' ..." -ForegroundColor Cyan
   Write-Host

   $var | Set-ClusterParameter -Multiple @{"Address"=$IPAddress;"ProbePort"=$ProbePort;"Subnetmask"=$SubnetMask;"Network"=$NetworkName;"OverrideAddressMatch"=$OverrideAddressMatch;"EnableDhcp"=$EnableDhcp}

   Write-Host

   $ActivateChanges = Read-Host "Do you want to take restart SAP cluster role '$SAPClusterRoleName', to activate the changes (yes/no)?"

   if($ActivateChanges -eq "yes"){
   Write-Host
   Write-Host "Activating changes..." -ForegroundColor Cyan

   Write-Host
   write-host "Taking SAP cluster IP resource '$SAPIPresourceName' offline ..." -ForegroundColor Cyan
   Stop-ClusterResource -Name $SAPIPresourceName
   sleep 5

   Write-Host "Starting SAP cluster role '$SAPClusterRoleName' ..." -ForegroundColor Cyan
   Start-ClusterGroup -Name $SAPClusterRoleName

   Write-Host "New ProbePort parameter is active." -ForegroundColor Green
   Write-Host

   Write-Host "New configuration parameters for SAP IP cluster resource '$SAPIPresourceName':" -ForegroundColor Cyan
   Write-Host
   Get-ClusterResource -Name $SAPIPresourceName | Get-ClusterParameter
   }else
   {
   Write-Host "Changes are not activated."
   }
   ```

   SAP <** *SID* > ** küme rolünü çevrimiçi duruma getirdikten sonra **ProbePort'un** yeni değere ayarlı olduğunu doğrulayın.

   ```PowerShell
   $SAPSID = "PR1"     # SAP <SID>

   $SAPNetworkIPClusterName = "SAP $SAPSID IP"
   Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter

   ```

   ![Şekil 59: Küme bağlantı noktasını yeni değeri ayarladıktan sonra yoklama][sap-ha-guide-figure-3049]

   _**Şekil 59:** Yeni değeri ayarladıktan sonra küme bağlantı noktasını yokla_

#### <a name="open-the-windows-firewall-probe-port"></a><a name="4498c707-86c0-4cde-9c69-058a7ab8c3ac"></a>Windows güvenlik duvarı sondası bağlantı noktasını açma

Her iki küme düğümünde de bir Windows güvenlik duvarı sondası bağlantı noktası açmanız gerekir. Windows güvenlik duvarı sondası bağlantı noktasını açmak için aşağıdaki komut dosyasını kullanın. Ortamınız için PowerShell değişkenlerini güncelleştirin.

  ```PowerShell
  $ProbePort = 62000   # ProbePort of the Azure Internal Load Balancer

  New-NetFirewallRule -Name AzureProbePort -DisplayName "Rule for Azure Probe Port" -Direction Inbound -Action Allow -Protocol TCP -LocalPort $ProbePort
  ```

**ProbePort** **62000**olarak ayarlanır. Şimdi dosya paylaşımı ** \\\ascsha-clsap\sapmnt** diğer ana bilgisayarlarından erişebilirsiniz, örneğin **ascsha-dbas.**

### <a name="install-the-database-instance"></a><a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a>Veritabanı örneğini yükleme

Veritabanı örneğini yüklemek için SAP yükleme belgelerinde açıklanan işlemi izleyin.

### <a name="install-the-second-cluster-node"></a><a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a>İkinci küme düğümini yükleme

İkinci kümeyi yüklemek için SAP yükleme kılavuzundaki adımları izleyin.

### <a name="change-the-start-type-of-the-sap-ers-windows-service-instance"></a><a name="094bc895-31d4-4471-91cc-1513b64e406a"></a>SAP ERS Windows hizmet örneğinin başlangıç türünü değiştirme

SAP ERS Windows hizmetinin başlangıç türünü her iki küme düğümünde **de Otomatik (Gecikmeli Başlangıç)** olarak değiştirin.

![Şekil 60: SAP ERS örneğinin servis türünü gecikmiş otomatik][sap-ha-guide-figure-3050]

_**Şekil 60:** SAP ERS örneğinin hizmet türünü gecikmiş otomatik olarak değiştirme_

### <a name="install-the-sap-primary-application-server"></a><a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a>SAP Birincil Uygulama Sunucusunu Yükleme

BIRINCIL Uygulama Sunucusu (PAS) *örneğini,* PAS'ı barındırmak üzere belirlediğiniz sanal makineye SID>-di-0 <yükleyin. Azure veya DataKeeper'a özgü ayarlarda bağımlılık yoktur.

### <a name="install-the-sap-additional-application-server"></a><a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a>SAP Ek Uygulama Sunucusunu Yükleme

SAP Application Server örneğini barındırmak için belirlediğiniz tüm sanal makinelere bir SAP Ek Uygulama Sunucusu (AAS) yükleyin. Örneğin, <*SID*>-di-1'de <*SID* &gt;>-di-n.&lt;

> [!NOTE]
> Bu, yüksek kullanılabilirlik sap NetWeaver sisteminin kurulumunu tamamlar. Ardından, başarısız test ile devam edin.
>


## <a name="test-the-sap-ascsscs-instance-failover-and-sios-replication"></a><a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a>SAP ASCS/SCS örneği başarısız ve SIOS çoğaltma test
Failover Cluster Manager ve SIOS DataKeeper Management and Configuration aracını kullanarak sap ASCS/SCS örnek failover ve SIOS disk çoğaltmasını test etmek ve izlemek kolaydır.

### <a name="sap-ascsscs-instance-is-running-on-cluster-node-a"></a><a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a>SAP ASCS/SCS örneği küme düğümü A üzerinde çalışıyor

**SAP PR1** küme grubu Küme düğümü A üzerinde çalışıyor. Örneğin, **pr1-ascs-0**. **SAP PR1** küme grubunun bir parçası olan ve ASCS/SCS örneğinin kullandığı paylaşılan disk sürücüsü S'yi A düğümkümesine atayın.

![Şekil 61: Failover Cluster Manager: SAP <SID> küme grubu Küme düğümü A üzerinde çalışıyor][sap-ha-guide-figure-5000]

_**Şekil 61:** Failover Cluster Manager: SAP <*SID*> küme grubu küme düğümü A üzerinde çalışıyor_

SIOS DataKeeper Yönetimi ve Yapılandırma aracında, paylaşılan disk verilerinin a kümesindeki kaynak birim sürücü S'den b kümesi düğümündeki hedef ses düzeyi Sürücüsü S'ye eşzamanlı olarak çoğaltıldığını görebilirsiniz. Örneğin, **pr1-ascs-0 [10.0.0.40]** ile **pr1-ascs-1 [10.0.0.41]** olarak çoğaltılır.

![Şekil 62: SIOS DataKeeper'da, Yerel hacim Küme düğümü A'dan B kümesi düğümüne][sap-ha-guide-figure-5001]

_**Şekil 62:** SIOS DataKeeper'da, yerel birimi A kümesi düğümünden B kümesi düğümüne çoğaltma_

### <a name="failover-from-node-a-to-node-b"></a><a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a>A düğümünden Düğüm B'ye failover

1. SAP <*SID*> küme grubunun Küme düğümü A'dan B kümesidüğümüne başarısız olmasını başlatmak için bu seçeneklerden birini seçin:
   - Failover Cluster Yöneticisi'ni kullan  
   - Failover Cluster PowerShell'i kullan

   ```PowerShell
   $SAPSID = "PR1"     # SAP <SID>

   $SAPClusterGroup = "SAP $SAPSID"
   Move-ClusterGroup -Name $SAPClusterGroup

   ```
2. Windows konuk işletim sisteminde a kümesi düğümünün yeniden başlatılması (bu, SAP <*SID*> küme grubunun A düğümünden B düğümüne otomatik olarak başarısız olmasını başlatır).  
3. Küme düğümü A'yı Azure portalından yeniden başlatın (bu, SAP <*SID*> küme grubunun A düğümünden B düğümüne otomatik olarak başarısız olmasını başlatır).  
4. Azure PowerShell kullanarak küme düğümü A'yı yeniden başlatın (bu, SAP <*SID*> küme grubunun A düğümünden B düğümüne otomatik olarak başarısız olmasını başlatır).

   Başarısız olduktan sonra, SAP <*SID*> küme grubu B kümesi üzerinde çalışıyor. Örneğin, **pr1-ascs-1**üzerinde çalışıyor.

   ![Şekil 63: Failover Cluster Manager'da SAP <SID> küme grubu B kümesi üzerinde çalışıyor][sap-ha-guide-figure-5002]

   _**Şekil 63**: Failover Cluster Manager'da SAP <*SID*> küme grubu B kümesi üzerinde çalışıyor_

   Paylaşılan disk şimdi küme düğümü b. SIOS DataKeeper üzerine monte edilir küme düğümü B'deki kaynak hacim sürücüsüS'ten gelen verileri çoğaltarak Küme düğümü A'daki S'yi hedeflemek için kullanılır. Örneğin, **pr1-ascs-1 [10.0.0.41]** den **pr1-ascs-0 [10.0.0.40]** çoğalıyor.

   ![Şekil 64: SIOS DataKeeper yerel hacmi b kümesi düğümünden küme düğümüA kopyalar][sap-ha-guide-figure-5003]

   _**Şekil 64:** SIOS DataKeeper yerel hacmi b kümesi düğümünden küme düğümü A'ya kopyalar_
