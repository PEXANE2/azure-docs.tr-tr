---
title: Öğrenme politikası - Personalizer
description: Öğrenme ayarları, model eğitiminin *hiperparametrelerini* belirler. Farklı öğrenme ayarlarında eğitilmiş aynı verilerin iki modeli farklı sona erer.
ms.topic: conceptual
ms.date: 02/20/2020
ms.openlocfilehash: abe6a2a2ec9b9978230d894c69193469f6e932e6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79219351"
---
# <a name="learning-policy-and-settings"></a>Öğrenme politikası ve ayarları

Öğrenme ayarları, model eğitiminin *hiperparametrelerini* belirler. Farklı öğrenme ayarlarında eğitilmiş aynı verilerin iki modeli farklı sona erer.

[Öğrenme ilkesi ve ayarları](how-to-settings.md#configure-rewards-for-the-feedback-loop) Azure portalındaki Personalizer kaynağınızda ayarlanır.

## <a name="import-and-export-learning-policies"></a>Öğrenme politikaları ithalat ve ihracat

Azure portalından öğrenme ilkesi dosyalarını içe aktarAbilir ve dışa aktarabilirsiniz. Varolan ilkeleri kaydetmek, test etmek, değiştirmek ve kaynak kod denetiminizde gelecekteki başvuru ve denetim yapıları olarak arşivlemek için bu yöntemi kullanın.

Personalizer kaynağınız için Azure portalında bir öğrenme ilkesini nasıl içe [aktarıp](how-to-manage-model.md#import-a-new-learning-policy) dışa aktarırım öğrenin.

## <a name="understand-learning-policy-settings"></a>Öğrenme politikası ayarlarını anlama

Öğrenme ilkesindeki ayarların değiştirilmesi amaçlanmamıştır. Ayarları yalnızca Personalizer'ı nasıl etkilediğini anlarsanız değiştirin. Bu bilgi olmadan, Personalizer modelleri geçersiz kılma da dahil olmak üzere sorunlara neden olabilir.

Personalizer eğitmek ve olayları puan [vowpalwabbit](https://github.com/VowpalWabbit) kullanır. Vowpalwabbit kullanarak öğrenme ayarlarının nasıl değiştirilebildiğini anlatan [vowpalwabbit belgelerine](https://github.com/VowpalWabbit/vowpal_wabbit/wiki/Command-line-arguments) bakın. Doğru komut satırı bağımsız değişkenlerini aldıktan sonra, komutu aşağıdaki biçime sahip bir dosyaya kaydedin (bağımsız değişkenlerin özellik değerini istenen komutla değiştirin) ve Dosyayı Azure portalındaki **Model ve Öğrenme Ayarları** bölmesinde Kilikçi kaynağınız için öğrenme ayarlarını almak için yükleyin.

Aşağıda `.json` bir öğrenme ilkesi örneği verilmiştir.

```json
{
  "name": "new learning settings",
  "arguments": " --cb_explore_adf --epsilon 0.2 --power_t 0 -l 0.001 --cb_type mtr -q ::"
}
```

## <a name="compare-learning-policies"></a>Öğrenme politikalarını karşılaştırın

Farklı öğrenme ilkelerinin kişisel günlüklerde geçmiş verilerle nasıl performans gösterdiğini [çevrimdışı değerlendirmeler](concepts-offline-evaluation.md)yaparak karşılaştırabilirsiniz.

Geçerli öğrenme politikasıyla karşılaştırmak için [kendi öğrenme politikanızı yükleyin.](how-to-manage-model.md)

## <a name="optimize-learning-policies"></a>Öğrenme politikalarını optimize edin

Personalizer [çevrimdışı bir değerlendirmede](how-to-offline-evaluation.md)optimize edilmiş bir öğrenme ilkesi oluşturabilir. Çevrimdışı bir değerlendirmede daha iyi ödüllere sahip optimize edilmiş bir öğrenme politikası, Personalizer'da çevrimiçi kullanıldığında daha iyi sonuçlar verecektir.

Bir öğrenme ilkesini optimize ettikten sonra, doğrudan Personalizer'a uygulayabilirsiniz, böylece geçerli politikanın hemen yerini alır. Veya daha fazla değerlendirme için optimize edilmiş ilkeyi kaydedebilir ve daha sonra atılıp atılmayacağınıza, kaydedip uygulamayacağınıza karar verebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Etkin ve etkin olmayan olayları](concept-active-inactive-events.md)öğrenin.
