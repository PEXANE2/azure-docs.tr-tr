---
title: Azure VMware çözümleri (AVS)-AVS özel bulutlar
description: AVS özel bulutları ve kavramları hakkında bilgi edinin.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 2688edf281a6d8bc3d61e8e294c920f115f0f3f6
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024968"
---
# <a name="avs-private-cloud-overview"></a>AVS özel buluta genel bakış

AVS, VMware iş yüklerini dakikalar içinde genel bulutlara dönüştürür ve genişletir. AVS hizmetini kullanarak, VMware 'yi Azure çıplak altyapısına yerel olarak dağıtabilirsiniz. Dağıtımınız Azure konumlarında çalışır ve Azure bulutunun geri kalanı ile tamamen tümleştirilir.

AVS çözümü, tamamen VMware operasyonel devamlılığını sağlar. Bu çözüm, için genel bulut avantajları sağlar:

* Esneklik
* Yenilik
* Verimlilik

AVS ile toplam sahip olma maliyetinizi alçaltan bir bulut tüketim modelinden faydalanırsınız. İsteğe bağlı sağlama, Kullandıkça öde ve kapasite iyileştirmesi de sunar.

AVS ile tamamen uyumludur:

* Mevcut araçlar
* Becerilere
* İşlemler

Bu uyumluluk, takımlarınızın Azure bulutundaki iş yüklerini şu ilke türlerini kesintiye uğratmadan yönetmesine olanak sağlar:

* Ağ
* Güvenlik  
* Veri koruması  
* Denetim

AVS, altyapıyı ve tüm gerekli ağ ve yönetim hizmetlerini yönetir. AVS hizmeti takımınızın üzerine odaklanılmasını sağlar:

* İş değeri
* Uygulama sağlama
* İş sürekliliği
* Destek
* İlke zorlama

## <a name="avs-private-cloud-environment-overview"></a>AVS özel bulut ortamına genel bakış

Bir AVS özel bulutu, şunları destekleyen yalıtılmış bir VMware yığınına sahiptir:

* ESXi Konakları
* vCenter
* vSAN
* NSX

AVS özel bulutlar, AVS portalı aracılığıyla yönetilir. Kendi yönetim etki alanında kendi vCenter Server kuruluşları vardır.

Yığın üzerinde çalışır:

* Ayrılmış düğümler
* Yalıtılmış tam donanım düğümleri

Kullanıcılar, yığını aşağıdakiler dahil olmak üzere yerel VMware araçları aracılığıyla kullanır:

* vCenter
* NSX Yöneticisi

Azure konumlarında adanmış düğümler dağıtabilirsiniz. Daha sonra bunları Azure ve AVS ile yönetebilirsiniz. Bir AVS özel bulutu bir veya daha fazla vSphere kümesinden oluşur ve her küme 3 ile 16 arasında düğüm içerir.

Satın alınan, Kullandıkça Öde düğümlerini veya ayrılmış, ayrılmış düğümleri kullanarak bir AVS özel bulutu oluşturabilirsiniz.

Aşağıdaki bağlantıları kullanarak AVS özel bulutunu şirket içi ortamınıza ve Azure ağına bağlayabilirsiniz:

* Güvenli
* Özel VPN
* Azure ExpressRoute

AVS özel bulut ortamı, tek hata noktalarını ortadan kaldırmak için tasarlanmıştır:

* ESXi kümeleri vSphere yüksek kullanılabilirliğiyle yapılandırılır ve dayanıklılık için en az bir yedek düğüme sahip olacak şekilde boyutlandırılır.
* vSAN, yedekli birincil depolama alanı sağlar. vSan, tek bir hataya karşı koruma sağlamak için en az üç düğüm gerektirir. VSAN 'ı daha büyük kümeler için daha yüksek esneklik sağlayacak şekilde yapılandırabilirsiniz.
* Depolama hatasına karşı korunmak için, vCenter, PSC ve NSX Yöneticisi sanal makinelerini RAID-10 depolama ilkesi ile yapılandırabilirsiniz. vSphere HA, düğüm ve ağ hatalarıyla karşı koruma sağlar.

## <a name="scenarios-for-deploying-an-avs-private-cloud"></a>AVS özel bulutu dağıtmaya yönelik senaryolar

Aşağıda, AVS özel bulut dağıtımı için bazı örnek kullanım örnekleri verilmiştir.

