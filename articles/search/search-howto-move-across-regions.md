---
title: Hizmet kaynağınızı bölgeler arasında taşıma
titleSuffix: Azure Cognitive Search
description: Bu makalede, Azure Bilişsel Arama kaynaklarınızı Azure bulutundaki bir bölgeden diğerine nasıl taşıyacağınız gösterilmektedir.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: how-to
ms.custom: subject-moving-resources
ms.date: 03/06/2020
ms.openlocfilehash: c31a81d2836e9f8c00dec3c0c2eb3a43800a5322
ms.sourcegitcommit: 05a650752e9346b9836fe3ba275181369bd94cf0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2020
ms.locfileid: "79136270"
---
# <a name="move-your-azure-cognitive-search-service-to-another-azure-region"></a>Azure Bilişsel Arama hizmetinizi başka bir Azure bölgesine taşıyın

Bazen müşteriler, mevcut bir arama hizmetini başka bir bölgeye taşımayı sorgular. Şu anda, bu görevle ilgili yardım almak için yerleşik mekanizmalar veya alet yok. Bu makalede aşağıda özetlenen el ile bir işlem kalır.

> [!NOTE]
> Azure portal, tüm hizmetlerin bir **şablonu dışarı aktar** komutu vardır. Azure Bilişsel Arama söz konusu olduğunda, bu komut bir hizmetin temel tanımını (ad, konum, katman, çoğaltma ve bölüm sayısı) üretir, ancak hizmetinizin içeriğini tanımaz, ya da anahtar, rol ya da Günlükler üzerinde devam etmez. Komut var olsa da, bir arama hizmeti taşımak için kullanılması önerilmez.

## <a name="steps-for-moving-a-service"></a>Hizmeti taşıma adımları

Bir arama hizmetini farklı bir bölgeye taşımanız gerekiyorsa, yaklaşımınızın aşağıdaki adımlara benzer olması gerekir:

1. Bir hizmeti yeniden konumlandırın tam etkisini anlamak için ilgili hizmetleri belirler. Günlüğe kaydetme, bilgi deposu veya dış veri kaynağı olarak Azure Storage kullanıyor olabilirsiniz. AI zenginleştirme için bilişsel hizmetler kullanıyor olabilirsiniz. Diğer bölgelerdeki hizmetlere erişmek yaygın olarak kullanılır, ancak ek bant genişliği ücretleri ile gelir. Bilişsel hizmetler ve Azure Bilişsel Arama, AI zenginleştirme kullanıyorsanız aynı bölgede olması gerekir.

1. Hizmet üzerindeki nesnelerin tam listesi için mevcut hizmetinizi envanterini çıkarın. Günlüğe kaydetmeyi etkinleştirdiyseniz, bir geçmiş kaydı için ihtiyaç duyduğunuz tüm raporları oluşturun ve arşivleyin.

1. Azure Bilişsel Arama kullanılabilirliğini ve aynı bölgede oluşturmak isteyebileceğiniz ilgili hizmetlerin yanı sıra, yeni bölgede fiyatlandırma ve kullanılabilirliği denetleyin. Özellik eşliği olup olmadığını denetleyin. Bazı Önizleme özelliklerinin kullanılabilirliği sınırlıdır.

1. Yeni bölgede bir hizmet oluşturun ve mevcut dizinler, Dizin oluşturucular, veri kaynakları, becerileri, bilgi depoları ve eş anlamlı haritalar aracılığıyla kaynak koddan yeniden yayımlayın. Mevcut adı yeniden kullanabilmeniz için hizmet adları benzersiz olmalıdır.

1. Varsa dizinleri ve bilgi depolarını yeniden yükleyin. JSON verilerini bir dizine göndermek için uygulama kodunu kullanacaksınız veya dış kaynaklardan gelen belgeleri çekmek için dizin oluşturucularının yeniden çalıştırılması gerekir. 

1. Günlüğe kaydetmeyi etkinleştirin ve bunları kullanıyorsanız, güvenlik rollerini yeniden oluşturun.

1. İstemci uygulamalarını ve test paketlerini yeni hizmet adı ve API anahtarlarını kullanacak şekilde güncelleştirin ve tüm uygulamaları test edin.

1. Yeni hizmet tam olarak sınandıktan ve çalışır duruma getirildikten sonra eski hizmeti silin.

## <a name="next-steps"></a>Sonraki adımlar

