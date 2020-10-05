---
title: dosya dahil etme
description: dosya dahil etme
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/13/2018
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 837231ca07abcfdbd6ce932bb24bd890d91506a8
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "69541562"
---
Şimdi bir grafik veritabanı oluşturmak için Azure portalında Veri Gezgini aracını kullanabilirsiniz. 

1. Yeni **Veri Gezgini**  >  **grafik**' i seçin.

    **Grafik Ekle** alanı en sağda görüntülenir, görmek için sağa kaydırmanız gerekebilir.

    ![Azure portalındaki Veri Gezgini, Grafik Ekle sayfası](./media/cosmos-db-create-graph/azure-cosmosdb-data-explorer-graph.png)

2. **Grafik Ekle** sayfasında, yeni grafik için ayarları girin.

    Ayar|Önerilen değer|Açıklama
    ---|---|---
    Veritabanı Kimliği|sample-database|Yeni veritabanınızın adı olarak *sample-database* girin. Veritabanı adı 1 ile 255 karakter arasında olmalı, `/ \ # ?` içermemeli ve boşlukla bitmemelidir.
    Aktarım hızı|400 RU|Aktarım hızını saniyede 400 istek birimi (RU/s) olarak değiştirin. Daha sonra gecikme süresini azaltmak isterseniz işlem hızının ölçeğini artırabilirsiniz.
    Graf Kimliği|sample-graph|Yeni koleksiyonunuzun adı olarak *sample-graph* girin. Graf adı karakter gereksinimleri, veritabanı kimliklerine ilişkin karakter gereksinimleri ile aynıdır.
    Bölüm Anahtarı| /PK |Tüm Cosmos DB hesapları yatay olarak ölçeklendirmek için bir bölüm anahtarına ihtiyaç duyar. [Grafik veri bölümlendirme makalesinde](https://docs.microsoft.com/azure/cosmos-db/graph-partitioning)uygun bölüm anahtarını seçme hakkında bilgi edinin.

3. Form doldurulduktan sonra **Tamam**' ı seçin.
