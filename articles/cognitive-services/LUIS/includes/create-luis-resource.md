---
title: LUSıS kaynağı oluşturma
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/07/2020
ms.author: aahi
ms.openlocfilehash: ebfe7e80577b163218a7bc501fa4e3e9b206fd62
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80879231"
---
<a name="create-luis-resources"></a>

## <a name="create-luis-resources-in-azure-portal"></a>Azure portal için LUSıS kaynakları oluşturma

1. Azure portal LUSıS kaynaklarını oluşturmaya başlamak için [Bu bağlantıyı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne) kullanın.

1. Tüm gerekli ayarları girin:

    |Ad|Amaç|
    |--|--|
    |Abonelik adı| kaynak için faturalandırılacak abonelik.|
    |Kaynak grubu| Seçtiğiniz veya oluşturduğunuz özel bir kaynak grubu adı. Kaynak grupları, erişim ve yönetim için Azure kaynaklarını gruplandıreklemenize olanak tanır.|
    |Ad| Yazma ve tahmin uç noktası sorgularınız için özel alt etki alanı olarak kullanılan özel bir ad.|
    |Yazma konumu|Modelinizle ilişkili bölge.|
    |Fiyatlandırma Katmanı yazma|Fiyatlandırma Katmanı, saniye başına en fazla işlemi ve ayı belirler.|
    |Çalışma zamanı konumu|Yayımlanmış tahmin uç noktası çalışma zamanı ile ilişkili bölge.|
    |Çalışma zamanı Fiyatlandırma Katmanı|Fiyatlandırma Katmanı, saniye başına en fazla işlemi ve ayı belirler.|

    > [!div class="mx-imgBorder"]
    > [![Dil anlama kaynağı oluşturma](../media/luis-how-to-azure-subscription/create-resource-in-azure-small.png)](../media/luis-how-to-azure-subscription/create-resource-in-azure-small.png#lightbox)

1. **Gözden geçir + oluştur** ' a tıklayın ve kaynağın oluşturulmasını bekleyin.
1. Her iki kaynak de oluşturulduktan sonra, Azure portal, yeni yazma kaynağını seçin, sonra **hızlı başlangıç** **noktası URL 'sini** ve programlama yoluyla yazma için **anahtarı** alın.

> [!TIP]
> Kaynakları kullanmak için, LUO portalında [kaynakları atayın](../luis-how-to-azure-subscription.md#assign-an-authoring-resource-in-the-luis-portal-for-all-apps).