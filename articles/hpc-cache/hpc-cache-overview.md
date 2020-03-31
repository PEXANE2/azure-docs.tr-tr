---
title: Azure HPC Önbelleğine genel bakış
description: Yüksek performanslı bilgi işlem için bir dosya erişim hızlandırıcı çözümü olan Azure HPC Önbelleğini açıklar
author: ekpgh
ms.service: hpc-cache
ms.topic: overview
ms.date: 10/30/2019
ms.author: rohogue
ms.openlocfilehash: 2a008d22de5df8d091e868153205697b4bb343ee
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79241195"
---
# <a name="what-is-azure-hpc-cache"></a>Azure HPC Önbelleği nedir?

Azure HPC Önbelleği, yüksek performanslı bilgi işlem (HPC) görevleri için verilerinize erişimi hızlandırır. Azure'da dosyaları önbelleğe alarak Azure HPC Önbelleği, bulut bilgi işlemin ölçeklenebilirliğini mevcut iş akışınıza getirir. Bu hizmet, verilerinizin yerel veri merkezi ağ bağlantılı depolama (NAS) ortamıgibi WAN bağlantıları arasında depolandığı iş akışları için bile kullanılabilir.

Azure HPC Önbelleğinin başlatılması ve Azure portalından izlenmesi kolaydır. Varolan NFS depolama alanı veya yeni Blob kapsayıcıları, arka uç depolama hedefini değiştirseniz bile istemci erişimini basit kılan toplu ad alanının bir parçası haline gelebilir.

## <a name="use-cases"></a>Uygulama alanları

Azure HPC Önbelleği, şu gibi iş akışları için üretkenliği en iyi şekilde artırır:

* Okuma ağırlıklı dosya erişimi iş akışı
* NFS'ye erişilebilen depolama, Azure Blob veya her ikisinde depolanan veriler
* 75.000 CPU çekirdeğine kadar hesaplama çiftlikleri

Azure HPC Önbelleği birçok endüstride çok çeşitli iş akışlarına eklenebilir. Çok sayıda makinenin ölçekte ve düşük gecikme gecikmesi olan bir dizi dosyaya erişmeleri gereken tüm sistem bu hizmetten yararlanacaktır. Aşağıdaki bölümlerde belirli örnekler verilmiştir.

### <a name="visual-effects-vfx-rendering"></a>Görsel efektler (VFX) oluşturma

Azure HPC Önbelleği, medya ve eğlence de zaman açısından kritik hale getirme projeleri için veri erişimini hızlandırabilir. VFX işleme iş akışları genellikle çok sayıda bilgi işlem düğümü tarafından son dakika işleme gerektirir. Bu iş akışlarına ait veriler genellikle şirket içi NAS ortamında bulunur. Azure HPC Önbelleği, gecikme süresini azaltmak ve isteğe bağlı işleme esnekliğini artırmak için bu dosya verilerini bulutta önbelleğe alabilir.

### <a name="life-sciences"></a>Yaşam bilimleri

Birçok yaşam bilimleri iş akışları ölçek-out dosya önbelleğe yararlanabilir.

Genomik analiz iş akışlarını Azure'a taşımayı isteyen bir araştırma enstitüsü, Azure HPC Önbelleğini kullanarak bunları kolayca kaydırabilir. Önbellek POSIX dosya erişimi sağladığından, buluttaki varolan istemci iş akışlarını çalıştırmak için istemci tarafında değişiklik gerekmez.

Azure HPC Önbelleği, ikincil analiz, farmakolojik simülasyon veya AI odaklı görüntü analizi gibi görevlerde verimliliği artırmak için de kullanılabilir.

### <a name="financial-services-analytics"></a>Finansal hizmetler analizi

Azure HPC Önbellek dağıtımı, finansal hizmetler şirketlerine stratejik kararlar vermeleri için daha iyi bir fikir vermek için nicel analiz hesaplamalarını, risk analizi iş yüklerini ve Monte Carlo simülasyonlarını hızlandırmaya yardımcı olabilir.

## <a name="region-availability"></a>Bölge kullanılabilirliği

Azure HPC Önbelleği şu Azure bölgelerinde kullanılabilir:

* Doğu ABD
* Doğu ABD 2
* Kuzey Avrupa
* Batı Avrupa
* Güneydoğu Asya
* Sidney
* Batı ABD 2
* Güney Kore - Orta

En son kullanılabilirlik bilgileri için [Azure HPC Önbellek ürün sayfasını](https://azure.microsoft.com/services/hpc-cache) kontrol edin.

## <a name="service-availability"></a>Hizmet kullanılabilirliği

Azure HPC Önbelleği ile kullanacağınız her abonelik için erişim istemeniz gerekir. Bu kısıtlama, genel kullanılabilirliğin ilk aylarında hizmet kalitesinin sağlanmasına yardımcı olur.

[Bu formu](https://aka.ms/onboard-hpc-cache)doldurarak erişim isteyin. Aboneliğiniz erişim listesine eklendikten sonra önbellekler oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Özellikleri hakkında daha fazla bilgi edinmek için [Azure HPC Önbellek ürün sayfasını](https://azure.microsoft.com/services/hpc-cache) okuyun
* Ürün [ön koşulları](hpc-cache-prereqs.md) hakkında bilgi edinin
* Azure portalından [Bir Azure HPC Önbelleği Oluşturma](hpc-cache-create.md)
