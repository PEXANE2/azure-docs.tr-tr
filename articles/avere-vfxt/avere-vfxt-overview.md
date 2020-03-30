---
title: Azure için Avere vFXT
description: HPC için bulut önbellek katmanı olan Azure için Avere vFXT'ye giriş
author: ekpgh
ms.service: avere-vfxt
ms.topic: overview
ms.date: 01/13/2020
ms.author: rohogue
ms.openlocfilehash: a9dab2ca844feb1c24dfffc48b4f29cd1a311314
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76153200"
---
# <a name="what-is-avere-vfxt-for-azure"></a>Azure için Avere vFXT nedir?

Azure için Avere vFXT, yoğun veri kullanan yüksek performanslı bilgi işlem (HPC) görevlerine yönelik bir dosya sistemi önbellek çözümüdür. Bulut bilişimin ölçeklenebilirlik avantajlarından faydalanarak şirket içi donanımlarınızda bulunan veriler dahil olmak üzere tüm verilerinize ihtiyaç duyulduğu yerde ve zamanda erişim sağlanmasını mümkün kılar.

Avere vFXT şu yaygın bilgi işlem senaryolarını destekler:

* Karma bulut mimarisi - Azure için Avere vFXT, dosyaları taşımak zorunda kalmadan bulut bilgi işlemavantajı sağlayan bir donanım depolama sistemiyle çalışabilir.

* Bulut patlaması - Azure için Avere vFXT, verilerinizi tek bir proje için buluta taşımanıza veya tüm iş akışını kalıcı olarak "kaldırmave kaydırmaya" yardımcı olabilir.

![Blob depolama alanına ve şirket içi veri merkezine bağlı bir Azure aboneliğindeki Avere vFXT sisteminin ayrıntılarını gösteren diyagram](media/avere-vfxt-hybrid.png)

Azure için Avere vFXT bu durumlar için en uygun olan:

* HPC iş yükleri için yoğun okuma işlemleri
* Ortak NFS protokolünü kullanan uygulamalar
* 1000-40.000 CPU çekirdeğine sahip işlem grupları
* Şirket içi NAS donanımı, Azure Blob depolama alanı veya ikisiyle birden tümleştirme

Daha fazla bilgi için <https://azure.microsoft.com/services/storage/avere-vfxt/> adresini ziyaret edin.

## <a name="who-uses-avere-vfxt-for-azure"></a>Azure için Avere vFXT kimlere yöneliktir?

Avere vFXT, okuma açısından yoğun bilgi işlem görevlerinde yardımcı olabilir:

### <a name="visual-effects-rendering"></a>Görsel efekt oluşturma

Avere vFXT kümesi medya ve eğlence sektöründe zaman açısından kritik projeler için veri erişimini hızlandırabilir. Azure'da daha fazla önbellek alanı ve işlem düğümü ekleme imkanına sahip olduğunuz için büyük projeleri verimli bir şekilde işleyebilirsiniz.

### <a name="life-sciences"></a>Yaşam bilimleri

Avere vFXT, araştırmacıların Azure İşleme'de ikincil analiz iş akışları çalıştırmalarına ve konumları ne olursa olsun genomik verilere erişmelerine olanak tanır.

Farmasötik araştırmalarda, Avere vFXT kümeleri araştırmacıların ilaç hedef etkileşimlerini tahmin etmesine ve araştırma verilerini analiz etmesine yardımcı olarak ilaç keşfini hızlandırabilir.

### <a name="financial-services-analytics"></a>Finansal hizmetler analizi

Avere vFXT kümesi nicel analiz hesaplamalarını hızlandırmaya yardımcı olarak finansal kuruluşlara stratejik karar alma konusunda daha iyi içgörüler sunabilir.

## <a name="features-and-specifications"></a>Özellikler ve belirtimler

Avere vFXT sistemi, bir küme halinde yapılandırılmış üç veya daha fazla sanal kenar dosya düğümünden oluşur. İstemci makinelerin yanında konumlandırılarak doğrudan depolama alanını takmak yerine kümeyi takma seçeneği sunar.

Avere vFXT kümesi, istenen dosyaları önbelleğe alır. Tekrarlanan istekler %80'den daha fazla önbellekten sunulabilir.

### <a name="compatibility"></a>Uyumluluk

* NetApp veya Dell EMC Isilon donanımsal NAS sistemleri ile uyumludur
* Azure Blob ile uyumludur
* NFSv3 veya SMB2 protokolünü kullanır

Azure için Avere vFXT aşağıdaki Azure kaynaklarını kullanır:

|Azure bileşeni|   |
|----------|-----------|
|Sanal makineler|3 veya daha fazla E32s_v3|
|Premium SSD depolama alanı|200 GB işletim sistemi alanına ek olarak düğüm başına 1 TB-4 TB önbellek alanı |
|Depolama hesabı (isteğe bağlı) |v2|
|Veri arka uç depolama (isteğe bağlı) | Boş bir LRS Blob kapsayıcısı |

## <a name="next-steps"></a>Sonraki adımlar

Azure dağıtımı için kendi Avere vFXT'nizi planlamak ve oluşturmak için bu makaleleri okuyun.

* [Sisteminizi planlayın](avere-vfxt-deploy-plan.md)
* [Dağıtıma genel bakış](avere-vfxt-deploy-overview.md)
* [vFXT ortamını oluşturma](avere-vfxt-deploy.md)
