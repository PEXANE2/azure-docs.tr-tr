---
title: Bellek sızıntısını algıla - Azure Application Insights Smart Detection
description: Olası bellek sızıntıları için Azure Application Insights ile uygulamaları izleyin.
ms.topic: conceptual
ms.date: 12/12/2017
ms.openlocfilehash: 85d138518dfb1313a810657016e9fe3143887b6d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671707"
---
# <a name="memory-leak-detection-preview"></a>Bellek sızıntısı algılama (önizleme)

Application Insights, uygulamanızdaki her işlemin bellek tüketimini otomatik olarak analiz eder ve olası bellek sızıntıları veya artan bellek tüketimi konusunda sizi uyarabilir.

Bu özellik, uygulamanız için [performans sayaçlarını yapılandırmak](https://docs.microsoft.com/azure/application-insights/app-insights-performance-counters) dışında özel bir kurulum gerektirmez. Uygulamanız yeterli bellek performansı sayacı telemetri (örneğin, Özel Baytlar) oluşturduğunda etkindir.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Bu tür bir akıllı algılama bildirimini ne zaman alacağım?
Tipik bir bildirim, uygulamanızın bir parçası olan bir veya daha fazla işlemde ve/veya bir veya daha fazla makinede uzun bir süre boyunca bellek tüketiminde tutarlı bir artış izler. Makine öğrenimi algoritmaları, bellek sızıntısı desenine uyan artırılmış bellek tüketimini algılamak için kullanılır.

## <a name="does-my-app-really-have-a-problem"></a>Uygulamamın gerçekten bir sorunu var mı?
Hayır, bildirim, uygulamanızın kesinlikle bir sorunu olduğu anlamına gelmez. Bellek sızıntısı desenleri genellikle bir uygulama sorunu göstermek olsa da, bu desenler belirli bir işlem için tipik olabilir veya doğal bir iş gerekçesi olabilir ve yoksayılabilir.

## <a name="how-do-i-fix-it"></a>Nasıl düzeltebilirim?
Bildirimler tanılama çözümleme sürecinde desteklemek için tanılama bilgilerini içerir:
1. **Önceliklendirme.** Bildirim, bellek artış miktarını (GB'de) ve belleğin arttığı zaman aralığını gösterir. Bu, soruna öncelik atamanıza yardımcı olabilir.
2. **Kapsam.** Bellek kaç makine bellek sızıntısı deseni sergiledi? Olası bellek sızıntısı sırasında kaç özel durum tetiklendi? Bu bilgiler bildirimden elde edilebilir.
3. **Tanılamak.** Algılama, zaman içinde işlemin bellek tüketimini gösteren bellek sızıntısı deseni içerir. Sorunu daha fazla tanılamanıza yardımcı olmak için, destekleyici bilgilere bağlantı veren ilgili öğeleri ve raporları da kullanabilirsiniz.
