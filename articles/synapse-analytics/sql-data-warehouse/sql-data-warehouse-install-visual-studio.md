---
title: Visual Studio 2019'u Yükle
description: Synapse SQL için Visual Studio ve SQL Server Geliştirme Araçları (SSDT) yükleyin
services: synapse-analytics
ms.custom: vs-azure, azure-synapse
ms.workload: azure-vs
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 02/04/2020
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: aa2f903611f52f8f7a8ede0040b592a7dddd0e89
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80584463"
---
# <a name="getting-started-with-visual-studio-2019"></a>Visual Studio 2019 ile başlarken
Visual Studio **2019** SQL Server Data Tools (SSDT), aşağıdakileri yapmanıza olanak tanıyan tek bir araçtır:

- Uygulamaları bağlayın, sorgulayın ve geliştirin
- Tablolar, görünümler, depolanan yordamlar vb. dahil olmak üzere veri modelinizdeki tüm nesneleri görsel olarak keşfetmek için nesne gezgininden yararlanın.
- Nesneleriniz için T-SQL veri tanım dili (DDL) komut dosyaları oluşturma
- SSDT Veritabanı Projeleri ile devlet tabanlı bir yaklaşım kullanarak veri ambarınızı geliştirin
- Veritabanı projenizi Azure Depoları ile Git gibi kaynak denetim sistemleriyle tümleştirin
- Azure DevOps gibi otomasyon sunucularıyla sürekli tümleştirme ve dağıtım ardışık hatları ayarlama

## <a name="install-visual-studio-2019"></a>Visual Studio 2019'u Yükle
Visual Studio **16.3 ve üzerini**indirmek ve yüklemek için [Visual Studio 2019'u indirin][] ve yükleyin. Yükleme sırasında, veri depolama ve işleme iş yükünü seçin. Visual Studio 2019'da bağımsız SSDT kurulumuna artık gerek yok.

## <a name="unsupported-features-in-ssdt"></a>SSDT'de desteklenmeyen özellikler

Synapse SQL için özellik sürümlerisssSSDT desteği içermeyebilir zamanlar vardır. Aşağıdaki özellikler şu anda desteklenmeyen:

- [Somutlaştırılmış görünümler](https://docs.microsoft.com/sql/t-sql/statements/create-materialized-view-as-select-transact-sql?view=azure-sqldw-latest) (devam ediyor)
- [Sıralı Kümelenmiş Sütun lu Mağaza Dizinleri](https://docs.microsoft.com/sql/t-sql/statements/create-columnstore-index-transact-sql?view=azure-sqldw-latest#examples--and-) (devam ediyor)
- [COPY deyimi](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest) (devam ediyor)
- [İş yükü yönetimi](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-workload-management) - iş yükü grupları ve sınıflandırıcılar (devam ediyor)
- [Satır düzeyi güvenlik](https://docs.microsoft.com/sql/relational-databases/security/row-level-security?view=sql-server-ver15)
  - Özelliğin desteklenmesi için bir destek bileti gönderin veya [buraya](https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/39040057-ssdt-row-level-security) oy verin.
- [Dinamik veri maskeleme](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking?toc=%2Fazure%2Fsql-data-warehouse%2Ftoc.json&view=sql-server-2017#defining-a-dynamic-data-mask)
  - Özelliğin desteklenmesi için bir destek bileti gönderin veya [buraya](https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/39040048-ssdt-support-dynamic-data-masking) oy verin. 
- [PREDICT](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql?view=sql-server-ver15&viewFallbackFrom=azure-sqldw-latest) fonksiyonu 
- [Kısıtlamaları olan tablolar](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-table-constraints#table-constraints) desteklenmez. Bu tablo nesneleri için yapı eylemini "Yok" olarak ayarlayın.

## <a name="next-steps"></a>Sonraki adımlar

Artık SSDT'nin en son sürümüne sahip olduğunuza göre, SQL havuzunuza [bağlanmaya](sql-data-warehouse-query-visual-studio.md) hazırsınız.




<!--Other-->

[Visual Studio 2019’u İndirin]: https://visualstudio.microsoft.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
