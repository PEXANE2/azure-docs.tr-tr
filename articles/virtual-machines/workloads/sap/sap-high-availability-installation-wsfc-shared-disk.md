---
title: Azure'daki bir SAP ASCS/SCS örneği için SAP NetWeaver HA'yı Windows failover kümesine ve paylaşılan diske yükleyin | Microsoft Dokümanlar
description: SAP ASCS/SCS örneği için SAP NetWeaver HA'yı Windows failover kümesine ve paylaşılan diske nasıl yükleyeceğimiz öğrenin.
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: rdeltcheva
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 6209bcb3-5b20-4845-aa10-1475c576659f
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: radeltch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e50733c843dfd21e35572f00fc6690e1e84aba97
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79279838"
---
# <a name="install-sap-netweaver-ha-on-a-windows-failover-cluster-and-shared-disk-for-an-sap-ascsscs-instance-in-azure"></a>Azure'daki bir SAP ASCS/SCS örneği için WINDOWS failover kümesine ve paylaşılan diske SAP NetWeaver HA'yı yükleyin

[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2243692]:https://launchpad.support.sap.com/#/notes/2243692

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
[sap-high-availability-installation-wsfc-shared-disk]:sap-high-availability-installation-wsfc-shared-disk.md
[sap-ha-guide-8.9]:high-availability-guide.md#fe0bd8b5-2b43-45e3-8295-80bee5415716
[sap-ha-guide-8.11]:high-availability-guide.md#661035b2-4d0f-4d31-86f8-dc0a50d78158
[sap-hana-ha]:sap-hana-high-availability.md
[sap-suse-ascs-ha]:high-availability-guide-suse.md

[planning-guide]:planning-guide.md
[planning-guide-11]:planning-guide.md
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10

