---
title: Azure Cosmos DB bağlanmak için Robo 3T kullanın
description: MongoDB için Robo 3T ve Azure Cosmos DB API 'sini kullanarak Azure Cosmos DB nasıl bağlanacağınızı öğrenin
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 03/23/2020
author: timsander1
ms.author: tisande
ms.openlocfilehash: 627849acb902a721c80b98adbd16103b4729591b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81114184"
---
# <a name="use-robo-3t-with-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Azure Cosmos DB API'siyle Robo 3T kullanma

Robo 3T kullanarak Cosmos hesabına bağlanmak için şunları yapmanız gerekir:

* [Robo 3T](https://robomongo.org/) indirme ve yükleme
* Cosmos DB [bağlantı dizesi](connect-mongodb-account.md) bilgilerinizi içermelidir

> [!NOTE]
> Şu anda Robo 3T v 1.2 ve alt sürümleri, Cosmos DB MongoDB için API 'SI ile desteklenmektedir.

## <a name="connect-using-robo-3t"></a>Robo 3T kullanarak bağlanma

Cosmos hesabınızı Robo 3T bağlantı yöneticisine eklemek için aşağıdaki adımları uygulayın:

1. Azure Cosmos DB API MongoDB ile yapılandırılan Cosmos hesabınız için [buradaki](connect-mongodb-account.md)yönergeleri kullanarak bağlantı bilgilerini alın.

    ![Bağlantı dizesi dikey penceresinin ekran görüntüsü](./media/mongodb-robomongo/connectionstringblade.png)
2. *Robomongo 'yu* uygulamasını çalıştırın.

3. Bağlantılarınızı yönetmek için **Dosya** altındaki bağlantı düğmesine tıklayın. Ardından, **MongoDB bağlantıları** penceresinde **Oluştur** ' a tıklayarak **bağlantı ayarları** penceresini açarsınız.

4. **Bağlantı ayarları** penceresinde bir ad seçin. Ardından, 1. adımdaki bağlantı bilgilerinizin **ana bilgisayar** ve **bağlantı noktasını** bulun ve sırasıyla **Adres** ve **bağlantı noktasına**girin.

    ![Robomongo 'yu Manage bağlantılarının ekran görüntüsü](./media/mongodb-robomongo/manageconnections.png)
5. **Kimlik doğrulama** sekmesinde **kimlik doğrulama gerçekleştir**' e tıklayın. Ardından, veritabanınızı (varsayılan *yönetici*), **Kullanıcı adını** ve **parolayı**girin.
**Kullanıcı adı** ve **parola** , adım 1 ' deki bağlantı bilgileriniz içinde bulunabilir.

    ![Robomongo 'yu kimlik doğrulama sekmesinin ekran görüntüsü](./media/mongodb-robomongo/authentication.png)
6. **SSL** SEKMESINDE, **SSL protokolünü kullan**' ı Işaretleyin ve **kimlik doğrulama yöntemini** **otomatik olarak imzalanan sertifikayla**değiştirin.

    ![Robomongo 'yu SSL sekmesinin ekran görüntüsü](./media/mongodb-robomongo/SSL.png)
7. Son olarak, bağlanabilediğinizi doğrulamak için **Test** ' e tıklayın, sonra **kaydedin**.

## <a name="next-steps"></a>Sonraki adımlar

- [Studio 3T](mongodb-mongochef.md) 'ı Azure Cosmos DB MongoDB IÇIN API 'si ile nasıl kullanacağınızı öğrenin.
- MongoDB için Azure Cosmos DB API 'siyle MongoDB [örneklerini](mongodb-samples.md) gezin.
