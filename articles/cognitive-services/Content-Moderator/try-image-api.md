---
title: API Konsolu ile Orta görüntü - İçerik Moderatör
titleSuffix: Azure Cognitive Services
description: Görüntü içeriği için tarayıp denetleme denetleme iş akışlarını başlatmak için Azure İçerik Moderatörü'ndeki Görüntü Moderasyonu API'sini kullanın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: pafarley
ms.openlocfilehash: 714621fdcc307ee8b29567fc0d95ca41d31aa9e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75448251"
---
# <a name="moderate-images-from-the-api-console"></a>API konsolundan orta dereceli görüntüler

Görüntü içeriği için tarayıp denetleme denetleme iş akışlarını başlatmak için Azure İçerik Moderatörü'ndeki [Görüntü Moderasyonu API'sini](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c) kullanın. Ilımlılık işi içeriğinizi küfür için tarar ve özel ve paylaşılan engelleme listeleriyle karşılaştırır.

## <a name="use-the-api-console"></a>API konsolunu kullanma
API'yi çevrimiçi konsolda test etmeden önce abonelik anahtarınız gerekir. Bu, **Ayarlar** sekmesinde, **Ocp-Apim-Abonelik-Anahtar** kutusunda bulunur. Daha fazla bilgi için bkz. [Genel Bakış](overview.md).

1. Görüntü [Moderasyon API başvurusuna](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c)gidin.

   Resim - Görüntü ılımlılığı **değerlendirsayfası** açılır.

2. **API test konsolunu Aç**için, konumunuzu en yakından açıklayan bölgeyi seçin. 

   ![Resmi Deneyin - Sayfa bölge seçimini değerlendirin](images/test-drive-region.png)
  
   **Image - Evaluate** API konsolu açılır.

3. **Ocp-Apim-Abonelik-Anahtar** kutusuna abonelik anahtarınızı girin.

   ![Çalışmayı deneyin Resim - Konsol abonelik anahtarını değerlendirin](images/try-image-api-1.PNG)

4. İstek **gövdesi** kutusunda, varsayılan örnek görüntüyü kullanın veya tçalışmak için bir resim belirtin. Resmin kendisini ikili bit verisi olarak gönderebilir veya bir resim için herkese açık bir URL belirtebilirsiniz. 

   Bu örnekiçin, **İstek gövdesi** kutusunda sağlanan yolu kullanın ve sonra **Gönder'i**seçin. 

   ![Çalışmayın Görüntü - Konsol İstek gövdesini değerlendirin](images/try-image-api-2.PNG)

   Bu URL'deki resim:

   ![Çalışmayıdeneyin Resim - Konsol örnek görüntüsünü değerlendirin](images/sample-image.jpg) 

5. **Gönder**’i seçin.

6. API, her sınıflandırma için bir olasılık puanı döndürür. Ayrıca görüntünün koşulları karşılayıp karşılamadığını **(doğru** veya **yanlış)** bir belirleme yi verir. 

   ![Çalışmayız Görüntü - Konsol olasılık puanı ve koşul belirlemeyi değerlendirin](images/try-image-api-3.PNG)

## <a name="face-detection"></a>Yüz algılama

Görüntüdeki yüzleri bulmak için Görüntü Moderasyon API'sini kullanabilirsiniz. Bu seçenek, gizlilik le ilgili endişeleriniz olduğunda ve belirli bir yüzün platformunuzda yayınlanmasını engellemek istediğinizde yararlı olabilir. 

1. Görüntü [Moderasyon API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c)referansında, sol menüde, **Resim**altında, **Yüzleri Bul'u**seçin. 

   **Resim - Yüzleri Bul** sayfası açılır.

2. **API test konsolunu Aç**için, konumunuzu en yakından açıklayan bölgeyi seçin. 

   ![Resim'i deneyin - Yüzleri Bul sayfa bölge seçimi](images/test-drive-region.png)

   **Resim - Yüzleri Bul** API konsolu açılır.

3. Taran bir görüntü belirtin. Görüntünün kendisini ikili bit verisi olarak gönderebilir veya bir görüntüiçin herkese açık bir URL belirtebilirsiniz. Bu örnek, BIR CNN hikayesinde kullanılan bir resme bağlanır.

   ![Resmi Deneyin - Yüzleri Bul örnek görüntü](images/try-image-api-face-image.jpg)

   ![Resmi Deneyin - Yüzleri Bul örnek isteği](images/try-image-api-face-request.png)

4. **Gönder**’i seçin. Bu örnekte, API iki yüz bulur ve görüntüdeki koordinatlarını döndürür.

   ![Resmi Deneyin - Yüzleri Bul örnek Yanıt içerik kutusu](images/try-image-api-face-response.png)

## <a name="text-detection-via-ocr-capability"></a>OCR özelliği ile metin algılama

Görüntülerdeki metni algılamak için İçerik Moderatör OCR özelliğini kullanabilirsiniz.

1. Görüntü [Moderasyon API referans,](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c)sol menüde, **Resim**altında, **OCR**seçin. 

   **Resim - OCR** sayfası açılır.

2. **API test konsolunu Aç**için, konumunuzu en yakından açıklayan bölgeyi seçin. 

   ![Resim - OCR sayfa bölge seçimi](images/test-drive-region.png)

   **Image - OCR** API konsolu açılır.

3. **Ocp-Apim-Abonelik-Anahtar** kutusuna abonelik anahtarınızı girin.

4. İstek **gövde** kutusunda varsayılan örnek resmi kullanın. Bu, önceki bölümde kullanılan görüntünün aynısI.

5. **Gönder**’i seçin. Çıkarılan metin JSON'da görüntülenir:

   ![Resim - OCR örnek Yanıt içerik kutusu](images/try-image-api-ocr.PNG)

## <a name="next-steps"></a>Sonraki adımlar

Kodunuzda REST API'yi kullanın veya uygulamanıza görüntü Moderasyonu eklemek için [.NET SDK'yı hızlı bir şekilde izleyin.](dotnet-sdk-quickstart.md)
