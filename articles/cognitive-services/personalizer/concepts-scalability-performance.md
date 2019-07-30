---
title: Ölçeklenebilirlik ve performans-kişiselleştirici
titleSuffix: Azure Cognitive Services
description: 'Yüksek performanslı ve yüksek trafikli web siteleri ve uygulamalar, ölçeklenebilirlik ve performans için kişiselleştirmede göz önünde bulundurmanız gereken iki ana etkene sahiptir: gecikme süresi ve eğitim performansı.'
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 06/07/2019
ms.author: diberry
ms.openlocfilehash: d116f6bd389b1404ea723c965111cd05880e6c30
ms.sourcegitcommit: e3b0fb00b27e6d2696acf0b73c6ba05b74efcd85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68662810"
---
# <a name="scalability-and-performance"></a>Ölçeklenebilirlik ve performans

Yüksek performanslı ve yüksek trafikli web siteleri ve uygulamalar, ölçeklenebilirlik ve performans için Kişiselleştiriciye göz önünde bulundurmanız gereken iki ana etkene sahiptir:

* Derecelendirme API 'SI çağrıları yaparken düşük gecikme süresi tutma
* Eğitim aktarım hızının olay girişi ile devam ettiğinizden emin olma

Kişiselleştirme, REST API aracılığıyla iletişime ayrılan çağrı süresinin büyük bir bölümünü hızla bir derece döndürebilir. Azure, isteklere hızla yanıt verebilme özelliğini otomatik olarak sağlar.

##  <a name="low-latency-scenarios"></a>Düşük gecikmeli senaryolar

Bazı uygulamalar bir derece döndürürken düşük gecikme süreleri gerektirir. Bu gereklidir:

* Kullanıcının, derecelendirilen içeriği görüntülemeden önce fark edilebilir bir süre beklememasını sağlamak.
* Extreme trafiği yaşayan bir sunucuda, nadir işlem süresini ve ağ bağlantılarını kullanmaktan kaçının.

<!--

If your web site is scaled on your infrastructure, you can avoid making HTTP calls by hosting the Personalizer API in your own servers running a Docker container.

This change would be transparent to your application, other than using an endpoint URL referring to the running docker instances as opposed to an online service in the cloud.



### Extreme Low Latency Scenarios

If you require latencies under a millisecond, and have already tested using Personalizer via containers, please contact our support team so we can assess your scenario and provide guidance suited to your needs.

-->

## <a name="scalability-and-training-throughput"></a>Ölçeklenebilirlik ve eğitim performansı

Kişiselleştirici, API 'Lerin derecelendirmeden ve geri alındıktan sonra zaman uyumsuz olarak gönderilen iletilere bağlı olarak yeniden paketlenmiş bir model güncelleştirerek işe yarar. Bu iletiler, uygulama için bir Azure EventHub kullanılarak gönderilir.

 Çoğu uygulama, kişiselleştirici için en fazla katılma ve eğitim aktarım hızına ulaşacaktır. Bu sınıra ulaşıldığında, uygulama yavaşlamadan, Olay Hub 'ı sıralarının, temizlenmeden daha hızlı doldurulmuş olduğunu fark eder.

## <a name="how-to-estimate-your-throughput-requirements"></a>Verimlilik gereksinimlerinizi tahmin etme

* Bağlam ve eylem JSON belgelerinin uzunluklarını ekleyerek sıralama olayı başına ortalama bayt sayısını tahmin edin.
* Bu tahmini ortalama bayt ile 20 MB/sn bölün.

Örneğin, ortalama yükünüzü 500 özelliği varsa ve her biri tahmini 20 karakter ise, her olay yaklaşık 10 KB olur. Bu tahminlerde 20.000.000/10.000 = 2.000 olay/sn, bu da 173.000.000 olaylar/gün ile ilgilidir. 

Bu sınırlara ulaşmanız durumunda, lütfen mimari önerisi için destek ekibimize başvurun.

## <a name="next-steps"></a>Sonraki adımlar

[Kişiselleştirici oluşturma ve yapılandırma](how-to-settings.md).