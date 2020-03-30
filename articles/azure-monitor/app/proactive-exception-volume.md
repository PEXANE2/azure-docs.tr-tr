---
title: Özel durum hacminde anormal artış - Azure Uygulama Öngörüleri
description: Özel durum hacmindeki olağandışı desenler için Azure Uygulama İstatistikleri'nde Smart Detection ile uygulama özel durumlarını izleyin.
ms.topic: conceptual
ms.date: 12/08/2017
ms.openlocfilehash: a08fae4774a8afb9959f55ea3196cd1a45c33439
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671775"
---
# <a name="abnormal-rise-in-exception-volume-preview"></a>Özel durum hacminde anormal artış (önizleme)

Uygulama Öngörüleri, uygulamanızda atılan özel durumları otomatik olarak analiz eder ve özel durum telemetrinizdeki olağandışı desenler hakkında sizi uyarabilir.

Bu özellik, uygulamanız için [özel durum raporlaması yapılandırmadışında](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-exceptions#set-up-exception-reporting) özel bir kurulum gerektirmez. Uygulamanız yeterli özel durum telemetrisi oluşturduğunda etkindir.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Bu tür bir akıllı algılama bildirimini ne zaman alacağım?
Uygulamanız, önceki yedi gün içinde hesaplanan bir taban çizgisiyle karşılaştırıldığında, bir gün içinde belirli bir türdeki özel durumların sayısında anormal bir artış sergiliyorsa, bu tür bir bildirim alabilirsiniz.
Uygulama kullanımınızda doğal bir büyüme göz önünde bulundurularak, özel durum sayısındaki artışı tespit etmek için makine öğrenimi algoritmaları kullanılmaktadır.

## <a name="does-my-app-definitely-have-a-problem"></a>Uygulamamın kesinlikle bir sorunu mu var?
Hayır, bildirim, uygulamanızın kesinlikle bir sorunu olduğu anlamına gelmez. Aşırı sayıda özel durum genellikle bir uygulama sorunu işaret etse de, bu özel durumlar iyi huylu olabilir ve uygulamanız tarafından doğru şekilde işlenebilir.

## <a name="how-do-i-fix-it"></a>Nasıl düzeltebilirim?
Bildirimler tanılama sürecinde desteklemek için tanılama bilgilerini içerir:
1. **Önceliklendirme.** Bildirim, kaç kullanıcının veya kaç isteğin etkilendiğini gösterir. Bu, soruna öncelik atamanıza yardımcı olabilir.
2. **Kapsam.** Sorun tüm trafiği mi etkiliyor, yoksa sadece bir operasyonu mu? Bu bilgiler bildirimden elde edilebilir.
3. **Tanılamak.** Algılama, özel durum atıldığı yöntem ve özel durum türü hakkında bilgiler içerir. Sorunu daha fazla tanılamanıza yardımcı olmak için, destekleyici bilgilere bağlantı veren ilgili öğeleri ve raporları da kullanabilirsiniz.
