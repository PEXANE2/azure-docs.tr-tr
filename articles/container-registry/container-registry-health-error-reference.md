---
title: Sistem durumu denetimi için hata başvurusu-Azure Container Registry
description: Azure Container Registry ' de az ACR Check-Health Diagnostic komutu çalıştırılarak bulunan sorunlara yönelik olası çözümler ve hata kodları
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 07/02/2019
ms.author: danlep
ms.openlocfilehash: 672d446fa8dc27612c7b046cac109bfa4ca5fec5
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68309367"
---
# <a name="health-check-error-reference"></a>Sistem durumu denetimi hata başvurusu

[Az ACR Check-Health][az-acr-check-health] komutu tarafından döndürülen hata kodlarıyla ilgili ayrıntılar aşağıda verilmiştir. Her hata için olası çözümler listelenir.

## <a name="dockercommanderror"></a>DOCKER_COMMAND_ERROR

Bu hata, CLı için Docker istemcisinin bulunamadığı anlamına gelir. Sonuç olarak, aşağıdaki ek denetimler çalıştırılmaz: Docker sürümü bulunuyor, Docker Daemon durumunu değerlendiriyor ve Docker Pull komutu çalıştırılıyor.

*Olası çözümler*: Docker Client 'ı yükler; Sistem değişkenlerine Docker yolu ekleyin.

## <a name="dockerdaemonerror"></a>DOCKER_DAEMON_ERROR

Bu hata, Docker Daemon durumunun kullanılamadığı veya CLı kullanılarak erişilemeyen anlamına gelir. Sonuç olarak, Docker işlemleri ( `docker login` ve `docker pull`gibi) CLI aracılığıyla kullanılamaz.

*Olası çözümler*: Docker Daemon programını yeniden başlatın veya düzgün bir şekilde yüklendiğini doğrulayın.

## <a name="dockerversionerror"></a>DOCKER_VERSION_ERROR

Bu hata, CLı 'nin komutu `docker --version`çalıştıramayacağı anlamına gelir.

*Olası çözümler*: Komutu el ile çalıştırmayı deneyin, en son CLı sürümüne sahip olduğunuzdan emin olun ve hata iletisini araştırın.

## <a name="dockerpullerror"></a>DOCKER_PULL_ERROR

Bu hata, CLı 'nin ortamınıza örnek bir görüntü çekemediği anlamına gelir.

*Olası çözümler*: Bir görüntüyü çekmek için gereken tüm bileşenlerin düzgün şekilde çalıştığını doğrulayın.

## <a name="helmcommanderror"></a>HELM_COMMAND_ERROR

Bu hata, diğer HELI işlemlerini daha fazla kullanan CLı tarafından, Helu istemcisinin bulunamadıkları anlamına gelir.

*Olası çözümler*: Hele istemcisinin yüklü olduğunu ve yolunun sistem ortam değişkenlerine eklendiğini doğrulayın.

## <a name="helmversionerror"></a>HELM_VERSION_ERROR

Bu hata, CLı 'nin yüklü Held sürümünü belirleyemediği anlamına gelir. Bu durum, kullanılmakta olan Azure CLı sürümü (veya Held sürümü) kullanılmıyorsa oluşabilir.

*Olası çözümler*: En son Azure CLı sürümüne veya önerilen Held sürümüne güncelleştirin; komutu el ile çalıştırın ve hata iletisini araştırın.

## <a name="connectivitydnserror"></a>CONNECTIVITY_DNS_ERROR

Bu hata, verilen kayıt defteri oturum açma sunucusu için DNS 'nin ping işlemi yaptığı ancak yanıt vermediği anlamına gelir; bu da kullanılamaz. Bu, bazı bağlantı sorunlarını gösterebilir. Alternatif olarak, kayıt defteri mevcut olmayabilir, Kullanıcı kayıt defteri üzerinde izinlere sahip olmayabilir (oturum açma sunucusunu düzgün bir şekilde almak için) veya hedef kayıt defteri, Azure CLı 'de kullanılandan farklı bir bulutta bulunuyor olabilir.

*Olası çözümler*: Bağlantıyı doğrulama; kayıt defterinin var olduğunu ve kayıt defteri olduğunu doğrulayın; kullanıcının üzerinde doğru izinlere sahip olduğunu ve kayıt defterinin bulutu 'nın Azure CLı 'de kullanılan aynı olduğunu doğrulayın.

