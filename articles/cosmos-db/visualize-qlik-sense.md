---
title: Qlik Sense'i Azure Cosmos DB'ye bağlayın ve verilerinizi görselleştirin
description: Bu makalede, Azure Cosmos DB'yi Qlik Sense'e bağlamak ve verilerinizi görselleştirmek için gereken adımlar açıklanmaktadır.
ms.service: cosmos-db
author: SnehaGunda
ms.author: sngun
ms.topic: conceptual
ms.date: 05/23/2019
ms.reviewer: sngun
ms.openlocfilehash: 3a955060eb5f19544860c1c97abe1577084bef24
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67985539"
---
# <a name="connect-qlik-sense-to-azure-cosmos-db-and-visualize-your-data"></a>Qlik Sense'i Azure Cosmos DB'ye bağlayın ve verilerinizi görselleştirin

Qlik Sense, farklı kaynaklardan gelen verileri tek bir görünümde birleştiren bir veri görselleştirme aracıdır. Qlik Sense, verilerinizdeki olası her ilişkiyi dizinler, böylece verilere anında bilgi edinebilirsiniz. Qlik Sense'i kullanarak Azure Cosmos DB verilerini görselleştirebilirsiniz. Bu makalede, Azure Cosmos DB'yi Qlik Sense'e bağlamak ve verilerinizi görselleştirmek için gereken adımlar açıklanmaktadır. 

> [!NOTE]
> Qlik Sense'in Azure Cosmos DB'sine bağlanması şu anda yalnızca MongoDB hesapları için SQL API ve Azure Cosmos DB'nin API'si için desteklenir.

Qlik Sense'i Azure Cosmos DB'ye şu yolla bağlayabilirsiniz:

* Cosmos DB SQL API ODBC konektörü kullanarak.

* Qlik Sense MongoDB bağlayıcısını kullanarak Azure Cosmos DB'nin MongoDB api'si (şu anda önizlemede).

* Qlik Sense REST API konektörünü kullanarak Azure Cosmos DB'nin MongoDB ve SQL API api'si.

* Cosmos DB Mongo DB API Qlik Core için gRPC konektörü kullanarak.
Bu makalede, ODBC bağlayıcısı kullanarak Cosmos DB SQL API'ye bağlanmanın ayrıntıları açıklanmaktadır.

Bu makalede, ODBC bağlayıcısı kullanarak Cosmos DB SQL API'ye bağlanmanın ayrıntıları açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki yönergeleri izleyerek önce, aşağıdaki kaynakların hazır olduğundan emin olun:

