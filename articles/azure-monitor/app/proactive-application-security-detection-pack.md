---
title: Azure Application Insights ile güvenlik algılama paketi
description: Azure Application Insights ve olası güvenlik sorunları için akıllı algılama ile uygulamayı izleyin.
ms.topic: conceptual
ms.date: 12/12/2017
ms.openlocfilehash: b23ada6fe4596a2eee242cc9145789521caf697f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77669735"
---
# <a name="application-security-detection-pack-preview"></a>Uygulama güvenliği algılama paketi (Önizleme)

Application Insights, uygulamanız tarafından oluşturulan Telemetriyi otomatik olarak analiz eder ve olası güvenlik sorunlarını algılar. Bu özellik, olası güvenlik sorunlarını tanımlamanızı ve uygulamayı düzelterek veya gerekli güvenlik önlemlerini alarak bunları işleyebilmenizi sağlar.

Bu özellik, [uygulamanızı Telemetriyi gönderecek şekilde yapılandırma](https://docs.microsoft.com/azure/application-insights/app-insights-usage-overview)dışında özel bir kurulum gerektirmez.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Bu tür akıllı algılama bildirimini ne zaman alabilirim?
Algılanan üç güvenlik sorunu türü vardır:
1. Güvenli olmayan URL erişimi: uygulamadaki bir URL 'ye hem HTTP hem de HTTPS üzerinden erişilir. Genellikle, HTTPS isteklerini kabul eden bir URL, HTTP isteklerini kabul etmemelidir. Bu, uygulamanızda bir hata veya güvenlik sorunu olduğunu gösterebilir.
2. Güvensiz form: uygulamadaki bir form (veya başka bir "POST" isteği) HTTPS yerine HTTP kullanır. HTTP kullanımı, form tarafından gönderilen Kullanıcı verilerinin güvenliğini tehlikeye atabilir.
3. Şüpheli kullanıcı etkinliği: uygulamaya, aynı kullanıcı tarafından yaklaşık olarak aynı anda birden çok ülkede/bölgeden erişilir. Örneğin, aynı kullanıcı, Ispanya 'dan uygulamaya ve aynı saat içinde Birleşik Devletler erişti. Bu algılama, uygulamanıza yönelik olabilecek kötü amaçlı bir erişim denemesi olduğunu gösterir.

## <a name="does-my-app-definitely-have-a-security-issue"></a>Uygulamamın kesinlikle bir güvenlik sorunu var mı?
Hayır, bir bildirim, uygulamanızın kesinlikle güvenlik sorunu olduğu anlamına gelmez. Yukarıdaki senaryoların herhangi birini algılamada, çoğu durumda bir güvenlik sorunu olduğunu belirtebilirsiniz. Ancak algılama, doğal bir iş gerekçesini alabilir ve yoksayılabilir.

## <a name="how-do-i-fix-the-insecure-url-access-detection"></a>"Güvensiz URL erişimi" algılamayı Nasıl yaparım?.
1. **Değerlendirme.** Bildirim, güvenli olmayan URL 'Lere erişen kullanıcıların sayısını ve güvenli olmayan erişimin en çok etkilediği URL 'YI sağlar. Bu, soruna bir öncelik atamanıza yardımcı olabilir.
2. **Kapsam.** Kullanıcıların güvenli olmayan URL 'Lere ne yüzdesi erişmiş? Kaç URL etkileniyor? Bu bilgiler bildirimden elde edilebilir.
3. **Tanın.** Algılama, güvenli olmayan isteklerin listesini ve sorunu daha da tanılamanıza yardımcı olması için etkilenen URL 'lerin ve kullanıcıların listesini sağlar.

## <a name="how-do-i-fix-the-insecure-form-detection"></a>"Güvensiz form" algılamayı Nasıl yaparım? düzeltilsin mi?
1. **Değerlendirme.** Bildirim, güvenli olmayan formların ve verilerin riskli olma olasılığı olan kullanıcı sayısının sayısını sağlar. Bu, soruna bir öncelik atamanıza yardımcı olabilir.
2. **Kapsam.** Hangi form, güvensiz aktarımların en büyük sayısına ve zaman içinde güvenli olmayan iletimlerin dağıtımına dahil mi? Bu bilgiler bildirimden elde edilebilir.
3. **Tanın.** Algılama, sorunu daha da tanılamanıza yardımcı olması için güvenli olmayan formların ve her bir form için güvenli olmayan iletimlerin sayısını sağlar.

## <a name="how-do-i-fix-the-suspicious-user-activity-detection"></a>"Şüpheli kullanıcı etkinliği" algılamayı Nasıl yaparım?.
1. **Değerlendirme.** Bildirim, şüpheli davranışı gösteren farklı kullanıcı sayısını sağlar. Bu, soruna bir öncelik atamanıza yardımcı olabilir.
2. **Kapsam.** Şüpheli istekleri hangi ülkelerde/bölgelerde gerçekleştirsin? En şüpheli Kullanıcı hangisi? Bu bilgiler bildirimden elde edilebilir.
3. **Tanın.** Bu algılama, sorunun daha fazla tanılanmasına yardımcı olmak için şüpheli kullanıcıların, her kullanıcı için ülke/bölge listesinin listesini sağlar.
