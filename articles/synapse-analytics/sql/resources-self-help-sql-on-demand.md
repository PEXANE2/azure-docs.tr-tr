---
title: SQL on-demand Preview) kendi kendine yardım
description: Bu bölümde, SQL isteğe bağlı (önizleme) ile ilgili sorunları gidermenize yardımcı olabilecek bilgiler bulunur.
services: synapse analytics
author: vvasic-msft
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: vvasic
ms.reviewer: jrasnick
ms.openlocfilehash: e2c262915c928cf487cb84aeb3423d67e7a96e97
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424835"
---
# <a name="self-help-for-sql-on-demand-preview"></a>İsteğe bağlı SQL için kendi kendine yardım (önizleme)

Bu makalede, Azure Synapse Analytics'te isteğe bağlı SQL (önizleme) ile ilgili en sık karşılaşılan sorunları nasıl gideriyleçözebilirsiniz hakkında bilgiler yer alıyor.

## <a name="sql-on-demand-is-grayed-out-in-synapse-studio"></a>SYN'de isteğe bağlı SQL gri renkte

Synapse Studio isteğe bağlı OLARAK SQL bağlantısı kuramazsa, SQL isteğe bağlı olarak gri renkte olduğunu veya durum "Çevrimdışı" olarak gösterir. Genellikle, aşağıdaki durumlardan biri gerçekleştiğinde bu sorun oluşur:

1) Ağınız Azure Synapse arka ucuna iletişimi engeller. En sık karşılaşılan durum, bağlantı noktası 1443'ün engellenmesidir. SQL on-demand bu bağlantı noktası engelini kaldırmak için çalışmak için. Diğer sorunlar da sql on-demand de çalışmasını engelleyebilir, [daha fazla bilgi için tam sorun giderme kılavuzunu ziyaret edin.](../troubleshoot/troubleshoot-synapse-studio.md)
2) İsteğe bağlı OLARAK SQL'e giriş yapma izniniz yok. Erişim kazanmak için Azure Synapse çalışma alanı yöneticilerinden birinin sizi çalışma alanı yöneticisine veya SQL yöneticisi rolüne eklemesi gerekir. [Daha fazla bilgi için erişim denetimi yle ilgili tam kılavuzu ziyaret edin.](access-control.md)

## <a name="query-fails-because-file-cannot-be-opened"></a>Dosya açılamadığı için sorgu başarısız oldu

Sorgunuz 'Dosya var olmadığı için açılamaz veya başka bir işlem tarafından kullanılıyor' hatasıyla başarısız olursa ve her iki dosyanın da bulunduğundan eminseniz ve başka bir işlem tarafından kullanılmazsa, bu SQL on-demand dosyaya erişemeyeceği anlamına gelir. Azure Etkin Dizin kimliğinizin dosyaya erişme hakları olmadığından bu sorun genellikle gerçekleşir. Varsayılan olarak, SQL isteğe bağlı olarak Azure Active Directory kimliğinizi kullanarak dosyaya erişmeye çalışır. Bu sorunu gidermek için dosyaya erişmek için uygun haklara sahip olmanız gerekir. En kolay yolu, sorgulamaya çalıştığınız depolama hesabında kendinize 'Depolama Blob Veri Katılımcısı' rolünü vermektir. [Daha fazla bilgi için depolama alanı için Azure Active Directory erişim denetiminin tam kılavuzunu ziyaret edin.](../../storage/common/storage-auth-aad-rbac-portal.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) 

## <a name="query-fails-because-it-cannot-be-executed-due-to-current-resource-constraints"></a>Geçerli kaynak kısıtlamaları nedeniyle yürütülemediği için sorgu başarısız oldu 

Sorgunuz 'Bu sorgu geçerli kaynak kısıtlamaları nedeniyle yürütülemez' hata iletisiyle başarısız olursa, SQL OD'nin kaynak kısıtlamaları nedeniyle şu anda bu sorguyu yürütemediği anlamına gelir: 

- Lütfen makul boyutlardaveri türlerinin kullanıldığından emin olun. Ayrıca, dize sütunları için Parke dosyaları için şema belirtin, çünkü varsayılan olarak VARCHAR(8000). 

- Sorgunuz CSV dosyalarını hedefliyorsa, [istatistik oluşturmayı](develop-tables-statistics.md#statistics-in-sql-on-demand-preview)düşünün. 

- Sorguyu optimize etmek [için SQL on-demand için performans en iyi uygulamalarını](best-practices-sql-on-demand.md) ziyaret edin.  

## <a name="next-steps"></a>Sonraki adımlar

Sql isteğe bağlı olarak nasıl kullanılacağı hakkında daha fazla bilgi edinmek için aşağıdaki makaleleri gözden geçirin:

- [Tek CSV dosyasorgulama](query-single-csv-file.md)

- [Sorgu klasörleri ve birden çok CSV dosyası](query-folders-multiple-csv-files.md)

- [Belirli dosyaları sorgula](query-specific-files.md)

- [Parke dosyalarını sorgula](query-parquet-files.md)

- [Sorgu Parke iç içe türleri](query-parquet-nested-types.md)

- [JSON dosyalarını sorgula](query-json-files.md)

- [Görünüm oluşturma ve kullanma](create-use-views.md)

- [Dış tablolar oluşturma ve kullanma](create-use-external-tables.md)

- [Sorgu sonuçlarını depolama alanına depolama](create-external-table-as-select.md)
