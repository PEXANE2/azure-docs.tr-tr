---
title: Azure VMware çözümleri (AVS)-genel bakış
description: AVS hizmeti ile Azure 'da VMware çözümünün özellikleri, senaryoları ve avantajları hakkında bilgi edinin.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 5de5a11f520a6882bb474e9926ad370bf330be1e
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024883"
---
# <a name="what-is-azure-vmware-solutions-avs"></a>Azure VMware çözümleri (AVS) nedir?

**Azure VMware çözümü (AVS)** , Azure 'da VMware platformunu çalıştırmanızı sağlayan, tam olarak yönetilen bir hizmettir. Bu çözüm vSphere, vCenter, vSAN, NSX-T ve ilgili araçları içerir. VMware ortamınız Azure bulut konumlarında yerel olarak Azure çıplak altyapısında çalışır. Hizmet, VMware platformlarını verimli ve güvenli bir şekilde kullanmak için gereken tüm özellikleri içerir.

![AVS tarafından Azure 'da VMware çözümüne genel bakış](media/azure-vmware-solution-by-cloudsimple.png)

## <a name="features"></a>Özellikler

* VMware bulut ortamları için isteğe bağlı Self Servis sağlama. İsteğe bağlı kapasite ekleme ve kaldırma özelliği.
* VMware platform dağıtımı, yükseltme, yönetim düzlemi yedeklemesi, sistem durumu/kapasite izleme, uyarı, sorun giderme ve düzeltme.
* L2/L3 Hizmetleri ve Güvenlik Duvarı kural yönetimi de dahil olmak üzere VMware 'yi etkinleştirmek için gereken ağ oluşturma hizmetleri.
* Uç türü VPN, genel IP ve internet ağ geçitleri dahil olmak üzere ağ hizmetleri. Bu hizmetler Azure üzerinde çalışır ve Azure 'un güvenlik ve DDoS korumasını taşır.
* Maliyetleri azaltmak için kapasite rezervasyonu.
* Azure ve şirket içi için yüksek hızlı, düşük gecikmeli bağlantı.
* Müşterilerin Azure hizmetlerini tümleşik bir biçimde tüketmesi ve bu benzersiz "VMware bulutu ortak bir bulutta" mimarisinden yararlanması için çözüm mimarileri. Azure hizmetleri Azure AD, depolama, uygulama ağ geçitleri ve diğerlerini içerir.
* Size tam olarak adanmış ve diğer müşterilerin altyapısından fiziksel olarak yalıtılmış altyapı.
* Bu etkinlik yönetimi, kullanım, faturalandırma/ölçüm ve Kullanıcı yönetimi gibi yönetim özellikleri.
* 7/24 müşteri desteği.

## <a name="benefits"></a>Avantajlar

* **Işlemsel süreklilik**. AVS, VMware platformları için yerel erişim sağlar. AVS mimarisi, var olan kişilerle uyumludur:
    * Uygulamalar
    * Operations
    * Güvenlik
    * Backup
    * Olağanüstü durum kurtarma
    * Denetim
    * Uyumluluk araçları
    * İşlemler
* **Yeniden eğitim yok**. VMware platformu uyumluluğu, mevcut becerileri ve bilgileri kullanmanıza olanak sağlar.
* **Altyapı çevikliği**. Artık tüm kapasite ihtiyaçlarınızı tahmin etmek zorunda kalmıyor ve daha sonra boşa harcanan kapasite veya altyapı eksiklikleri ile bitmiyor. AVS bir bulut hizmeti olarak dağıtılır ve istediğiniz zaman kapasiteyi ekleyebilir veya azaltabilirsiniz
* **Güvenlik**. Azure aracılığıyla AVS ortamına erişim, yerleşik DDoS koruması ve güvenlik izleme sağlar.
* **Daha düşük maliyet**. AVS platformu yüksek düzeyde tasarlanmıştır ve yüksek düzeyde Otomasyon, operasyonel verimlilik ve ölçek ekonomislerini sağlar. Ayrıca, AVS, maliyetleri düşürmek için genel bir bulutta VMware 'nin varlığını faydalanan çözüm mimarileri yayınlar. Azure AD, Azure depolama 'ya yedekleme, uygulama ağ geçidi, yük dengeleyici ve diğer örneklere örnek olarak verilebilir.
* **Yeni bir karma platform**. Hizmet, Azure 'un geri kalanına yüksek hızlı, düşük gecikmeli erişim sağlar. Ayrıca, AVS yönetimi aynı kullanıcı arabirimi ve API 'YI kullanarak VMware sanal makinelerinin ve Azure 'un geri kalanının birleştirilmiş yönetimine izin verebilir. Geliştirme ekipleriniz hem ortak hem de özel platformlardan yararlanarak Tümleşik, tutarlı bir biçimde yararlanabilir.
* **Altyapı izleme, sorun giderme ve destek**. AVS, temel altyapınızı bir hizmet olarak çalıştırır. Başarısız donanım otomatik olarak değiştirilmiştir. AVS, ortamın sorunsuz çalışmasını sağlarken, tüketimine odaklanırsınız.
* **İlke uyumluluğu**. VMware tabanlı araçlarınızı, güvenlik yordamlarını, denetim uygulamalarınızı ve uyumluluk sertifikalarınızı koruyun.

## <a name="scenarios"></a>Senaryolar

* **Veri merkezi kullanımdan kaldırma veya geçiş**. Var olan veri merkezinizin sınırlarına ulaştığınızda veya donanımı yenilediğiniz zaman ek kapasite kazanın. Buluta gerekli kapasiteyi eklemek ve donanım yenilemeleri yönetme ile ilgili ek işlemleri ortadan kaldırmak çok kolay. Zaman alan Dönüştürmelere veya yeniden mimariye kıyasla bulut geçişlerinin riskini ve maliyetini azaltın. Bulut geçişlerini hızlandırmak için tanıdık VMware araçlarını ve yeteneklerini kullanın. Bulutta uygulamalarınızın hızını modernleştirin için Azure hizmetlerini kullanın.
* **İsteğe bağlı ' yı genişletin**. Yeni geliştirme ortamları veya mevsimsel kapasite patlamaları gibi, beklenmeyen ihtiyaçları karşılamak için buluta genişletin. İsteğe bağlı olarak kolayca yeni kapasite oluşturabilir ve bunu yalnızca ihtiyaç duyduğunuz sürece tutabilirsiniz. Hem şirket içinde hem de bulutta aynı mimari ve ilkelerle, ön yatırımınızı düşürün, sağlama hızını hızlandırın ve karmaşıklığı azaltabilirsiniz.
* **Azure bulutunda olağanüstü durum kurtarma ve sanal masaüstleri**. Azure bulutundaki verilere, uygulamalara ve masaüstlerine uzaktan erişim sağlayın. Yüksek bant genişliğine sahip bağlantılarla, olayları kurtararak verileri hızlı bir şekilde karşıya yükleyip indirirler. Düşük gecikmeli ağlar, kullanıcıların bir masaüstü uygulamasından beklediği sürelerle hızlı yanıt süreleri sağlar. AVS ile, AVS portalı ve tanıdık VMware araçlarını kullanarak tüm ilkelerinizi ve ağınızı bulutta çoğaltmak çok kolaydır. Kurtarma ve çoğaltma kolaylığı, DR ve VDı uygulamalarının oluşturulmasına ve yönetilmesine ilişkin çabaları ve riskleri önemli ölçüde azaltır.
* **Yüksek performanslı uygulamalar ve veritabanları**. AVS, en zorlu VMware iş yüklerinizi çalıştırmak için tasarlanan hiper yakınsanmış bir mimari sağlar. Oracle, Microsoft SQL Server, ara yazılım sistemleri ve yüksek performanslı SQL veritabanlarını çalıştırın. Şirket içi, Azure 'da VMware ve Azure özel iş yükleriyle, performansı tehlikeye atmadan karma uygulamalar çalıştırmanıza olanak sağlayan, yüksek hızda 25 Gbps ağ bağlantısı olan bulut 'u kendi veri merkezinizle yaşayın.
* **Doğru karma**. VMware ve Azure arasında DevOps 'u birleştirin. Azure hizmetleri ve tüm iş yüklerinizde uygulanabilen çözümler için VMware yönetimini iyileştirin. Veri merkezinizi genişletmek veya uygulamalarınızı yeniden mimarmadan genel bulut hizmetlerine erişin. Azure 'da VMware uygulamaları için kimlikleri, erişim denetim ilkelerini, günlüğe kaydetmeyi ve izlemeyi merkezileştirin.

![Senaryolar](media/cloudsimple-scenarios.png)

## <a name="next-steps"></a>Sonraki adımlar

* [AVS hizmeti oluştur](quickstart-create-cloudsimple-service.md)
* [AVS özel bulutu oluştur](quickstart-create-private-cloud.md)
