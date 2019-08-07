---
title: SSS-CloudSimple 'a göre VMware çözümü
description: CloudSimple tarafından Azure VMware çözümü hakkında sık sorulan sorular
author: sharaths-cs
ms.author: b-shsury
ms.date: 05/24/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 80380a1c33927029e000e59a5834f297340f5be3
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68816234"
---
# <a name="frequently-asked-questions-about-vmware-solution-by-cloudsimple"></a>CloudSimple ile VMware çözümü hakkında sık sorulan sorular

CloudSimple tarafından hizmeti ve nasıl kullanacağınızı anlamanıza yardımcı olan Azure VMware çözümü hakkında sık sorulan sorular ve yanıtlar. Sorular ve yanıtlar aşağıdaki kategorilerde düzenlenir:

* CloudSimple hizmeti
* Bağlantı
* Ağ
* Güvenlik
* İşlem
* Depolama
* VMware
* Azure tümleştirmesi
 
## <a name="cloudsimple-service"></a>CloudSimple hizmeti

**CloudSimple Azure VMware çözümü nedir?**

CloudSimple ile Azure VMware çözümü, VMware iş yüklerini Azure 'da dakikalar içinde özel ve adanmış bulutlara genişletir. Çözümü sağlar, altyapıyı yönetir ve şirket içi ile Azure arasında iş yüklerini düzenler. Uygulamalarınız şirket içinde ve Azure 'da tam olarak aynı çalıştığından, uygulamalarınızın yeniden mimarının karmaşıklığı olmadan bulutun esneklik ve hizmetlerinden yararlanabilirsiniz. CloudSimple, isteğe bağlı sağlama, Kullandıkça öde ve kapasite iyileştirmesi sağlayan bir bulut tüketim modeliyle toplam sahip olma maliyetinizi azaltır. Özellikler, avantajlar ve senaryolar için bkz. [CloudSimple Azure VMware çözümü nedir?](cloudsimple-vmware-solutions-overview.md).

**CloudSimple özel bulutu nedir?**

Azure konumlarında Microsoft Azure altyapısına (donanım ve veri merkezi alanı) dağıtılan yüksek performanslı bir işlem, depolama ve ağ ortamından oluşan özel, adanmış bir bulut sağlacaksınız. Özel bulut hizmet olarak yerel bir VMware platformu sağlar. VMware koşullarında, her özel bulut vCenter Server tam olarak bir örneğini içerir. VCenter Server, bir veya daha fazla vSphere kümesinde bulunan birden çok ESXi düğümünü, karşılık gelen vSAN depolamayla birlikte yönetir. CloudSimple hizmeti, Azure aboneliğinizde birden çok özel bulut içerebilir. Özel bulutlar hakkında daha fazla bilgi için bkz. [özel buluta genel bakış](cloudsimple-private-cloud.md).

**CloudSimple hizmeti nerede kullanılabilir?**

CloudSimple, Doğu ABD ve Batı ABD bölgelerinde kullanılabilir.

**CloudSimple için aboneliğimi etkinleştirmek Nasıl yaparım? mı?**

Cloudsimple hizmeti aboneliğinizi etkinleştirmek [azurevmwaresales@microsoft.com](mailto:azurevmwaresales@microsoft.com) için ' de Microsoft hesabı temsilcinizle iletişime geçin. CloudSimple hizmetinin etkin olmasını istediğiniz e-postada abonelik KIMLIĞINIZI belirtin. 

**CloudSimple portalına erişmek Nasıl yaparım? mı?**

