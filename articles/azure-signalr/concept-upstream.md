---
title: Azure SignalR hizmetindeki yukarı akış ayarları
description: Yukarı akış ayarlarının ve yukarı akış iletilerinin protokol tanıtımı
author: chenyl
ms.service: signalr
ms.topic: conceptual
ms.date: 06/11/2020
ms.author: chenyl
ms.openlocfilehash: 7434e8796ddcd89968a0ffa0328a823d635f51c9
ms.sourcegitcommit: 55b2bbbd47809b98c50709256885998af8b7d0c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84988718"
---
# <a name="upstream-settings"></a>Yukarı akış ayarları

Yukarı akış, SignalR hizmetinin, sunucusuz moddaki bir uç nokta kümesine ileti ve bağlantı olayları göndermesini sağlayan bir özelliktir. Yukarı akış, sunucusuz modda istemcilerden hub yöntemi çağırmak ve istemci bağlantıları bağlandığında veya bağlantısı kesildiğinde uç noktaların bildirim almasına izin vermek için kullanılabilir.

> [!NOTE]
> Yalnızca sunucusuz mod, yukarı akış ayarlarını yapılandırabilir.

## <a name="upstream-settings-details"></a>Yukarı akış ayarları ayrıntıları

Yukarı akış ayarları, hassas öğelerin sıralı bir listesinden oluşur. Her öğe `URL Template` , iletilerin nereden gönderileceğini belirten bir ve bir yapılandırma kümesi içerir `Rules` `Authentication` . Belirtilen olay gerçekleştiğinde, öğe `Rules` sırayla bir kez denetlenir ve iletiler, ilk eşleşen öğenin yukarı AKıŞ URL 'sine gönderilir.

### <a name="url-template-settings"></a>URL şablonu ayarları

URL, çeşitli desenleri desteklemek için parametreli olabilir. Önceden tanımlanmış üç parametre vardır:

