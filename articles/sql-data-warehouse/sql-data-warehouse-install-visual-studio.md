---
title: SQL Data Warehouse için Visual Studio ve SSDT 'yi yükler | Microsoft Docs
description: Azure SQL Data Warehouse için Visual Studio'yu ve SQL Server Veri Araçları'nı (SSDT) Yükleme
services: sql-data-warehouse
ms.custom: vs-azure
ms.workload: azure-vs
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/05/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: aa904ac62ce41cc89945ddfdef2e426143e42bb4
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68479487"
---
# <a name="install-visual-studio-and-ssdt-for-sql-data-warehouse"></a>SQL Data Warehouse için Visual Studio ve SSDT 'yi yükler
SQL veri ambarı için uygulama geliştirmek üzere Visual Studio 2019 kullanın. Şu anda Visual Studio 2019 SSDT, SQL veri ambarı için desteklenmiyor. 

Visual Studio 'Yu SSDT ile kullanmak, SQL veri ambarınızdaki tabloları, görünümleri, saklı yordamları ve birçok daha fazla nesneyi görsel olarak araştırmak için SQL Server Nesne Gezgini kullanmanıza olanak sağlar. Ayrıca sorguları çalıştırmanızı sağlar.

> [!NOTE]
> SQL Data Warehouse, henüz Visual Studio Veritabanı Projelerini desteklemiyor. Bu özellikte düzenli güncelleştirmeler almak için lütfen [UserVoice]' ı oylayın.
> 
> 

## <a name="step-1-install-visual-studio"></a>1\. adım: Visual Studio'yu yükleme
Visual Studio 'Yu indirmek ve yüklemek için bu bağlantıları izleyin. Zaten Visual Studio 2013 veya daha yeni bir sürüm yüklüyse, adım 2 ' ye atlayabilir, SSDT 'yi yükleyebilirsiniz.

1. [Visual Studio 'Yu indirin][].
2. MSDN'de bulunan [Visual Studio’yu Yükleme][Installing Visual Studio] kılavuzunu izleyin ve varsayılan yapılandırmaları seçin.

## <a name="step-2-install-ssdt"></a>2\. adım: SSDT 'yi yükler
Visual Studio için SSDT 'yi yüklemek için, önce bu adımları izleyerek Visual Studio içinden bir SSDT güncelleştirmesi olup olmadığını denetleyin.

1. Visual Studio 'da **Araçlar** / **Uzantılar ve güncelleştirmeler** ' e tıklayın... / **Güncelleştirmeleriyle**
2. **Ürün Güncelleştirmeleri**'ni seçip **Veritabanı araçları için Microsoft SQL Server Güncelleştirmesi** olup olmadığına bakın.

Güncelleştirme bulunmazsa en son sürümün yüklü olması gerekir. SSDT'nin yüklendiğini doğrulamak **Yardım** / **Microsoft Visual Studio Hakkında**'ya tıklayın ve SQL Server Veri Araçları'nın listede olup olmadığına bakın. Yükleme seçeneği Visual Studio 'dan kullanılamazsa, SSDT 'yi el ile indirip yüklemek için [SSDT indirme][SSDT Download] sayfasını ziyaret edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
SSDT 'nin en son sürümüne sahip olduğunuza göre, SQL veri ambarınıza [bağlanmaya][connect] hazırsınız.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Visual Studio 'Yu indirin]: https://www.visualstudio.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
