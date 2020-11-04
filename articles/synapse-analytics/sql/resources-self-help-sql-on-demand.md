---
title: Sunucusuz SQL Havuzu (Önizleme) kendi kendine yardım
description: Bu bölüm sunucusuz SQL Havuzu (Önizleme) ile ilgili sorunları gidermenize yardımcı olabilecek bilgiler içerir.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql
ms.date: 05/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: 9753fc491cb5950d679ae3633a18cdd5c1170291
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93317301"
---
# <a name="self-help-for-serverless-sql-pool-preview"></a>Sunucusuz SQL havuzu için kendi kendine yardım (Önizleme)

Bu makale, Azure SYNAPSE Analytics 'te sunucusuz SQL Havuzu (Önizleme) ile ilgili sık karşılaşılan sorunların nasıl giderileceği hakkında bilgiler içerir.

## <a name="serverless-sql-pool-is-grayed-out-in-synapse-studio"></a>SYNAPSE Studio 'da sunucusuz SQL havuzu gri renkte

SYNAPSE Studio sunucusuz SQL havuzuyla bağlantı kuramazsa, sunucusuz SQL havuzunun gri olduğunu veya "çevrimdışı" durumunu gösterdiğini fark edeceksiniz. Genellikle, bu sorun aşağıdaki durumlardan biri gerçekleştiğinde oluşur:

1) Ağınız Azure SYNAPSE arka ucu iletişimini engelliyor. En sık karşılaşılan durum 1443 numaralı bağlantı noktasıdır. Sunucusuz SQL havuzunun çalışmasını sağlamak için bu bağlantı noktasının engelini kaldırın. Diğer sorunlar sunucusuz SQL havuzunun de çalışmasını engelleyebilir, [daha fazla bilgi için tam sorun giderme kılavuzu ' nu ziyaret edin](../troubleshoot/troubleshoot-synapse-studio.md).
2) Sunucusuz SQL havuzunda oturum açma izniniz yok. Erişim kazanmak için, Azure SYNAPSE çalışma alanı yöneticilerinin sizi çalışma alanı yöneticisine veya SQL yöneticisi rolüne eklemesi gerekir. [Daha fazla bilgi için Access Control 'ta tam kılavuzu ziyaret edin](access-control.md).

## <a name="query-fails-because-file-cannot-be-opened"></a>Dosya açılamadığından sorgu başarısız oluyor

Sorgunuz mevcut olmadığı veya başka bir işlem tarafından kullanıldığı için ' dosya açılamıyor ' hatası ile başarısız olursa ve her iki dosyanın de bulunduğundan ve başka bir işlem tarafından kullanılmadığından, sunucusuz SQL havuzunun dosyaya erişemeyeceği anlamına gelir. Bu sorun genellikle Azure Active Directory kimliğiniz dosyaya erişim haklarına sahip olmadığı için oluşur. Varsayılan olarak, sunucusuz SQL havuzu Azure Active Directory kimliğinizi kullanarak dosyaya erişmeye çalışıyor. Bu sorunu çözmek için, dosyaya erişmek için uygun haklara sahip olmanız gerekir. En kolay yol sorgulamaya çalıştığınız depolama hesabı üzerinde kendinize 'Depolama Blob Verilerine Katkıda Bulunan' rolü vermektir. [Depolama için Azure Active Directory erişim denetimiyle ilgili tam kılavuzu ziyaret ederek daha fazla bilgi edinebilirsiniz](../../storage/common/storage-auth-aad-rbac-portal.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json). 

## <a name="query-fails-because-it-cannot-be-executed-due-to-current-resource-constraints"></a>Geçerli kaynak kısıtlamaları nedeniyle yürütülemediğinden sorgu başarısız oldu 

Sorgunuz hata iletisiyle başarısız olursa, ' Bu sorgu geçerli kaynak kısıtlamaları nedeniyle yürütülemiyor ', kaynak kısıtlamaları nedeniyle sunucusuz SQL havuzunun bunu şu anda yürütemeyeceği anlamına gelir: 

- Makul boyutlarda veri türlerinin kullanıldığından emin olun. Ayrıca, varsayılan olarak VARCHAR (8000) olacak şekilde dize sütunları için Parquet dosyaları için şema belirtin. 

- Sorgunuz CSV dosyalarını hedefliyorsa, [istatistik oluşturmayı](develop-tables-statistics.md#statistics-in-serverless-sql-pool-preview)düşünün. 

- Sorguyu iyileştirmek için [sunucusuz SQL havuzu için en iyi performans uygulamalarını](best-practices-sql-on-demand.md) ziyaret edin.  

## <a name="create-statement-is-not-supported-in-master-database"></a>Ana veritabanında ' DEYIM' oluşturma desteklenmiyor

Sorgunuz hata iletisiyle başarısız olursa:

> ' Sorgu yürütülemedi. Hata: dış tablo/VERI kaynağı/VERITABANı KAPSAMLı KIMLIK BILGILERI/dosya BIÇIMI oluşturma ana veritabanında desteklenmiyor. ' 

Bu, sunucusuz SQL havuzundaki ana veritabanının oluşturmayı desteklemediği anlamına gelir:
  - Dış tablolar
  - Dış veri kaynakları
  - Veritabanı kapsamlı kimlik bilgileri
  - Dış dosya biçimleri

Çözüm:

  1. Bir kullanıcı veritabanı oluşturun:

```sql
CREATE DATABASE <DATABASE_NAME>
```

  2. Create ifadesini, daha önce ana veritabanı için başarısız olan <DATABASE_NAME> bağlamında yürütün. 
  
  Dış dosya biçimi oluşturma örneği:
    
```sql
USE <DATABASE_NAME>
CREATE EXTERNAL FILE FORMAT [SynapseParquetFormat] 
WITH ( FORMAT_TYPE = PARQUET)
```

## <a name="next-steps"></a>Sonraki adımlar

Sunucusuz SQL havuzu kullanma hakkında daha fazla bilgi edinmek için aşağıdaki makaleleri gözden geçirin:

- [Tek CSV dosyasını sorgula](query-single-csv-file.md)

- [Sorgu klasörleri ve birden çok CSV dosyası](query-folders-multiple-csv-files.md)

- [Belirli dosyaları sorgula](query-specific-files.md)

- [Parquet dosyalarını sorgulama](query-parquet-files.md)

- [Parquet iç içe türlerini sorgulama](query-parquet-nested-types.md)

- [JSON dosyalarını sorgulama](query-json-files.md)

- [Görünümleri oluşturma ve kullanma](create-use-views.md)

- [Dış tablolar oluşturma ve kullanma](create-use-external-tables.md)

- [Sorgu sonuçlarını depolama alanında saklama](create-external-table-as-select.md)
