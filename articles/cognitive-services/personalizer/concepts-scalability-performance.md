---
title: Ölçeklenebilirlik ve Performans - Personalizer
titleSuffix: Azure Cognitive Services
description: 'Yüksek performanslı ve yüksek trafikli web siteleri ve uygulamaları ölçeklenebilirlik ve performans için Personalizer ile göz önünde bulundurulması gereken iki ana faktöre sahiptir: gecikme süresi ve eğitim iş ortası.'
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 10/24/2019
ms.author: diberry
ms.openlocfilehash: 5ac9a870cb05328f040febd0f8161a97f0982e09
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73490778"
---
# <a name="scalability-and-performance"></a>Ölçeklenebilirlik ve Performans

Yüksek performanslı ve yüksek trafikli web siteleri ve uygulamaları ölçeklenebilirlik ve performans için Personalizer ile göz önünde bulundurulması gereken iki ana faktör var:

* Rank API aramaları yaparken düşük gecikme yi tutma
* Eğitim çıktısı olay girişine ayak uydurun

Kişiselleştirme, arama süresinin çoğunun REST API aracılığıyla iletişime adanmış olmasıyla hızla bir sıralama döndürebilir. Azure, isteklere hızla yanıt verme özelliğini otomatik olarak ölçeklendirecek.

##  <a name="low-latency-scenarios"></a>Düşük gecikme li senaryoları

Bazı uygulamalar, bir sıralamayı döndürerken düşük gecikmeler gerektirir. Düşük gecikmeler gereklidir:

* Kullanıcının sıralı içeriği görüntülemeden önce fark edilir bir süre beklemesini engellemek için.
* Aşırı trafik yaşayan bir sunucunun az bilgi işlem süresini ve ağ bağlantılarını bağlamayı önlemesine yardımcı olmak için.


## <a name="scalability-and-training-throughput"></a>Ölçeklenebilirlik ve eğitim iş bilgililiği

Personalizer, Rank ve Reward API'lerinden sonra Personalizer tarafından eşzamanlı olarak gönderilen iletilere dayalı olarak yeniden eğitilen bir modeli güncelleyerek çalışır. Bu iletiler uygulama için bir Azure EventHub kullanılarak gönderilir.

 Çoğu uygulamanın Personalizer'ın maksimum katılım ve eğitim bölümüne ulaşması olası değildir. Bu maksimuma ulaşmak uygulamayı yavaşlatmaz, ancak Olay Hub kuyruklarının temizlenebilirlerinden daha hızlı doldurulduğu anlamına gelir.

## <a name="how-to-estimate-your-throughput-requirements"></a>Hesap gereksinimlerinizi nasıl tahmin edin?

* Bağlam ve eylem JSON belgelerinin uzunluklarını ekleyerek sıralama olayı başına ortalama bayt sayısını tahmin edin.
* 20MB/sn'yi bu tahmini ortalama baytlara bölün.

Örneğin, ortalama yükünüzün 500 özelliği varsa ve her biri tahmini 20 karakterse, her olay yaklaşık 10 kb'dır. Bu tahminlerle, 20.000.000 / 10.000 = 2.000 olaylar/sn, yaklaşık 173 milyon olay/gün. 

Bu sınırlara ulaşıyorsanız, mimari tavsiyesi için lütfen destek ekibimizle iletişime geçin.

## <a name="next-steps"></a>Sonraki adımlar

[Personalizer oluşturun ve yapılandırın.](how-to-settings.md)