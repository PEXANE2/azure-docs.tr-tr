---
title: MongoDB uygulamasını Azure Cosmos DB bağlama
description: Azure portal bir MongoDB uygulamasını bağlantı dizesi ile Azure Cosmos DB bağlama hakkında bilgi edinin
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 12/02/2019
ms.reviewer: sngun
ms.openlocfilehash: 0f2cd1b7228f2cc9cadb84232222f658a512a81f
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79246857"
---
# <a name="connect-a-mongodb-application-to-azure-cosmos-db"></a>MongoDB uygulamasını Azure Cosmos DB bağlama
MongoDB bağlantı dizesi kullanarak MongoDB uygulamanızı Azure Cosmos DB'ye bağlamayı öğrenin. Ardından MongoDB uygulamanızın veri deposu olarak bir Azure Cosmos veritabanı kullanabilirsiniz. 

Bu öğreticide bağlantı dizesi bilgilerini almak için iki yol sağlanır:

- .NET, Node. js, MongoDB kabuğu, Java ve Python sürücüleriyle kullanım için [hızlı başlangıç yöntemi](#QuickstartConnection)
- Diğer sürücülerle kullanılmak üzere [özel bağlantı dizesi yöntemi](#GetCustomConnection)

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure hesabı. Azure hesabınız yoksa, şimdi [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun. 
- Cosmos hesabı. Yönergeler için bkz. [MongoDB ve .NET SDK için Azure Cosmos DB API 'sini kullanarak Web uygulaması oluşturma](create-mongodb-dotnet.md).

## <a id="QuickstartConnection"></a>Hızlı Başlangıç kullanarak MongoDB bağlantı dizesini alın
1. Bir Internet tarayıcısında [Azure Portal](https://portal.azure.com)oturum açın.
2. **Azure Cosmos DB** dikey PENCERESINDE, API 'yi seçin. 
3. Hesap dikey penceresinin sol bölmesinde **hızlı başlangıç**' a tıklayın. 
4. Platformunuzu seçin ( **.net**, **Node. js**, **MongoDB kabuğu**, **Java**, **Python**). Sürücü veya aracınız listede görmüyorsanız endişelenmeyin, sürekli olarak daha fazla bağlantı kodu parçacığı belgeliyoruz. Görmek istediğiniz şekilde lütfen aşağıdaki açıklamayı inceleyin. Kendi bağlantınızı oluşturmayı öğrenmek için [hesabın bağlantı dizesi bilgilerini](#GetCustomConnection)okuyun.
5. Kod parçacığını kopyalayıp MongoDB uygulamanıza yapıştırın.

    ![Hızlı başlangıç dikey penceresi](./media/connect-mongodb-account/QuickStartBlade.png)

## <a id="GetCustomConnection"></a>Özelleştirecek MongoDB bağlantı dizesini al
1. Bir Internet tarayıcısında [Azure Portal](https://portal.azure.com)oturum açın.
2. **Azure Cosmos DB** dikey PENCERESINDE, API 'yi seçin. 
3. Hesap dikey penceresinin sol bölmesinde **bağlantı dizesi**' ne tıklayın. 
4. **Bağlantı dizesi** dikey penceresi açılır. Önceden oluşturulmuş bir bağlantı dizesi dahil olmak üzere MongoDB için bir sürücü kullanarak hesaba bağlanmak için gerekli tüm bilgilere sahiptir.

    ![Bağlantı Dizesi dikey penceresi](./media/connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a>Bağlantı dizesi gereksinimleri
> [!Important]
> Azure Cosmos DB katı güvenlik gereksinimleri ve standartları vardır. Azure Cosmos DB hesapların *SSL*aracılığıyla kimlik doğrulaması ve güvenli iletişim olması gerekir. 
>
>

Azure Cosmos DB, standart MongoDB bağlantı dizesi URI biçimini destekler ve birkaç belirli gereksinimi vardır: Azure Cosmos DB hesapları, SSL aracılığıyla kimlik doğrulaması ve güvenli iletişim gerektirir. Bu nedenle, bağlantı dizesi biçimi:

    mongodb://username:password@host:port/[database]?ssl=true

Bu dizenin değerleri, daha önce gösterilen **bağlantı dizesi** dikey penceresinde kullanılabilir:

* Kullanıcı adı (gerekli): Cosmos hesap adı.
* Parola (gerekli): Cosmos hesap parolası.
* Ana bilgisayar (gerekli): Cosmos hesabının FQDN 'SI.
* Bağlantı noktası (gerekli): 10255.
* Veritabanı (isteğe bağlı): bağlantının kullandığı veritabanı. Veritabanı sağlanmazsa, varsayılan veritabanı "test" dir.
* SSL = true (gerekli)

Örneğin, **bağlantı dizesi** dikey penceresinde gösterilen hesabı göz önünde bulundurun. Geçerli bir bağlantı dizesi:

    mongodb://contoso123:0Fc3IolnL12312asdfawejunASDF@asdfYXX2t8a97kghVcUzcDv98hawelufhawefafnoQRGwNj2nMPL1Y9qsIr9Srdw==@contoso123.documents.azure.com:10255/mydatabase?ssl=true

## <a name="next-steps"></a>Sonraki adımlar

- [Studio 3T](mongodb-mongochef.md) 'ı Azure Cosmos DB MongoDB IÇIN API 'si ile nasıl kullanacağınızı öğrenin.
- MongoDB için Azure Cosmos DB API 'SI ile [Robo 3T kullanmayı](mongodb-robomongo.md) öğrenin.
- MongoDB için Azure Cosmos DB API 'siyle MongoDB [örneklerini](mongodb-samples.md) gezin.
