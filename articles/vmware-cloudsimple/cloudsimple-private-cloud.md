---
title: CloudSimple-Azure tarafından VMware çözümünde özel bulutlar
description: CloudSimple özel bulutları ve kavramları hakkında bilgi edinin.
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 9885366d5987870fe2739083ff47abaae9ef6ed1
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68816197"
---
# <a name="cloudsimple-private-cloud-overview"></a>CloudSimple özel buluta genel bakış

CloudSimple, VMware iş yüklerini dakikalar içinde genel bulutlara dönüştürür ve genişletir. CloudSimple hizmetini kullanarak VMware 'yi Azure çıplak altyapısına yerel olarak dağıtabilirsiniz. Dağıtımınız Azure konumlarında çalışır ve Azure bulutunun geri kalanı ile tamamen tümleştirilir.

* CloudSimple çözümü, tamamen VMware operasyonel devamlılığını sağlar. Bu çözüm, için genel bulut avantajları sağlar:
  * Esneklik
  * Yenilik
  * Verimlilik
* CloudSimple sayesinde, toplam sahip olma maliyetinizi alçaltan bir bulut tüketim modelinden faydalanırsınız. İsteğe bağlı sağlama, Kullandıkça öde ve kapasite iyileştirmesi de sunar.
* CloudSimple, ile tamamen uyumludur:
  * Mevcut araçlar
  * Beceriler
  * İşlemler
* Bu uyumluluk, takımlarınızın, ilkelerinizi kesintiye uğratmadan Azure bulutundaki iş yüklerini yönetmesini sağlar:
  * Ağ
  * Güvenlik  
  * Veri koruma  
  * Denetim
* CloudSimple, altyapıyı ve tüm gerekli ağ ve yönetim hizmetlerini yönetir. CloudSimple hizmeti takımınızın odaklanılmasını sağlar:
  * İş değeri
  * Uygulama sağlama
  * İş sürekliliği
  * Destek
  * İlke uygulama

## <a name="private-cloud-environment-overview"></a>Özel bulut ortamına genel bakış

Özel bulut, bu ortamlar gibi yalıtılmış bir VMware yığınına sahiptir:

* ESXi Konakları
* vCenter
* vSAN
* NSX

Özel bulutlar, kendi yönetim etki alanında bir vCenter sunucusu tarafından yönetilir.

Yığın üzerinde çalışır:

* Adanmış düğümler
* Yalıtılmış tam donanım düğümleri

Kullanıcılar, yığını aşağıdakiler dahil olmak üzere yerel VMware araçları aracılığıyla kullanır:

* vCenter
* NSX Yöneticisi

Azure konumlarında adanmış düğümler dağıtabilirsiniz. Daha sonra bunları Azure ve CloudSimple ile yönetebilirsiniz. Bir özel bulut, bir veya daha fazla vSphere kümesinden oluşur ve her küme 3 ile 16 arasında düğüm içerir.

Sağlanan düğümleri kullanarak özel bir bulut oluşturabilirsiniz:

* Kullandıkça Öde düğümleri
* Ayrılmış, adanmış düğümler

Özel bulutu aşağıdaki bağlantıları kullanarak şirket içi ortamınıza ve Azure ağına bağlayabilirsiniz:

* Güvenlik
* Özel VPN
* Azure ExpressRoute

Özel bulut ortamı, tek bir hata noktası olmasını önlemek için tasarlanmıştır:

* ESXi kümeleri vSphere yüksek kullanılabilirliğiyle yapılandırılır ve dayanıklılık için en az bir yedek düğüme sahip olacak şekilde boyutlandırılır.
* vSAN, yedekli birincil depolama alanı sağlar. vSan, tek bir hataya karşı koruma sağlamak için en az üç düğüm gerektirir. VSAN 'ı daha büyük kümeler için daha yüksek esneklik sağlayacak şekilde yapılandırabilirsiniz.
* Depolama hatasına karşı korunmak için, vCenter, PSC ve NSX Yöneticisi sanal makinelerini RAID-10 depolama ilkesi ile yapılandırabilirsiniz. Daha sonra, düğüm ve ağ hatalarıyla karşı vSphere HA tarafından korunur.

