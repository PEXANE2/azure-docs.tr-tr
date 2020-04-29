---
title: İzleme önem derecesi oranına göre düşme-Azure Application Insights
description: Akıllı algılama ile izleme telemetrisinde olağan dışı desenler için Azure Application Insights ile uygulama izlemelerini izleyin.
ms.topic: conceptual
ms.date: 11/27/2017
ms.openlocfilehash: 30bdd30ac9c49bb79a3c48bae8149ec761756dd4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77671690"
---
# <a name="degradation-in-trace-severity-ratio-preview"></a>İzleme önem oranına göre düşme (Önizleme)

İzlemeler, arka planda neler olduğunu söyleyecektir. uygulamalarda yaygın olarak kullanılır. İşlemler yanlış gittiğinde, izleme işlemleri istenmeyen duruma kadar olan olayların dizisine önemli görünürlük sağlar. İzlemeler genellikle yapılandırılmamış olsa da, bunların önem derecesindeki bir şekilde öğrenilmesi gereken bir şey vardır. Bir uygulamanın kararlı durumunda, "iyi" izlemeler (*bilgi* ve *ayrıntı*) ve "kötü" izlemeler (*Uyarı*, *hata*ve *kritik*) arasındaki oranı kararlı kalacak şekilde bekletireceğiz. Bu varsayımla, "kötü" izlemelerinin herhangi bir sayıda nedenden dolayı (örneğin geçici ağ sorunları) belirli bir ölçüde düzenli olarak gerçekleşebileceği varsayılır. Ancak gerçek bir sorun büyümeye başladığında, genellikle "kötü" izlemelerinin ve "iyi" izlemelerinin göreli oranındaki artış olarak bildirim yapılır. Application Insights akıllı algılama, uygulamanız tarafından günlüğe kaydedilen izlemeleri otomatik olarak analiz eder ve izleme telemetrinizin önem derecesine göre sizi olağan dışı desenler hakkında uyarabilir.

Bu özellik, uygulamanız için izleme günlüğü yapılandırma dışında özel bir kurulum gerektirmez (bkz. [.net](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-trace-logs) veya [Java](https://docs.microsoft.com/azure/application-insights/app-insights-java-trace-logs)için izleme günlüğü dinleyicisini yapılandırma). Uygulamanız yeterli sayıda özel durum telemetrisi oluşturduğunda etkin olur.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Bu tür akıllı algılama bildirimini ne zaman alabilirim?
"İyi" izlemeler (bir *bilgi* düzeyiyle günlüğe kaydedilmiş *izlemeler) ve*"kötü" izlemeler (bir *Uyarı*düzeyiyle günlüğe kaydedilmiş izlemeler, *hata*veya *önemli*) ile karşılaştırıldığında, önceki yedi gün içinde hesaplanan bir taban çizgisine kıyasla, bu tür bir bildirim elde edebilirsiniz.

## <a name="does-my-app-definitely-have-a-problem"></a>Uygulamamın kesinlikle bir sorunu var mı?
Hayır, bir bildirim uygulamanızın kesinlikle bir sorun olduğu anlamına gelmez. "İyi" ve "kötü" izlemeler arasındaki orandaki bir azalma bir uygulama sorunu belirtebilse de, bu oran değişikliği zararsız olabilir. Örneğin, artış, uygulamadaki yeni bir akışın var olan akışlardan daha fazla "kötü" izlemeler yaymasına neden olmuş olabilir.

## <a name="how-do-i-fix-it"></a>Nasıl düzeltebilirim?
Bildirimler tanılama sürecinde desteklanacak tanılama bilgilerini içerir:
1. **Değerlendirme.** Bildirim, kaç tane işlemin etkilendiğini gösterir. Bu, soruna bir öncelik atamanıza yardımcı olabilir.
2. **Kapsam.** Sorun tüm trafiği etkiliyor mu, yoksa yalnızca bir işlem mi? Bu bilgiler bildirimden elde edilebilir.
3. **Tanın.** Sorunu daha da tanılamanıza yardımcı olması için, destek bilgilerine bağlanan ilgili öğeleri ve raporları kullanabilirsiniz.


