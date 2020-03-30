---
title: SAP NetWeaver için Azure Sanal Makineler dağıtımı | Microsoft Dokümanlar
description: Azure'da Linux sanal makinelerinde SAP yazılımLarını nasıl dağıtılayın öğrenin.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: MSSedusch
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 1c4f1951-3613-4a5a-a0af-36b85750c84e
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/16/2019
ms.author: sedusch
ms.openlocfilehash: 7fb87380047d046a580d1ad62b1d7107a94bb297
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80239897"
---
# <a name="azure-virtual-machines-deployment-for-sap-netweaver"></a>SAP NetWeaver için Azure Sanal Makineler dağıtımı

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
[2367194]:https://launchpad.support.sap.com/#/notes/2367194

[azure-cli]:../../../cli-install-nodejs.md
[azure-cli-2]:https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest
[azure-portal]:https://portal.azure.com
[azure-ps]:/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-resource-manager/management/azure-subscription-service-limits]:../../../azure-resource-manager/management/azure-subscription-service-limits.md
[azure-resource-manager/management/azure-subscription-service-limits-subscription]:../../../azure-resource-manager/management/azure-subscription-service-limits.md#subscription-limits

[dbms-guide]:dbms-guide.md (SAP için Azure Sanal Makineler DBMS dağıtımı)
[dbms-guide-2.1]:dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f (VM'ler ve VHD'ler için önbelleğe alma)
[dbms-guide-2.2]:dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 (Yazılım RAID)
[dbms-guide-2.3]:dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 (Microsoft Azure Depolama)
[dbms-guide-2]:dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 (RDBMS dağıtımının yapısı)
[dbms-guide-3]:dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 (Azure VM'lerle yüksek kullanılabilirlik ve olağanüstü durum kurtarma)
[dbms-guide-5.5.1]:dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268 (SQL Server 2012 SP1 CU4 ve sonrası)
[dbms-guide-5.5.2]:dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b (SQL Server 2012 SP1 CU3 ve önceki sürümler)
[dbms-guide-5.6]:dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 (Azure Marketi'nden SQL Server görüntüsü kullanma)
[dbms-guide-5.8]:dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 (Azure özetinde SAP için Genel SQL Server)
[dbms-guide-5]:dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 (SQL Server RDBMS'e özgü)
[dbms-guide-8.4.1]:dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 (Depolama yapılandırması)
[dbms-guide-8.4.2]:dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d (Yedekleme ve geri yükleme)
[dbms-guide-8.4.3]:dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c (Yedekleme ve geri yükleme için performans konuları)
[dbms-guide-8.4.4]:dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 (Diğer)
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

[deployment-guide]:deployment-guide.md (SAP için Azure Sanal Makineler dağıtımı)
[deployment-guide-2.2]:deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 (SAP kaynakları)
[deployment-guide-3.1.2]:deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab (Özel bir görüntü kullanarak VM dağıtma)
[deployment-guide-3.2]:deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 (Senaryo 1: SAP için Azure Marketinden VM dağıtma)
[deployment-guide-3.3]:deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 (Senaryo 2: SAP için özel bir görüntüile VM dağıtma)
[deployment-guide-3.4]:deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1 (Senaryo 3: SAP ile genelleştirilmeyen bir Azure VHD kullanarak bir VM'yi şirket içinde taşıma)
[deployment-guide-3]:deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e (Microsoft Azure'da SAP için VM'lerin dağıtım senaryoları)
[deployment-guide-4.1]:deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 (Azure PowerShell cmdlets dağıtma)
[deployment-guide-4.2]:deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e (SAP ile ilgili PowerShell cmdlets'i indirin ve içe aktarın)
[deployment-guide-4.3]:deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc (Şirket içi etki alanına VM'ye katılın - Yalnızca Windows)
[deployment-guide-4.4.2]:deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 (Linux)
[deployment-guide-4.4]:deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d (Azure VM Aracısını indirin, yükleyin ve etkinleştirin)
[deployment-guide-4.5.1]:deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 (Azure PowerShell)
[deployment-guide-4.5.2]:deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f (Azure CLI)
[deployment-guide-4.5]:deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca (SAP için Azure Uzantısını Yapılandırma)
[deployment-guide-5.1]:deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 (SAP için Azure Uzantısı için hazırlık denetimi)
[deployment-guide-5.2]:deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 (SAP yapılandırması için Azure Uzantısı için sistem durumu denetimi)
[deployment-guide-5.3]:deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 (SAP için Azure Uzantısı Sorun Giderme)

[deployment-guide-configure-monitoring-scenario-1]:deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b (VM Uzantısını Yapılandırma)
[deployment-guide-configure-proxy]:deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d (Proxy'yi yapılandırma)
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
[deployment-guide-troubleshooting-chapter]:deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b (SAP Ana Bilgisayar Aracısı için uçuça veri toplama için denetimler ve sorun giderme)

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

[msdn-set-Azvmaemextension]:https://docs.microsoft.com/powershell/module/az.compute/set-azvmaemextension

[planning-guide]:planning-guide.md (AZURE Sanal Makineler SAP için planlama ve uygulama)
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff (Kaynak)
[planning-guide-11]:planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 (Azure Sanal Makinelerde çalışan SAP NetWeaver için yüksek kullanılabilirlik ve olağanüstü durum kurtarma)
[planning-guide-11.4.1]:planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 (SAP Uygulama Sunucuları için yüksek kullanılabilirlik)
[planning-guide-11.5]:planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f (SAP örnekleri için Otomatik Başlatma'yı kullanma)
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803 (Yalnızca Bulut - Şirket içi müşteri ağına bağımlılık yapmadan Azure'da Sanal Makine dağıtımları)
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 (Tesisler arası - Azure'da tek veya birden fazla SAP VM'nin şirket içi ağla tam olarak entegre olarak dağıtılması)
[planning-guide-3.1]:planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a (Azure bölgeleri)
[planning-guide-3.2.1]:planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 (Hata etki alanları)
[planning-guide-3.2.2]:planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 (Etki alanlarını yükseltme)
[planning-guide-3.2.3]:planning-guide.md#18810088-f9be-4c97-958a-27996255c665 (Azure kullanılabilirlik kümeleri)
[planning-guide-3.2]:planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 (Microsoft Azure sanal makineler kavramı)
[planning-guide-3.3.2]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium Depolama)
[planning-guide-5.1.1]:planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 (Genelleştirilmeyen bir diskle bir VM'yi şirket içinde Azure'a taşıma)
[planning-guide-5.1.2]:planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c (Müşteriye özgü bir görüntüyle VM dağıtma)
[planning-guide-5.2.1]:planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef (Genelleştirilmeyen bir diskle bir VM'yi şirket içinde Azure'a taşımaya hazırlık)
[planning-guide-5.2.2]:planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 (SAP için müşteriye özgü bir görüntüyle VM dağıtmaya hazırlık)
[planning-guide-5.2]:planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 (Azure için SAP ile VM'ler hazırlama)
[planning-guide-5.3.1]:planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 (Azure diski ile Azure görüntüsü arasındaki fark)
[planning-guide-5.3.2]:planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a (Şirket içinde Azure'a VHD yükleme)
[planning-guide-5.4.2]:planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 (Azure Depolama hesapları arasında diskleri kopyalama)
[planning-guide-5.5.1]:planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646 (SAP dağıtımları için VM/VHD yapısı)
[planning-guide-5.5.3]:planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d (Ekli diskler için otomatik montaj ayarlama)
[planning-guide-7.1]:planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 (SAP NetWeaver demo/eğitim senaryosuna sahip tek VM)
[planning-guide-7]:planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 (SAP örneklerinin Yalnızca Bulut'a dağıtım kavramları)
[planning-guide-9.1]:planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 (SAP için Azure İzleme Çözümü)
[planning-guide-azure-premium-storage]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium Depolama)
[planning-guide-managed-disks]:planning-guide.md#c55b2c6e-3ca1-4476-be16-16c81927550f (Yönetilen Diskler)
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
[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd (Microsoft Azure ağ)
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f (Depolama: Microsoft Azure Depolama ve veri diskleri)

[resource-group-authoring-templates]:../../../resource-group-authoring-templates.md
[resource-group-overview]:../../../azure-resource-manager/management/overview.md
[resource-groups-networking]:../../../networking/network-overview.md
[sap-pam]:https://support.sap.com/pam (SAP Ürün Kullanılabilirlik Matrisi)
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image-md%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-os-disk-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk-md%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-2-tier-user-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image-md%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-md%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image-md%2Fazuredeploy.json
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
[virtual-machines-Az-versus-azuresm]:virtual-machines-linux-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md (Azure Kaynak Yöneticisi şablonlarını ve Azure CLI şablonlarını kullanarak sanal makineleri dağıtma ve yönetme)
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md (Azure Kaynak Yöneticisi ve PowerShell'i kullanarak sanal makineleri yönetme)
[virtual-machines-windows-agent-user-guide]:../../extensions/agent-windows.md
[virtual-machines-linux-agent-user-guide]:../../extensions/agent-linux.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../extensions/agent-linux.md#command-line-options
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
[virtual-machines-manage-availability]:../../linux/manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:../../virtual-machines-windows-ps-create.md
[virtual-machines-sizes]:../../linux/sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]:./../../windows/sqlclassic/virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]:./../../windows/sqlclassic/virtual-machines-windows-classic-ps-sql-int-listener.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]:./../../windows/sql/virtual-machines-windows-sql-high-availability-dr.md
[virtual-machines-sql-server-infrastructure-services]:./../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md
[virtual-machines-sql-server-performance-best-practices]:./../../windows/sql/virtual-machines-windows-sql-performance.md
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
[vpn-gateway-site-to-site-create]:../../../vpn-gateway/vpn-gateway-site-to-site-create.md
[vpn-gateway-vpn-faq]:../../../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../../../cli-install-nodejs.md
[xplat-cli-azure-resource-manager]:../../../xplat-cli-azure-resource-manager.md

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Azure Sanal Makineler, bilgi işlem ve depolama kaynaklarına en az sürede ve uzun satın alma döngüleri olmadan ihtiyaç duyan kuruluşlar için çözümdür. SAP NetWeaver tabanlı uygulamalar gibi klasik uygulamaları Azure'da dağıtmak için Azure Sanal Makineleri'ni kullanabilirsiniz. Ek şirket içi kaynaklar olmadan bir uygulamanın güvenilirliğini ve kullanılabilirliğini genişletin. Azure Sanal Makineler, azure sanal makinelerini kuruluşunuzun şirket içi etki alanına, özel bulutlara ve SAP sistem ortamına entegre edebilirsiniz.

Bu makalede, alternatif dağıtım seçenekleri ve sorun giderme de dahil olmak üzere, SAP uygulamalarını Azure'daki sanal makinelere (VM) dağıtma adımlarını kapsamaktayız. Bu makale, [SAP NetWeaver için Azure Sanal Makineler planlama ve uygulamasındaki][planning-guide]bilgilere dayanmaktadır. Ayrıca SAP yazılımını yüklemek ve dağıtmak için birincil kaynaklar olan SAP yükleme belgelerini ve SAP Notlarını tamamlar.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

SAP yazılım dağıtımı için bir Azure sanal makinesi kurmak birden çok adım ve kaynak içerir. Başlamadan önce, Azure'daki sanal makinelere SAP yazılımı yüklemenin ön koşullarıyla karşılaştığından emin olun.

### <a name="local-computer"></a>Yerel bilgisayar

Windows veya Linux VM'leri yönetmek için PowerShell komut dosyası ve Azure portalı kullanabilirsiniz. Her iki araç için de Windows 7 veya windows'un daha sonraki bir sürümünü çalıştıran yerel bir bilgisayara ihtiyacınız vardır. Yalnızca Linux VM'lerini yönetmek istiyorsanız ve bu görev için bir Linux bilgisayarı kullanmak istiyorsanız, Azure CLI'yi kullanabilirsiniz.

### <a name="internet-connection"></a>İnternet bağlantısı

SAP yazılım dağıtımı için gereken araçları ve komut dosyalarını indirmek ve çalıştırmak için Internet'e bağlı olmalısınız. SAP için Azure Uzantısı'nı çalıştıran Azure VM'nin interneterişimi de gerekir. Azure VM bir Azure sanal ağının veya şirket içi etki alanının bir parçasıysa, [proxy'yi Yapılandır'da][deployment-guide-configure-proxy]açıklandığı gibi ilgili proxy ayarlarının ayarlandığından emin olun.

### <a name="microsoft-azure-subscription"></a>Microsoft Azure aboneliği

Etkin bir Azure hesabına ihtiyacınız var.

### <a name="topology-and-networking"></a>Topoloji ve ağ

AZURE'daki SAP dağıtımının topolojisini ve mimarisini tanımlamanız gerekir:

* Kullanılacak Azure depolama hesapları
* SAP sistemini dağıtmak istediğiniz sanal ağ
* SAP sistemini dağıtmak istediğiniz kaynak grubu
* SAP sistemini dağıtmak istediğiniz azure bölgesi
* SAP yapılandırması (iki katmanlı veya üç katmanlı)
* VM boyutları ve VM'lere monte edilecek ek veri disklerinin sayısı
* SAP Düzeltme ve Taşıma Sistemi (CTS) yapılandırması

SAP yazılım dağıtım işlemine başlamadan önce Azure depolama hesapları (gerekirse) veya Azure sanal ağları oluşturun ve yapılandırın. Bu kaynakların nasıl oluşturulup yapılandırılabildiğini öğrenmek [için SAP NetWeaver için Azure Sanal Makineleri planlaması ve uygulamasına][planning-guide]bakın.

### <a name="sap-sizing"></a>SAP boyutlandırma

SAP boyutlandırma için aşağıdaki bilgileri bilin:

* Örneğin, SAP Hızlı Boyutlandırma aracını ve SAP Uygulama Performans Standardı (SAPS) numarasını kullanarak öngörülen SAP iş yükü
* SAP sisteminin gerekli CPU kaynağı ve bellek tüketimi
* Saniyede gerekli giriş/çıkış (G/Ç) işlemleri
* Azure'da VM'ler arasındaki nihai iletişimin gerekli ağ bant genişliği
* Şirket içi varlıklar ve Azure tarafından dağıtılan SAP sistemi arasında gerekli ağ bant genişliği

### <a name="resource-groups"></a>Kaynak grupları

Azure Kaynak Yöneticisi'nde, Azure aboneliğinizdeki tüm uygulama kaynaklarını yönetmek için kaynak gruplarını kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış][resource-group-overview].

## <a name="resources"></a>Kaynaklar

### <a name="sap-resources"></a><a name="42ee2bdb-1efc-4ec7-ab31-fe4c22769b94"></a>SAP kaynakları

SAP yazılım dağıtımınızı ayarlarken aşağıdaki SAP kaynaklarına ihtiyacınız vardır:

* SAP Note [1928533], olan:
  * SAP yazılımının dağıtımı için desteklenen Azure VM boyutları listesi
  * Azure VM boyutları için önemli kapasite bilgileri
  * Desteklenen SAP yazılımı ve işletim sistemi (OS) ve veritabanı kombinasyonları
  * Microsoft Azure'da Windows ve Linux için gerekli SAP çekirdeği sürümü

* SAP Note [2015553,] Azure'da SAP destekli SAP yazılım dağıtımları için ön koşulları listeler.
* SAP Note [2178632,] Azure'da SAP için bildirilen tüm izleme ölçümleri hakkında ayrıntılı bilgilere sahiptir.
* SAP Note [1409604,] Azure'da Windows için gerekli SAP Host Agent sürümüne sahiptir.
* SAP Note [2191498,] Azure'da Linux için gerekli SAP Host Agent sürümüne sahiptir.
* SAP Note [2243692,] Azure'da Linux'ta SAP lisanslama hakkında bilgi edinmiştir.
* SAP Note [1984787,] SUSE Linux Enterprise Server 12 hakkında genel bilgilere sahiptir.
* SAP Note [2002167] Red Hat Enterprise Linux 7.x hakkında genel bilgilere sahiptir.
* SAP Note [2069760] Oracle Linux 7.x hakkında genel bilgilere sahiptir.
* SAP Note [1999351,] SAP için Azure Gelişmiş İzleme Uzantısı için ek sorun giderme bilgilerine sahiptir.
* SAP Note [1597355] Linux için takas alanı hakkında genel bilgilere sahiptir.
* [Azure SCN sayfasındaki SAP'de](https://wiki.scn.sap.com/wiki/x/Pia7Gg) haberler ve yararlı kaynaklar koleksiyonu vardır.
* [SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) Linux için gerekli tüm SAP Notları'na sahiptir.
* Azure PowerShell'in bir parçası olan SAP'ye özel [PowerShell][azure-ps]cmdlets.
* [Azure CLI'nin][azure-cli]bir parçası olan SAP'ye özgü Azure CLI komutları.

### <a name="windows-resources"></a><a name="42ee2bdb-1efc-4ec7-ab31-fe4c22769b94"></a>Windows kaynakları

Bu Microsoft makaleleri Azure'daki SAP dağıtımlarını kapsar:

* [SAP NetWeaver için Azure Sanal Makineler planlaması ve uygulaması][planning-guide]
* [SAP NetWeaver için Azure Sanal Makineler dağıtımı (bu makale)][deployment-guide]
* [SAP NetWeaver için Azure Sanal Makineler DBMS dağıtımı][dbms-guide]

## <a name="deployment-scenarios-for-sap-software-on-azure-vms"></a><a name="b3253ee3-d63b-4d74-a49b-185e76c4088e"></a>Azure VM'lerde SAP yazılımı için dağıtım senaryoları

Azure'da VM'leri ve ilişkili diskleri dağıtmak için birden çok seçeneğiniz vardır. Dağıtım seçenekleri arasındaki farkları anlamak önemlidir, çünkü seçtiğiniz dağıtım türüne göre VM'lerinizi dağıtıma hazırlamak için farklı adımlar atabilirsiniz.

### <a name="scenario-1-deploying-a-vm-from-the-azure-marketplace-for-sap"></a><a name="db477013-9060-4602-9ad4-b0316f8bb281"></a>Senaryo 1: SAP için Azure Marketinden VM dağıtma

VM'nizi dağıtmak için Microsoft veya Azure Marketi'ndeki bir üçüncü taraf tarafından sağlanan bir resmi kullanabilirsiniz. Market, Windows Server'ın bazı standart işletim sistemi görüntülerini ve farklı Linux dağıtımlarını sunar. Veritabanı yönetim sistemi (DBMS) SK'leri (örneğin, Microsoft SQL Server) içeren bir görüntü de dağıtabilirsiniz. DBMS SKUs'lu görüntüleri kullanma hakkında daha fazla bilgi için [SAP NetWeaver için Azure Sanal Makineler DBMS dağıtımına][dbms-guide]bakın.

Aşağıdaki akış şeması, Azure Marketi'nden bir VM dağıtmak için SAP'ye özgü adımlar dizisini gösterir:

![Azure Marketi'nden bir VM görüntüsü kullanarak SAP sistemleri için VM dağıtımının akış şeması][deployment-guide-figure-100]

#### <a name="create-a-virtual-machine-by-using-the-azure-portal"></a>Azure portalını kullanarak sanal bir makine oluşturma

Azure Marketi'nden bir görüntü içeren yeni bir sanal makine oluşturmanın en kolay yolu Azure portalını kullanmaktır.

1.  <https://portal.azure.com/#create/hub> kısmına gidin.  Veya Azure portalı menüsünde **+ Yeni'yi**seçin.
1.  **İşlem'i**seçin ve ardından dağıtmak istediğiniz işletim sistemi türünü seçin. Örneğin, Windows Server 2012 R2, SUSE Linux Enterprise Server 12 (SLES 12), Red Hat Enterprise Linux 7.2 (RHEL 7.2) veya Oracle Linux 7.2. Varsayılan liste görünümü desteklenen tüm işletim sistemlerini göstermez. Tam liste için **tümünü** gör'üni seçin. SAP yazılım dağıtımı için desteklenen işletim sistemleri hakkında daha fazla bilgi için SAP Note [1928533'e]bakın.
1.  Bir sonraki sayfada, hüküm ve koşulları gözden geçirin.
1.  Dağıtım **modeli seç** kutusunda **Kaynak Yöneticisi'ni**seçin.
1.  **Oluştur'u**seçin.

Sihirbaz, ağ arabirimleri ve depolama hesapları gibi gerekli tüm kaynaklara ek olarak sanal makineyi oluşturmak için gerekli parametreleri ayarlamada size yol sunar. Bu parametrelerden bazıları şunlardır:

1. **Temel Bilgiler**:
   * **Adı**: Kaynağın adı (sanal makine adı).
   * **VM disk türü**: İşletim sistemi diskinin disk türünü seçin. Veri diskleriniz için Premium Depolama'yı kullanmak istiyorsanız, işletim sistemi diski için de Premium Depolama'yı kullanmanızı öneririz.
   * **Kullanıcı adı ve parola** veya **SSH ortak anahtarı**: Sağlama sırasında oluşturulan kullanıcının kullanıcı adını ve parolasını girin. Bir Linux sanal makinesi için, makinede oturum açmanız için kullandığınız genel Secure Shell (SSH) anahtarını girebilirsiniz.
   * **Abonelik**: Yeni sanal makineyi sağlamak için kullanmak istediğiniz aboneliği seçin.
   * **Kaynak grubu**: VM için kaynak grubunun adı. Yeni bir kaynak grubunun adını veya zaten var olan bir kaynak grubunun adını girebilirsiniz.
   * **Konum**: Yeni sanal makinenin dağıtılanın. Sanal makineyi şirket içi ağınıza bağlamak istiyorsanız, Azure'u şirket içi ağınıza bağlayan sanal ağın konumunu seçtiğinizden emin olun. Daha fazla bilgi [için, SAP NetWeaver için Azure Sanal Makineler planlaması ve uygulamasında][planning-guide]Microsoft Azure [ağı'na][planning-guide-microsoft-azure-networking] bakın.
1. **Boyut**:

     Desteklenen VM türlerinin listesi için SAP Note [1928533'e]bakın. Azure Premium Depolama'yı kullanmak istiyorsanız doğru VM türünü seçtiğinizden emin olun. Tüm VM türleri Premium Depolamayı desteklemez. Daha fazla bilgi için [bkz: Depolama: Microsoft Azure Depolama ve veri diskleri][planning-guide-storage-microsoft-azure-storage-and-data-disks] ve Azure Premium [Depolama, SAP NetWeaver için Azure Sanal Makineler planlaması ve][planning-guide] [uygulamasında.][planning-guide-azure-premium-storage]

1. **Ayarlar**:
   * **Depolama**
     * **Disk Türü**: İşletim sistemi diskinin disk türünü seçin. Veri diskleriniz için Premium Depolama'yı kullanmak istiyorsanız, işletim sistemi diski için de Premium Depolama'yı kullanmanızı öneririz.
     * **Yönetilen diskleri kullanın**: Yönetilen Diskler kullanmak istiyorsanız Evet'i seçin. Yönetilen Diskler hakkında daha fazla bilgi için planlama kılavuzundaki [Yönetilen Diskler][planning-guide-managed-disks] bölümüne bakın.
     * **Depolama hesabı**: Varolan bir depolama hesabı seçin veya yeni bir depo oluşturun. Tüm depolama türleri SAP uygulamalarını çalıştırmak için çalışmaz. Depolama türleri hakkında daha fazla bilgi için [RDBMS Dağıtımları için bir VM'nin Depolama yapısına](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general#65fa79d6-a85f-47ee-890b-22e794f51a64)bakın.
   * **Ağ**
     * **Sanal ağ** ve **Subnet**: Sanal makineyi intranetinizle entegre etmek için şirket içi ağınıza bağlı sanal ağı seçin.
     * **Genel IP adresi**: Kullanmak istediğiniz genel IP adresini seçin veya yeni bir genel IP adresi oluşturmak için parametreleri girin. Sanal makinenize Internet üzerinden erişmek için herkese açık bir IP adresi kullanabilirsiniz. Sanal makinenize güvenli erişime yardımcı olmak için bir ağ güvenlik grubu oluşturduğunuzdan da emin olun.
     * **Ağ güvenlik grubu**: Daha fazla bilgi için [ağ güvenlik gruplarıyla ağ trafiğini denetleme'ye][virtual-networks-nsg]bakın.
   * **Uzantılar**: Sanal makine uzantılarını dağıtıma ekleyerek yükleyebilirsiniz. Bu adımda uzantıları eklemeniz gerekmez. SAP desteği için gereken uzantılar daha sonra yüklenir. Bu kılavuzda [SAP için Azure Uzantısını Yapılandır'a][deployment-guide-4.5] bakın.
   * **Yüksek Kullanılabilirlik**: Bir kullanılabilirlik kümesi seçin veya yeni bir kullanılabilirlik kümesi oluşturmak için parametreleri girin. Daha fazla bilgi için [Azure kullanılabilirlik kümeleri'ne][planning-guide-3.2.3]bakın.
   * **İzleme**
     * **Önyükleme tanılama**: Önyükleme tanılama için **Devre Dışı at'ı** seçebilirsiniz.
     * **Konuk İşletim Sistemi tanılama**: Tanılamayı izlemek için **Devre Dışı At'ı** seçebilirsiniz.

1. **Özet**:

   Seçimlerinizi gözden geçirin ve ardından **Tamam'ı**seçin.

Sanal makineniz seçtiğiniz kaynak grubunda dağıtılır.

#### <a name="create-a-virtual-machine-by-using-a-template"></a>Şablon kullanarak sanal makine oluşturma

[Azure-quickstart-templates GitHub deposunda][azure-quickstart-templates-github]yayınlanan SAP şablonlarından birini kullanarak sanal bir makine oluşturabilirsiniz. Ayrıca [Azure portalı][virtual-machines-windows-tutorial], [PowerShell][virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]veya [Azure CLI'yi][virtual-machines-linux-tutorial]kullanarak sanal bir makine de oluşturabilirsiniz.

* [**İki katmanlı yapılandırma (yalnızca bir sanal makine) şablonu** (sap-2 katmanlı-pazar-görüntü)][sap-templates-2-tier-marketplace-image]

  Yalnızca bir sanal makine kullanarak iki katmanlı bir sistem oluşturmak için bu şablonu kullanın.
* [**İki katmanlı yapılandırma (yalnızca bir sanal makine) şablonu - Yönetilen Diskler** (sap-2 katmanlı-marketplace-image-md)][sap-templates-2-tier-marketplace-image-md]

  Yalnızca bir sanal makine ve Yönetilen Diskler kullanarak iki katmanlı bir sistem oluşturmak için bu şablonu kullanın.
* [**Üç katmanlı yapılandırma (birden çok sanal makine) şablonu** (sap-3 katmanlı-pazar-görüntü)][sap-templates-3-tier-marketplace-image]

  Birden çok sanal makine kullanarak üç katmanlı bir sistem oluşturmak için bu şablonu kullanın.
* [**Üç katmanlı yapılandırma (birden çok sanal makine) şablonu - Yönetilen Diskler** (sap-3 katmanlı-marketplace-image-md)][sap-templates-3-tier-marketplace-image-md]

  Birden çok sanal makine ve Yönetilen Diskler kullanarak üç katmanlı bir sistem oluşturmak için bu şablonu kullanın.

Azure portalına şablon için aşağıdaki parametreleri girin:

1. **Temel Bilgiler**:
   * **Abonelik**: Şablonu dağıtmak için kullanılacak abonelik.
   * **Kaynak grubu**: Şablonu dağıtmak için kullanılacak kaynak grubudur. Yeni bir kaynak grubu oluşturabilir veya abonelikte varolan bir kaynak grubu seçebilirsiniz.
   * **Konum**: Şablonun dağıtılanın. Varolan bir kaynak grubu seçtiyseniz, bu kaynak grubunun konumu kullanılır.

1. **Ayarlar**:
   * **SAP Sistem Kimliği**: SAP Sistem Kimliği (SID).
   * **İşletim sistemi türü**: Dağıtmak istediğiniz işletim sistemi, örneğin, Windows Server 2012 R2, SUSE Linux Enterprise Server 12 (SLES 12), Red Hat Enterprise Linux 7.2 (RHEL 7.2) veya Oracle Linux 7.2.

     Liste görünümü desteklenen tüm işletim sistemlerini göstermez. SAP yazılım dağıtımı için desteklenen işletim sistemleri hakkında daha fazla bilgi için SAP Note [1928533'e]bakın.
   * **SAP sistem boyutu**: SAP sisteminin boyutu.

     Yeni sistemin sağladığı SAPS sayısı. Sistemin kaç SAPS gerektirdiğinden emin değilseniz, SAP Teknoloji Ortağınıza veya Sistem Entegratörünüze danışın.
   * **Sistem kullanılabilirliği** (yalnızca üç katmanlı şablon): Sistem kullanılabilirliği.

     Yüksek kullanılabilirlik yüklemesi için uygun bir yapılandırma için **HA'yı** seçin. ABAP SAP Central Services (ASCS) için iki veritabanı sunucusu ve iki sunucu oluşturulur.
   * **Depolama türü** (yalnızca iki katmanlı şablon): Kullanılacak depolama türü.

     Daha büyük sistemler için Azure Premium Depolama'yı kullanmanızı şiddetle öneririz. Depolama türleri hakkında daha fazla bilgi için şu kaynaklara bakın:
      * [SAP DBMS Örneği için Azure Premium SSD Depolama Kullanımı][2367194]
      * [RDBMS Dağıtımları için VM'nin depolama yapısı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general#65fa79d6-a85f-47ee-890b-22e794f51a64)
      * [Premium Depolama: Azure Sanal Makine iş yükleri için yüksek performanslı depolama][storage-premium-storage-preview-portal]
      * [Microsoft Azure Depolama'ya Giriş][storage-introduction]
   * **Admin kullanıcı adı** ve **Yönetici şifresi**: Kullanıcı adı ve şifre.
     Sanal makinede oturum açma için yeni bir kullanıcı oluşturulur.
   * **Yeni veya varolan alt ağ**: Yeni bir sanal ağ ve alt ağ oluşturulup oluşturulmadığını veya varolan bir alt ağ kullanılıp kullanılmadığını belirler. Şirket içi ağınıza bağlı bir sanal ağınız **varsa, Varolan'ı**seçin.
   * **Subnet ID**: VM'yi, vm'nin atanması gereken tanımlanmış bir alt ağına yerleştirmek istiyorsanız, o alt ağın kimliğini adlandırın. Kimlik genellikle şu şekilde görünür:&lt;/subscriptions/ subscription&lt;id>/resourceGroups/ kaynak grup adı&lt;>/providers/Microsoft.Network/virtualNetworks/ sanal ağ adı>/subnets/&lt;subnet adı>

1. **Şartlar ve koşullar**:  
    Yasal koşulları gözden geçirin ve kabul edin.

1. **Satın al**'ı seçin.

Azure Marketi'nden bir resim kullandığınızda Azure VM Aracısı varsayılan olarak dağıtılır.

#### <a name="configure-proxy-settings"></a>Ara sunucu ayarlarını yapılandırma

Şirket içi ağınızın nasıl yapılandırıldığına bağlı olarak, VM'nizde proxy ayarlamanız gerekebilir. VM'niz vpn veya ExpressRoute üzerinden şirket içi ağınıza bağlıysa, VM Internet'e erişemeyebilir ve SAP uzantısı aracılığıyla gerekli VM uzantılarını indiremeyebilir veya SAP Ana Bilgisayar aracısı için Azure altyapı bilgilerini toplayamayacaktır Azure için. Daha fazla bilgi için [proxy'yi yapılandır'a][deployment-guide-configure-proxy]bakın.

#### <a name="join-a-domain-windows-only"></a>Bir etki alanına katılma (yalnızca Windows)

Azure dağıtımınız, Azure sitesinden siteye VPN bağlantısı veya ExpressRoute [(SAP NetWeaver için Azure Sanal Makineler planlaması ve uygulamasında][planning-guide]çapraz *tesisler* olarak adlandırılır) aracılığıyla şirket içi Active Directory veya DNS örneğine bağlıysa, VM'nin şirket içi bir etki alanına katılması beklenir. Bu görevle ilgili hususlar hakkında daha fazla bilgi için [bkz.][deployment-guide-4.3]

#### <a name="configure-vm-extension"></a><a name="ec323ac3-1de9-4c3a-b770-4ff701def65b"></a>VM Uzantısını Yapılandırma

SAP'nin ortamınızı desteklediğinden emin olmak için, SAP için [Azure Uzantısını Yapılandır'da][deployment-guide-4.5]açıklandığı şekilde SAP için Azure Uzantısı'nı ayarlayın. SAP için ön koşulları ve [SAP kaynaklarında][deployment-guide-2.2]listelenen kaynaklarda SAP Çekirdeği ve SAP Ana Bilgisayar Aracısı'nın gerekli minimum sürümlerini denetleyin.

#### <a name="vm-extension-for-sap-check"></a>SAP denetimi için VM uzantısı

SAP Ana Bilgisayar Aracısı için [uçtan uca veri toplama için Denetimler ve sorun giderme][deployment-guide-troubleshooting-chapter]de açıklandığı gibi SAP için VM Uzantı'nın çalışıp çalışmadığını denetleyin.

#### <a name="post-deployment-steps"></a>Dağıtım sonrası adımlar

VM'yi oluşturduktan ve VM dağıtıldıktan sonra, gerekli yazılım bileşenlerini VM'ye yüklemeniz gerekir. Bu vm dağıtımı türündeki dağıtım/yazılım yükleme sırası nedeniyle, yüklenecek yazılım Azure'da, başka bir VM'de veya eklenebilir bir disk olarak zaten kullanılabilir olmalıdır. Veya, şirket içi varlıklara (yükleme hisseleri) bağlantının verildiği bir şirket içi senaryo kullanmayı düşünün.

VM'nizi Azure'da dağıttıktan sonra, SAP yazılımını şirket içi ortamda olduğu gibi VM'nize yüklemek için aynı yönergeleri ve araçları uygulayın. SAP yazılımını bir Azure VM'ye yüklemek için, SAP ve Microsoft SAP yükleme ortamını Azure VHD'lere veya Yönetilen Disklere yüklemenizi ve depolamanızı veya gerekli tüm SAP yükleme ortamına sahip bir dosya sunucusu olarak çalışan bir Azure VM oluşturmanızı önerir.

### <a name="scenario-2-deploying-a-vm-with-a-custom-image-for-sap"></a><a name="54a1fc6d-24fd-4feb-9c57-ac588a55dff2"></a>Senaryo 2: SAP için özel bir görüntüile VM dağıtma

Bir işletim sisteminin veya DBMS'nin farklı sürümlerifarklı yama gereksinimlerine sahip olduğundan, Azure Marketi'nde bulduğunuz görüntüler gereksinimlerinizi karşılamayabilir. Bunun yerine, daha sonra yeniden dağıtabileceğiniz kendi işletim sistemi/DBMS VM görüntünüzü kullanarak bir VM oluşturmak isteyebilirsiniz.
Linux için özel bir resim oluşturmak için Windows için bir resim oluşturmaktan farklı adımlar kullanırsınız.

---
> ![Windows][Logo_Windows] Windows
>
> Birden çok sanal makine dağıtmak için kullanabileceğiniz bir Windows görüntüsü hazırlamak için, Windows ayarlarının (Windows SID ve ana bilgisayar adı gibi) şirket içi VM'de soyutlanmış veya genelleştirilmiş olması gerekir. Bunu yapmak için [sysprep](https://msdn.microsoft.com/library/hh825084.aspx) kullanabilirsiniz.
>
> ![Linux][Logo_Linux] Linux
>
> Birden çok sanal makine dağıtmak için kullanabileceğiniz bir Linux görüntüsü hazırlamak için, bazı Linux ayarlarının şirket içi VM'de soyutlanmış veya genelleştirilmiş olması gerekir. Bunu yapmak `waagent -deprovision` için kullanabilirsiniz. Daha fazla bilgi için [Azure'da çalışan bir Linux sanal makinesini ve Azure][virtual-machines-linux-capture-image] [Linux aracısı kullanım kılavuzuna][virtual-machines-linux-agent-user-guide-command-line-options]bakın.
>
>

---
Özel bir görüntü hazırlayıp oluşturabilir ve ardından birden çok yeni VM oluşturmak için kullanabilirsiniz. Bu, [SAP NetWeaver için Azure Sanal Makineler planlama ve uygulamasında][planning-guide]açıklanmıştır. Yeni bir SAP sistemi yüklemek için SAP Yazılım Sağlama Yöneticisi'ni kullanarak (sanal makineye bağlı bir diskten veritabanı yedeklemesini geri yükler) veya DBMS'niz varsa, veritabanı içeriğinizi doğrudan Azure depolamasından geri yükleyerek ayarlayın destekler. Daha fazla bilgi için [SAP NetWeaver için Azure Sanal Makineler DBMS dağıtımına][dbms-guide]bakın. Şirket içi VM'nize (özellikle iki katmanlı sistemler için) zaten bir SAP sistemi yüklediyseniz, SAP Yazılım Sağlama Yöneticisi (SAP Note [1619720)]tarafından desteklenen Sistem Yeniden Adlandırma yordamını kullanarak Azure VM'nin dağıtımından sonra SAP sistem ayarlarını uyarlayabilirsiniz. Aksi takdirde, Azure VM'yi dağıttıktan sonra SAP yazılımını yükleyebilirsiniz.

Aşağıdaki akış şeması, özel bir görüntüden VM dağıtmak için SAP'ye özgü adım sırasını gösterir:

![Özel Pazar Yerinde VM görüntüsü kullanarak SAP sistemleri için VM dağıtımının akış şeması][deployment-guide-figure-300]

#### <a name="create-a-virtual-machine-by-using-the-azure-portal"></a>Azure portalını kullanarak sanal bir makine oluşturma

Yönetilen Disk görüntüsünden yeni bir sanal makine oluşturmanın en kolay yolu Azure portalını kullanmaktır. Disk Görüntüsünü Yönetme hakkında daha fazla bilgi için [Azure'da genelleştirilmiş bir VM'nin yönetilen görüntüsünü yakalama'yı](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource) okuyun

1.  <https://ms.portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Compute%2Fimages> kısmına gidin. Veya Azure portalı menüsünde **Görseller'i**seçin.
1.  Dağıtmak istediğiniz Yönetilen Disk görüntüsünü seçin ve **VM Oluştur'a** tıklayın

Sihirbaz, ağ arabirimleri ve depolama hesapları gibi gerekli tüm kaynaklara ek olarak sanal makineyi oluşturmak için gerekli parametreleri ayarlamada size yol sunar. Bu parametrelerden bazıları şunlardır:

1. **Temel Bilgiler**:
   * **Adı**: Kaynağın adı (sanal makine adı).
   * **VM disk türü**: İşletim sistemi diskinin disk türünü seçin. Veri diskleriniz için Premium Depolama'yı kullanmak istiyorsanız, işletim sistemi diski için de Premium Depolama'yı kullanmanızı öneririz.
   * **Kullanıcı adı ve parola** veya **SSH ortak anahtarı**: Sağlama sırasında oluşturulan kullanıcının kullanıcı adını ve parolasını girin. Bir Linux sanal makinesi için, makinede oturum açmanız için kullandığınız genel Secure Shell (SSH) anahtarını girebilirsiniz.
   * **Abonelik**: Yeni sanal makineyi sağlamak için kullanmak istediğiniz aboneliği seçin.
   * **Kaynak grubu**: VM için kaynak grubunun adı. Yeni bir kaynak grubunun adını veya zaten var olan bir kaynak grubunun adını girebilirsiniz.
   * **Konum**: Yeni sanal makinenin dağıtılanın. Sanal makineyi şirket içi ağınıza bağlamak istiyorsanız, Azure'u şirket içi ağınıza bağlayan sanal ağın konumunu seçtiğinizden emin olun. Daha fazla bilgi [için, SAP NetWeaver için Azure Sanal Makineler planlaması ve uygulamasında][planning-guide]Microsoft Azure [ağı'na][planning-guide-microsoft-azure-networking] bakın.
1. **Boyut**:

     Desteklenen VM türlerinin listesi için SAP Note [1928533'e]bakın. Azure Premium Depolama'yı kullanmak istiyorsanız doğru VM türünü seçtiğinizden emin olun. Tüm VM türleri Premium Depolamayı desteklemez. Daha fazla bilgi için [bkz: Depolama: Microsoft Azure Depolama ve veri diskleri][planning-guide-storage-microsoft-azure-storage-and-data-disks] ve Azure Premium [Depolama, SAP NetWeaver için Azure Sanal Makineler planlaması ve][planning-guide] [uygulamasında.][planning-guide-azure-premium-storage]

1. **Ayarlar**:
   * **Depolama**
     * **Disk Türü**: İşletim sistemi diskinin disk türünü seçin. Veri diskleriniz için Premium Depolama'yı kullanmak istiyorsanız, işletim sistemi diski için de Premium Depolama'yı kullanmanızı öneririz.
     * **Yönetilen diskleri kullanın**: Yönetilen Diskler kullanmak istiyorsanız Evet'i seçin. Yönetilen Diskler hakkında daha fazla bilgi için planlama kılavuzundaki [Yönetilen Diskler][planning-guide-managed-disks] bölümüne bakın.
   * **Ağ**
     * **Sanal ağ** ve **Subnet**: Sanal makineyi intranetinizle entegre etmek için şirket içi ağınıza bağlı sanal ağı seçin.
     * **Genel IP adresi**: Kullanmak istediğiniz genel IP adresini seçin veya yeni bir genel IP adresi oluşturmak için parametreleri girin. Sanal makinenize Internet üzerinden erişmek için herkese açık bir IP adresi kullanabilirsiniz. Sanal makinenize güvenli erişime yardımcı olmak için bir ağ güvenlik grubu oluşturduğunuzdan da emin olun.
     * **Ağ güvenlik grubu**: Daha fazla bilgi için [ağ güvenlik gruplarıyla ağ trafiğini denetleme'ye][virtual-networks-nsg]bakın.
   * **Uzantılar**: Sanal makine uzantılarını dağıtıma ekleyerek yükleyebilirsiniz. Bu adımda uzantı eklemeniz gerekmez. SAP desteği için gereken uzantılar daha sonra yüklenir. Bu kılavuzda [SAP için Azure Uzantısını Yapılandır'a][deployment-guide-4.5] bakın.
   * **Yüksek Kullanılabilirlik**: Bir kullanılabilirlik kümesi seçin veya yeni bir kullanılabilirlik kümesi oluşturmak için parametreleri girin. Daha fazla bilgi için [Azure kullanılabilirlik kümeleri'ne][planning-guide-3.2.3]bakın.
   * **İzleme**
     * **Önyükleme tanılama**: Önyükleme tanılama için **Devre Dışı at'ı** seçebilirsiniz.
     * **Konuk İşletim Sistemi tanılama**: Tanılamayı izlemek için **Devre Dışı At'ı** seçebilirsiniz.

1. **Özet**:

   Seçimlerinizi gözden geçirin ve ardından **Tamam'ı**seçin.

Sanal makineniz seçtiğiniz kaynak grubunda dağıtılır.

#### <a name="create-a-virtual-machine-by-using-a-template"></a>Şablon kullanarak sanal makine oluşturma

Azure portalından özel bir işletim sistemi görüntüsü kullanarak dağıtım oluşturmak için aşağıdaki SAP şablonlarından birini kullanın. Bu şablonlar [azure-quickstart-templates GitHub deposunda][azure-quickstart-templates-github]yayınlanır. PowerShell'i kullanarak sanal bir makine [PowerShell][virtual-machines-upload-image-windows-resource-manager]de oluşturabilirsiniz.

* [**İki katmanlı yapılandırma (yalnızca bir sanal makine) şablonu** (sap-2 katmanlı-kullanıcı-görüntü)][sap-templates-2-tier-user-image]

  Yalnızca bir sanal makine kullanarak iki katmanlı bir sistem oluşturmak için bu şablonu kullanın.
* [**İki katmanlı yapılandırma (yalnızca bir sanal makine) şablonu - Yönetilen Disk Görüntüsü** (sap-2 katmanlı-kullanıcı-görüntü-md)][sap-templates-2-tier-user-image-md]

  Yalnızca bir sanal makine ve yönetilen disk görüntüsü kullanarak iki katmanlı bir sistem oluşturmak için bu şablonu kullanın.
* [**Üç katmanlı yapılandırma (birden çok sanal makine) şablonu** (sap-3 katmanlı-kullanıcı-görüntü)][sap-templates-3-tier-user-image]

  Birden çok sanal makine veya kendi işletim sistemi resminizi kullanarak üç katmanlı bir sistem oluşturmak için bu şablonu kullanın.
* [**Üç katmanlı yapılandırma (birden çok sanal makine) şablonu - Yönetilen Disk Görüntüsü** (sap-3 katmanlı-kullanıcı-görüntü-md)][sap-templates-3-tier-user-image-md]

  Birden çok sanal makine veya kendi işletim sistemi resminizi ve Yönetilen Disk görüntünüzü kullanarak üç katmanlı bir sistem oluşturmak için bu şablonu kullanın.

Azure portalına şablon için aşağıdaki parametreleri girin:

1. **Temel Bilgiler**:
   * **Abonelik**: Şablonu dağıtmak için kullanılacak abonelik.
   * **Kaynak grubu**: Şablonu dağıtmak için kullanılacak kaynak grubudur. Yeni bir kaynak grubu oluşturabilir veya abonelikte varolan bir kaynak grubu seçebilirsiniz.
   * **Konum**: Şablonun dağıtılanın. Varolan bir kaynak grubu seçtiyseniz, bu kaynak grubunun konumu kullanılır.
1. **Ayarlar**:
   * **SAP Sistem Kimliği**: SAP Sistem Kimliği.
   * **İşletim sistemi türü**: Dağıtmak istediğiniz işletim sistemi türü (Windows veya Linux).
   * **SAP sistem boyutu**: SAP sisteminin boyutu.

     Yeni sistemin sağladığı SAPS sayısı. Sistemin kaç SAPS gerektirdiğinden emin değilseniz, SAP Teknoloji Ortağınıza veya Sistem Entegratörünüze danışın.
   * **Sistem kullanılabilirliği** (yalnızca üç katmanlı şablon): Sistem kullanılabilirliği.

     Yüksek kullanılabilirlik yüklemesi için uygun bir yapılandırma için **HA'yı** seçin. İki veritabanı sunucusu ve ASCS için iki sunucu oluşturulur.
   * **Depolama türü** (yalnızca iki katmanlı şablon): Kullanılacak depolama türü.

     Daha büyük sistemler için Azure Premium Depolama'yı kullanmanızı şiddetle öneririz. Depolama türleri hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:
      * [SAP DBMS Örneği için Azure Premium SSD Depolama Kullanımı][2367194]
      * [RDBMS Dağıtımları için VM'nin depolama yapısı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general#65fa79d6-a85f-47ee-890b-22e794f51a64)
      * [Premium Depolama: Azure sanal makine iş yükleri için yüksek performanslı depolama][storage-premium-storage-preview-portal]
      * [Microsoft Azure Depolama'ya Giriş][storage-introduction]
   * **Kullanıcı resmi VHD URI** (yalnızca yönetilmeyen disk görüntüsü şablonu): Örneğin, özel&lt;işletim sistemi görüntü VHD'nin URI'si>.blob.core.windows.net/vhds/userimage.vhd hesap adını https://.
   * **Kullanıcı resmi depolama hesabı** (yalnızca yönetilmeyen disk görüntüsü şablonu): Özel işletim sistemi görüntüsünün &lt;depolandığı depolama&lt;hesabının adı, örneğin, hesap adı>.blob.core.windows.net/vhds/userimage.vhd https:// hesap adı>.
   * **userImageId** (yalnızca yönetilen disk görüntüsü şablonu): Kullanmak istediğiniz Yönetilen Disk görüntüsünün kimliği
   * **Admin kullanıcı adı** ve **Yönetici şifresi**: Kullanıcı adı ve şifre.

     Sanal makinede oturum açma için yeni bir kullanıcı oluşturulur.
   * **Yeni veya varolan alt ağ**: Yeni bir sanal ağ ve alt ağ oluşturulup oluşturulmadığını veya varolan bir alt ağ kullanılıp kullanılmadığını belirler. Şirket içi ağınıza bağlı bir sanal ağınız **varsa, Varolan'ı**seçin.
   * **Subnet ID**: VM'yi, vm'nin atanması gereken tanımlanmış bir alt ağına yerleştirmek istiyorsanız, o alt ağın kimliğini adlandırın. Kimlik genellikle şu şekilde görünür:&lt;/subscriptions/ subscription&lt;id>/resourceGroups/ kaynak grup adı&lt;>/providers/Microsoft.Network/virtualNetworks/ sanal ağ adı>/subnets/&lt;subnet adı>

1. **Şartlar ve koşullar**:  
    Yasal koşulları gözden geçirin ve kabul edin.

1. **Satın al**'ı seçin.

#### <a name="install-the-vm-agent-linux-only"></a>VM Aracısını yükleyin (yalnızca Linux)

Önceki bölümde açıklanan şablonları kullanmak için Linux Aracısı'nın kullanıcı resmine zaten yüklenmiş olması gerekir veya dağıtım başarısız olur. [Azure VM Aracısını İndir, yükle ve etkinleştirme][deployment-guide-4.4]de açıklandığı gibi VM Aracısını kullanıcı resmine indirin ve yükleyin. Şablonları kullanmazsanız, VM Aracısını daha sonra da yükleyebilirsiniz.

#### <a name="join-a-domain-windows-only"></a>Bir etki alanına katılma (yalnızca Windows)

Azure dağıtımınız, Azure sitesinden siteye VPN bağlantısı veya Azure ExpressRoute [(SAP NetWeaver için Azure Sanal Makineler planlaması ve uygulamasında][planning-guide]çapraz *tesisler* olarak adlandırılır) aracılığıyla şirket içi Active Directory veya DNS örneğine bağlıysa, VM'nin şirket içi bir etki alanına katılması beklenir. Bu adımla ilgili hususlar hakkında daha fazla bilgi için [bkz.][deployment-guide-4.3]

#### <a name="configure-proxy-settings"></a>Ara sunucu ayarlarını yapılandırma

Şirket içi ağınızın nasıl yapılandırıldığına bağlı olarak, VM'nizde proxy ayarlamanız gerekebilir. VM'niz VPN veya ExpressRoute üzerinden şirket içi ağınıza bağlıysa, VM Internet'e erişemeyebilir ve gerekli VM uzantılarını indiremeyebilir veya Azure için SAP uzantısı aracılığıyla SAP Ana Bilgisayar aracısı için Azure altyapı bilgilerini toplayamayacak, [bkz.][deployment-guide-configure-proxy]

#### <a name="configure-azure-vm-extension-for-sap"></a>SAP için Azure VM Uzantısını Yapılandırma

SAP'nin ortamınızı desteklediğinden emin olmak için, SAP için [Azure Uzantısını Yapılandır'da][deployment-guide-4.5]açıklandığı şekilde SAP için Azure Uzantısı'nı ayarlayın. SAP için ön koşulları ve [SAP kaynaklarında][deployment-guide-2.2]listelenen kaynaklarda SAP Çekirdeği ve SAP Ana Bilgisayar Aracısı'nın gerekli minimum sürümlerini denetleyin.

#### <a name="sap-vm-extension-check"></a>SAP VM Uzantı denetimi

SAP Ana Bilgisayar Aracısı için [uçtan uca veri toplama için Denetimler ve sorun giderme][deployment-guide-troubleshooting-chapter]de açıklandığı gibi SAP için VM Uzantı'nın çalışıp çalışmadığını denetleyin.


### <a name="scenario-3-moving-an-on-premises-vm-by-using-a-non-generalized-azure-vhd-with-sap"></a><a name="a9a60133-a763-4de8-8986-ac0fa33aa8c1"></a>Senaryo 3: SAP ile genelleştirilmeyen bir Azure VHD kullanarak şirket içi VM'yi taşıma

Bu senaryoda, belirli bir SAP sistemini şirket içi ortamdan Azure'a taşımayı planlıyorsunuz. Bunu, işletim sistemi, SAP ikilileri ve sonunda DBMS ikilileri ve DBMS'nin veri ve günlük dosyalarıyla vhd'leri Azure'a yükleyerek yapabilirsiniz. Senaryo 2'de açıklanan senaryonun [aksine: SAP için özel bir görüntüye sahip bir VM dağıtma,][deployment-guide-3.3]bu durumda, şirket içi ortamda yapılandırıldıkları için ana bilgisayar adını, SAP SID'yi ve SAP kullanıcı hesaplarını Azure VM'de tutarsınız. İşletim sistemi genelleme yapmanız gerekmez. Bu senaryo, SAP ortamının bir kısmının şirket içinde çalıştığı ve bir kısmının Azure'da çalıştığı binalar arası senaryolar için genellikle geçerlidir.

Bu senaryoda, VM Aracısı dağıtım sırasında otomatik olarak **yüklenmez.** SAP için VM Aracısı ve SAP için Azure Uzantısı'nın SAP NetWeaver'ı Azure'da çalıştırması gerektiğinden, sanal makineyi oluşturduktan sonra her iki bileşeni de el ile indirmeniz, yüklemeniz ve etkinleştirmeniz gerekir.

Azure VM Aracısı hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın.

---
> ![Windows][Logo_Windows] Windows
>
> [Azure Sanal Makine Aracısı'na genel bakış][virtual-machines-windows-agent-user-guide]
>
> ![Linux][Logo_Linux] Linux
>
> [Azure Linux Aracısı Kullanım Kılavuzu][virtual-machines-linux-agent-user-guide]
>
>

---

Aşağıdaki akış şeması, genelleştirilmiş olmayan bir Azure VHD kullanarak şirket içi Bir VM'yi taşımak için adım dizisini gösterir:

![VM diski kullanarak SAP sistemleri için VM dağıtımının akış şeması][deployment-guide-figure-400]

Disk azure'da zaten yüklenmiş ve tanımlanmışsa (bkz. [SAP NetWeaver için Azure Sanal Makineler planlaması ve uygulaması),][planning-guide]sonraki birkaç bölümde açıklanan görevleri yapın.

#### <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Azure portalı üzerinden özel bir işletim sistemi diski kullanarak dağıtım oluşturmak için, [Azure hızlı başlangıç şablonları GitHub deposunda][azure-quickstart-templates-github]yayınlanan SAP şablonundan yararlanın. PowerShell'i kullanarak el ile sanal bir makine de oluşturabilirsiniz.

* [**İki katmanlı yapılandırma (yalnızca bir sanal makine) şablonu** (sap-2 katmanlı-kullanıcı-disk)][sap-templates-2-tier-os-disk]

  Yalnızca bir sanal makine kullanarak iki katmanlı bir sistem oluşturmak için bu şablonu kullanın.
* [**İki katmanlı yapılandırma (yalnızca bir sanal makine) şablonu - Yönetilen Disk** (sap-2 katmanlı-kullanıcı-disk-md)][sap-templates-2-tier-os-disk-md]

  Yalnızca bir sanal makine ve yönetilen disk kullanarak iki katmanlı bir sistem oluşturmak için bu şablonu kullanın.

Azure portalına şablon için aşağıdaki parametreleri girin:

1. **Temel Bilgiler**:
   * **Abonelik**: Şablonu dağıtmak için kullanılacak abonelik.
   * **Kaynak grubu**: Şablonu dağıtmak için kullanılacak kaynak grubudur. Yeni bir kaynak grubu oluşturabilir veya abonelikte varolan bir kaynak grubu seçebilirsiniz.
   * **Konum**: Şablonun dağıtılanın. Varolan bir kaynak grubu seçtiyseniz, bu kaynak grubunun konumu kullanılır.
1. **Ayarlar**:
   * **SAP Sistem Kimliği**: SAP Sistem Kimliği.
   * **İşletim sistemi türü**: Dağıtmak istediğiniz işletim sistemi türü (Windows veya Linux).
   * **SAP sistem boyutu**: SAP sisteminin boyutu.

     Yeni sistemin sağladığı SAPS sayısı. Sistemin kaç SAPS gerektirdiğinden emin değilseniz, SAP Teknoloji Ortağınıza veya Sistem Entegratörünüze danışın.
   * **Depolama türü** (yalnızca iki katmanlı şablon): Kullanılacak depolama türü.

     Daha büyük sistemler için Azure Premium Depolama'yı kullanmanızı şiddetle öneririz. Depolama türleri hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:
      * [SAP DBMS Örneği için Azure Premium SSD Depolama Kullanımı][2367194]
      * [RDBMS Dağıtımları için VM'nin depolama yapısı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general#65fa79d6-a85f-47ee-890b-22e794f51a64)
      * [Premium Depolama: Azure Sanal Makine iş yükleri için yüksek performanslı depolama][storage-premium-storage-preview-portal]
      * [Microsoft Azure Depolama'ya Giriş][storage-introduction]
   * **OS disk VHD URI** (yalnızca yönetilmeyen disk şablonu): Özel işletim&lt;sistemi diskinin URI'si, örneğin, https:// hesap adı>.blob.core.windows.net/vhds/osdisk.vhd.
   * **Os disk Yönetilen Disk Kimliği** (yalnızca yönetilen disk şablonu): Yönetilen Disk Işletim Sistemi diskinin Kimliği, /subscriptions/92d102f7-81a5-4df7-9877-54987ba97dd9/resourceGroups/group/providers/Microsoft.Compute/disks/WIN
   * **Yeni veya varolan alt ağ**: Yeni bir sanal ağ ve alt ağ oluşturulup oluşturulmadığını veya varolan bir alt ağ kullanılıp kullanılmadığını belirler. Şirket içi ağınıza bağlı bir sanal ağınız **varsa, Varolan'ı**seçin.
   * **Subnet ID**: VM'yi, vm'nin atanması gereken tanımlanmış bir alt ağına yerleştirmek istiyorsanız, o alt ağın kimliğini adlandırın. Kimlik genellikle şu şekilde görünür:&lt;/subscriptions/ subscription&lt;id>/resourceGroups/ kaynak grup adı&lt;>/providers/Microsoft.Network/virtualNetworks/ sanal ağ adı>/subnets/&lt;subnet adı>

1. **Şartlar ve koşullar**:  
    Yasal koşulları gözden geçirin ve kabul edin.

1. **Satın al**'ı seçin.

#### <a name="install-the-vm-agent"></a>VM Aracısını Yükleyin

Önceki bölümde açıklanan şablonları kullanmak için, VM Aracısı işletim sistemi diskine yüklenmesi gerekir veya dağıtım başarısız olur. [Azure VM Aracısını İndir, yükle ve etkinleştirme'de][deployment-guide-4.4]açıklandığı gibi VM Aracısını VM'de indirin ve yükleyin.

Önceki bölümde açıklanan şablonları kullanmazsanız, vm aracısını daha sonra da yükleyebilirsiniz.

#### <a name="join-a-domain-windows-only"></a>Bir etki alanına katılma (yalnızca Windows)

Azure dağıtımınız, Azure sitesinden siteye VPN bağlantısı veya ExpressRoute [(SAP NetWeaver için Azure Sanal Makineler planlaması ve uygulamasında][planning-guide]çapraz *tesisler* olarak adlandırılır) aracılığıyla şirket içi Active Directory veya DNS örneğine bağlıysa, VM'nin şirket içi bir etki alanına katılması beklenir. Bu görevle ilgili hususlar hakkında daha fazla bilgi için [bkz.][deployment-guide-4.3]

#### <a name="configure-proxy-settings"></a>Ara sunucu ayarlarını yapılandırma

Şirket içi ağınızın nasıl yapılandırıldığına bağlı olarak, VM'nizde proxy ayarlamanız gerekebilir. VM'niz VPN veya ExpressRoute üzerinden şirket içi ağınıza bağlıysa, VM Internet'e erişemeyebilir ve gerekli VM uzantılarını indiremeyebilir veya Azure için SAP uzantısı aracılığıyla SAP Ana Bilgisayar aracısı için Azure altyapı bilgilerini toplayamayacak, [bkz.][deployment-guide-configure-proxy]

#### <a name="configure-azure-vm-extension-for-sap"></a>SAP için Azure VM Uzantısını Yapılandırma

SAP'nin ortamınızı desteklediğinden emin olmak için, SAP için [Azure Uzantısını Yapılandır'da][deployment-guide-4.5]açıklandığı şekilde SAP için Azure Uzantısı'nı ayarlayın. SAP için ön koşulları ve [SAP kaynaklarında][deployment-guide-2.2]listelenen kaynaklarda SAP Çekirdeği ve SAP Ana Bilgisayar Aracısı'nın gerekli minimum sürümlerini denetleyin.

#### <a name="sap-vm-check"></a>SAP VM denetimi

SAP Ana Bilgisayar Aracısı için [uçtan uca veri toplama için Denetimler ve sorun giderme][deployment-guide-troubleshooting-chapter]de açıklandığı gibi SAP için VM uzantısının çalışıp çalışmadığını denetleyin.

## <a name="update-the-configuration-of-azure-extension-for-sap"></a>SAP için Azure Uzantısı yapılandırmasını güncelleştirme

Aşağıdaki senaryolardan herhangi birinde SAP için Azure Uzantısı yapılandırmasını güncelleştirme:
* Ortak Microsoft/SAP ekibi VM uzantısının yeteneklerini genişletir ve daha fazla veya daha az sayaç ister.
* Microsoft, verileri teslim eden temel Azure altyapısının yeni bir sürümünü tanıtıyor ve SAP için Azure Uzantısı'nın bu değişikliklere uyarlanması gerekiyor.
* Azure VM'nize ek veri diskleri bağlarsınız veya bir veri diskini kaldırırsınız. Bu senaryoda, depolama yla ilgili verilerin toplanmasını güncelleştirin. Son noktaları ekleyerek veya silerek veya BIR VM'ye IP adresleri atayarak yapılandırmanızı değiştirme, uzantı yapılandırmasını etkilemez.
* Azure VM'nizin boyutunu( örneğin, A5 boyutundan diğer VM boyutuna) değiştirirsiniz.
* Azure VM'nize yeni ağ arabirimleri eklersiniz.

Ayarları güncelleştirmek için, SAP için [Azure Uzantısını Yapılandır'daki][deployment-guide-4.5]adımları izleyerek SAP için Azure Uzantısı yapılandırmasını güncelleştirin.

## <a name="detailed-tasks-for-sap-software-deployment"></a>SAP yazılım dağıtımı için ayrıntılı görevler

Bu bölümde, yapılandırma ve dağıtım işleminde belirli görevleri yapmak için ayrıntılı adımlar vardır.

### <a name="deploy-azure-powershell-cmdlets"></a><a name="604bcec2-8b6e-48d2-a944-61b0f5dee2f7"></a>Azure PowerShell cmdlets dağıtma

1. Microsoft [Azure İndirme'ye](https://azure.microsoft.com/downloads/)gidin.
1. **PowerShell**altında Komut **PowerShell**satırı araçları **altında, Windows install'ı**seçin.
1. Microsoft Download Manager iletişim kutusunda, indirilen dosya için (örneğin, WindowsAzurePowershellGet.3f.3f.3fnew.exe), **Çalıştır'ı**seçin.
1. Microsoft Web Platform Installer'ı (Microsoft Web PI) çalıştırmak için **Evet'i**seçin.
1. Şuna benzer bir sayfa görüntülenir:

   ![Azure PowerShell cmdlets için yükleme sayfası][deployment-guide-figure-500]<a name="figure-5"></a>

1. **Yükle'yi**seçin ve ardından Microsoft Yazılım Lisans Koşullarını kabul edin.
1. PowerShell yüklendi. Yükleme sihirbazını kapatmak için **Bitir'i** seçin.

Genellikle aylık olarak güncelleştirilen PowerShell cmdlets güncellemeleri için sık sık kontrol edin. Güncelleştirmeleri denetlemenin en kolay yolu, adım 5'te gösterilen yükleme sayfasına kadar önceki yükleme adımlarını yapmaktır. Cmdletlerin çıkış tarihi ve çıkış numarası, adım 5'te gösterilen sayfada yer almaktadır. SAP Note [1928533] veya SAP Note [2015553'te]aksi belirtilmedikçe, Azure PowerShell cmdlets'in en son sürümüyle çalışmanızı öneririz.

Bilgisayarınızda yüklü Azure PowerShell cmdlets sürümünü kontrol etmek için şu PowerShell komutunu çalıştırın:

```powershell
(Get-Module Az.Compute).Version
```

Sonuç şu na benzer:

![Azure PowerShell cmdlet sürüm denetiminin sonucu][deployment-guide-figure-600]
<a name="figure-6"></a>

Bilgisayarınızda yüklü Azure cmdlet sürümü geçerli sürümse, yükleme sihirbazının ilk sayfası ürün başlığına **ekleyerek (Yüklü)** olarak gösterir (aşağıdaki ekran görüntüsüne bakın). PowerShell Azure cmdlets'iniz günceldir. Yükleme sihirbazını kapatmak için **Çık'ı**seçin.

![Azure PowerShell cmdlets'in en son sürümünün yüklenmiş olduğunu belirten Azure PowerShell cmdlets için yükleme sayfası][deployment-guide-figure-700]
<a name="figure-7"></a>

### <a name="deploy-azure-cli"></a><a name="1ded9453-1330-442a-86ea-e0fd8ae8cab3"></a>Azure CLI'yi dağıtma

1. Microsoft [Azure İndirme'ye](https://azure.microsoft.com/downloads/)gidin.
1. **Komut satırı araçları**altında , Azure komut satırı **arabirimi**altında, işletim sisteminiz için **Install** bağlantısını seçin.
1. Microsoft Download Manager iletişim kutusunda, indirilen dosya için (örneğin, WindowsAzureXPlatCLI.3f.3f.3fnew.exe), **Çalıştır'ı**seçin.
1. Microsoft Web Platform Installer'ı (Microsoft Web PI) çalıştırmak için **Evet'i**seçin.
1. Şuna benzer bir sayfa görüntülenir:

   ![Azure PowerShell cmdlets için yükleme sayfası][deployment-guide-figure-500]<a name="figure-5"></a>

1. **Yükle'yi**seçin ve ardından Microsoft Yazılım Lisans Koşullarını kabul edin.
1. Azure CLI yüklendi. Yükleme sihirbazını kapatmak için **Bitir'i** seçin.

Genellikle aylık olarak güncelleştirilen Azure CLI güncelleştirmelerini sık sık denetleyin. Güncelleştirmeleri denetlemenin en kolay yolu, adım 5'te gösterilen yükleme sayfasına kadar önceki yükleme adımlarını yapmaktır.

Bilgisayarınızda yüklü olan Azure CLI sürümünü denetlemek için şu komutu çalıştırın:

```console
azure --version
```

Sonuç şu na benzer:

![Azure CLI sürüm denetiminin sonucu][deployment-guide-figure-760]
<a name="0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda"></a>

### <a name="join-a-vm-to-an-on-premises-domain-windows-only"></a><a name="31d9ecd6-b136-4c73-b61e-da4a29bbc9cc"></a>Şirket içi etki alanına Bir VM'ye katılma (yalnızca Windows)

SAP VM'leri, şirket içi Active Directory ve DNS'nin Azure'da genişletildiği bir şirket içi senaryoda dağıtırsanız, VM'lerin şirket içi bir etki alanına katılması beklenir. Bir VM'e şirket içi etki alanına katılmak için attığınız ayrıntılı adımlar ve şirket içi bir etki alanının üyesi olmak için gereken ek yazılımlar müşteriye göre değişir. Genellikle, bir VM'ye şirket içi etki alanına katılmak için kötü amaçlı yazılımdan koruma yazılımı ve yedekleme veya izleme yazılımı gibi ek yazılımlar yüklemeniz gerekir.

Bu senaryoda, bir VM ortamınızdaki bir etki alanına katıldığında Internet proxy ayarları zorlanırsa, Konuk VM'deki Windows Yerel Sistem Hesabı'nın (S-1-5-18) aynı proxy ayarlarına sahip olduğundan da emin olmanız gerekir. En kolay seçenek, etki alanındaki sistemler için geçerli olan bir etki alanı Grup İlkesi kullanarak proxy'yi zorlamaktır.

### <a name="download-install-and-enable-the-azure-vm-agent"></a><a name="c7cbb0dc-52a4-49db-8e03-83e7edc2927d"></a>Azure VM Aracısını indirin, yükleyin ve etkinleştirin

Genelleştirilmeyen bir işletim sistemi görüntüsünden dağıtılan sanal makineler için (örneğin, Windows Sistem Hazırlama veya sysprep, araçtan kaynaklanmayan bir görüntü), Azure VM Aracı'nı el ile indirmeniz, yüklemeniz ve etkinleştirmeniz gerekir.

Azure Marketi'nden bir VM dağıtırsanız, bu adım gerekli değildir. Azure Marketi'ndeki görüntülerde zaten Azure VM Aracısı vardır.

#### <a name="windows"></a><a name="b2db5c9a-a076-42c6-9835-16945868e866"></a>Windows

1. Azure VM Aracısını İndirin:
   1.  Azure [VM Aracısı yükleyici paketini](https://go.microsoft.com/fwlink/?LinkId=394789)indirin.
   1.  VM Agent MSI paketini yerel olarak kişisel bir bilgisayarda veya sunucuda saklayın.
1. Azure VM Aracısını yükleyin:
   1.  Dağıtılan Azure VM'ye Uzak Masaüstü Protokolü (RDP) kullanarak bağlanın.
   1.  VM'de bir Windows Explorer penceresi açın ve VM Aracısı'nın MSI dosyasının hedef dizinini seçin.
   1.  Azure VM Aracısı Yükleyici MSI dosyasını yerel bilgisayarınızdan/sunucunuzdan VM'deki VM Aracısı'nın hedef dizinine sürükleyin.
   1.  VM'deki MSI dosyasına çift tıklayın.
1. Şirket içi etki alanlarıyla birleşen VM'ler için, proxy'yi [Yapılandır'da][deployment-guide-configure-proxy]açıklandığı gibi, nihai Internet proxy ayarlarının VM'deki Windows Yerel Sistem hesabına (S-1-5-18) de uygulandığından emin olun. VM Aracısı bu bağlamda çalışır ve Azure'a bağlanabilmesi gerekir.

Azure VM Aracısını güncelleştirmek için kullanıcı etkileşimi gerekmez. VM Aracısı otomatik olarak güncelleştirilir ve VM yeniden başlatma gerektirmez.

#### <a name="linux"></a><a name="6889ff12-eaaf-4f3c-97e1-7c9edc7f7542"></a>Linux

Linux için VM Aracısını yüklemek için aşağıdaki komutları kullanın:

* **SUSE Linux Kurumsal Sunucu (SLES)**

  ```console
  sudo zypper install WALinuxAgent
  ```

* **Red Hat Enterprise Linux (RHEL) veya Oracle Linux**

  ```console
  sudo yum install WALinuxAgent
  ```

Aracı zaten yüklüyse, Azure Linux Aracısını güncellemek için, [Bir VM'de Azure Linux Aracısını Güncelleştir'de açıklanan adımları GitHub'dan en son sürüme][virtual-machines-linux-update-agent]yapın.

### <a name="configure-the-proxy"></a><a name="baccae00-6f79-4307-ade4-40292ce4e02d"></a>Proxy'yi yapılandırma

Windows'da proxy yapılandırmak için attığınız adımlar, Linux'ta proxy yapılandırma şekilde farklıdır.

#### <a name="windows"></a>Windows

Yerel Sistem hesabının Internet'e erişmesi için proxy ayarlarının doğru şekilde ayarlanabilmesi gerekir. Proxy ayarlarınız Grup İlkesi tarafından ayarlanmazsa, Yerel Sistem hesabının ayarlarını yapılandırabilirsiniz.

1. **Başlat'a**git, **gpedit.msc'yi**girin ve sonra **Enter'u**seçin.
1. **Bilgisayar Yapılandırma** > **Yönetim Şablonları** > **Windows Bileşenleri** > **Internet Explorer'ı**seçin. Ayar **Makine başına proxy ayarlarını (kullanıcı başına değil)** devre dışı bırakıldığından veya yapılandırılmadığını zedelediğinden emin olun.
1. **Denetim Masası'nda,** **Ağ ve Paylaşım Merkezi** > **İnternet Seçenekleri'ne**gidin.
1. **Bağlantılar** sekmesinde **LAN ayarları** düğmesini seçin.
1. **Ayarları otomatik olarak algıla** onay kutusunu temizleyin.
1. LAN onay **kutunuz için proxy sunucusu kullanın'ı** seçin ve ardından proxy adresini ve bağlantı noktasını girin.
1. **Gelişmiş** düğmesini seçin.
1. Özel **Durumlar** kutusuna IP adresini **168.63.129.16**girin. **Tamam'ı**seçin.

#### <a name="linux"></a>Linux

Microsoft Azure Konuk Aracısı'nın vb. waagent.conf adresinde \\bulunan\\yapılandırma dosyasında doğru proxy'yi yapılandırın.

Aşağıdaki parametreleri ayarlayın:

1. **HTTP proxy ana bilgisayar**. Örneğin, **proxy.corp.local**olarak ayarlayın.

   ```console
   HttpProxy.Host=<proxy host>

   ```

1. **HTTP proxy bağlantı noktası**. Örneğin, **80**olarak ayarlayın.

   ```console
   HttpProxy.Port=<port of the proxy host>

   ```

1. Aracıyı yeniden başlatın.

   ```console
   sudo service waagent restart
   ```

\\Vb\\waagent.conf proxy ayarları da gerekli VM uzantıları için geçerlidir. Azure depolarını kullanmak istiyorsanız, bu depolara giden trafiğin şirket içi intranetinizden geçmediğinden emin olun. Zorunlu tünel oluşturmayı etkinleştirmek için kullanıcı tanımlı rotalar oluşturduysanız, siteden siteye VPN bağlantınız üzerinden değil, depolara trafiği yönlendiren bir rota eklediğinizden emin olun.

* **SLES**

  Ayrıca etc\\regionserverclnt.cfg \\listelenen IP adresleri için yollar eklemeniz gerekir. Aşağıdaki şekil bir örnek gösterir:

  ![Zorlamalı tünel oluşturma][deployment-guide-figure-50]


* **RHEL**

  Ayrıca \\etc\\yum.repos.d\\rhui-load-balancers listelenen ana bilgisayarların IP adresleri için yollar eklemeniz gerekir. Örneğin, önceki şemaya bakın.

* **Oracle Linux**

  Azure'da Oracle Linux için depo yok. Oracle Linux için kendi depolarınızı yapılandırmanız veya genel depoları kullanmanız gerekir.

Kullanıcı tanımlı rotalar hakkında daha fazla bilgi [için, Kullanıcı tanımlı rotalar ve IP yönlendirme][virtual-networks-udr-overview]bakın.

### <a name="configure-the-azure-extension-for-sap"></a><a name="d98edcd3-f2a1-49f7-b26a-07448ceb60ca"></a>SAP için Azure Uzantısını Yapılandırma

[Azure'da SAP için VM'lerin Dağıtım senaryolarında][deployment-guide-3]açıklandığı gibi VM'yi hazırladığınızda, Azure VM Aracısı sanal makineye yüklenir. Bir sonraki adım, küresel Azure veri merkezlerinde Azure Uzantısı Deposu'nda bulunan SAP için Azure Uzantısı'nı dağıtmaktır. Daha fazla bilgi [için, SAP NetWeaver için Azure Sanal Makineler planlaması ve uygulamasına][planning-guide-9.1]bakın.

PowerShell veya Azure CLI'yi, SAP için Azure Uzantısı'nı yüklemek ve yapılandırmak için kullanabilirsiniz. Uzantıyı bir Windows makinesi ni kullanarak Windows veya Linux VM'ye yüklemek için [Azure PowerShell'e][deployment-guide-4.5.1]bakın. Uzantıyı Linux masaüstü kullanarak Bir Linux VM'ye yüklemek için Bkz. [Azure CLI.][deployment-guide-4.5.2]

#### <a name="azure-powershell-for-linux-and-windows-vms"></a><a name="987cf279-d713-4b4c-8143-6b11589bb9d4"></a>Linux ve Windows VM'leri için Azure PowerShell

PowerShell'i kullanarak SAP için Azure Uzantısı'nı yüklemek için:

1. Azure PowerShell cmdlet'in en son sürümünü yüklediğinizden emin olun. Daha fazla bilgi için bkz: [Azure PowerShell cmdlets dağıtma.][deployment-guide-4.1]  
1. Aşağıdaki PowerShell cmdlet’ini çalıştırın.
    Kullanılabilir ortamların listesi için `commandlet Get-AzEnvironment`çalıştırın. Global Azure'u kullanmak istiyorsanız, ortamınız **AzureCloud'dur.** Çin'deki Azure için **AzureChinaCloud'u**seçin.

    ```powershell
    $env = Get-AzEnvironment -Name <name of the environment>
    Connect-AzAccount -Environment $env
    Set-AzContext -SubscriptionName <subscription name>

    Set-AzVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
    ```

Hesap verilerinizi girdikten ve Azure sanal makinesini tanımladıktan sonra, komut dosyası gerekli uzantıları dağır ve gerekli özellikleri etkinleştirir. Bu işlem birkaç dakika sürebilir.
Hakkında daha `Set-AzVMAEMExtension`fazla bilgi için [Set-AzVMAEMExtension][msdn-set-Azvmaemextension]'ye bakın.

![SAP'ye özel Azure cmdlet Set-AzVMAEMExtension'in başarılı bir şekilde yürütülmesi][deployment-guide-figure-900]

Yapılandırma, `Set-AzVMAEMExtension` SAP için ana bilgisayar veri toplamayapılandırmak için tüm adımları yapar.

Komut dosyası çıktısı aşağıdaki bilgileri içerir:

* Os diski ve tüm ek veri diskleri için veri toplamanın yapılandırıldıklarının teyidi.
* Sonraki iki ileti, belirli bir depolama hesabı için Depolama Ölçümleri yapılandırmasını onaylar.
* Bir çıktı satırı SAP yapılandırması için VM Uzantısı'nın gerçek güncelleştirmedurumunu verir.
* Başka bir çıktı satırı yapılandırmanın dağıtıldığını veya güncelleştirildiğini doğrular.
* Çıktının son satırı bilgilendirmedir. SAP yapılandırması için VM Uzantısını test etme seçeneklerinizi gösterir.
* SAP yapılandırması için Azure VM Uzantısı'nın tüm adımlarının başarıyla yürütülüp yürütülmediğini ve Azure Altyapısının gerekli verileri sağladığını denetlemek için, SAP için Azure Uzantısı için [Hazırlık denetimine][deployment-guide-5.1]devam edin.
* Azure Tanılama'nın ilgili verileri toplaması için 15-30 dakika bekleyin.

#### <a name="azure-cli-for-linux-vms"></a><a name="408f3779-f422-4413-82f8-c57a23b4fc2f"></a>Linux VM'leri için Azure CLI

Azure CLI'yi kullanarak SAP için Azure Uzantısı'nı yüklemek için:

   1. Azure klasik [CLI'yi yükleyin.][azure-cli]
   1. Azure hesabınızla oturum açın:

      ```console
      azure login
      ```

   1. Azure Kaynak Yöneticisi moduna geç:

      ```console
      azure config mode arm
      ```

   1. SAP için Azure Uzantısını etkinleştir:

      ```console
      azure vm enable-aem <resource-group-name> <vm-name>
      ```

1. Azure CLI 2.0 kullanarak yükleme

   1. [Azure CLI][azure-cli-2]2.0'ı yükleyin.
   1. Azure hesabınızla oturum açın:

      ```azurecli
      az login
      ```

   1. Azure CLI AEM Uzantısını Yükleme
  
      ```azurecli
      az extension add --name aem
      ```
  
   1. Uzantıyı
  
      ```azurecli
      az vm aem set -g <resource-group-name> -n <vm name>
      ```

1. SAP için Azure Uzantısı'nın Azure Linux VM'de etkin olduğunu doğrulayın. Dosya \\nın var\\\\lib AzureEnhancedMonitor\\PerfCounters olup olmadığını kontrol edin. Varsa, komut istemiyle, SAP için Azure Uzantısı tarafından toplanan bilgileri görüntülemek için bu komutu çalıştırın:

   ```console
   cat /var/lib/AzureEnhancedMonitor/PerfCounters
   ```

   Çıktı şuna benzer:

   ```output
   ...
   2;cpu;Current Hw Frequency;;0;2194.659;MHz;60;1444036656;saplnxmon;
   2;cpu;Max Hw Frequency;;0;2194.659;MHz;0;1444036656;saplnxmon;
   ...
   ```

## <a name="checks-and-troubleshooting-for-end-to-end-data-collection-for-sap-host-agent"></a><a name="564adb4f-5c95-4041-9616-6635e83a810b"></a>SAP Ana Bilgisayar Aracısı için uçuça veri toplama için denetimler ve sorun giderme

Azure VM'nizi dağıttıktan ve SAP için ilgili Azure Uzantısını ayarladıktan sonra, uzantının tüm bileşenlerinin beklendiği gibi çalışıp çalışmadığını kontrol edin.

SAP için Azure Uzantısı için Hazırlık denetiminde açıklandığı gibi SAP için Azure Uzantısı için hazırlık [denetimini][deployment-guide-5.1]çalıştırın. Tüm hazırlık denetimi sonuçları pozitifse ve ilgili tüm performans sayaçları iyi görünüyorsa, SAP için Azure Uzantısı başarıyla ayarlanmışolur. [SAP][deployment-guide-2.2]Kaynaklarında SAP Notları'nda açıklandığı gibi SAP Ana Bilgisayar Aracısı yüklemeye devam edebilirsiniz. Hazır denetim sayaçların eksik olduğunu gösteriyorsa, SAP yapılandırması için Azure Uzantısı için Sistem Durumu denetiminde açıklandığı gibi SAP için Azure Uzantısı için sistem durumu [denetimini çalıştırın.][deployment-guide-5.2] Daha fazla sorun giderme seçeneği [için SAP için Sorun Giderme Azure Uzantısı'na][deployment-guide-5.3]bakın.

### <a name="readiness-check-for-the-azure-extension-for-sap"></a><a name="bb61ce92-8c5c-461f-8c53-39f5e5ed91f2"></a>SAP için Azure Uzantısı için hazırlık denetimi

Bu denetim, SAP uygulamanızda görünen tüm performans ölçümlerinin SAP için temel Azure Uzantısı tarafından sağlandıklarından emin olur.

#### <a name="run-the-readiness-check-on-a-windows-vm"></a>Windows VM'de hazırlık denetimini çalıştırma

1. Azure sanal makinesinde oturum açın (yönetici hesabı kullanmak gerekli değildir).
1. Bir komut istemi penceresi açın.
1. Komut isteminde, dizini SAP için Azure Uzantısı yükleme klasörüne\\\\değiştirin:\\C: Paketler Eklentileri Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\&lt;sürümü>\\damla

   Uzantı yolundaki *sürüm* değişebilir. Yükleme klasöründe uzantının birden çok sürümü için klasörler görüyorsanız, AzureEnhancedMonitoring Windows hizmetinin yapılandırmasını kontrol edin ve ardından *yürütülebilire Giden Yol*olarak belirtilen klasöre geçin.

   ![SAP için Azure Uzantısı'nı çalıştıran hizmetin özellikleri][deployment-guide-figure-1000]

1. Komut istemi, herhangi bir parametre olmadan **azperflib.exe** çalıştırın.

   > [!NOTE]
   > Azperflib.exe bir döngü içinde çalışır ve toplanan sayaçları her 60 saniyede bir günceller. Döngüyü sona erdirmek için Komut İstemi penceresini kapatın.
   >
   >

SAP için Azure Uzantısı yüklenmediyse veya AzureEnhancedMonitoring hizmeti çalışmıyorsa, uzantı doğru şekilde yapılandırılmamıştır. Uzantın nasıl dağıtılanın gerektiği hakkında ayrıntılı bilgi için [SAP için Azure Uzantısı sorun giderme][deployment-guide-5.3]'ye bakın.

> [!NOTE]
> Azperflib.exe kendi amaçları için kullanılamaz bir bileşenidir. Yalnızca SAP Ana Bilgisayar Aracısı için VM ile ilgili Azure altyapı verilerini sunan bir bileşendir.
> 

##### <a name="check-the-output-of-azperflibexe"></a>azperflib.exe çıktısını kontrol edin

Azperflib.exe çıktısı SAP için doldurulan tüm Azure performans sayaçlarını gösterir. Toplanan sayaçlar listesinin en altında, özet ve sistem durumu göstergesi SAP için Azure Uzantısı'nın durumunu gösterir.

![Herhangi bir sorun olduğunu gösterir azperflib.exe, yürütülerek sağlık kontrolü çıktısı][deployment-guide-figure-1100]
<a name="figure-11"></a>

Boş olarak bildirilen **Sayaçlar toplam** çıktısı ve önceki şekilde gösterilen **Sağlık durumu**için döndürülen sonucu denetleyin.

Elde edilen değerleri aşağıdaki gibi yorumlayın:

| Azperflib.exe sonuç değerleri | SAP sistem durumu için Azure Uzantısı |
| --- | --- |
| **API Aramaları - kullanılamıyor** | Kullanılamayan sayaçlar sanal makine yapılandırması için geçerli olmayabilir veya hatalardır. Bkz. **Sağlık durumu.** |
| **Sayaçlar toplam - boş** |Aşağıdaki iki Azure depolama sayacı boş olabilir: <ul><li>Depolama Oku Op Latency Server msec</li><li>Depolama Okuma Op Latency E2E msec</li></ul>Diğer tüm sayaçların değerleri olmalıdır. |
| **Sağlık durumu** |İade durumu **Tamam'ı**gösteriyorsa yalnızca Tamam. |
| **Tanılama** |Sağlık durumu hakkında detaylı bilgi. |

Sistem **Durumu durumu** değeri **tamam**değilse, [SAP yapılandırması için Azure Uzantısı için Sistem Durumu denetimindeki][deployment-guide-5.2]yönergeleri izleyin.

#### <a name="run-the-readiness-check-on-a-linux-vm"></a>Linux VM'de hazırlık denetimini çalıştırın

1. SSH kullanarak Azure Sanal Makinesi'ne bağlanın.

1. SAP için Azure Uzantısı çıktısını kontrol edin.

   a.  `more /var/lib/AzureEnhancedMonitor/PerfCounters` öğesini çalıştırın

   **Beklenen sonuç**: Performans sayaçlarının listesini verir. Dosya boş olmamalıdır.

   b. `cat /var/lib/AzureEnhancedMonitor/PerfCounters | grep Error` öğesini çalıştırın

   **Beklenen sonuç**: Hatanın **yok**olduğu bir satır verir , örneğin, **3;config; Hata;;0;0; yok;0;1456416792;tst-servercs;**

   c. `more /var/lib/AzureEnhancedMonitor/LatestErrorRecord` öğesini çalıştırın

   **Beklenen sonuç**: Boş olarak döndürür veya yok.

Önceki denetim başarılı olmadıysa, şu ek denetimleri çalıştırın:

1. Waagent yüklü ve etkin olduğundan emin olun.

   a.  `sudo ls -al /var/lib/waagent/` öğesini çalıştırın

     **Beklenen sonuç**: Waagent dizininin içeriğini listeler.

   b.  `ps -ax | grep waagent` öğesini çalıştırın

   **Beklenen sonuç**: Benzer bir girişi görüntüler:`python /usr/sbin/waagent -daemon`

1. SAP için Azure Uzantısı'nın yüklü ve çalışır durumda olduğundan emin olun.

   a.  `sudo sh -c 'ls -al /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-*/'` öğesini çalıştırın

   **Beklenen sonuç**: SAP dizini için Azure Uzantısı'nın içeriğini listeler.

   b. `ps -ax | grep AzureEnhanced` öğesini çalıştırın

   **Beklenen sonuç**: Benzer bir girişi görüntüler:`python /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-2.0.0.2/handler.py daemon`

1. SAP Note [1031096'da]açıklandığı gibi SAP Ana `saposcol`Bilgisayar Aracısını yükleyin ve çıkışını kontrol edin.

   a.  `/usr/sap/hostctrl/exe/saposcol -d` öğesini çalıştırın

   b.  `dump ccm` öğesini çalıştırın

   c.  **Virtualization_Configuration\Gelişmiş İzleme Erişimi** ölçümünün **doğru**olup olmadığını kontrol edin.

Zaten bir SAP NetWeaver ABAP uygulama sunucunuz yüklüyse, işlem ST06'yı açın ve gelişmiş izlemenin etkin olup olmadığını kontrol edin.

Bu denetimlerden herhangi biri başarısız olursa ve uzantının yeniden nasıl dağıtılanacı hakkında ayrıntılı bilgi [için, SAP için Azure Uzantısı sorun giderme bölümüne][deployment-guide-5.3]bakın.

### <a name="health-check-for-the-azure-extension-for-sap-configuration"></a><a name="e2d592ff-b4ea-4a53-a91a-e5521edb6cd1"></a>SAP yapılandırması için Azure Uzantısı için sistem durumu denetimi

Bazı altyapı verileri SAP için Azure Uzantısı için Hazırlık [denetiminde][deployment-guide-5.1]açıklanan testte belirtildiği gibi `Test-AzVMAEMExtension` doğru teslim edilmezse, Azure altyapısının ve SAP için Azure Uzantısı'nın doğru yapılandırılıp yapılandırılmadığını kontrol etmek için cmdlet'i çalıştırın.

1. [Azure PowerShell cmdlet'in dağıtımında][deployment-guide-4.1]açıklandığı gibi Azure PowerShell cmdlet'in en son sürümünü yüklediğinizden emin olun.
1. Aşağıdaki PowerShell cmdlet’ini çalıştırın. Kullanılabilir ortamların listesi için cmdlet'i `Get-AzEnvironment`çalıştırın. Genel Azure'u kullanmak için **AzureCloud** ortamını seçin. Çin'deki Azure için **AzureChinaCloud'u**seçin.

   ```powershell
   $env = Get-AzEnvironment -Name <name of the environment>
   Connect-AzAccount -Environment $env
   Set-AzContext -SubscriptionName <subscription name>
   Test-AzVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
   ```

1. Hesap verilerinizi girin ve Azure sanal makinesini tanımlayın.

   ![SAP'ye özgü Azure cmdlet Test-VMConfigForSAP_GUI giriş sayfası][deployment-guide-figure-1200]

1. Komut dosyası seçtiğiniz sanal makinenin yapılandırmasını sınar.

   ![SAP için Azure Uzantısı'nın başarılı testinin çıktısı][deployment-guide-figure-1300]

Her sağlık denetimi sonucunun **tamam**olduğundan emin olun. Bazı denetimler **Tamam**görüntülemiyorsa, [SAP için Azure Uzantısını Yapılandır'da][deployment-guide-4.5]açıklandığı gibi güncelleştirme cmdlet'ini çalıştırın. 15 dakika bekleyin ve SAP [yapılandırması için Azure Uzantısı][deployment-guide-5.1] için Azure Uzantısı ve Sistem [Durumu denetimi][deployment-guide-5.2]için Hazırlık denetiminde açıklanan denetimleri yineleyin. Denetimler hala bazı veya tüm sayaçlarla ilgili bir sorun gösteriyorsa, [SAP için Azure Uzantısı sorun giderme'ye][deployment-guide-5.3]bakın.

> [!Note]
> Yönetilen Standart Azure Diskleri kullandığınız durumlarda bazı uyarılarla karşılaşabilirsiniz. "Tamam" döndüren testler yerine uyarılar görüntülenir. Bu normaldir ve bu disk türü nde tasarlanmıştır. Ayrıca bkz: [SAP için Azure Uzantısı sorun giderme][deployment-guide-5.3]
> 

### <a name="troubleshooting-azure-extension-for-sap"></a><a name="fe25a7da-4e4e-4388-8907-8abc2d33cfd8"></a>SAP için Azure Uzantısı Sorun Giderme

#### <a name="windowslogo_windows-azure-performance-counters-do-not-show-up-at-all"></a>![Windows][Logo_Windows] Azure performans sayaçları hiç görünmüyor

AzureEnhancedMonitoring Windows hizmeti Azure'da performans ölçümleri toplar. Hizmet doğru yüklenmediyse veya VM'nizde çalışmıyorsa, performans ölçümleri toplanabilir.

##### <a name="the-installation-directory-of-the-azure-extension-for-sap-is-empty"></a>SAP için Azure Uzantısı yükleme dizini boş

###### <a name="issue"></a>Sorun

Yükleme\\dizini C:\\Paket\\Eklentileri Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\&lt;sürümü>\\damla boş.

###### <a name="solution"></a>Çözüm

Uzantı yüklü değil. Bunun bir proxy sorunu olup olmadığını belirleyin (daha önce açıklandığı gibi). Makineyi yeniden başlatmanız veya yapılandırma `Set-AzVMAEMExtension` komut dosyasını yeniden çalıştırmanız gerekebilir.

##### <a name="service-for-azure-extension-for-sap-does-not-exist"></a>SAP için Azure Uzantısı hizmeti yok

###### <a name="issue"></a>Sorun

AzureEnhancedMonitoring Windows hizmeti yok.

Azperflib.exe çıkış bir hata atar:

![azperflib.exe'nin yürütülmesi, SAP için Azure Uzantısı'nın hizmetinin çalışmadığını gösterir][deployment-guide-figure-1400]
<a name="figure-14"></a>

###### <a name="solution"></a>Çözüm

Hizmet yoksa, SAP için Azure Uzantısı doğru yüklenmedi. [Azure'da SAP için VM'lerin Dağıtım senaryolarında][deployment-guide-3]dağıtım senaryonuz için açıklanan adımları kullanarak uzantıyı yeniden dağıtın.

Uzantıyı dağıttıktan sonra, bir saat sonra Azure performans sayaçlarının Azure VM'de sağlanıp sağlanmadığını yeniden kontrol edin.

##### <a name="service-for-azure-extension-for-sap-exists-but-fails-to-start"></a>SAP için Azure Uzantısı hizmeti var, ancak başlatılamaması

###### <a name="issue"></a>Sorun

AzureEnhancedMonitoring Windows hizmeti vardır ve etkindir, ancak başlatılmaz. Daha fazla bilgi için uygulama olay günlüğünü kontrol edin.

###### <a name="solution"></a>Çözüm

Yapılandırma yanlış. SAP için [Azure Uzantısını Yapılandır'da][deployment-guide-4.5]açıklandığı gibi, VM'de SAP için Azure Uzantısı'nı yeniden başlatın.

#### <a name="windowslogo_windows-some-azure-performance-counters-are-missing"></a>![Windows][Logo_Windows] Bazı Azure performans sayaçları eksik

AzureEnhancedMonitoring Windows hizmeti Azure'da performans ölçümleri toplar. Hizmet çeşitli kaynaklardan veri alır. Bazı yapılandırma verileri yerel olarak toplanır ve bazı performans ölçümleri Azure Tanılama'dan okunur. Depolama sayaçları, depolama abonelik düzeyinde günlük alanınızdan kullanılır.

SAP Note [1999351] kullanarak sorun giderme sorunu çözmezse, `Set-AzVMAEMExtension` yapılandırma komut dosyası yeniden çalıştırın. Depolama analitiği veya tanılama sayaçları etkinleştirildikten hemen sonra oluşturulamayabileceğinden bir saat beklemeniz gerekebilir. Sorun devam ederse, Windows için BC-OP-NT-AZR bileşeninde veya Linux sanal makinesi için BC-OP-LNX-AZR bileşeninde bir SAP müşteri destek iletisi açın.

#### <a name="linuxlogo_linux-azure-performance-counters-do-not-show-up-at-all"></a>![Linux][Logo_Linux] Azure performans sayaçları hiç görünmüyor

Azure'daki performans ölçümleri bir daemon tarafından toplanır. Daemon çalışmıyorsa, performans ölçümleri toplanabilir.

##### <a name="the-installation-directory-of-the-azure-extension-for-sap-is-empty"></a>SAP için Azure Uzantısı yükleme dizini boş

###### <a name="issue"></a>Sorun

Dizin \\var\\\\lib\\ waagent SAP için Azure Uzantısı için bir alt dizini yok.

###### <a name="solution"></a>Çözüm

Uzantı yüklü değil. Bunun bir proxy sorunu olup olmadığını belirleyin (daha önce açıklandığı gibi). Makineyi yeniden başlatmanız ve/veya yapılandırma `Set-AzVMAEMExtension` komut dosyasını yeniden çalıştırmanız gerekebilir.

##### <a name="the-execution-of-set-azvmaemextension-and-test-azvmaemextension-show-warning-messages-stating-that-standard-managed-disks-are-not-supported"></a>Set-AzVMAEMExtension ve Test-AzVMAEMExtension'in yürütülmesi, Standart Yönetilen Disklerin desteklenmediğini belirten uyarı iletilerini gösterir

###### <a name="issue"></a>Sorun

Set-AzVMAEMExtension veya Test-AzVMAEMExtension iletileri yürütüldüğünde bunlar gösterilir:

<pre><code>
WARNING: [WARN] Standard Managed Disks are not supported. Extension will be installed but no disk metrics will be available.
WARNING: [WARN] Standard Managed Disks are not supported. Extension will be installed but no disk metrics will be available.
WARNING: [WARN] Standard Managed Disks are not supported. Extension will be installed but no disk metrics will be available.
</code></pre>

Daha önce açıklandığı gibi azperfli.exe yürütme sağlıklı olmayan bir durum gösteren bir sonuç alabilirsiniz. 

###### <a name="solution"></a>Çözüm

İletiler, Standart Azure Depolama Hesapları istatistiklerini denetlemek için SAP Uzantısı tarafından SAP Uzantısı tarafından kullanılan API'leri teslim etmemiş olmasından kaynaklanır. Bu bir endişe meselesi değil. Standart Disk Depolama hesapları için veri toplama nın nedeni, sık sık oluşan G/Ç'lerin azaltmasıydı. Yönetilen diskler, depolama hesabındaki disk sayısını sınırlayarak bu tür azaltmaları önler. Bu nedenle, bu tür verilere sahip olmamak kritik değildir.


#### <a name="linuxlogo_linux-some-azure-performance-counters-are-missing"></a>![Linux][Logo_Linux] Bazı Azure performans sayaçları eksik

Azure'daki performans ölçümleri, çeşitli kaynaklardan veri alan bir daemon tarafından toplanır. Bazı yapılandırma verileri yerel olarak toplanır ve bazı performans ölçümleri Azure Tanılama'dan okunur. Depolama sayaçları, depolama aboneliğinizdeki günlüklerden gelir.

Bilinen sorunların eksiksiz ve güncel bir listesi için, SAP için Azure Uzantısı için ek sorun giderme bilgilerine sahip SAP Note [1999351'e]bakın.

SAP Note [1999351'i] kullanarak sorun giderme sorunu `Set-AzVMAEMExtension` çözmezse, [SAP için Azure Uzantısını Yapılandır'da][deployment-guide-4.5]açıklandığı gibi yapılandırma komut dosyasını yeniden çalıştırın. Depolama analitiği veya tanılama sayaçları etkinleştirildikten hemen sonra oluşturulamayabileceğinden, bir saat beklemeniz gerekebilir. Sorun devam ederse, Windows için BC-OP-NT-AZR bileşeninde veya Linux sanal makinesi için BC-OP-LNX-AZR bileşeninde bir SAP müşteri destek iletisi açın.
