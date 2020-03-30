---
title: Microsoft Azure Backup Server v3 için sürüm notları
description: Bu makalede, Microsoft Azure Yedekleme Sunucusu (MABS) v3 için bilinen sorunlar ve geçici geçici servisler hakkında bilgi verilmektedir.
ms.topic: conceptual
ms.date: 11/22/2018
ms.asset: 0c4127f2-d936-48ef-b430-a9198e425d81
ms.openlocfilehash: b47d83e0e3714f3f035397acaadeac9cda39d12a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74172272"
---
# <a name="release-notes-for-microsoft-azure-backup-server"></a>Microsoft Azure Yedekleme Sunucusu için sürüm notları

Bu makalede, Microsoft Azure Yedekleme Sunucusu (MABS) V3 için bilinen sorunları ve geçici geçici geçici servisler sağlanmaktadır.

## <a name="backup-and-recovery-fails-for-clustered-workloads"></a>Kümelenmiş iş yükleri için yedekleme ve kurtarma başarısız olur

**Açıklama:** Yedekleme/geri yükleme, MABS V2'yi MABS V3'e yükselttikten sonra Veritabanı kullanılabilirlik grubunda (DAG) Hyper-V cluster veya SQL cluster (SQL Always All On) veya Exchange gibi kümelenmiş veri kaynakları için başarısız olur.

**Geçici çözüm:** Bunu önlemek için SQL Server Management Studio'yu (SSMS)) açın ve DPM DB'de aşağıdaki SQL komut dosyasını çalıştırın:

```sql
    IF EXISTS (SELECT * FROM dbo.sysobjects
        WHERE id = OBJECT_ID(N'[dbo].[tbl_PRM_DatasourceLastActiveServerMap]')
        AND OBJECTPROPERTY(id, N'IsUserTable') = 1)
        DROP TABLE [dbo].[tbl_PRM_DatasourceLastActiveServerMap]
        GO

        CREATE TABLE [dbo].[tbl_PRM_DatasourceLastActiveServerMap] (
            [DatasourceId]          [GUID]          NOT NULL,
            [ActiveNode]            [nvarchar](256) NULL,
            [IsGCed]                [bit]           NOT NULL
            ) ON [PRIMARY]
        GO

        ALTER TABLE [dbo].[tbl_PRM_DatasourceLastActiveServerMap] ADD
    CONSTRAINT [pk__tbl_PRM_DatasourceLastActiveServerMap__DatasourceId] PRIMARY KEY NONCLUSTERED
        (
            [DatasourceId]
        )  ON [PRIMARY],

    CONSTRAINT [DF_tbl_PRM_DatasourceLastActiveServerMap_IsGCed] DEFAULT
        (
            0
        ) FOR [IsGCed]
    GO
```

## <a name="upgrade-to-mabs-v3-fails-in-russian-locale"></a>MABS V3 yükseltme Rus yerel başarısız

**Açıklama:** MABS V2'den MABS V3'e Rus yerel olarak yükseltme **4387**hata koduyla başarısız olur.

**Geçici çözüm:** Rusça yükleme paketini kullanarak MABS V3'e yükseltmek için aşağıdaki adımları yapın:

1. SQL veritabanınızı [yedekleyin](https://docs.microsoft.com/sql/relational-databases/backup-restore/create-a-full-database-backup-sql-server?view=sql-server-2017#SSMSProcedure) ve MABS V2'yi kaldırın (kaldırma sırasında korunan verileri saklamayı seçin).
2. SQL 2017 'ye (Kurumsal) yükseltin ve yükseltmenin bir parçası olarak raporlamayı kaldırın.
3. [Yükle](https://docs.microsoft.com/sql/reporting-services/install-windows/install-reporting-services?view=sql-server-2017#install-your-report-server) SQL Server Reporting Services (SSRS).
4. [Yükle](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) SQL Server Management Studio (SSMS).
5. [SQL 2017 ile SSRS yapılandırmasında](https://docs.microsoft.com/azure/backup/backup-azure-microsoft-azure-backup#upgrade-mabs)belgelenen parametreleri kullanarak Raporlamayı yapılandırın.
6. [Yükle](backup-azure-microsoft-azure-backup.md) MABS V3.
7. [Geri Yükleme](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-database-backup-using-ssms?view=sql-server-2017) SQL SSMS kullanarak ve [burada](https://docs.microsoft.com/previous-versions/system-center/data-protection-manager-2010/ff634215(v=technet.10))açıklandığı gibi DPM-Sync aracı çalıştırın.
8. Dbo.tbl_DLS_GlobalSetting tablosundaki 'DataBaseVersion' özelliğini aşağıdaki komutu kullanarak güncelleştirin:

```sql
        UPDATE dbo.tbl_DLS_GlobalSetting
        set PropertyValue = '13.0.415.0'
        where PropertyName = 'DatabaseVersion'
```

9. MSDPM hizmetini başlatın.

## <a name="next-steps"></a>Sonraki adımlar

[MABS V3'te Yenilikler](backup-mabs-whats-new-mabs.md)
