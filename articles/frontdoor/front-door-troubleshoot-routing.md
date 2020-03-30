---
title: Azure Ön Kapı yapılandırma sorunları
description: Bu eğitimde, Ön Kapınız için karşılaşabileceğiniz bazı sık karşılaşılan sorunları kendi kendine nasıl giderilen öğreneceksiniz.
services: frontdoor
documentationcenter: ''
author: sharad4u
editor: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2018
ms.author: sharadag
ms.openlocfilehash: 962c884eb8adc05e5d50b6b254d5c3f0b18af556
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79471515"
---
# <a name="troubleshooting-common-routing-issues"></a>Sık karşılaşılan yönlendirme sorunlarını giderme

Bu makalede, Azure Ön Kapı yapılandırmanızda karşılaşabileceğiniz sık karşılaşılan yönlendirme sorunlarının bazılarının nasıl giderilen giderilmesi açıklanmaktadır.

## <a name="503-response-from-front-door-after-a-few-seconds"></a>Birkaç saniye sonra Front Door 503 yanıt

### <a name="symptom"></a>Belirti

- Ön Kapıdan geçmeden arka uca gönderilen düzenli istekler başarılı dır, ancak Ön Kapı üzerinden gitmek 503 hata yanıtı verir.

- Ön Kapı'daki arıza birkaç saniye sonra gösterir (genellikle 30 saniye sonra)

### <a name="cause"></a>Nedeni

Bu belirti, arka uçünüzün Ön Kapı'dan isteği almak için zaman alakart yapılandırmasının (varsayılan 30 saniye) ötesine geçtiği veya Ön Kapı'dan isteğe yanıt göndermek için bu zaman sonu değerinin ötesine geçtiği nde ortaya çıkar. 

### <a name="troubleshooting-steps"></a>Sorun giderme adımları

- İsteğinizi doğrudan arka uca gönderin (Ön Kapıdan geçmeden) ve arka uçunuzu yanıtlamak için gereken olağan süreyi görün.
- İsteği Ön Kapı üzerinden gönderin ve 503 yanıtı görüp görmediğinize bakın. Değilse, o zaman bu bir zaman ara sorunu olmayabilir. Lütfen desteğe başvurun.
- Ön Kapı üzerinden gitmek 503 hata yanıt koduyla sonuçlanırsa, varsayılan zaman aşımını 4 dakikaya (240 saniye) kadar uzatmak için ön kapınız için sendReceiveTimeout alanını yapılandırın. Ayar altında `backendPoolSettings` ve denir. `sendRecvTimeoutSeconds` 

## <a name="requests-sent-to-the-custom-domain-returns-400-status-code"></a>Özel etki alanına gönderilen istekler 400 Durum Kodu döndürür

### <a name="symptom"></a>Belirti

- Bir Ön Kapı oluşturdunuz, ancak etki alanı veya ön uç ana bilgisayariçin bir istek http 400 durum kodu döndürüyor.

- Özel bir etki alanından, yapılandırdığınız ön uç ana bilgisayara bir DNS eşleme oluşturdunuz. Ancak, özel etki alanı ana bilgisayarı adına bir istek göndermek bir HTTP 400 durum kodu döndürür ve yapılandırdığınız arka uç(lar) için rota görünmüyor.

### <a name="cause"></a>Nedeni

Ön uç ana bilgisayar olarak eklediğiniz özel etki alanı için bir yönlendirme kuralı yapılandırmadıysanız, bu belirti ortaya çıkabilir. Ön Kapı alt etki alanı (*.azurefd.net) altında ön yüz ana bilgisayar için yapılandırılmış olsa bile, özel etki alanınızın DNS eşlemesiyle ön yüz ana bilgisayarı için açıkça bir yönlendirme kuralının eklenmesi gerekir.

### <a name="troubleshooting-steps"></a>Sorun giderme adımları

Özel etki alanından istenen arka uç havuzuna yönlendirme kuralı ekleyin.

## <a name="front-door-is-not-redirecting-http-to-https"></a>Ön Kapı HTTP'yi HTTPS'ye yönlendirmiyor

### <a name="symptom"></a>Belirti

Ön Kapınızda HTTP'yi HTTPS'ye yönlendirin yazan bir yönlendirme kuralı vardır, ancak etki alanına erişmek protokol olarak HTTP'yi korur.

### <a name="cause"></a>Nedeni

Ön Kapınızın yönlendirme kurallarını doğru şekilde yapılandırmadıysanız, bu davranış gerçekleşebilir. Temel olarak, geçerli yapılandırmanız belirli değildir ve çakışan kuralları olabilir.

### <a name="troubleshooting-steps"></a>Sorun giderme adımları

## <a name="request-to-frontend-hostname-returns-404-status-code"></a>Frontend hostname İstek Returns 404 Durum Kodu

