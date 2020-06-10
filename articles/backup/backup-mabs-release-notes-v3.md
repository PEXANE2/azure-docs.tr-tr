---
title: Microsoft Azure Backup Server v3 için sürüm notları
description: Bu makalede, Microsoft Azure Backup Server (MABS) v3 için bilinen sorunlar ve geçici çözümler hakkında bilgi verilmektedir.
ms.topic: conceptual
ms.date: 06/03/2020
ms.asset: 0c4127f2-d936-48ef-b430-a9198e425d81
ms.openlocfilehash: 2f67b73612bd970c903b179a4a02c787ee0320b0
ms.sourcegitcommit: d7fba095266e2fb5ad8776bffe97921a57832e23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84629144"
---
# <a name="release-notes-for-microsoft-azure-backup-server"></a>Microsoft Azure Backup sunucusu için sürüm notları

Bu makalede Microsoft Azure Backup Server (MABS) v3 için bilinen sorunlar ve geçici çözümler sağlanmaktadır.

## <a name="backup-and-recovery-fails-for-clustered-workloads"></a>Kümelenmiş iş yükleri için yedekleme ve kurtarma başarısız oluyor

**Açıklama:** MABS v2 'yi MABS v3 sürümüne yükselttikten sonra, Hyper-V kümesi veya SQL kümesi (SQL her zaman açık) veya veritabanı kullanılabilirlik grubu (DAG) gibi kümelenmiş veri kaynakları için yedekleme/geri yükleme başarısız olur.

**Geçici çözüm:** Bunu engellemek için SQL Server Management Studio (SSMS)) açın ve aşağıdaki SQL betiğini DPM DB 'de çalıştırın:

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

## <a name="upgrade-to-mabs-v3-fails-in-russian-locale"></a>Rusça yerel ayarında MABS v3 'e yükseltme başarısız oluyor

**Açıklama:** MABS v2 'den MABS v3 sürümüne yükseltme, Rusça yerel ayarında **4387**hata koduyla başarısız oluyor.

**Geçici çözüm:** Rusça Install paketini kullanarak MABS v3 'e yükseltmek için aşağıdaki adımları uygulayın:

1. SQL veritabanınızı [yedekleyin](https://docs.microsoft.com/sql/relational-databases/backup-restore/create-a-full-database-backup-sql-server?view=sql-server-2017#SSMSProcedure) ve Mabs v2 'yi kaldırın (kaldırma sırasında korumalı verileri tutmayı seçin).
2. Yükseltme kapsamında SQL 2017 (Enterprise) ve raporlamayı Kaldır ' a yükseltin.
3. [Yüklemesi](https://docs.microsoft.com/sql/reporting-services/install-windows/install-reporting-services?view=sql-server-2017#install-your-report-server) SQL Server Reporting Services (SSRS).
4. [Yüklemesi](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) SQL Server Management Studio (SSMS).
5. [SQL 2017 Ile SSRS yapılandırmasında](https://docs.microsoft.com/azure/backup/backup-azure-microsoft-azure-backup#upgrade-mabs)belgelenen parametreleri kullanarak raporlamayı yapılandırın.
6. [Yüklemesi](backup-azure-microsoft-azure-backup.md) MABS V3.
7. [Geri yükle](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-database-backup-using-ssms?view=sql-server-2017) SSMS kullanarak SQL ve [burada](https://docs.microsoft.com/system-center/dpm/back-up-the-dpm-server?view=sc-dpm-2019#using-dpmsync)AÇıKLANDıĞı gibi DPM-Sync aracını çalıştırın.
8. Aşağıdaki komutu kullanarak dbo. tbl_DLS_GlobalSetting tablosundaki ' DataBaseVersion ' özelliğini güncelleştirin:

    ```sql
            UPDATE dbo.tbl_DLS_GlobalSetting
            set PropertyValue = '13.0.415.0'
            where PropertyName = 'DatabaseVersion'
    ```

9. MSDPM hizmetini başlatın.

## <a name="after-installing-ur1-the-mabs-reports-arent-updated-with-new-rdl-files"></a>UR1 yükledikten sonra MABS raporları yeni RDL dosyalarıyla güncellenmez

**Açıklama**: UR1 Ile, mabs raporu biçimlendirme sorunu, güncelleştirilmiş rdl dosyaları ile düzeltilir. Yeni RDL dosyaları, var olan dosyalarla otomatik olarak değiştirilmez.

**Geçici çözüm**: rdl dosyalarını değiştirmek için aşağıdaki adımları izleyin:

1. MABS makinesinde SQL Reporting Services Web portalı URL 'sini açın.
1. Web portalı URL 'sinde, DPMReports klasörü şu biçimde bulunur**`DPMReports_<GUID>`**

    >[!NOTE]
    >Bu adlandırma kuralına sahip her zaman yalnızca bir klasör vardır. MABS önceki bir sürümden yükseltilirse, başka bir eski klasör de olabilir, ancak bunu açamazsınız.

    ![DPMReports klasörü](./media/backup-mabs-release-notes-v3/dpm-reports-folder.png)

1. Klasörü seçin ve açın **`DPMReports_<GUID>`** . Bireysel rapor dosyaları aşağıda gösterildiği gibi listelenecektir.

    ![Bireysel rapor dosyalarının listesi](./media/backup-mabs-release-notes-v3/individual-report-files.png)

1. **Raporla**bitmeyen rapor dosyalarını seçin, **seçeneğine** sağ tıklayıp **Yönet**' i seçin.

    ![Rapor dosyaları için Yönet ' i seçin](./media/backup-mabs-release-notes-v3/manage-files.png)

1. Yeni sayfada, dosyaları en son rapor dosyalarıyla değiştirmek için **Değiştir** seçeneğini belirleyin.

    En son rapor dosyaları yolda bulunabilir`<MABS Installation Directory>\Program Files\Microsoft Azure Backup Server\DPM\DPM\bin\DpmReports`

    Örneğin, `C:\Program Files\Microsoft Azure Backup Server\DPM\DPM\bin\DpmReports`

    ![Dosyaları en son rapor dosyalarıyla değiştirin](./media/backup-mabs-release-notes-v3/replace-files.png)

    Dosyalar değiştirildikten sonra, **ad** ve **açıklamanın** bozulmadan ve boş olmadığından emin olun.

1. Dosyalar değiştirildikten sonra, MABS hizmetlerini yeniden başlatın ve rapor dosyalarını kullanın.

## <a name="next-steps"></a>Sonraki adımlar

[MABS yenilikleri](backup-mabs-whats-new-mabs.md)
