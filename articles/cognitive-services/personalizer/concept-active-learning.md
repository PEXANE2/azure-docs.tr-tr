---
title: Etkin ve etkin olmayan olaylar-kişiselleştirici
titleSuffix: Azure Cognitive Services
description: Bu makalede, kişiselleştirici hizmeti içinde etkin ve etkin olmayan olayların, öğrenme ayarlarının ve öğrenme ilkelerinin kullanımı ele alınmaktadır.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: diberry
ms.openlocfilehash: 90658e030c907a9fd99dd8fb9a6e90698d72b1f0
ms.sourcegitcommit: f53cd24ca41e878b411d7787bd8aa911da4bc4ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75834472"
---
# <a name="active-and-inactive-events"></a>Etkin olan ve olmayan olaylar

Uygulamanız, derecelendirme API 'sini çağırdığında, uygulamanın **Rewarterctionıd** alanında göstermesi gereken eylemi alırsınız.  Bu andan itibaren, kişiselleştirici aynı EventID 'ye sahip bir yeniden çağrı bekliyor. Ödül puanı, modeli gelecekteki derece çağrılar için eğitebilmek üzere kullanılacaktır. EventID için bir ödül çağrısı alınmıyorsa, varsayılan bir yeniden eşleşme uygulanır. Varsayılan yeniden ödüller Azure portal ayarlanır.

Bazı senaryolarda, uygulamanın, sonucun ne şekilde kullanılacağını veya kullanıcıya görüntülendiğini bilse bile, uygulamanın sırasıyla derece çağrısı yapması gerekebilir. Bu durum, örneğin, yükseltilen içeriğin sayfa işleme bir pazarlama kampanyası tarafından üzerine yazılmasından kaynaklanıyor olabilir. Sıra çağrısının sonucu hiç kullanılmadıysa ve Kullanıcı onu hiç görmüyorsa, karşılık gelen bir ödül çağrısı göndermeyin.

Genellikle, şu durumlarda bu senaryolar meydana gelir:

* Kullanıcının görebileceğini veya görmeyebilirsiniz prerendering kullanıcı ARABIRIMI.
* Uygulamanız, çok sayıda gerçek zamanlı bağlamla derecelendirme çağrılarının yapıldığı ve uygulamanın çıktıyı kullanmasının veya kullanmayabilir.

Bu durumlarda, etkinliğin _devre dışı_olmasını isteyen derece çağrısı yapmak Için kişiselleştirici kullanın. Kişiselleştirici bu olay için bir ödül beklemez ve varsayılan bir ödül uygulamaz.
İş mantığınızdaki daha sonra uygulama, sıralama çağrısından bilgileri kullanıyorsa, olayı _etkinleştirmeniz_ yeterlidir. Etkinlik etkin olduğunda, kişiselleştirici bir olay yeniden bekliyor. Reward API 'sine açık bir çağrı yapılmıyorsa, kişiselleştirici varsayılan bir ödül uygular.

## <a name="inactive-events"></a>Etkin olmayan olaylar

Bir olaya yönelik eğitimi devre dışı bırakmak için `learningEnabled = False`kullanarak derece çağırın. Etkin olmayan bir olay için, EventID için bir ödül gönderirseniz veya bu EventID için `activate` API 'sini çağırdığınızda öğrenme işlemi örtülü olarak etkinleştirilir.

## <a name="learning-settings"></a>Öğrenme ayarları

Öğrenme ayarları, model eğitiminin *hiper parametrelerini* belirlenir. Farklı öğrenme ayarları üzerinde eğitilen aynı verilerin iki modeli farklı olacaktır.

### <a name="import-and-export-learning-policies"></a>Öğrenme ilkelerini içeri ve dışarı aktarma

Azure portal öğrenme ilkesi dosyalarını içeri ve dışarı aktarabilirsiniz. Mevcut ilkeleri kaydetmek, test etmek, değiştirmek ve bunları kaynak kodu denetiinizde daha sonra başvurmak ve denetlemek için yapıtlar olarak arşivlemek için bu yöntemi kullanın.

Bir öğrenme [ilkesini içeri ve dışarı aktarmayı öğrenin](how-to-learning-policy.md) .

### <a name="understand-learning-policy-settings"></a>Öğrenme ilkesi ayarlarını anlama

Öğrenme ilkesindeki ayarların değiştirilmesi amaçlanmamaktadır. Ayarları yalnızca, kişiselleştirici tarafından nasıl etkilendiklerini anladıysanız değiştirin. Bu bilgi olmadan, geçersiz kılma için kişiselleştirici modeller dahil olmak üzere sorunlara neden olabilirsiniz.

Kişiselleştirici olayları eğitme ve Puanlama için [vowpalwabbit](https://github.com/VowpalWabbit) kullanır. Vowpalwabbit kullanarak öğrenme ayarlarını düzenleme hakkındaki [vowpalwabbit belgelerine](https://github.com/VowpalWabbit/vowpal_wabbit/wiki/Command-line-arguments) bakın. Doğru komut satırı bağımsız değişkenlerine sahip olduktan sonra, komutu aşağıdaki biçimde bir dosyaya kaydedin (bağımsız değişkenler özellik değerini istenen komutla değiştirin) ve kişiselleştirme kaynağı için Azure portal **model ve öğrenme ayarları** bölmesinde öğrenme ayarlarını içeri aktarmak üzere dosyayı karşıya yükleyin.

Aşağıdaki `.json` bir öğrenme ilkesi örneğidir.

```json
{
  "name": "new learning settings",
  "arguments": " --cb_explore_adf --epsilon 0.2 --power_t 0 -l 0.001 --cb_type mtr -q ::"
}
```

### <a name="compare-learning-policies"></a>Öğrenme ilkelerini karşılaştırın

[Çevrimdışı değerlendirmeler](concepts-offline-evaluation.md)yaparak, farklı öğrenme Ilkelerinin kişiselleştirici günlüklerinde geçmiş verilere karşı nasıl gerçekleştirileceğini karşılaştırabilirsiniz.

Geçerli öğrenme ilkesiyle karşılaştırmak için [kendi öğrenme Ilkelerinizi karşıya yükleyin](how-to-learning-policy.md) .

### <a name="optimize-learning-policies"></a>Öğrenme ilkelerini iyileştirme

Kişiselleştirici, [çevrimdışı bir değerlendirmede](how-to-offline-evaluation.md)iyileştirilmiş bir öğrenme ilkesi oluşturabilir. Çevrimdışı değerlendirmede daha iyi bir değerlendirme olan iyileştirilmiş bir öğrenme ilkesi, kişiselleştirmede çevrimiçi olarak kullanıldığında daha iyi sonuçlar verir.

Bir öğrenme ilkesini iyileştirdikten sonra, geçerli ilkenin hemen yerini alacak şekilde doğrudan Kişiselleştiriciye uygulayabilirsiniz. Ayrıca, daha fazla değerlendirme için iyileştirilmiş ilkeyi kaydedebilir ve daha sonra atma, kaydetme veya uygulama seçeneklerinden hangisi olduğuna karar verebilirsiniz.
