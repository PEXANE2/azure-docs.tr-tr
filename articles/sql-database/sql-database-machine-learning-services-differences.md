---
title: Makine Öğrenimi Hizmetleri için temel farklar (önizleme)
description: Bu konu, Azure SQL Veritabanı Makine Öğrenme Hizmetleri (R ile) ve SQL Server Machine Learning Services arasındaki temel farklılıkları açıklar.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: carlrab
manager: cgronlun
ms.date: 11/20/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 34ba75b6126024c9cd43d6fe474f7c1b62dd990f
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81453158"
---
# <a name="key-differences-between-machine-learning-services-in-azure-sql-database-preview-and-sql-server"></a>Azure SQL Veritabanı'ndaki Machine Learning Services (önizleme) ve SQL Server arasındaki temel farklar

Azure SQL Veritabanı Makine Öğrenme Hizmetleri'nin (R ile) işlevselliği (önizleme) [SQL Server Machine Learning Services'a](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning)benzer. Aşağıda bazı önemli farklılıklar vardır.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="language-support"></a>Dil desteği

SQL Server [genişletilebilirlik çerçevesi](https://docs.microsoft.com/sql/advanced-analytics/concepts/extensibility-framework)üzerinden R ve Python desteğine sahiptir. SQL Veritabanı her iki dili de desteklemez. Önemli farklar şunlardır:

- R, SQL Veritabanı'nda desteklenen tek dildir. Python desteği yoktur.
- R sürümü 3.4.4 olduğunu.
- `sp_configure`Üzerinden yapılandırmaya `external scripts enabled` gerek yoktur.

## <a name="package-management"></a>Paket yönetimi

R paket yönetimi ve yükleme çalışmaları SQL Veritabanı ve SQL Server arasında farklıdır. Bu farklılıklar şunlardır:

- R paketleri [sqlmlutils](https://github.com/Microsoft/sqlmlutils) veya [CREATE EXTERNAL LIBRARY](https://docs.microsoft.com/sql/t-sql/statements/create-external-library-transact-sql)ile yüklenir.
- Paketler giden ağ çağrılarını gerçekleştiremez. Bu sınırlama, SQL Server'daki [Machine Learning Services için varsayılan güvenlik duvarı kurallarına](https://docs.microsoft.com//sql/advanced-analytics/security/firewall-configuration) benzer, ancak SQL Veritabanı'nda değiştirilemez.
- Harici çalışma sürelerine (Java gibi) bağlı olan veya yükleme veya kullanım için OS API'lerine erişmeye ihtiyaç duyan paketler için destek yoktur.

## <a name="writing-to-a-temporary-table"></a>Geçici bir tabloya yazma

RODBC'yi Azure SQL Veritabanı'nda kullanıyorsanız, `sp_execute_external_script` oturum içinde veya dışında oluşturulan geçici bir tabloya yazamazsınız. Geçici çözüm, `sp_execute_external_script` sorgudan önce oluşturulan genel geçici tabloya yazmak için [RxOdbcData](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxodbcdata) ve [rxDataStep](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxdatastep) 'i (overwrite=FALSE ve append="rows") kullanmaktır.

## <a name="resource-governance"></a>Kaynak idaresi

[Kaynak Yöneticisi](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) ve dış kaynak havuzları aracılığıyla R kaynaklarını sınırlamak mümkün değildir.

Genel önizleme sırasında, R kaynakları SQL Veritabanı kaynaklarının en fazla %20'sine ayarlanır ve seçtiğiniz hizmet katmanına bağlıdır. Daha fazla bilgi için Azure [SQL Veritabanı satın alma modellerine](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers)bakın.
### <a name="insufficient-memory-error"></a>Yetersiz bellek hatası

R için yeterli bellek yoksa, bir hata iletisi alırsınız. Sık karşılaşılan hata iletileri şunlardır:

- İstek kimliği için 'R' komut dosyası için çalışma süresi ile iletişim kuramıyor: ******* Lütfen 'R' çalışma zamanı gereksinimlerini kontrol edin
- 'R' komut dosyası hatası HRESULT 0x80004004 ile 'sp_execute_external_script' yürütülmesi sırasında oluştu. ... harici bir komut dosyası hatası oluştu: "... C işlevinde bellek (0 Mb) tahsis edilemedi 'R_AllocStringBuffer'"
- Harici bir komut dosyası hatası oluştu: Hata: boyut vektörü tahsis edemez.

Bellek kullanımı, R komut dosyalarınızda ne kadar kullanıldığına ve yürütülen paralel sorgu sayısına bağlıdır. Yukarıdaki hataları alırsanız, bunu çözmek için veritabanınızı daha yüksek bir hizmet katmanına ölçeklendirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakışa bakın, [R ile Azure SQL Veritabanı Makine Öğrenme Hizmetleri (önizleme)](sql-database-machine-learning-services-overview.md).
- Azure SQL Veritabanı Makine Öğrenme Hizmetleri 'ni (önizleme) sorgulamak için R'yi nasıl kullanacağınızı öğrenmek için [Quickstart kılavuzuna](sql-database-connect-query-r.md)bakın.
- Bazı basit R komut dosyalarıyla başlamak için Azure [SQL Veritabanı Makine Öğrenme Hizmetleri'nde (önizleme) basit R komut dosyası oluşturma ve çalıştırma](sql-database-quickstart-r-create-script.md)'ya bakın.