* [Qlik Sense Desktop'ı](https://www.qlik.com/us/try-or-buy/download-qlik-sense) indirin veya [Qlik Sense pazar öğesini yükleyerek](https://azuremarketplace.microsoft.com/marketplace/apps/qlik.qlik-sense)Azure'da Qlik Sense'i ayarlayın.

* Video [oyunu verilerini](https://www.kaggle.com/gregorut/videogamesales)indirin, bu örnek veriler CSV formatındadır. Bu verileri bir Cosmos DB hesabında saklayacak ve Qlik Sense'de görselleştireceksiniz.

* Hızlı başlangıç makalesinin hesap bölümünde açıklanan adımları kullanarak bir Azure Cosmos DB SQL API hesabı [oluşturun.](create-sql-api-dotnet.md#create-account)

* [Bir veritabanı ve koleksiyon oluşturun](create-sql-api-java.md#add-a-container) – Koleksiyon oluşturma değerini 1000 RU/s olarak ayarla'yı kullanabilirsiniz. 

* Örnek video oyunu satış verilerini Cosmos DB hesabınıza yükleyin. Azure Cosmos DB veri geçiş aracını kullanarak verileri içe aktarabilir, [sıralı](import-data.md#SQLSeqTarget) veya toplu veri [aktarımını](import-data.md#SQLBulkTarget) yapabilirsiniz. Verilerin Cosmos DB hesabına aktarılması yaklaşık 3-5 dakika sürer.

* ODBC sürücü makalesi [ile Cosmos DB'ye bağlanmadaki](odbc-driver.md) adımları kullanarak ODBC sürücüsünü indirin, yükleyin ve yapılandırın. Video oyunu verileri basit bir veri kümesidir ve şemayı yeniden ayarlamak zorunda değilsiniz, yalnızca varsayılan toplama eşleme şemasını kullanın.

## <a name="connect-qlik-sense-to-cosmos-db"></a>Qlik Sense'i Cosmos DB'ye bağlayın

1. Qlik Sense'i açın ve **yeni uygulama oluştur'u**seçin. Uygulamanız için bir ad sağlayın ve **Oluştur'u**seçin.

   ![Yeni bir Qlik Sense uygulaması oluşturun](./media/visualize-qlik-sense/create-new-qlik-sense-app.png)

2. Yeni uygulama başarıyla oluşturulduktan sonra, **Uygulamayı Aç'ı** seçin ve **dosyalardan ve diğer kaynaklardan veri ekle'yi**seçin. 

3. Veri kaynaklarından, yeni bağlantı kurulum penceresini açmak için **ODBC'yi** seçin. 

4. Kullanıcı **DSN'ye** geçin ve daha önce oluşturduğunuz ODBC bağlantısını seçin. Bağlantı için bir ad sağlayın ve **Oluştur'u**seçin. 

   ![Yeni bağlantı oluşturma](./media/visualize-qlik-sense/create-new-connection.png)

5. Bağlantıyı oluşturduktan sonra, veritabanını, video oyunu verilerinin bulunduğu koleksiyonu seçebilir ve ardından önizlemesini yapabilirsiniz.

   ![Veritabanını ve koleksiyonu seçin](./media/visualize-qlik-sense/choose-database-and-collection.png) 

6. Sonraki seçverileri Qlik Sense'e yüklemek için **veri ekle'yi** seçin. Qlik Sense'e veri yükledikten sonra, öngörüler oluşturabilir ve veriler üzerinde analiz ler gerçekleştirebilirsiniz. Bilgileri kullanabilir veya video oyunları satışlarını keşfederek kendi uygulamanızı oluşturabilirsiniz. Aşağıdaki resim gösterir 

   ![Verileri görselleştirme](./media/visualize-qlik-sense/visualize-data.png)

### <a name="limitations-when-connecting-with-odbc"></a>ODBC'ye bağlanırken sınırlamalar 

Cosmos DB, geliştirici gereksinimleri ne olursa olsun modellenmiş sürücüleri içeren şemasız dağıtılmış bir veritabanıdır. ODBC sürücüsü, sütunları, veri türlerini ve diğer özellikleri çıkarmak için şema içeren bir veritabanı gerektirir. SQL API ANSI SQL olmadığından, normal SQL sorgusu veya ilişkisel özelliği olan DML sözdizimi Cosmos DB SQL API için geçerli değildir. Bu nedenle, ODBC sürücüsü aracılığıyla verilen SQL deyimleri, tüm yapılar için eşdeğerleri olmayan Cosmos DB'ye özgü SQL sözdizimine çevrilir. Bu çeviri sorunlarını önlemek için, ODBC bağlantısını kurarken bir şema uygulamanız gerekir. [ODBC sürücü](odbc-driver.md) makalesi ile bağlantı size şemayı yapılandırmanıza yardımcı olacak öneriler ve yöntemler sunar. Cosmos DB hesabındaki her veritabanı/koleksiyon için bu eşlemi oluşturduğunuzdan emin olun.

## <a name="next-steps"></a>Sonraki Adımlar

Power BI gibi farklı bir görselleştirme aracı kullanıyorsanız, aşağıdaki dokümandaki yönergeleri kullanarak bu araca bağlanabilirsiniz:

* [Power BI konektörünü kullanarak Cosmos DB verilerini görselleştirin](powerbi-visualize.md)
