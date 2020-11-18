---
title: İstemci uygulamasından bir komutu güncelleştirme
titleSuffix: Azure Cognitive Services
description: bir komutu istemci uygulamasından güncelleştirme
services: cognitive-services
author: encorona-ms
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/20/2020
ms.author: encorona
ms.openlocfilehash: 1bffb09d0f49bbd0059e8a528d67bfe215f0650d
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94654376"
---
# <a name="update-a-command-from-the-client"></a>İstemciden bir komut güncelleştirme

Bu makalede, bir istemci uygulamasından devam eden bir komutun nasıl güncelleştireceğinizi öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar
> [!div class = "checklist"]
> * Önceden oluşturulmuş [Özel Komutlar uygulaması](quickstart-custom-commands-application.md)

## <a name="update-the-state-of-a-command"></a>Komutun durumunu güncelleştirme

İstemci uygulamanız, ses girişi olmadan devam eden bir komutun durumunu güncelleştirmek istiyorsa, komutu güncelleştirmek için bir olay gönderebilirsiniz.

Bu senaryoyu göstermek için, devam eden bir komutun (turun) durumunu güncelleştirmek üzere aşağıdaki olay etkinliğini gönderebiliriz. 

```json
{
  "type": "event",
  "name": "RemoteUpdate",
  "value": {
    "updatedCommand": {
      "name": "TurnOnOff",
      "updatedParameters": {
        "OnOff": "on"
      },
      "cancel": false
    },
    "updatedGlobalParameters": {},
    "processTurn": true
  }
}
```

Bu etkinliğin anahtar özniteliklerinin gözden geçirilmesini sağlar.

| Öznitelik | Açıklama |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **tür** | Etkinlik "Event" türündedir ve olay adının "RemoteUpdate" olması gerekir. |
| **değer** | "Value" özniteliği, geçerli komutu güncelleştirmek için gerekli olan öznitelikleri içerir. |
| **updatedCommand** | "UpdatedCommand" özniteliği, komutun adını içerir, "updatedParameters" parametrelerinin adı ve güncelleştirilmiş değerlerini içeren bir eşlemedir. |
| **İptal** | Devam eden komutun iptal edilmesi gerekiyorsa, "iptal" özniteliğini true olarak ayarlayın. |
| **updatedGlobalParameters** | "UpdatedGlobalParameters" özniteliği de tıpkı "updatedParameters" gibi, genel parametreler için kullanılan bir haritadır. |
| **processTurn** | Etkinlik gönderildikten sonra, etkinliğin işlenmesi gerekiyorsa, "processTurn" özniteliğini true olarak ayarlayın. |

Bu senaryoyu özel komutlar portalında test edebilirsiniz.

1. Önceden oluşturduğunuz Özel Komutlar uygulamasını açın. 
1. Eğit ve test ' e tıklayın.
1. "Aç" gönder.
1. Yan paneli açın ve etkinlik Düzenleyicisi ' ne tıklayın.
1. Önceki bölümde belirtilen RemoteCommand olayını yazın ve gönderin.
    > [!div class="mx-imgBorder"]
    > ![Uzak komut gönder](media/custom-commands/send-remote-command-activity.png)

"OnOff" parametresinin değeri, konuşma veya metin yerine istemciden bir etkinlik kullanılarak "açık" olarak nasıl ayarlandığına ilişkin bir değer.

## <a name="update-the-catalog-of-the-parameter-for-a-command"></a>Bir komutun parametresinin kataloğunu güncelleştirme

Bir parametre için geçerli seçeneklerin listesini yapılandırdığınızda, parametre değerleri uygulama için genel olarak tanımlanır. 

Bizim örneğimizde, SubjectDevice parametresi, konuşmadan bağımsız olarak desteklenen değerlerin sabit bir listesine sahip olacaktır.

Her görüşme için parametrenin kataloğuna yeni girişler eklemeniz gereken durumlarda, aşağıdaki etkinliği gönderebilirsiniz.

```json
{
  "type": "event",
  "name": "RemoteUpdate",
  "value": {
    "catalogUpdate": {
      "commandParameterCatalogs": {
        "TurnOnOff": [
          {
            "name": "SubjectDevice",
            "values": {
              "stereo": [
                "cd player"
              ]
            }
          }
        ]
      }
    },
    "processTurn": false
  }
}
```
Bu etkinlikle, "TurnOnOff" komutunda "SubjectDevice" parametresinin kataloğuna "stereo" girdisi ekledik.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/custom-commands/update-catalog-with-remote-activity.png" alt-text="Güncelleştirme kataloğu":::

Birkaç şeyi göz önünde bulundurmanız.
1. Bu etkinliği yalnızca bir kez göndermeniz gerekir (bir bağlantıyı başlattıktan sonra ideal olarak).
1. Bu etkinliği gönderdikten sonra, olay ParameterCatalogsUpdated 'nin istemciye geri gönderilmesini beklemeniz gerekir.

## <a name="add-additional-context-from-the-client-application"></a>İstemci uygulamasından ek bağlam ekleme

İstemci uygulamasından, daha sonra özel komutlar uygulamanızda kullanılabilecek bir konuşma başına ek bağlam ayarlayabilirsiniz. 

Örneğin, özel komutlar uygulamasına bağlı olan cihazın KIMLIĞINI ve adını göndermek istediğiniz senaryoyu düşünün.

Bu senaryoyu test etmek için geçerli uygulamamızda yeni bir komut oluşturalım.
1. Getdeviceınfo adlı yeni bir komut oluşturun.
1. "Cihaz bilgilerini al" ile örnek tümce ekleyin.
1. "Bitti" tamamlanma kuralında, clientContext özniteliklerini içeren bir konuşma yanıtı gönder eylemi ekleyin.
    > ![Bağlam ile konuşma yanıtı gönder](media/custom-commands/send-speech-response-context.png)
1. Uygulamanızı kaydedin, eğitme ve test edin.
1. Sınama penceresinde bir kez, istemci bağlamını güncelleştirmek için bir etkinlik gönderin.
    > ```json
    >{
    >   "type": "event",
    >   "name": "RemoteUpdate",
    >   "value": {
    >     "clientContext": {
    >       "deviceId": "12345",
    >       "deviceName": "My device"
    >     },
    >     "processTurn": false
    >   }
    >}
    > ```
1. "Cihaz bilgilerini al" metnini gönder.
    > ![İstemci bağlamı gönderme etkinliği](media/custom-commands/send-client-context-activity.png)

Birkaç şeyi göz önünde bulundurmanız.
1. Bu etkinliği yalnızca bir kez göndermeniz gerekir (bir bağlantıyı başlattıktan sonra ideal olarak).
1. ClientContext için karmaşık nesneleri kullanabilirsiniz.
1. Istemci bağlamını kullanarak, etkinlik gönderme ve Web uç noktalarını çağırırken konuşma yanıtlarını kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web uç noktasından bir komutu güncelleştirme](./how-to-custom-commands-update-command-from-web-endpoint.md)