+ [Bir katman seçin](search-sku-tier.md)
+ [Arama hizmeti oluşturma](search-create-service-portal.md)
+ [Arama belgelerini yükleme](search-what-is-data-import.md)
+ [Günlüğü etkinleştirme](search-monitor-logs.md)


<!-- To move your Azure Cognitive Service account from one region to another, you will create an export template to move your subscription(s). After moving your subscription, you will need to move your data and recreate your service.

In this article, you'll learn how to:

> [!div class="checklist"]
> * Export a template.
> * Modify the template: adding the target region, search and storage account names.
> * Deploy the template to create the new search and storage accounts.
> * Verify your service status in the new region
> * Clean up resources in the source region.

## Prerequisites

- Ensure that the services and features that your account uses are supported in the target region.

- For preview features, ensure that your subscription is whitelisted for the target region. For more information about preview features, see [knowledge stores](https://docs.microsoft.com/azure/search/knowledge-store-concept-intro), [incremental enrichment](https://docs.microsoft.com/azure/search/cognitive-search-incremental-indexing-conceptual), and [private endpoint](https://docs.microsoft.com/azure/search/service-create-private-endpoint).

## Assessment and planning

When you move your search service to the new region, you will need to [move your data to the new storage service](https://docs.microsoft.com/azure/storage/common/storage-account-move?tabs=azure-portal#configure-the-new-storage-account) and then rebuild your indexes, skillsets and knowledge stores. You should record current settings and copy json files to make the rebuilding of your service easier and faster.

## Moving your search service's resources

To start you will export and then modify a Resource Manager template.

### Export a template

1. Sign in to the [Azure portal](https://portal.azure.com).

2. Go to your Resource Group page.

> [!div class="mx-imgBorder"]
> ![Resource Group page example](./media/search-move-resource/export-template-sample.png)

3. Select **All resources**.

3. In the left hand navigation menu select **Export template**.

4. Choose **Download** in the **Export template** page.

5. Locate the .zip file that you downloaded from the portal, and unzip that file to a folder of your choice.

The zip file contains the .json files that comprise the template and scripts to deploy the template.

### Modify the template

You will modify the template by changing the search and storage account names and regions. The names must follow the rules for each service and region naming conventions. 

To obtain region location codes, see [Azure Locations](https://azure.microsoft.com/global-infrastructure/locations/).  The code for a region is the region name with no spaces, **Central US** = **centralus**.

1. In the Azure portal, select **Create a resource**.

2. In **Search the Marketplace**, type **template deployment**, and then press **ENTER**.

3. Select **Template deployment**.

4. Select **Create**.

5. Select **Build your own template in the editor**.

6. Select **Load file**, and then follow the instructions to load the **template.json** file that you downloaded and unzipped in the previous section.

7. In the **template.json** file, name the target search and storage accounts by setting the default value of the search and storage account names. 

8. Edit the **location** property in the **template.json** file to the target region for both your search and storage services. This example sets the target region to `centralus`.

```json
},
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Search/searchServices",
            "apiVersion": "2020-03-13",
            "name": "[parameters('searchServices_target_region_search_name')]",
            "location": "centralus",
            "sku": {
                "name": "standard"
            },
            "properties": {
                "replicaCount": 1,
                "partitionCount": 1,
                "hostingMode": "Default"
            }
        },
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-06-01",
            "name": "[parameters('storageAccounts_tagetstorageregion_name')]",
            "location": "centralus",
            "sku": {
                "name": "Standard_RAGRS",
                "tier": "Standard"
            },
```

### Deploy the template

1. Save the **template.json** file.

2. Enter or select the property values:

- **Subscription**: Select an Azure subscription.

- **Resource group**: Select **Create new** and give the resource group a name.

- **Location**: Select an Azure location.

3. Click the **I agree to the terms and conditions stated above** checkbox, and then click the **Select Purchase** button.

## Verifying your services' status in new region

To verify the move, open the new resource group and your services will be listed with the new region.

To move your data from your source region to the target region, please see this article's guidelines for [moving your data to the new storage account](https://docs.microsoft.com/azure/storage/common/storage-account-move?tabs=azure-portal#move-data-to-the-new-storage-account).

## Clean up resources in your original region

To commit the changes and complete the move of your service account, delete the source service account.

## Next steps

[Create an index](https://docs.microsoft.com/azure/search/search-get-started-portal)

[Create a skillset](https://docs.microsoft.com/azure/search/cognitive-search-quickstart-blob)

[Create a knowledge store](https://docs.microsoft.com/azure/search/knowledge-store-create-portal) -->