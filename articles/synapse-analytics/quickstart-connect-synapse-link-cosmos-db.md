---
title: 'Hızlı başlangıç: Azure Cosmos DB için Azure SYNAPSE bağlantısına bağlanma'
description: SYNAPSE bağlantısı ile bir Azure Cosmos DB SYNAPSE çalışma alanına bağlama
services: synapse-analytics
author: ArnoMicrosoft
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/21/2020
ms.author: acomet
ms.reviewer: jrasnick
ms.openlocfilehash: 56b018326e8673e6ac9e625a5732a08238f982fd
ms.sourcegitcommit: 0b2367b4a9171cac4a706ae9f516e108e25db30c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86274331"
---
# <a name="connect-to-azure-synapse-link-for-azure-cosmos-db"></a>Azure Cosmos DB için Azure SYNAPSE bağlantısına bağlanma

Bu makalede, SYNAPSE link ile Azure SYNAPSE Analytics Studio 'dan bir Azure Cosmos DB veritabanına nasıl erişebileceğiniz açıklanır. 

## <a name="prerequisites"></a>Önkoşullar

Bir Azure Cosmos DB hesabını çalışma alanınıza bağlanmadan önce, ihtiyacınız olan birkaç şey vardır.

* Bu [hızlı](https://docs.microsoft.com/azure/cosmos-db/how-to-manage-database-account) başlangıcı izleyerek mevcut Azure Cosmos DB hesabı veya yeni bir hesap oluşturun
* Bu [hızlı](https://docs.microsoft.com/azure/synapse-analytics/quickstart-create-workspace) başlangıcı izleyerek var olan SYNAPSE çalışma alanı veya yeni bir çalışma alanı oluştur 

## <a name="enable-azure-cosmos-db-analytical-store"></a>Analitik depoyu Azure Cosmos DB etkinleştir

Büyük ölçekli analizleri, işletimsel performansınızı etkilemeden Azure Cosmos DB olarak çalıştırmak için, Azure Cosmos DB için SYNAPSE bağlantısının etkinleştirilmesini öneririz. Bu işlev, Azure SYNAPSE 'de bir kapsayıcıya ve yerleşik desteğe HTAP özelliği getirir. Cosmos DB kapsayıcıları için SYNAPSE bağlantısını etkinleştirmek üzere bu hızlı başlangıcı izleyin.

## <a name="navigate-to-synapse-studio"></a>SYNAPSE Studio 'ya gidin

SYNAPSE çalışma alanınızdan **SYNAPSE Studio 'Yu Başlat**' ı seçin. SYNAPSE Studio giriş sayfasında, **veri nesne Gezgini**size götürür.

## <a name="connect-an-azure-cosmos-db-database-to-a-synapse-workspace"></a>Bir Azure Cosmos DB veritabanını bir Synapse çalışma alanına bağlama

Azure Cosmos DB bir veritabanını bağlama, bağlı hizmet olarak yapılır. Cosmos DB bağlı bir hizmet, kullanıcıların Azure SYNAPSE Analytics veya SQL için Apache Spark verileri Azure Cosmos DB 'e gözatmasına, okumasına ve yazmasına olanak sağlar.

Veri Nesne Gezgini, aşağıdaki adımları uygulayarak bir Azure Cosmos DB veritabanını doğrudan bağlayabilirsiniz:

1. ***+*** Verilerin yakınında simge Seç
2. **Dış verilere bağlan** ' ı seçin
3. Bağlanmak istediğiniz API 'yi seçin: SQL veya MongoDB
4. ***Devam et*** 'i seçin
5. Bağlı hizmeti adlandırın. Ad Nesne Gezgini görüntülenir ve veritabanı ve kapsayıcılara bağlanmak için SYNAPSE çalışma zamanları tarafından kullanılır. Kolay bir ad kullanmanızı öneririz.
6. **Cosmos DB hesap adı** ve **veritabanı adını** seçin
7. Seçim Hiçbir bölge belirtilmemişse, SYNAPSE çalışma zamanı işlemleri, analitik deponun etkinleştirildiği en yakın bölgeye yönlendirilir. Ancak, kullanıcılarınızın analitik depoya Cosmos DB erişmesini istediğiniz bölgeyi el ile ayarlayabilirsiniz. **Ek bağlantı özellikleri** ' ni ve ardından **Yeni**' yi seçin. **Özellik adı**altında, ***preferredregion*** yazın ve **değeri** Istediğiniz bölgeye ayarlayın (örnek: WestUS2, sözcükler ve sayılar arasında boşluk yoktur)
8. ***Oluştur***’u seçin

Azure Cosmos DB veritabanları Azure Cosmos DB bölümünde **bağlantılı** sekmenin altında görünür. Bir HTAP 'ın etkin Azure Cosmos DB kapsayıcısını yalnızca OLTP kapsayıcısından aşağıdaki simgelerle ayırt edebilirsiniz:

**SYNAPSE kapsayıcısı**:

![HTAP kapsayıcısı](./media/quickstart-connect-synapse-link-cosmosdb/htap-container.png)

**Yalnızca OLTP kapsayıcısı**:

![OLTP kapsayıcısı](./media/quickstart-connect-synapse-link-cosmosdb/oltp-container.png)

## <a name="quickly-interact-with-code-generated-actions"></a>Kod tarafından oluşturulan eylemlerle hızlıca etkileşim kurun

Bir kapsayıcıya sağ tıkladığınızda Spark veya SQL çalışma zamanının tetikleyeceği hareketlerin listesini görürsünüz. Bir kapsayıcıya yazmak Azure Cosmos DB Işlem deposu aracılığıyla gerçekleşir ve Istek birimlerini kullanır.  

## <a name="next-steps"></a>Sonraki adımlar

* [SYNAPSE ve Azure Cosmos DB arasında nelerin desteklendiğini öğrenin](./synapse-link/concept-synapse-link-cosmos-db-support.md)
* [Azure SYNAPSE Analytics için Apache Spark analitik bir depoyu sorgulama hakkında bilgi edinin](synapse-link/how-to-query-analytical-store-spark.md)