---
title: Öğrenme ilkesi-kişiselleştirici
description: Öğrenme ayarları, model eğitiminin *hiper parametrelerini* belirlenir. Farklı öğrenme ayarları üzerinde eğitilen aynı verilerin iki modeli farklı olacaktır.
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 02/20/2020
ms.openlocfilehash: 1d2038e3796df843736eb80f7e5645f4141c82f4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91253641"
---
# <a name="learning-policy-and-settings"></a>Öğrenme ilkesi ve ayarları

Öğrenme ayarları, model eğitiminin *hiper parametrelerini* belirlenir. Farklı öğrenme ayarları üzerinde eğitilen aynı verilerin iki modeli farklı olacaktır.

[Öğrenme ilkesi ve ayarları](how-to-settings.md#configure-rewards-for-the-feedback-loop) Azure Portal, kişiselleştirici kaynağınız üzerinde ayarlanır.

## <a name="import-and-export-learning-policies"></a>Öğrenme ilkelerini içeri ve dışarı aktarma

Azure portal öğrenme ilkesi dosyalarını içeri ve dışarı aktarabilirsiniz. Mevcut ilkeleri kaydetmek, test etmek, değiştirmek ve bunları kaynak kodu denetiinizde daha sonra başvurmak ve denetlemek için yapıtlar olarak arşivlemek için bu yöntemi kullanın.

Kişiselleştirici kaynağınız için Azure portal bir öğrenme [ilkesini içeri ve dışarı aktarmayı öğrenin](how-to-manage-model.md#import-a-new-learning-policy) .

## <a name="understand-learning-policy-settings"></a>Öğrenme ilkesi ayarlarını anlama

Öğrenme ilkesindeki ayarların değiştirilmesi amaçlanmamaktadır. Ayarları yalnızca, kişiselleştirici tarafından nasıl etkilendiklerini anladıysanız değiştirin. Bu bilgi olmadan, geçersiz kılma için kişiselleştirici modeller dahil olmak üzere sorunlara neden olabilirsiniz.

Kişiselleştirici olayları eğitme ve Puanlama için [vowpalwabbit](https://github.com/VowpalWabbit) kullanır. Vowpalwabbit kullanarak öğrenme ayarlarını düzenleme hakkındaki [vowpalwabbit belgelerine](https://github.com/VowpalWabbit/vowpal_wabbit/wiki/Command-line-arguments) bakın. Doğru komut satırı bağımsız değişkenlerine sahip olduktan sonra, komutu aşağıdaki biçimde bir dosyaya kaydedin (bağımsız değişkenler özellik değerini istenen komutla değiştirin) ve kişiselleştirme kaynağı için Azure portal **model ve öğrenme ayarları** bölmesinde öğrenme ayarlarını içeri aktarmak üzere dosyayı karşıya yükleyin.

Aşağıda `.json` bir öğrenme ilkesi örneği verilmiştir.

```json
{
  "name": "new learning settings",
  "arguments": " --cb_explore_adf --epsilon 0.2 --power_t 0 -l 0.001 --cb_type mtr -q ::"
}
```

## <a name="compare-learning-policies"></a>Öğrenme ilkelerini karşılaştırın

[Çevrimdışı değerlendirmeler](concepts-offline-evaluation.md)yaparak, farklı öğrenme Ilkelerinin kişiselleştirici günlüklerinde geçmiş verilere karşı nasıl gerçekleştirileceğini karşılaştırabilirsiniz.

Geçerli öğrenme ilkesiyle karşılaştırmak için [kendi öğrenme Ilkelerinizi karşıya yükleyin](how-to-manage-model.md) .

## <a name="optimize-learning-policies"></a>Öğrenme ilkelerini iyileştirme

Kişiselleştirici, [çevrimdışı bir değerlendirmede](how-to-offline-evaluation.md)iyileştirilmiş bir öğrenme ilkesi oluşturabilir. Çevrimdışı değerlendirmede daha iyi bir değerlendirme olan iyileştirilmiş bir öğrenme ilkesi, kişiselleştirmede çevrimiçi olarak kullanıldığında daha iyi sonuçlar verir.

Bir öğrenme ilkesini iyileştirdikten sonra, geçerli ilkenin hemen yerini alacak şekilde doğrudan Kişiselleştiriciye uygulayabilirsiniz. Ayrıca, daha fazla değerlendirme için iyileştirilmiş ilkeyi kaydedebilir ve daha sonra atma, kaydetme veya uygulama seçeneklerinden hangisi olduğuna karar verebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Etkin ve etkin olmayan olayları](concept-active-inactive-events.md)öğrenin.
