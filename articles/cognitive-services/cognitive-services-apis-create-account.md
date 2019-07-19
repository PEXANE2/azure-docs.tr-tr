---
title: Azure portal bilişsel hizmetler kaynağı oluşturma
titlesuffix: Azure Cognitive Services
description: Azure portal bir kaynağı oluşturup abone olarak Azure bilişsel hizmetler 'i kullanmaya başlayın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 07/16/2019
ms.author: aahi
ms.openlocfilehash: e9616f3014288e4b2580f474c49c646928db5a08
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68334192"
---
# <a name="create-a-cognitive-services-resource-using-the-azure-portal"></a>Azure portal kullanarak bilişsel hizmetler kaynağı oluşturma

Azure portal kullanarak Azure bilişsel hizmetler 'i kullanmaya başlamak için bu hızlı başlangıcı kullanın. Bilişsel hizmetler, Azure aboneliğinizde oluşturduğunuz Azure [kaynakları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal) tarafından temsil edilir. Kaynağı oluşturduktan sonra, uygulamalarınızın kimliğini doğrulamak için oluşturulan anahtarları ve uç noktayı kullanın. 

## <a name="prerequisites"></a>Önkoşullar

* Geçerli bir Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)

[!INCLUDE [cognitive-services-subscription-types](../../includes/cognitive-services-subscription-types.md)]

## <a name="create-a-new-azure-cognitive-services-resource"></a>Yeni bir Azure bilişsel hizmetler kaynağı oluşturma

Bilişsel hizmetler kaynağı oluşturmadan önce, kaynağı içermesi için bir Azure Kaynak grubunuz olmalıdır. Yeni bir kaynak oluşturduğunuzda, yeni bir kaynak grubu oluşturma veya var olanı kullanma seçeneğiniz vardır. Bu makalede, yeni bir kaynak grubu oluşturma gösterilmektedir.

1. [Azure Portal](https://portal.azure.com)oturum açın ve **+ kaynak oluştur ' a**tıklayın.

    ![Bilişsel Hizmetler API'si seçin](media/cognitive-services-apis-create-account/azurePortalScreenMulti.png)

2. Kullanılabilir bilişsel Hizmetleri aşağıdaki yollarla bulabilirsiniz:
    * Arama çubuğunu kullanın ve abone olmak istediğiniz hizmetin adını girin.
        * Çoklu hizmet kaynağı oluşturmak için arama çubuğuna bilişsel **Hizmetler** girin ve bilişsel **Hizmetler** kaynağını seçin.

        ![Bilişsel hizmetler için arama](media/cognitive-services-apis-create-account/azureCogServSearchMulti.png)

    * Tüm kullanılabilir bilişsel hizmetler 'i görmek için **Azure Marketi**altında **AI + Machine Learning**seçin. İlgilendiğiniz hizmeti görmüyorsanız, **Tümünü görüntüle** ' ye tıklayın ve bilişsel **Hizmetler**' e gidin. Tüm Bilişsel Hizmetler API'si kataloğunu görüntülemek için **daha fazla** ' ya tıklayın.
    
        ![Bilişsel Hizmetler API'si seçin](media/cognitive-services-apis-create-account/azureMarketplace.png)

3. **Oluştur** sayfasında, aşağıdaki bilgileri sağlayın:

    > [!IMPORTANT]
    > Azure bilişsel hizmetler 'i çağırırken ihtiyacınız olabileceği için Azure konumunuzu unutmayın.

    |    |    |
    |--|--|
    | **Name** | Bilişsel hizmetler kaynağınız için açıklayıcı bir ad. Örneğin, *Mybiliveservicesaccount*. |
    | **Abonelik** | Kullanılabilir Azure aboneliklerinizden birini seçin. |
    | **Location** | Bilişsel hizmet örneğinizin konumu. Farklı konumlar gecikme gösterebilir, ancak kaynağınızın çalışma zamanı kullanılabilirliğini etkilemez. |
    | **Fiyatlandırma katmanı** | Bilişsel hizmetler hesabınızın maliyeti, seçtiğiniz seçeneklere ve kullanımınıza bağlıdır. Daha fazla bilgi için bkz. API [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/).
    | **Kaynak grubu** | Bilişsel hizmetler kaynağını içerecek [Azure Kaynak grubu](https://docs.microsoft.com/azure/architecture/cloud-adoption/governance/resource-consistency/azure-resource-access#what-is-an-azure-resource-group) . Yeni bir grup oluşturabilir veya önceden var olan bir gruba ekleyebilirsiniz. |

    ![Kaynak oluşturma ekranı](media/cognitive-services-apis-create-account/resource_create_screen.png)


## <a name="get-the-keys-for-your-resource"></a>Kaynağınızın anahtarlarını alın

Kaynağınızı oluşturduktan sonra, bu kaynağı sabitledikten sonra Azure panosundan erişebilirsiniz. Aksi takdirde, **kaynak gruplarında**bulabilirsiniz. Kaynağı seçtikten sonra, **kaynak yönetimi**altında **anahtarlar** ' ı seçerek anahtarları alabilirsiniz.

[!INCLUDE [cognitive-services-environment-variables](../../includes/cognitive-services-environment-variables.md)]

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
