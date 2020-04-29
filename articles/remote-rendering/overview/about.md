---
title: Hakkında
description: Azure uzaktan Işleme giriş
author: florianborn71
ms.author: flborn
ms.date: 02/05/2020
ms.topic: overview
ms.openlocfilehash: a06c63152cb56be6d94cccc472d2e1d65651d6ce
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80679957"
---
# <a name="about-azure-remote-rendering"></a>Azure Remote Rendering hakkında

> [!IMPORTANT]
> **Azure uzaktan işleme** Şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

*Azure uzaktan işleme* (ARR), bulutta yüksek kaliteli ve Etkileşimli 3B içerik işlemenizi ve bunu HoloLens 2 gibi cihazlara gerçek zamanlı olarak göndermenizi sağlayan bir hizmettir.

![Örnek model](../media/arr-engine.png)

Untethered cihazlar karmaşık modelleri işlemek için sınırlı hesaplama gücüne sahiptir. Birçok uygulama için, bu kabul edilemez, ancak her türlü şekilde görsel uygunluğu azaltır.

*Uzaktan işleme* , işleme iş yükünü buluttaki yüksek kaliteli GPU 'lara taşıyarak bu sorunu çözer. Bulutta barındırılan bir grafik Altyapısı görüntüyü işler, video akışı olarak kodlar ve hedef cihaza akışlar.

## <a name="hybrid-rendering"></a>Karma işleme

Çoğu uygulamada, yalnızca karmaşık bir modeli işlemek yeterli değildir. Kullanıcıya işlevsellik sağlamak için özel kullanıcı arabirimine de ihtiyacınız vardır. Azure uzaktan Işleme, *karma işlemeyi*desteklemek yerine ADANMıŞ bir UI çerçevesini kullanmaya zorlamaz. Bu,, [mrtk](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/GettingStartedWithTheMRTK.html)gibi tercih ettiğiniz yöntemi kullanarak cihazdaki öğeleri işleyebileceğiniz anlamına gelir.

Bir çerçevenin sonunda Azure uzaktan Işleme, yerel olarak işlenmiş içeriğinizi uzak görüntüyle otomatik olarak birleştirir. Bu, doğru occluson ile de yapabiliyor.

## <a name="multi-gpu-rendering"></a>Çok GPU işleme

Bazı modeller, yüksek kaliteli bir GPU için bile etkileşimli çerçeve ücretleri üzerinde oluşturulamayacak kadar karmaşıktır. Özellikle endüstriyel görselleştirmede bu, yaygın bir sorundur. Limitleri daha fazla göndermek için Azure uzaktan Işleme iş yükünü birden fazla GPU 'ya dağıtabilir. Sonuçlar tek bir görüntüde birleştirilir ve bu işlem, Kullanıcı için tamamen saydam hale getirir.

## <a name="high-level-architecture"></a>Üst düzey mimari

Bu diyagramda, uzaktan işleme mimarisi gösterilmektedir:

![Mimari](./media/arr-high-level-architecture.png)

Görüntü oluşturma için tam bir döngüyle aşağıdaki adımlar yer verilmiştir:

1. İstemci tarafı: çerçeve kurulumu
    1. Kodunuz: Kullanıcı girişi işlendi, sahne grafiği güncelleştirilir
    1. ARR Code: sahne grafiği güncelleştirmeleri ve tahmin edilen baş poz sunucuya gönderilir
1. Sunucu tarafı: Uzaktan işleme
    1. İşleme altyapısı, kullanılabilir GPU 'larda işlemeyi dağıtır
    1. Birden çok GPU 'dan alınan çıkış tek bir görüntüye alınır
    1. Görüntü video akışı olarak kodlanır, istemciye geri gönderilir
1. İstemci tarafı: sonlandırma
    1. Kodunuz: Isteğe bağlı yerel içerik (UI, işaretçiler,...) işlenir
    1. ARR Code: ' Sun ' üzerinde yerel olarak işlenmiş içerik, video akışıyla otomatik olarak birleştirilir

Ağ gecikmesi ana sorundur. İstek gönderme ve sonucu alma arasındaki yeniden etkinleştirme süresi genellikle etkileşimli çerçeve ücretleri için çok uzun. Bu nedenle, herhangi bir anda birden çok çerçeve uçuş durumunda olabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Sistem gereksinimleri](system-requirements.md)
* [Hızlı başlangıç: Unity ile model Işleme](../quickstarts/render-model.md)
