---
title: Azure ön kapı yapılandırma sorunlarını giderme
description: Bu öğreticide, ön kapılarınız için yüz yüze olabilecek bazı yaygın sorunları nasıl giderebileceğinizi öğreneceksiniz.
services: frontdoor
documentationcenter: ''
author: duongau
editor: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/30/2020
ms.author: duau
ms.openlocfilehash: dbce9019e33c07dd4faa91ffd490eba4d313c675
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91630619"
---
# <a name="troubleshooting-common-routing-issues"></a>Yaygın yönlendirme sorunlarını giderme

Bu makalede, Azure ön kapısının yapılandırmasına yönelik olabilecek bazı yaygın yönlendirme sorunları nasıl giderilir açıklanmaktadır.

## <a name="503-response-from-front-door-after-a-few-seconds"></a>birkaç saniye sonra ön kapıdan gelen yanıt 503

### <a name="symptom"></a>Belirti

* Ön kapıya geçmeden arka ucunuza gönderilen normal istekler ilerlemeden devam ediyor, ancak ön kapıya geçiş 503 hata yanıtlarına neden oluyor.
* Ön kapıdan gelen hata birkaç saniye sonra gösterilir (genellikle 30 saniye sonra)

### <a name="cause"></a>Nedeni

Bu sorunun nedeni iki işlemlerden biri olabilir:
 
* Arka ucunuz, ön kapıdan gelen isteği almak için yapılandırılan zaman aşımından (varsayılan değer 30 saniye) daha uzun sürüyor.
* Ön kapıdan gelen isteğe yanıt göndermek için gereken süre, zaman aşımı değerinden daha uzun sürüyor. 

### <a name="troubleshooting-steps"></a>Sorun giderme adımları

* İsteği doğrudan arka ucunuza gönderin (ön kapıya geçmeden) ve arka ucunuzun yanıt vermesi için ne kadar zaman aldığını öğrenin.
* İsteği ön kapıdan gönderin ve 503 yanıt aldığınızı öğrenin. Aksi takdirde, sorun bir zaman aşımı sorunu olmayabilir. Desteğe başvurun.
* Ön kapıdan geçmesi 503 hata yanıt kodu ile sonuçlanırsa, `sendReceiveTimeout` alanı ön kapılarınız için yapılandırın. Varsayılan zaman aşımını 4 dakikaya kadar genişletebilirsiniz (240 saniye). Ayarı öğesinin altında bulunur `backendPoolSettings` ve çağrılır `sendRecvTimeoutSeconds` . 

## <a name="requests-sent-to-the-custom-domain-returns-400-status-code"></a>Özel etki alanına gönderilen istekler 400 durum kodunu döndürür

### <a name="symptom"></a>Belirti

* Bir ön kapı oluşturdunuz, ancak etki alanına veya ön uç konağına bir istek HTTP 400 durum kodu döndürüyor.
* Özel bir etki alanı için yapılandırdığınız ön uç konağına bir DNS eşlemesi oluşturdunuz. Ancak, özel etki alanı ana bilgisayar adına bir istek gönderdiğinizde bir HTTP 400 durum kodu döndürülür. Yapılandırdığınız arka uca yönlendirilmez.

### <a name="cause"></a>Nedeni

Bu sorun, ön uç ana bilgisayarı olarak eklenen özel etki alanı için bir yönlendirme kuralı yapılandırmadıysanız oluşur. Bu ön uç ana bilgisayarı için bir yönlendirme kuralının açık olarak eklenmesi gerekir. Ön uç ana bilgisayar için (*. azurefd.net) bir tane zaten yapılandırılmış olsa bile.

### <a name="troubleshooting-steps"></a>Sorun giderme adımları

Trafiği seçilen arka uç havuzuna yönlendirmek için özel etki alanı için bir yönlendirme kuralı ekleyin.

## <a name="front-door-doesnt-redirect-http-to-https"></a>Ön kapı HTTP 'yi HTTPS 'ye yönlendirmez

### <a name="symptom"></a>Belirti

Ön Kapıınızın HTTPS 'ye yeniden yönlendirme kuralı olan bir yönlendirme kuralı vardır ancak etki alanına erişmek için protokol olarak HTTP devam eder.

### <a name="cause"></a>Nedeni

Bu davranış, yönlendirme kurallarını ön kapılarınız için doğru şekilde yapılandırmadıysanız oluşabilir. Temel olarak, geçerli yapılandırmanız özel değildir ve çakışan kurallara sahip olabilir.

