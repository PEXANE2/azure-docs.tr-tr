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
ms.openlocfilehash: b8e37a15f473e5777fe5412b785a0916bd78f095
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80655909"
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

- [Somutlaştırılmış görünümler](/sql/t-sql/statements/create-materialized-view-as-select-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [Sıralı Kümelenmiş Sütunlu Mağaza Dizinleri](/sql/t-sql/statements/create-columnstore-index-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest#examples--and-)
- [COPY deyimi](/sql/t-sql/statements/copy-into-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [İş yükü yönetimi](sql-data-warehouse-workload-management.md) - iş yükü grupları ve sınıflandırıcılar
- [Satır düzeyi güvenlik](/sql/relational-databases/security/row-level-security?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
  - Özelliğin desteklenmesi için bir destek bileti gönderin veya [buraya](https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/39040057-ssdt-row-level-security) oy verin.
- [Dinamik veri maskeleme](/sql/relational-databases/security/dynamic-data-masking?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest#defining-a-dynamic-data-mask)
  - Özelliğin desteklenmesi için bir destek bileti gönderin veya [buraya](https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/39040048-ssdt-support-dynamic-data-masking) oy verin.
- [Kısıtlamaları olan tablolar](sql-data-warehouse-table-constraints.md#table-constraints) desteklenmez. Bu tablo nesneleri için yapı eylemini "Yok" olarak ayarlayın.

## <a name="next-steps"></a>Sonraki adımlar

Artık SSDT'nin en son sürümüne sahip olduğunuza göre, SQL havuzunuza [bağlanmaya](sql-data-warehouse-query-visual-studio.md) hazırsınız.

<!--Other-->

[Visual Studio 2019’u İndirin]: https://visualstudio.microsoft.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
