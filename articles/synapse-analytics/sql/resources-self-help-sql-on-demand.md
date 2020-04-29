---
title: SQL isteğe bağlı Önizleme) kendi kendine yardım
description: Bu bölüm, isteğe bağlı SQL (Önizleme) ile ilgili sorunları gidermenize yardımcı olabilecek bilgiler içerir.
services: synapse analytics
author: vvasic-msft
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: vvasic
ms.reviewer: jrasnick
ms.openlocfilehash: e2c262915c928cf487cb84aeb3423d67e7a96e97
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81424835"
---
# <a name="self-help-for-sql-on-demand-preview"></a>İsteğe bağlı SQL için kendi kendine yardım (Önizleme)

Bu makale, Azure SYNAPSE Analytics 'te SQL isteğe bağlı (Önizleme) ile ilgili sık karşılaşılan sorunların nasıl giderileceği hakkında bilgiler içerir.

## <a name="sql-on-demand-is-grayed-out-in-synapse-studio"></a>SYNAPSE Studio 'da isteğe bağlı SQL gri

SYNAPSE Studio isteğe bağlı SQL bağlantısı kuramazsa, isteğe bağlı SQL 'nin gri olduğunu veya "çevrimdışı" durumunu gösterdiğini fark edeceksiniz. Genellikle, bu sorun aşağıdaki durumlardan biri gerçekleştiğinde oluşur:

1) Ağınız Azure SYNAPSE arka ucu iletişimini engelliyor. En sık karşılaşılan durum 1443 numaralı bağlantı noktasıdır. SQL isteğe bağlı olarak, bu bağlantı noktasının engelini kaldırın. Daha fazla bilgi edinmek için, diğer sorunlar hakkında SQL isteğe bağlı olarak [sorun giderme kılavuzunu ziyaret edebilirsiniz](../troubleshoot/troubleshoot-synapse-studio.md).
2) İsteğe bağlı SQL 'de oturum açma izniniz yok. Erişim kazanmak için, Azure SYNAPSE çalışma alanı yöneticilerinin sizi çalışma alanı yöneticisine veya SQL yöneticisi rolüne eklemesi gerekir. [Daha fazla bilgi için Access Control 'ta tam kılavuzu ziyaret edin](access-control.md).

## <a name="query-fails-because-file-cannot-be-opened"></a>Dosya açılamadığından sorgu başarısız oluyor

Sorgunuz mevcut olmadığı veya başka bir işlem tarafından kullanıldığı için ' dosya açılamıyor ' hatası ile başarısız olursa ve her iki dosyanın da mevcut olduğundan ve bu dosya başka bir işlem tarafından kullanılmadığından, SQL isteğe bağlı olarak dosyaya erişemez demektir. Bu sorun genellikle Azure Active Directory kimliğiniz dosyaya erişim haklarına sahip olmadığı için oluşur. Varsayılan olarak, SQL isteğe bağlı, Azure Active Directory kimliğinizi kullanarak dosyaya erişmeye çalışıyor. Bu sorunu çözmek için, dosyaya erişmek için uygun haklara sahip olmanız gerekir. En kolay yol, sorgulama yapmaya çalıştığınız depolama hesabında kendinize ait ' Depolama Blobu veri katılımcısı ' rolünü vermaktır. [Daha fazla bilgi için Azure Active Directory Access Control hakkında tam kılavuzu ziyaret edin](../../storage/common/storage-auth-aad-rbac-portal.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json). 

## <a name="query-fails-because-it-cannot-be-executed-due-to-current-resource-constraints"></a>Geçerli kaynak kısıtlamaları nedeniyle yürütülemediğinden sorgu başarısız oldu 

Sorgunuz hata iletisiyle başarısız olursa, ' Bu sorgu geçerli kaynak kısıtlamaları nedeniyle yürütülemiyor ', kaynak kısıtlamaları nedeniyle SQL OD 'nin şu anda yürütemediği anlamına gelir: 

- Lütfen makul boyutlarda veri türlerinin kullanıldığından emin olun. Ayrıca, varsayılan olarak VARCHAR (8000) olacak şekilde dize sütunları için Parquet dosyaları için şema belirtin. 

- Sorgunuz CSV dosyalarını hedefliyorsa, [istatistik oluşturmayı](develop-tables-statistics.md#statistics-in-sql-on-demand-preview)düşünün. 

- Sorguyu iyileştirmek için [isteğe bağlı SQL için en iyi performans uygulamalarını](best-practices-sql-on-demand.md) ziyaret edin.  

## <a name="next-steps"></a>Sonraki adımlar

İsteğe bağlı SQL kullanımı hakkında daha fazla bilgi edinmek için aşağıdaki makaleleri gözden geçirin:

- [Tek CSV dosyasını sorgula](query-single-csv-file.md)

- [Sorgu klasörleri ve birden çok CSV dosyası](query-folders-multiple-csv-files.md)

- [Belirli dosyaları sorgula](query-specific-files.md)

- [Parquet dosyalarını sorgulama](query-parquet-files.md)

- [Parquet iç içe türlerini sorgulama](query-parquet-nested-types.md)

- [JSON dosyalarını sorgulama](query-json-files.md)

- [Görünümleri oluşturma ve kullanma](create-use-views.md)

- [Dış tablolar oluşturma ve kullanma](create-use-external-tables.md)

- [Sorgu sonuçlarını depolama alanında saklama](create-external-table-as-select.md)