### <a name="symptom"></a>Belirti

- Bir Ön Kapı oluşturdunuz ve bir ön uç ana bilgisayar, içinde en az bir arka uç bulunan bir arka uç havuzu ve ön uç ana bilgisayarını arka uç havuzuna bağlayan bir yönlendirme kuralı nı yapılandırıldınız. HTTP 404 durum kodu döndürüldİğİnden, yapılandırılmış ön yüz ana bilgisayara istek gönderirken içeriğiniz kullanılamıyor gibi görünüyor.

### <a name="cause"></a>Nedeni

Bu belirti için birkaç olası nedeni vardır:

- Arka uç arka uça bakan bir kamu değildir ve Ön Kapı tarafından görülemez.
- Arka uç yanlış yapılandırılmıştır ve bu da Ön Kapı'nın yanlış istek göndermesine neden olur (diğer bir zamanda arka uç yalnızca HTTP'yi kabul eder, ancak HTTPS'ye izin vermeyi denetlememişsiniz, bu yüzden Ön Kapı HTTPS isteklerini iletmeye çalışıyor).
- Arka uç, istekle birlikte arka uca iletilen ana bilgisayar üstbilgisini reddeder.
- Arka uç yapılandırması henüz tam olarak dağıtılmadı.

### <a name="troubleshooting-steps"></a>Sorun giderme adımları

1. Dağıtım Süresi
   - Yapılandırmanın dağıtılması için ~10 dakika beklediğinden emin olun.

2. Arka Uç Ayarlarını Kontrol Edin
    - İsteğin yönlendirmesi gereken arka uç havuzuna gidin (yönlendirme kuralını nasıl yapılandırdığınıza bağlıdır) ve _arka uç ana bilgisayar türü_ ve arka uç ana bilgisayarı adının doğru olduğunu doğrulayın. Arka uç özel bir ana bilgisayarsa, doğru yazıldığından emin olun. 

    - HTTP ve HTTPS bağlantı noktalarınızı kontrol edin. Çoğu durumda, 80 ve 443 (sırasıyla), doğrudur ve hiçbir değişiklik gerekli olacaktır. Ancak, arka uçbu şekilde yapılandırılmamış ve farklı bir bağlantı noktasında dinliyor bir şans vardır.

        - Frontend ana bilgisayarının yönlendirmesi gereken arka uçlar için yapılandırılan _Arka Uç ana_ bilginini denetleyin. Çoğu durumda, bu üstbilgi _Backend ana bilgisayar adı_ile aynı olmalıdır. Ancak, ters uç farklı bir şey bekliyorsa, yanlış bir değer çeşitli HTTP 4xx durum kodlarıneden olabilir. Arka uçunuzun IP adresini girdiyseniz, Arka uç _ana bilgisayar üstbilgisini_ arka uç ana bilgisayar adına ayarlamanız gerekebilir.


3. Yönlendirme Kuralı Ayarlarını Kontrol Etme
    - Söz konusu Frontend ana bilgisayar adından arka uç havuzuna yönlendirilmesi gereken yönlendirme kuralına gidin. Kabul edilen protokollerin doğru şekilde yapılandırıldığından veya yapılandırılmazsa, isteği ileterken Ön Kapı protokolünün kullanacağından emin olun. _Kabul edilen protokoller_ alanı, Ön Kapı'nın hangi istekleri kabul etmesi gerektiğini belirler ve _Yönlendirme protokolü,_ isteği arka uca iletmek için Ön Kapı'nın hangi protokolü kullanması gerektiğini belirler.
         - Örnek olarak, arka uç yalnızca HTTP isteklerini kabul ederse aşağıdaki yapılandırmalar geçerli olacaktır:
            - _Kabul edilen protokoller_ HTTP ve HTTPS'dir. _Yönlendirme protokolü_ HTTP'dir. HTTPS izin verilen bir protokol olduğundan ve bir istek HTTPS olarak geldiğinden, Ön Kapı https kullanarak iletmeyi dener.

            - _Kabul edilen protokoller_ HTTP'dir. _Yönlendirme protokolü,_ istek veya HTTPS ile eşleşir.

    - _Url Yeniden Yazma_ varsayılan olarak devre dışı bırakılır ve bu alanı yalnızca kullanılabilir hale getirmek istediğiniz arka uç barındırılan kaynakların kapsamını daraltmak istiyorsanız kullanmanız gerekir. Devre dışı bırakıldığında, Ön Kapı aldığı aynı istek yolunu iletir. Bu alanın yanlış yapılandırılmış olması ve Ön Kapı'nın arka uçtan kullanılamayan bir kaynak istemesi ve böylece bir HTTP 404 durum kodu döndürmesi mümkündür.

