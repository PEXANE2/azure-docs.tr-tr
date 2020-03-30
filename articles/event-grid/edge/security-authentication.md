---
title: Güvenlik ve kimlik doğrulama - Azure Olay Grid IoT Edge | Microsoft Dokümanlar
description: IoT Edge'deki Event Grid'de güvenlik ve kimlik doğrulaması.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/06/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 5dfa17fd702b76e2cfaa7a91066dbc6749c1069e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76844522"
---
# <a name="security-and-authentication"></a>Güvenlik ve kimlik doğrulaması

Güvenlik ve kimlik doğrulama gelişmiş bir kavramdır ve önce Olay Izgara temellerine aşinalık gerektirir. IoT Edge'deki Event Grid'de yeniyseniz [buradan](concepts.md) başlayın. Olay Izgara modülü, IoT Edge'deki mevcut güvenlik altyapısı üzerine kurulur. Ayrıntılar ve kurulum için [bu belgelere](../../iot-edge/security.md) bakın.

Aşağıdaki bölümlerde bu ayarların nasıl güvenli ve kimlik doğrulaması yapılır ayrıntılı olarak açıklanmıştır:

* TLS yapılandırması
* Gelen istemci kimlik doğrulaması
* Giden sunucu kimlik doğrulaması
* Giden istemci kimlik doğrulaması

>[!IMPORTANT]
>Olay Izgara modülü güvenliği ve kimlik doğrulama kaldıraç IoT Edge mevcut altyapı. Varsayım, IoT Edge alt sisteminin güvenli olduğudur.

>[!IMPORTANT]
>Olay Izgara **yapılandırması varsayılan olarak güvenlidir.** Aşağıdaki alt bölümlerde kimlik doğrulama yönlerini geçersiz kılmak için kullanabileceğiniz tüm seçenekleri ve olası değer(ler) açıklayınız. Herhangi bir değişiklik yapmadan önce etkisini anlayın. Değişikliklerin etkili oltası için Olay Izgara modülünün yeniden dağıtılması gerekir.

## <a name="tls-configuration-aka-server-authentication"></a>TLS yapılandırması (diğer bir sunucu kimlik doğrulaması)

Olay Izgara modülü hem HTTP hem de HTTPS uç noktalarını barındırar. Her IoT Edge modülüne IoT Edge'in güvenlik daemonu tarafından bir sunucu sertifikası atanır. Bitiş noktasını korumak için sunucu sertifikasını kullanırız. Son kullanma tarihinde, modül Otomatik olarak IoT Edge güvenlik daemon yeni bir sertifika ile yeniler.

Varsayılan olarak, yalnızca HTTPS iletişimine izin verilir. Bu davranışı **yapılandırma inbound__serverAuth__tlsPolicy** yoluyla geçersiz kılabilirsiniz. Aşağıdaki tablo, bu özelliğin olası değerini(ler) yakalar.

| Olası değer(ler) | Açıklama |
| ---------------- | ------------ |
| Katı | Varsayılan. Yalnızca HTTPS'yi etkinleştirin
| Etkin | Hem HTTP hem de HTTPS'yi etkinleştirin
| Devre dışı | Yalnızca HTTP'yi etkinleştirin

## <a name="inbound-client-authentication"></a>Gelen istemci kimlik doğrulaması

İstemciler, yönetim ve/veya çalışma zamanı işlemleri yapan varlıklardır. İstemciler diğer IoT Edge modülleri, IoT olmayan uygulamalar olabilir.

Olay Izgara modülü iki tür istemci kimlik doğrulamasını destekler:

* Paylaşılan Erişim İmzası (SAS) anahtar tabanlı
* sertifika tabanlı

Varsayılan olarak, Olay Izgara modülü yalnızca sertifika tabanlı kimlik doğrulamasını kabul etmek üzere yapılandırılır. Başlangıçta, Event Grid modülü IoT Edge güvenlik daemon"undan "TrustBundle"ı alır ve herhangi bir istemci sertifikasını doğrulamak için kullanır. Bu zincire çözüm meyen istemci sertifikaları `UnAuthorized`ile reddedilecektir.

### <a name="certificate-based-client-authentication"></a>Sertifika tabanlı istemci kimlik doğrulaması

Sertifika tabanlı kimlik doğrulaması varsayılan olarak açıktır. Özellik **inbound__clientAuth__clientCert__enabled**üzerinden sertifika tabanlı kimlik doğrulamasını devre dışı bırakmayı seçebilirsiniz. Aşağıdaki tablo olası değeri(ler) yakalar.

