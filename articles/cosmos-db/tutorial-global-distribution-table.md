---
title: Tablo API'si için Azure Cosmos DB küresel dağıtım öğreticisi
description: Genel dağıtımın Azure Cosmos DB Tablo API'si hesaplarında nasıl çalıştığını ve tercih edilen bölge listesinin nasıl yapılandırılacağını öğrenin
author: sakash279
ms.author: akshanka
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: tutorial
ms.date: 01/30/2020
ms.reviewer: sngun
ms.openlocfilehash: f06fbe482327d01917f648ccdec34baa15c46e80
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93073919"
---
# <a name="set-up-azure-cosmos-db-global-distribution-using-the-table-api"></a>Tablo API’sini kullanarak Azure Cosmos DB genel dağıtımını ayarlama
[!INCLUDE[appliesto-table-api](includes/appliesto-table-api.md)]

Bu makale aşağıdaki görevleri kapsar: 

> [!div class="checklist"]
> * Azure portalını kullanarak genel dağıtımı yapılandırma
> * [Tablo API’sini](table-introduction.md) kullanarak genel dağıtımı yapılandırma

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-table-api"></a>Tablo API’sini kullanarak tercih edilen bir bölgeye bağlanma

[Genel dağıtımın](distribute-data-globally.md)avantajlarından yararlanabilmek için, istemci uygulamaları uygulamasının çalıştığı geçerli konumu belirtmelidir. Bu, özelliği ayarlanarak yapılır `CosmosExecutorConfiguration.CurrentRegion` . `CurrentRegion`Özelliği tek bir konum içermelidir. Her istemci örneği, düşük gecikmeli okumalar için kendi bölgelerini belirtebilir. Bölgenin, "Batı ABD" gibi [görünen adları](/previous-versions/azure/reference/gg441293(v=azure.100)) kullanılarak adlandırılması gerekir. 

Azure Cosmos DB Tablo API'si SDK 'Sı, hesap yapılandırmasına ve geçerli bölgesel kullanılabilirliğe göre iletişim kurmak için en iyi uç noktayı otomatik olarak seçer. İstemcilere daha iyi gecikme sağlamak için en yakın bölgeyi önceliklendirir. Geçerli özelliği ayarladıktan sonra `CurrentRegion` okuma ve yazma istekleri aşağıdaki gibi yönlendirilir:

* **Okuma istekleri:** Tüm okuma istekleri yapılandırılmış öğesine gönderilir `CurrentRegion` . Yakınlık temelinde, SDK otomatik olarak yüksek kullanılabilirlik için bir geri dönüş coğrafi çoğaltılan bölge seçer.

* **Yazma istekleri:** SDK, tüm yazma isteklerini otomatik olarak geçerli yazma bölgesine gönderir. Çok bölgeli yazmaları olan bir hesapta, geçerli bölge yazma isteklerini de görecektir. Yakınlık temelinde, SDK otomatik olarak yüksek kullanılabilirlik için bir geri dönüş coğrafi çoğaltılan bölge seçer.

`CurrentRegion`Özelliği belirtmezseniz, SDK tüm işlemler için geçerli yazma bölgesini kullanır.

Örneğin, bir Azure Cosmos hesabı "Batı ABD" ve "Doğu ABD" bölgelerinde yer alıyorsa. "Batı ABD" yazma bölgesidir ve uygulama "Doğu ABD" içinde mevcutsa. CurrentRegion özelliği yapılandırılmamışsa, tüm okuma ve yazma istekleri her zaman "Batı ABD" bölgesine yönlendirilir. CurrentRegion özelliği yapılandırılmışsa, tüm okuma istekleri "Doğu ABD" bölgesinden sunulur.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdakileri yaptınız:

> [!div class="checklist"]
> * Azure portalını kullanarak genel dağıtımı yapılandırma
> * Azure Cosmos DB Tablo API’lerini kullanarak genel dağıtımı yapılandırma