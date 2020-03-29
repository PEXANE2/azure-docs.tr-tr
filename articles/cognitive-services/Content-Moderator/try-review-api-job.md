---
title: REST API konsolu ile ılımlılık işlerini kullanma - İçerik Moderatör
titleSuffix: Azure Cognitive Services
description: Azure İçerik Moderatörü'nde resim veya metin içeriği için uçlardan uca içerik denetleme işleri başlatmak için Gözden Geçirme API'sinin iş işlemlerini kullanın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 10/24/2019
ms.author: pafarley
ms.openlocfilehash: 83ee8e0c0583cba72da8702e196f0f38128f8d8a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "72935963"
---
# <a name="define-and-use-moderation-jobs-rest"></a>Moderasyon işlerini tanımlayın ve kullanın (REST)

Bir ılımlılık iş içerik ılımlılık, iş akışları ve değerlendirmeleri işlevselliği için sarıcı bir tür olarak hizmet vermektedir. Bu kılavuz, içerik denetleme işlerini başlatmak ve denetlemek için IŞ REST API'larını nasıl kullanacağınızı gösterir. API'lerin yapısını anladıktan sonra, bu çağrıları REST uyumlu herhangi bir platforma kolayca iletebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- İçerik Moderatör [İnceleme araç](https://contentmoderator.cognitive.microsoft.com/) sitesinde oturum açın veya bir hesap oluşturun.
- (İsteğe bağlı) İşinizle birlikte kullanmak üzere [özel bir iş akışı tanımlayın;](./Review-Tool-User-Guide/Workflows.md) varsayılan iş akışını da kullanabilirsiniz.

## <a name="create-a-job"></a>Bir iş oluşturma

Bir ılımlılık işi oluşturmak [için, İş - ApI](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5) başvuru sayfası oluşturun ve abonelik bölgeniz için düğmeyi seçin (bunu [Gözden Geçirme aracının](https://contentmoderator.cognitive.microsoft.com/) **Kimlik Bilgileri** sayfasındaKi Bitiş Noktası URL'sinde bulabilirsiniz). Bu, REST API çağrılarını kolayca oluşturabileceğiniz ve çalıştırabileceğiniz API konsolu'nu başlatır.

![İş - Sayfa bölge seçimi oluşturma](images/test-drive-job-1.png)

### <a name="enter-rest-call-parameters"></a>REST çağrı parametrelerini girin

REST çağrısını oluşturmak için aşağıdaki değerleri girin:

- **teamName**: [İnceleme aracı](https://contentmoderator.cognitive.microsoft.com/) hesabınızı ayarlarken oluşturduğunuz takım kimliği (İnceleme aracınızın Kimlik Bilgileri ekranında **Kimlik** alanında bulunur).
- **ContentType**: Bu "Resim", "Metin" veya "Video" olabilir.
- **ContentId**: Özel tanımlayıcı dize. Bu dize API'ye aktarılır ve geri arama yoluyla döndürülür. İç tanımlayıcıları veya meta verileri ılımlılık işinin sonuçlarıyla ilişkilendirmede yararlıdır.
- **İş Akışı Adı**: Daha önce oluşturduğunuz iş akışının adı (veya varsayılan iş akışı için "varsayılan").
- **CallbackEndpoint**: (İsteğe bağlı) İnceleme tamamlandığında geri arama bilgilerini almak için URL.
- **Ocp-Apim-Abonelik-Anahtar**: İçerik Moderatör anahtarınız. Bunu [Gözden Geçir aracının](https://contentmoderator.cognitive.microsoft.com) **Ayarlar** sekmesinde bulabilirsiniz.

### <a name="fill-in-the-request-body"></a>İstek gövdesini doldurma

REST aramanızın gövdesi bir alan içerir, **ContentValue**. Metni moderediyorsanız ham metin içeriğine yapıştırın veya görüntü/video moderating ediyorsanız bir resim veya video URL'si girin. Aşağıdaki örnek resim URL'sini kullanabilirsiniz:[https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg](https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg)

![İş - Konsol sorgu parametreleri, üstbilgi ve İstek gövde kutusu oluşturma](images/job-api-console-inputs.PNG)

### <a name="submit-your-request"></a>İsteğinizi gönderin

**Gönder**’i seçin. İşlem başarılı olursa, **Yanıt** durumu `200 OK`ve **Yanıt içerik** kutusu iş için bir kimlik görüntüler. Aşağıdaki adımlarda kullanmak üzere bu kimliği kopyalayın.

![İnceleme - Konsol Oluştur Yanıt içerik kutusu gözden geçirme kimliğini görüntüler](images/test-drive-job-3.PNG)

## <a name="get-job-status"></a>İş durumunu alma

Çalışan veya tamamlanmış bir işin durumunu ve ayrıntılarını almak için [İş - ApI](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c3) başvuru sayfasına gidin ve bölgenizin (anahtarınızın yönetildiği bölge) düğmesini seçin.

![İş - Bölge seçimini alın](images/test-drive-region.png)

Yukarıdaki bölümde olduğu gibi REST arama parametrelerini girin. Bu adım için **JobId,** işi oluşturduğunuzda aldığınız benzersiz kimlik dizesidir. **Gönder**’i seçin. İşlem başarılı olursa, **Yanıt** durumu `200 OK`ve **Yanıt içerik** kutusu işi aşağıdaki gibi JSON biçiminde görüntüler:

```json
{  
  "Id":"2018014caceddebfe9446fab29056fd8d31ffe",
  "TeamName":"some team name",
  "Status":"Complete",
  "WorkflowId":"OCR",
  "Type":"Image",
  "CallBackEndpoint":"",
  "ReviewId":"201801i28fc0f7cbf424447846e509af853ea54",
  "ResultMetaData":[  
    {  
      "Key":"hasText",
      "Value":"True"
    },
    {  
      "Key":"ocrText",
      "Value":"IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n"
    }
  ],
  "JobExecutionReport":[  
    {  
      "Ts":"2018-01-07T00:38:29.3238715",
      "Msg":"Posted results to the Callbackendpoint: https://requestb.in/vxke1mvx"
    },
    {  
      "Ts":"2018-01-07T00:38:29.2928416",
      "Msg":"Job marked completed and job content has been removed"
    },
    {  
      "Ts":"2018-01-07T00:38:29.0856472",
      "Msg":"Execution Complete"
    },
    {  
      "Ts":"2018-01-07T00:38:26.7714671",
      "Msg":"Successfully got hasText response from Moderator"
    },
    {  
      "Ts":"2018-01-07T00:38:26.4181346",
      "Msg":"Getting hasText from Moderator"
    },
    {  
      "Ts":"2018-01-07T00:38:25.5122828",
      "Msg":"Starting Execution - Try 1"
    }
  ]
}
```

![İş - REST çağrı yanıtı alın](images/test-drive-job-5.png)

### <a name="examine-the-new-reviews"></a>Yeni inceleme(ler) inceleyin

İçerik işiniz bir incelemenin oluşturulmasıyla sonuçlandıysa, inceleme [aracında](https://contentmoderator.cognitive.microsoft.com)görüntüleyebilirsiniz. **Resim**/**Metin** > /**Videosunu** **İncele'yi**seçin (kullandığınız içeriğe bağlı olarak). İçerik insan incelemesi için hazır görünmelidir. Bir insan moderatör otomatik olarak atanan etiketleri ve tahmin verilerini gözden geçirir ve nihai bir ılımlılık kararı gönderdikten sonra, iş API tüm bu bilgileri belirlenen geri arama bitiş noktası bitiş noktasına gönderir.

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, REST API'sini kullanarak içerik Moderasyon işlerinin nasıl oluşturulup sorgulanı nızı öğrendiniz. Ardından, işleri [E-ticaret Denetleme](./ecommerce-retail-catalog-moderation.md) Öğreticisi gibi uçdan uca bir Denetleme senaryosuna tümleştirin.