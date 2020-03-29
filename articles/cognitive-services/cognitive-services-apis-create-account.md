---
title: Azure portalında Bilişsel Hizmetler kaynağı oluşturma
titleSuffix: Azure Cognitive Services
description: Azure portalındaki bir kaynağa abone olarak Azure Bilişsel Hizmetleri'ne başlayın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: aahi
ms.openlocfilehash: dd4444bf42bcc8dda95f8fa37b42a365538efa85
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79219483"
---
# <a name="create-a-cognitive-services-resource-using-the-azure-portal"></a>Azure portalını kullanarak Bilişsel Hizmetler kaynağı oluşturma

Azure Bilişsel Hizmetleri'ni kullanmaya başlamak için bu hızlı başlangıcı kullanın. Azure portalında bir Bilişsel Hizmet kaynağı oluşturduktan sonra, bir bitiş noktası ve uygulamalarınızın kimliğini doğrulamak için bir anahtar alırsınız.


[!INCLUDE [cognitive-services-subscription-types](../../includes/cognitive-services-subscription-types.md)]

## <a name="prerequisites"></a>Ön koşullar

* Geçerli bir Azure aboneliği - [Ücretsiz bir abonelik oluşturun.](https://azure.microsoft.com/free/)

## <a name="create-a-new-azure-cognitive-services-resource"></a>Yeni bir Azure Bilişsel Hizmetler kaynağı oluşturun

1. Kaynak oluşturma.

    #### <a name="multi-service-resource"></a>[Çoklu hizmet kaynağı](#tab/multiservice)
    
    Çoklu hizmet kaynağı portalda **Bilişsel Hizmetler** olarak adlandırılır. [Bilişsel Hizmetler kaynağı oluşturun.](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAllInOne)
    
    Şu anda, çoklu hizmet kaynağı aşağıdaki Bilişsel Hizmetlere erişim sağlar:
    
    |                  |                                                      |                    |                               |                  |
    |------------------|------------------------------------------------------|--------------------|-------------------------------|------------------|
    | Görüntü İşleme  | Content Moderator                                    | Yüz               | Dil Anlama (LUIS) | Metin Analizi   |
    | Translator Metin Çevirisi  | Bing Arama v7 <br>(Web, Görüntü, Haber, Video, Görsel) | Bing Özel Arama | Bing Varlık Arama            | Bing Otomatik Öneri |
    | Bing Yazım Denetimi |                                                      |                    |                               |                  |
    
    #### <a name="single-service-resource"></a>[Tek hizmet kaynağı](#tab/singleservice)

    Kullanılabilir Bilişsel Hizmetler için bir kaynak oluşturmak için aşağıdaki bağlantıları kullanın:

    | Görsel                      | Konuşma                  | Dil                          | Karar             | Search                 |
    |-----------------------------|-------------------------|-----------------------------------|----------------------|------------------------|
    | [Görüntü işleme](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision)         | [Konuşma Hizmetleri](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices)     | [Sürükleyici okuyucu](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesImmersiveReader)              | [Anomali Algılayıcısı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector) | [Bing Arama API V7](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesBingSearch-v7) |
    | [Özel görme hizmeti](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesCustomVision) | [Konuşmacı Tanıma](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeakerRecognition) | [Dil Anlama (LUIS)](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne) | [Content Moderator](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator) | [Bing Özel Arama](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesBingCustomSearch) |
    | [Yüz](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFace)                    |                         | [QnA Üreticisi](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker)                     | [Kişiselleştirme](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer)     | [Bing Varlık Arama](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesBingEntitySearch) |
    | [Mürekkep Tanıma](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesInkRecognizer)        |                         | [Metin Analizi](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics)                |                      | [Bing Yazım Denetimi](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesBingSpellCheck-v7)   |
    |           |                         | [Çevirmen Metni](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextTranslation)               |                      | [Bing Otomatik Öneri](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesBingAutosuggest-v7)                       |
    ***

3. **Oluştur** sayfasında aşağıdaki bilgileri sağlayın:

    #### <a name="multi-service-resource"></a>[Çoklu hizmet kaynağı](#tab/multiservice)

    |    |    |
    |--|--|
    | **Adı** | Bilişsel hizmetler kaynağınız için açıklayıcı bir ad. Örneğin, *MyCognitiveServicesResource*. |
    | **Abonelik** | Kullanılabilir Azure aboneliklerinizden birini seçin. |
    | **Konum** | Bilişsel hizmet örneğinizin konumu. Farklı konumlar gecikme süresine neden olabilir, ancak kaynağınızın çalışma zamanı kullanılabilirliği üzerinde hiçbir etkisi yoktur. |
    | **Fiyatlandırma katmanı** | Bilişsel Hizmetler hesabınızın maliyeti seçtiğiniz seçeneklere ve kullanımınıza bağlıdır. Daha fazla bilgi için API [fiyatlandırma ayrıntılarına](https://azure.microsoft.com/pricing/details/cognitive-services/)bakın.
    | **Kaynak grubu** | Bilişsel Hizmetler kaynağınızı içerecek Azure kaynak grubu. Yeni bir grup oluşturabilir veya önceden varolan bir gruba ekleyebilirsiniz. |

    ![Kaynak oluşturma ekranı](media/cognitive-services-apis-create-account/resource_create_screen-multi.png)

    **Oluştur'u**tıklatın.

    #### <a name="single-service-resource"></a>[Tek hizmet kaynağı](#tab/singleservice)

    |    |    |
    |--|--|
    | **Adı** | Bilişsel hizmetler kaynağınız için açıklayıcı bir ad. Örneğin, *TextAnalyticsResource*. |
    | **Abonelik** | Kullanılabilir Azure aboneliklerinizden birini seçin. |
    | **Konum** | Bilişsel hizmet örneğinizin konumu. Farklı konumlar gecikme süresine neden olabilir, ancak kaynağınızın çalışma zamanı kullanılabilirliği üzerinde hiçbir etkisi yoktur. |
    | **Fiyatlandırma katmanı** | Bilişsel Hizmetler hesabınızın maliyeti seçtiğiniz seçeneklere ve kullanımınıza bağlıdır. Daha fazla bilgi için API [fiyatlandırma ayrıntılarına](https://azure.microsoft.com/pricing/details/cognitive-services/)bakın.
    | **Kaynak grubu** | Bilişsel Hizmetler kaynağınızı içerecek Azure kaynak grubu. Yeni bir grup oluşturabilir veya önceden varolan bir gruba ekleyebilirsiniz. |

    ![Kaynak oluşturma ekranı](media/cognitive-services-apis-create-account/resource_create_screen.png)

    **Oluştur'u**tıklatın.

    ***


## <a name="get-the-keys-for-your-resource"></a>Kaynağınızın anahtarlarını alın

1. Kaynağınız başarıyla dağıtıldıktan sonra, **Sonraki Adımlar**altında **kaynağa git'i** tıklatın.

    ![Bilişsel Hizmetler Ara](media/cognitive-services-apis-create-account/resource-next-steps.png)

2. Açılan hızlı başlangıç bölmesinden, anahtarınıza ve bitiş noktanıza erişebilirsiniz.

    ![Anahtar ve bitiş noktası alın](media/cognitive-services-apis-create-account/get-cog-serv-keys.png)

[!INCLUDE [cognitive-services-environment-variables](../../includes/cognitive-services-environment-variables.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel Hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynak veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, grupta bulunan diğer kaynakları da siler.

1. Azure portalında sol taraftaki menüyü genişleterek hizmet menüsünü açın ve **Kaynak Grupları**'nı seçerek kaynak gruplarınızın listesini görüntüleyin.
2. Silinecek kaynağı içeren kaynak grubunu bulma
3. Kaynak grubu listesine sağ tıklayın. **Kaynak grubunu sil**'i seçip onaylayın.

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Bilişsel Hizmetleri'ndeki istekleri doğrulama](authentication.md)
* [Azure Bilişsel Hizmetler nedir?](Welcome.md)
* [Doğal dil desteği](language-support.md)
* [Docker konteyner desteği](cognitive-services-container-support.md)
