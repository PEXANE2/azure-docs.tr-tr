---
title: PowerShell ile Azure Analysis Services yönetme | Microsoft Docs
description: Sunucu oluşturma, işlemleri askıya alma veya hizmet düzeyini değiştirme gibi yaygın yönetim görevleri için Azure Analysis Services PowerShell cmdlet 'lerini açıklar.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 28f414c5eaaea7b987f2c3694cb8fc73b70838e9
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "92018789"
---
# <a name="manage-azure-analysis-services-with-powershell"></a>Azure Analysis Services’ı PowerShell ile yönetme

Bu makalede, Azure Analysis Services sunucu ve veritabanı yönetim görevlerini gerçekleştirmek için kullanılan PowerShell cmdlet 'leri açıklanır. 

Sunucu oluşturma veya silme, sunucu işlemlerini askıya alma veya sürdürme veya hizmet düzeyi (katman) Azure Analysis Services cmdlet 'lerini değiştirme gibi sunucu kaynak yönetimi görevleri. Rol üyeleri ekleme veya kaldırma gibi veritabanlarını yönetmeye yönelik diğer görevler, SQL Server Analysis Services ile aynı SqlServer modülüne dahil edilen cmdlet 'leri kullanır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="permissions"></a>İzinler

Çoğu PowerShell görevi, yönettiğiniz Analysis Services sunucuda yönetici ayrıcalıklarına sahip olmanızı gerektirir. Zamanlanmış PowerShell görevleri katılımsız işlemlerdir. Zamanlayıcı 'yı çalıştıran hesap veya hizmet sorumlusu Analysis Services sunucuda yönetici ayrıcalıklarına sahip olmalıdır. 

Azure PowerShell cmdlet 'leri kullanan sunucu işlemleri için, hesabınız veya zamanlayıcı çalıştıran hesap, [Azure rol tabanlı erişim denetimi (Azure RBAC)](../role-based-access-control/overview.md)içindeki kaynak için sahip rolüne de ait olmalıdır. 

## <a name="resource-and-server-operations"></a>Kaynak ve sunucu işlemleri 

Modül-Install- [az. AnalysisServices](https://www.powershellgallery.com/packages/Az.AnalysisServices)   
Belgeler- [az. AnalysisServices başvurusu](/powershell/module/az.analysisservices)

## <a name="database-operations"></a>Veritabanı işlemleri

Azure Analysis Services veritabanı işlemleri SQL Server Analysis Services aynı SqlServer modülünü kullanır. Ancak, Azure Analysis Services için tüm cmdlet 'ler desteklenmez. 

SqlServer modülü, göreve özgü veritabanı yönetim cmdlet 'lerinin yanı sıra tablosal model betik dili (TMSL) sorgusu veya betiği kabul eden genel amaçlı Invoke-ASCmd cmdlet 'ini sağlar. Azure Analysis Services için SqlServer modülündeki aşağıdaki cmdlet 'ler desteklenir.

Modül Install- [SqlServer](https://www.powershellgallery.com/packages/SqlServer)   
Belgeler- [SqlServer başvurusu](/powershell/module/sqlserver)

### <a name="supported-cmdlets"></a>Desteklenen Cmdlet 'ler

|Cmdlet|Açıklama|
|------------|-----------------| 
|[Add-Rolemebir](/powershell/module/sqlserver/Add-RoleMember)|Bir veritabanı rolüne üye ekleyin.| 
|[Backup-ASDatabase](/powershell/module/sqlserver/backup-asdatabase)|Analysis Services bir veritabanını yedekleyin.|  
|[Remove-Rolemeoda](/powershell/module/sqlserver/remove-rolemember)|Bir üyeyi veritabanı rolünden kaldırın.|   
|[Invoke-ASCmd](/powershell/module/sqlserver/invoke-ascmd)|Bir TMSL betiği yürütün.|
|[Invoke-ProcessASDatabase](/powershell/module/sqlserver/invoke-processasdatabase)|Bir veritabanını işleyin.|  
|[Invoke-ProcessPartition](/powershell/module/sqlserver/invoke-processpartition)|Bir bölümü işleyin.| 
|[Invoke-ProcessTable](/powershell/module/sqlserver/invoke-processtable)|Bir tabloyu işleyin.|  
|[Birleştirme-bölüm](/powershell/module/sqlserver/merge-partition)|Bölüm birleştirme.|  
|[Restore-ASDatabase](/powershell/module/sqlserver/restore-asdatabase)|Bir Analysis Services veritabanını geri yükleyin.| 
  

## <a name="related-information"></a>İlgili bilgiler

* [SQL Server PowerShell](/sql/powershell/sql-server-powershell)      
* [PowerShell modülünü indir SQL Server](/sql/ssms/download-sql-server-ps-module)   
* [SSMS 'yi indir](/sql/ssms/download-sql-server-management-studio-ssms)   
* [PowerShell Galerisi içinde SqlServer modülü](https://www.powershellgallery.com/packages/SqlServer)    
* [Uyumluluk düzeyi 1200 ve üzeri için tablolu model programlama](/analysis-services/tabular-model-programming-compatibility-level-1200/tabular-model-programming-for-compatibility-level-1200)