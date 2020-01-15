---
title: Pusula kullanarak Azure Cosmos DB bağlanma
description: Azure Cosmos DB verileri depolamak ve yönetmek için MongoDB pusula 'i kullanmayı öğrenin.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: overview
ms.date: 06/24/2019
author: LuisBosquez
ms.author: lbosq
ms.openlocfilehash: 5c01bd02b1fef7c7b27a3119465cad53155b9bbf
ms.sourcegitcommit: b5106424cd7531c7084a4ac6657c4d67a05f7068
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75940956"
---
# <a name="use-mongodb-compass-to-connect-to-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Azure Cosmos DB API 'sine bağlanmak için MongoDB pusula kullanma 

Bu öğreticide, Cosmos DB veri depolarken ve/veya yönetirken [MongoDB pusula](https://www.mongodb.com/products/compass) kullanımı gösterilmektedir. Bu izlenecek yol için Azure Cosmos DB MongoDB API 'sini kullanıyoruz. Bilmediğiniz kişiler için, pusula MongoDB için bir GUI 'dir. Genellikle verilerinizi görselleştirmek, geçici sorguları çalıştırmak ve verilerinizi yönetmek için kullanılır. 

Cosmos DB, Microsoft 'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Cosmos DB temel dağıtım ve yatay ölçek özelliğinden faydalanabilir ve her şey belge, anahtar/değer ve grafik veritabanlarını hızlıca oluşturup sorgulayabilirsiniz.


## <a name="pre-requisites"></a>Önkoşullar 
Robo 3T kullanarak Cosmos DB hesabınıza bağlanmak için şunları yapmanız gerekir:

* [Pusula](https://www.mongodb.com/download-center/compass?jmp=hero) indirme ve yükleme
* Cosmos DB [bağlantı dizesi](connect-mongodb-account.md) bilgilerinizi içermelidir

## <a name="connect-to-cosmos-dbs-api-for-mongodb"></a>MongoDB için Cosmos DB API 'sine bağlanma 
Cosmos DB hesabınızı pusula 'e bağlamak için aşağıdaki adımları izleyebilirsiniz:

1. Azure Cosmos DB API MongoDB ile yapılandırılan Cosmos hesabınız için [buradaki](connect-mongodb-account.md)yönergeleri kullanarak bağlantı bilgilerini alın.

    ![Bağlantı dizesi dikey penceresinin ekran görüntüsü](./media/mongodb-compass/mongodb-compass-connection.png)

2. Cosmos DB içindeki **birincil/ikincil bağlantı dizeniz** yanında **Panoya Kopyala** yazılı düğmeye tıklayın. Bu düğmeye tıkladığınızda tüm bağlantı dizeniz panonuza kopyalanır. 

    ![Panoya kopyala düğmesinin ekran görüntüsü](./media/mongodb-compass/mongodb-connection-copy.png)

3. Masaüstünüzde/makinenizde pusula ' i açın ve **Bağlan** ' a ve ardından **Bağlan... öğesine**tıklayın. 

4. Pusula, panodaki bir bağlantı dizesini otomatik olarak algılar ve bağlanmak için kullanmak isteyip istemediğinizi sorar. Aşağıdaki ekran görüntüsünde gösterildiği gibi **Evet** ' e tıklayın.

    ![Bağlanmak için pusula isteminin ekran görüntüsü](./media/mongodb-compass/mongodb-compass-detect.png)

5. Yukarıdaki adımda **Evet 'e** tıklandıktan sonra bağlantı dizesindeki ayrıntılarınız otomatik olarak doldurulur. Boş bırakılmış olduğundan emin olmak için **çoğaltma kümesi adı** alanında otomatik olarak doldurulmuş değeri kaldırın. 

    ![Bağlanmak için pusula isteminin ekran görüntüsü](./media/mongodb-compass/mongodb-compass-replica.png)

6. Sayfanın alt kısmındaki **Bağlan** ' a tıklayın. Cosmos DB hesabınız ve veritabanları artık MongoDB pusula içinde görünür olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

- [Studio 3T](mongodb-mongochef.md) 'ı Azure Cosmos DB MongoDB IÇIN API 'si ile nasıl kullanacağınızı öğrenin.
- MongoDB için Azure Cosmos DB API 'siyle MongoDB [örneklerini](mongodb-samples.md) gezin.