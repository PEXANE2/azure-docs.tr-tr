---
title: Azure Ön Kapı lı Web Uygulaması Güvenlik Duvarı için ilke ayarları
description: Web Uygulaması Güvenlik Duvarı (WAF) öğrenin.
author: vhorne
ms.service: web-application-firewall
ms.topic: article
services: web-application-firewall
ms.date: 08/21/2019
ms.author: victorh
ms.openlocfilehash: 08b21ccd7f7958f00546583f680ecb8cde4a20c8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75932608"
---
# <a name="policy-settings-for-web-application-firewall-on-azure-front-door"></a>Azure Ön Kapı'da Web Uygulaması Güvenlik Duvarı için ilke ayarları

Web Uygulaması Güvenlik Duvarı (WAF) ilkesi, web uygulamalarınıza erişimi bir dizi özel ve yönetilen kuralla denetlemenize olanak tanır. WAF ilke adı benzersiz olmalıdır. Varolan bir adı kullanmaya çalışırsanız bir doğrulama hatası alırsınız. Bu makalede açıklandığı gibi, bu ilke için belirtilen tüm kurallar için geçerli birden çok ilke düzeyi ayarları vardır.

## <a name="waf-state"></a>WAF eyaleti

Ön Kapı için bir WAF politikası aşağıdaki iki durumdan birinde olabilir:
- **Etkinleştirilmiş:** Bir ilke etkinleştirildiğinde, WAF gelen istekleri etkin bir şekilde inceler ve kural tanımlarına göre ilgili eylemleri alır
- **Devre dışı:** - Bir ilke devre dışı bırakıldığında, WAF denetimi duraklatılır. Gelen istekler WAF'ı atlar ve Ön Kapı yönlendirmesine göre arka uçlara gönderilir.

## <a name="waf-mode"></a>WAF modu

WAF ilkesi aşağıdaki iki modda çalışacak şekilde yapılandırılabilir:

- **Algılama modu** Algılama modunda çalıştırıldığında, WAF isteği ve eşleşen WAF kuralını WAF günlüklerine izlemek ve günlüğe kaydetmek dışında herhangi bir işlem yapmaz. Ön Kapı için günlük tanılamasını açın (portalı kullanırken, azure portalındaki **Tanılama** bölümüne giderek bu elde edilebilir).

- **Önleme modu** Önleme modunda çalışacak şekilde yapılandırıldığında, bir istek bir kuralla eşleşirse WAF belirtilen eylemi alır. Eşleşen istekler WAF günlüklerinde de günlüğe kaydedilir.

## <a name="waf-response-for-blocked-requests"></a>Engellenen istekler için WAF yanıtı

Varsayılan olarak, WAF eşleşen bir kural nedeniyle bir isteği engellediğinde, 403 durum kodunu döndürür - **İstek iletisi engellenir.** Bir başvuru dizesi de günlüğe kaydetme için döndürülür.

Bir istek WAF tarafından engellendiğinde özel yanıt durum kodu ve yanıt iletisi tanımlayabilirsiniz. Aşağıdaki özel durum kodları desteklenir:

- 200 TAMAM
- 403 Yasak
- 405 Yönteme izin verilmiyor
- 406 Kabul edilemez
- 429 Çok fazla istek

Özel yanıt durum kodu ve yanıt iletisi bir ilke düzeyi ayarıdır. Yapılandırıldıktan sonra, engellenen tüm istekler aynı özel yanıt durumunu ve yanıt iletisini alır.

## <a name="uri-for-redirect-action"></a>Eylem yönlendirmek için URI

BIR WAF ilkesinde yer alan kurallardan herhangi biri için **YÖNLENDIRME** eylemi seçiliyse istekleri yönlendirmek için bir URI tanımlamanız gerekir. Bu yeniden yönlendirme URI geçerli bir HTTP(S) sitesi olması gerekir ve bir kez yapılandırıldıktan sonra, bir "YÖNLENDIRME" eylemi ile kuralları eşleşen tüm istekler belirtilen siteye yönlendirilir.


## <a name="next-steps"></a>Sonraki adımlar
- WAF [özel yanıtlarını](waf-front-door-configure-custom-response-code.md) nasıl tanımlayın öğrenin
