---
title: Özel anahtar sözcükler oluşturma-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Cihazınız her zaman bir anahtar sözcüğü (veya tümceciği) dinler. Kullanıcı anahtar sözcüğünü söyyorsa, cihaz sonraki tüm sesleri buluta gönderir ve Kullanıcı konuşmayı durduruncaya kadar. Anahtar keliinizi özelleştirmek, cihazınızı ayırt etmenin ve markanızı güçlendirmenin etkili bir yoludur.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/20/2019
ms.author: trbye
ms.custom: devx-track-csharp
ms.openlocfilehash: d80f244f7b5e17d730451093070b971e9aa041b9
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88919021"
---
# <a name="custom-keyword-basics"></a>Özel anahtar sözcük hakkındaki temel bilgiler

Bu makalede, konuşma Studio ve konuşma SDK 'sını kullanarak özel anahtar sözcüklerle çalışmanın temellerini öğreneceksiniz. Anahtar sözcüğü, ürününüzün ses özellikli olmasını sağlayan bir kelime veya kısa bir tümceciktir. Anahtar sözcük modellerini, konuşma Studio 'da oluşturursunuz ve ardından, uygulamalarınızda konuşma SDK 'Sı ile kullandığınız bir model dosyasını dışarı aktarabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımlarda bir konuşma aboneliği ve konuşma SDK 'Sı gerekir. Henüz bir aboneliğiniz yoksa [konuşma hizmetini ücretsiz deneyin](get-started.md). SDK 'yı almak için platformunuzun [Install Guide](quickstarts/setup-platform.md) bölümüne bakın.

## <a name="create-a-keyword-in-speech-studio"></a>Konuşma Studio 'da anahtar sözcük oluşturma

Özel bir anahtar sözcüğü kullanabilmeniz için, [konuşma Studio](https://aka.ms/sdsdk-speechportal)'Daki [özel anahtar](https://aka.ms/sdsdk-wakewordportal) sözcük sayfasını kullanarak bir anahtar sözcük oluşturmanız gerekir. Anahtar sözcük sağlamadıktan sonra, `.table` konuşma SDK 'sı ile kullanabileceğiniz bir dosya oluşturur.

> [!IMPORTANT]
> Özel anahtar sözcük modelleri ve elde edilen `.table` dosyalar **yalnızca** konuşma Studio 'da oluşturulabilir.
> SDK 'dan veya REST çağrılarıyla özel anahtar sözcükler oluşturamazsınız.

1. [Konuşma Studio](https://aka.ms/sdsdk-speechportal) 'ya gidin ve **oturum açın** veya henüz bir konuşma aboneliğiniz yoksa, [**abonelik oluştur**](https://go.microsoft.com/fwlink/?linkid=2086754)' u seçin.

1. [Özel anahtar sözcük](https://aka.ms/sdsdk-wakewordportal) sayfasında **Yeni bir proje**oluşturun. 

1. Bir **ad**, isteğe bağlı bir **Açıklama**girin ve dili seçin. Dil başına bir proje gerekir ve şu anda dille sınırlı destek vardır `en-US` .

    ![Anahtar sözcük projenizi açıkla](media/custom-keyword/custom-kws-portal-new-project.png)

1. Listeden projenizi seçin. 

    ![Anahtar sözcük projenizi seçin](media/custom-keyword/custom-kws-portal-project-list.png)

1. Yeni bir anahtar sözcük modeli oluşturmak için **modeli eğitme**' ye tıklayın.

1. Model için bir **ad** , isteğe bağlı bir **Açıklama**ve istediğiniz **anahtar sözcüğü** girin ve ardından **İleri**' ye tıklayın. Etkin anahtar sözcük seçme [yönergelerine](speech-devices-sdk-kws-guidelines.md#choose-an-effective-keyword) bakın.

    ![Anahtar keliinizi girin](media/custom-keyword/custom-kws-portal-new-model.png)

1. Portal, anahtar kelime için aday söylenişleri oluşturur. Oynat düğmelerine tıklayarak her bir adayı dinleyin ve yanlış olan herhangi bir söylenin yanındaki denetimleri kaldırın. Yalnızca güzel Söylenişler denetlendikten sonra, anahtar sözcük modelini oluşturmaya başlamak için **eğitme** ' ye tıklayın. 

    ![Anahtar keliinizi gözden geçirin](media/custom-keyword/custom-kws-portal-choose-prons.png)

1. Modelin oluşturulması otuz dakika kadar sürebilir. Model tamamlandığında anahtar sözcük listesi **Işlemden** **başarılı** olarak değişecektir. Sonra dosyayı indirebilirsiniz.

    ![Anahtar keliinizi gözden geçirin](media/custom-keyword/custom-kws-portal-download-model.png)

1. İndirilen dosya bir `.zip` Arşiv. Arşivi ayıklayın ve uzantılı bir dosya görürsünüz `.table` . Bu, sonraki bölümde SDK ile birlikte kullandığınız dosyadır. bu nedenle yolunu aklınızda olduğunuzdan emin olun. dosya adı, anahtar kelimesinin adını yansıtır, örneğin bir anahtar sözcük **etkinleştirme cihazı** dosya adına sahiptir `Activate_device.table` .

## <a name="use-a-keyword-model-with-the-sdk"></a>SDK ile anahtar sözcük modeli kullanma

İlk olarak, `FromFile()` bir döndüren statik işlevini kullanarak anahtar sözcük modeli dosyanızı yükleyin `KeywordRecognitionModel` . `.table`Konuşma Studio 'dan indirdiğiniz dosyanın yolunu kullanın. Ayrıca, `AudioConfig` varsayılan mikrofonu kullanarak bir oluşturun ve ardından Ses Yapılandırması ' nı kullanarak yeni bir örneğini oluşturun `KeywordRecognizer` .

```csharp
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;

var keywordModel = KeywordRecognitionModel.FromFile("your/path/to/Activate_device.table");
using var audioConfig = AudioConfig.FromDefaultMicrophoneInput();
using var keywordRecognizer = new KeywordRecognizer(audioConfig);
```

Ardından, anahtar sözcük tanımayı çalıştırmak, `RecognizeOnceAsync()` model nesneniz geçirerek bir çağrısıyla yapılır. Bu, anahtar sözcük tanınana kadar olan bir anahtar sözcük tanıma oturumu başlatır. Bu nedenle, genellikle bu tasarım modelini çok iş parçacıklı uygulamalarda veya bir uyandırma sözcüğünün süresiz olarak bekleyebilirsiniz kullanım durumlarında kullanırsınız.

```csharp
KeywordRecognitionResult result = await keywordRecognizer.RecognizeOnceAsync(keywordModel);
```

> [!NOTE]
> Burada gösterilen örnek, `SpeechConfig` kimlik doğrulama bağlamı için bir nesne gerektirmediğinden ve arka uca başvurmadığından yerel anahtar sözcük tanımayı kullanır. Ancak, her iki anahtar sözcük tanıma ve doğrulamayı [sürekli bir arka uç bağlantısı](https://docs.microsoft.com/azure/cognitive-services/speech-service/tutorial-voice-enable-your-bot-speech-sdk#view-the-source-code-that-enables-keyword)kullanarak çalıştırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Konuşma CIHAZLARı SDK hızlı başlangıç](https://aka.ms/sdsdk-quickstart)ile özel anahtar keliinizi test edin.
