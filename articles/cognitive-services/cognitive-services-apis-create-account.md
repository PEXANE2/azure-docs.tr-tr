---
title: Azure portal bilişsel hizmetler kaynağı oluşturma
titleSuffix: Azure Cognitive Services
description: Azure portal bir kaynağı oluşturup abone olarak Azure bilişsel hizmetler 'i kullanmaya başlayın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 07/16/2019
ms.author: aahi
ms.openlocfilehash: ae25ffdf3f819c30fdfb1d3110042579f0b197b9
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70381737"
---
# <a name="create-a-cognitive-services-resource-using-the-azure-portal"></a>Azure portal kullanarak bilişsel hizmetler kaynağı oluşturma

Azure bilişsel hizmetler 'i kullanmaya başlamak için bu hızlı başlangıcı kullanın. Azure portal bilişsel hizmet kaynağı oluşturduktan sonra, uygulamalarınızın kimliğini doğrulamak için bir uç nokta ve anahtar alırsınız.


[!INCLUDE [cognitive-services-subscription-types](../../includes/cognitive-services-subscription-types.md)]

## <a name="prerequisites"></a>Önkoşullar

* Geçerli bir Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/).

## <a name="create-a-new-azure-cognitive-services-resource"></a>Yeni bir Azure bilişsel hizmetler kaynağı oluşturma

1. Bir kaynak oluşturun.

    #### <a name="multi-service-resourcetabmultiservice"></a>[Çoklu hizmet kaynağı](#tab/multiservice)

    Çoklu hizmet kaynağı, portalda bilişsel **Hizmetler** olarak adlandırılır. Bilişsel [Hizmetler kaynağı oluşturun](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAllInOne).
    
    Şu anda, **Bu hizmetler çoklu** hizmet anahtarları kullanılarak çağrılamaz: Soru-Cevap Oluşturma, konuşma Hizmetleri, Özel Görüntü İşleme ve anomali algılayıcısı.
    

    #### <a name="single-service-resourcetabsingleservice"></a>[Tek hizmet kaynağı](#tab/singleservice)

    Kullanılabilir bilişsel hizmetler için bir kaynak oluşturmak üzere aşağıdaki bağlantıları kullanın:

    | Görsel                      | Konuşma                  | Dil                          | Karar verme             | Ara                 |
    |-----------------------------|-------------------------|-----------------------------------|----------------------|------------------------|
    | [Görüntü işleme](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision)         | [Konuşma Hizmetleri](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices)     | [Modern okuyucu](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesImmersiveReader)              | [Anomali algılayıcısı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector) | [Bing Arama API v7](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesBingSearch-v7) |
    | [Özel Vision hizmeti](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesCustomVision) | [Konuşmacı Tanıma](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeakerRecognition) | [Language Understanding (LUIS)](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUIS) | [Content Moderator](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator) | [Bing Özel Arama](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesBingCustomSearch) |
    | [Yüz tanıma](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFace)                    |                         | [Soru-Cevap Oluşturma](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker)                     | [Kişiselleştirici](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer)     | [Bing Varlık Arama](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesBingEntitySearch) |
    | [Mürekkep tanıyıcı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesInkRecognizer)        |                         | [Metin Analizi](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics)                |                      | [Bing Yazım Denetimi](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesBingSpellCheck-v7)   |
    |           |                         | [Translator Metin Çevirisi](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextTranslation)               |                      | [Bing Otomatik Öneri](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesBingAutosuggest-v7)                       |
    ***

