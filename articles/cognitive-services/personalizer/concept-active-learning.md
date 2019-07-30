---
title: Etkin öğrenme-kişiselleştirici
titleSuffix: Azure Cognitive Services
description: ''
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: diberry
ms.openlocfilehash: 8c1579be3d11ae14ca45ee861de2d4f705e5d62c
ms.sourcegitcommit: e3b0fb00b27e6d2696acf0b73c6ba05b74efcd85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68663722"
---
# <a name="active-learning-and-learning-policies"></a>Etkin öğrenme ve öğrenme ilkeleri 

Uygulamanız, derecelendirme API 'sini çağırdığında içeriğin bir derecesini alırsınız. İş mantığı, içeriğin kullanıcıya gösterilmesi gerekip gerekmediğini öğrenmek için bu derecelendirmeyi kullanabilir. Derecelendirilen içeriği görüntülediğinizde, bu _etkin_ bir sıra olayıdır. Uygulamanız, _etkin_ olmayan bir sıra olayıdır. 

Etkin sıra olay bilgileri Kişiselleştiriciye döndürülür. Bu bilgiler, geçerli öğrenme ilkesi aracılığıyla modele eğitim etmeye devam etmek için kullanılır.

## <a name="active-events"></a>Etkin olaylar

Etkin olaylar her zaman kullanıcıya gösterilmeli ve öğrenme döngüsünü kapatmak için yeniden çağrı döndürülmelidir. 

### <a name="inactive-events"></a>Etkin olmayan olaylar 

Etkin olmayan olaylar, kullanıcıya derecelendirilen içerikten seçim yapma şansı verilmediğinden temeldeki modeli değiştirmemelidir.

## <a name="dont-train-with-inactive-rank-events"></a>Etkin olmayan derecelendirme olaylarıyla eğmeyin 

Bazı uygulamalarda, uygulamanızın sonuçları kullanıcıya görüntülemesi durumunda, henüz farkında olmadan sıralama API 'sini çağırmanız gerekebilir. 

Bunun nedeni:

* Kullanıcının görebiliyorsa veya görmeyebilir bir kullanıcı ARABIRIMI önceden oluşturuluyor olabilir. 
* Uygulamanız, daha az gerçek zamanlı bağlamla derecelendirme çağrılarının yapıldığı ve bunların çıktıları uygulama tarafından kullanılabilecek tahmine dayalı kişiselleştirmeye başlayabilir. 

### <a name="disable-active-learning-for-inactive-rank-events-during-rank-call"></a>Derecelendirme çağrısı sırasında etkin olmayan derecelendirme olayları için etkin öğrenmeyi devre dışı bırak

Otomatik öğrenmeyi devre dışı bırakmak için ile birlikte `learningEnabled = False`derece çağırın.

Etkin olmayan bir olay için öğrenme, derece için bir ödül gönderirseniz örtülü olarak etkinleştirilir.

## <a name="learning-policies"></a>Öğrenme ilkeleri

Öğrenme ilkesi, model eğitiminin belirli *hiper parametrelerini* belirler. Farklı öğrenme ilkelerine eğitilen aynı verilerin iki modeli farklı davranır.

### <a name="importing-and-exporting-learning-policies"></a>Öğrenme Ilkelerini içeri ve dışarı aktarma

Azure portal öğrenme ilkesi dosyalarını içeri ve dışarı aktarabilirsiniz. Bu, mevcut ilkeleri kaydetmenize, test etmenize, bunları değiştirmenize ve bunları kaynak kodu denetiinizde daha sonra başvuru ve denetim için yapıtlar olarak arşivlemenize olanak tanır.

### <a name="learning-policy-settings"></a>Öğrenme ilkesi ayarları

**Öğrenme ilkesindeki** ayarların değiştirilmesi amaçlanmamaktadır. Yalnızca kişiselleştirmeden nasıl etkilendikleri hakkında bilgi alırken ayarları değiştirin. Ayarları bu bilgi olmadan değiştirmek, geçersiz kılma için kişiselleştirici modeller dahil olmak üzere yan etkilere neden olur.

### <a name="comparing-effectiveness-of-learning-policies"></a>Öğrenme ilkelerinin verimliliğini karşılaştırma

Farklı öğrenme Ilkelerinin, [çevrimdışı değerlendirmeler](concepts-offline-evaluation.md)gerçekleştirerek, kişiselleştirici günlüklerinde geçmiş verilere karşı nasıl gerçekleştirileceğini karşılaştırabilirsiniz.

Geçerli öğrenme ilkesiyle karşılaştırmak için [kendi öğrenme Ilkelerinizi karşıya yükleyin](how-to-offline-evaluation.md) .

### <a name="discovery-of-optimized-learning-policies"></a>İyileştirilmiş öğrenme ilkelerinin keşfi

Kişiselleştirici, [çevrimdışı bir değerlendirme](how-to-offline-evaluation.md)yaparken daha iyileştirilmiş bir öğrenme ilkesi oluşturabilir. Çevrimdışı bir değerlendirmede daha iyi bir performans elde etmek için gösterilen daha iyileştirilmiş bir öğrenme ilkesi, kişiselleştirmede çevrimiçi olarak kullanıldığında daha iyi sonuçlar verir.

İyileştirilmiş bir öğrenme ilkesi oluşturulduktan sonra, geçerli ilkenin hemen yerini alacak şekilde doğrudan kişiselleştirmeye uygulayabilir ya da daha fazla değerlendirme için kaydedebilir ve gelecekte atma, kaydetme veya uygulama hakkında daha sonra karar verebilirsiniz.