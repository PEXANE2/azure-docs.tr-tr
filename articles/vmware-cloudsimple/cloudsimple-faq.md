---
title: SSS-Azure VMware çözümleri (AVS)
description: Azure VMware çözümleri (AVS) hakkında sık sorulan sorular
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/15/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: c3808491c84f6c76a51c914aac6ee5e5ee370970
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025070"
---
# <a name="frequently-asked-questions-about-vmware-solution-by-avs"></a>AVS 'ye göre VMware çözümü hakkında sık sorulan sorular

## <a name="avs-service"></a>AVS hizmeti

**Azure VMware çözümleri (AVS) nedir?**

Azure VMware çözümleri (AVS), VMware iş yüklerini Azure 'da dakikalar içinde özel, adanmış bulutlara dönüştürür ve genişletir. AVS sağlama, altyapıyı yönetme ve şirket içi ile Azure arasında iş yüklerini düzenleme işlemlerini gerçekleştirir. Uygulamalarınız şirket içinde ve Azure 'da tam olarak aynı çalıştığından, uygulamalarınızın yeniden mimarının karmaşıklığı olmadan bulutun esneklik ve hizmetlerinden yararlanabilirsiniz. AVS, isteğe bağlı sağlama, Kullandıkça öde ve kapasite iyileştirmesi sağlayan bir bulut tüketim modeliyle toplam sahip olma maliyetinizi düşürür. Özellikler, avantajlar ve senaryolar için bkz. [AVS tarafından Azure 'Da VMware çözümü nedir?](cloudsimple-vmware-solutions-overview.md)

**AVS özel bulutu nedir?**

Bir AVS özel bulutu, Azure konumlarında Microsoft Azure altyapısına (donanım ve veri merkezi alanı) dağıtılan yüksek performanslı bir işlem, depolama ve ağ ortamından oluşan özel, ayrılmış bir bulutdır. Bir AVS özel bulutu, yerel bir VMware ' hizmet olarak platform ' sağlar. VMware koşullarında, her bir AVS özel bulutu vCenter Server tam olarak bir örneğini içerir. VCenter Server, bir veya daha fazla vSphere kümesinde bulunan birden çok ESXi düğümünü, karşılık gelen sanal SAN (vSAN) depolaması ile birlikte yönetir. Bir AVS hizmeti, Azure aboneliğinizde birden çok AVS özel bulutu içerebilir. Daha ayrıntılı bilgi için bkz. [AVS özel buluta genel bakış](cloudsimple-private-cloud.md).

**AVS hizmeti nerede kullanılabilir?**

AVS Doğu ABD, Batı ABD ve Batı Avrupa bölgelerinde çok yakında kullanıma sunulacak.

**Nasıl yaparım? AVS için aboneliğimi etkinleştirmek istiyor musunuz?**

Microsoft hesabı temsilcinizle, AVS hizmeti aboneliğinizi etkinleştirmek için [azurevmwaresales@microsoft.com](mailto:azurevmwaresales@microsoft.com) başvurabilirsiniz. , AVS hizmetinin etkinleştirilmesini istediğiniz e-postada abonelik KIMLIĞINIZI girin. 

**AVS portalına erişmek Nasıl yaparım? mı?**

Azure portal AVS portalına erişirsiniz. Ayrıntılar için bkz. [Azure Portal VMware çözümleri (AVS) portalına erişme](access-cloudsimple-portal.md).

**Bir AVS özel bulutunun kapasitesini artırmak Nasıl yaparım? mı?**

Kapasiteyi artırmak için Azure portal ek düğümler satın alın ve ardından düğümleri kullanarak AVS portalından özel bulutunuzu genişletin. Mevcut bir vSphere kümesine ek düğümler ekleyebilir veya bunları yeni bir vSphere kümesine ekleyebilirsiniz. Ayrıntılar için bkz. [AVS özel bulutunu genişletme](expand-private-cloud.md).

**Bakım sırasında AVS özel bulutum ne olur?**

AVS zamanlanan bir bakım aralığından önce birkaç gün önce bildirim sağlar. Bakım, AVS özel bulutunuzun kullanılabilirliğini güvence altına almak için kesintiye uğramamış bir şekilde yapılır. Bakım aşağıdaki türlerde olabilir:

* **AVS altyapısı**. AVS altyapısı yüksek oranda kullanılabilir olacak şekilde tasarlanmıştır. Bu tür bakım aralığı sırasında, herhangi bir hizmet kesintisini önlemek için yedekli bileşenler aynı anda güncelleştirilir. AVS özel bulut vCenter, tüm sanal makineler, AVS özel bulutunuzun internet bağlantısı ve şirket içi veya Azure bağlantıları için erişim sağlayabilirsiniz.
* **AVS portalı**. Bu tür bakım aralığı sırasında, AVS portalındaki bazı özellikler devre dışı bırakılabilir veya erişilemez durumda olabilir. Bakım aralığından önceki bildirim, bakım gerçekleşirken Özellik sınırlamalarıyla ilgili ayrıntıları içerir.

## <a name="connectivity"></a>Bağlantı

**AVS bölgesi ağına bağlantı seçeneklerim nelerdir?**

AVS, AVS bölge ağınıza bağlanmak için aşağıdaki bağlantı seçeneklerini sağlar. Aynı anda birden çok seçenek kullanılabilir.

* Şirket **içi veri MERKEZINIZDEN AVS bölge ağına ExpressRoute bağlantısı**. Bu, şirket içi ExpressRoute bağlantı hattınızı AVS ExpressRoute bağlantı hattına bağlamak için Global Reach kullanan yüksek hızlı, düşük gecikme süreli, güvenli bir özel bağlantıdır. Bağlantıyı ayarlama hakkında yönergeler için bkz. [ExpressRoute kullanarak şirket IÇINDEN AVS 'ye bağlanma](on-premises-connection.md).
* **Azure sanal AĞıNıZDAN AVS bölge ağınıza ExpressRoute bağlantısı**. Bu, Sanal ağınızı Azure üzerinde AVS ExpressRoute bağlantı hattına bağlamak için sanal ağ geçitleri kullanan yüksek hızlı, düşük gecikme süreli ve güvenli bir özel bağlantıdır. Bağlantıyı ayarlamayla ilgili yönergeler için bkz. [ExpressRoute kullanarak AVS özel bulut ortamınızı Azure sanal ağına bağlama](azure-expressroute-connection.md).
* Şirket **içi veri MERKEZINIZDEN AVS bölge ağınıza siteden sıteye VPN bağlantısı**. Bu, şirket içi VPN cihazınızdan AVS özel bulut bölgenize olan güvenli bir sanal özel ağ. Ayrıntılar için bkz. [AVS AĞıNDA VPN ağ geçitlerini ayarlama](vpn-gateway.md).

**Nasıl yaparım? bir AVS özel buluta mı bağlanacak?**

AVS portalında AVS özel bulutunuzun ayrıntılarını görüntüleyebilirsiniz. AVS özel bulutuna karşılık gelen vCenter 'a bağlanmak için önce siteden siteye VPN, Noktadan siteye VPN veya ExpressRoute kullanarak bir ağ bağlantısının yapıldığını doğrulayın. Ardından, Azure portal AVS portalını başlatın ve giriş sayfasında veya AVS özel bulut ayrıntıları sayfasında **vSphere Istemcisini Başlat** ' a tıklayın.

**ExpressRoute devrelerinin avantajı nedir?**

Azure ExpressRoute bağlantı hattı yüksek hızlı, düşük gecikmeli, güvenli bir bağlantıdır. AVS, her müşteri için bölge başına adanmış bir ExpressRoute devresi sağlar. Bu devreyi kullanarak şirket içi veya Azure aboneliğinizden güvenli bir bağlantı kurabilirsiniz.

**AVS 'ye bağlanmak için ağ maliyetleri nelerdir? Tüm çıkış ücretleri AVS ve Azure arasında mi yoksa bölgeler arasında mi uygulanır?**

Ağ çıkışı için bir AVS ücreti yoktur. Azure Standart ücretleri, sanal ağınızdan veya şirket içi ExpressRoute bağlantı hattınızdan gelen çıkış trafiği için geçerlidir.

## <a name="networking"></a>Networking (Ağ İletişimi)

**AVS özel bulutum için hangi ağ özellikleri kullanılabilir?**

VLAN 'Lar (ve alt ağları) ve güvenlik duvarı tabloları sağlayabilir ve AVS özel bulutunuzda çalışan bir sanal makineyle eşlenen genel IP adresleri atayabilirsiniz. Ağ özellikleriyle ilgili ayrıntılar için bkz. [VLAN ve alt ağlara genel bakış](cloudsimple-vlans-subnets.md), [güvenlik duvarı tablolarına genel](cloudsimple-firewall-tables.md)bakış ve [genel IP adresine genel bakış](cloudsimple-public-ip-address.md).

**Nasıl yaparım? AVS özel Bulutumdaki Uygulamalarım için farklı alt ağlar mı ayarlanacak?**

AVS portalından, AVS özel bulutunuzda VLAN 'Lar oluşturun. Bir VLAN oluşturduktan sonra, VLAN ' ı kullanarak AVS özel bulut vCenter 'unuzda dağıtılmış bir bağlantı noktası grubu oluşturabilir ve dağıtılmış bağlantı noktası grubuna bağlı sanal makineler oluşturabilirsiniz. VLAN/subnet için güvenlik duvarı tablolarını etkinleştirebilir ve ağ trafiğinin güvenliğini sağlamak için güvenlik duvarı kuralları tanımlayabilirsiniz.

**AVS özel bulutlarım için hangi güvenlik duvarı ayarları kullanılabilir?**

Kuzey-Güney ve Doğu-Batı trafiği için kurallar yapılandırabilirsiniz. Kurallar bir güvenlik duvarı tablosunda tanımlanır. Güvenlik Duvarı tablosu, AVS özel bulutunuzda VLAN 'Lara eklenebilir. Ayrıntılar için bkz. [AVS özel bulutları için güvenlik duvarı tablolarını ve kurallarını ayarlama](firewall.md).

**AVS özel bulut ortammdaki VM 'lere genel IP adresleri atayabilir miyim?**

AVS portalında, yeni bir genel IP adresi ayırabilir ve bunu bir sanal makinenin veya bir gerecin özel IP adresiyle ilişkilendirebileceğiniz bir cihaz oluşturabilirsiniz. Ayrıca, portalda belirli bağlantı noktalarından ve IP adreslerinden gelen trafiğe izin vermek için yeni güvenlik duvarı kuralları oluşturabilir veya mevcut güvenlik duvarı kurallarını uygulayabilirsiniz. Ayrıntılar için bkz. [AVS özel bulut ortamı için genel IP adresleri ayırma](public-ips.md).

## <a name="security"></a>Güvenlik

**AVS 'deki güvenlik seçeneklerim nelerdir?**

AVS, AVS özel bulut ortamınızı güvenli hale getirmek için aşağıdaki güvenlik özelliklerini sağlar:

* **Bekleyen şifreleme verileri**. AVS özel bulutunuzda vSAN depolamada bulunan bekleyen verileri şifreleyebilirsiniz. vSAN, Azure vNet 'iniz veya şirket içi ortamınızda dağıtılabilecek dış anahtar yönetim sunucularını destekler. Ayrıntılar için bkz. [AVS özel bulutunuz Için vSAN şifrelemesini yapılandırma](vsan-encryption.md).
* **Ağ güvenliği**. AVS özel bulutunuz ile internet, AVS özel bulutu ve şirket içi ortamınız ya da AVS özel bulutunuzun alt ağları arasında uygulanan güvenlik duvarı kurallarıyla ağ trafiği akışını denetleyin.
* **Güvenli, özel bağlantı**. Şirket içi ağınız ile Azure aboneliğiniz arasında güvenli, özel bir bağlantı oluşturulur.

## <a name="compute"></a>Bilgi İşlem

**Hangi tür konaklar kullanılabilir?**

AVS bu konak türlerini sunar:

* **CS28 düğümü:** CPU: 2x 2,2 GHz, toplam 28 çekirdek, 48 HT.  RAM: 256 GB.  Depolama: 1600 GB NVMe önbelleği, 5760 GB veri (tümü-Flash). Ağ: 4x25Gbe NIC
* **CS36 düğümü:** CPU 2x 2,3 GHz, toplam 36 çekirdek, 72 HT.  RAM: 512 GB.  Depolama: 3200 GB NVMe Cache 11520 GB veri (tümü-Flash).  Ağ: 4x25Gbe NIC
* **CS36m düğümü:** CPU 2x 2,3 GHz, toplam 36 çekirdek, 72 HT.  RAM: 576 GB.  Depolama: 3200 GB NVMe Cache 13360 GB veri (tümü-Flash).  Ağ: 4x25Gbe NIC

**Donanım hatalarının tümü nasıl işlenir?**

Tüm AVS altyapısı, AVS platformu ve hizmet işlemleri takımlarımız tarafından sürekli olarak izlenir. Bir donanım hatası algılanırsa, AVS özel buluta yeni bir düğüm eklenir ve başarısız olan düğüm kaldırılır.

## <a name="storage"></a>Depolama

**Bir AVS özel bulutu 'nda ne tür bir depolama destekleniyor?**

AVS, her AVS özel bulutuna sahip tüm-Flash VMware vSAN depolama alanı sunar. Her vSphere kendi vSAN veri deposu ile oluşturulur. Ayrıntılar için bkz. [AVS özel bulut VMware bileşenleri-vSAN depolaması](vmware-components.md#vsan-storage).

**Verilerin şifrelenmesi destekleniyor mu?**
Evet. VNET özel bulutunuzda vSAN depolama alanını, vSAN 'da depolanan verileri şifrelemek için şirket içinde veya Azure 'da dağıtılan bir anahtar yönetim sunucusu (KMS) kullanmak üzere ayarlayabilirsiniz.

**Hatalı diskler nasıl işlenir?**

AVS, AVS özel bulutunun tüm donanım bileşenlerini sürekli olarak izler. Bir disk hatası algılanırsa veya disk başarısız olarak tanımlanmışsa (buluşsal yöntemler temelinde), AVS özel bulutuna yeni bir düğüm otomatik olarak eklenir. Başarısız veya hatalı diski olan düğüm, AVS özel bulutlarından kaldırılır.

## <a name="vmware"></a>VMware

**Şirket içinden uygulamalar ve veriler için büyük ölçekli karşıya yükleme veya geçiş Nasıl yaparım? istiyor musunuz?**

AVS, yerel bir VMware vSphere çözümü sağlar. Toplu veri geçişi için tüm VMware araçları, AVS özel bulutuyla birlikte kullanılabilir. Seçeneklere şunlar dahildir:

* Verilerin toplu geçirilmesi için VMware HCX.
* Şirket içinden AVS 'ye Depolama vMotion kullanılarak verilerin soğuk geçirilmesi.

**Herhangi bir VMware aracını yükleyebilir miyim?**

AVS, yerel bir VMware vSphere çözümü sağlar. Şirket içi vSphere ortamınızı yönetmek için kullanılan tüm VMware araçları, AVS 'de kullanılabilir. AVS, VMware araçlarını yüklemek için kendi lisansını getir (KLG) modelini destekler.

**Güncelleştirmeler ve yükseltmeler nasıl yönetilir?**

AVS, AVS özel bulutunuzun tüm altyapı bileşenlerini sorunsuz bir şekilde yönetir ve güncelleştirir. VMware veya altyapı satıcıları tarafından yayınlanan tüm güncelleştirmeler ve güvenlik yamaları, AVS tarafından nitelendirildikleri anda güncelleştirme için zamanlanır.

AVS, AVS özel bulutu 'nda yüklü uygulamaların yükseltmelerini veya güncelleştirmelerini gerçekleştirmez.

## <a name="azure-integration"></a>Azure tümleştirmesi

**Hangi Azure hizmetleri destekleniyor?**

AVS, Azure 'da Aboneliğinize yönelik bir Azure ExpressRoute bağlantısı sağlar. Aboneliğinizde çalışan tüm hizmetler, AVS özel bulutuna bağlanabilir. Örneğin:

* AVS vCenter için bir kimlik kaynağı olarak **Azure Active Directory** .
* AVS özel bulutunuzun yedeklerini, görüntülerini ve diğer verilerini depolamak için **Azure depolama** .
* Ortak ve AVS özel bulutlarını kapsayan bir uygulama mimarisine sahip **karma uygulamalar** . Örneğin, Azure 'da, AVS özel bulutunuzda uygulama ve veritabanı sunucularına erişen Web sunucuları oluşturabilirsiniz.
* VMware desteği günlüğü, performans ölçümleri ve güvenlik yönetimi üzerinde çalışan iş yükleri için **Azure izleyici** ve **Azure Güvenlik Merkezi** .

**Nasıl yaparım? VMware kiracılarımı Azure 'a eşleyin mi?**

AVS, Azure portal AVS özel bulutunda VMware VM 'lerinizi yönetmek için benzersiz bir özellik sağlar. İstenen kaynak kısıtlamalarıyla yapılandırılmış bir vCenter kaynak havuzu, genel yöneticiniz tarafından aboneliğiniz ile eşleştirilebilir. 

**Azure ile hangi lisans avantajlarını alabilirim?**

AVS ile Azure hibrit kullanım avantajı avantajlarından yararlanarak lisanslarda %90 tasarruf edebilirsiniz. Bu avantaj, yatırımınızı Microsoft lisanslarına karşı korur ve diğer bulut çözümlerine göre TCO 'nizi düşürür. Ayrıca Windows Server 2008 ve 2008 Microsoft SQL Server için genişletilmiş güvenlik güncelleştirmeleri de alırsınız. Kendi lisansını getir (KLG) modeli, Veeae ve Zerto gibi ortak uygulamalar için maliyetleri düşük tutmaya yardımcı olur. 
