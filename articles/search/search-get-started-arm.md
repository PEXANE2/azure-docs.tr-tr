---
title: Hizmetinizi dağıtmak için bir Azure Resource Manager şablonu kullanma
titleSuffix: Azure Cognitive Search
description: Azure Resource Manager şablonunu kullanarak bir Azure Bilişsel Arama hizmet örneğini hızlıca dağıtabilirsiniz.
manager: nitinme
author: tchristiani
ms.author: terrychr
ms.service: cognitive-search
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 03/20/2020
ms.openlocfilehash: 6da2c324872a86c2c0ce6f55801bc7b0ee8d713e
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80607627"
---
# <a name="quickstart-deploy-cognitive-search-using-a-resource-manager-template"></a>Hızlı başlangıç: Kaynak Yöneticisi şablonu kullanarak Bilişsel Arama dağıtma

Bu makalede, Azure portal Azure Bilişsel Arama kaynağını dağıtmak için Kaynak Yöneticisi şablonu kullanma işleminde size yol gösterilir.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-a-cognitive-search-service"></a>Bilişsel Arama hizmeti oluşturma

### <a name="review-the-template"></a>Şablonu gözden geçirin

Bu hızlı başlangıçta kullanılan şablon [Azure şablonlarından](https://docs.microsoft.com/azure/templates/Microsoft.Search/2015-08-19/searchservices).

:::code language="json"source="~/quickstart-templates/101-azure-search-create/azuredeploy.json" range="1-86" highlight="4-50":::

Bu şablonda tanımlanan Azure kaynağı:

- [Microsoft. Search/searchServices](https://docs.microsoft.com/azure/templates/Microsoft.Search/2015-08-19/searchServices): Azure bilişsel arama hizmeti oluşturma

### <a name="deploy-the-template"></a>Şablonu dağıtma

Aşağıdaki görüntüyü seçerek Azure'da oturum açıp bir şablon açın. Şablon bir Azure Bilişsel Arama kaynağı oluşturur.

[![Azure’a dağıtma](./media/search-get-started-arm/arm-deploybuttona.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2F101-azure-search-create%2Fazuredeploy.json)

Portal, kolayca parametre değerleri sağlamanıza olanak tanıyan bir form görüntüler. Bazı parametreler şablondaki varsayılan değerlerle önceden doldurulur. Aboneliğiniz, kaynak grubunuz, konumunuz ve hizmet adınızı sağlamanız gerekir. Bilişsel hizmetler 'i bir [AI zenginleştirme](https://docs.microsoft.com/azure/search/cognitive-search-concept-intro) ardışık düzeninde kullanmak istiyorsanız, örneğin ikili resim dosyalarını metin için çözümlemek üzere, hem bilişsel arama hem de bilişsel hizmetler sunan bir konum seçin. Her iki hizmetin de AI zenginleştirme iş yükleri için aynı bölgede olması gerekir. Formu tamamladıktan sonra, hüküm ve koşulları kabul etmeniz ve sonra dağıtımınızı tamamlayabilmeniz için satın al düğmesini seçmeniz gerekir.

> [!div class="mx-imgBorder"]
> ![Şablon Azure portal görüntüleme](./media/search-get-started-arm/arm-portalscrnsht.png)

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

Dağıtımınız tamamlandığında, yeni kaynak grubunuza ve portalda yeni arama hizmetinize erişebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Bilişsel Arama hızlı başlangıçler ve öğreticiler bu hızlı başlangıç üzerine inşa. Sonraki hızlı başlangıç ve öğreticilerle çalışmaya devam etmeyi planlıyorsanız, bu kaynağı yerinde bırakmak isteyebilirsiniz. Artık gerekli değilse, Bilişsel Arama hizmetini ve ilgili kaynakları silen kaynak grubunu silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Azure Resource Manager şablonu kullanarak bir Bilişsel Arama hizmeti oluşturdunuz ve dağıtımı doğruladı. Bilişsel Arama ve Azure Resource Manager hakkında daha fazla bilgi edinmek için aşağıdaki makalelere devam edin.

 - [Azure bilişsel arama genel bakışını](https://docs.microsoft.com/azure/search/search-what-is-azure-search) okuyun
 - Arama hizmetiniz için [bir dizin oluşturma](https://docs.microsoft.com/azure/search/search-get-started-portal)
 - Portal Sihirbazı 'nı kullanarak [bir arama uygulaması oluşturma](https://docs.microsoft.com/azure/search/search-create-app-portal)
 - Verilerden bilgi ayıklamak için [bir beceri oluşturun](https://docs.microsoft.com/azure/search/cognitive-search-quickstart-blob)


