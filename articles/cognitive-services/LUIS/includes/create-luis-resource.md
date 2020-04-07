---
title: LUIS kaynağı oluşturma
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/06/2020
ms.author: dapine
ms.openlocfilehash: 88289686e5b091ef3ec309ee9b54ee0f895c8c22
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80754343"
---
<a name="create-luis-resources"></a>

## <a name="create-luis-resources-in-azure-portal"></a>Azure portalında LUIS kaynakları oluşturma

1. Azure portalında LUIS kaynakları oluşturmaya başlamak için [bu bağlantıyı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne) kullanın.

1. Gerekli tüm ayarları girin:

    |Adı|Amaç|
    |--|--|
    |Abonelik adı| kaynak için faturalandırılacak abonelik.|
    |Kaynak grubu| Seçtiğiniz veya oluşturduğunuz özel bir kaynak grubu adı. Kaynak grupları, Azure kaynaklarını erişim ve yönetim için gruplandırmanıza olanak tanır.|
    |Adı| Yazma ve tahmin bitiş noktası sorgularınız için özel alt etki alanınız olarak kullanılan seçtiğiniz özel bir ad.|
    |Yazma konumu|Modelinizle ilişkili bölge.|
    |Fiyatlandırma katmanı yazma|Fiyatlandırma katmanı saniye ve ay başına maksimum hareketi belirler.|
    |Çalışma zamanı konumu|Yayımlanmış tahmin bitiş noktası çalışma saatinizle ilişkili bölge.|
    |Runtime fiyatlandırma katmanı|Fiyatlandırma katmanı saniye ve ay başına maksimum hareketi belirler.|

    > [!div class="mx-imgBorder"]
    > [![Dil anlama kaynağını oluşturma](../media/luis-how-to-azure-subscription/create-resource-in-azure-small.png)](../media/luis-how-to-azure-subscription/create-resource-in-azure-small.png#lightbox)

1. **Gözden Geçir + 'yı** tıklatın ve kaynağın oluşturulmasını bekleyin.
1. Her iki kaynak oluşturulduktan sonra, hala Azure portalında, yeni yazma kaynağını seçin, ardından yazma **bitiş noktası URL'sini** ve programlamak için **anahtar** almaya **başlar.**

> [!TIP]
> Kaynakları kullanmak için, LUIS portalında kaynakları [atayın.](../luis-how-to-azure-subscription.md#assign-an-authoring-resource-in-the-luis-portal-for-all-apps)