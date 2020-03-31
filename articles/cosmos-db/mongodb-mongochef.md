---
title: MongoDB için Azure Cosmos DB'nin API'sine bağlanmak için Studio 3T'yi kullanın
description: Studio 3T'yi kullanarak Azure Cosmos DB'nin MongoDB için API'sine nasıl bağlanıştırığa öğrenin.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 03/20/2020
author: timsander1
ms.author: tisande
ms.custom: seodec18
ms.openlocfilehash: 9b8c3a36dbdd3a14d0bd325c22421033a1765df3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80063694"
---
# <a name="connect-to-an-azure-cosmos-account-using-studio-3t"></a>Studio 3T'yi kullanarak Azure Cosmos hesabına bağlanma

Studio 3T'yi kullanarak Azure Cosmos DB'nin MongoDB api'sine bağlanmak için şunları yapmanız gerekir:

* [Studio 3T'yi](https://studio3t.com/)indirin ve kurun.
* Azure Cosmos hesabınızın [bağlantı dize](connect-mongodb-account.md) bilgilerini alın.

> [!NOTE]
> Şu anda, Robo 3T v1.2 ve alt sürümleri MongoDB için Cosmos DB's API ile desteklenir.

## <a name="create-the-connection-in-studio-3t"></a>Studio 3T'de bağlantı oluşturma

Azure Cosmos hesabınızı Studio 3T bağlantı yöneticisine eklemek için aşağıdaki adımları kullanın:

1. [MongoDB uygulamasını Azure Cosmos DB makalesine bağlayın'daki](connect-mongodb-account.md) yönergeleri kullanarak Azure Cosmos DB'nizin MongoDB hesabı için API'sinin bağlantı bilgilerini alın.

    ![Bağlantı dize sayfasının ekran görüntüsü](./media/mongodb-mongochef/ConnectionStringBlade.png)

2. Bağlantı Yöneticisi'ni açmak için **Bağlan'ı** tıklatın ve ardından **Yeni Bağlantı'yı** tıklatın

    ![Studio 3T bağlantı yöneticisinin ekran görüntüsü](./media/mongodb-mongochef/ConnectionManager.png)
3. Yeni **Bağlantı** penceresinde, **Sunucu** sekmesinde Azure Cosmos hesabının HOST (FQDN) ve PORT'u girin.

    ![Studio 3T bağlantı yöneticisi sunucu sekmesinin ekran görüntüsü](./media/mongodb-mongochef/ConnectionManagerServerTab.png)
4. Yeni **Bağlantı** penceresinde, **Kimlik Doğrulama** sekmesinde Kimlik Doğrulama Modu **Temel'i (MONGODB-CR veya SCARM-SHA-1)** seçin ve KULLANICI ADI ve Parola'yı girin.  Varsayılan kimlik doğrulama db (yönetici) kabul edin veya kendi değerinizi sağlayın.

    ![Studio 3T bağlantı yöneticisi kimlik doğrulama sekmesinin ekran görüntüsü](./media/mongodb-mongochef/ConnectionManagerAuthenticationTab.png)
5. Yeni **Bağlantı** penceresinde, **SSL** sekmesinde, onay kutusunu ve **sunucunun kendi imzaladığı SSL sertifikalarını** radyo **düğmesine bağlamak için SSL'i kullan protokolünü** işaretleyin.

    ![Studio 3T bağlantı yöneticisi SSL sekmesinin ekran görüntüsü](./media/mongodb-mongochef/ConnectionManagerSSLTab.png)
6. Bağlantı bilgilerini doğrulamak için **Test Bağlantısı** düğmesini tıklatın, Yeni Bağlantı penceresine dönmek için **Tamam'ı** tıklatın ve sonra **Kaydet'i**tıklatın.

    ![Studio 3T test bağlantı penceresinin ekran görüntüsü](./media/mongodb-mongochef/TestConnectionResults.png)

## <a name="use-studio-3t-to-create-a-database-collection-and-documents"></a>Veritabanı, koleksiyon ve belgeler oluşturmak için Studio 3T'yi kullanın
Studio 3T kullanarak bir veritabanı, koleksiyon ve belge oluşturmak için aşağıdaki adımları gerçekleştirin:

1. **Bağlantı Yöneticisi'nde**bağlantıyı vurgulayın ve **Bağlan'ı**tıklatın.

    ![Studio 3T bağlantı yöneticisinin ekran görüntüsü](./media/mongodb-mongochef/ConnectToAccount.png)
2. Ana bilgisayara sağ tıklayın ve **Veritabanı Ekle'yi**seçin.  Veritabanı adı sağlayın ve **Tamam'ı**tıklatın.

    ![Studio 3T Veritabanı Ekle seçeneğinin ekran görüntüsü](./media/mongodb-mongochef/AddDatabase1.png)
3. Veritabanına sağ tıklayın ve **Koleksiyon Ekle'yi**seçin.  Bir koleksiyon adı sağlayın ve **Oluştur'u**tıklatın.

    ![Studio 3T Ekle Koleksiyonu seçeneğinin ekran görüntüsü](./media/mongodb-mongochef/AddCollection.png)
4. **Koleksiyon** menüsü öğesini tıklatın, ardından **Belge Ekle'yi**tıklatın.

    ![Studio 3T Ekle Belge menü öğesinin ekran görüntüsü](./media/mongodb-mongochef/AddDocument1.png)
5. Belge Ekle iletişim kutusunda, aşağıdakileri yapıştırın ve ardından **Belge Ekle'yi**tıklatın.

        {
        "_id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
               { "firstName": "Thomas" },
               { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "isRegistered": true
        }
6. Başka bir belge ekleyin, aşağıdaki içerik ile bu kez:

        {
        "_id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam",
                 "givenName": "Jesse",
                "gender": "female", "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            {
                "familyName": "Miller",
                 "givenName": "Lisa",
                 "gender": "female",
                 "grade": 8 }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
        }
7. Örnek sorgu yürütün. Örneğin, soyadı 'Andersen' olan aileleri arayın ve ebeveynleri ve devlet alanlarını iade edin.

    ![Mongo Chef sorgu sonuçlarının ekran görüntüsü](./media/mongodb-mongochef/QueryDocument1.png)

## <a name="next-steps"></a>Sonraki adımlar

- MongoDB için Azure Cosmos DB'nin API'si ile [Robo 3T'yi](mongodb-robomongo.md) nasıl kullanacağınızı öğrenin.
- Azure Cosmos DB'nin MongoDB için API'si ile MongoDB [örneklerini](mongodb-samples.md) keşfedin.
