---
title: Pekiştirme Öğrenme - Personalizer
titleSuffix: Azure Cognitive Services
description: Personalizer, daha iyi sıralama önerileri yapmak için eylemler ve geçerli bağlam hakkındaki bilgileri kullanır. Bu eylemler ve bağlam hakkındaki bilgiler, özellik olarak adlandırılan öznitelikler veya özelliklerdir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 36071cdee25cfa99fc54b0e5c0c0aa822cb5fe2f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68662831"
---
# <a name="what-is-reinforcement-learning"></a>Pekiştirme Öğrenme Nedir?

Pekiştirme Öğrenme, kullanımından geri bildirim alarak davranışları öğrenen makine öğrenimine yönelik bir yaklaşımdır.
 
Reinforcement Öğrenme çalışmaları:

* Bir davranışı yürürlüğe koymak için bir fırsat veya özgürlük derecesi sağlamak - karar vermek veya seçimler yapmak gibi.
* Çevre ve seçimler hakkında bağlamsal bilgi sağlamak.
* Davranışın belirli bir amaca ne kadar iyi ulaştığı hakkında geri bildirim sağlamak.

Pekiştirme öğrenmenin birçok alt türü ve stili olsa da, Personalizer'da konsept şu şekilde çalışır:

* Uygulamanız, bir alternatif listesinden bir içerik parçasını gösterme olanağı sağlar.
* Uygulamanız her alternatif ve kullanıcının bağlamı hakkında bilgi sağlar.
* Başvurunuz bir _ödül puanı_hesaplar.

Pekiştirme öğrenme için bazı yaklaşımlar aksine, Personalizer çalışmak için bir simülasyon gerektirmez. Onun öğrenme algoritmaları bir dış dünya (karşı kontrol) tepki ve oluşturmak için zaman ve para maliyet benzersiz bir fırsat olduğunu ve sıfır olmayan bir pişmanlık (olası ödül kaybı) olduğunu bir anlayış ile her veri noktasından öğrenmek için tasarlanmıştır suboptimal performans olur.

## <a name="what-type-of-reinforcement-learning-algorithms-does-personalizer-use"></a>Personalizer ne tür takviye öğrenme algoritmaları kullanır?

Personalizer mevcut sürümü **bağlamsal haydutlar**kullanır , belirli bir bağlamda, karar veya ayrık eylemler arasında seçimler yapma etrafında çerçeveli öğrenme güçlendirme için bir yaklaşım.

_Karar belleği,_ bir bağlam verilen, mümkün olan en iyi kararı yakalamak için eğitilmiş model, doğrusal modeller kümesi kullanır. Bunlar tekrar tekrar iş sonuçları göstermiştir ve kanıtlanmış bir yaklaşım, kısmen çünkü çok geçişli eğitim gerek kalmadan çok hızlı bir şekilde gerçek dünyadan öğrenebilirsiniz, ve kısmen çünkü denetimli öğrenme modelleri ve derin nöral tamamlayabilir ağ modelleri.

Keşif/yararlanma trafik ayırması, keşif için ayarlanan yüzdeyi takiben rasgele yapılır ve keşif için varsayılan algoritma epsilon-açgözlüdür.

### <a name="history-of-contextual-bandits"></a>Bağlamsal Haydutların Tarihi

John Langford, bağlamsal Haydutlar (Langford ve Zhang [2007]) adını güçlendirme öğrenmenin ölçülebilir bir alt kümesini tanımlamak için icat etti ve bu paradigmada nasıl öğrenilir anlayışımızı geliştiren yarım düzine makale üzerinde çalıştı:

* Beygelzimer ve ark. [2011]
* Dudík ve ark. [2011a,b]
* Agarwal ve ark. [2014, 2012]
* Beygelzimer ve Langford [2009]
* Li ve ark. [2010]

John ayrıca daha önce Joint Prediction (ICML 2015), Bağlamsal Haydut Teorisi (NIPS 2013), Active Learning (ICML 2009) ve Örnek Karmaşıklık Sınırları (ICML 2003) gibi konularda çeşitli eğitimler vermiştir.

## <a name="what-machine-learning-frameworks-does-personalizer-use"></a>Personalizer hangi makine öğrenimi çerçevelerini kullanır?

Personalizer şu anda machine learning'in temeli olarak [Vowpal Wabbit'i](https://github.com/VowpalWabbit/vowpal_wabbit/wiki) kullanmektedir. Bu çerçeve, kişiselleştirme sıralarını yaparken ve modeli tüm etkinliklerle eğitirken maksimum verim ve en düşük gecikmeye olanak sağlar.

## <a name="references"></a>Başvurular

* [Düşük Teknik Borçla Bağlamsal Kararlar Alma](https://arxiv.org/abs/1606.03966)
* [Adil Sınıflandırmaya İndirim Yaklaşımı](https://arxiv.org/abs/1803.02453)
* [Sabit Olmayan Dünyalarda Etkin Bağlamsal Haydutlar](https://arxiv.org/abs/1708.01799)
* [Kalıntı Kayıp Tahmini: Takviye: Hiçbir Artımlı Geribildirim ile öğrenme](https://openreview.net/pdf?id=HJNMYceCW)
* [Güçlendirme Öğrenme ile Eylemler için Talimatlar ve Görsel Gözlemler Haritalama](https://arxiv.org/abs/1704.08795)
* [Öğretmeninizden Daha İyi Arama Yapmayı Öğrenme](https://arxiv.org/abs/1502.02206)

## <a name="next-steps"></a>Sonraki adımlar

[Çevrimdışı değerlendirme](concepts-offline-evaluation.md) 