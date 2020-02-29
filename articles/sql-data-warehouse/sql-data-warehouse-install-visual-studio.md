---
title: Visual Studio 2019 'yi yükler
description: SQL Analytics için Visual Studio ve SQL Server geliştirme araçları 'nı (SSDT) yükler
services: sql-data-warehouse
ms.custom: azure-synapse
ms.workload: azure-vs
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 02/04/2020
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: b9ad0f4aedc5a095bfa70dec1b67e52cd4d152e0
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78195348"
---
# <a name="getting-started-with-visual-studio-2019"></a>Visual Studio 2019 ile çalışmaya başlama
Visual Studio **2019** SQL Server veri araçları (SSDT), aşağıdakileri yapabilmenizi sağlayan tek bir araçtır:

- Uygulamaları bağlama, sorgulama ve geliştirme
- Tablolar, görünümler, saklı yordamlar ve vb. dahil olmak üzere veri modelinizdeki tüm nesneleri görsel olarak araştırmak için Nesne Gezgini 'nden yararlanın.
- Nesneleriniz için T-SQL veri tanımlama dili (DDL) betikleri oluşturun
- SSDT veritabanı projeleriyle durum tabanlı bir yaklaşım kullanarak veri Ambarınızı geliştirme
- Veritabanı projenizi git gibi kaynak denetim sistemleriyle tümleştirin Azure Repos
- Azure DevOps gibi Otomasyon sunucularıyla sürekli tümleştirme ve dağıtım işlem hatları ayarlama

## <a name="install-visual-studio-2019"></a>Visual Studio 2019 'yi yükler
Visual Studio **16,3 ve üstünü**indirmek ve yüklemek için bkz. [Visual Studio 2019 'yi indirin][] . Yüklemesi sırasında veri depolama ve işleme iş yükünü seçin. Visual Studio 2019 için tek başına SSDT yüklemesi artık gerekli değildir.

## <a name="unsupported-features-in-ssdt"></a>SSDT 'de desteklenmeyen özellikler

SQL Analytics Özellik sürümlerinin SSDT için destek içermediği durumlar vardır. Şu özellikler Şu anda desteklenmiyor:

- [Gerçekleştirilmiş görünümler](https://docs.microsoft.com/sql/t-sql/statements/create-materialized-view-as-select-transact-sql?view=azure-sqldw-latest) (devam ediyor)
- [Sıralı kümelenmiş columnstore dizinleri](https://docs.microsoft.com/sql/t-sql/statements/create-columnstore-index-transact-sql?view=azure-sqldw-latest#examples--and-) (devam ediyor)
- [Copy deyimleri](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest) (devam ediyor)
- [Iş yükü yönetimi](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-workload-management) -iş yükü grupları ve sınıflandırıcılar (devam ediyor)
- [Satır düzeyi güvenlik](https://docs.microsoft.com/sql/relational-databases/security/row-level-security?view=sql-server-ver15)
  - Desteklenen özelliği almak için bir destek bileti veya [buraya](https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/39040057-ssdt-row-level-security) oy verin.
- [Dinamik veri maskeleme](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking?toc=%2Fazure%2Fsql-data-warehouse%2Ftoc.json&view=sql-server-2017#defining-a-dynamic-data-mask)
  - Desteklenen özelliği almak için bir destek bileti veya [buraya](https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/39040048-ssdt-support-dynamic-data-masking) oy verin. 
- [Tahmin](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql?view=sql-server-ver15&viewFallbackFrom=azure-sqldw-latest) işlevi 
- [Kısıtlamaları olan tablolar](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-table-constraints#table-constraints) desteklenmiyor. Bu tablo nesneleri için, oluşturma eylemini "none" olarak ayarlayın.

## <a name="next-steps"></a>Sonraki adımlar

SSDT 'nin en son sürümüne sahip olduğunuza göre, SQL havuzunuza [bağlanmaya][connect] hazırsınız demektir.

<!--Anchors-->

<!--Image references-->

<!--Articles-->

[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->

[Visual Studio 2019 'yi indirin]: https://visualstudio.microsoft.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
