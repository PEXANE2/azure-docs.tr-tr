---
title: Bir Web uç noktasından komut güncelleştirme
titleSuffix: Azure Cognitive Services
description: bir Web uç noktasından komut güncelleştirme
services: cognitive-services
author: encorona-ms
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/20/2020
ms.author: encorona
ms.openlocfilehash: 4432843ac93002bc92068db191706352234d76e6
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94571292"
---
# <a name="update-a-command-from-a-web-endpoint"></a>Bir Web uç noktasından komut güncelleştirme

İstemci uygulamanız, ses girişi olmadan devam eden bir komutun durumunu güncelleştirmek istiyorsa, komutu güncelleştirmek için bir Web uç noktasına çağrı kullanabilirsiniz.

Bu makalede, bir Web uç noktasından devam eden bir komutun nasıl güncelleştireceğinizi öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar
> [!div class = "checklist"]
> * Önceden oluşturulmuş [Özel Komutlar uygulaması](quickstart-custom-commands-application.md)

## <a name="create-an-azure-function"></a>Azure İşlevi oluşturma 

Bu örnekte, aşağıdaki girişi (veya bu girişin bir alt kümesini) destekleyen HTTP-Triggered bir [Azure işlevi](https://docs.microsoft.com/azure/azure-functions/) gerekir.

```JSON
{
  "conversationId": "SomeConversationId",
  "currentCommand": {
    "name": "SomeCommandName",
    "parameters": {
      "SomeParameterName": "SomeParameterValue",
      "SomeOtherParameterName": "SomeOtherParameterValue"
    }
  },
  "currentGlobalParameters": {
      "SomeGlobalParameterName": "SomeGlobalParameterValue",
      "SomeOtherGlobalParameterName": "SomeOtherGlobalParameterValue"
  }
}
```

Bu girdinin anahtar özniteliklerinin gözden geçirilmesini sağlar.

| Öznitelik | Açıklama |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **ConversationId** | "konuşma kimliği", görüşmenin benzersiz tanımlayıcısıdır ve bu kimliğin istemci uygulamasından oluşturulup oluşturulmadığını unutmayın. |
| **currentCommand** | "currentCommand", şu anda konuşmada etkin olan komuttur. |
| **ada** | "ad" komutun adıdır ve "parametreler" parametrelerin geçerli değerleriyle bir eşlemedir. |
| **currentGlobalParameters** | "currentGlobalParameters" Ayrıca "Parameters" gibi bir haritadır, ancak genel parametreler için kullanılır. |

Azure Işlevinin çıktısının aşağıdaki biçimi desteklemesi gerekir.

```JSON
{
  "updatedCommand": {
    "name": "SomeCommandName",
    "updatedParameters": {
      "SomeParameterName": "SomeParameterValue"
    },
    "cancel": false
  },
  "updatedGlobalParameters": {
    "SomeGlobalParameterName": "SomeGlobalParameterValue"
  }
}
```

[İstemciden bir komut güncelleştirilirken](./how-to-custom-commands-update-command-from-client.md)kullanılan aynı olduğundan, bu biçimi tanınduyabilirsiniz. 

Şimdi NodeJS tabanlı bir Azure Işlevi oluşturun ve bu kodu kopyalayın ve yapıştırın

```nodejs
module.exports = async function (context, req) {
    context.log(req.body);
    context.res = {
        body: {
            updatedCommand: {
                name: "IncrementCounter",
                updatedParameters: {
                    Counter: req.body.currentCommand.parameters.Counter + 1
                }
            }
        }
    };
}
```

Özel komutlardan bu Azure Işlevini çağırdığımızda, görüşmenin geçerli değerlerini göndereceğiz ve güncelleştirmek istediğimiz parametreleri geri döneceğiz veya geçerli komutu iptal etmek istiyoruz.

## <a name="update-the-existing-custom-commands-app"></a>Var olan özel komutlar uygulamasını güncelleştirme

Şimdi, mevcut özel komutlar uygulamasıyla Azure Işlevini yedeklim.

1. Incrementcounter adlı yeni bir komut ekleyin.
1. "Increment" değeriyle yalnızca bir örnek cümle ekleyin.
1. Varsayılan değeri 0 olan tür numarası olan sayaç adlı yeni bir parametre (yukarıdaki Azure Işlevinde belirtilen aynı ad) ekleyin.
1. Azure Işlevinizin URL 'siyle birlikte IncrementEndpoint adlı yeni bir Web uç noktası ekleyin ve uzak güncelleştirmeler etkindir.
    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/custom-commands/set-web-endpoint-with-remote-updates.png" alt-text="Web uç noktasını uzak güncelleştirmelerle ayarla":::
1. "IncrementRule" adlı yeni bir etkileşim kuralı oluşturun ve bir çağrı Web uç noktası eylemi ekleyin.
    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/custom-commands/increment-rule-web-endpoint.png" alt-text="Artış kuralı":::
1. Eylem yapılandırmasında IncrementEndpoint öğesini seçin, sayaç değeriyle konuşma yanıtı göndermek için başarılı olarak yapılandırın ve hata iletisiyle başarısız yapın.
    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/custom-commands/set-increment-counter-call-endpoint.png" alt-text="Artış sayacı çağrı uç noktasını ayarla":::
1. Kullanıcının girişini beklemek için kuralın yürütme sonrası durumunu ayarlayın

## <a name="test-it"></a>Test etme

1. Uygulamanızı kaydetme ve eğitme
1. Test 'e tıklayın
1. Birkaç kez "artış" (ıncrementcounter komutu için örnek cümle) gönderin
    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/custom-commands/increment-counter-example.png" alt-text="Artış sayacı örneği":::

Her bir Azure Işlevi tarafından her bir sayaç parametresinin değerinin nasıl artdığına dikkat edin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özel Komutlar uygulamanız için CI/CD sürecini etkinleştirme](./how-to-custom-commands-deploy-cicd.md)