CloudSimple portalına Azure portal erişirsiniz. CloudSimple portalına erişme hakkında daha fazla bilgi için bkz. [Azure Portal cloudsimple Portal tarafından VMware çözümüne erişme](https://docs.azure.cloudsimple.com/access-cloudsimple-portal).

**Özel bulutun kapasitesini artırmak Nasıl yaparım? mı?**

Azure portal düğümleri sağlayın ve CloudSimple portalından özel bulutunuzu genişletebilirsiniz. Mevcut bir vSphere kümesine düğüm ekleyerek veya yeni bir vSphere kümesi oluşturarak özel bulutunuzu genişletebilirsiniz. Yordam hakkında daha fazla bilgi için bkz. [CloudSimple özel bulutu genişletme](https://docs.azure.cloudsimple.com/expand-private-cloud).

**Bakım sırasında özel bulutum ne olur?**

CloudSimple, zamanlanan bakımın öncesinde düzenli bildirim günleri sağlar. Bakım, özel bulutunuzun kullanılabilirliğini sağlamak için kalıcı olmayan bir şekilde yapılır. Bakım aşağıdaki türlerde olabilir:

- **Cloudsimple altyapısı**: CloudSimple altyapısı yüksek oranda kullanılabilir olacak şekilde tasarlanmıştır. Bakım sırasında özel bulutunuzun bağlantısı ve kullanılabilirliği, gereksiz bileşenleri herhangi bir etki olmadan tek bir kez güncelleştirerek, çok daha fazla. Özel bulut vCenter, tüm sanal makineler, özel bulutunuzun internet bağlantısı ve şirket içi veya Azure bağlantıları için erişiminiz vardır.
- **Cloudsimple portalı**: Bakım sırasında CloudSimple portalındaki bazı özellikler erişilebilir olmayabilir veya devre dışı bırakılabilir. Bakım bildirimi portalda nelerin yapılabilecekleri hakkında bilgi içerir.

## <a name="connectivity"></a>Bağlantı

**CloudSimple bölgesi ağınıza yönelik bağlantı seçeneklerim nelerdir?**

CloudSimple, CloudSimple bölge ağınıza bağlanmak için üç farklı bağlantı seçeneği sağlar. Üç seçenek de birlikte kullanılabilir:

- Şirket içi veri merkezinizden CloudSimple bölge ağına Azure ExpressRoute bağlantısı: Global Reach kullanarak, şirket içi ExpressRoute bağlantı hattınızı CloudSimple ExpressRoute devrenizi ile köprüleyerek yüksek hızlı, düşük gecikmeli güvenli bir özel bağlantı. Bağlantıyı kurmak için bkz. [ExpressRoute kullanarak şirket Içinden CloudSimple 'A bağlanma](https://docs.azure.cloudsimple.com/on-premises-connection).
- Azure sanal ağınızdan CloudSimple bölgesi ağınıza ExpressRoute bağlantısı: Sanal ağ geçitleri kullanarak Azure 'daki Sanal ağınızı CloudSimple ExpressRoute devreniz ile köprüleyerek yüksek hızlı, düşük gecikmeli güvenli bir özel bağlantı. Bağlantıyı kurmak için bkz. [ExpressRoute kullanarak CloudSimple özel bulut ortamınızı Azure sanal ağına bağlama](https://docs.azure.cloudsimple.com/azure-expressroute-connection).
- Şirket içi veri merkezinizden CloudSimple bölgesi ağınıza siteden siteye VPN bağlantısı: Şirket içi VPN cihazınızdan CloudSimple özel bulut bölgenize yönelik güvenli bir sanal özel ağ. Bağlantıyı kurmak için bkz. Şirket [içi ağınız ve CloudSimple özel bulutu arasında BIR VPN bağlantısı kurma](https://docs.azure.cloudsimple.com/set-up-vpn).

**Nasıl yaparım? özel buluta mı bağlanacak?**

Özel bulutunuzun ayrıntılarını CloudSimple portalında görüntüleyebilirsiniz. Özel buluta karşılık gelen vCenter 'a bağlanmak için, siteden siteye, Noktadan siteye veya ExpressRoute kullanarak bir ağ bağlantısının kurulu olduğundan emin olun. Ardından, Azure portal CloudSimple Portal ' ı başlatın. Giriş sayfasında veya özel bulut ayrıntıları sayfasında **vSphere Istemcisini Başlat** ' ı seçin.

**ExpressRoute devresinin avantajı nedir?**

Azure ExpressRoute bağlantı hattı, yüksek hızlı ve düşük gecikmeli güvenli bağlantı sağlar. CloudSimple, her müşteri için bölge başına adanmış bir ExpressRoute devresi sağlar. Bu devreyi kullanarak şirket içi ve Azure aboneliğinizden güvenli bağlantı kurabilirsiniz.

**CloudSimple 'a ve CloudSimple 'a bağlanmak için ağ maliyetleri nelerdir? CloudSimple 'dan Azure 'a giden çıkış ücretleri var mı? Bölgeler arasında çıkış ücretleri var mı?**

Ağ çıkışı için ücret alınmaz. Azure Standart ücretleri, sanal ağınızdan veya şirket içi bir ExpressRoute bağlantı hattından gelen çıkış trafiği için geçerlidir.

## <a name="networking"></a>Ağ

**Özel bulutum için hangi ağ özellikleri kullanılabilir?**

VLAN 'Ları ve bunların alt ağlarını ve güvenlik duvarı tablolarını sağlayabilirsiniz. Ortak IP adresleri atayabilir ve özel bulutunuzda çalışan bir sanal makineye eşleme yapabilirsiniz. Daha fazla bilgi için bkz. [VLAN ve alt ağlara genel bakış](cloudsimple-vlans-subnets.md), [güvenlik duvarı tablolarına genel](cloudsimple-firewall-tables.md)bakış ve [genel IP adresine genel bakış](cloudsimple-public-ip-address.md)

**Nasıl yaparım? özel Bulutumdaki Uygulamalarım için farklı alt ağlar mı ayarlanacak?**

Özel bulutunuzda, CloudSimple portalından VLAN 'Lar oluşturabilirsiniz. VLAN 'ı oluşturduktan sonra, VLAN kullanarak özel bulut vCenter 'unuzda dağıtılmış bir bağlantı noktası grubu oluşturabilir ve dağıtılmış bağlantı noktası grubuna bağlı sanal makineler oluşturabilirsiniz. VLAN veya subnet için bir güvenlik duvarı tablosu etkinleştirebilir ve ağ trafiğinin güvenliğini sağlamak için güvenlik duvarı kuralları tanımlayabilirsiniz.

**Özel bulutlarım için hangi güvenlik duvarı ayarları kullanılabilir?**

Kuzey-Güney ve Doğu-Batı trafiği için kurallar yapılandırabilirsiniz. Kurallar bir güvenlik duvarı tablosunda tanımlanır. Güvenlik Duvarı tablosu, özel bulutunuzda VLAN 'Lara bağlanabilir. Kurulum yordamı için bkz. [özel bulutlar için güvenlik duvarı tablolarını ve kurallarını ayarlama](https://docs.azure.cloudsimple.com/firewall).

**Özel bulut ortammdaki VM 'Ler için genel IP adresleri atayabilir miyim?**

CloudSimple portalında, kolayca yeni bir genel IP adresi ayırabilir ve bunu sanal makinenizin veya bir gerecin özel IP adresiyle ilişkilendirebileceğiniz bir cihaz oluşturabilirsiniz. Ayrıca, belirli bağlantı noktalarından ve portalda belirli IP adresi kümelerinden gelen trafiğe izin vermek için yeni güvenlik duvarı kuralları oluşturabilir veya mevcut güvenlik duvarı kurallarını uygulayabilirsiniz. Kurulum yordamı için, bkz. [özel bulut ortamı için genel IP adresleri ayırma](https://docs.azure.cloudsimple.com/public-ips).

## <a name="security"></a>Güvenlik

**CloudSimple 'ta güvenlik seçeneklerim nelerdir?**

CloudSimple özel bulutu, özel bulut ortamınızı güvenli hale getirmek için aşağıdaki güvenlik özelliklerini sağlar:

- **Bekleyen şifreleme verileri:** Özel bulutunuzda vSAN depolamada bulunan bekleyen verileri şifreleyebilirsiniz. vSAN, Azure sanal ağınızda veya şirket içi ortamınızda dağıtılabilecek bir dış anahtar yönetim sunucusunu destekler. Daha fazla bilgi için bkz. [CloudSimple özel bulutunuz Için vSAN şifrelemesini yapılandırma](https://docs.azure.cloudsimple.com/vsan-encryption).
- **Ağ güvenliği:** Güvenlik duvarı kurallarını kullanarak, şirket içinde ve özel bulutunuzun içindeki özel bulutunuzda bulunan ağ trafiği akışını denetleyin.
- **Güvenli, özel bağlantı:** Şirket içi ağınız ile Azure aboneliğiniz arasında güvenli, özel bağlantı.

## <a name="compute"></a>İşlem

**Hangi tür konaklar kullanılabilir?**

CloudSimple iki ana bilgisayar türü sunar:

* **CS28 düğümü**: CPU: 2x 2,2 GHz, toplam 28 çekirdek, 48 HT. RAM: 256 GB. Depolama: 1600-GB NVMe önbelleği, 5760 GB veri (tümü-Flash). Network 2x25Gbe NIC.
* **CS36 düğümü**: CPU 2x 2,3 GHz, toplam 36 çekirdek, 72 HT. RAM: 512 GB. Depolama: 3200-GB NVMe önbellek 11.520-GB veri (tümü-Flash). Network 2x25Gbe NIC.

**Donanım arızaları nasıl işlenir?**

Tüm CloudSimple altyapısı, CloudSimple platformu ve hizmet işlemleri ekipleri tarafından sürekli olarak izlenir. Bir donanım hatası algılanırsa, özel bulutunuzda yeni bir düğüm eklenir. Hatalı düğüm, özel bulutunuzun yüksek oranda kullanılabilir olmasını sağlamak için kaldırılır.

## <a name="storage"></a>Depolama

**Özel bulutta ne tür bir depolama alanı desteklenir?**

CloudSimple, her özel bulutla birlikte **Tüm-Flash VMware vSAN depolama alanı** sunar. Her vSphere kendi vSAN veri deposu ile oluşturulur. Daha fazla bilgi için bkz. [özel bulut VMware bileşenleri-vSAN depolaması](https://docs.azure.cloudsimple.com/vmware-components/#vsan-storage).

**Verilerin şifrelenmesi destekleniyor mu?**
Evet. Özel bulutunuzda vSAN depolamayı, şirket içinde veya vSAN 'da depolanan verilerin şifrelenmesi için şirket içinde veya Azure 'da dağıtılan bir anahtar yönetim sunucusu (KMS) kullanmak üzere ayarlayabilirsiniz.

**Hatalı diskler nasıl işlenir?**

CloudSimple izlemesi, özel bulutun tüm donanım bileşenlerini sürekli olarak izler. Disk hatası algılanırsa veya bir disk buluşsal yöntemler temelinde başarısız olarak tanımlanmışsa, özel buluta otomatik olarak yeni bir düğüm eklenir. Hatalı veya hatalı diski olan düğüm özel buluttan kaldırılır.

## <a name="vmware"></a>VMware

**Şirket içi uygulamalar ve verilerin büyük ölçekli bir şekilde karşıya yüklenmesi ve geçirilmesi Nasıl yaparım?.**

CloudSimple, yerel bir VMware vSphere çözümü sağlar. Toplu veri geçişi için kullanılan herhangi bir araç, CloudSimple özel bulutu ile kullanılabilir. Kullanılabilir seçeneklerden bazıları şunlardır:

- Verilerin toplu geçirilmesi için VMware HCX.
- Şirket içinden CloudSimple 'a Depolama vMotion kullanarak verilerin soğuk geçirilmesi.

**Herhangi bir VMware aracını yükleyebilir miyim?**

CloudSimple, yerel bir VMware vSphere çözümü sağlar. Şirket içi bir vSphere ortamını yönetmek için kullanılan herhangi bir araç CloudSimple üzerinde kullanılabilir. CloudSimple, VMware araçlarını yüklemek için kendi lisansını getir (KLG) modelini destekler.

**Güncelleştirmeler ve yükseltmeler nasıl yönetilir?**

CloudSimple, özel bulutunuzun tüm altyapı bileşenlerini sorunsuz bir şekilde yönetir ve güncelleştirir. VMware veya altyapı satıcıları tarafından yayınlanan tüm güncelleştirmeler veya güvenlik düzeltme ekleri, CloudSimple tarafından nitelendirilerek güncelleştirme için zamanlanır.

CloudSimple, özel bulutta yüklü olan uygulamaların yükseltmelerini veya güncelleştirmelerini gerçekleştirmez.

## <a name="azure-integration"></a>Azure tümleştirmesi

**Hangi Azure hizmetleri destekleniyor?**

CloudSimple, Azure 'da aboneliğinize Azure ExpressRoute bağlantısı sağlar. Aboneliğinizde çalışan hizmetlerin özel bulutunuz için ağ bağlantısı vardır ve özel bulutunuzun bağlanmasına izin verebilirsiniz. Örnekler:

- **Azure Active Directory**: CloudSimple vCenter için kimlik kaynağı olarak Azure Active Directory kullanın.
- **Azure depolama**: Yedeklemeleri, resimleri ve özel bulutunuzun diğer verilerini depolamak için depolama alanını kullanın.
- **Karma uygulamalar**: Ortak ve özel bulutları kapsayan uygulama mimarisi oluşturabilirsiniz. Örneğin, Azure 'da bir CloudSimple özel bulutu üzerindeki uygulama ve veritabanı sunucularına erişen Web sunucuları oluşturabilirsiniz.
- **Azure izleyici** ve **Azure Güvenlik Merkezi**: VMware 'de çalışan iş yükü, günlük, performans ölçümleri ve güvenlik yönetimi için Izleyici ve Güvenlik Merkezi 'ni kullanabilir.

**Nasıl yaparım? VMware kiracılarımı Azure 'a eşleyin mi?**

CloudSimple, VMware VM 'lerinizi Azure portal özel bir bulutta yönetmek için benzersiz bir özellik sağlar. İstediğiniz kaynak kısıtlamalarıyla yapılandırılmış bir vCenter kaynak havuzu, genel yönetici tarafından aboneliğinize eşlenebilir. 

**Azure ile hangi lisans avantajlarını alabilirim?**

CloudSimple sayesinde, Microsoft lisanslarını kullanarak yatırımlarınızı korumak ve diğer bulutlara kıyasla toplam sahiplik maliyetinizi düşürmek için Azure Hibrit Avantajı avantajlarından yararlanabilir ve lisanslarda yüzde 90 ' a varan bir tasarruf sağlayabilirsiniz. Ayrıca Windows Server 2008 ve 2008 Microsoft SQL Server için genişletilmiş güvenlik güncelleştirmeleri de alırsınız. Veeam, Zerto ve diğerleri gibi yaygın uygulamalar için KLG ile maliyetlerinizi buluta koruyun. 
