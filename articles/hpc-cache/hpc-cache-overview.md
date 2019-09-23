---
title: Azure HPC önbellek önizlemesine genel bakış
description: Yüksek performanslı bilgi işlem için bir dosya erişim Hızlandırıcısı çözümü olan Azure HPC Cache 'i açıklar
author: ekpgh
ms.service: hpc-cache
ms.topic: overview
ms.date: 09/19/2019
ms.author: v-erkell
ms.openlocfilehash: f4c858d7a71cf02b4a8fe181deecbf3a1b652885
ms.sourcegitcommit: a19bee057c57cd2c2cd23126ac862bd8f89f50f5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71180690"
---
# <a name="what-is-azure-hpc-cache-preview"></a>Azure HPC Önbelleği nedir? (Önizleme)

Azure HPC Cache, yüksek performanslı bilgi işlem (HPC) görevleri için verilerinize erişimi hızlandırır. Azure 'daki dosyaları önbelleğe alarak, verilerinizin WAN bağlantılarında depolandığı iş akışları için (örneğin, yerel veri merkezi ağa bağlı depolama (NAS) ortamınızda) bulut bilgi işlemin ölçeklenebilirliğini de kullanılabilir hale getirir.

Azure HPC önbelleğinin Azure portal kolayca başlatılması ve izlenmesi kolay bir işlemdir. Mevcut NFS depolaması veya yeni blob kapsayıcıları, arka uç depolama hedefini değiştirseniz bile, istemci erişiminin basit olmasını sağlayan, toplanmış ad alanının bir parçası haline gelebilir.

## <a name="use-cases"></a>Uygulama alanları

Azure HPC Cache, aşağıdaki gibi iş akışları için en iyi verimliliği geliştirir:

* Okuma ağır dosya erişimi iş akışı
* NFS erişimli depolama, Azure Blob veya her ikisine de depolanan veriler
* 75.000 adede kadar CPU çekirdeğini hesaplama grupları

Azure HPC önbelleği birçok sektör genelinde çok çeşitli iş akışlarına eklenebilir. Çok sayıda makinenin bir dosya kümesine ölçekli ve düşük gecikme süresiyle bu hizmetten faydalanır olması gereken tüm sistem. Aşağıdaki bölümler belirli örneklere sahiptir.

### <a name="visual-effects-vfx-rendering"></a>Görsel etkiler (VFX) işleme

Medya ve eğlence sürümünde Azure HPC önbelleği, zaman açısından kritik işleme projeleri için veri erişimini hızlandırabilir. VFX işleme iş akışları çoğunlukla çok sayıda işlem düğümü tarafından son dakikalık işleme gerektirir. Bu iş akışlarının verileri genellikle şirket içi bir NAS ortamında bulunur. Azure HPC önbelleği, gecikme süresini azaltmak ve isteğe bağlı işleme esnekliği geliştirmek için bulutta bu dosya verilerini önbelleğe alabilir.

### <a name="life-sciences"></a>Yaşam bilimleri

Birçok yaşam bilimleri iş akışı, genişleme dosya önbelleklemesi avantajlarından yararlanabilir.

Genomik analiz iş akışlarının Azure 'a bağlantı noktası almak isteyen bir Araştırma Enstitüsü, Azure HPC cache kullanarak bunları kolayca kaydırabilirler. Önbellek, POSIX dosya erişimi sağladığından, mevcut istemci tarafı iş akışını herhangi bir değişiklik yapmadan bulutta çalıştırabilirler.

Azure HPC Cache ayrıca ikincil analiz, ilaç macological simülasyonu veya AI odaklı görüntü analizi gibi görevlerde verimliliği artırmak için de yararlanılabilir olabilir.

### <a name="financial-services-analytics"></a>Finansal hizmetler analizi

Azure HPC önbelleği, finansal hizmetler şirketlerinin stratejik kararlar vermesini daha iyi kavramak için nicel Analysis hesaplamaları, risk analizi iş yükleri ve Monte Carlo benzetimleri hızlandırmaya yardımcı olabilir.

## <a name="region-availability"></a>Bölge kullanılabilirliği

Azure HPC önbelleği şu Azure bölgelerinde kullanılabilir:

* East US
* Doğu ABD 2
* Kuzey Avrupa
* Batı Avrupa
* Güneydoğu Asya
* Batı ABD 2

En son kullanılabilirlik bilgileri için [Azure HPC önbellek ürün sayfasını](https://azure.microsoft.com/services/hpc-cache) kontrol edin.

## <a name="preview-availability"></a>Önizleme kullanılabilirliği

Azure HPC önbelleği genel önizlemesi, hizmet kalitesi sağlamak için kısıtlıdır. [Bu formu](https://aka.ms/onboard-hpc-cache)doldurarak erişim isteyin. Aboneliğiniz erişim listesine eklendikten sonra, test önbellekleri oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Özellikleri hakkında daha fazla bilgi edinmek için [Azure HPC önbellek ürün sayfasını](https://azure.microsoft.com/services/hpc-cache) okuyun
* Ürün [önkoşulları](hpc-cache-prereqs.md) hakkında bilgi edinin
* Azure portal [bir Azure HPC önbelleği oluşturun](hpc-cache-create.md)