## <a name="scenarios-for-deploying-a-private-cloud"></a>Özel bulut dağıtmaya yönelik senaryolar

* **Veri merkezi kullanımdan kaldırma veya geçiş**

  * Var olan veri merkezinizin sınırlarına ulaştığınızda veya donanımı yenilediğiniz zaman ek kapasite kazanın.
  * Gerekli kapasiteyi buluta ekleyin ve donanım yenilemelerinin yönetilmesine engel olursunuz.
  * Zaman alan Dönüştürmelere veya yeniden mimariye kıyasla bulut geçişlerinin riskini ve maliyetini azaltın.
  * Bulut geçişlerini hızlandırmak için tanıdık VMware araçlarını ve yeteneklerini kullanın. Bulutta uygulamalarınızın hızını modernleştirin için Azure hizmetlerini kullanın.

* **İsteğe bağlı Genişlet**

  * Yeni geliştirme ortamları veya mevsimsel kapasite patlamaları gibi, beklenmeyen ihtiyaçları karşılamak için buluta genişletin.
  * İsteğe bağlı olarak yeni kapasite oluşturun ve yalnızca ihtiyaç duyduğunuz sürece saklayın.
  * Hem şirket içinde hem de bulutta aynı mimari ve ilkelerle, ön yatırımınızı düşürün, sağlama hızını hızlandırın ve karmaşıklığı azaltabilirsiniz.

* **Azure bulutunda olağanüstü durum kurtarma ve sanal masaüstleri**

  * Azure bulutundaki verilere, uygulamalara ve masaüstlerine uzaktan erişim sağlayın. Yüksek bant genişliğine sahip bağlantılarla, olayları kurtarmak için verileri hızlı bir şekilde karşıya yükler/indirirler. Düşük gecikmeli ağlar, kullanıcıların bir masaüstü uygulamasından beklediği sürelerle hızlı yanıt süreleri sağlar.

  * CloudSimple portalını ve tanıdık VMware araçlarını kullanarak buluttaki tüm ilkelerinizi ve ağınızı çoğaltın. Bu çoğaltma, DR ve VDı uygulamalarının oluşturulmasına ve yönetilmesine ilişkin çabayı ve riskleri azaltır.

* **Yüksek performanslı uygulamalar ve veritabanları**

  * En zorlu iş yüklerinizi CloudSimple tarafından sunulan hiper yakınsama mimarisiyle çalıştırın.
  * Oracle, Microsoft SQL Server, ara yazılım sistemleri ve yüksek performanslı SQL veritabanlarını çalıştırın.

  * Yüksek hızda 25 Gbps ağ bağlantıları sayesinde bulutu kendi veri merkezinizle yaşayın. Yüksek hızlı bağlantılar, performansa ödün vermeden şirket içi, Azure 'da VMware ve Azure özel iş yüklerine yayılan hibrit uygulamalar çalıştırmanızı sağlar.

* **Doğru karma**

  * VMware ve Azure hizmetleri arasında DevOps 'u birleştirin.
  * Azure hizmetleri ve tüm iş yüklerinizde uygulanabilen çözümler için VMware yönetimini iyileştirin.
  * Veri merkezinizi genişletmek veya uygulamalarınızı yeniden mimarmadan genel bulut hizmetlerine erişin.
  * Azure 'da VMware uygulamaları için kimlikleri, erişim denetim ilkelerini, günlüğe kaydetmeyi ve izlemeyi merkezileştirin.

## <a name="limits"></a>Sınırlar

Aşağıdaki tabloda, özel bir bulutun kaynaklarına ait düğüm sınırları gösterilmektedir.

| Resource | Sınır |
|----------|-------|
| Özel bir bulut oluşturmak için düğüm sayısı alt sınırı | 3 |
| Özel buluttaki bir kümede bulunan en fazla düğüm sayısı | 16 |
| Özel buluttaki en fazla düğüm sayısı | 64 |
| Yeni kümedeki düğüm sayısı alt sınırı | 3 |

## <a name="next-steps"></a>Sonraki adımlar

* [Özel bulut oluşturmayı](https://docs.azure.cloudsimple.com/create-private-cloud/) öğrenin
* [Özel bir bulut ortamını yapılandırmayı](quickstart-create-private-cloud.md) öğrenin