---
title: Kişiselleştirici nasıl çalıştığı-kişiselleştirici
description: Kişiselleştirici _döngüsü_ , içeriğiniz için en iyi eylemi tahmin eden modeli oluşturmak için makine öğrenimini kullanır. Model, bu verilere yalnızca derecelendirme ve ödül çağrılarında gönderdiğiniz verilerinize göre eğitilir.
ms.topic: conceptual
ms.date: 02/18/2020
ms.openlocfilehash: 836c207213ac52a60e27da6fc957418187059023
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "77623770"
---
# <a name="how-personalizer-works"></a>Kişiselleştirme nasıl çalışır?

_Öğrenme döngünüz_olan kişiselleştirici kaynağı, içeriğiniz için en iyi eylemi tahmin eden modeli oluşturmak için makine öğrenimi 'ni kullanır. Model, bu verilere yalnızca **Derecelendirme** ve **ödül** çağrılarında gönderdiğiniz verilerinize göre eğitilir. Her döngü birbirleriyle tamamen bağımsızdır.

## <a name="rank-and-reward-apis-impact-the-model"></a>Derecelendirme ve Reward API 'Leri modeli etkiler

Özellikler ve _bağlam özellikleriyle_ _işlemleri_ derecelendirme API 'sine gönderirsiniz. **Derecelendirme** API 'si şu iki seçenekten birini kullanmanıza karar verir:

* _Yararlanma_: geçmiş verilere göre en iyi eyleme karar vermeye yönelik geçerli model.
* _Keşfet_: en üstteki eylem yerine farklı bir eylem seçin. [Bu yüzdeyi](how-to-settings.md#configure-exploration-to-allow-the-learning-loop-to-adapt) , Azure Portal, kişiselleştirici kaynağınız için yapılandırırsınız.

Ödül Puanını belirlersiniz ve bu puanı ödül API 'sine gönderirsiniz. **Reward** API 'si:

* Her bir derece çağrısının özelliklerini ve yeniden puanlarını kaydederek modeli eğitmek için veri toplar.
* , _Öğrenme ilkesinde_belirtilen yapılandırmaya göre modeli güncelleştirmek için bu verileri kullanır.

## <a name="your-system-calling-personalizer"></a>Kişiselleştirici çağıran sistem

Aşağıdaki görüntüde, derece ve geri aramaları çağırmanın mimari akışı gösterilmektedir:

![alternatif metin](./media/how-personalizer-works/personalization-how-it-works.png "Kişiselleştirmenin çalışması")

1. Özellikler ve _bağlam özellikleriyle_ _işlemleri_ derecelendirme API 'sine gönderirsiniz.

    * Kişiselleştirici, geçerli modelden mi yararlanacağına yoksa model için yeni seçimler keşfetmesine karar verir.
    * Derecelendirme sonucu EventHub öğesine gönderilir.
1. En üst sıra, sisteminize geri dönüş _eylem kimliği_olarak döndürülür.
    Sisteminiz bu içeriği gösterir ve kendi iş kurallarınızı temel alarak bir ödül puanı belirler.
1. Sisteminiz öğrenme döngüsüne geri dönüş puanı döndürür.
    * Kişiselleştirmede bir ödül alındığında, bu, EventHub öğesine gönderilir.
    * Rank ve ödül, bağıntılı.
    * AI modeli, bağıntı sonuçlarına göre güncelleştirilir.
    * Çıkarım altyapısı yeni modelle güncellenir.

## <a name="personalizer-retrains-your-model"></a>Kişiselleştirici, modelinize geri çekme

Kişiselleştirici, Azure portal kişiselleştirici kaynağınız üzerindeki **model sıklığı güncelleştirme** ayarınızı temel alarak modelinize geri çeker.

Kişiselleştirici, Azure portal, kişiselleştirme kaynağınızın, **veri saklama** ayarına göre şu anda korunan tüm verileri kullanır.

## <a name="research-behind-personalizer"></a>Kişiselleştirici arkasında araştırma

Kişiselleştirici, Microsoft Research 'daki incelemeler, araştırma etkinlikleri ve devam eden araştırma dahil olmak üzere [pekiştirmeye dayalı Learning](concepts-reinforcement-learning.md) 'in alanında son teknoloji bilimine ve araştırmasına dayalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Kişiselleştiriciye yönelik [popüler senaryolar](where-can-you-use-personalizer.md) hakkında bilgi edinin