| Olası değer(ler) | Açıklama |
| ----------------  | ------------ |
| true | Varsayılan. İstemci sertifikasını sunmak için Olay Izgara modülündeki tüm istekleri gerektirir. Ayrıca, **inbound__clientAuth__clientCert__source**yapılandırmanız gerekir.
| yanlış | Bir istemciyi sertifika ibraz etmeye zorlama.

Aşağıdaki **tablo, inbound__clientAuth__clientCert__source** için olası değeri(ler) yakalar

| Olası değer(ler) | Açıklama |
| ---------------- | ------------ |
| IoT Edge | Varsayılan. Tüm istemci sertifikalarını doğrulamak için IoT Edge'in Güven Paketini kullanır.

İstemci varsayılan olarak kendi imzalı bir müşteri sunarsa, Olay Izgara modülü bu tür istekleri reddeder. **Inbound__clientAuth__clientCert__allowUnknownCA** özelliği aracılığıyla kendi imzalı istemci sertifikalarına izin vermeyi seçebilirsiniz. Aşağıdaki tablo olası değeri(ler) yakalar.

| Olası değer(ler) | Açıklama |
| ----------------  | ------------|
| true | Varsayılan. İmzalı sertifikaların başarıyla sunulmasını sağlar.
| yanlış | Kendi imzalı sertifikalar sunulursa istekleri başarısız olur.

>[!IMPORTANT]
>Üretim senaryolarında, **inbound__clientAuth__clientCert__allowUnknownCA'yi** **inbound__clientAuth__clientCert__allowUnknownCA** false olarak ayarlamak isteyebilirsiniz.

### <a name="sas-key-based-client-authentication"></a>SAS anahtar tabanlı istemci kimlik doğrulaması

Sertifika tabanlı kimlik doğrulamasına ek olarak, Olay Izgara modülü SAS Anahtar tabanlı kimlik doğrulamasını da yapabilir. SAS anahtarı, gelen tüm aramaları doğrulamak için kullanması gereken Olay Izgara modülünde yapılandırılan gizli bir anahtar gibidir. İstemcilerin HTTP Başlık 'aeg-sas-key' gizli belirtmeniz gerekir. Eşleşmediği takdirde `UnAuthorized` istek reddedilir.

SAS anahtar tabanlı kimlik doğrulamasını denetlemek için yapılandırma **inbound__clientAuth__sasKeys__enabled.**

| Olası değer(ler) | Açıklama  |
| ----------------  | ------------ |
| true | SAS anahtar tabanlı kimlik doğrulamasına izin verir. **inbound__clientAuth__sasKeys__key1** veya **inbound__clientAuth__sasKeys__key2** gerektirir
| yanlış | Varsayılan. SAS Anahtar tabanlı kimlik doğrulaması devre dışı bırakılır.

 **inbound__clientAuth__sasKeys__key1** ve **inbound__clientAuth__sasKeys__key2,** gelen isteklere karşı kontrol etmek üzere Olay Izgara modüllerini yapılandırdığınız anahtarlardır. Anahtarlardan en az birinin yapılandırılması gerekir. İsteme talebinde bulunan istemcinin anahtarı istek üstbilgisinin bir parçası olarak sunması gerekir '**aeg-sas-key**' . Her iki anahtar da yapılandırılırsa, istemci anahtarlardan birini sunabilir.

> [!NOTE]
>Her iki kimlik doğrulama yöntemini de yapılandırabilirsiniz. Böyle bir durumda SAS anahtarı önce denetlenir ve yalnızca bu başarısız olursa, sertifika tabanlı kimlik doğrulaması gerçekleştirilir. Bir isteğin başarılı olabilmesi için, kimlik doğrulama yöntemlerinden yalnızca birinin başarılı olması gerekir.

## <a name="outbound-client-authentication"></a>Giden istemci kimlik doğrulaması

Giden bağlamda istemci Olay Izgara modülü ne başvurur. Yapılan işlem, abonelere olay teslim ediyor. Abone modülleri sunucu olarak kabul edilir.

Her IoT Edge modülüne IoT Edge'in güvenlik daemonu tarafından bir Kimlik Sertifikası atanır. Giden aramalar için kimlik sertifikasını kullanırız. Son kullanma tarihinde, modül Otomatik olarak IoT Edge güvenlik daemon yeni bir sertifika ile yeniler.

Giden istemci kimlik doğrulamasını denetlemek için yapılandırma **outbound__clientAuth__clientCert__enabled.**

| Olası değer(ler) | Açıklama |
| ----------------  | ------------ |
| true | Varsayılan. Bir sertifika sunmak için Olay Izgara modülünden gelen tüm giden istekleri gerektirir. **outbound__clientAuth__clientCert__source**yapılandırması gerekiyor.
| yanlış | Sertifikasını sunmak için Olay Izgara modülüne gerek yok.

