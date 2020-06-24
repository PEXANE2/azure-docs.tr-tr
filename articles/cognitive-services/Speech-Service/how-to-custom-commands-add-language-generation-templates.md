---
title: 'Nasıl yapılır: konuşma yanıtları için dil oluşturma şablonlarını kullanma-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu makalede, dil oluşturma şablonlarını özel komutlarla nasıl kullanacağınızı öğreneceksiniz. Dil oluşturma şablonları, istemciye gönderilen yanıtları özelleştirmenize olanak tanır ve yanıtlara varyansı ortaya çıkarabilir.
services: cognitive-services
author: singhsaumya
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/18/2020
ms.author: sausin
ms.openlocfilehash: 0cbc57922b31f1b3879bb2cad8a988a1ba4cc368
ms.sourcegitcommit: 4042aa8c67afd72823fc412f19c356f2ba0ab554
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85307961"
---
# <a name="add-language-generation-templates-for-speech-responses"></a>Konuşma yanıtları için dil oluşturma şablonları ekleme

Bu makalede, dil oluşturma şablonlarını özel komutlarla nasıl kullanacağınızı öğreneceksiniz. Dil oluşturma şablonları, istemciye gönderilen yanıtları özelleştirmenize olanak tanır ve yanıtlara varyansı ortaya çıkarabilir. Dil oluşturma özelleştirmesi şu şekilde sağlanabilir:

- Dil oluşturma şablonlarının kullanımı
- Uyarlamalı ifadelerin kullanımı

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki makalelerde bulunan adımları tamamlamış olmanız gerekir:

> [!div class="checklist"]
> * [Nasıl yapılır: basit komutlarla uygulama oluşturma](./how-to-custom-commands-create-application-with-simple-commands.md)
> * [Nasıl yapılır: komutlara parametreler ekleme](./how-to-custom-commands-add-parameters-to-commands.md)

## <a name="language-generation-templates-overview"></a>Dil üretimi şablonlarına genel bakış

Özel komutlar şablonları, BotFramework 'in [LG şablonlarını](https://aka.ms/speech/cc-lg-format)temel alır. Özel komutlar gerektiğinde yeni bir LG şablonu oluşturduğundan (yani, parametrelerde veya eylemlerdeki konuşma yanıtları için) LG şablonunun adını belirtmeniz gerekmez. Bu nedenle, şablonunuzu şöyle tanımlamak yerine şunları yapın:

 ```
    # CompletionAction
    - Ok, turning {OnOff} the {SubjectDevice}
    - Done, turning {OnOff} the {SubjectDevice}
    - Proceeding to turn {OnOff} {SubjectDevice}
 ```

Yalnızca adı olmayan şablonun gövdesini aşağıdaki gibi tanımlamanız gerekir.

> [!div class="mx-imgBorder"]
> ![Şablon Düzenleyicisi örneği](./media/custom-commands/template-editor-example.png)


Bu değişiklik, istemciye gönderilen konuşma yanıtlarının çeşitlemesini tanıtır. Bu nedenle, aynı utterlik için ilgili konuşma yanıtı, belirtilen seçeneklerin dışında rastgele kullanıma sunulur.

