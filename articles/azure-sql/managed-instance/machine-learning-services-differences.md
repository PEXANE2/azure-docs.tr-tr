---
title: Machine Learning Services için önemli farklılıklar (Önizleme)
description: Bu makalede, Azure SQL yönetilen örneği ve SQL Server Machine Learning Services arasındaki temel farklılıklar açıklanmaktadır Machine Learning Services.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: garyericson
ms.author: garye
ms.reviewer: sstein, davidph
manager: cgronlun
ms.date: 10/26/2020
ms.openlocfilehash: adf454ac697f8cabf4256ebfc5baa5d0d1c76264
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/28/2020
ms.locfileid: "92782476"
---
# <a name="key-differences-between-machine-learning-services-in-azure-sql-managed-instance-and-sql-server"></a>Azure SQL Yönetilen Örneği ve SQL Server içindeki Machine Learning Services özellikleri arasındaki temel farklar

[Azure SQL yönetilen örneği (Önizleme) içindeki Machine Learning Services](machine-learning-services-overview.md) işlevselliği, [SQL Server Machine Learning Services](/sql/advanced-analytics/what-is-sql-server-machine-learning)ile neredeyse aynıdır. Aşağıda bazı önemli farklılıklar verilmiştir.

> [!IMPORTANT]
> Azure SQL yönetilen örneği 'nde Machine Learning Services Şu anda genel önizleme aşamasındadır. Kaydolmak için bkz. [önizlemeye kaydolma](machine-learning-services-overview.md#signup).

## <a name="preview-limitations"></a>Önizleme sınırlamaları

Hizmet, önizleme aşamasında aşağıdaki sınırlamalara sahiptir:

- Geri döngü bağlantıları çalışmıyor ( [Python veya R betiğindeki SQL Server geri döngü bağlantısına](/sql/machine-learning/connect/loopback-connection)bakın).
- Dış kaynak havuzları desteklenmez.
- Yalnızca Python ve R desteklenir. Java gibi dış diller eklenemez.
- [Ileti geçirme arabirimini](/message-passing-interface/microsoft-mpi) (MPI) kullanan senaryolar desteklenmez.

Hizmet düzeyi hedefi (SLO) güncelleştirmesi söz konusu olduğunda, SLO 'yu güncelleştirin ve R/Python için ayrılmış kaynak sınırlarını yeniden etkinleştirmek üzere bir destek bileti yükseltin.

## <a name="language-support"></a>Dil desteği

SQL yönetilen örneği 'nde Machine Learning Services ve hem Python hem de R [genişletilebilirlik çerçevesini](/sql/advanced-analytics/concepts/extensibility-framework)destekler SQL Server. Temel farklılıklar şunlardır:

- Python ve R 'nin ilk sürümleri, SQL yönetilen örneği ve SQL Server Machine Learning Services arasında farklıdır:

  | Sistem               | Python | R     |
  |----------------------|--------|-------|
  | SQL Yönetilen Örnek | 3.7.1  | 3.5.2 |
  | SQL Server           | 3.5.2  | 3.3.3 |

- Aracılığıyla yapılandırmaya gerek yoktur `external scripts enabled` `sp_configure` . Önizlemeye [kaydolduktan](machine-learning-services-overview.md#signup) sonra, makine öğrenimi Azure SQL yönetilen örneği için etkinleştirilir.

## <a name="packages"></a>Paketler

Python ve R paketi yönetimi, SQL yönetilen örneği ve SQL Server arasında farklı şekilde çalışır. Bu farklılıklar şunlardır:

- Dış çalışma zamanları (Java gibi) bağlı olan veya yükleme ya da kullanım için işletim sistemi API 'Lerine erişime ihtiyacı olan paketler için destek yoktur.
- Paketler, giden ağ çağrılarını gerçekleştirebilir (önizlemede daha önce değişiklik). Giden ağ çağrılarını etkinleştirmek için [ağ güvenlik grubu](/azure/virtual-network/network-security-groups-overview) düzeyinde doğru giden güvenlik kurallarını ayarlayabilirsiniz.

Python ve R paketlerinin yönetilmesi hakkında daha fazla bilgi için bkz.:

- [Python paket bilgilerini alma](/sql/machine-learning/package-management/python-package-information?context=/azure/azure-sql/managed-instance/context/ml-context&view=azuresqldb-mi-current&preserve-view=true)
- [R paket bilgilerini alma](/sql/machine-learning/package-management/r-package-information?context=/azure/azure-sql/managed-instance/context/ml-context&view=azuresqldb-mi-current&preserve-view=true)

## <a name="resource-governance"></a>Kaynak idaresi

R kaynaklarını [Resource Governor](/sql/relational-databases/resource-governor/resource-governor) ve dış kaynak havuzları aracılığıyla kısıtlamak mümkün değildir.

Genel önizleme aşamasında R kaynakları SQL Yönetilen Örneği kaynaklarının en fazla %20'sine ulaşacak şekilde ayarlanır ve bu değer, seçtiğiniz hizmet katmanına göre değişir. Daha fazla bilgi için bkz. [Azure SQL veritabanı satın alma modelleri](../database/purchasing-models.md).

### <a name="insufficient-memory-error"></a>Yetersiz bellek hatası

R için yeterli bellek yoksa bir hata iletisiyle karşılaşırsınız. Yaygın hata iletileri:

- `Unable to communicate with the runtime for 'R' script for request id: *******. Please check the requirements of 'R' runtime`
- `'R' script error occurred during execution of 'sp_execute_external_script' with HRESULT 0x80004004. ...an external script error occurred: "..could not allocate memory (0 Mb) in C function 'R_AllocStringBuffer'"`
- `An external script error occurred: Error: cannot allocate vector of size.`

Bellek kullanımı, R betiklerinizde kullanılan bellek miktarına ve yürütülen paralel sorgu sayısına göre değişir. Yukarıdaki hataları alıyorsanız, veritabanınızı daha yüksek bir hizmet katmanına ölçeklendirerek bu sorunu çözebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. genel bakış, [Azure SQL yönetilen örneği Machine Learning Services](machine-learning-services-overview.md).
- Machine Learning Services 'de Python kullanmayı öğrenmek için bkz. [Python betiklerini çalıştırma](/sql/machine-learning/tutorials/quickstart-python-create-script?context=/azure/azure-sql/managed-instance/context/ml-context&view=azuresqldb-mi-current&preserve-view=true).
- Machine Learning Services 'de R 'yi nasıl kullanacağınızı öğrenmek için bkz. [Run r Scripts](/sql/machine-learning/tutorials/quickstart-r-create-script?context=/azure/azure-sql/managed-instance/context/ml-context&view=azuresqldb-mi-current&preserve-view=true).