[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md



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

Bu makalede, bir SAP ASCS/SCS örneğini kümelemek için windows server failover kümesi ve küme paylaşılan diski kullanarak Azure'da yüksek kullanılabilirliksap sap sisteminin nasıl yüklenir ve yapılandırılabildiğini açıklar.

## <a name="prerequisites"></a>Ön koşullar

Yüklemeye başlamadan önce şu belgeleri inceleyin:

* [Mimari kılavuzu: Bir küme paylaşılan disk kullanarak Windows failover kümesinde sap ASCS/SCS örneğini kümeleme][sap-high-availability-guide-wsfc-shared-disk]

* [BIR SAP ASCS/SCS örneği için Windows failover kümesi ve paylaşılan disk kullanarak Azure altyapısını SAP HA için hazırlama][sap-high-availability-infrastructure-wsfc-shared-disk]

Kurulumlar kullandığınız DBMS sistemine bağlı olarak değiştiğinden, bu makalede DBMS kurulumlarını açıklamız. DBMS ile ilgili yüksek kullanılabilirlik sorunlarının, farklı DBMS satıcılarının Azure için desteklediği işlevlerle giderildigini varsayıyoruz. Örnekler, SQL Server için AlwaysOn veya veritabanı yansıtmave Oracle veritabanları için Oracle Data Guard'dır. Bu makalede kullandığımız senaryoda, DBMS'ye daha fazla koruma eklemeyiz.

Azure'da kümelenmiş SAP ASCS veya SCS yapılandırmasıyla farklı DBMS hizmetleri etkileşimde bulunduğunda özel bir husus yoktur.

> [!NOTE]
> SAP NetWeaver ABAP sistemlerinin, Java sistemlerinin ve ABAP+Java sistemlerinin kurulum prosedürleri hemen hemen aynıdır. En önemli fark, bir SAP ABAP sisteminin bir ASCS örneğine sahip olmasıdır. SAP Java sisteminde bir SCS örneği vardır. SAP ABAP+Java sisteminde aynı Microsoft failover küme grubunda çalışan bir ASCS örneği ve bir SCS örneği vardır. Her SAP NetWeaver yükleme yığını için herhangi bir yükleme farklılıkları açıkça belirtilmiştir. Diğer tüm parçaların aynı olduğunu varsayabilirsiniz.  
>
>

## <a name="install-sap-with-a-high-availability-ascsscs-instance"></a><a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a>SAP'yi yüksek kullanılabilirlikli BIR ASCS/SCS örneğiyle yükleme

> [!IMPORTANT]
> Sayfa dosyanızı SIOS DataKeeper yansıtılmış ciltlere yerleştirmeyin. DataKeeper yansıtılmış birimleri desteklemez. Sayfa dosyanızı varsayılan olan bir Azure sanal makinenin geçici sürücü D'sinde bırakabilirsiniz. Zaten orada değilse, Windows sayfa dosyasını Azure sanal makinenizin D'sini sürücüsüne taşıyın.
>
>

SAP'yi yüksek kullanılabilirlikli bir ASCS/SCS örneğiyle yüklemek şu görevleri içerir:

* Kümelenmiş SAP ASCS/SCS örneği için sanal bir ana bilgisayar adı oluşturun.
* SAP ilk küme düğüm'üne yükleyin.
* ASCS/SCS örneğinin SAP profilini değiştirin.
* Sonda bağlantı noktası ekleyin.
* Windows güvenlik duvarı sondası bağlantı noktasını açın.

### <a name="create-a-virtual-host-name-for-the-clustered-sap-ascsscs-instance"></a><a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a>Kümelenmiş SAP ASCS/SCS örneği için sanal bir ana bilgisayar adı oluşturma

1. Windows DNS yöneticisinde, ASCS/SCS örneğinin sanal ana bilgisayar adı için bir DNS girişi oluşturun.

   > [!IMPORTANT]
   > ASCS/SCS örneğinin sanal ana bilgisayar adına atadığınız IP adresi, Azure Yük Dengeleyicisi'ne\<(SID\>-lb-ascs) atadığınız IP adresiyle aynı olmalıdır.  
   >
   >

   Sanal SAP ASCS/SCS ana bilgisayar adının IP adresi (pr1-ascs-sap), Azure Yük Bakiyesi'nin (pr1-lb-ascs) IP adresiyle aynıdır.

   ![Şekil 1: SAP ASCS/SCS küme sanal adı ve TCP/IP adresi için DNS girişini tanımlayın][sap-ha-guide-figure-3046]

   _**Şekil 1:** SAP ASCS/SCS küme sanal adı ve TCP/IP adresi için DNS girişini tanımlayın_

2. Sanal ana bilgisayar adına atanan IP adresini tanımlamak için **DNS Yöneticisi** > **Etki Alanı'nı**seçin.

   ![Şekil 2: SAP ASCS/SCS küme yapılandırması için yeni sanal ad ve TCP/IP adresi][sap-ha-guide-figure-3047]

   _**Şekil 2:** SAP ASCS/SCS küme yapılandırması için yeni sanal ad ve TCP/IP adresi_

### <a name="install-the-sap-first-cluster-node"></a><a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a>SAP ilk küme düğüm'e yükleme

1. Küme düğümü A'daki ilk küme düğümü seçeneğini çalıştırın. Örneğin, pr1-ascs-0*host'ta.
2. Azure dahili yük dengeleyicisinin varsayılan bağlantı noktalarını tutmak için şunları seçin:

   * **ABAP sistemi**: **ASCS** örnek numarası **00**
   * **Java sistemi**: **SCS** örnek numarası **01**
   * **ABAP+Java sistemi**: **ASCS** örnek numarası **00** ve **SCS** örnek numarası **01**

   ABAP ASCS örneği için 00 ve Java SCS örneği için 01 dışındaki örnek numaralarını kullanmak için öncelikle Azure dahili yük dengeleyici varsayılan yük dengeleme kurallarını değiştirin. Daha fazla bilgi için bkz: [Azure dahili yük dengeleyicisi için ASCS/SCS varsayılan yük dengeleme kurallarını değiştirin.][sap-ha-guide-8.9]

Sonraki birkaç görev standart SAP yükleme belgelerinde açıklanmaz.

> [!NOTE]
> SAP yükleme belgeleri, ilk ASCS/SCS küme düğümünün nasıl yüklenir olduğunu açıklar.
>
>

### <a name="modify-the-sap-profile-of-the-ascsscs-instance"></a><a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a>ASCS/SCS örneğinin SAP profilini değiştirme

İlk olarak, yeni bir profil parametresi ekleyin. Profil parametresi, SAP iş süreçleri ile enqueue sunucusu arasındaki bağlantıların çok uzun süre boşta kaldığında kapanmasını önler. [SAP ASCS/SCS örneğinin her iki küme düğümünde de kayıt defteri ekleme girdilerinde][sap-ha-guide-8.11]sorun senaryosundan söz ediyoruz. Bu bölümde, bazı temel TCP/IP bağlantı parametrelerinde de iki değişiklik sıyoruz. İkinci adımda, bağlantıların Azure dahili yük `keep_alive` dengeleyicisinin boşta eşiğine çarpmaması için enqueue sunucusunu sinyal gönderecek şekilde ayarlamanız gerekir.

ASCS/SCS örneğinin SAP profilini değiştirmek için:

1. Sap ASCS/SCS örnek profiline bu profil parametresini ekleyin:

   ```
   enque/encni/set_so_keepalive = true
   ```
   Örneğimizde, yol:

   `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_ASCS00_pr1-ascs-sap`

   Örneğin, SAP SCS örnek profiline ve ilgili yola:

   `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_SCS01_pr1-ascs-sap`

2. Değişiklikleri uygulamak için SAP ASCS/SCS örneğini yeniden başlatın.

### <a name="add-a-probe-port"></a><a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a>Sonda bağlantı noktası ekleme

Tüm küme yapılandırmasının Azure Yük Dengeleyicisi ile çalışmasını sağlamak için dahili yük bakiyeleyicisinin sonda işlevini kullanın. Azure dahili yük dengeleyicisi genellikle gelen iş yükünü katılımcı sanal makineler arasında eşit olarak dağıtır.

 Ancak, yalnızca bir örnek etkin olduğundan, bu bazı küme yapılandırmalarında çalışmaz. Diğer örnek pasiftir ve iş yükünün hiçbirini kabul edemez. Sonda işlevi, Azure dahili yük dengeleyicisinin yalnızca etkin bir örneğe çalışma atamasına yardımcı olur. Sonda işlevi yle, dahili yük dengeleyicisi hangi örneklerin etkin olduğunu algılayabilir ve ardından yalnızca iş yüküolan örneği hedefleyebilir.

Sonda bağlantı noktası eklemek için:

1. Aşağıdaki PowerShell komutunu çalıştırarak geçerli **ProbePort** değerini kontrol edin:

   ```powershell
   $SAPSID = "PR1"     # SAP <SID>

   $SAPNetworkIPClusterName = "SAP $SAPSID IP"
   Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter
   ```

   Komutu küme yapılandırmasındaki sanal makinelerden birinden yürütün.

2. Bir sonda bağlantı noktası tanımlayın. Varsayılan sonda bağlantı noktası numarası 0'dır. Örneğimizde, sonda bağlantı noktası 62000 kullanırız.

   ![Şekil 3: Küme yapılandırma sondası bağlantı noktası varsayılan olarak 0'dır][sap-ha-guide-figure-3048]

   _**Şekil 3:** Varsayılan küme yapılandırma sondası bağlantı noktası 0_

   Bağlantı noktası numarası SAP Azure Kaynak Yöneticisi şablonlarında tanımlanır. PowerShell'deki bağlantı noktası numarasını atayabilirsiniz.

   SAP \<SID\> IP küme kaynağı için yeni bir ProbePort değeri ayarlamak için, ortamınız için PowerShell değişkenlerini güncelleştirmek için aşağıdaki PowerShell komut dosyasını çalıştırın:

   ```powershell
   $SAPSID = "PR1"      # SAP <SID>
   $ProbePort = 62000   # ProbePort of the Azure internal load balancer

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

   SAP \<SID\> küme rolünü çevrimiçi duruma getirdikten sonra **ProbePort'un** yeni değere ayarlı olduğundan doğrulayın.

   ```powershell
   $SAPSID = "PR1"     # SAP <SID>

   $SAPNetworkIPClusterName = "SAP $SAPSID IP"
   Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter

   ```
   Komut dosyası çalıştırıldıktan sonra, değişiklikleri etkinleştirmek için SAP küme grubunu yeniden başlatmanız istenir.

   ![Şekil 4: Küme bağlantı noktasını yeni değeri ayarladıktan sonra sondalama][sap-ha-guide-figure-3049]

   _**Şekil 4:** Yeni değeri ayarladıktan sonra küme bağlantı noktasını yokla_

### <a name="open-the-windows-firewall-probe-port"></a><a name="4498c707-86c0-4cde-9c69-058a7ab8c3ac"></a>Windows güvenlik duvarı sondası bağlantı noktasını açma

Her iki küme düğümünde de bir Windows güvenlik duvarı sondası bağlantı noktası açın. Windows güvenlik duvarı sondası bağlantı noktasını açmak için aşağıdaki komut dosyasını kullanın. Ortamınız için PowerShell değişkenlerini güncelleştirin.

  ```powershell
  $ProbePort = 62000   # ProbePort of the Azure internal load balancer

  New-NetFirewallRule -Name AzureProbePort -DisplayName "Rule for Azure Probe Port" -Direction Inbound -Action Allow -Protocol TCP -LocalPort $ProbePort
  ```

**ProbePort** **62000**olarak ayarlanır. Şimdi, dosya paylaşımı \\\ascsha-clsap\sapmnt diğer ana bilgisayarlarından erişebilirsiniz, örneğin ascsha-dbas.

## <a name="install-the-database-instance"></a><a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a>Veritabanı örneğini yükleme

Veritabanı örneğini yüklemek için SAP yükleme belgelerinde açıklanan işlemi izleyin.

## <a name="install-the-second-cluster-node"></a><a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a>İkinci küme düğümini yükleme

İkinci kümeyi yüklemek için SAP yükleme kılavuzunda açıklanan adımları izleyin.

## <a name="change-the-start-type-of-the-sap-ers-windows-service-instance"></a><a name="094bc895-31d4-4471-91cc-1513b64e406a"></a>SAP ERS Windows hizmet örneğinin başlangıç türünü değiştirme

SAP ERS Windows hizmetinin başlangıç türünü her iki küme düğümünde **de Otomatik (Gecikmeli Başlangıç)** olarak değiştirin.

![Şekil 5: SAP ERS örneğinin servis türünü gecikmiş otomatik][sap-ha-guide-figure-3050]

_**Şekil 5:** SAP ERS örneğinin hizmet türünü gecikmiş otomatik olarak değiştirme_

## <a name="install-the-sap-primary-application-server"></a><a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a>SAP Birincil Uygulama Sunucusunu Yükleme

Birincil Uygulama Sunucusu (PAS) \<\>örneği SID -di-0'ı PAS'ı barındırmak üzere belirlediğiniz sanal makineye yükleyin. Azure'da bağımlılık yoktur. DataKeeper'a özgü ayarlar yok.

## <a name="install-the-sap-additional-application-server"></a><a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a>SAP Ek Uygulama Sunucusunu Yükleme

SAP Application Server örneğini barındırmak için belirlediğiniz tüm sanal makinelere bir SAP Ek Uygulama Sunucusu (AAS) yükleyin. Örneğin, SID \<\> \<-di-1'den\>SID&lt;-di- n'ye&gt;kadar.

> [!NOTE]
> Bu, yüksek kullanılabilirlik sap NetWeaver sisteminin kurulumunu tamamlar. Ardından, başarısız test ile devam edin.
>


## <a name="test-the-sap-ascsscs-instance-failover-and-sios-replication"></a><a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a>SAP ASCS/SCS örneği başarısız ve SIOS çoğaltma test
Failover Cluster Manager ve SIOS DataKeeper Management and Configuration aracını kullanarak sap ASCS/SCS örnek failover ve SIOS disk çoğaltmasını test etmek ve izlemek kolaydır.

### <a name="sap-ascsscs-instance-is-running-on-cluster-node-a"></a><a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a>SAP ASCS/SCS örneği küme düğümü A üzerinde çalışıyor

SAP PR1 küme grubu Küme düğümü A üzerinde çalışıyor. Örneğin, pr1-ascs-0 üzerinde. SAP PR1 küme grubunun bir parçası olan paylaşılan disk sürücüsü S'yi A kümesi nesle atayın. ASCS/SCS örneği de disk sürücüsü S kullanır. 

![Şekil 6: Failover Cluster \<Manager: SAP SID\> küme grubu küme düğümü A üzerinde çalışıyor][sap-ha-guide-figure-5000]

_**Şekil 6:** Failover Cluster Manager: \<\> SAP SID küme grubu küme düğümü A üzerinde çalışıyor_

SIOS DataKeeper Yönetimi ve Yapılandırma aracında, paylaşılan disk verilerinin a kümesindeki kaynak birim sürücü S'den b kümesi düğümündeki hedef ses düzeyi Sürücüsü S'ye eşzamanlı olarak çoğaltıldığını görebilirsiniz. Örneğin, pr1-ascs-0 [10.0.0.40] ile pr1-ascs-1 [10.0.0.41] olarak çoğaltılır.

![Şekil 7: SIOS DataKeeper'da, Yerel hacmi A kümesi düğümünden B kümesi düğümüne çoğaltma][sap-ha-guide-figure-5001]

_**Şekil 7:** SIOS DataKeeper'da, yerel birimi A kümesi düğümünden B kümesi düğümüne çoğaltma_

### <a name="failover-from-node-a-to-node-b"></a><a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a>A düğümünden Düğüm B'ye failover

1. SAP \<SID\> küme grubunun küme düğümü A'dan B kümesi düğümüne başarısız olmasını başlatmak için bu seçeneklerden birini seçin:
   - Failover Küme Yöneticisi  
   - Failover Küme PowerShell

   ```powershell
   $SAPSID = "PR1"     # SAP <SID>

   $SAPClusterGroup = "SAP $SAPSID"
   Move-ClusterGroup -Name $SAPClusterGroup

   ```
2. Windows konuk işletim sistemi içinde küme düğümü A'yı yeniden başlatın. Bu, SAP \<SID\> küme grubunun A düğümünden B düğümüne otomatik olarak başarısız olmasını başlatır.  
3. Azure portalından küme düğümü A'yı yeniden başlatın. Bu, SAP \<SID\> küme grubunun A düğümünden B düğümüne otomatik olarak başarısız olmasını başlatır.  
4. Azure PowerShell'i kullanarak küme düğümü A'yı yeniden başlatın. Bu, SAP \<SID\> küme grubunun A düğümünden B düğümüne otomatik olarak başarısız olmasını başlatır.

   Başarısız olduktan sonra, \<\> SAP SID küme grubu B kümesi üzerinde çalışıyor. Örneğin, pr1-ascs-1 üzerinde çalışıyor.

   ![Şekil 8: Failover Cluster Manager'da SAP \<SID\> küme grubu B kümesi üzerinde çalışıyor][sap-ha-guide-figure-5002]

   _**Şekil 8**: Failover Cluster \<Manager'da SAP SID\> küme grubu B kümesi üzerinde çalışıyor_

   Paylaşılan disk şimdi küme düğümü b. SIOS DataKeeper üzerine monte edilir küme düğümü B'deki kaynak hacim sürücüsüS'ten gelen verileri çoğaltarak Küme düğümü A'daki S'yi hedeflemek için kullanılır. Örneğin, pr1-ascs-1 [10.0.0.41] den pr1-ascs-0 [10.0.0.40] çoğalıyor.

   ![Şekil 9: SIOS DataKeeper yerel hacmi b kümesi düğümünden küme düğümüA kopyalar][sap-ha-guide-figure-5003]

   _**Şekil 9:** SIOS DataKeeper yerel hacmi b kümesi düğümünden küme düğümü A'ya kopyalar_
