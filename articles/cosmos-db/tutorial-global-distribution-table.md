---
title: Tablo API'si için Azure Cosmos DB küresel dağıtım öğreticisi
description: Azure Cosmos DB Tablo API hesaplarında küresel dağılımın nasıl çalıştığını ve tercih edilen bölgeler listesini nasıl yapılandırılabildiğini öğrenin
author: sakash279
ms.author: akshanka
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: tutorial
ms.date: 01/30/2020
ms.reviewer: sngun
ms.openlocfilehash: 627086bdb13acdd29821af399f90fee8deaae432
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76900188"
---
# <a name="set-up-azure-cosmos-db-global-distribution-using-the-table-api"></a>Tablo API’sini kullanarak Azure Cosmos DB genel dağıtımını ayarlama

Bu makale aşağıdaki görevleri kapsar: 

> [!div class="checklist"]
> * Azure portalını kullanarak genel dağıtımı yapılandırma
> * [Tablo API’sini](table-introduction.md) kullanarak genel dağıtımı yapılandırma

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-table-api"></a>Tablo API’sini kullanarak tercih edilen bir bölgeye bağlanma

[Genel dağıtımdan](distribute-data-globally.md)yararlanabilmek için istemci uygulamaları, uygulamalarının çalıştığı geçerli konumu belirtmelidir. Bu `CosmosExecutorConfiguration.CurrentRegion` özellik ayarlayarak yapılır. Özellik `CurrentRegion` tek bir konum içermelidir. Her istemci örneği düşük gecikme sonu okumaları için kendi bölgesini belirtebilir. Bölge, "Batı ABD" gibi [görüntü adlarını](https://msdn.microsoft.com/library/azure/gg441293.aspx) kullanarak adlandırılmalıdır. 

Azure Cosmos DB Table API SDK, hesap yapılandırmasına ve geçerli bölgesel kullanılabilirlik durumuna bağlı olarak iletişim kurmak için en iyi bitiş noktasını otomatik olarak seçer. Müşterilere daha iyi gecikme sonu sağlamak için en yakın bölgeye öncelik verir. Geçerli `CurrentRegion` özelliği ayarladıktan sonra, okuma ve yazma istekleri aşağıdaki gibi yönlendirilir:

* **İstekleri okuyun:** Tüm okuma istekleri yapılandırılan `CurrentRegion`asürüye gönderilir. Yakınlığı temel alan SDK, yüksek kullanılabilirlik için otomatik olarak coğrafi olarak çoğaltılan bir bölge seçer.

* **İstek yazma:** SDK, tüm yazma isteklerini geçerli yazma bölgesine otomatik olarak gönderir. Çok ana hesapta, geçerli bölge yazma isteklerine de hizmet edecektir. Yakınlığı temel alan SDK, yüksek kullanılabilirlik için otomatik olarak coğrafi olarak çoğaltılan bir bölge seçer.

`CurrentRegion` Özelliği belirtmezseniz, SDK tüm işlemler için geçerli yazma bölgesini kullanır.

Örneğin, bir Azure Cosmos hesabı "Batı ABD" ve "Doğu ABD" bölgelerindeyse. "Batı ABD" yazma bölgesi ise ve uygulama "Doğu ABD"de mevcutsa. CurrentRegion özelliği yapılandırılmamışsa, tüm okuma ve yazma istekleri her zaman "Batı ABD" bölgesine yönlendirilir. CurrentRegion özelliği yapılandırılırsa, tüm okuma istekleri "Doğu ABD" bölgesinden sunulur.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdakileri yaptınız:

> [!div class="checklist"]
> * Azure portalını kullanarak genel dağıtımı yapılandırma
> * Azure Cosmos DB Tablo API’lerini kullanarak genel dağıtımı yapılandırma

