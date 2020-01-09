---
title: MongoDB için Azure Cosmos DB API 'sine bağlanmak için Studio 3T kullanın
description: Studio 3T kullanarak MongoDB için Azure Cosmos DB API 'sine nasıl bağlanacağınızı öğrenin.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 11/01/2019
author: sivethe
ms.author: sivethe
ms.custom: seodec18
ms.openlocfilehash: 533917e4cc39a1f4885a1604c11480fe09870c61
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75441605"
---
# <a name="connect-to-an-azure-cosmos-account-using-studio-3t"></a>Studio 3T kullanarak bir Azure Cosmos hesabına bağlanma

Studio 3T kullanarak MongoDB için Azure Cosmos DB API 'sine bağlanmak için şunları yapmanız gerekir:

* [Studio 3T](https://studio3t.com/)'yi indirip yükleyin.
* Azure Cosmos hesabınızın [bağlantı dizesi](connect-mongodb-account.md) bilgilerine sahip olmanız gerekir.

> [!NOTE]
> Şu anda Robo 3T v 1.2 ve alt sürümleri, Cosmos DB MongoDB için API 'SI ile desteklenmektedir.

## <a name="create-the-connection-in-studio-3t"></a>Studio 3t'yi bağlantı oluşturma

Azure Cosmos hesabınızı Studio 3T bağlantı Yöneticisi 'ne eklemek için aşağıdaki adımları kullanın:

1. MongoDB uygulaması için Azure Cosmos DB API 'sine yönelik bağlantı bilgilerini [Azure Cosmos DB uygulamasına bağlama](connect-mongodb-account.md) makalesindeki yönergeleri kullanarak alın.

    ![Bağlantı dizesi sayfasının ekran görüntüsü](./media/mongodb-mongochef/ConnectionStringBlade.png)

2. Tıklayın **Connect** Bağlantı Yöneticisi'ni açmak için ardından **yeni bağlantı**

    ![Studio 3T bağlantı Yöneticisi ekran görüntüsü](./media/mongodb-mongochef/ConnectionManager.png)
3. **Yeni bağlantı** penceresindeki **sunucu** sekmesinde, Azure Cosmos hesabının (FQDN) ana bilgisayarını ve bağlantı noktasını girin.

    ![Studio 3T bağlantı Yöneticisi sunucu sekmesinin ekran görüntüsü](./media/mongodb-mongochef/ConnectionManagerServerTab.png)
4. İçinde **yeni bağlantı** penceresi, **kimlik doğrulaması** sekmesinde, kimlik doğrulama modu seçme **temel (MONGODB CR veya SCARM-SHA-1)** kullanıcı adı ve parola girin.  Varsayılan kimlik doğrulaması db (Yönetici) kabul edin veya kendi değer sağlayın.

    ![Studio 3T bağlantı Yöneticisi kimlik doğrulama sekmesinin ekran görüntüsü](./media/mongodb-mongochef/ConnectionManagerAuthenticationTab.png)
5. İçinde **yeni bağlantı** penceresi, **SSL** sekmesinde, onay **bağlanmak için SSL kullan Protokolü** onay kutusunu ve **sunucu otomatik olarak imzalanan SSL sertifikalarını kabul et**  radyo düğmesi.

    ![Studio 3T bağlantı Yöneticisi SSL sekmesinin ekran görüntüsü](./media/mongodb-mongochef/ConnectionManagerSSLTab.png)
6. Tıklayın **Test Bağlantısı** bağlantı bilgilerini doğrulamak için düğmeyi **Tamam** yeni bağlantı penceresine dönün ve ardından **Kaydet**.

    ![Studio 3T test bağlantı penceresinin ekran görüntüsü](./media/mongodb-mongochef/TestConnectionResults.png)

## <a name="use-studio-3t-to-create-a-database-collection-and-documents"></a>Bir veritabanı, koleksiyon ve belgelerin oluşturmak için Studio 3t'yi kullanma
Veritabanı, koleksiyon ve belgelerin Studio 3T kullanma oluşturmak için aşağıdaki adımları gerçekleştirin:

1. İçinde **Bağlantı Yöneticisi**, bağlantı vurgulayıp **Connect**.

    ![Studio 3T bağlantı Yöneticisi ekran görüntüsü](./media/mongodb-mongochef/ConnectToAccount.png)
2. Konağa sağ tıklayın ve seçin **veritabanı ekleme**.  Bir veritabanı adı girin ve tıklatın **Tamam**.

    ![Studio 3T veritabanı ekleme seçeneğinin ekran görüntüsü](./media/mongodb-mongochef/AddDatabase1.png)
3. Veritabanına sağ tıklayın ve seçin **koleksiyon Ekle**.  Bir koleksiyon adı girin ve tıklatın **Oluştur**.

    ![Studio 3T koleksiyon ekle seçeneğinin ekran görüntüsü](./media/mongodb-mongochef/AddCollection.png)
4. Tıklayın **koleksiyon** menü öğesi, ardından **Belge Ekle**.

    ![Studio 3T belge Ekle menü öğesinin ekran görüntüsü](./media/mongodb-mongochef/AddDocument1.png)
5. Belge Ekle iletişim kutusunda, aşağıdakini yapıştırın ve ardından **Belge Ekle**.

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
6. Bu süre aşağıdaki içeriğe sahip başka bir belge ekleyin:

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
7. Örnek sorgu yürütün. Örneğin, Soyadı 'Andersen' aileleriyle arayabilir ve durumu alanları ve üst öğeleri döndürür.

    ![Mongo Chef sorgu sonuçlarının ekran görüntüsü](./media/mongodb-mongochef/QueryDocument1.png)

## <a name="next-steps"></a>Sonraki adımlar

- MongoDB için Azure Cosmos DB API 'SI ile [Robo 3T kullanmayı](mongodb-robomongo.md) öğrenin.
- MongoDB için Azure Cosmos DB API 'siyle MongoDB [örneklerini](mongodb-samples.md) gezin.
