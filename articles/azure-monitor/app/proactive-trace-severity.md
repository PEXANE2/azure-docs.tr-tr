---
title: İzleme önem oranında bozulma - Azure Uygulama Öngörüleri
description: Smart Detection ile izleme telemetrisinde olağandışı desenler için Azure Uygulama Öngörüleri ile uygulama izlemelerini izleyin.
ms.topic: conceptual
ms.date: 11/27/2017
ms.openlocfilehash: 30bdd30ac9c49bb79a3c48bae8149ec761756dd4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671690"
---
# <a name="degradation-in-trace-severity-ratio-preview"></a>İz önem oranındaki bozulma (önizleme)

İzler, arka planda neler olduğunu anlatmaya yardımcı olduğu için uygulamalarda yaygın olarak kullanılır. İşler ters gittiğinde, izler istenmeyen duruma giden olaylar dizisine önemli bir görünürlük sağlar. İzler genellikle yapılandırılmamış olsa da, somut olarak onlardan öğrenilebilir bir şey var - onların şiddeti düzeyi. Bir uygulamanın sabit durumunda, "iyi" izlemeler *(Bilgi* ve *Verbose)* ve "kötü" izlemeler *(Uyarı,* *Hata*, ve *Kritik)* arasındaki oranın sabit kalmasını bekleriz. Varsayım, "kötü" izlemelerin belirli bir sayıda nedenden (örneğin geçici ağ sorunları) bağlı olarak belirli bir ölçüde düzenli olarak gerçekleşebileceğidir. Ama gerçek bir sorun büyümeye başladığında, genellikle "kötü" izler vs "iyi" izleri göreli oranında bir artış olarak tezahür eder. Application Insights Smart Detection, uygulamanız tarafından günlüğe kaydedilen izleri otomatik olarak analiz eder ve izleme telemetrinizin şiddetindeki olağandışı desenler hakkında sizi uyarabilir.

Bu özellik, uygulamanız için izleme günlüğü yapılandırma dışında özel bir kurulum gerektirmez [(.NET](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-trace-logs) veya [Java](https://docs.microsoft.com/azure/application-insights/app-insights-java-trace-logs)için izleme günlüğü dinleyicisini nasıl yapılandırılabilirsiniz). Uygulamanız yeterli özel durum telemetrisi oluşturduğunda etkindir.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Bu tür bir akıllı algılama bildirimini ne zaman alacağım?
"İyi" izlemeler *(Bilgi* veya *Verbose*düzeyiyle günlüğe kaydedilmiş izlemeler) ve "kötü" izlemeler *(Uyarı,* *Hata*veya *Ölümcül*düzeyiyle günlüğe kaydedilmiş izlemeler) arasındaki oran, önceki yedi gün içinde hesaplanan bir taban çizgisiyle karşılaştırıldığında, belirli bir günde aşağılayıcı ysa, bu tür bir bildirim alabilirsiniz.

## <a name="does-my-app-definitely-have-a-problem"></a>Uygulamamın kesinlikle bir sorunu mu var?
Hayır, bildirim, uygulamanızın kesinlikle bir sorunu olduğu anlamına gelmez. "İyi" ve "kötü" izlemeler arasındaki oranbir bozulma bir uygulama sorunu gösterebilir olsa da, oran bu değişiklik iyi huylu olabilir. Örneğin, artış, uygulamada varolan akışlardan daha fazla "kötü" iz yayan yeni bir akıştan kaynaklanıyor olabilir).

## <a name="how-do-i-fix-it"></a>Nasıl düzeltebilirim?
Bildirimler tanılama sürecinde desteklemek için tanılama bilgilerini içerir:
1. **Önceliklendirme.** Bildirim, kaç işlemden etkilendiğini gösterir. Bu, soruna öncelik atamanıza yardımcı olabilir.
2. **Kapsam.** Sorun tüm trafiği mi etkiliyor, yoksa sadece bir operasyonu mu? Bu bilgiler bildirimden elde edilebilir.
3. **Tanılamak.** Sorunu daha fazla tanılamanıza yardımcı olmak için, destekleyici bilgilere bağlantı veren ilgili öğeleri ve raporları kullanabilirsiniz.


