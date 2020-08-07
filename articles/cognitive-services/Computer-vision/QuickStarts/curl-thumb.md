---
title: 'Hızlı başlangıç: küçük resim oluşturma-REST, kıvrımlı'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, cURL ile Görüntü İşleme API’sini kullanarak bir görüntüden küçük resim oluşturacaksınız.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 08/05/2020
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 1c2ab3f0ef84007d8b9233c45d39fcfbacb5eaab
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87835315"
---
# <a name="quickstart-generate-a-thumbnail-using-the-computer-vision-rest-api-and-curl"></a>Hızlı başlangıç: Görüntü İşleme REST API ve kıvrımlı kullanarak küçük resim oluşturma

Bu hızlı başlangıçta, Görüntü İşleme REST API kullanarak görüntüden bir küçük resim oluşturursunuz. İstenen yüksekliği ve genişliği belirtirsiniz, bu da giriş görüntüsünden en fazla farklılık gösterebilir. Görüntü İşleme, ilgilendiğiniz alanı saptamak ve bu bölge etrafında kırpma koordinatları oluşturmak için akıllı kırpma kullanır.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services/) 
* [cURL](https://curl.haxx.se/)
* Azure aboneliğiniz olduktan sonra, <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision"  title=" "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> anahtarınızı ve uç noktanızı almak için Azure Portal bir görüntü işleme kaynağı oluşturun görüntü işleme bir kaynak oluşturun. Dağıtıldıktan sonra **Kaynağa Git ' e**tıklayın.
  * Uygulamanızı Görüntü İşleme hizmetine bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
  * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.

## <a name="create-and-run-the-sample-command"></a>Örnek komutu oluşturma ve çalıştırma

Örneği oluşturup çalıştırmak için aşağıdaki adımları uygulayın:

1. Aşağıdaki komutu bir metin düzenleyicisine kopyalayın.
1. Gerektiğinde komutta aşağıdaki değişiklikleri yapın:
    1. `<subscriptionKey>` değerini abonelik anahtarınızla değiştirin.
    1. `<thumbnailFile>` değerini, küçük resmin kaydedileceği dosyanın yolu ve adıyla değiştirin.
    1. İstek URL 'sinin () ilk kısmını `westcentralus` kendi uç nokta URL 'inizdeki metinle değiştirin.
        [!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]
    1. İsteğe bağlı olarak, istek gövdesindeki görüntü URL’sini (`https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\`) içinden küçük resim oluşturulacak farklı bir görüntünün URL’si ile değiştirin.
1. Bir komut istemi penceresi açın.
1. Komutu metin düzenleyicisinden komut istemi penceresine yapıştırın.
1. Programı çalıştırmak için ENTER tuşuna basın.

    ```bash
    curl -H "Ocp-Apim-Subscription-Key: <subscriptionKey>" -o <thumbnailFile> -H "Content-Type: application/json" "https://westus.api.cognitive.microsoft.com/vision/v3.0/generateThumbnail?width=100&height=100&smartCropping=true" -d "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\"}"
    ```

## <a name="examine-the-response"></a>Yanıtı inceleme

Başarılı bir yanıt, küçük resim görüntüsünü `<thumbnailFile>` içinde belirtilen dosyaya yazar. İstek başarısız olursa yanıt, bir hata kodu ve nelerin yanlış gittiğini belirlemeye yardımcı olması için bir ileti içerir. İstek başarılı olursa ancak oluşturulan küçük resim geçerli bir görüntü dosyası değilse, abonelik anahtarınız geçersiz olabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bir görüntüyü analiz etme, ünlüler ve yer işaretlerini belirleme, küçük resim oluşturma ve yazdırılmış ve el yazısı metinleri ayıklama için Görüntü İşleme API'si keşfeder. Görüntü İşleme API'sini hızlı bir şekilde denemeniz için [Open API test konsolu](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-ga/operations/56f91f2e778daf14a499f20c) konusuna bakın.

> [!div class="nextstepaction"]
> [Görüntü İşleme API’sini keşfetme](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-ga/operations/56f91f2e778daf14a499f21b)
