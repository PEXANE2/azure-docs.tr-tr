---
title: 'Hızlı Başlangıç: Küçük resim oluşturma-REST, kıvrımlı'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, cURL ile Görüntü İşleme API’sini kullanarak bir görüntüden küçük resim oluşturacaksınız.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 0d430f7c9a6e9d59b20cd1420b65f70d9c6994d5
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70141431"
---
# <a name="quickstart-generate-a-thumbnail-using-the-computer-vision-rest-api-and-curl"></a>Hızlı Başlangıç: Görüntü İşleme REST API ve kıvrımlı kullanarak küçük resim oluşturma

Bu hızlı başlangıçta, Görüntü İşleme REST API kullanarak görüntüden bir küçük resim oluşturursunuz. İstenen yüksekliği ve genişliği belirtirsiniz, bu da giriş görüntüsünden en fazla farklılık gösterebilir. Görüntü İşleme, ilgilendiğiniz alanı saptamak ve bu bölge etrafında kırpma koordinatları oluşturmak için akıllı kırpma kullanır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

- [cURL](https://curl.haxx.se/windows)’niz olmalıdır.
- Görüntü İşleme için bir abonelik anahtarınız olması gerekir. Deneme bilişsel [Hizmetler](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision)'den ücretsiz bir deneme anahtarı edinebilirsiniz. Ya da Görüntü İşleme abone olmak ve anahtarınızı almak için bilişsel [Hizmetler oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ' daki yönergeleri izleyin.

## <a name="get-thumbnail-request"></a>Küçük Resim Alma isteği

[Küçük Resim Alma yöntemi](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) ile, bir görüntünün küçük resmini alabilirsiniz.

Örneği çalıştırmak için aşağıdaki adımları uygulayın:

1. Aşağıdaki kodu bir düzenleyicinin içine kopyalayın.
1. `<Subscription Key>` değerini geçerli abonelik anahtarınızla değiştirin.
1. `<File>` değerini küçük resmin kaydedileceği yol ve dosya adı ile değiştirin.
1. Gerekirse İstek URL’sini (`https://westcentralus.api.cognitive.microsoft.com/vision/v2.0`) abonelik anahtarlarınızı aldığınız konumu kullanacak şekilde değiştirin.
1. İsteğe bağlı olarak, analiz etmek için görüntüyü (`{\"url\":\"...`) değiştirin.
1. cURL yüklü bir bilgisayarda bir komut penceresi açın.
1. Kodu pencereye yapıştırıp komutu çalıştırın.

>[!NOTE]
>REST çağrınızda abonelik anahtarlarınızı almak için kullandığınız konumu kullanmanız gerekir. Örneğin, güvenlik anahtarlarınızı westus konumundan aldıysanız, aşağıdaki URL’de "westcentralus" değerini "westus" olarak değiştirin.

## <a name="create-and-run-the-sample-command"></a>Örnek komutu oluşturma ve çalıştırma

Örneği oluşturup çalıştırmak için aşağıdaki adımları uygulayın:

1. Aşağıdaki komutu bir metin düzenleyicisine kopyalayın.
1. Gerektiğinde komutta aşağıdaki değişiklikleri yapın:
    1. `<subscriptionKey>` değerini abonelik anahtarınızla değiştirin.
    1. `<thumbnailFile>` değerini, küçük resmin kaydedileceği dosyanın yolu ve adıyla değiştirin.
    1. İstek URL 'sinin (`westcentralus`) ilk kısmını kendi uç nokta URL 'inizdeki metinle değiştirin.
        [!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]
    1. İsteğe bağlı olarak, istek gövdesindeki görüntü URL’sini (`https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\`) içinden küçük resim oluşturulacak farklı bir görüntünün URL’si ile değiştirin.
1. Bir komut istemi penceresi açın.
1. Metin düzenleyicisindeki komutu komut istemi penceresine yapıştırın ve komutu çalıştırın.

    ```bash
    curl -H "Ocp-Apim-Subscription-Key: <subscriptionKey>" -o <thumbnailFile> -H "Content-Type: application/json" "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail?width=100&height=100&smartCropping=true" -d "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\"}"
    ```

## <a name="examine-the-response"></a>Yanıtı inceleme

Başarılı bir yanıt, küçük resim görüntüsünü `<thumbnailFile>` içinde belirtilen dosyaya yazar. İstek başarısız olursa yanıt, bir hata kodu ve nelerin yanlış gittiğini belirlemeye yardımcı olması için bir ileti içerir. İstek başarılı olursa ancak oluşturulan küçük resim geçerli bir görüntü dosyası değilse, abonelik anahtarınız geçersiz olabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bir görüntüyü analiz etme, ünlüler ve yer işaretlerini belirleme, küçük resim oluşturma ve yazdırılmış ve el yazısı metinleri ayıklama için Görüntü İşleme API'si keşfeder. Görüntü İşleme API'sini hızlı bir şekilde denemeniz için [Open API test konsolu](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console) konusuna bakın.

> [!div class="nextstepaction"]
> [Görüntü İşleme API’sini keşfetme](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
