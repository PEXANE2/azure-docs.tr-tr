---
title: Hakkında
description: Azure uzaktan Işleme giriş
author: florianborn71
ms.author: flborn
ms.date: 02/05/2020
ms.topic: overview
ms.openlocfilehash: 6001fff54b86b7c18684092aad6c7fdf64f81990
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101729955"
---
# <a name="about-azure-remote-rendering"></a>Azure Remote Rendering hakkında

*Azure uzaktan işleme* (ARR), bulutta yüksek kaliteli ve Etkileşimli 3B içerik işlemenizi ve bunu HoloLens 2 gibi cihazlara gerçek zamanlı olarak göndermenizi sağlayan bir hizmettir.

![Oluşturulmuş yüksek kaliteli, etkileşimli 3D otomobil altyapısı örneği gösteren diyagram.](../media/arr-engine.png)

Untethered cihazlar karmaşık modelleri işlemek için sınırlı hesaplama gücüne sahiptir. Birçok uygulama için, bu kabul edilemez, ancak her türlü şekilde görsel uygunluğu azaltır. Aşağıdaki ekran görüntüsünde, model ortak bir içerik oluşturma aracı kullanılarak seçilmiş olan bir modelle tam ayrıntılarla karşılaştırılır:

![Örnek model](./media/engine-model-decimated.png)

Azaltılmış model yaklaşık 200.000 üçgenden oluşur (ayrıntılı iç parçalar dahil) ve özgün modeldeki 18.000.000 üçgenden fazla.

*Uzaktan işleme* , işleme iş yükünü buluttaki yüksek kaliteli GPU 'lara taşıyarak bu sorunu çözer. Bulutta barındırılan bir grafik Altyapısı görüntüyü işler, video akışı olarak kodlar ve hedef cihaza akışlar.

## <a name="hybrid-rendering"></a>Karma işleme

Çoğu uygulamada, yalnızca karmaşık bir modeli işlemek yeterli değildir. Kullanıcıya işlevsellik sağlamak için özel kullanıcı arabirimine de ihtiyacınız vardır. Azure uzaktan Işleme, *karma işlemeyi* desteklemek yerine ADANMıŞ bir UI çerçevesini kullanmaya zorlamaz. Bu,, [mrtk](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/GettingStartedWithTheMRTK.html)gibi tercih ettiğiniz yöntemi kullanarak cihazdaki öğeleri işleyebileceğiniz anlamına gelir.

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
