---
title: Sistem durumu denetimleri için hata başvurusu
description: Azure Konteyner Kayıt Defteri'nde az acr denetim-sağlık tanı komutu çalıştırılarak bulunan sorunların hata kodları ve olası çözümleri
ms.topic: article
ms.date: 07/02/2019
ms.openlocfilehash: 971b28b2bf8d9ac22cec0efe979837886762cf17
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80289150"
---
# <a name="health-check-error-reference"></a>Sistem durumu denetimi hata başvurusu

Aşağıda [az acr denetim durumu][az-acr-check-health] komutu tarafından döndürülen hata kodları ile ilgili ayrıntılar yer alıyor. Her hata için olası çözümler listelenir.

## <a name="docker_command_error"></a>DOCKER_COMMAND_ERROR

Bu hata, CLI için Docker istemcisi bulunamadı anlamına gelir. Sonuç olarak, aşağıdaki ek denetimler çalıştırılmaz: Docker sürümünü bulmak, Docker daemon durumunu değerlendirmek ve Docker çekme komutu çalıştırmak.

*Olası çözümler*: Docker istemcisi yükleyin; sistem değişkenlerine Docker yolunu ekleyin.

## <a name="docker_daemon_error"></a>DOCKER_DAEMON_ERROR

Bu hata, Docker daemon durumunun kullanılamadığı veya CLI kullanılarak ulaşılabildiği anlamına gelir. Sonuç olarak, Docker işlemleri `docker login` (cli `docker pull`aracılığıyla kullanılamaz.

*Olası çözümler*: Docker daemon'u yeniden başlatın veya düzgün bir şekilde takılmış olduğunu doğrulayın.

## <a name="docker_version_error"></a>DOCKER_VERSION_ERROR

Bu hata, CLI'nin komutu `docker --version`çalıştıramadığını gösterir.

*Olası çözümler*: Komutu el ile çalıştırmayı deneyin, en son CLI sürümüne sahip olduğundan emin olun ve hata iletisini araştırın.

## <a name="docker_pull_error"></a>DOCKER_PULL_ERROR

Bu hata, CLI'nin ortamınıza örnek bir görüntü çekememesi anlamına gelir.

*Olası çözümler*: Görüntüyü çekmek için gereken tüm bileşenlerin düzgün çalıştığını doğrulayın.

## <a name="helm_command_error"></a>HELM_COMMAND_ERROR

Bu hata, Helm istemcisinin CLI tarafından bulunabildiği anlamına gelir ve bu da diğer Helm işlemlerini engellemez.

*Olası çözümler*: Helm istemcisinin yüklü olduğunu ve yolunun sistem ortamı değişkenlerine eklendiğinden doğrulayın.

## <a name="helm_version_error"></a>HELM_VERSION_ERROR

Bu hata, CLI'nin yüklü Miğfer sürümünü belirleyemediği anlamına gelir. Bu durum, kullanılan Azure CLI sürümü (veya Miğfer sürümü) geçersizse gerçekleşebilir.

*Olası çözümler*: En son Azure CLI sürümüne veya önerilen Helm sürümüne güncelleştirin; komutu el ile çalıştırın ve hata iletisini araştırın.

## <a name="connectivity_dns_error"></a>CONNECTIVITY_DNS_ERROR

Bu hata, verilen kayıt defteri giriş sunucusu için DNS ping edildi ama yanıt vermedi, bu da kullanılamaz anlamına gelir anlamına gelir. Bu, bazı bağlantı sorunlarını gösterebilir. Alternatif olarak, kayıt defteri olmayabilir, kullanıcı kayıt defterinde izinlere sahip olmayabilir (oturum açma sunucusunu düzgün bir şekilde almak için) veya hedef kayıt defteri Azure CLI'de kullanılandan farklı bir bulutta.

*Olası çözümler*: Bağlantıyı doğrulayın; kayıt defterinin yazımını doğrulayın ve bu kayıt defteri nin var olduğunu; kullanıcının üzerinde doğru izinlere sahip olduğunu ve kayıt defterinin bulutunun Azure CLI'de kullanılanla aynı olduğunu doğrulayın.

## <a name="connectivity_forbidden_error"></a>CONNECTIVITY_FORBIDDEN_ERROR

Bu hata, verilen kayıt defteri için sorun bitiş noktası 403 Yasak HTTP durumu ile yanıt anlamına gelir. Bu hata, kullanıcıların büyük olasılıkla sanal ağ yapılandırması nedeniyle kayıt defterine erişemedikleri anlamına gelir. Şu anda yapılandırılan güvenlik duvarı `az acr show --query networkRuleSet --name <registry>`kurallarını görmek için çalıştırın.

*Olası çözümler*: Sanal ağ kurallarını kaldırın veya geçerli istemci IP adresini izin verilen listeye ekleyin.

## <a name="connectivity_challenge_error"></a>CONNECTIVITY_CHALLENGE_ERROR

Bu hata, hedef kayıt defterinin zorlu bitiş noktasının bir sorun çıkarmadığı anlamına gelir.

*Olası çözümler*: Bir süre sonra tekrar deneyin. Hata devam ederse, 'de https://aka.ms/acr/issuesbir sorun açın.

## <a name="connectivity_aad_login_error"></a>CONNECTIVITY_AAD_LOGIN_ERROR

Bu hata, hedef kayıt defterinin zorlu bitiş noktasının bir sorun çıkardığı anlamına gelir, ancak kayıt defteri Azure Etkin Dizin kimlik doğrulamasını desteklemez.

*Olası çözümler*: Örneğin yönetici kimlik bilgileriyle kimlik doğrulamanın farklı bir yolunu deneyin. Kullanıcıların Azure Active Directory'yi kullanarak kimlik doğrulaması https://aka.ms/acr/issuesgerekirse, 'de bir sorun açın.

## <a name="connectivity_refresh_token_error"></a>CONNECTIVITY_REFRESH_TOKEN_ERROR

Bu hata, kayıt defteri giriş sunucusunun yenilenme belirteciyle yanıt vermediği, bu nedenle hedef kayıt defterine erişimin reddedildiği anlamına gelir. Bu hata, kullanıcının kayıt defterinde doğru izinlere sahip olmaması veya Azure CLI'nin kullanıcı kimlik bilgileri eskiyse oluşabilir.

*Olası çözümler*: Kullanıcının kayıt defterinde doğru izinlere sahip olup olmadığını doğrulayın; izinleri, belirteçleri ve kimlik bilgilerini yenilemek için çalıştırın. `az login`

## <a name="connectivity_access_token_error"></a>CONNECTIVITY_ACCESS_TOKEN_ERROR

Bu hata, kayıt defteri giriş sunucusunun bir erişim belirteciyle yanıt vermediği, böylece hedef kayıt defterine erişimin reddedildiği anlamına gelir. Bu hata, kullanıcının kayıt defterinde doğru izinlere sahip olmaması veya Azure CLI'nin kullanıcı kimlik bilgileri eskiyse oluşabilir.

*Olası çözümler*: Kullanıcının kayıt defterinde doğru izinlere sahip olup olmadığını doğrulayın; izinleri, belirteçleri ve kimlik bilgilerini yenilemek için çalıştırın. `az login`

## <a name="connectivity_ssl_error"></a>CONNECTIVITY_SSL_ERROR

Bu hata, istemcinin kapsayıcı kayıt defterine güvenli bir bağlantı kuramadığı anlamına gelir. Bu hata genellikle bir proxy sunucusu çalıştırıyor veya kullanıyorsanız oluşur.

*Potansiyel çözümler*: Bir proxy arkasında çalışma hakkında daha fazla bilgi [burada bulabilirsiniz.](https://github.com/Azure/azure-cli/blob/master/doc/use_cli_effectively.md#working-behind-a-proxy)

## <a name="login_server_error"></a>LOGIN_SERVER_ERROR

Bu hata, CLI'nin verilen kayıt defterinin giriş sunucusunu bulamadığı ve geçerli bulut için varsayılan sonek bulunmadığı anlamına gelir. Bu hata, kayıt defteri yoksa, kullanıcı kayıt defterinde doğru izinlere sahip değilse, kayıt defterinin bulutu ve geçerli Azure CLI bulutu eşleşmiyorsa veya Azure CLI sürümü eskimişse oluşabilir.

*Olası çözümler*: Yazım hatasının doğru olduğunu ve kayıt defterinin var olduğunu doğrulayın; kullanıcının kayıt defterinde doğru izinlere sahip olduğunu ve kayıt defteri ve CLI ortamının bulutlarının eşleşip eşleşip eşleşmediğini doğrulayın; Azure CLI'yi en son sürüme güncelleyin.

## <a name="notary_version_error"></a>NOTARY_VERSION_ERROR

Bu hata, CLI'nin Docker/Noter'in şu anda yüklü olan sürümüyle uyumlu olmadığı anlamına gelir. Bu sorunu çözmek için Docker yüklemenizin Noter istemcisini el ile değiştirerek noter.exe sürümünüzü 0.6.0'dan önceki bir sürüme düşürmeyi deneyin.

## <a name="next-steps"></a>Sonraki adımlar

Kayıt defterinin durumunu denetleme seçenekleri [için](container-registry-check-health.md)bkz.

Azure Kapsayıcı Kayıt Defteri hakkında sık sorulan sorular ve bilinen diğer sorunlar için [SSS](container-registry-faq.md) bölümüne bakın.





<!-- LINKS - internal -->
[az-acr-check-health]: /cli/azure/acr#az-acr-check-health