3. **Oluştur** sayfasında, aşağıdaki bilgileri sağlayın:

    #### <a name="multi-service-resourcetabmultiservice"></a>[Çoklu hizmet kaynağı](#tab/multiservice)

    |    |    |
    |--|--|
    | **Name** | Bilişsel hizmetler kaynağınız için açıklayıcı bir ad. Örneğin, *Mybiliveservicesresource*. |
    | **Abonelik** | Kullanılabilir Azure aboneliklerinizden birini seçin. |
    | **Location** | Bilişsel hizmet örneğinizin konumu. Farklı konumlar gecikme gösterebilir, ancak kaynağınızın çalışma zamanı kullanılabilirliğini etkilemez. Azure bilişsel hizmetler 'i çağırırken ihtiyacınız olabileceği için Azure konumunuzu unutmayın. |
    | **Fiyatlandırma katmanı** | Bilişsel hizmetler hesabınızın maliyeti, seçtiğiniz seçeneklere ve kullanımınıza bağlıdır. Daha fazla bilgi için bkz. API [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/).
    | **Kaynak grubu** | Bilişsel hizmetler kaynağını içerecek Azure Kaynak grubu. Yeni bir grup oluşturabilir veya önceden var olan bir gruba ekleyebilirsiniz. |

    ![Kaynak oluşturma ekranı](media/cognitive-services-apis-create-account/resource_create_screen-multi.png)

    **Oluştur**'a tıklayın.

    #### <a name="single-service-resourcetabsingleservice"></a>[Tek hizmet kaynağı](#tab/singleservice)

    |    |    |
    |--|--|
    | **Name** | Bilişsel hizmetler kaynağınız için açıklayıcı bir ad. Örneğin, *TextAnalyticsResource*. |
    | **Abonelik** | Kullanılabilir Azure aboneliklerinizden birini seçin. |
    | **Location** | Bilişsel hizmet örneğinizin konumu. Farklı konumlar gecikme gösterebilir, ancak kaynağınızın çalışma zamanı kullanılabilirliğini etkilemez. Azure bilişsel hizmetler 'i çağırırken ihtiyacınız olabileceği için Azure konumunuzu unutmayın. |
    | **Fiyatlandırma katmanı** | Bilişsel hizmetler hesabınızın maliyeti, seçtiğiniz seçeneklere ve kullanımınıza bağlıdır. Daha fazla bilgi için bkz. API [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/).
    | **Kaynak grubu** | Bilişsel hizmetler kaynağını içerecek Azure Kaynak grubu. Yeni bir grup oluşturabilir veya önceden var olan bir gruba ekleyebilirsiniz. |

    ![Kaynak oluşturma ekranı](media/cognitive-services-apis-create-account/resource_create_screen.png)

    **Oluştur**'a tıklayın.

    ***

## <a name="get-the-keys-for-your-resource"></a>Kaynağınızın anahtarlarını alın

Kaynağınız başarıyla dağıtıldıktan sonra, **sonraki adımlar**altında **Kaynağa Git ' e** tıklayın.

![Bilişsel hizmetler için arama](media/cognitive-services-apis-create-account/resource-next-steps.png)

Açılan hızlı başlangıç bölmesinden, anahtarınız ve uç noktanıza erişebilirsiniz.

![Anahtar ve uç nokta al](media/cognitive-services-apis-create-account/get-cog-serv-keys.png)

[!INCLUDE [cognitive-services-environment-variables](../../includes/cognitive-services-environment-variables.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, grupta yer alan diğer tüm kaynakları da siler.

1. Azure portalında sol taraftaki menüyü genişleterek hizmet menüsünü açın ve **Kaynak Grupları**'nı seçerek kaynak gruplarınızın listesini görüntüleyin.
2. Silinecek kaynağı içeren kaynak grubunu bulun
3. Kaynak grubu listesine sağ tıklayın. **Kaynak grubunu sil**'i seçip onaylayın.

## <a name="see-also"></a>Ayrıca bkz.

* [Azure bilişsel hizmetler 'e yönelik isteklerin kimliğini doğrulama](authentication.md)
* [Azure bilişsel hizmetler nedir?](Welcome.md)
* [Doğal dil desteği](language-support.md)
* [Docker kapsayıcı desteği](cognitive-services-container-support.md)
