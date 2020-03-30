---
title: Azure Uygulama Öngörüleri ile Güvenlik Algılama Paketi
description: Olası güvenlik sorunları için Azure Uygulama Öngörüleri ve Akıllı Algılama ile uygulamayı izleyin.
ms.topic: conceptual
ms.date: 12/12/2017
ms.openlocfilehash: b23ada6fe4596a2eee242cc9145789521caf697f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77669735"
---
# <a name="application-security-detection-pack-preview"></a>Uygulama güvenlik algılama paketi (önizleme)

Application Insights, uygulamanız tarafından oluşturulan telemetriyi otomatik olarak analiz eder ve olası güvenlik sorunlarını algılar. Bu özellik, olası güvenlik sorunlarını belirlemenize ve uygulamayı düzelterek veya gerekli güvenlik önlemlerini alarak bunları işlemenizi sağlar.

Bu özellik, [uygulamanızı telemetri gönderecek şekilde yapılandırmak](https://docs.microsoft.com/azure/application-insights/app-insights-usage-overview)dışında özel bir kurulum gerektirmez.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Bu tür bir akıllı algılama bildirimini ne zaman alacağım?
Algılanan üç tür güvenlik sorunu vardır:
1. Güvensiz URL erişimi: Uygulamadaki bir URL'ye hem HTTP hem de HTTPS üzerinden erişiliyor. Genellikle, HTTPS isteklerini kabul eden bir URL HTTP isteklerini kabul etmemelidir. Bu, uygulamanızda bir hata veya güvenlik sorunu gösterebilir.
2. Güvensiz form: uygulamadaki bir form (veya diğer "POST" isteği) HTTPS yerine HTTP kullanır. HTTP'nin kullanılması, form tarafından gönderilen kullanıcı verilerini tehlikeye atabilir.
3. Şüpheli kullanıcı etkinliği: Uygulamaya aynı kullanıcı tarafından yaklaşık aynı anda birden çok ülkeden/bölgeden erişiliyor. Örneğin, aynı kullanıcı aynı saat içinde İspanya ve ABD'den uygulamaya erişmiş. Bu algılama, uygulamanıza yönelik olası bir kötü amaçlı erişim girişimini gösterir.

## <a name="does-my-app-definitely-have-a-security-issue"></a>UygulamamDa kesinlikle bir güvenlik sorunu var mı?
Hayır, bildirim, uygulamanızın kesinlikle bir güvenlik sorunu olduğu anlamına gelmez. Yukarıdaki senaryolardan herhangi birinin algılanması, çoğu durumda bir güvenlik sorununa işaret edebilir. Ancak, algılama doğal bir iş gerekçesi olabilir ve yoksayılabilir.

## <a name="how-do-i-fix-the-insecure-url-access-detection"></a>"Güvenli Olmayan URL erişimi" algılamasını nasıl düzeltebilirim?
1. **Önceliklendirme.** Bildirim, güvenli olmayan URL'lere erişen kullanıcı sayısını ve güvenli olmayan erişimden en çok etkilenen URL'yi sağlar. Bu, soruna öncelik atamanıza yardımcı olabilir.
2. **Kapsam.** Kullanıcıların yüzde kaçı güvenli olmayan URL'lere erişmiş? Kaç URL etkilendi? Bu bilgiler bildirimden elde edilebilir.
3. **Tanılamak.** Algılama, sorunu daha fazla tanılamanıza yardımcı olmak için güvenli olmayan isteklerin listesini ve etkilenen URL'lerin ve kullanıcıların listesini sağlar.

## <a name="how-do-i-fix-the-insecure-form-detection"></a>"Güvensiz form" algılamasını nasıl düzeltebilirim?
1. **Önceliklendirme.** Bildirim, güvenli olmayan formların sayısını ve verileri tehlikeye girme potansiyeli olan kullanıcı sayısını sağlar. Bu, soruna öncelik atamanıza yardımcı olabilir.
2. **Kapsam.** En fazla sayıda güvensiz iletimde hangi form yer aldı ve zaman içinde güvensiz iletimlerin dağılımı nedir? Bu bilgiler bildirimden elde edilebilir.
3. **Tanılamak.** Algılama, sorunu daha fazla tanılamanıza yardımcı olmak için güvenli olmayan formların listesini ve her form için güvenli olmayan iletim sayısının dökümünü sağlar.

## <a name="how-do-i-fix-the-suspicious-user-activity-detection"></a>"Şüpheli kullanıcı etkinliği" algılamasını nasıl düzeltebilirim?
1. **Önceliklendirme.** Bildirim, şüpheli davranışı sergileyen farklı kullanıcı sayısını sağlar. Bu, soruna öncelik atamanıza yardımcı olabilir.
2. **Kapsam.** Şüpheli talepler hangi ülkelerden/bölgelerden geldi? En çok hangisi şüpheliydi? Bu bilgiler bildirimden elde edilebilir.
3. **Tanılamak.** Algılama, sorunu daha fazla tanılamanıza yardımcı olmak için şüpheli kullanıcıların listesini ve her kullanıcı için ülkelerin/bölgelerin listesini sağlar.