### <a name="troubleshooting-steps"></a>Sorun giderme adımları

## <a name="request-to-frontend-hostname-returns-404-status-code"></a>Ön uç konak adı isteği 404 durum kodu döndürüyor

### <a name="symptom"></a>Belirti

 Ön uç konağını, içinde en az bir arka uca sahip bir arka uç havuzunu ve ön uç konağını arka uç havuzuna bağlayan bir yönlendirme kuralını yapılandırarak bir ön kapı oluşturdunuz. Bir HTTP 404 durum kodu döndürüldüğünden, yapılandırılmış ön uç konağına bir istek yaptığınızda içeriğiniz kullanılamaz.

### <a name="cause"></a>Nedeni

Bu belirtinin birkaç olası nedeni vardır:

* Arka uç, genel kullanıma açık bir arka uç değildir ve ön kapıya görünür değildir.
* Arka uç yanlış yapılandırılmış, ön kapıda yanlış istek gönderilmesine neden olur. Diğer bir deyişle, arka ucunuz yalnızca HTTP 'yi kabul eder ve HTTPS 'ye izin vermeyi işaretlemeyin. Bu nedenle, ön kapı HTTPS isteklerini iletmeyi deniyor.
* Arka uç, istekle birlikte arka uca iletilen konak üstbilgisini reddediyor.
* Arka uç yapılandırması henüz tam olarak dağıtılmadı.

### <a name="troubleshooting-steps"></a>Sorun giderme adımları

1. Dağıtım saati
   * Yapılandırmanın dağıtılması için yaklaşık 10 dakika beklediğinizden emin olun.

2. Arka uç ayarlarını denetleyin
    * İsteğin yönlendirilmesi gereken arka uç havuzuna gidin (yönlendirme kuralını nasıl yapılandırdığınıza bağlıdır). *Arka uç konak türünün* ve arka uç ana bilgisayar adının doğru olduğundan emin olun. Arka uç özel bir ana bilgisayar ise doğru yazdığınızdan emin olun. 

    * HTTP ve HTTPS bağlantı noktalarınızı denetleyin. Çoğu durumda, 80 ve 443 (sırasıyla) doğru olur ve hiçbir değişiklik yapmanız gerekmez. Ancak, arka ucunuzun bu şekilde yapılandırılmadığı ve farklı bir bağlantı noktasında dinleme yaptığı bir şansınız vardır.

        * Ön uç konağın yönlendirilmesi gereken _arka uç ana bilgisayar üst bilgisini_ denetleyin. Çoğu durumda bu üst bilgi, *arka uç ana bilgisayar adıyla*aynı olmalıdır. Ancak, arka uç farklı bir şeyi bekliyorsa, yanlış bir değer çeşitli HTTP 4xx durum kodlarına neden olabilir. Arka ucunuzun IP adresini girverirseniz, arka uç *ana bilgisayar üst bilgisini* arka ucun ana bilgisayar adına ayarlamanız gerekebilir.

3. Yönlendirme kuralı ayarlarını denetleyin:
    * Söz konusu ön uç ana bilgisayar adından bir arka uç havuzuna yönlendirilmesi gereken yönlendirme kuralına gidin. İstek iletilirken kabul edilen protokollerin doğru yapılandırıldığından emin olun. *Kabul edilen protokoller* alanı hangi Isteklerin ön kapısının kabul edeceğini belirler. *İletme Protokolü* , isteği arka uca iletmek için kullanılacak protokol ön kapısının ne olduğunu belirler.
         * Örnek olarak, arka uç yalnızca HTTP isteklerini kabul ediyorsa aşağıdaki yapılandırma geçerli olacaktır:
            * *Kabul edilen protokoller* http ve https 'dir. *İletme Protokolü* http 'dir. HTTPS izin verilen bir protokol olduğundan ve bir istek HTTPS olarak verildiyse, ön kapı HTTPS kullanarak iletmeyi dener.

            * *Kabul edilen protokoller* http. *İletme Protokolü* , istek ya da http ile eşleşiyor.

    - *URL yeniden yazma* varsayılan olarak devre dışıdır. Bu alan, yalnızca kullanılabilir hale getirmek istediğiniz arka uç barındırılan kaynakların kapsamını daraltmak istiyorsanız kullanılır. Devre dışı bırakıldığında, ön kapıda aldığı istek yolunu iletecektir. Bu alanı hatalı yapılandırmak mümkündür. Bu nedenle, ön kapı mevcut olmayan arka uca bir kaynak istiyorsa, bir HTTP 404 durum kodu döndürür.
