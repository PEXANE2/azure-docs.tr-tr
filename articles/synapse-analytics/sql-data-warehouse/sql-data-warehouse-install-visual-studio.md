---
title: Visual Studio 2019 'yi yükler
description: SYNAPSE SQL için Visual Studio ve SQL Server geliştirme araçları 'nı (SSDT) yükler
services: synapse-analytics
ms.custom: vs-azure, azure-synapse
ms.workload: azure-vs
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 05/11/2020
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 9f36fb952b21b058fb50dc567f714e8bdb665d6c
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83200323"
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

Visual Studio **16,3 ve üstünü**indirmek ve yüklemek için bkz. [Visual Studio 2019 'yi indirme](https://visualstudio.microsoft.com/downloads/) . Yüklemesi sırasında veri depolama ve işleme iş yükünü seçin. Visual Studio 2019 için tek başına SSDT yüklemesi artık gerekli değildir.

## <a name="unsupported-features-in-ssdt"></a>SSDT 'de desteklenmeyen özellikler

SYNAPSE SQL için özellik sürümlerinin SSDT desteğinin olmadığı durumlar vardır. Şu özellikler Şu anda desteklenmiyor:


- [Iş yükü yönetimi](sql-data-warehouse-workload-management.md) -iş yükü grupları ve sınıflandırıcılar
- [Satır düzeyi güvenlik](/sql/relational-databases/security/row-level-security?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
  - Desteklenen özelliği almak için bir [destek bileti veya oy](https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/39040057-ssdt-row-level-security) verin.
- [Dinamik veri maskeleme](/sql/relational-databases/security/dynamic-data-masking?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest#defining-a-dynamic-data-mask)
   - Desteklenen özelliği almak için bir [destek bileti veya oy](https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/39040048-ssdt-support-dynamic-data-masking) verin.

## <a name="next-steps"></a>Sonraki adımlar

SSDT 'nin en son sürümüne sahip olduğunuza göre, SQL havuzunuza [bağlanmaya](sql-data-warehouse-query-visual-studio.md) hazırsınız demektir.
