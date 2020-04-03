---
title: Hizmetinizi dağıtmak için Azure Kaynak Yöneticisi şablonu kullanın
titleSuffix: Azure Cognitive Search
description: Azure kaynak yöneticisi şablonunu kullanarak bir Azure Bilişsel Arama hizmeti örneğini hızla dağıtabilirsiniz.
manager: nitinme
author: tchristiani
ms.author: terrychr
ms.service: cognitive-search
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 03/20/2020
ms.openlocfilehash: 6da2c324872a86c2c0ce6f55801bc7b0ee8d713e
ms.sourcegitcommit: 515482c6348d5bef78bb5def9b71c01bb469ed80
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80607627"
---
# <a name="quickstart-deploy-cognitive-search-using-a-resource-manager-template"></a>Hızlı Başlangıç: Kaynak Yöneticisi şablonunu kullanarak Bilişsel Arama'yı dağıtma

Bu makale, Azure portalında bir Azure Bilişsel Arama kaynağı dağıtmak için Kaynak Yöneticisi şablonu kullanma işleminde size yol gösterir.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="create-a-cognitive-search-service"></a>Bilişsel Arama hizmeti oluşturma

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlatmada kullanılan şablon [Azure şablonlarındandır.](https://docs.microsoft.com/azure/templates/Microsoft.Search/2015-08-19/searchservices)

:::code language="json"source="~/quickstart-templates/101-azure-search-create/azuredeploy.json" range="1-86" highlight="4-50":::

Bu şablonda tanımlanan Azure kaynağı:

- [Microsoft.Search/searchServices](https://docs.microsoft.com/azure/templates/Microsoft.Search/2015-08-19/searchServices): Bir Azure Bilişsel Arama hizmeti oluşturma

### <a name="deploy-the-template"></a>Şablonu dağıtma

Aşağıdaki görüntüyü seçerek Azure'da oturum açıp bir şablon açın. Şablon bir Azure Bilişsel Arama kaynağı oluşturur.

[![Azure’a dağıtma](./media/search-get-started-arm/arm-deploybuttona.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2F101-azure-search-create%2Fazuredeploy.json)

Portal, parametre değerlerini kolayca sağlamanızı sağlayan bir form görüntüler. Bazı parametreler şablondaki varsayılan değerlerle önceden doldurulmuştür. Aboneliğinizi, kaynak grubunuzu, konumunuzu ve hizmet adınızı sağlamanız gerekir. Bilişsel Hizmetleri bir [AI zenginleştirme](https://docs.microsoft.com/azure/search/cognitive-search-concept-intro) ardışık alanında kullanmak istiyorsanız, örneğin metin için ikili görüntü dosyalarını çözümlemek için, hem Bilişsel Arama hem de Bilişsel Hizmetler sunan bir konum seçin. Her iki hizmetin de AI zenginleştirme iş yükleri için aynı bölgede olması gerekir. Formu tamamladıktan sonra, şart ve koşulları kabul etmeniz ve ardından dağıtımınızı tamamlamak için satın alma düğmesini seçmeniz gerekir.

> [!div class="mx-imgBorder"]
> ![Şablonun Azure portal ekranı](./media/search-get-started-arm/arm-portalscrnsht.png)

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

Dağıtımınız tamamlandığında yeni kaynak grubunuza ve yeni arama hizmetinize portaldan erişebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Bilişsel Arama hızlı başlar ve öğreticiler bu quickstart üzerine inşa. Sonraki hızlı başlangıçlar ve öğreticilerle çalışmaya devam etmeyi planlıyorsanız, bu kaynağı yerinde bırakmak isteyebilirsiniz. Artık gerekmediğinde, Bilişsel Arama hizmetini ve ilgili kaynakları silen kaynak grubunu silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure Kaynak Yöneticisi şablonunu kullanarak bir Bilişsel Arama hizmeti oluşturdunuz ve dağıtımı doğruladınız. Bilişsel Arama ve Azure Kaynak Yöneticisi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere devam edin.

 - Azure [Bilişsel Arama'ya genel bakış](https://docs.microsoft.com/azure/search/search-what-is-azure-search) okuma
 - Arama hizmetiniz için [bir dizin oluşturma](https://docs.microsoft.com/azure/search/search-get-started-portal)
 - Portal sihirbazını kullanarak [arama uygulaması oluşturma](https://docs.microsoft.com/azure/search/search-create-app-portal)
 - Verilerinizden bilgi almak için [bir beceri oluşturma](https://docs.microsoft.com/azure/search/cognitive-search-quickstart-blob)


