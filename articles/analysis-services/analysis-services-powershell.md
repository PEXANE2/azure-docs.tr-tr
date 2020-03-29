---
title: PowerShell ile Azure Analiz Hizmetlerini Yönetin | Microsoft Dokümanlar
description: Sunucu oluşturma, işlemleri askıya alma veya hizmet düzeyini değiştirme gibi yaygın yönetim görevleri için Azure Analiz Hizmetleri PowerShell cmdlets'i açıklar.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: reference
ms.date: 10/28/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 2c8f4c0541d97a189087af692658cfe794eaaf7e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73572704"
---
# <a name="manage-azure-analysis-services-with-powershell"></a>Azure Analysis Services’ı PowerShell ile yönetme

Bu makalede, Azure Çözümleme Hizmetleri sunucusu ve veritabanı yönetim görevlerini gerçekleştirmek için kullanılan PowerShell cmdlets açıklanmaktadır. 

Sunucu oluşturma veya silme, sunucu işlemlerini askıya alma veya sürdürme veya hizmet düzeyini (katman) değiştirme (katman) gibi sunucu kaynak yönetimi görevleri Azure Analiz Hizmetleri cmdlets'i kullanır. SQL Server Analysis Services ile aynı SqlServer modülünde yer alan rol üye ekleme veya kaldırma, işleme veya bölümleme gibi veritabanlarını yönetmek için diğer görevler.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="permissions"></a>İzinler

Çoğu PowerShell görevi, yönettiğiniz Analiz Hizmetleri sunucusunda Yönetici ayrıcalıklarına sahip olduğunuzu gerektirir. Zamanlanmış PowerShell görevleri gözetimsiz işlemlerdir. Zamanlayıcıyı çalıştıran hesap veya hizmet sorumlusunun Çözümhizmetleri sunucusunda Yönetici ayrıcalıkları olmalıdır. 

Azure PowerShell cmdlets kullanan sunucu işlemlerinde, hesabınız veya hesap çalışan zamanlayıcı, [Azure Role Tabanlı Erişim Denetimi 'ndeki (RBAC)](../role-based-access-control/overview.md)kaynak için Sahibi rolüne de ait olmalıdır. 

## <a name="resource-and-server-operations"></a>Kaynak ve sunucu işlemleri 

Yükleme modülü - [Az.AnalysisServices](https://www.powershellgallery.com/packages/Az.AnalysisServices)   
Dokümantasyon - [Az.AnalysisServices referans](/powershell/module/az.analysisservices)

## <a name="database-operations"></a>Veritabanı işlemleri

Azure Analiz Hizmetleri veritabanı işlemleri, SQL Server Analysis Services ile aynı SqlServer modüllerini kullanır. Ancak, Azure Çözümleme Hizmetleri için tüm cmdlet'ler desteklenmez. 

SqlServer modülü, göreve özel veritabanı yönetimi cmdletlerinin yanı sıra, Tabular Model Komut Dosyası Dili (TMSL) sorgusu nu veya komut dosyasını kabul eden genel amaçlı Invoke-ASCmd cmdlet'i de sağlar. SqlServer modülündeki aşağıdaki cmdletler Azure Analiz Hizmetleri için desteklenir.

Modülü yükleyin - [SqlServer](https://www.powershellgallery.com/packages/SqlServer)   
Dokümantasyon - [SqlServer başvurusu](/powershell/module/sqlserver)

### <a name="supported-cmdlets"></a>Desteklenen cmdlets

|Cmdlet|Açıklama|
|------------|-----------------| 
|[Ekle-RolMember](https://docs.microsoft.com/powershell/module/sqlserver/Add-RoleMember)|Veritabanı rolüne bir üye ekleyin.| 
|[Yedekleme-ASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/backup-asdatabase)|Analiz Hizmetleri veritabanını yedekleme.|  
|[Remove-RoleMember](https://docs.microsoft.com/powershell/module/sqlserver/remove-rolemember)|Bir üyeyi veritabanı rolünden kaldırın.|   
|[Invoke-ASCmd](https://docs.microsoft.com/powershell/module/sqlserver/invoke-ascmd)|Bir TMSL komut dosyası çalıştırın.|
|[Invoke-ProcessASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processasdatabase)|Bir veritabanını işleme.|  
|[Invoke-ProcessPartition](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processpartition)|Bir bölümü işleme.| 
|[Invoke-ProcessTable](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processtable)|Bir tabloyu işleme.|  
|[Birleştirme-Bölme](https://docs.microsoft.com/powershell/module/sqlserver/merge-partition)|Bir bölümü birleştirin.|  
|[Geri Yükleme-ASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/restore-asdatabase)|Çözümleme Hizmetleri veritabanını geri yükleyin.| 
  

## <a name="related-information"></a>İlgili bilgiler

* [SQL Server PowerShell](https://docs.microsoft.com/sql/powershell/sql-server-powershell)      
* [SQL Server PowerShell Modüllerini İndir](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [SSMS İndir](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   
* [PowerShell Gallery'de SqlServer modülü](https://www.powershellgallery.com/packages/SqlServer)    
* [Uyumluluk Düzeyi 1200 ve üzeri için Tabular Model Programlama](https://docs.microsoft.com/analysis-services/tabular-model-programming-compatibility-level-1200/tabular-model-programming-for-compatibility-level-1200)
