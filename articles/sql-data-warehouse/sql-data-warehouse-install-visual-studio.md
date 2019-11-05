---
title: SQL Data Warehouse için Visual Studio 2019 'yi yükler | Microsoft Docs
description: Azure SQL Data Warehouse için Visual Studio'yu ve SQL Server Veri Araçları'nı (SSDT) Yükleme
services: sql-data-warehouse
ms.custom: vs-azure
ms.workload: azure-vs
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 10/17/2019
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 0ca59080b4056ba74e2f05038e07d647f12de765
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73485049"
---
# <a name="getting-started-with-visual-studio-2019-for-sql-data-warehouse"></a>SQL veri ambarı için Visual Studio 2019 ile çalışmaya başlama
Visual Studio **2019** SQL Server veri araçları (SSDT), aşağıdakileri yapabilmenizi sağlayan tek bir araçtır:

- SQL veri ambarı için uygulama bağlama, sorgulama ve geliştirme 
- Tablolar, görünümler, saklı yordamlar ve vb. dahil olmak üzere veri modelinizdeki tüm nesneleri görsel olarak araştırmak için Nesne Gezgini 'nden yararlanın.
- Nesneleriniz için T-SQL veri tanımlama dili (DDL) betikleri oluşturun
- SSDT veritabanı projeleriyle durum tabanlı bir yaklaşım kullanarak veri Ambarınızı geliştirme
- Veritabanı projenizi git gibi kaynak denetim sistemleriyle tümleştirin Azure Repos
- Azure DevOps gibi Otomasyon sunucularıyla sürekli tümleştirme ve dağıtım işlem hatları ayarlama

> [!NOTE]
> Visual Studio SSDT veritabanı projeleri Şu anda önizlemededir. Bu özellikte düzenli güncelleştirmeler almak için lütfen [UserVoice]' ı oylayın.

## <a name="install-visual-studio-2019-preview"></a>Visual Studio 2019 Preview 'ı yükler
Visual Studio **16,3 ve üstünü**indirmek ve yüklemek için bkz. [Visual Studio 2019 'yi indirin][] . Yüklemesi sırasında veri depolama ve işleme iş yükünü seçin. Visual Studio 2019 için tek başına SSDT yüklemesi artık gerekli değildir.

## <a name="reporting-issues-with-ssdt-visual-studio-2019-preview"></a>SSDT Visual Studio 2019 (Önizleme) ile ilgili sorunları bildirme
SSDT 'yi SQL veri ambarı ile kullanırken oluşan sorunları bildirmek için şu e-posta dağıtım listesini e-postayla gönderin: <sqldwssdtpreview@service.microsoft.com>

## <a name="next-steps"></a>Sonraki adımlar
SSDT 'nin en son sürümüne sahip olduğunuza göre, SQL veri ambarınıza [bağlanmaya][connect] hazırsınız.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Visual Studio 2019 'yi indirin]: https://visualstudio.microsoft.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
