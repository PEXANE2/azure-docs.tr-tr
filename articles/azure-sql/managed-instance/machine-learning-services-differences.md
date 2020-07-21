---
title: Machine Learning Services için önemli farklılıklar (Önizleme)
description: Bu konuda, Azure SQL yönetilen örneği ve SQL Server Machine Learning Services arasındaki temel farklılıklar açıklanmaktadır Machine Learning Services.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: garyericson
ms.author: garye
ms.reviewer: carlrab, davidph
manager: cgronlun
ms.date: 05/27/2020
ms.openlocfilehash: f267b155fe21e5dee1a7c488c999fe194ed38eb1
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86504127"
---
# <a name="key-differences-between-machine-learning-services-in-azure-sql-managed-instance-and-sql-server"></a>Azure SQL Yönetilen Örneği ve SQL Server içindeki Machine Learning Services özellikleri arasındaki temel farklar

[Azure SQL yönetilen örneği (Önizleme) içindeki Machine Learning Services](machine-learning-services-overview.md) işlevselliği, [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning)ile neredeyse aynıdır. Aşağıda bazı önemli farklılıklar verilmiştir.

> [!IMPORTANT]
> Azure SQL yönetilen örneği 'nde Machine Learning Services Şu anda genel önizleme aşamasındadır. Kaydolmak için bkz. [önizlemeye kaydolma](machine-learning-services-overview.md#signup).

## <a name="preview-limitations"></a>Önizleme sınırlamaları

Hizmet, önizleme aşamasında aşağıdaki sınırlamalara sahiptir:

- Geri döngü bağlantıları çalışmıyor ( [Python veya R betiğindeki SQL Server geri döngü bağlantısına](/sql/machine-learning/connect/loopback-connection)bakın).
- Dış kaynak havuzları desteklenmez.
- Yalnızca Python ve R desteklenir. Java gibi dış diller eklenemez.
- [Ileti geçirme arabirimini](https://docs.microsoft.com/message-passing-interface/microsoft-mpi) (MPI) kullanan senaryolar desteklenmez.

Hizmet Düzeyi Hedefi (SLO) güncelleştirmesi için lütfen SLO'yu güncelleştirin ve R/Python için ayrılmış kaynak sınırlarının yeniden etkinleştirilmesi amacıyla bir destek bileti oluşturun.

## <a name="language-support"></a>Dil desteği

SQL yönetilen örneği 'nde Machine Learning Services ve hem Python hem de R [genişletilebilirlik çerçevesini](https://docs.microsoft.com/sql/advanced-analytics/concepts/extensibility-framework)destekler SQL Server. Temel farklılıklar şunlardır:

- Python ve R 'nin ilk sürümleri, SQL yönetilen örneği ve SQL Server Machine Learning Services arasında farklıdır:

  | Sistem               | Python | R     |
  |----------------------|--------|-------|
  | SQL Yönetilen Örnek | 3.7.1  | 3.5.2 |
  | SQL Server           | 3.5.2  | 3.3.3 |

- Aracılığıyla yapılandırmaya gerek yoktur `external scripts enabled` `sp_configure` . Önizlemeye [kaydolduktan](machine-learning-services-overview.md#signup) sonra, makine öğrenimi Azure SQL yönetilen örneği için etkinleştirilir.

## <a name="packages"></a>Paketler

Python ve R paketi yönetimi, SQL yönetilen örneği ve SQL Server arasında farklı şekilde çalışır. Bu farklılıklar şunlardır:

- Paketler giden ağ çağrıları gerçekleştiremez. Bu sınırlama, SQL Server [Machine Learning Services için varsayılan güvenlik duvarı kurallarına](https://docs.microsoft.com//sql/advanced-analytics/security/firewall-configuration) benzerdir, ancak SQL yönetilen örneği 'nde değiştirilemez.
- Dış çalışma zamanları (Java gibi) bağlı olan veya yükleme ya da kullanım için işletim sistemi API 'Lerine erişime ihtiyacı olan paketler için destek yoktur.

Python ve R paketlerinin yönetilmesi hakkında daha fazla bilgi için bkz.:

- [Python paket bilgilerini alma](https://docs.microsoft.com/sql/machine-learning/package-management/python-package-information?context=azure/sql-database/context/ml-context&view=sql-server-ver15)
- [R paket bilgilerini alma](https://docs.microsoft.com/sql/machine-learning/package-management/r-package-information?context=azure/sql-database/context/ml-context&view=sql-server-ver15)

## <a name="resource-governance"></a>Kaynak idaresi

R kaynaklarını [Resource Governor](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) ve dış kaynak havuzları aracılığıyla kısıtlamak mümkün değildir.

Genel önizleme aşamasında R kaynakları SQL Yönetilen Örneği kaynaklarının en fazla %20'sine ulaşacak şekilde ayarlanır ve bu değer, seçtiğiniz hizmet katmanına göre değişir. Daha fazla bilgi için bkz. [Azure SQL veritabanı satın alma modelleri](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers).

### <a name="insufficient-memory-error"></a>Yetersiz bellek hatası

R için yeterli bellek yoksa bir hata iletisiyle karşılaşırsınız. Yaygın hata iletileri:

- İstek kimliği: * * * * * * * için ' R ' betiği için çalışma zamanıyla iletişim kurulamıyor. Lütfen "R" çalışma zamanının gereksinimlerini denetleyin
- "sp_execute_external_script" yürütülürken "R" betiği hatası oluştu, HRESULT 0x80004004. ...bir dış betik hatası oluştu: "..."R_AllocStringBuffer" C işlevinde bellek ayrılamadı (0 Mb)"
- Dış betik hatası oluştu: hata: boyutun vektörü ayrılamıyor.

Bellek kullanımı, R betiklerinizde kullanılan bellek miktarına ve yürütülen paralel sorgu sayısına göre değişir. Yukarıdaki hataları alıyorsanız, veritabanınızı daha yüksek bir hizmet katmanına ölçeklendirerek bu sorunu çözebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. genel bakış, [Azure SQL yönetilen örneği Machine Learning Services](machine-learning-services-overview.md).
- Machine Learning Services 'de Python kullanmayı öğrenmek için bkz. [Python betiklerini çalıştırma](https://docs.microsoft.com/sql/machine-learning/tutorials/quickstart-python-create-script?context=/azure/azure-sql/managed-instance/context/ml-context&view=sql-server-ver15).
- Machine Learning Services 'de R 'yi nasıl kullanacağınızı öğrenmek için bkz. [Run r Scripts](https://docs.microsoft.com/sql/machine-learning/tutorials/quickstart-r-create-script?context=/azure/azure-sql/managed-instance/context/ml-context&view=sql-server-ver15).