LG şablonlarından yararlanmak, uyarlamalı ifadeler kullanan komutlar için karmaşık konuşma yanıtları tanımlamanızı da sağlar. Daha fazla ayrıntı için [LG şablonları biçimine](https://aka.ms/speech/cc-lg-format) bakabilirsiniz. Varsayılan olarak özel komutlar, aşağıdaki küçük farklılıklar ile tüm özellikleri destekler:

* LG şablonları varlıklarda $ {entityName} olarak temsil edilir. Özel komutlarda varlıkları kullanmıyoruz, ancak parametreler $ {parameterName} veya {parameterName} gösterimlerden biriyle değişken olarak kullanılabilir
* Şablon kompozisyonu ve genişletmesi özel komutlarda desteklenmez. Bunun nedeni, dosyayı doğrudan düzenlememelisiniz `.lg` , ancak yalnızca otomatik olarak oluşturulan şablonların yanıtları.
* LG tarafından eklenen özel işlevler özel komutlarda desteklenmez. Önceden tanımlanmış işlevler hala desteklenmektedir.
* Seçenekler (Strict, yeniden Değiştir & lineBreakStyle) özel komutlarda desteklenmez.

## <a name="add-template-responses-to-turnonoff-command"></a>TurnOnOff komutuna şablon yanıtları ekleme

Şu yapılandırmayla yeni bir parametre eklemek için **Turnonoff** komutunu değiştirin:

| Ayar            | Önerilen değer       | 
| ------------------ | --------------------- | 
| Name               | `SubjectContext`         | 
| Geneldir          | unchecked             | 
| Gerekli           | unchecked               | 
| Tür               | Dize                |
| Varsayılan değer      | `all` |
| Yapılandırma      | Önceden tanımlanmış giriş değerlerini iç katalogdan kabul et | 
| Önceden tanımlanmış giriş değerleri | `room`, `bathroom`, `all`|

### <a name="modify-completion-rule"></a>Tamamlanma kuralını Değiştir

Mevcut tamamlama kuralı **ConfirmationResponse**' nın **Eylemler** bölümünü düzenleyin. **Eylem düzenleme** açılır penceresinde, **Şablon Düzenleyicisi** ' ne geçin ve metni aşağıdaki örnekle değiştirin.

```
- IF: @{SubjectContext == "all" && SubjectDevice == "lights"}
    - Ok, turning all the lights {OnOff}
- ELSEIF: @{SubjectDevice == "lights"}
    - Ok, turning {OnOff} the {SubjectContext} {SubjectDevice}
- ELSE:
    - Ok, turning the {SubjectDevice} {OnOff}
    - Done, turning {OnOff} the {SubjectDevice}
```

Uygulamanızı aşağıdaki şekilde **eğitme** ve **Test** etme. Şablon değerinin birden çok alternatifine ve ayrıca Uyarlamalı ifadelerin kullanımına bağlı olarak, yanıtların çeşitlemesine dikkat edin.

* Giriş: TV 'yi açın
* Çıkış: Tamam, TV 'yi açma
* Giriş: TV 'yi açın
* Çıkış: bitti, TV açık
* Giriş: ışıkları kapatma
* Çıkış: Tamam, tüm ışıkları kapatma
* Giriş: Oda ışıklarını kapatma
* Çıkış: Tamam, Oda ışıklarını kapatma

## <a name="use-custom-voice"></a>Özel ses kullan

Özel komutlar yanıtlarını özelleştirmenin başka bir yolu da özel bir çıkış sesi seçkullanmaktır. Varsayılan sesi özel bir sese geçirmek için aşağıdaki adımları kullanın.

1. Özel komutlar uygulamanızda, sol bölmeden **Ayarlar** ' ı seçin.
1. Orta bölmeden **özel ses** ' i seçin.
1. Tablodan istenen özel veya genel sesi seçin.
1. **Kaydet**’i seçin.

> [!div class="mx-imgBorder"]
> ![Parametrelere sahip örnek cümleler](media/custom-commands/select-custom-voice.png)

> [!NOTE]
> - **Genel sesler**için **sinir türleri** yalnızca belirli bölgelerde kullanılabilir. Kullanılabilirliği denetlemek için [bölgeye/uç noktaya göre standart ve sinir seslikleri](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#standard-and-neural-voices)bölümüne bakın.
> - **Özel sesler**Için özel ses projesi sayfasından oluşturulabilirler. Bkz. [özel sesle çalışmaya başlama](./how-to-custom-voice.md).

Artık uygulama, varsayılan ses yerine seçili sesle yanıt verir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma SDK 'sını kullanarak özel komutlarınızı tümleştirin](./how-to-custom-commands-setup-speech-sdk.md).