## <a name="connectivityforbiddenerror"></a>CONNECTIVITY_FORBIDDEN_ERROR

Bu hata, verilen kayıt defteri için zorluk bitiş noktasının 403 yasaklanmış bir HTTP durumuyla yanıt verdiğini gösterir. Bu hata, büyük olasılıkla bir sanal ağ yapılandırması nedeniyle, kullanıcıların kayıt defterine erişemeyeceği anlamına gelir.

*Olası çözümler*: Sanal ağ kurallarını kaldırın veya geçerli istemci IP adresini izin verilen listeye ekleyin.

## <a name="connectivitychallengeerror"></a>CONNECTIVITY_CHALLENGE_ERROR

Bu hata, hedef kayıt defterinin sınama uç noktasının bir zorluk yayınlamadığı anlamına gelir.

*Olası çözümler*: Bir süre sonra yeniden deneyin. Hata devam ederse, öğesinde https://aka.ms/acr/issues bir sorun açın.

## <a name="connectivityaadloginerror"></a>CONNECTIVITY_AAD_LOGIN_ERROR

Bu hata, hedef kayıt defterinin sınama uç noktasının bir zorluk verdiği, ancak kayıt defterinin Azure Active Directory kimlik doğrulamasını desteklemediği anlamına gelir.

*Olası çözümler*: Örneğin, yönetici kimlik bilgileriyle kimlik doğrulaması yapmanın farklı bir yolunu deneyin. Kullanıcıların Azure Active Directory kullanarak kimlik doğrulaması yapması gerekiyorsa, ' de https://aka.ms/acr/issues bir sorun açın.

## <a name="connectivityrefreshtokenerror"></a>CONNECTIVITY_REFRESH_TOKEN_ERROR

Bu hata, kayıt defteri oturum açma sunucusunun yenileme belirteciyle yanıt vermediği anlamına gelir; bu nedenle hedef kayıt defterine erişim reddedildi. Bu hata, kullanıcının kayıt defterinde doğru izinlere sahip olmaması veya Azure CLı için Kullanıcı kimlik bilgilerinin eski olması durumunda meydana gelebilir.

*Olası çözümler*: Kullanıcının kayıt defterinde doğru izinlere sahip olduğunu doğrulayın; izinleri `az login` , belirteçleri ve kimlik bilgilerini yenilemek için ' i çalıştırın.

## <a name="connectivityaccesstokenerror"></a>CONNECTIVITY_ACCESS_TOKEN_ERROR

Bu hata, kayıt defteri oturum açma sunucusunun bir erişim belirteciyle yanıt vermediği, hedef kayıt defterine erişimin reddedilmediği anlamına gelir. Bu hata, kullanıcının kayıt defterinde doğru izinlere sahip olmaması veya Azure CLı için Kullanıcı kimlik bilgilerinin eski olması durumunda meydana gelebilir.

*Olası çözümler*: Kullanıcının kayıt defterinde doğru izinlere sahip olduğunu doğrulayın; izinleri `az login` , belirteçleri ve kimlik bilgilerini yenilemek için ' i çalıştırın.

## <a name="loginservererror"></a>LOGIN_SERVER_ERROR

Bu hata, CLı 'nin verilen kayıt defteri 'nin oturum açma sunucusunu bulamadığı ve geçerli bulut için varsayılan sonekin bulunmadığı anlamına gelir. Kayıt defteri yoksa, kullanıcının kayıt defterinde doğru izinleri yoksa, kayıt defterinin bulutu ve geçerli Azure CLı bulutu eşleşmiyorsa veya Azure CLı sürümü artık kullanılmıyor ise bu hata oluşabilir.

*Olası çözümler*: Yazımın doğru olduğundan ve kayıt defterinin var olduğundan emin olun; kullanıcının kayıt defterinde doğru izinlere sahip olduğunu ve kayıt defterinin ve CLı ortamının bulutlarının eşleştiğini doğrulayın; Azure CLı 'yı en son sürüme güncelleştirin.

## <a name="next-steps"></a>Sonraki adımlar

Bir kayıt defterinin sistem durumunu denetleme seçenekleri için bkz. [Azure Container Registry 'nin sistem durumunu denetleme](container-registry-check-health.md).

Sık sorulan sorular ve Azure Container Registry ilgili diğer bilinen sorunlar için [SSS](container-registry-faq.md) bölümüne bakın.





<!-- LINKS - internal -->
[az-acr-check-health]: /cli/azure/acr#az-acr-check-health
