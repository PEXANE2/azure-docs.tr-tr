---
title: Azure Site Kurtarma ile SAP NetWeaver olağanüstü durum kurtarma yı ayarlama
description: Azure Site Kurtarma ile SAP NetWeaver için olağanüstü durum kurtarmayı nasıl ayarlayamaize hazırlanın.
author: sideeksh
manager: rochakm
ms.topic: how-to
ms.date: 11/27/2018
ms.openlocfilehash: 29acd1b00d23e4f1c2f241027dadbbb406e5e049
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77190789"
---
# <a name="set-up-disaster-recovery-for-a-multi-tier-sap-netweaver-app-deployment"></a>Çok katmanlı SAP NetWeaver uygulaması dağıtımı için olağanüstü durum kurtarma yı ayarlama

Büyük ve orta ölçekli SAP dağıtımlarının çoğu bir tür olağanüstü durum kurtarma çözümü kullanır. Sap gibi uygulamalara daha fazla temel iş süreci taşındıkça sağlam ve test edilebilir olağanüstü durum kurtarma çözümlerinin önemi artmıştır. Azure Site Kurtarma test edilmiş ve SAP uygulamaları ile tümleşik. Site Kurtarma, şirket içi felaket kurtarma çözümlerinin çoğunun yeteneklerini ve rakip çözümlere göre daha düşük toplam sahip olma maliyetini aşar.

Site Kurtarma ile şunları yapabilirsiniz:
* Bileşenleri Azure'a kopyalayarak şirket içinde çalışan SAP NetWeaver ve NetWeaver dışı üretim uygulamalarının korunmasını etkinleştirin.
* Bileşenleri başka bir Azure veri merkezine kopyalayarak Azure'da çalışan SAP NetWeaver ve NetWeaver olmayan üretim uygulamalarının korunmasını etkinleştirin.
* SAP dağıtımınızı Azure'a geçirmek için Site Recovery'yi kullanarak bulut geçişini basitleştirin.
* SAP uygulamalarını test etmek için isteğe bağlı bir üretim klonu oluşturarak SAP proje yükseltmelerini, testini ve prototipoluşturmayı basitleştirin.

