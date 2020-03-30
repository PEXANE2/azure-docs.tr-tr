---
title: CloudSimple tarafından Azure VMware Çözümü - Özel Bulutlar
description: CloudSimple Private Clouds ve kavramları hakkında bilgi edinin.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 4fb930603455ed1a5df5d357fcab669f41a0c28c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77024968"
---
# <a name="cloudsimple-private-cloud-overview"></a>CloudSimple Private Cloud'a genel bakış

CloudSimple, VMware iş yüklerini birkaç dakika içinde genel bulutlara dönüştürür ve genişletir. CloudSimple hizmetini kullanarak, VMware'i Azure çıplak metal altyapısında yerel olarak dağıtabilirsiniz. Dağıtımınız Azure konumlarında yaşar ve Azure bulutunun geri kalanıyla tam olarak bütünleşir.

CloudSimple çözümü tam VMware operasyonel süreklilik sağlar. Bu çözüm, aşağıdakilerin genel bulut avantajlarından yararlanır:

* Esneklik
* Yenilik
* Verimlilik

CloudSimple ile, toplam sahip olma maliyetinizi düşüren bir bulut tüketimi modelinden yararlanırsınız. Ayrıca isteğe bağlı sağlama, büyüdükçe öde ve kapasite optimizasyonu sunar.

CloudSimple ile tam uyumludur:

* Varolan araçlar
* Beceri
* İşlemler

Bu uyumluluk, ekiplerinizin bu tür ilkeleri bozmadan Azure bulutundaki iş yüklerini yönetmesini sağlar:

* Ağ
* Güvenlik  
* Veri koruma  
* Denetim

CloudSimple, altyapıyı ve gerekli tüm ağ ve yönetim hizmetlerini yönetir. CloudSimple hizmeti, ekibinizin aşağıdakilere odaklanmasını sağlar:

* İş değeri
* Uygulama sağlama
* İş sürekliliği
* Destek
* İlke zorlama

## <a name="private-cloud-environment-overview"></a>Özel Bulut ortamına genel bakış

Özel Bulut, şu ları destekleyen yalıtılmış bir VMware yığınıdır:

* ESXi ev sahipleri
* vCenter
* vSAN
* Nsx

Özel Bulutlar CloudSimple portalı üzerinden yönetilir. Kendi yönetim etki alanında kendi vCenter sunucuları vardır.

Yığın çalışır:

* Ayrılmış düğümler
* Yalıtılmış çıplak metal donanım düğümleri

Kullanıcılar yığını yerel VMware araçları aracılığıyla tüketirler, aşağıdakiler de dahil olmak üzere:

* vCenter
* NSX Yöneticisi

Azure konumlarında özel düğümler dağıtabilirsiniz. Ardından Bunları Azure ve CloudSimple ile yönetebilirsiniz. Özel Bulut bir veya daha fazla vSphere kümesinden oluşur ve her küme 3 ila 16 düğüm içerir.

Satın alınan, kullandıkça öde düğümlerini veya ayrılmış, ayrılmış düğümleri kullanarak Özel Bulut oluşturabilirsiniz.

Aşağıdaki bağlantıları kullanarak Özel Bulut'u şirket ortamınıza ve Azure ağına bağlayabilirsiniz:

* Güvenlik
* Özel VPN
* Azure ExpressRoute

Özel Bulut ortamı, tek hata noktalarını ortadan kaldırmak için tasarlanmıştır:

* ESXi kümeleri vSphere yüksek kullanılabilirliği ile yapılandırılır ve esneklik için en az bir yedek düğüm olacak şekilde boyutlandırılır.
* vSAN gereksiz birincil depolama sağlar. vSan tek bir hataya karşı koruma sağlamak için en az üç düğüm gerektirir. VSAN'ı daha büyük kümeler için daha yüksek esneklik sağlayacak şekilde yapılandırabilirsiniz.
* Depolama hatasına karşı korumak için vCenter, PSC ve NSX Manager VM'leri RAID-10 depolama ilkesiyle yapılandırabilirsiniz. vSphere HA düğüm ve ağ arızalarına karşı korur.

## <a name="scenarios-for-deploying-a-private-cloud"></a>Özel Bulut dağıtma senaryoları

Özel Bulut dağıtımı için bazı örnek kullanım örnekleri aşağıda verilmiştir.