|Önceden tanımlanmış parametre|Description|
|---------|---------|
|Hub| Hub, bir SignalR kavramıdır. Hub, bir yalıtım birimidir, kullanıcıların ve ileti tesliminin kapsamı bir hub ile kısıtlanır|
|alan| Kategori aşağıdaki değerlerden biri olabilir: <ul><li>**Bağlantılar**: bağlantı ömrü olayları. Bir istemci bağlantısı bağlıyken veya bağlantısı kesildiğinde tetiklenir. *Bağlı* ve *bağlantısı kesilmiş* olayları ekleme</li><li>**mesajlar**: istemciler hub metodunu çağırdığınızda tetiklenir. *Bağlantılar* kategorisi hariç diğer tüm olayları dahil etme</li></ul>|
|olay| *İleti* kategorisi için, olay, istemcilerin gönderdikleri [çağırma iletisindeki](https://github.com/dotnet/aspnetcore/blob/master/src/SignalR/docs/specs/HubProtocol.md#invocation-message-encoding) *hedeftir* . *Bağlantılar* kategorisi için yalnızca *bağlı* ve *bağlantısı kesik* kullanılır.|

Bu önceden tanımlanmış parametreler ' de kullanılabilir `URL Pattern` ve parametreler, yukarı akış URL 'si değerlendirilirken belirtilen bir değerle değiştirilmelidir. Örneğin 
```
http://host.com/{hub}/api/{category}/{event}
```
' Chat ' hub 'ındaki bir istemci bağlantısı bağlandığında, şu URL 'ye bir ileti gönderilir:
```
http://host.com/chat/api/connections/connected
```
Hub 'da bir istemci `chat` hub metodunu çağırdığında `broadcast` Şu URL 'ye bir ileti gönderilir:
```
http://host.com/chat/api/messages/broadcast
```

### <a name="rules-settings"></a>Kural ayarları

*Hub kuralları*, *Kategori kuralları* ve *olay kuralları* için kurallar oluşturabilirsiniz. Eşleşen kural üç biçimi destekler. *Olay kurallarını* örnek olarak alın:
- Herhangi bir olayla eşleştirmek için yıldız işareti (*) kullanın.
- Birden çok olayı birleştirmek için Kullanıcı virgül (,). Örneğin, `connected, disconnected` *bağlanan* ve *bağlantısı kesilen*olayları eşleştirir.
- Olayı eşleştirmek için tam olay adını kullanın. Örneğin, `connected` *bağlantılı* olayla eşleşir.

### <a name="authentication-settings"></a>Kimlik doğrulaması ayarları

Her yukarı akış ayarları öğesi için *kimlik doğrulamasını* ayrı ayrı yapılandırabilirsiniz. *Kimlik doğrulaması*yapılandırıldığında, yukarı akış Iletisinin *kimlik doğrulama* üstbilgisinde bir belirteç ayarlanır. Geçerli, SignalR hizmeti şu kimlik doğrulama türünü destekler
- Yok
- Managedıdentity kimliğine

*Managedıdentity*' yi seçtiğinizde, SignalR hizmetinde yönetilen kimliği önceden etkinleştirmeniz ve isteğe bağlı olarak bir *kaynak*belirtmeniz gerekir. Ayrıntılar için bkz. [Azure SignalR hizmeti için Yönetilen kimlikler kullanma](howto-use-managed-identity.md) .

## <a name="create-upstream-settings"></a>Yukarı akış ayarları oluşturma

### <a name="create-upstream-settings-via-azure-portal"></a>Azure portal aracılığıyla yukarı akış ayarları oluşturma

1. Azure SignalR Service 'e gidin.
2. *Ayarlar* Bland ' ye tıklayın ve *hizmet modunu* *sunucusuz*olarak değiştirin. *Yukarı akış ayarları* aşağıda gösterildiği gibi görüntülenir:

    :::image type="content" source="media/concept-upstream/upstream-portal.png" alt-text="Yukarı akış ayarları":::

3. URL 'leri *yukarı akış URL 'Si düzenine*göre doldur, *hub kuralları* gibi ayarlar varsayılan değeri gösterir.
4. *Hub kuralları*, *olay kuralları*, *Kategori kuralları* ve *yukarı akış kimlik doğrulaması*gibi ayarları ayarlamak için *hub kuralları*değerine tıklayın. Ayarları aşağıda gösterildiği gibi düzenlemenizi sağlayan bir sayfa.

    :::image type="content" source="media/concept-upstream/upstream-detail-portal.png" alt-text="Yukarı akış ayarları":::

5. *Yukarı akış kimlik doğrulamasını*ayarlamak için, önce yönetilen kimliği etkinleştirdiğinizden emin olun ve ardından *yukarı akış kimlik doğrulaması*altında *yönetilen kimliği kullan* ' ı seçin. Gereksinimlerinize göre, *kimlik doğrulama kaynak kimliği*altında herhangi bir seçeneği belirleyebilirsiniz. Ayrıntılar için bkz. [yönetilen kimliği etkinleştirme](howto-use-managed-identity.md) .

### <a name="create-upstream-settings-via-arm-template"></a>ARM şablonu aracılığıyla yukarı akış ayarları oluşturma

[Kaynak Yöneticisi şablonu](https://docs.microsoft.com/azure/azure-resource-manager/templates/overview)kullanarak yukarı akış ayarları oluşturmak için, özelliği `upstream` özelliğindeki özelliği ayarlayın `properties` . Aşağıdaki kod parçacıkları, `upstream` yukarı akış ayarlarını oluşturma ve güncelleştirme özelliğinin nasıl ayarlanacağını göstermektedir.

```JSON
{
  "properties": {
    "upstream": {
      "templates": [
        {
          "UrlTemplate": "http://host.com/{hub}/api/{category}/{event}",
          "EventPattern": "*",
          "HubPattern": "*",
          "CategoryPattern": "*",
          "Auth": {
            "Type": "ManagedIdentity",
            "ManagedIdentity": {
              "Resource": "<resource>"
            }
          }
        }
      ]
    }
  }
}
```

## <a name="signalr-serverless-protocol"></a>SignalR sunucusuz Protokolü

SignalR hizmeti aşağıdaki protokolleri izleyen uç noktalara iletiler gönderiyor.

### <a name="method"></a>Yöntem

POST

### <a name="request-header"></a>İstek üst bilgisi

|Name |Description|
|---------|---------|
|X-ASRS-bağlantı kimliği |İstemci bağlantısı için bağlantı kimliği|
|X-ASRS-hub |İstemci bağlantısının ait olduğu merkez|
|X-ASRS-kategori |İletinin ait olduğu kategori|
|X-ASRS-olay |İletinin ait olduğu olay|
|X-ASRS-Imza |Doğrulama için kullanılan HMAC. Ayrıntılar için bkz. [imza](#signature) .|
|X-ASRS-Kullanıcı talepleri |İstemci bağlantısı taleplerinin bir grubu|
|X-ASRS-Kullanıcı kimliği |İletiyi gönderen istemcinin kullanıcı kimliği|
|X-ASRS-Client-Query |İstemciler hizmete bağlandıklarında isteğin sorgusu|
|Kimlik doğrulaması |*Managedıdentity* kullanılırken isteğe bağlı bir belirteç |

### <a name="request-body"></a>İstek gövdesi

#### <a name="connected"></a>Bağlı

Content-Type: Application/JSON

#### <a name="disconnected"></a>Bağlantı kesildi

İçerik türü:`application/json`

|Name  |Tür  |Description  |
|---------|---------|---------|
|Hata |string |Bağlantının hata iletisi kapatıldı. Bağlantılar hata olmadan kapatıldığında boştur|

#### <a name="invocation-message"></a>Çağırma iletisi

İçerik türü: `application/json` veya`application/x-msgpack`

|Name  |Tür  |Description  |
|---------|---------|---------|
|Invocationıd |string | İsteğe bağlı bir dize bir çağrı iletisini temsil eder. [Etkinleştirmeleri](https://github.com/dotnet/aspnetcore/blob/master/src/SignalR/docs/specs/HubProtocol.md#invocations)içindeki ayrıntıları bulun.|
|Hedef |string | *Olay* ile aynı ve [çağırma iletisindeki](https://github.com/dotnet/aspnetcore/blob/master/src/SignalR/docs/specs/HubProtocol.md#invocation-message-encoding) *hedefle* aynı. |
|Arguments |Nesne dizisi |Hedefte başvurulan yönteme uygulanacak bağımsız değişkenleri içeren bir dizi. |

### <a name="signature"></a>İmza

Hizmet, `X-ASRS-Connection-Id` anahtar olarak hem birincil erişim anahtarını hem de ikincil erişim anahtarını kullanarak değer IÇIN SHA256 kodunu hesaplar `HMAC` ve `X-ASRS-Signature` yukarı akış için http istekleri yaparken üst bilgide ayarlar:
```
Hex_encoded(HMAC_SHA256(accessKey, connection-id))
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SignalR hizmeti için Yönetilen kimlikler kullanma](howto-use-managed-identity.md)
- [Azure SignalR Hizmeti ile Azure İşlevleri geliştirme ve yapılandırma](signalr-concept-serverless-development-config.md)