[Azure Site Kurtarma'yı](site-recovery-overview.md)kullanarak SAP NetWeaver uygulama dağıtımlarını koruyabilirsiniz. Bu makale, Site Kurtarma'yı kullanarak başka bir Azure veri merkezine çoğaltırken Azure'da üç katmanlı SAP NetWeaver dağıtımını korumak için en iyi uygulamaları kapsar. Makalede desteklenen senaryolar ve yapılandırmalar ve test failovers (olağanüstü durum kurtarma matkaplar) ve gerçek failovers nasıl yapılır açıklanır.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, aşağıdaki görevleri nasıl yapacağınızı bildiğinizden emin olun:

* [Sanal bir makineyi Azure'a çoğaltma](azure-to-azure-walkthrough-enable-replication.md)
* [Kurtarma ağı tasarlama](site-recovery-azure-to-azure-networking-guidance.md)
* [Azure'da bir test başarısızmı yapma](azure-to-azure-walkthrough-test-failover.md)
* [Azure'da başarısız olun](site-recovery-failover.md)
* [Etki alanı denetleyicisi çoğaltma](site-recovery-active-directory.md)
* [SQL Server örneğini çoğaltma](site-recovery-sql.md)

## <a name="supported-scenarios"></a>Desteklenen senaryolar

Aşağıdaki senaryolarda bir olağanüstü durum kurtarma çözümü uygulamak için Site Kurtarma'yı kullanabilirsiniz:
* Bir Azure veri merkezinde çalışan SAP sistemleriniz vardır ve bunları başka bir Azure veri merkezine (Azure'dan Azure'a olağanüstü durum kurtarma) çoğaltırsınız. 
   Daha fazla bilgi için [Azure'dan Azure'a çoğaltma mimarisine](https://aka.ms/asr-a2a-architecture)bakın.
* Şirket içinde VMware (veya fiziksel) sunucularda çalışan SAP sistemleriniz vardır. Ayrıca, SAP sistemlerini bir Azure veri merkezinde (VMware-to-Azure olağanüstü durum kurtarma) bir olağanüstü durum kurtarma sitesine kopyalayabiliyorsunuz. 
   Bu senaryo bazı ek bileşenler gerektirir. Daha fazla bilgi için [VMware-to-Azure çoğaltma mimarisine](https://aka.ms/asr-v2a-architecture)bakın.
* Hyper-V'de çalışan SAP sistemleriniz var. Ayrıca, SAP sistemlerini bir Azure veri merkezinde (Hyper-V-to-Azure olağanüstü durum kurtarma) bir olağanüstü durum kurtarma sitesine kopyalayabiliyorsunuz.
   Bu senaryo bazı ek bileşenler gerektirir. Daha fazla bilgi için Bkz. [Hyper-V-to-Azure çoğaltma mimarisi.](https://aka.ms/asr-h2a-architecture)

Bu makalede, **Azure'dan Azure'a** olağanüstü durum kurtarma senaryosu kullanıyoruz. Senaryo, Site Kurtarma'nın SAP olağanüstü durum kurtarma yeteneklerini gösterir. Site Kurtarma çoğaltma uygulamaya özgü olmadığından, açıklanan işlemin diğer senaryolar için de geçerli olması beklenir.

### <a name="required-foundation-services"></a>Gerekli temel hizmetleri
Bu makalede tartıştığımız senaryoda, aşağıdaki temel hizmetleri dağıtılır:
* Azure ExpressRoute veya Azure VPN Ağ Geçidi
* Azure'da çalışan en az bir Azure Active Directory etki alanı denetleyicisi ve DNS sunucusu

Site Kurtarma'yı dağıtmadan önce bu altyapıyı kurmanızı öneririz.

## <a name="reference-sap-application-deployment"></a>Başvuru SAP uygulama dağıtımı

Bu başvuru mimarisi, YÜKSEK kullanılabilirlik ile Azure'da bir Windows ortamında SAP NetWeaver çalıştırıyor. Bu mimari, kuruluşunuzun gereksinimlerini karşılamak üzere değiştirebileceğiniz belirli sanal makine (VM) boyutlarıyla dağıtılır.

![Tipik bir SAP dağıtım deseni diyagramı](./media/site-recovery-sap/sap-netweaver_latest.png)

## <a name="disaster-recovery-considerations"></a>Olağanüstü durum kurtarmayla konusunda dikkat edilmesi gerekenler

Olağanüstü durum kurtarma için, ikincil bir bölgeye geçebilmelisin. Her katman, olağanüstü durum kurtarma koruması sağlamak için farklı bir strateji kullanır.

#### <a name="vms-running-sap-web-dispatcher-pools"></a>SAP Web Dispatcher havuzlarını çalıştıran VM'ler

Web Dispatcher bileşeni SAP uygulama sunucuları arasında SAP trafiği için bir yük dengeleyici olarak çalışır. Azure Yük Bakiyesi, Web Dispatcher bileşeni için yüksek kullanılabilirlik sağlamak için paralel Web Gönderici kurulumlarını uygular. Web Dispatcher, bakiyeciler havuzunda bulunan Web Göndericileri arasında HTTP(S) trafik dağıtımı için bir round-robin yapılandırması kullanır.

#### <a name="vms-running-application-servers-pools"></a>Uygulama sunucuları havuzlarını çalıştıran VM'ler
SMLG işlemi ABAP uygulama sunucuları için giriş gruplarını yönetir. SAPGUI'ler ve RFC trafiği için SAP uygulama sunucusu havuzları arasında iş yükünü dağıtmak için Merkezi Hizmetler'in ileti sunucusundaki yük dengeleme işlevini kullanır. Site Kurtarma'yı kullanarak bu yönetimi çoğaltabilirsiniz.

#### <a name="vms-running-sap-central-services-clusters"></a>SAP Merkezi Hizmetler kümelerini çalıştıran VM'ler
Bu başvuru mimarisi, uygulama katmanındaki VM'lerde Merkezi Hizmetleri çalıştırAr. Merkezi Hizmetler, tek bir VM'de yken olası bir tek bir hata noktasıdır. Tipik dağıtım ve yüksek kullanılabilirlik gereksinimleri değildir.

Yüksek kullanılabilirlik çözümlerini uygulamak için paylaşılan bir disk kümesi ni veya dosya paylaşım kümesini kullanabilirsiniz. Paylaşılan bir disk kümesi için VM'leri yapılandırmak için Windows Server Failover Cluster'ı kullanın. Bulut tanığını çoğunluk tanığı olarak kullanmanızı öneririz.

 > [!NOTE]
 > Site Kurtarma bulut tanığını çoğaltmadığından, bulut tanığını olağanüstü durum kurtarma bölgesine dağıtmanızı öneririz.

[SIOS DataKeeper Cluster Edition,](https://azuremarketplace.microsoft.com/marketplace/apps/sios_datakeeper.sios-datakeeper-8) küme üzerinde başarısız küme ortamını desteklemek için küme paylaşılan birim işlevini yapar. İşlevde, SIOS DataKeeper Cluster küme düğümlerine ait bağımsız diskleri çoğaltır. Azure paylaşılan diskleri yerel olarak desteklemediği için, SIOS tarafından sağlanan çözümler gerektirir.

Ayrıca, bir dosya paylaşım kümesi uygulayarak kümeleme işleyebilir. [SAP,](https://blogs.sap.com/2018/03/19/migration-from-a-shared-disk-cluster-to-a-file-share-cluster) /sapmnt global dizinlerine UNC yolu üzerinden erişmek için Merkezi Hizmetler dağıtım modelini yakın zamanda değiştirdi. Yine de /sapmnt UNC payının yüksek oranda kullanılabilir olduğundan emin olmamızı öneririz. Merkezi Hizmetler örneğini kontrol edebilirsiniz. Windows Server 2016'da Dosya Sunucusu (SOFS) ve Depolama Alanları Doğrudan (S2D) özelliğiyle Windows Server Failover Cluster özelliğini kullanın.

 > [!NOTE]
 > Site Kurtarma şu anda depolama alanları doğrudan ve SIOS Datakeeper pasif düğüm kullanan sanal makinelerin yalnızca kilitlenme tutarlı nokta çoğaltma destekler.


## <a name="more-disaster-recovery-considerations"></a>Daha fazla olağanüstü durum kurtarma konuları

Azure bölgelerinde tam SAP dağıtımının başarısız olmasını düzenlemek için Site Kurtarma'yı kullanabilirsiniz.
Olağanüstü durum kurtarma yı ayarlama adımları şunlardır:

1. Sanal makineleri çoğaltma
1. Kurtarma ağı tasarlama
1. Etki alanı denetleyicisi çoğaltma
1. Veri tabanı katmanını çoğaltma
1. Yük devretme testi gerçekleştirin
1. Yük devretme gerçekleştirin

Bu örnekte kullanılan her katmanın olağanüstü durum kurtarma önerisi aşağıdaverilmiştir.

 **SAP katmanları** | **Öneri**
 --- | ---
**SAP Web Dispatcher havuzu** |  Site Kurtarma'yı kullanarak çoğaltma 
**SAP Uygulama sunucu havuzu** |  Site Kurtarma'yı kullanarak çoğaltma 
**SAP Merkezi Hizmetler kümesi** |  Site Kurtarma'yı kullanarak çoğaltma 
**Etkin dizin sanal makineleri** |  Etkin dizin çoğaltma kullanma 
**SQL Veritabanı sunucuları** |  SQL Server Always On çoğaltma'yı kullanın

## <a name="replicate-virtual-machines"></a>Sanal makineleri çoğaltma

Tüm SAP uygulaması sanal makinelerini Azure olağanüstü durum kurtarma veri merkezine çoğaltmaya başlamak [için, sanal makineyi Azure'a Çoğaltma](azure-to-azure-walkthrough-enable-replication.md)kılavuzunu izleyin.

* Active Directory ve DNS'yi koruma kılavuzu için [Active Directory ve DNS'yi nasıl koruyacağınızı](site-recovery-active-directory.md)öğrenin.

* SQL Server'da çalışan veritabanı katmanını koruma kılavuzu için [SQL Server'ı nasıl koruyacağınızı](site-recovery-sql.md)öğrenin.

## <a name="networking-configuration"></a>Ağ yapılandırması

Statik bir IP adresi kullanıyorsanız, sanal makinenin almasını istediğiniz IP adresini belirtebilirsiniz. IP adresini ayarlamak **için, İşlem ve Ağ ayarları** > **Ağ arabirimi kartına**gidin.

![Site Kurtarma Ağı arabirim kartı bölmesinde özel bir IP adresinin nasıl ayarlanacağını gösteren ekran görüntüsü](./media/site-recovery-sap/sap-static-ip.png)


## <a name="create-a-recovery-plan"></a>Kurtarma planı oluşturma

Kurtarma planı, bir hata sırasında çok katmanlı bir uygulamada çeşitli katmanların sırasını destekler. Sıralama, uygulama tutarlılığını korumaya yardımcı olur. Çok katmanlı bir web uygulaması için bir kurtarma planı oluşturduğunuzda, [Site Kurtarma'yı kullanarak kurtarma planı oluştur'da](site-recovery-create-recovery-plans.md)açıklanan adımları tamamlayın.

### <a name="add-virtual-machines-to-failover-groups"></a>Başarısız gruplara sanal makineler ekleme

1. Uygulama sunucusu, web gönderici ve SAP Central hizmetleri VM'lerini ekleyerek bir kurtarma planı oluşturun.
1. VM'leri gruplandırmak için **Özelleştir'i** seçin. Varsayılan olarak, tüm VM'ler Grup 1'in bir parçasıdır.

### <a name="add-scripts-to-the-recovery-plan"></a>Kurtarma planına komut dosyaları ekleme
Uygulamalarınızın düzgün çalışması için Azure sanal makinelerinde bazı işlemler yapmanız gerekebilir. Bu işlemleri başarısız olduktan sonra veya bir test başarısızolduğu sırasında yapın. Ayrıca bazı başarısız lık sonrası işlemleri otomatikleştirebilirsiniz. Örneğin, DNS girişini güncelleştirin ve kurtarma planına karşılık gelen komut dosyalarını ekleyerek bağlamaları ve bağlantıları değiştirin.

Azure'a Dağıt'ı seçerek en çok kullanılan Site Kurtarma komut dosyalarını Azure Otomasyon **hesabınıza**dağıtabilirsiniz. Yayımlanmış herhangi bir komut dosyası kullandığınızda, komut dosyasındaki kılavuzu izleyin.

[![Azure'a Dağıt](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)

1. SQL Server kullanılabilirlik grubu üzerinde başarısız olmak için Grup 1'e bir eylem öncesi komut dosyası ekleyin. Örnek komut dosyalarında yayınlanan ASR-SQL-FailoverAG komut dosyasını kullanın. Komut dosyasındaki kılavuzu izleyin ve komut dosyasında gerekli değişiklikleri uygun şekilde yapın.
1. Web katmanının (Grup 1) başarısız sanal makinelerine bir yük dengeleyicisi eklemek için eylem sonrası komut dosyası ekleyin. Örnek komut dosyalarında yayınlanan ASR-AddSingleLoadBalancer komut dosyasını kullanın. Komut dosyasındaki kılavuzu izleyin ve gerektiğinde komut dosyasında gerekli değişiklikleri yapın.

![SAP Kurtarma Planı](./media/site-recovery-sap/sap_recovery_plan.png)


## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

1. Azure portalında Kurtarma Hizmetleri kasanızı seçin.
1. SAP uygulamaları için oluşturduğunuz kurtarma planını seçin.
1. **Yük Devretme Testi**'ni seçin.
1. Test başarısız lık işlemini başlatmak için kurtarma noktasını ve Azure sanal ağını seçin.
1. İkincil ortam dolduğunda, doğrulamaları gerçekleştirin.
1. Doğrulamalar tamamlandığında, **Temizleme testi failover'ı**seçerek başarısız ortamı temizleyin.

Daha fazla bilgi için [bkz.](site-recovery-test-failover-to-azure.md)

## <a name="run-a-failover"></a>Yük devretme çalıştırma

1. Azure portalında Kurtarma Hizmetleri kasanızı seçin.
1. SAP uygulamaları için oluşturduğunuz kurtarma planını seçin.
1. **Yük devretme**'yi seçin.
1. Başarısız işlemi başlatmak için kurtarma noktasını seçin.

Daha fazla bilgi için Site [Kurtarma'da Failover'a](site-recovery-failover.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
* Site Kurtarma'yı kullanarak SAP NetWeaver dağıtımları için olağanüstü durum kurtarma çözümü oluşturma hakkında daha fazla bilgi edinin. Indirilebilir teknik inceleme [SAP NetWeaver bakınız: Site Kurtarma ile bir Felaket Kurtarma Çözümü Oluşturma.](https://aka.ms/asr_sap) Teknik incelemede çeşitli SAP mimarileri için öneriler açıklanmıştır. Desteklenen uygulamaları ve SAP için VM türlerini Azure'da görebilirsiniz. Olağanüstü durum kurtarma çözümünüzü test etmek için plan seçenekleri de vardır.
* Site Kurtarma'yı kullanarak [diğer iş yüklerini çoğaltma](site-recovery-workload.md) hakkında daha fazla bilgi edinin.
