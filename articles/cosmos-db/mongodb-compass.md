---
title: Pusula'yı kullanarak Azure Cosmos DB'ye bağlanın
description: Azure Cosmos DB'de verileri depolamak ve yönetmek için MongoDB Compass'ı nasıl kullanacağınızı öğrenin.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 03/20/2020
author: LuisBosquez
ms.author: lbosq
ms.openlocfilehash: c683ec0c4b3a536b0627a7c1c8abf28ee4f83663
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80757020"
---
# <a name="use-mongodb-compass-to-connect-to-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Azure Cosmos DB'nin API'sine bağlanmak için MongoDB Compass'ı kullanın

Bu öğretici, Cosmos DB'de veri depolarken ve/veya yönetirken [MongoDB Compass'ın](https://www.mongodb.com/products/compass) nasıl kullanılacağını göstermektedir. Bu geçiş için Azure Cosmos DB'nin MongoDB api'sini kullanıyoruz. O sizin yabancı için, Pusula MongoDB için bir GUI olduğunu. Genellikle verilerinizi görselleştirmek, geçici sorgular çalıştırmak ve verilerinizi yönetmek için kullanılır.

Cosmos DB, Microsoft'un dünya çapında dağıtılan çok modelli veritabanı hizmetidir. Cosmos DB'nin çekirdeğindeki küresel dağıtım ve yatay ölçek özelliklerinden yararlanan belge, anahtar/değer ve grafik veritabanlarını hızla oluşturabilir ve sorgulayabilirsiniz.

## <a name="pre-requisites"></a>Ön koşullar

MongoDB Compass'ı kullanarak Cosmos DB hesabınıza bağlanmak için şunları

* [Compass'ı](https://www.mongodb.com/download-center/compass?jmp=hero) indirin ve kurun
* Cosmos DB [bağlantı dize](connect-mongodb-account.md) bilgilerinize sahip

> [!NOTE]
> Şu anda, Azure Cosmos DB'nin MongoDB Server sürüm 3.2 için API'si MongoDB Compass ile desteklenir.

## <a name="connect-to-cosmos-dbs-api-for-mongodb"></a>MongoDB için Cosmos DB'nin API'sine bağlanın

Cosmos DB hesabınızı Pusula'ya bağlamak için aşağıdaki adımları izleyebilirsiniz:

1. Azure Cosmos DB'nin API MongoDB ile yapılandırılan Cosmos hesabınıza ait bağlantı bilgilerini [buradaki](connect-mongodb-account.md)yönergeleri kullanarak alın.

    ![Bağlantı string blade ekran görüntüsü](./media/mongodb-compass/mongodb-compass-connection.png)

2. Cosmos DB'deki **Birincil/İkincil bağlantı dizenizin** yanındaki **panoya kopyala** yazan düğmeye tıklayın. Bu düğmeyi tıklattığınızda, tüm bağlantı dizenizi panonuza kopyalar.

    ![Pano düğmesine kopyanın ekran görüntüsü](./media/mongodb-compass/mongodb-connection-copy.png)

3. Masaüstünüzde/makinenizde Pusula'yı açın ve **Bağlan'a** tıklayın ve sonra **Bağlan...**.

4. Pusula panodaki bir bağlantı dizesini otomatik olarak algılar ve bağlanmak için kullanmak isteyip istemediğinizi sormak ister. Aşağıdaki ekran görüntüsünde gösterildiği gibi **Evet'e** tıklayın.

    ![Bağlanmak için Pusula isteminin ekran görüntüsü](./media/mongodb-compass/mongodb-compass-detect.png)

5. Yukarıdaki adımda **Evet'i** tıklattığınızda, bağlantı dizesinden bilgileriniz otomatik olarak doldurulur. Boş bırakıldığından emin olmak için **Yineleme Kümesi Adı** alanında otomatik olarak doldurulan değeri kaldırın.

    ![Bağlanmak için Pusula isteminin ekran görüntüsü](./media/mongodb-compass/mongodb-compass-replica.png)

6. Sayfanın altındaki **Bağlan'ı** tıklatın. Cosmos DB hesabınız ve veritabanlarınız artık MongoDB Compass içinde görünür olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Cosmos DB'nin MongoDB için API'si ile [Studio 3T'yi](mongodb-mongochef.md) nasıl kullanacağınızı öğrenin.
- Azure Cosmos DB'nin MongoDB için API'si ile MongoDB [örneklerini](mongodb-samples.md) keşfedin.
