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
ms.openlocfilehash: c7db2b4d49e3b9297c32d2e11ffe7c7702c17544
ms.sourcegitcommit: 32242bf7144c98a7d357712e75b1aefcf93a40cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70274626"
---
# <a name="create-a-cognitive-services-resource-using-the-azure-portal"></a>Azure portal kullanarak bilişsel hizmetler kaynağı oluşturma

Azure portal kullanarak bir Azure bilişsel hizmetler kaynağı oluşturmak için bu hızlı başlangıcı kullanın. Bilişsel hizmetler kaynağını başarıyla oluşturduktan sonra, uygulamalarınızın kimliğini doğrulamak için kullanabileceğiniz bir uç nokta ve anahtar elde edersiniz.

## <a name="prerequisites"></a>Önkoşullar

* Geçerli bir Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/).

[!INCLUDE [cognitive-services-subscription-types](../../includes/cognitive-services-subscription-types.md)]

## <a name="create-a-new-azure-cognitive-services-resource"></a>Yeni bir Azure bilişsel hizmetler kaynağı oluşturma

Bilişsel hizmetler kaynağı oluşturmadan önce, kaynağı içermesi için bir Azure Kaynak grubunuz olmalıdır. Yeni bir kaynak oluşturduğunuzda, yeni bir kaynak grubu oluşturma veya var olanı kullanma seçeneğiniz vardır. Bu makalede, yeni bir kaynak grubu oluşturma gösterilmektedir.

1. [Azure Portal](https://portal.azure.com)oturum açın ve **+ kaynak oluştur ' a**tıklayın.

    ![Bilişsel Hizmetler API'si seçin](media/cognitive-services-apis-create-account/azurePortalScreenMulti.png)

2. Daha önce açıklandığı gibi, birden çok hizmet kaynağı veya tek hizmet kaynağı kullanarak bilişsel hizmetler kaynağını iki şekilde oluşturabilirsiniz.

    #### <a name="multi-service-resourcetabmultiservice"></a>[Çoklu hizmet kaynağı](#tab/multiservice)

    Çoklu hizmet kaynağı oluşturmak için arama çubuğuna bilişsel **Hizmetler** girin.

    ![Bilişsel hizmetler için arama](media/cognitive-services-apis-create-account/azureCogServSearchMulti.png)

    Bilişsel Hizmetler sayfasında **Oluştur**' u seçin.

    ![Bilişsel Hizmetler hesabı oluşturma](media/cognitive-services-apis-create-account/azurecogservsearchmulti-2.png)

    #### <a name="single-service-resourcetabsingleservice"></a>[Tek hizmet kaynağı](#tab/singleservice)

    Tüm kullanılabilir bilişsel hizmetler 'i görmek için **Azure Marketi**altında **AI + Machine Learning**seçin. İlgilendiğiniz hizmeti görmüyorsanız, **Tümünü görüntüle** ' ye tıklayın ve bilişsel **Hizmetler**' e gidin. Bilişsel hizmetler kataloğunun tamamını görüntülemek için **daha fazla gör** ' e tıklayın.

    İlgilendiğiniz hizmetten sonra **Oluştur**' a tıklayın.
    
    ![Bilişsel Hizmetler API'si seçin](media/cognitive-services-apis-create-account/azureMarketplace.png)

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

Kaynak başarıyla oluşturulduktan sonra, ekranın sağ üst kısmında bir açılır bildirim alırsınız. Bildirimden, oluşturduğunuz bilişsel hizmet kaynağını görmek için **Kaynağa Git** ' e tıklayın. 

![Bilişsel hizmet kaynağına git](media/cognitive-services-apis-create-account/cog-serv-go-to-resource.png)

Açılan hızlı başlangıç bölmesinden uç noktanıza ve anahtarınıza erişebilirsiniz.

![Anahtar ve uç nokta al](media/cognitive-services-apis-create-account/get-cog-serv-keys.png)

[!INCLUDE [cognitive-services-environment-variables](../../includes/cognitive-services-environment-variables.md)]

## <a name="pricing-tiers-and-billing"></a>Fiyatlandırma katmanları ve faturalandırma

Fiyatlandırma katmanları (ve faturalandırılan miktar), kimlik doğrulama bilgilerinizi kullanarak göndereceğiniz işlem sayısına bağlıdır. Her fiyatlandırma katmanı şunları belirtir:
* saniye başına izin verilen en fazla işlem sayısı (TPS).
* fiyatlandırma katmanında etkinleştirilen hizmet özellikleri.
* Önceden tanımlı işlem sayısının maliyeti. Bu tutarın üzerine gitmek, hizmetinize ilişkin [fiyatlandırma ayrıntılarında](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) belirtilen ek bir ücretden oluşmasına neden olur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, grupta yer alan diğer tüm kaynakları da siler.

Azure portalını kullanarak kaynak grubunu kaldırmak için:

1. Azure portalında sol taraftaki menüyü genişleterek hizmet menüsünü açın ve **Kaynak Grupları**'nı seçerek kaynak gruplarınızın listesini görüntüleyin.
2. Silinecek kaynak grubunu bulun ve listenin sağ tarafındaki daha fazla düğmesine (...) sağ tıklayın.
3. **Kaynak grubunu sil**'i seçip onaylayın.

## <a name="see-also"></a>Ayrıca bkz.

* [Azure bilişsel hizmetler 'e yönelik isteklerin kimliğini doğrulama](authentication.md)
* [Azure bilişsel hizmetler nedir?](Welcome.md)
* [Doğal dil desteği](language-support.md)
* [Docker kapsayıcı desteği](cognitive-services-container-support.md)
