---
title: Azure Cosmos DB'ye bağlanmak için Robo 3T'yi kullanın
description: Robo 3T ve Azure Cosmos DB'nin MongoDB için API'sini kullanarak Azure Cosmos DB'ye nasıl bağlanışlarınızı öğrenin
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 03/23/2020
author: timsander1
ms.author: tisande
ms.openlocfilehash: 68b880957754439b3b88a0cccff2218a5942f967
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80548808"
---
# <a name="use-robo-3t-with-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Azure Cosmos DB API'siyle Robo 3T kullanma

Robo 3T'yi kullanarak Cosmos hesabına bağlanmak için şunları yapmamalısınız:

* [Robo 3T'yi](https://robomongo.org/) indirin ve kurun
* Cosmos DB [bağlantı dize](connect-mongodb-account.md) bilgilerinize sahip

> [!NOTE]
> Şu anda, Robo 3T v1.2 ve alt sürümleri MongoDB için Cosmos DB's API ile desteklenir.

## <a name="connect-using-robo-3t"></a>Robo 3T kullanarak bağlanma

Cosmos hesabınızı Robo 3T bağlantı yöneticisine eklemek için aşağıdaki adımları gerçekleştirin:

1. Azure Cosmos DB'nin API MongoDB ile yapılandırılan Cosmos hesabınıza ait bağlantı bilgilerini [buradaki](connect-mongodb-account.md)yönergeleri kullanarak alın.

    ![Bağlantı string blade ekran görüntüsü](./media/mongodb-robomongo/connectionstringblade.png)
2. *Robomongo.exe* çalıştırın

3. Bağlantılarınızı yönetmek için **Dosya'nın** altındaki bağlantı düğmesini tıklatın. Ardından, **Bağlantı Ayarları** penceresini açacak **Olan MongoDB Bağlantıları** penceresinde **Oluştur'u** tıklatın.

4. Bağlantı **Ayarları** penceresinde bir ad seçin. Ardından, Bağlantı **Noktası** ve **Bağlantı Noktası'nı** Adım 1'deki bağlantı bilgilerinizden bulun ve bunları sırasıyla **Adres** ve **Bağlantı Noktası'na**girin.

    ![Robomongo Bağlantılarını Yönet ekran görüntüsü](./media/mongodb-robomongo/manageconnections.png)
5. Kimlik **Doğrulama** sekmesinde **kimlik doğrulamayı gerçekleştir'i**tıklatın. Daha sonra Veritabanınızı girin (varsayılan *yöneticidir),* **Kullanıcı Adı** ve **Şifre.**
Hem **Kullanıcı Adı** hem de **Parola,** Adım 1'deki bağlantı bilgilerinizde bulunabilir.

    ![Robomongo Kimlik Doğrulama Sekmesi ekran görüntüsü](./media/mongodb-robomongo/authentication.png)
6. **SSL** sekmesinde, **SSL protokolünü kullan'ı**denetleyin, ardından **Kimlik Doğrulama Yöntemini** **Kendi İmzalanan Sertifika**olarak değiştirin.

    ![Robomongo SSL Sekmesi ekran görüntüsü](./media/mongodb-robomongo/SSL.png)
7. Son olarak, bağlanabildiğinizi doğrulamak için **Test'i** tıklatın ve ardından **Kaydet'i kaydedin.**

## <a name="next-steps"></a>Sonraki adımlar

- Azure Cosmos DB'nin MongoDB için API'si ile [Studio 3T'yi](mongodb-mongochef.md) nasıl kullanacağınızı öğrenin.
- Azure Cosmos DB'nin MongoDB için API'si ile MongoDB [örneklerini](mongodb-samples.md) keşfedin.
