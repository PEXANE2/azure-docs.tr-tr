---
title: SSS - CloudSimple tarafından Azure VMware Çözümü
description: CloudSimple tarafından Azure VMware Çözümü için sık sorulan sorular
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/15/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 95d8c2974ea372dd59d15a9f2cc31ed171acd932
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77025070"
---
# <a name="frequently-asked-questions-about-vmware-solution-by-cloudsimple"></a>CloudSimple tarafından VMware Çözümü hakkında sık sorulan sorular

## <a name="cloudsimple-service"></a>CloudSimple hizmeti

**CloudSimple tarafından Azure VMware Çözümü nedir?**

CloudSimple tarafından Azure VMware Solution, VMware iş yüklerini birkaç dakika içinde Azure'daki özel bulutları dönüştürür ve genişletir. CloudSimple, şirket içi ve Azure arasındaki iş yüklerini sağlama, altyapıyı yönetme ve düzenleme ile ilgilenir. Uygulamalarınız şirket içinde ve Azure'da tam olarak aynı şekilde çalıştığı için, uygulamalarınızı yeniden architecta etme karmaşıklığı olmadan bulutun esnekliğinden ve hizmetlerinden yararlanırsınız. CloudSimple, isteğe bağlı sağlama, büyüdükçe öde ve kapasite optimizasyonu sağlayan bir bulut tüketimi modeliyle toplam sahip olma maliyetinizi düşürür.  Özellikler, avantajlar ve senaryolar için [CloudSimple tarafından Azure'da VMware Çözümü nedir](cloudsimple-vmware-solutions-overview.md) bkz.

**CloudSimple Özel Bulut nedir?**

CloudSimple Private Cloud, Azure konumlarında Microsoft Azure altyapısında (donanım ve veri merkezi alanı) dağıtılan yüksek performanslı bir bilgi işlem, depolama ve ağ ortamından oluşan özel ve özel bir buluttür.  Özel Bulut, yerel bir VMware 'hizmet olarak platform' sağlar. VMware açısından, her Özel Bulut vCenter Server tam bir örnek içerir. vCenter Server, bir veya daha fazla vSphere Cluster'da bulunan birden çok ESXi düğümlerini ve ilgili Sanal SAN (vSAN) depolama alanını yönetir. CloudSimple hizmeti, Azure aboneliğinizde birden çok Özel Bulut içerebilir.  Daha fazla ayrıntı için [Bkz. Özel Bulut genel bakışı.](cloudsimple-private-cloud.md)

**CloudSimple hizmeti nerede kullanılabilir?**

CloudSimple, Doğu ABD, Batı ABD ve Batı Avrupa bölgelerinde kullanılabilir ve ek bölgeler yakında kullanıma sunulacaktır.

**CloudSimple aboneliğimi nasıl etkinleştirebilirim?**

CloudSimple hizmeti aboneliğinizi [azurevmwaresales@microsoft.com](mailto:azurevmwaresales@microsoft.com) etkinleştirmek için Microsoft hesap temsilcinize başvurabilirsiniz. CloudSimple hizmetinin etkin olmasını istediğiniz e-postada abonelik kimliğinizi sağlayın.  

**CloudSimple portalına nasıl erişebilirim?**

Azure portalından CloudSimple portalına erişebilirsiniz.  Ayrıntılar için Azure [portalından CloudSimple portalıtarafından VMware Çözümüne Eriş'e](access-cloudsimple-portal.md)bakın.

**Özel Bulut'un kapasitesini nasıl artırabilirim?**

Kapasiteyi artırmak için Azure portalından ek düğümler satın alın ve ardından Özel Bulut'unuzu CloudSimple portalından genişletmek için düğümleri kullanın.  Varolan bir vSphere kümesine ek düğümler ekleyebilir veya bunları yeni bir vSphere kümesine ekleyebilirsiniz.  Ayrıntılar için bkz: [CloudSimple Private Cloud'u Genişlet.](expand-private-cloud.md)

**Bakım sırasında Özel Bulut'uma ne olur?**

CloudSimple, zamanlanan bir bakım aralığından birkaç gün önce bildirim sağlar.  Bakım, Özel Bulut'unuzun kullanılabilirliğini sağlamak için kesintisiz bir şekilde yapılır.  Bakım aşağıdaki türde olabilir:

* **CloudSimple altyapı**.  CloudSimple altyapısı son derece kullanılabilir olacak şekilde tasarlanmıştır.  Bu tür bir bakım aralığı sırasında, yedek bileşenler herhangi bir hizmet kesintisini önlemek için teker teker güncelleştirilir. Özel Bulut vCenter'ınıza, tüm sanal makinelerinize, Özel Bulut'uza internet bağlantısına ve şirket içi veya Azure'a bağlantılara erişiminizi sürdürürsiniz.
* **CloudSimple portalı**. Bu tür bir bakım aralığı sırasında, CloudSimple portalındaki bazı özellikler devre dışı bırakılmış veya erişilemez olabilir.  Bakım aralığından önceki bildirim, bakım gerçekleşirken özellik sınırlamaları hakkındaki ayrıntıları içerir.

## <a name="connectivity"></a>Bağlantı

**CloudSimple bölge ağına bağlantı seçeneklerim nelerdir?**

CloudSimple, CloudSimple bölge ağınıza bağlanmak için aşağıdaki bağlantı seçeneklerini sağlar. Aynı anda birden çok seçenek kullanılabilir.

* **Şirket içi veri merkezinizden CloudSimple bölge ağına ExpressRoute bağlantısı.** Bu, şirket içi ExpressRoute devrenizi CloudSimple ExpressRoute devrenize bağlamak için Global Reach'i kullanan yüksek hızlı, düşük gecikmeli ve güvenli bir özel bağlantıdır. Bağlantıyı kurma yla ilgili talimatlar için [ExpressRoute'u kullanarak şirket içi Connect'ten CloudSimple'a](on-premises-connection.md)bakın.
* **Azure sanal ağınızdaki ExpressRoute bağlantısı CloudSimple bölge ağınıza.** Bu, Azure'daki sanal ağınızı CloudSimple ExpressRoute devrenize bağlamak için sanal ağ ağ geçitlerini kullanan yüksek hızlı, düşük gecikmeli ve güvenli bir özel bağlantıdır. Bağlantıyı kurma yla ilgili talimatlar için [bkz.](azure-expressroute-connection.md)
* **Şirket içi veri merkezinizden CloudSimple bölge ağınıza site-to-Site VPN bağlantısı.** Bu, şirket içi VPN aygıtınızdan CloudSimple Private Cloud bölgenize kadar güvenli bir sanal özel ağdır.  Ayrıntılar için bkz: [CloudSimple ağında VPN ağ geçitleri ayarlayın.](vpn-gateway.md)