Sertifikanın kaynağını denetleyen yapılandırma **outbound__clientAuth__clientCert__source.**

| Olası değer(ler) | Açıklama |
| ---------------- | ------------ |
| IoT Edge | Varsayılan. Modülün IoT Edge güvenlik daemon tarafından yapılandırılan kimlik sertifikasını kullanır.

### <a name="outbound-server-authentication"></a>Giden Sunucu Kimlik Doğrulaması

Olay Izgara abonesinin hedef türlerinden biri "Webhook"tür. Varsayılan olarak, bu tür aboneler için yalnızca HTTPS uç noktaları kabul edilir.

Webhook hedef **ilkesini**outbound__webhook__httpsOnly denetlemek için yapılandırma.

| Olası değer(ler) | Açıklama |
| ----------------  | ------------ |
| true | Varsayılan. Yalnızca HTTPS bitiş noktasına sahip abonelere izin verir.
| yanlış | HTTP veya HTTPS bitiş noktası olan abonelere izin verir.

Varsayılan olarak, Olay Izgara modülü abonenin sunucu sertifikasını doğrular. **doğrulamayı geçersiz**outbound__webhook__skipServerCertValidation atlayabilirsiniz. Olası değerler şunlardır:

| Olası değer(ler) | Açıklama |
| ----------------  | ------------ |
| true | Abonenin sunucu sertifikasını doğrulamayın.
| yanlış | Varsayılan. Abonenin sunucu sertifikasını doğrulayın.

Abonenin sertifikası kendi imzası varsa, varsayılan olarak Olay Izgara modülü bu tür aboneleri reddeder. Kendi imzalı sertifikaya izin vermek **için outbound__webhook__allowUnknownCA**geçersiz kılabilirsiniz. Aşağıdaki tablo olası değeri(ler) yakalar.

| Olası değer(ler) | Açıklama |
| ----------------  | ------------ |
| true | Varsayılan. İmzalı sertifikaların başarıyla sunulmasını sağlar.
| yanlış | Kendi imzalı sertifikalar sunulursa istekleri başarısız olur.

>[!IMPORTANT]
>Üretim senaryolarında **outbound__webhook__allowUnknownCA** **yanlış**olarak ayarlamak isteyeceksiniz.

> [!NOTE]
>IoT Edge ortamı kendi imzalı sertifikalar oluşturur. Öneri, üretim iş yükleri için yetkili CA'lar tarafından verilen sertifikalar oluşturmak ve hem gelen hem de **giden** **false'da UnknownCA** özelliğine izin vermektir.

## <a name="summary"></a>Özet

Olay Izgara modülü **varsayılan olarak güvenlidir.** Üretim dağıtımlarınız için bu varsayılanları korumanızı öneririz.

Yapılandırma sırasında kullanılacak yol gösterici ilkeler şunlardır:

* Modüle yalnızca HTTPS isteklerine izin verin.
* Yalnızca sertifika tabanlı istemci kimlik doğrulaması izin verin. Yalnızca tanınmış CA'lar tarafından verilen sertifikalara izin verin. Kendi imzalı sertifikalara izin verme.
* Disallow SASKey tabanlı istemci kimlik doğrulaması.
* Giden aramalarda Olay Izgara modülün kimlik sertifikasını her zaman sunun.
* Webhook hedef türleri için yalnızca HTTPS abonelerine izin verin.
* Webhook hedef türleri için abonenin sunucu sertifikasını her zaman doğrulayın. Yalnızca tanınmış CA'lar tarafından verilen sertifikalara izin verin. Kendi imzalı sertifikalara izin verme.

Varsayılan olarak, Olay Izgara modülü aşağıdaki yapılandırma ile dağıtılır:

 ```json
 {
  "Env": [
    "inbound__serverAuth__tlsPolicy=strict",
    "inbound__serverAuth__serverCert__source=IoTEdge",
    "inbound__clientAuth__sasKeys__enabled=false",
    "inbound__clientAuth__clientCert__enabled=true",
    "inbound__clientAuth__clientCert__source=IoTEdge",
    "inbound__clientAuth__clientCert__allowUnknownCA=true",
    "outbound__clientAuth__clientCert__enabled=true",
    "outbound__clientAuth__clientCert__source=IoTEdge",
    "outbound__webhook__httpsOnly=true",
    "outbound__webhook__skipServerCertValidation=false",
    "outbound__webhook__allowUnknownCA=true"
  ],
  "HostConfig": {
    "PortBindings": {
      "4438/tcp": [
        {
          "HostPort": "4438"
        }
      ]
    }
  }
}
```
