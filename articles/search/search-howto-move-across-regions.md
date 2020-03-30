---
title: Hizmet kaynağınızı bölgeler arasında taşıma
titleSuffix: Azure Cognitive Search
description: Bu makalede, Azure Bilişsel Arama kaynaklarınızı Azure bulutundaki bir bölgeden diğerine nasıl taşıyacağınızı gösterecektir.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: how-to
ms.custom: subject-moving-resources
ms.date: 03/24/2020
ms.openlocfilehash: 00f16d11f7a9cd276772eda5e91d6e117ada8c9f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80246315"
---
# <a name="move-your-azure-cognitive-search-service-to-another-azure-region"></a>Azure Bilişsel Arama hizmetinizi başka bir Azure bölgesine taşıyın

Bazen müşteriler, bir arama hizmetini başka bir bölgeye taşıma hakkında sorular sorar. Şu anda, bu göreve yardımcı olacak yerleşik bir mekanizma veya araç bulunmamaktadır, ancak bu makale, aynı sonuca ulaşmak için el ile ilgili adımları anlamanıza yardımcı olabilir.

> [!NOTE]
> Azure portalında, tüm hizmetlerin bir **Dışa Aktarma şablonu komutu** var. Azure Bilişsel Arama söz konusu olduğunda, bu komut bir hizmetin temel bir tanımını (ad, konum, katman, yineleme ve bölüm sayısı) üretir, ancak hizmetiniçeriğini tanımaz veya anahtarlar, roller veya günlükler üzerinde taşımaz. Komut olsa da, bir arama hizmetini taşımak için kullanmanızı önermiyoruz.

## <a name="guidance-for-moving-a-service"></a>Bir hizmeti taşıma kılavuzu

1. Azure Bilişsel Arama'dan daha fazlasını taşımanız gerektiğinde, bir hizmetin yerini değiştirmenin tam etkisini anlamak için bağımlılıkları ve ilgili hizmetleri belirleyin.

   Azure Depolama günlüğe kaydetme, bilgi deposu oluşturma için kullanılır ve AI zenginleştirme ve dizin oluşturma için yaygın olarak kullanılan bir dış veri kaynağıdır. Bilişsel Hizmetler AI zenginleştirme bir bağımlılıktır. AI zenginleştirme kullanıyorsanız, hem Bilişsel Hizmetler hem de arama hizmetiniz aynı bölgede olmalıdır.

1. Ne taşıyacağını bilmek için hizmetteki tüm nesnelerin bir envanterini oluşturun: dizinler, eşanlamlı haritalar, dizinleyiciler, veri kaynakları, beceri kümeleri. Günlük günlüğe kaydetmeyi etkinleştirdikten sonra, geçmiş bir kayıt için ihtiyaç duyabileceğiniz raporları oluşturun ve arşivleyin.

1. Azure Bilişsel Arama'nın yanı sıra yeni bölgedeki ilgili hizmetlerin kullanılabilirliğini sağlamak için yeni bölgedeki fiyatlandırmayı ve kullanılabilirliği denetleyin. Özelliklerin çoğu tüm bölgelerde kullanılabilir, ancak bazı önizleme özellikleri sınırlı kullanılabilirliğe sahiptir.

1. Yeni bölgede bir hizmet oluşturun ve varolan dizinler, eşanlamlı lar, dizin oluşturup, veri kaynakları ve beceri kümelerini kaynak kodundan yeniden yayımlayın. Varolan adı yeniden kullanamamak için hizmet adlarının benzersiz olması gerektiğini unutmayın. Bilişsel Hizmetlerbağlantılarının aynı bölge gereksinimi açısından hala geçerli olup olmadığını görmek için her skillset'i denetleyin. Ayrıca, bilgi depoları oluşturulduysa, farklı bir hizmet kullanıyorsanız Azure Depolama'nın bağlantı dizelerini denetleyin.

1. Varsa dizinleri ve bilgi depolarını yeniden yükleyin. JSON verilerini bir dizin içine itmek için uygulama kodunu veya belgeleri dış kaynaklardan çekmek için dizin leyicileri yeniden çalıştırmak için kullanırsınız. 

1. Günlüğe kaydetmeyi etkinleştirin ve bunları kullanıyorsanız, güvenlik rollerini yeniden oluşturun.

1. Yeni hizmet adını ve API anahtarlarını kullanmak ve tüm uygulamaları test etmek için istemci uygulamalarını ve test paketlerini güncelleştirin.

1. Yeni hizmet tam olarak sınandıktan ve çalışmaya başladıktan sonra eski hizmeti silin.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki bağlantılar, yukarıda özetlenen adımları tamamlarken daha fazla bilgi bulmanıza yardımcı olabilir.

+ [Azure Bilişsel Arama fiyatlandırması ve bölgeleri](https://azure.microsoft.com/pricing/details/search/)
+ [Katman seçme](search-sku-tier.md)
+ [Arama hizmeti oluşturma](search-create-service-portal.md)
+ [Arama belgelerini yükleme](search-what-is-data-import.md)
+ [Günlü kaydını etkinleştir](search-monitor-logs.md)


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