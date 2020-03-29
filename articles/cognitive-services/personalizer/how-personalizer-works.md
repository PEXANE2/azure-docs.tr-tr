---
title: Personalizer Nasıl Çalışır - Personalizer
description: Personalizer _döngüsü,_ içeriğiniz için en iyi eylemi öngören modeli oluşturmak için makine öğrenimini kullanır. Model, yalnızca Rank ve Reward çağrılarıyla gönderdiğiniz verilerinizde eğitilir.
ms.topic: conceptual
ms.date: 02/18/2020
ms.openlocfilehash: 836c207213ac52a60e27da6fc957418187059023
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77623770"
---
# <a name="how-personalizer-works"></a>Kişiselleştirme nasıl çalışır?

Personalizer kaynak, _öğrenme döngüsü_, içeriğiniz için üst eylem tahmin modeli oluşturmak için makine öğrenme kullanır. Model, yalnızca **Rank** ve **Reward** çağrılarıyla gönderdiğiniz verilerinizde eğitilir. Her döngü birbirinden tamamen bağımsızdır.

## <a name="rank-and-reward-apis-impact-the-model"></a>Sıralama ve Ödül API'leri modeli etkiler

Özellikler ve bağlam _özellikleri_ _içeren eylemleri_ Rank API'sine gönderirsiniz. **Rank** API'si aşağıdakilerden birini kullanmaya karar verir:

* _Exploit_: Geçmiş verilere dayalı olarak en iyi eylemi karara karar vermek için geçerli model.
* _Keşfet_: Üst eylem yerine farklı bir eylem seçin. Bu yüzdeyi Azure portalındaki Personalizer kaynağınız için [yapılandırırsınız.](how-to-settings.md#configure-exploration-to-allow-the-learning-loop-to-adapt)

Ödül puanını belirleyin ve bu puanı Ödül API'sine gönderin. **Ödül** API:

* Her rütbe çağrısının özelliklerini ve ödül puanlarını kaydederek modeli eğitmek için veri toplar.
* _Bu verileri, Öğrenme İlkesi'nde_belirtilen yapılandırmayı temel alan modeli güncelleştirmek için kullanır.

## <a name="your-system-calling-personalizer"></a>Sisteminiz Personalizer'ı arıyor

Aşağıdaki resim, Rank ve Reward çağrılarını çağırmanın mimari akışını gösterir:

![alternatif metin](./media/how-personalizer-works/personalization-how-it-works.png "Kişiselleştirme Nasıl Çalışır?")

1. Özellikler ve bağlam _özellikleri_ _içeren eylemleri_ Rank API'sine gönderirsiniz.

    * Personalizer, geçerli modelden yararlanıp yararlanmamaya veya model için yeni seçenekler keşfetmeye karar verir.
    * Sıralama sonucu EventHub'a gönderilir.
1. Üst rütbe _ödül eylem kimliği_olarak sisteminize döndürülür.
    Sisteminiz bu içeriği sunar ve kendi iş kurallarınıza göre bir ödül puanı belirler.
1. Sisteminiz ödül puanını öğrenme döngüsüne döndürür.
    * Personalizer ödülü aldığında, ödül EventHub'a gönderilir.
    * Rütbe ve ödül ilişkilidir.
    * AI modeli korelasyon sonuçlarına göre güncelleştirilir.
    * Çıkarım motoru yeni modelle güncelleştirilir.

## <a name="personalizer-retrains-your-model"></a>Personalizer modelinizi yeniler

Personalizer, Azure portalındaki Personalizer kaynağınızdaki **Model sıklık güncelleştirme** ayarınızı temel alınarak modelinizi yeniden yeniler.

Personalizer, Azure portalındaki Personalizer kaynağınızdaki gün sayısındaki **Veri saklama** ayarına bağlı olarak, şu anda tutulan tüm verileri kullanır.

## <a name="research-behind-personalizer"></a>Personalizer arkasında Araştırma

Personalizer, Microsoft Research'te makaleler, araştırma faaliyetleri ve devam eden araştırma alanları da dahil olmak üzere [Güçlendirme Öğrenimi](concepts-reinforcement-learning.md) alanında en ileri bilim ve araştırmalara dayanmaktadır.

## <a name="next-steps"></a>Sonraki adımlar

Personalizer için [en iyi senaryolar](where-can-you-use-personalizer.md) hakkında bilgi edinin