---
title: REST API konsolu ile ılımlılık iş akışlarını tanımlayın - İçerik Moderatör
titleSuffix: Azure Cognitive Services
description: Azure İçerik Moderatör İnceleme API'lerini, içerik ilkelerinize göre özel iş akışları ve eşikleri tanımlamak için kullanabilirsiniz.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: pafarley
ms.openlocfilehash: 3e58be4b94457d95d28cf6528b9151e4be1802cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "72754176"
---
# <a name="define-and-use-moderation-workflows-rest"></a>Moderasyon iş akışlarını tanımlayın ve kullanın (REST)

İş akışları, içeriği daha verimli işlemek için kullanabileceğiniz bulut tabanlı özelleştirilmiş filtrelerdir. İş akışları, içeriği farklı şekillerde filtrelemek ve ardından uygun eylemi yapmak için çeşitli hizmetlere bağlanabilir. Bu kılavuz, iş akışları oluşturmak ve kullanmak için API konsolu aracılığıyla iş akışı REST API'lerini nasıl kullanacağınızı gösterir. API'lerin yapısını anladıktan sonra, bu çağrıları REST uyumlu herhangi bir platforma kolayca iletebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- İçerik Moderatör [İnceleme araç](https://contentmoderator.cognitive.microsoft.com/) sitesinde oturum açın veya bir hesap oluşturun.

## <a name="create-a-workflow"></a>İş akışı oluşturma

İş akışı oluşturmak veya güncelleştirmek için **[İş Akışı - API](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59)** başvuru sayfası oluşturun veya güncelleyin ve anahtar bölgeniz için düğmeyi seçin (bunu Gözden Geçirme [aracının](https://contentmoderator.cognitive.microsoft.com/) **Kimlik Bilgileri** sayfasında Ki Satınız URL'sinde bulabilirsiniz). Bu, REST API çağrılarını kolayca oluşturabileceğiniz ve çalıştırabileceğiniz API konsolu'nu başlatır.

![İş Akışı - Sayfa bölge seçimi oluştur veya güncelleştir](images/test-drive-region.png)

### <a name="enter-rest-call-parameters"></a>REST çağrı parametrelerini girin

**Takım**, **iş akışı adı**ve **Ocp-Apim-Abonelik-Anahtar**için değerleri girin:

- **takım**: [Gözden Geçirme aracı](https://contentmoderator.cognitive.microsoft.com/) hesabınızı ayarlarken oluşturduğunuz takım kimliği (İnceleme aracınızın Kimlik Bilgileri ekranında **Kimlik** alanında bulunur).
- **iş akışı adı**: Eklenecek yeni bir iş akışının adı (veya varolan bir iş akışını güncelleştirmek istiyorsanız varolan bir ad).
- **Ocp-Apim-Abonelik-Anahtar**: İçerik Moderatör anahtarınız. Bunu [Gözden Geçir aracının](https://contentmoderator.cognitive.microsoft.com) **Ayarlar** sekmesinde bulabilirsiniz.

![İş Akışı - Konsol sorgu parametreleri ve üstbilgileri oluşturma veya güncelleme](images/workflow-console-parameters.PNG)

### <a name="enter-a-workflow-definition"></a>İş akışı tanımı girin

1. **Açıklama** ve **Tür** (ya da) `Image` `Text`ayrıntıları ile JSON isteği girmek için **İstek gövde** kutusunu değiştirin.
2. **İfade**için varsayılan iş akışı JSON ifadesini kopyalayın. Son JSON dizeniz şu na benzemelidir:

```json
{
  "Description":"<A description for the Workflow>",
  "Type":"Text",
  "Expression":{
    "Type":"Logic",
    "If":{
      "ConnectorName":"moderator",
      "OutputName":"isAdult",
      "Operator":"eq",
      "Value":"true",
      "Type":"Condition"
    },
    "Then":{
      "Perform":[
        {
          "Name":"createreview",
          "CallbackEndpoint":null,
          "Tags":[

          ]
        }
      ],
      "Type":"Actions"
    }
  }
}
```

> [!NOTE]
> Bu API'yi kullanarak iş akışlarınız için basit, karmaşık ve hatta iç içe geçen ifadeleri tanımlayabilirsiniz. [İş Akışı - Oluştur veya Güncelleştir](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) belgelerinde daha karmaşık mantık örnekleri vardır.

### <a name="submit-your-request"></a>İsteğinizi gönderin
  
**Gönder**’i seçin. İşlem başarılı olursa, **Yanıt** durumu `200 OK`ve **Yanıt içerik** `true`kutusu görüntülenir.

### <a name="examine-the-new-workflow"></a>Yeni iş akışını inceleyin

Gözden [Geçir aracında,](https://contentmoderator.cognitive.microsoft.com/) **Ayarlar** > **İş Akışları'nı**seçin. Yeni iş akışınız listede görünmelidir.

![İş akışlarının araç listesini gözden geçirme](images/workflow-console-new-workflow.PNG)

İş akışınız için **Edit** seçeneğini seçin ve **Tasarımcı** sekmesine gidin. Burada, JSON mantığının sezgisel bir temsilini görebilirsiniz.

![Seçili iş akışı için tasarımcı sekmesi](images/workflow-console-new-workflow-designer.PNG)

## <a name="get-workflow-details"></a>İş akışı ayrıntılarını alın

Varolan bir iş akışı yla ilgili ayrıntıları almak için **[İş Akışı -](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b44b3f9b0711b43c4c58)** API başvuru sayfasına gidin ve bölgenizin (anahtarınızın yönetildiği bölge) düğmesini seçin.

![İş akışı - Bölge seçimini alın](images/test-drive-region.png)

Yukarıdaki bölümde olduğu gibi REST arama parametrelerini girin. Bu **kez, iş akışı adının** varolan bir iş akışının adı olduğundan emin olun.

![Sorgu parametreleri ve üstbilgi alma](images/workflow-get-default.PNG)

**Gönder**’i seçin. İşlem başarılı olursa, **Yanıt** durumu `200 OK`ve **Yanıt içeriği** kutusu iş akışını Aşağıdaki gibi JSON biçiminde görüntüler:

```json
{
  "Name":"default",
  "Description":"Default",
  "Type":"Image",
  "Expression":{
    "If":{
      "ConnectorName":"moderator",
      "OutputName":"isadult",
      "Operator":"eq",
      "Value":"true",
      "AlternateInput":null,
      "Type":"Condition"
    },
    "Then":{
      "Perform":[
        {
          "Name":"createreview",
          "Subteam":null,
          "CallbackEndpoint":null,
          "Tags":[

          ]
        }
      ],
      "Type":"Actions"
    },
    "Else":null,
    "Type":"Logic"
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [İçerik Denetleme işleri](try-review-api-job.md)ile iş akışlarını nasıl kullanacağınızı öğrenin.