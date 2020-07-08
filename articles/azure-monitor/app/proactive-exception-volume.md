---
title: Özel durum hacminde olağan dışı artış-Azure Application Insights
description: Özel durum hacminde olağan dışı desenler için Azure Application Insights 'te akıllı algılama ile uygulama özel durumlarını izleyin.
ms.topic: conceptual
ms.date: 12/08/2017
ms.openlocfilehash: a08fae4774a8afb9959f55ea3196cd1a45c33439
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77671775"
---
# <a name="abnormal-rise-in-exception-volume-preview"></a>Özel durum biriminde olağan dışı artış (Önizleme)

Application Insights uygulamanızda oluşturulan özel durumları otomatik olarak analiz eder ve özel durum telemetrinizdeki olağan dışı desenler hakkında sizi uyarabilir.

Bu özellik, uygulamanız için [özel durum raporlamayı yapılandırmadan](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-exceptions#set-up-exception-reporting) farklı bir kurulum gerektirmez. Uygulamanız yeterli sayıda özel durum telemetrisi oluşturduğunda etkin olur.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Bu tür akıllı algılama bildirimini ne zaman alabilirim?
Bu tür bir bildirime, uygulamanız bir gün içinde belirli bir tür özel durum sayısında, önceki yedi gün içinde hesaplanan bir taban çizgisine kıyasla olağan dışı bir artış yaşanıyorsa elde edebilirsiniz.
Makine öğrenimi algoritmaları, uygulama kullanımınızın doğal bir büyümesini hesaba katlarken özel durum sayısı artışı algılamak için kullanılır.

## <a name="does-my-app-definitely-have-a-problem"></a>Uygulamamın kesinlikle bir sorunu var mı?
Hayır, bir bildirim uygulamanızın kesinlikle bir sorun olduğu anlamına gelmez. Aşırı sayıda özel durum genellikle bir uygulama sorununa işaret etse de, bu özel durumlar sizin uygulamanız tarafından zararsız olabilir ve doğru şekilde işlenebilir.

## <a name="how-do-i-fix-it"></a>Nasıl düzeltebilirim?
Bildirimler tanılama sürecinde desteklanacak tanılama bilgilerini içerir:
1. **Değerlendirme.** Bildirim, kaç kullanıcının veya kaç isteğin etkilendiğini gösterir. Bu, soruna bir öncelik atamanıza yardımcı olabilir.
2. **Kapsam.** Sorun tüm trafiği etkiliyor mu, yoksa yalnızca bir işlem mi? Bu bilgiler bildirimden elde edilebilir.
3. **Tanın.** Algılama, özel durumun oluşturulduğu yöntemin yanı sıra özel durum türü hakkında bilgiler içerir. Sorunu daha da tanılamanıza yardımcı olması için, destek bilgilerine bağlanan ilgili öğeleri ve raporları da kullanabilirsiniz.