### <a name="data-center-retirement-or-migration"></a>Veri merkezi kullanımdan kaldırma veya geçiş

* Var olan veri merkezinizin sınırlarına ulaştığınızda veya donanımı yenilediğiniz zaman ek kapasite kazanın.
* Buluta gerekli kapasiteyi ekleyin ve donanım yenilemelerinin yönetilmesine engel olursunuz.
* Zaman alan Dönüştürmelere veya yeniden mimariye kıyasla bulut geçişlerinin riskini ve maliyetini azaltın.
* Bulut geçişlerini hızlandırmak için tanıdık VMware araçlarını ve yeteneklerini kullanın. Bulutta uygulamalarınızın hızını modernleştirin için Azure hizmetlerini kullanın.

### <a name="expand-on-demand"></a>İsteğe bağlı Genişlet

* Yeni geliştirme ortamları veya mevsimsel kapasite patlamaları gibi, beklenmeyen ihtiyaçları karşılamak için buluta genişletin.
* İsteğe bağlı olarak yeni kapasite oluşturun ve yalnızca ihtiyaç duyduğunuz sürece saklayın.
* Hem şirket içinde hem de bulutta aynı mimari ve ilkelerle, ön yatırımınızı düşürün, sağlama hızını hızlandırın ve karmaşıklığı azaltabilirsiniz.

### <a name="disaster-recovery-and-virtual-desktops-in-the-azure-cloud"></a>Azure bulutunda olağanüstü durum kurtarma ve sanal masaüstleri

* Azure bulutundaki verilere, uygulamalara ve masaüstlerine uzaktan erişim sağlayın. Yüksek bant genişliğine sahip bağlantılarla, olayları kurtarmak için verileri hızlı bir şekilde karşıya yükler/indirirler. Düşük gecikmeli ağlar, kullanıcıların bir masaüstü uygulamasından beklediği sürelerle hızlı yanıt süreleri sağlar.

* AVS portalını ve tanıdık VMware araçlarını kullanarak buluttaki tüm ilkelerinizi ve ağınızı çoğaltın. Çoğaltma, DR ve VDı uygulamalarının oluşturulmasına ve yönetilmesine ilişkin çabayı ve riski azaltır.

### <a name="high-performance-applications-and-databases"></a>Yüksek performanslı uygulamalar ve veritabanları

* En zorlu iş yüklerinizi, AVS tarafından sunulan hiper yakınsama mimarisiyle çalıştırın.
* Oracle, Microsoft SQL Server, ara yazılım sistemleri ve yüksek performanslı SQL veritabanlarını çalıştırın.
* Yüksek hızda 25 Gbps ağ bağlantıları sayesinde bulutu kendi veri merkezinizle yaşayın. Yüksek hızlı bağlantılar, performansa ödün vermeden şirket içi, Azure 'da VMware ve Azure özel iş yüklerine yayılan hibrit uygulamalar çalıştırmanızı sağlar.

### <a name="true-hybrid"></a>Doğru karma

* VMware ve Azure hizmetleri arasında DevOps 'u birleştirin.
* Azure hizmetleri ve tüm iş yüklerinizde uygulanabilen çözümler için VMware yönetimini iyileştirin.
* Veri merkezinizi genişletmek veya uygulamalarınızı yeniden mimarmadan genel bulut hizmetlerine erişin.
* Azure 'da VMware uygulamaları için kimlikleri, erişim denetim ilkelerini, günlüğe kaydetmeyi ve izlemeyi merkezileştirin.

## <a name="limits"></a>Sınırlar

Aşağıdaki tabloda, bir AVS özel bulutunun kaynaklarıyla ilgili düğüm sınırları listelenmektedir.

| Kaynak | Sınır |
|----------|-------|
| Bir AVS özel bulutu oluşturmak için gereken en az düğüm sayısı | 3 |
| Bir AVS özel bulutu 'ndaki kümede bulunan en fazla düğüm sayısı | 16 |
| Bir AVS özel bulutundaki en fazla düğüm sayısı | 64 |
| Yeni kümedeki düğüm sayısı alt sınırı | 3 |

## <a name="next-steps"></a>Sonraki adımlar

* [AVS özel bulutu oluşturmayı](create-private-cloud.md) öğrenin
* [AVS özel bulut ortamının nasıl yapılandırılacağını](quickstart-create-private-cloud.md) öğrenin
