---
title: Machine Learning Services için önemli farklılıklar (Önizleme)
description: Bu konuda, Azure SQL veritabanı Machine Learning Services (R ile) ve SQL Server Machine Learning Services arasındaki temel farklılıklar açıklanmaktadır.
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
ms.openlocfilehash: 533e2b9e50a92cce1419da521d8cebc4955e4df6
ms.sourcegitcommit: 95931aa19a9a2f208dedc9733b22c4cdff38addc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74462120"
---
# <a name="key-differences-between-machine-learning-services-in-azure-sql-database-preview-and-sql-server"></a>Azure SQL veritabanı 'nda (Önizleme) Machine Learning Services ve SQL Server arasındaki önemli farklılıklar

(Önizleme) içinde (R ile) Azure SQL veritabanı Machine Learning Services işlevleri, [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning)benzerdir. Aşağıda bazı önemli farklılıklar verilmiştir.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="language-support"></a>Dil desteği

SQL Server [genişletilebilirlik çerçevesi](https://docs.microsoft.com/sql/advanced-analytics/concepts/extensibility-framework)aracılığıyla R ve Python desteğine sahiptir. SQL veritabanı her iki dili de desteklemez. Temel farklılıklar şunlardır:

- R, SQL veritabanı 'nda desteklenen tek dildir. Python desteği yoktur.
- R sürümü 3.4.4 ' dir.
- `external scripts enabled` `sp_configure`aracılığıyla yapılandırmaya gerek yoktur. [Kaydolduktan](sql-database-machine-learning-services-overview.md#signup)sonra, SQL veritabanınız için makine öğrenimi etkinleştirilmiştir.

## <a name="package-management"></a>Paket yönetimi

R paketi yönetimi ve yükleme SQL veritabanı ile SQL Server arasında farklılık fark. Bu farklılıklar şunlardır:

- R paketleri [sqlmlutils](https://github.com/Microsoft/sqlmlutils) aracılığıyla YÜKLENIR veya [dış kitaplık oluşturur](https://docs.microsoft.com/sql/t-sql/statements/create-external-library-transact-sql).
- Paketler giden ağ çağrıları gerçekleştiremez. Bu sınırlama, SQL Server [Machine Learning Services için varsayılan güvenlik duvarı kurallarına](https://docs.microsoft.com//sql/advanced-analytics/security/firewall-configuration) benzerdir, ancak SQL veritabanı 'nda değiştirilemez.
- Dış çalışma zamanları (Java gibi) bağlı olan veya yükleme ya da kullanım için işletim sistemi API 'Lerine erişime ihtiyacı olan paketler için destek yoktur.

## <a name="writing-to-a-temporary-table"></a>Geçici bir tabloya yazma

Azure SQL veritabanı 'nda RODBC kullanıyorsanız, `sp_execute_external_script` oturumunun içinde mi yoksa dışında mı oluşturulduğuna bakılmaksızın geçici bir tabloya yazamıyoruz. Geçici çözüm, `sp_execute_external_script` sorgusundan önce oluşturulan küresel geçici bir tabloya yazmak için [Rxodbcdata](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxodbcdata) ve [Rxdatastep](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxdatastep) (overwrite = false ve Append = "Rows" ile birlikte) kullanmaktır.

## <a name="resource-governance"></a>Kaynak idaresi

R kaynaklarını [Resource Governor](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) ve dış kaynak havuzları aracılığıyla kısıtlamak mümkün değildir.

Genel Önizleme sırasında, R kaynakları SQL veritabanı kaynaklarının en fazla %20 ' si olarak ayarlanır ve seçtiğiniz hizmet katmanına bağlıdır. Daha fazla bilgi için bkz. [Azure SQL veritabanı satın alma modelleri](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers).
### <a name="insufficient-memory-error"></a>Yetersiz bellek hatası

R için yeterli kullanılabilir bellek yoksa bir hata iletisi alırsınız. Yaygın hata iletileri:

- İstek kimliği: * * * * * * * için ' R ' betiği için çalışma zamanıyla iletişim kurulamıyor. Lütfen ' R ' çalışma zamanının gereksinimlerini denetleyin
- HRESULT 0x80004004 ile ' sp_execute_external_script ' yürütülmesi sırasında ' R ' betik hatası oluştu. ... dış betik hatası oluştu: ".. ' R_AllocStringBuffer ' "C işlevinde bellek (0 MB) ayrılamadı
- Dış betik hatası oluştu: hata: boyutun vektörü ayrılamıyor.

Bellek kullanımı, R betiklerinizde ne kadar kullanıldığına ve yürütülen paralel sorguların sayısına bağlıdır. Yukarıdaki hataları alıyorsanız, bu sorunu çözmek için veritabanınızı daha yüksek bir hizmet katmanına ölçeklendirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. genel bakış, [Azure SQL veritabanı Machine Learning Services R (Önizleme)](sql-database-machine-learning-services-overview.md).
- R 'yi kullanarak Azure SQL veritabanı Machine Learning Services (Önizleme) sorgulama hakkında bilgi edinmek için [hızlı başlangıç kılavuzuna](sql-database-connect-query-r.md)bakın.
- Bazı basit R betiklerini kullanmaya başlamak için bkz. [Azure SQL veritabanı 'nda basit r betikleri oluşturma ve çalıştırma Machine Learning Services (Önizleme)](sql-database-quickstart-r-create-script.md).
