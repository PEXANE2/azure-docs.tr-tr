---
title: Ölçüm Danışmanı hizmetine anomali geri bildirimi sağlama
titleSuffix: Azure Cognitive Services
description: Ölçüm Danışmanı Örneğinizde bulunan bozukluklar hakkında nasıl geri bildirim gönderileceğini öğrenin ve sonuçları ayarlayın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: conceptual
ms.date: 09/10/2020
ms.author: aahi
ms.openlocfilehash: 50d422edf1a4b45132d0b86eac9d4947cef5e5bf
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941194"
---
# <a name="adjust-anomaly-detection-using-feedback"></a>Geri bildirimi kullanarak anomali algılamayı ayarlama

Ölçüm Izleyicisi tarafından sunulan anomali algılama sonuçlarının bir kısmını tatmin ediyorsanız, verilerinize uygulanan modeli etkileyecek geri bildirimleri el ile ekleyebilirsiniz. 

Geri bildirim ek açıklama modunu etkinleştirmek için sayfanın sağ üst köşesindeki düğmeleri kullanın.

:::image type="content" source="../media/feedback/annotation-mode.png" alt-text="Geri bildirim ek açıklaması modu.":::

Geri bildirim ek açıklama modu etkinleştirildikten sonra, bir nokta veya birden çok sürekli nokta için geri bildirimde bulunun.

## <a name="give-feedback-for-one-point"></a>Bir nokta için geri bildirimde bulunun 

Geri bildirim ek açıklaması modu etkinleştirildiğinde bir noktaya tıklayarak **geri bildirim Ekle** bölmesini açın. Uygulamak istediğiniz geri bildirim türünü ayarlayabilirsiniz. Bu geri bildirim, gelecekteki noktalara yönelik olarak algılamaya eklenecektir.  

* Noktanın ölçüm Izleyicisi tarafından yanlış etiketlenmişse **anomali** ' i seçin. Bir noktanın bir anomali olup olmayacağını belirtebilirsiniz. 
* Noktanın bir eğilim değişikliği başlangıcını olduğunu düşünüyorsanız **değişiklik noktası** ' nı seçin.
* Mevsimsellik belirtmek için **dönemi** seçin. Ölçüm Izleyicisi mevsimsellik aralıklarını otomatik olarak algılayabilir veya bunu el ile belirtebilirsiniz. 

**Yorum** metin kutusunda bir yorumu aynı anda bırakmayı düşünün ve geri bildiriminizi kaydetmek için **Kaydet** ' e tıklayın.

:::image type="content" source="../media/feedback/feedback-menu.png" alt-text="Geri bildirim menüsü.":::

## <a name="give-feedback-for-multiple-continuous-points"></a>Birden çok sürekli noktaya ilişkin geri bildirimde bulunun

Bir seferde birden çok sürekli noktaya geri bildirimde bulunmak için aşağı tıklayıp farenizi Not eklemek istediğiniz noktalara sürükleyin. Yukarıdaki ile aynı geri bildirim menüsünü görürsünüz. Aynı geri bildirim, **Kaydet**' e tıkladıktan sonra seçili olan tüm noktalara uygulanır.

:::image type="content" source="../media/feedback/continuous-points.png" alt-text="Birden çok noktayı seçin":::

## <a name="how-to-view-my-feedback"></a>Geri bildirimlerimi görüntüleme

Bir noktanın anomali algılama işleminin değişip değişmediğini görmek için, noktanın üzerine gelin. Araç İpucu **geri bildirimle etkilendi:** algılama değiştirilmişse true. **Yanlış**görüntüleniyorsa, noktada geri bildirim hesaplaması yapılır, ancak anomali algılama sonucu değiştirilmedi.

:::image type="content" source="../media/feedback/affected-point.png" alt-text="Nokta, geri bildirimden etkilendi":::

## <a name="when-should-i-annotate-an-anomaly-as-normal"></a>"Normal" olarak bir anomali açıklama ekleme yapmam gerekir

Anomali ' i düşünebileceğiniz birçok neden, yanlış bir alarm. Aşağıdaki senaryolardan biri uygulanmasının ardından aşağıdaki ölçüm Danışmanı özelliklerini kullanmayı göz önünde bulundurun:


|Senaryo  |Öneri |
|---------|---------|
|Anomali, bilinen veri kaynağı değişikliğini (örneğin, bir sistem değişikliği) neden olur.     | Bu senaryonun düzenli olarak yeniden oluşması beklenmiyorsa bu anomali açıklama eklemeyin.        |
|Anomali, tatil nedeniyle oluşur.     | Anomali algılamayı belirtilen zamanlarda işaretlemek için [önceden ayarlanmış olayları](configure-metrics.md#preset-events) kullanın.       |
|Algılanan anomali bir model (örneğin, hafta sonları) vardır ve bu durumlar anormal olmamalıdır.      |Geri bildirim özelliğini veya önceden ayarlanmış olayları kullanın.        |

## <a name="next-steps"></a>Sonraki adımlar
- [Bir olayı tanılayın](diagnose-incident.md).
- [Ölçümleri yapılandırma ve ince ayar algılama yapılandırması](configure-metrics.md)
- [Kanca kullanarak uyarıları yapılandırma ve bildirimler alma](../how-tos/alerts.md)