**Özel Bulut'a nasıl bağlanırım?**

CloudSimple portalında Özel Bulut'unuzun ayrıntılarını görüntüleyebilirsiniz. Özel Bulut'unuza karşılık gelen vCenter'a bağlanmak için öncelikle Siteden Siteye VPN, Noktadan Siteye VPN veya ExpressRoute kullanarak bir ağ bağlantısı kurulduğunu doğrulayın. Ardından, Azure portalından CloudSimple portalını başlatın ve Giriş sayfasında veya Özel Bulut ayrıntıları sayfasında **vSphere İstemci** Başlat'ı tıklatın.

**ExpressRoute devrelerinin avantajı nedir?**

Azure ExpressRoute devresi yüksek hızlı, düşük gecikmeli, güvenli bir bağlantıdır.  CloudSimple, müşteri başına bölge başına özel bir ExpressRoute devresi sağlar.  Bu devreyi kullanarak şirket içinden veya Azure aboneliğinizden güvenli bir bağlantı kurabilirsiniz.

**CloudSimple'a bağlanmak için ağ maliyetleri nelerdir?  CloudSimple ve Azure arasında veya bölgeler arasında herhangi bir çıkış ücreti geçerli midir?**

Ağ çıkışları için CloudSimple ücreti yoktur.  Azure standart oranları, sanal ağınızdan veya şirket içi ExpressRoute devrenizden gelen çıkış trafiği için geçerlidir.

## <a name="networking"></a>Ağ Oluşturma

**Özel Bulut'um için hangi ağ özellikleri kullanılabilir?**

VUN'lar (ve alt ağları) ve güvenlik duvarı tabloları sağlayabilir ve özel bulutunuzda çalışan sanal bir makineye bu haritayı genel IP adresleri atayabilirsiniz. Ağ özellikleri hakkında ayrıntılı bilgi için [VUN'lara ve alt ağlara genel bakış,](cloudsimple-vlans-subnets.md) [Güvenlik Duvarı tablolarına genel bakış](cloudsimple-firewall-tables.md)ve Genel IP [adresine genel bakış](cloudsimple-public-ip-address.md)alabilirsiniz.

**Özel Bulut'umdaki uygulamalarım için farklı alt ağlar nasıl ayarlayabilirim?**