### <a name="data-center-retirement-or-migration"></a>Veri merkezi emeklilik veya geçiş

* Mevcut veri merkezinizin sınırlarına ulaştığınızda veya donanımınızı yenilediğinizde ek kapasite elde edin.
* Bulutta gerekli kapasiteyi ekleyin ve donanım yenilemelerini yönetmenin baş ağrılarını ortadan kaldırın.
* Zaman alan dönüşümlere veya yeniden mimariye kıyasla bulut geçişlerinin risk lerini ve maliyetini azaltın.
* Bulut geçişlerini hızlandırmak için tanıdık VMware araçlarını ve becerilerini kullanın. Bulutta, uygulamalarınızı hızınızda modernize etmek için Azure hizmetlerini kullanın.

### <a name="expand-on-demand"></a>İsteğe bağlı genişletme

* Yeni geliştirme ortamları veya mevsimsel kapasite patlamaları gibi beklenmeyen gereksinimleri karşılamak için buluta genişletin.
* İsteğe bağlı olarak yeni kapasite oluşturun ve yalnızca ihtiyacınız olduğu sürece saklayın.
* Hem şirket içinde hem de bulutta aynı mimari ve politikalarla ön yatırımınızı azaltın, tedarik hızını hızlandırın ve karmaşıklığı azaltın.

### <a name="disaster-recovery-and-virtual-desktops-in-the-azure-cloud"></a>Azure bulutundaki olağanüstü durum kurtarma ve sanal masaüstü bilgisayarlar

* Azure bulutundaki verilere, uygulamalara ve masaüstü bilgisayarlara uzaktan erişim kurun. Yüksek bant genişliği neşredilen bağlantılarla, olaylardan kurtulmak için hızlı bir şekilde veri yüklersiniz/ indirirsiniz. Düşük gecikmeli ağlar, kullanıcıların bir masaüstü uygulamasından beklediği hızlı yanıt süreleri sağlar.

* CloudSimple portalını ve tanıdık VMware araçlarını kullanarak tüm ilkelerinizi ve ağ ağınızı bulutta çoğaltın. Çoğaltma, DR ve VDI uygulamaları oluşturma ve yönetme çabasını ve riskini azaltır.

### <a name="high-performance-applications-and-databases"></a>Yüksek performanslı uygulamalar ve veritabanları

* CloudSimple tarafından sağlanan hyperconverged mimarisi ile en zorlu iş yüklerinizi çalıştırın.
* Oracle, Microsoft SQL sunucusu, ara yazılım sistemleri ve yüksek performanslı NO-SQL veritabanlarını çalıştırın.
* Yüksek hızlı 25 Gbps ağ bağlantılarıyla bulutu kendi veri merkeziniz olarak yaşayın. Yüksek hızlı bağlantılar, performanstan ödün vermeden şirket içi, Azure'da VMware ve Azure özel iş yüklerini kapsayan karma uygulamaları çalıştırmanızı sağlar.

### <a name="true-hybrid"></a>Gerçek hibrid

* VMware ve Azure hizmetlerinde DevOps'leri birliyi birle.
* Azure hizmetleri ve tüm iş yükleri nizde uygulanabilecek çözümler için VMware yönetimini optimize edin.
* Veri merkezinizi genişletmek veya uygulamalarınızı yeniden yeniden architect'e getirmek zorunda kalmadan genel bulut hizmetlerine erişin.
* Azure'daki VMware uygulamaları için kimlikleri, erişim denetim ilkelerini, günlük günlüğe kaydetme ve izlemeyi merkezileştirin.

## <a name="limits"></a>Sınırlar

Aşağıdaki tabloda, Özel Bulut'un kaynaklarındaki düğüm sınırları listelendir.

| Kaynak | Sınır |
|----------|-------|
| Özel Bulut oluşturmak için minimum düğüm sayısı | 3 |
| Özel Bulut'ta kümedeki maksimum düğüm sayısı | 16 |
| Özel Bulut'ta maksimum düğüm sayısı | 64 |
| Yeni kümedeki en az düğüm sayısı | 3 |

## <a name="next-steps"></a>Sonraki adımlar

* [Özel Bulut oluşturmayı](create-private-cloud.md) öğrenin
* Özel Bulut ortamını nasıl [yapılandırılatırmayı](quickstart-create-private-cloud.md) öğrenin
