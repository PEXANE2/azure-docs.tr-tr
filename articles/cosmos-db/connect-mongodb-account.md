---
title: Azure Cosmos DB’ye MongoDB uygulaması bağlama
description: Azure portalından bağlantı dizesini alarak Bir MongoDB uygulamasını Azure Cosmos DB'ye nasıl bağlayabilirsiniz öğrenin
author: timsander1
ms.author: tisande
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 03/19/2020
ms.reviewer: sngun
ms.openlocfilehash: e3ab6282a3c61e12dce5dd17bc0859c0d73a7724
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80051705"
---
# <a name="connect-a-mongodb-application-to-azure-cosmos-db"></a>Azure Cosmos DB’ye MongoDB uygulaması bağlama

MongoDB bağlantı dizesi kullanarak MongoDB uygulamanızı Azure Cosmos DB'ye bağlamayı öğrenin. Ardından MongoDB uygulamanızın veri deposu olarak bir Azure Cosmos veritabanı kullanabilirsiniz.

Bu öğreticide bağlantı dizesi bilgilerini almak için iki yol sağlanır:

- [.NET,](#get-the-mongodb-connection-string-by-using-the-quick-start)Node.js, MongoDB Shell, Java ve Python sürücüleri ile kullanılmak üzere hızlı başlatma yöntemi
- [Özel bağlantı dize yöntemi,](#get-the-mongodb-connection-string-to-customize)diğer sürücülerile kullanmak için

## <a name="prerequisites"></a>Ön koşullar

- Bir Azure hesabı. Azure hesabınız yoksa, şimdi ücretsiz bir [Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
- Bir Cosmos hesabı. Talimatlar için, [MongoDB ve .NET SDK için Azure Cosmos DB'nin API'yi kullanarak bir web uygulaması oluşturun'a](create-mongodb-dotnet.md)bakın.

## <a name="get-the-mongodb-connection-string-by-using-the-quick-start"></a>Hızlı başlatmayı kullanarak MongoDB bağlantı dizesini alın

1. Bir Internet tarayıcısında Azure [portalında](https://portal.azure.com)oturum açın.
2. Azure **Cosmos DB** bıçakta API'yi seçin.
3. Hesap bıçağının sol bölmesinde **Hızlı başlat'ı**tıklatın.
4. Platformunuzu seçin (**.NET**, **Node.js**, **MongoDB Shell**, **Java**, **Python**). Sürücünüzün veya aracınızın listelenmemesini istemiyorsanız, endişelenmeyin, sürekli olarak daha fazla bağlantı kodu snippet'i belgeliyoruz. Lütfen görmek istedikleriniz hakkında aşağıda yorum yapın. Kendi bağlantınızı nasıl kurarak işlerinizi nasıl kururabilirsiniz öğrenmek için [hesabın bağlantı dize bilgilerini al'ı](#get-the-mongodb-connection-string-to-customize)okuyun.
5. Kod parçacıklarını MongoDB uygulamanıza kopyalayıp yapıştırın.

    ![Hızlı başlangıç bıçağı](./media/connect-mongodb-account/QuickStartBlade.png)

## <a name="get-the-mongodb-connection-string-to-customize"></a>Özelleştirmek için MongoDB bağlantı dizesini alın

1. Bir Internet tarayıcısında Azure [portalında](https://portal.azure.com)oturum açın.
2. Azure **Cosmos DB** bıçakta API'yi seçin.
3. Hesap bıçağının sol bölmesinde **Bağlantı Dizesini**tıklatın.
4. **Connection String** bıçağı açılır. Önceden oluşturulmuş bir bağlantı dizesi de dahil olmak üzere, MongoDB için bir sürücü kullanarak hesaba bağlanmak için gerekli tüm bilgilere sahiptir.

   [![Bağlantı String](./media/connect-mongodb-account/ConnectionStringBlade.png) blade](./media/connect-mongodb-account/ConnectionStringBlade.png#lightbox)

## <a name="connection-string-requirements"></a>Bağlantı dize gereksinimleri

> [!Important]
> Azure Cosmos DB sıkı güvenlik gereksinimlerine ve standartlara sahiptir. Azure Cosmos DB hesapları kimlik doğrulaması ve *SSL*üzerinden güvenli iletişim gerektirir. 
>
>

Azure Cosmos DB, standart MongoDB bağlantı dizesi URI biçimini birkaç özel gereksinimle destekler: Azure Cosmos DB hesapları kimlik doğrulaması ve SSL üzerinden güvenli iletişim gerektirir. Yani, bağlantı dize biçimi:

    mongodb://username:password@host:port/[database]?ssl=true

Bu dize değerleri daha önce gösterilen **Bağlantı String** bıçak mevcuttur:

* Kullanıcı adı (gerekli): Cosmos hesap adı.
* Şifre (gerekli): Cosmos hesap parolası.
* Ana bilgisayar (gerekli): Cosmos hesabının FQDN.
* Bağlantı noktası (gerekli): 10255.
* Veritabanı (isteğe bağlı): Bağlantının kullandığı veritabanı. Veritabanı sağlanmadıysa, varsayılan veritabanı "sınama" olur.
* ssl=true (gerekli)

Örneğin, Bağlantı String bıyıkta gösterilen hesabı göz önünde **bulundurun.** Geçerli bir bağlantı dizesi:

    mongodb://contoso123:0Fc3IolnL12312asdfawejunASDF@asdfYXX2t8a97kghVcUzcDv98hawelufhawefafnoQRGwNj2nMPL1Y9qsIr9Srdw==@contoso123.documents.azure.com:10255/mydatabase?ssl=true

## <a name="next-steps"></a>Sonraki adımlar

- Azure Cosmos DB'nin MongoDB için API'si ile [Studio 3T'yi](mongodb-mongochef.md) nasıl kullanacağınızı öğrenin.
- MongoDB için Azure Cosmos DB'nin API'si ile [Robo 3T'yi](mongodb-robomongo.md) nasıl kullanacağınızı öğrenin.
- Azure Cosmos DB'nin MongoDB için API'si ile MongoDB [örneklerini](mongodb-samples.md) keşfedin.