CloudSimple portalından Özel Bulut'unuzda VLAN'ler oluşturursunuz.  Bir VLAN oluşturduktan sonra, VLAN'ı kullanarak Özel Bulut vCenter'ınızda dağıtılmış bir bağlantı noktası grubu oluşturabilir ve dağıtılmış bağlantı noktası grubuna bağlı sanal makineler oluşturabilirsiniz.  VLAN/alt ağ için güvenlik duvarı tablolarını etkinleştirebilir ve ağ trafiğini güvence altına almak için güvenlik duvarı kurallarını tanımlayabilirsiniz.

**Özel Bulutlar'ım için hangi güvenlik duvarı ayarları kullanılabilir?**

Kuzey-güney ve doğu-batı trafiği için kuralları yapılandırabilirsiniz.  Kurallar bir güvenlik duvarı tablosunda tanımlanır.  Güvenlik duvarı tablosu, Özel Bulut'unuzdaki VLAN'lara eklenebilir.  Ayrıntılar için bkz: [Özel Bulutlar için güvenlik duvarı tabloları ve kuralları ayarlayın.](firewall.md)

**Özel Bulut ortamımdaki VM'lere genel IP adresleri atayabilir miyim?**

CloudSimple portalında, yeni bir genel IP adresi tahsis edebilir ve sanal bir makinenin veya bir cihazın özel IP adresiyle ilişkilendirebilirsiniz.  Ayrıca, portaldaki belirli bağlantı noktalarından ve IP adreslerinden gelen trafiğin trafiğine izin vermek için yeni güvenlik duvarı kuralları oluşturabilir veya varolan güvenlik duvarı kurallarını uygulayabilirsiniz. Ayrıntılar için, [Özel Bulut ortamı için genel IP adreslerini ayırma'ya](public-ips.md)bakın.

## <a name="security"></a>Güvenlik

**CloudSimple'daki güvenlik seçeneklerim nelerdir?**

CloudSimple, Özel Bulut ortamınızı güvence altına almak için aşağıdaki güvenlik özelliklerini sağlar:

* **Veri istirahat şifreleme**. Özel Bulut'unuzdavSAN depolama alanında bulunan istirahatte verileri şifreleyebilirsiniz. vSAN, Azure vNet'inizde veya şirket içi ortamınızda dağıtılabilen harici anahtar yönetim sunucularını destekler.  Ayrıntılar için [CloudSimple Private Cloud'unuzun vSAN şifrelemesine yapılandırılması](vsan-encryption.md)na bakın.
* **Ağ güvenliği.** Özel Bulut'unuz ile Internet, Özel Bulut ve şirket içi ortamınız arasında veya Özel Bulut'unuzun alt ağları arasında geçerli olan güvenlik duvarı kurallarıyla ağ trafiği akışını kontrol edin.
* **Güvenli, özel bağlantı.** Şirket içi ağınızla Azure aboneliğiniz arasında güvenli ve özel bir bağlantı kurulur.

## <a name="compute"></a>İşlem

**Ne tür ana bilgisayarlar mevcuttur?**

CloudSimple bu ana bilgisayar türlerini sunar:

* **CS28 düğümü:** Cpu:2x 2.2 GHz, toplam 28 çekirdek, 48 HT.  RAM: 256 GB.  Depolama: 1600 GB NVMe önbellek, 5760 GB veri (All-Flash). Ağ: 4x25Gbe NIC
* **CS36 düğümü:** Cpu 2x 2.3 GHz, toplam 36 çekirdek, 72 HT.  RAM: 512 GB.  Depolama: 3200 GB NVMe önbellek 11520 GB veri (All-Flash).  Ağ: 4x25Gbe NIC
* **CS36m düğüm:** Cpu 2x 2.3 GHz, toplam 36 çekirdek, 72 HT.  RAM: 576 GB.  Depolama: 3200 GB NVMe önbellek 13360 GB veri (All-Flash).  Ağ: 4x25Gbe NIC

**Donanım hataları nasıl işlenir?**

Tüm CloudSimple altyapısı CloudSimple platformu ve hizmet operasyon ekiplerimiz tarafından sürekli olarak izlenmektedir.  Bir donanım hatası algılanırsa, Özel Bulut'unuza yeni bir düğüm eklenir ve başarısız düğüm kaldırılır.

## <a name="storage"></a>Depolama

**Özel Bulut'ta ne tür depolama alanı desteklenir?**

CloudSimple, her Özel Bulut ile tüm flash VMware vSAN depolama alanı sunar.  Her vSphere kendi vSAN veri deposu ile oluşturulur.  Ayrıntılar için [Bkz. Özel Bulut VMware bileşenleri - vSAN depolama](vmware-components.md#vsan-storage).

**Verilerin şifrelemesi desteklenir mi?**
Evet.  VSAN'da depolanan verileri şifrelemek için şirket içinde veya Azure'da dağıtılan önemli bir yönetim sunucusu (KMS) kullanacak şekilde Özel Bulut'unuzdaki vSAN depolama alanını ayarlayabilirsiniz.

**Başarısız diskler nasıl işlenir?**

CloudSimple, Private Cloud'un tüm donanım bileşenlerini sürekli olarak izler.  Bir disk hatası algılanırsa veya bir disk başarısız olarak tanımlanırsa (sezgisel olarak temel alınarak), Özel Bulut'a otomatik olarak yeni bir düğüm eklenir.  Başarısız veya başarısız diskile düğüm Özel Bulut kaldırılır.

## <a name="vmware"></a>VMware

**Şirket içinde büyük ölçekli yükleme veya uygulama ve veri geçişini nasıl gerçekleştirebilirim?**

CloudSimple yerel bir VMware vSphere çözümü sağlar.  Toplu veri geçişi için tüm VMware araçları Özel Bulut'unuzun yanında kullanılabilir.  Seçeneklere şunlar dahildir:

* Verilerin toplu geçişi için VMware HCX.
* Storage vMotion'ı şirket içinde CloudSimple'a kullanarak verilerin soğuk geçişi.

**Herhangi bir VMware aracı yükleyebilir miyim?**

CloudSimple yerel bir VMware vSphere çözümü sağlar.  Şirket içi vSphere ortamınızı yönetmek için kullanılan tüm VMware araçları CloudSimple'da kullanılabilir.  CloudSimple, VMware araçlarını yüklemek için kendi lisansınızı getir (BYOL) modelini destekler.

**Güncelleştirmeler ve yükseltmeler nasıl yönetilir?**

CloudSimple, Özel Bulut'unuzun tüm altyapı bileşenlerini kesintisiz kesintisiz bir şekilde yönetir ve günceller.  VMware veya altyapı satıcıları tarafından yayımlanan tüm güncelleştirmeler ve güvenlik yamaları, CloudSimple tarafından uygun oldukları anda güncelleştirilmelidir.

CloudSimple, Özel Bulut'ta yüklü uygulamaların yükseltmelerini veya güncelleştirmelerini gerçekleştirmez.

## <a name="azure-integration"></a>Azure Tümleştirmesi

**Hangi Azure hizmetleri desteklenir?**

CloudSimple, Azure'daki aboneliğinize Azure ExpressRoute bağlantısı sağlar.  Aboneliğinizde çalışan tüm hizmetler Özel Bulut'unuza bağlanabilir.  Örneklere şunlar dahildir:

* CloudSimple vCenter'ınız için kimlik kaynağı olarak **Azure Active Directory.**
* Özel Bulut'unuzdan yedekleme, resim ve diğer verileri depolamak için **azure depolama.**
* Genel ve Özel Bulutlar'ı kapsayan bir uygulama mimarisine sahip **karma uygulamalar.**  Örneğin, Azure'da Özel Bulut'unuzda uygulama ve veritabanı sunucularıerişen web sunucuları oluşturabilirsiniz.
* VMware'de çalışan iş yükleri için **Azure monitörü** ve **Azure güvenlik merkezi** günlük, performans ölçümleri ve güvenlik yönetimi sağlar.

**VMware kiracılarımı Azure ile nasıl eşlerim?**

CloudSimple, Azure portalından Özel Bulut'taki VMware VM'lerinizi yönetme ye özgü bir yetenek sağlar.  İstenilen kaynak kısıtlamalarıyla yapılandırılan bir vCenter kaynak havuzu, genel yöneticiniz tarafından aboneliğinize eşlenebilir.  

**Azure ile hangi lisans avantajlarından yararlanırım?**

CloudSimple ile Azure Karma Kullanım Avantajı'ndan yararlanabilir ve lisanslarda %90'a kadar tasarruf edebilirsiniz. Bu avantaj, Microsoft lisanslarına yatırımınızı korur ve TCO'nuzu diğer bulut çözümlerine göre düşürür. Ayrıca Windows Server 2008 ve Microsoft SQL Server 2008 için genişletilmiş güvenlik güncelleştirmeleri alırsınız.  Kendi lisansını getir (BYOL) modeli, Veeam ve Zerto gibi yaygın uygulamaların maliyetlerini düşük tutmanıza yardımcı olur.  
