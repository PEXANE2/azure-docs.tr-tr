---
title: Machine Learning Services için önemli farklılıklar (Önizleme)
description: Bu konuda, Azure SQL yönetilen örneği ve SQL Server Machine Learning Services arasındaki temel farklılıklar açıklanmaktadır Machine Learning Services.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: garyericson
ms.author: garye
ms.reviewer: carlrab, davidph
manager: cgronlun
ms.date: 05/27/2020
ms.openlocfilehash: 02211a09df2d2744119ea8fbe6f0ecaa63dba020
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84045112"
---
# <a name="key-differences-between-machine-learning-services-in-azure-sql-managed-instance-and-sql-server"></a>Azure SQL yönetilen örneği ve SQL Server Machine Learning Services arasındaki önemli farklılıklar

[Azure SQL yönetilen örneği (Önizleme) içindeki Machine Learning Services](machine-learning-services-overview.md) işlevselliği, [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning)ile neredeyse aynıdır. Aşağıda bazı önemli farklılıklar verilmiştir.

> [!IMPORTANT]
> Azure SQL yönetilen örneği 'nde Machine Learning Services Şu anda genel önizleme aşamasındadır. Kaydolmak için bkz. [önizlemeye kaydolma](machine-learning-services-overview.md#signup).

## <a name="preview-limitations"></a>Önizleme sınırlamaları

Önizleme sırasında hizmet aşağıdaki sınırlamalara sahiptir:

- Geri döngü bağlantıları çalışmıyor ( [Python veya R betiğindeki SQL Server geri döngü bağlantısına](/sql/machine-learning/connect/loopback-connection)bakın).
- Dış kaynak havuzları desteklenmez.
- Yalnızca Python ve R desteklenir. Java gibi dış diller eklenemez.
- [Ileti geçirme arabirimini](https://docs.microsoft.com/message-passing-interface/microsoft-mpi) (MPI) kullanan senaryolar desteklenmez.

Hizmet düzeyi hedefi (SLO) güncelleştirmesi söz konusu olduğunda, lütfen SLO 'yu güncelleştirin ve R/Python için ayrılmış kaynak sınırlarını yeniden etkinleştirmek üzere bir destek bileti yükseltin.

## <a name="language-support"></a>Dil desteği

SQL yönetilen örneği 'nde Machine Learning Services ve hem Python hem de R [genişletilebilirlik çerçevesini](https://docs.microsoft.com/sql/advanced-analytics/concepts/extensibility-framework)destekler SQL Server. Temel farklılıklar şunlardır:

- Python ve R 'nin ilk sürümleri, SQL yönetilen örneği ve SQL Server Machine Learning Services arasında farklıdır:

  |                      | Python | R     |
  |----------------------|--------|-------|
  | SQL Yönetilen Örnek | 3.7.1  | 3.5.2 |
  | SQL Server           | 3.5.2  | 3.3.3 |

- Aracılığıyla yapılandırmaya gerek yoktur `external scripts enabled` `sp_configure` . Önizlemeye [kaydolduktan](machine-learning-services-overview.md#signup) sonra, SQL veritabanınız için makine öğrenimi etkinleştirilmiştir.

## <a name="packages"></a>Paketler

Python ve R paketi yönetimi, SQL yönetilen örneği ve SQL Server arasında farklı şekilde çalışır. Bu farklılıklar şunlardır:

- Paketler giden ağ çağrıları gerçekleştiremez. Bu sınırlama, SQL Server [Machine Learning Services için varsayılan güvenlik duvarı kurallarına](https://docs.microsoft.com//sql/advanced-analytics/security/firewall-configuration) benzerdir, ancak SQL yönetilen örneği 'nde değiştirilemez.
- Dış çalışma zamanları (Java gibi) bağlı olan veya yükleme ya da kullanım için işletim sistemi API 'Lerine erişime ihtiyacı olan paketler için destek yoktur.

Python ve R paketlerinin yönetilmesi hakkında daha fazla bilgi için bkz.:

- [Python paket bilgilerini al](https://docs.microsoft.com/sql/machine-learning/package-management/python-package-information?context=azure/sql-database/context/ml-context&view=sql-server-ver15)
- [R paketi bilgilerini al](https://docs.microsoft.com/sql/machine-learning/package-management/r-package-information?context=azure/sql-database/context/ml-context&view=sql-server-ver15)

## <a name="resource-governance"></a>Kaynak idaresi

R kaynaklarını [Resource Governor](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) ve dış kaynak havuzları aracılığıyla kısıtlamak mümkün değildir.

Genel Önizleme sırasında, R kaynakları SQL yönetilen örnek kaynaklarının en fazla %20 ' si olarak ayarlanır ve seçtiğiniz hizmet katmanına bağlıdır. Daha fazla bilgi için bkz. [Azure SQL veritabanı satın alma modelleri](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers).

### <a name="insufficient-memory-error"></a>Yetersiz bellek hatası

R için yeterli kullanılabilir bellek yoksa bir hata iletisi alırsınız. Yaygın hata iletileri:

- İstek kimliği: * * * * * * * için ' R ' betiği için çalışma zamanıyla iletişim kurulamıyor. Lütfen ' R ' çalışma zamanının gereksinimlerini denetleyin
- HRESULT 0x80004004 ile ' sp_execute_external_script ' yürütülmesi sırasında ' R ' betik hatası oluştu. ... dış betik hatası oluştu: ".. ' R_AllocStringBuffer ' "C işlevinde bellek (0 MB) ayrılamadı
- Dış betik hatası oluştu: hata: boyutun vektörü ayrılamıyor.

Bellek kullanımı, R betiklerinizde ne kadar kullanıldığına ve yürütülen paralel sorguların sayısına bağlıdır. Yukarıdaki hataları alıyorsanız, bu sorunu çözmek için veritabanınızı daha yüksek bir hizmet katmanına ölçeklendirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. genel bakış, [Azure SQL yönetilen örneği Machine Learning Services](machine-learning-services-overview.md).
- Machine Learning Services 'de Python kullanmayı öğrenmek için bkz. [Python betiklerini çalıştırma](https://docs.microsoft.com/sql/machine-learning/tutorials/quickstart-python-create-script?context=/azure/azure-sql/managed-instance/context/ml-context&view=sql-server-ver15).
- Machine Learning Services 'de R 'yi nasıl kullanacağınızı öğrenmek için bkz. [Run r Scripts](https://docs.microsoft.com/sql/machine-learning/tutorials/quickstart-r-create-script?context=/azure/azure-sql/managed-instance/context/ml-context&view=sql-server-ver15).
