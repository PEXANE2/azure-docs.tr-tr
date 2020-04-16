---
title: Delta Gölü Nedir
description: Delta Gölü'ne genel bakış ve Azure Synapse Analytics'in bir parçası olarak nasıl çalıştığı
services: synapse-analytics
author: euangMS
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: euang
ms.reviewer: euang
ms.openlocfilehash: 52758eab645fa0bb89cb499a5c617df62c21279e
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81429206"
---
# <a name="what-is-delta-lake"></a>Delta Gölü nedir?

Azure Synapse Analytics, Linux Foundation Delta Lake ile uyumludur. Delta Lake Apache Spark ve büyük veri iş yükleri ACID (atomiklik, tutarlılık, izolasyon ve dayanıklılık) işlemleri getiren bir açık kaynak depolama katmanıdır.

## <a name="key-features"></a>Önemli özellikler

| Özellik | Açıklama |
| --- | --- |
| **ASİt İşlemlerİ** | Veri gölleri genellikle birden çok işlem ve boru hattı aracılığıyla doldurulur ve bazıları okumalarla eş zamanlı olarak veri yazar. Delta Gölü ve işlemlerin eklenmesinden önce, veri mühendisleri veri bütünlüğünü sağlamak için el ile hataya meyilli bir süreçten geçmek zorunda kaldılar. Delta Gölü veri göllerine tanıdık ASİt işlemlerini getiriyor. Bu serializability, izolasyon düzeyi güçlü düzeyde sağlar. [Delta Gölü'ne Dalma: İşlem Günlüğü'nü Açma](https://databricks.com/blog/2019/08/21/diving-into-delta-lake-unpacking-the-transaction-log.html)hakkında daha fazla bilgi edinin.|
| **Ölçeklenebilir Meta veri işleme** | Büyük verilerde, meta verilerin kendisi bile "büyük veri" olabilir. Delta Lake, tüm meta verilerini işlemek için Spark'ın dağıtılmış işlem gücünden yararlanarak meta verileri de veri gibi ele adatır. Sonuç olarak, Delta Gölü milyarlarca bölüm ve dosyaile petabayt ölçekli tabloları rahatlıkla işleyebilir. |
| **Zaman Yolculuğu (veri sürümü)** | Bir değişikliği "geri alma" veya önceki sürüme geri dönme yeteneği, işlemlerin temel özelliklerinden biridir. Delta Lake, denetimler, geri almalar veya denemeleri yeniden oluşturmak için verilerin önceki sürümlerine geri dönmenizi sağlayan verilerin anlık görüntülerini sağlar. [Büyük Ölçekli Veri Gölleri için Delta Lake Time Travel tanıtımı](https://databricks.com/blog/2019/02/04/introducing-delta-time-travel-for-large-scale-data-lakes.html)hakkında daha fazla bilgi edinin. |
| **Açık Biçim** | Apache Parke, Delta Lake'in temel biçimidir ve biçime özgü verimli sıkıştırma ve kodlama şemalarından yararlanmanızı sağlar. |
| **Birleşik Toplu İşlem ve Akış Kaynağı ve Lavabo** | Delta Gölü'ndeki bir tablo hem toplu işlem tablosu, hem de akış kaynağı ve lavabodur. Akış veri yutma, toplu tarihi geri doldurma ve etkileşimli sorgular tüm kutusunda çalışmak. |
| **Şema Uygulama** | Şema zorlama, veri türlerinin doğru olduğundan ve gerekli sütunların bulunmasını sağlamaya yardımcı olarak hatalı verilerin veri tutarsızlığına neden olmasını önler. Daha fazla bilgi için, [Delta Gölü into Diving: Schema Enforcement & Evolution](https://databricks.com/blog/2019/09/24/diving-into-delta-lake-schema-enforcement-evolution.html) |
| **Şema Evrimi** | Delta Gölü, geçiş DDL yazmak zorunda kalmadan otomatik olarak uygulanabilen bir tablo şemasında değişiklik yapmanızı sağlar. Daha fazla bilgi için, [Delta Gölü into Diving: Schema Enforcement & Evolution](https://databricks.com/blog/2019/09/24/diving-into-delta-lake-schema-enforcement-evolution.html) |
| **Denetim Geçmişi** | Delta Lake işlem günlüğü, değişikliklerin tam denetim izini sağlayan verilerde yapılan her değişiklikle ilgili ayrıntıları kaydeder. |
| **Güncellemeler ve Silmeler** | Delta Lake çeşitli işlevler için Scala / Java / Python ve SQL API'lerini destekler. Birleştirme, güncelleştirme ve silme işlemleri için destek, uyumluluk gereksinimlerini karşılamanıza yardımcı olur. Daha fazla bilgi için, Birleştirme, güncelleştirme ve DML komutlarını silmek için kod parçacıkları içeren [Python API'lerini kullanarak Delta Lake Tables'ta](https://databricks.com/blog/2019/10/03/simple-reliable-upserts-and-deletes-on-delta-lake-tables-using-python-apis.html)Delta Lake Release ve Simple, Reliable Ups and Deletes'i [duyurun.](https://delta.io/news/delta-lake-0-4-0-released/) |
| **%100 Apache Spark API ile uyumlu** | Geliştiriciler, mevcut Spark uygulamalarıyla tam olarak uyumlu olduğu için Delta Lake'i mevcut veri boru hatlarıyla en az değişiklikle kullanabilirler. |

Tam dokümantasyon için [Delta Gölü Dokümantasyon Sayfasına](https://docs.delta.io/latest/delta-intro.html) bakın

Daha fazla bilgi için [Delta Lake Project'e](https://lfprojects.org)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [.NET Apache Spark belgeleri için](/dotnet/spark?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics)
