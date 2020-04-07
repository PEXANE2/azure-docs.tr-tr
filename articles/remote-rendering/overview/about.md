---
title: Hakkında
description: Azure Uzaktan İşleme'ye Giriş
author: florianborn71
ms.author: flborn
ms.date: 02/05/2020
ms.topic: overview
ms.openlocfilehash: a06c63152cb56be6d94cccc472d2e1d65651d6ce
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679957"
---
# <a name="about-azure-remote-rendering"></a>Azure Uzaktan İşleme Hakkında

> [!IMPORTANT]
> **Azure Uzaktan İşleme** şu anda genel önizlemede.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

*Azure Uzaktan İşleme* (ARR), bulutta yüksek kaliteli, etkileşimli 3D içerik oluşturmanıza ve HoloLens 2 gibi cihazlara gerçek zamanlı olarak aktarmanızı sağlayan bir hizmettir.

![Örnek model](../media/arr-engine.png)

Teketekli aygıtlar karmaşık modelleri işlemek için sınırlı hesaplama gücüne sahiptir. Birçok uygulama için kabul edilemez olsa da, herhangi bir şekilde görsel sadakat azaltmak olacaktır.

*Uzaktan İşleme,* render iş yükünü buluttaki üst düzey GPU'lara taşıyarak bu sorunu çözer. Bulut tarafından barındırılan grafik motoru görüntüyü işler, video akışı olarak kodlar ve bunu hedef aygıta aktarAr.

## <a name="hybrid-rendering"></a>Karma görüntüleme

Çoğu uygulamada, yalnızca karmaşık bir modeli işlemek için yeterli değildir. Ayrıca, kullanıcıya işlevsellik sağlamak için özel kullanıcı arama aracı gerekir. Azure Uzaktan İşleme, sizi özel bir bilgisayar birimi çerçevesi kullanmaya zorlamaz, bunun yerine *Karma Oluşturma'yı*destekler. Bu, [MRTK](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/GettingStartedWithTheMRTK.html)gibi tercih ettiğiniz yöntemi kullanarak öğeleri aygıtta işleyebilirsiniz anlamına gelir.

Bir çerçevenin sonunda, Azure Uzaktan İşleme daha sonra yerel olarak işlenen içeriğiuzaktan görüntüyle otomatik olarak birleştirir. Hatta doğru tıkanıklık ile bunu yapabiliyor.

## <a name="multi-gpu-rendering"></a>Çoklu GPU işleme

Bazı modeller, üst düzey bir GPU için bile etkileşimli kare hızlarında işlemek için çok karmaşıktır. Özellikle endüstriyel görselleştirmede bu yaygın bir sorundur. Azure Uzaktan İşlem, sınırları daha da zorlamak için iş yükünü birden çok GPU'ya dağıtabilir. Sonuçlar tek bir görüntüde birleştirilerek işlem kullanıcı için tamamen saydam hale gelir.

## <a name="high-level-architecture"></a>Üst düzey mimari

Bu diyagram, uzaktan işleme mimarisini gösterir:

![Mimari](./media/arr-high-level-architecture.png)

Görüntü oluşturma için tam bir döngü aşağıdaki adımları içerir:

1. İstemci tarafı: Çerçeve kurulumu
    1. Kodunuz: Kullanıcı girişi işlenir, sahne grafiği güncellenir
    1. ARR kodu: Sahne grafiği güncellemeleri ve öngörülen kafa pozu sunucuya gönderilir
1. Sunucu tarafı: Uzaktan görüntüleme
    1. Rendering motoru, işleme yi kullanılabilir GPU'lar arasında dağıtır
    1. Birden fazla GPU'dan elde edilen çıktı tek bir görüntüde oluşur
    1. Görüntü video akışı olarak kodlanır, istemciye geri gönderilir
1. İstemci tarafı: Sonlandırma
    1. Kodunuz: İsteğe bağlı yerel içerik (Kullanıcı GI, işaretçiler, ...) işlenir
    1. ARR kodu: 'present'da, yerel olarak işlenen içerik video akışıyla otomatik olarak birleştirilir

Ağ gecikmesi ana sorundur. İstek gönderme ile sonucu alma arasındaki dönüş süresi genellikle etkileşimli kare hızları için çok uzundur. Bu nedenle birden fazla kare herhangi bir zamanda uçuş olabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Sistem gereksinimleri](system-requirements.md)
* [Quickstart: Unity ile bir model oluşturma](../quickstarts/render-model.md)
