---
title: Azure Cosmos DB için Azure SYNAPSE bağlantısına (Önizleme) bağlanma
description: Azure SYNAPSE bağlantısı ile bir Azure Cosmos DB SYNAPSE çalışma alanına bağlama
services: synapse-analytics
author: ArnoMicrosoft
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: synapse-link
ms.date: 04/21/2020
ms.author: acomet
ms.reviewer: jrasnick
ms.openlocfilehash: fa33f2d9e0c4606587ca5ef65a3f4a021319431f
ms.sourcegitcommit: 3988965cc52a30fc5fed0794a89db15212ab23d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2020
ms.locfileid: "85195054"
---
# <a name="connect-to-azure-synapse-link-preview-for-azure-cosmos-db"></a>Azure Cosmos DB için Azure SYNAPSE bağlantısına (Önizleme) bağlanma

Bu makalede Azure SYNAPSE bağlantısıyla Azure SYNAPSE Analytics Studio 'dan bir Azure Cosmos DB veritabanına nasıl erişebileceğiniz açıklanır.

## <a name="prerequisites"></a>Ön koşullar

Azure Cosmos DB bir veritabanını çalışma alanınıza bağlanmadan önce şunlar gerekir:

> [!IMPORTANT]
> Azure Cosmos DB için Azure SYNAPSE bağlantısı şu anda yönetilen sanal ağ etkin olmayan çalışma alanlarında destekleniyor. 

* Mevcut Azure Cosmos DB veritabanı veya bu [hızlı](https://docs.microsoft.com/azure/cosmos-db/how-to-manage-database-account) başlangıcı izleyerek yeni bir hesap oluştur
* Bu [hızlı](https://docs.microsoft.com/azure/synapse-analytics/quickstart-create-workspace) başlangıcı izleyerek var olan SYNAPSE çalışma alanı veya yeni bir çalışma alanı oluştur 

## <a name="enable-azure-cosmos-db-analytical-store"></a>Analitik depoyu Azure Cosmos DB etkinleştir

Büyük ölçekli analizleri, işletimsel performansınızı etkilemeden Azure Cosmos DB olarak çalıştırmak için, Azure Cosmos DB için SYNAPSE bağlantısının etkinleştirilmesini öneririz. SYNAPSE link, Azure SYNAPSE 'de bir kapsayıcıya ve yerleşik desteğe HTAP özelliği sunar.

## <a name="navigate-to-synapse-studio"></a>SYNAPSE Studio 'ya gidin

SYNAPSE çalışma alanınızdan **SYNAPSE Studio 'Yu Başlat**' ı seçin. SYNAPSE Studio giriş sayfasında, **veri nesne Gezgini**size götürür.

## <a name="connect-an-azure-cosmos-db-database-to-a-synapse-workspace"></a>Bir Azure Cosmos DB veritabanını bir Synapse çalışma alanına bağlama

Azure Cosmos DB bir veritabanını bağlama, bağlantılı hizmet olarak yapılır. Azure Cosmos DB bağlı bir hizmet, kullanıcıların Azure SYNAPSE Analytics veya SQL için Apache Spark verileri Azure Cosmos DB 'e gözatmasına, okumasına ve yazmasına olanak sağlar.

Veri Nesne Gezgini, aşağıdaki adımları uygulayarak bir Azure Cosmos DB veritabanına doğrudan bağlanabilirsiniz:

1. ***+*** Verilerin yakınında simge Seç
2. **Dış verilere bağlan** ' ı seçin
3. Bağlanmak istediğiniz API 'yi seçin: MongoDB için SQL API veya API
4. ***Devam et*** 'i seçin
5. Bağlı hizmeti adlandırın. Ad Nesne Gezgini görüntülenir ve veritabanı ve kapsayıcılara bağlanmak için SYNAPSE çalışma zamanları tarafından kullanılır. Kolay bir ad kullanmanızı öneririz.
6. **Azure Cosmos DB hesap adı** ve **veritabanı adını** seçin
7. Seçim Hiçbir bölge belirtilmemişse, SYNAPSE çalışma zamanı işlemleri, analitik deponun etkinleştirildiği en yakın bölgeye yönlendirilir. Ancak, kullanıcılarınızın analitik depoya Azure Cosmos DB erişmesini istediğiniz bölgeyi el ile ayarlayabilirsiniz. **Ek bağlantı özellikleri** ' ni ve ardından **Yeni**' yi seçin. **Özellik adı**altında, ***preferredregion*** yazın ve **değeri** Istediğiniz bölgeye ayarlayın (örnek: WestUS2, sözcükler ve sayı arasında boşluk yoktur)
8. ***Oluştur*** ' u seçin

Azure Cosmos DB veritabanları Azure Cosmos DB bölümünde **bağlantılı** sekmenin altında görünür. Azure Cosmos DB ile, bir HTAP etkin kapsayıcıyı aşağıdaki simgelerle yalnızca OLTP kapsayıcısından ayırt edebilirsiniz:

**Yalnızca OLTP kapsayıcısı**:

![OLTP kapsayıcısı](../media/quickstart-connect-synapse-link-cosmosdb/oltp-container.png)

**Kapsayıcı etkin öğesine dokunun**:

![HTAP kapsayıcısı](../media/quickstart-connect-synapse-link-cosmosdb/htap-container.png)

## <a name="quickly-interact-with-code-generated-actions"></a>Kod tarafından oluşturulan eylemlerle hızlıca etkileşim kurun

Bir kapsayıcıya sağ tıklayarak Spark veya SQL çalışma zamanı tetiklenecek hareketlerle ilgili bir listeniz vardır. Bir kapsayıcıya yazmak Azure Cosmos DB Işlem deposu aracılığıyla gerçekleşir ve Istek birimlerini kullanır.  

## <a name="next-steps"></a>Sonraki adımlar

* [SYNAPSE ve Azure Cosmos DB arasında nelerin desteklendiğini öğrenin](./concept-synapse-link-cosmos-db-support.md)
* [Spark ile analitik depoyu sorgulama hakkında bilgi edinin](./how-to-query-analytical-store-spark.md)