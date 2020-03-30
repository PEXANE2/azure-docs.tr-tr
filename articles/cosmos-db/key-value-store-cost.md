---
title: Önemli bir değer deposu olarak Azure Cosmos DB için birim ücretleri isteme
description: Anahtar/değer deposu olarak kullanıldığında basit yazma ve okuma işlemleri için Azure Cosmos DB'nin istek birimi ücretleri hakkında bilgi edinin.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/23/2019
ms.author: sngun
ms.custom: seodec18
ms.openlocfilehash: 5b2ee8b5bf19f16d7f7f04e9515fe591db7132f1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77647517"
---
# <a name="azure-cosmos-db-as-a-key-value-store--cost-overview"></a>Önemli bir değer deposu olarak Azure Cosmos DB – maliyete genel bakış

Azure Cosmos DB, yüksek kullanılabilir, büyük ölçekli uygulamaları kolayca oluşturmak için küresel olarak dağıtılan, çok modelli bir veritabanı hizmetidir. Varsayılan olarak, Azure Cosmos DB sindirene tüm verileri otomatik olarak ve verimli bir şekilde dizine getirir. Bu, verilerüzerinde hızlı ve tutarlı [SQL](how-to-sql-query.md) (ve [JavaScript)](stored-procedures-triggers-udfs.md)sorguları sağlar. 

Bu makalede, basit yazma ve okuma işlemleri için Azure Cosmos DB'nin anahtar/değer deposu olarak kullanılmasının maliyeti açıklanmaktadır. Yazma işlemleri, veri öğelerinin eklemelerini, değiştirmelerini, silmesini ve yukarısını içerir. Azure Cosmos DB, tüm çok bölgeli hesaplar için %99,999 kullanılabilirlik SLA garantisinin yanı sıra, okumalar ve (indekslenmiş) yazmalar için 10 ms'lik <garantili bir gecikme alanı sunar. 

## <a name="why-we-use-request-units-rus"></a>İstek Birimlerini (RUs) neden kullanıyoruz?

Azure Cosmos DB performansı, [İstek Birimlerinde](request-units.md) (RU/s) ifade edilen sağlanan iş ortası miktarına dayanır. Sağlama ikinci bir tanecikli olup, RU/s cinsinden satın alınır[(saatlik faturalandırma ile karıştırılmamalıdır).](https://azure.microsoft.com/pricing/details/cosmos-db/) RUs, uygulama için gerekli iş tamlaması sağlamayı kolaylaştıran mantıksal bir soyutlama (para birimi) olarak düşünülmelidir. Kullanıcılar okuma ve yazma iş bölümü arasında ayrım yapmayı düşünmek zorunda değildir. RUs'un tek para birimi modeli, verilen kapasiteyi okuma ve yazma arasında paylaşmak için verimlilik oluşturur. Bu sağlanan kapasite modeli, hizmetin öngörülebilir ve tutarlı bir **iş artışı, garantili düşük gecikme sonu ve yüksek kullanılabilirlik**sağlamasına olanak tanır. Son olarak, RU modeli iş miktarını göstermek için kullanılırken, sağlanan her RU'nun da tanımlanmış miktarda kaynağı vardır (örn. bellek, çekirdek/CPU ve IOPS).

Küresel olarak dağıtılan bir veritabanı sistemi olarak Cosmos DB, gecikme, iş ortası, tutarlılık ve yüksek kullanılabilirliği kapsayan kapsamlı SLA'lar sağlayan tek Azure hizmetidir. Sağlama yaptığınız iş, Cosmos hesabınızla ilişkili bölgelerin her birine uygulanır. Okumalar için Cosmos DB, aralarından seçim yapabileceğiniz birden çok, iyi tanımlanmış [tutarlılık düzeyi](consistency-levels.md) sunar. 

Aşağıdaki tabloda, varsayılan otomatik dizin oluşturma kapalı olan boyut 1 KB ve 100 KBs veri öğesine dayalı okuma ve yazma işlemleri gerçekleştirmek için gereken RUs sayısı gösterilmektedir. 

|Öğe Boyutu|1 Okundu|1 Yaz|
|-------------|------|-------|
|1 KB|1 RU|5 RUs|
|100 KB|10 RU|50 Rus|

## <a name="cost-of-reads-and-writes"></a>Okuma ve yazma maliyeti

1.000 RU/s sağlarsanız, bu tutar 3,6 milyon RU/saat'tir ve saat için (ABD ve Avrupa'da) $0,08'e mal olur. 1 KB boyutunda bir veri öğesi için, bu, sağlanan iş lerinizi kullanarak 3,6 milyon okuma veya 0,72 milyon yazma (3,6 milyon RU / 5) tüketebileceğiniz anlamına gelir. Normalleştirilmiş milyon okur ve yazar, maliyet 0,022 $ / milyon okur ($ 0,08 / 3,6) ve $ 0,111/million yazıyor ($ 0,08 / 0,72) olacaktır. Aşağıdaki tabloda gösterildiği gibi milyon başına maliyet en az olur.

|Öğe Boyutu|Maliyeti 1 milyon okuma|Maliyeti 1 milyon yazıyor|
|-------------|-------|--------|
|1 KB|0,022 $|0,111 TL|
|100 KB|0,222 $|1.111 DOLAR|


Temel blob veya nesne mağazaları hizmetlerinin çoğu 0,40 $ milyon okuma işlem başına ve 5 milyon $ yazma işlemi başına ücret. En iyi şekilde kullanılırsa, Cosmos DB diğer çözümlerden (1 KB işlemler için) %98'e kadar daha ucuz olabilir.

## <a name="next-steps"></a>Sonraki adımlar

* İş yükleriniz için iş lerinizi tahmin etmek için [RU hesap makinesini](https://cosmos.azure.com/capacitycalculator/) kullanın.

