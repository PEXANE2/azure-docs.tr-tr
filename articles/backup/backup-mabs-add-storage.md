---
title: Azure Yedekleme Sunucusu ile Modern Yedekleme Depolama'yı kullanma
description: Azure Yedekleme Sunucusu'ndaki yeni özellikler hakkında bilgi edinin. Bu makalede, Yedekleme Sunucusu yüklemenizin nasıl yükseltilen anlatılmaktadır.
ms.topic: conceptual
ms.date: 11/13/2018
ms.openlocfilehash: c6346d7b0275a00271c1787b378a63b8365edf2d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74172371"
---
# <a name="add-storage-to-azure-backup-server"></a>Azure Backup Sunucusu’na depolama alanı ekleme

Azure Backup Server V2 ve daha sonra yüzde 50 depolama tasarrufu, üç kat daha hızlı yedekleme ve daha verimli depolama sunan Modern Yedekleme Depolama'yı destekler. Ayrıca iş yüküne duyarlı depolama da sunar.

> [!NOTE]
> Modern Yedekleme Depolama'yı kullanmak için Windows Server 2016'da Backup Server V2 veya V3 veya Windows Server 2019'da V3'ü çalıştırmanız gerekir.
> Backup Server V2'yi Windows Server'ın önceki bir sürümünde çalıştırıyorsanız, Azure Yedekleme Sunucusu Modern Yedekleme Depolama'dan yarar kazaz. Bunun yerine, Yedekleme Server V1'de olduğu gibi iş yüklerini de korur. Daha fazla bilgi için Backup Server sürüm [koruma matrisine](backup-mabs-protection-matrix.md)bakın.
>
> Gelişmiş yedekleme performansları elde etmek için Windows Server 2019'da katmanlı depolama alanıyla MABS v3 dağıtmanızı öneririz. Katmanlı depolamayı yapılandırmak için adımlar için lütfen DPM makalesine bakın "[Katmanlı Depolama ile MBS ayarlayın](https://docs.microsoft.com/system-center/dpm/add-storage?view=sc-dpm-2019#set-up-mbs-with-tiered-storage)" .

## <a name="volumes-in-backup-server"></a>Yedekleme Sunucusundaki Birimler

Backup Server V2 veya daha sonra depolama birimleri kabul eder. Bir birim eklediğinizde, Yedekleme Sunucusu, modern yedekleme depolamanın gerektirdiği Esnek Dosya Sistemi'ne (ReFS) ses düzeyini biçimlendirır. Bir birim eklemek ve gerekirse daha sonra genişletmek için bu iş akışını kullanmanızı öneririz:

1. Yedekleme Sunucusu'nun VM'de ayarlanın.
2. Depolama havuzundaki sanal diskte birim oluşturma:
    1. Depolama havuzuna bir disk ekleyin ve basit düzene sahip bir sanal disk oluşturun.
    2. Ek diskler ekleyin ve sanal diski genişletin.
    3. Sanal diskte birim oluşturun.
3. Birimleri Yedekleme Sunucusu'na ekleyin.
4. İş yüküne duyarlı depolamayı yapılandırın.

## <a name="create-a-volume-for-modern-backup-storage"></a>Modern Yedekleme Depolama için bir birim oluşturma

Yedekleme Server V2 veya daha sonra disk depolama olarak hacimleri ile kullanarak depolama üzerinde kontrol sağlamak yardımcı olabilir. Birim tek bir disk olabilir. Ancak, gelecekte depolama alanını genişletmek istiyorsanız, depolama alanları kullanılarak oluşturulan diskten bir birim oluşturun. Bu, yedekleme depolama biriminin hacmini genişletmek istiyorsanız yardımcı olabilir. Bu bölümde, bu kurulumile bir birim oluşturmak için en iyi uygulamaları sunar.

1. Server Manager'da **Dosya ve Depolama Hizmetleri** > **Birimleri** > **Depolama Havuzları'nı**seçin. **Physical Disks**altında **Yeni Depolama Havuzu'ni**seçin.

    ![Yeni bir depolama havuzu oluşturma](./media/backup-mabs-add-storage/mabs-add-storage-1.png)

2. **GÖREVLER** açılır kutusunda Yeni **Sanal Disk'i**seçin.

    ![Sanal disk ekleme](./media/backup-mabs-add-storage/mabs-add-storage-2.png)

3. Depolama havuzunu seçin ve ardından **Fiziksel Disk Ekle'yi**seçin.

    ![Fiziksel disk ekleme](./media/backup-mabs-add-storage/mabs-add-storage-3.png)

4. Fiziksel diski seçin ve ardından **Sanal Diski Genişlet'i**seçin.

    ![Sanal diski genişletme](./media/backup-mabs-add-storage/mabs-add-storage-4.png)

5. Sanal diski seçin ve ardından **Yeni Birim'i**seçin.

    ![Yeni bir birim oluşturma](./media/backup-mabs-add-storage/mabs-add-storage-5.png)

6. Sunucu **ve disk** iletişim kutusunu seç'te sunucuyu ve yeni diski seçin. Ardından **İleri'yi**seçin.

    ![Sunucu yu ve diski seçin](./media/backup-mabs-add-storage/mabs-add-storage-6.png)

## <a name="add-volumes-to-backup-server-disk-storage"></a>Yedekleme Sunucusu disk depolamasına hacim ekleme

> [!NOTE]
>
> - Sütun sayısını 1'de tutmak için havuza yalnızca bir disk ekleyin. Daha sonra gerektiğinde diskler ekleyebilirsiniz.
> - Bir bakışta depolama havuzuna birden çok disk eklerseniz, disk sayısı sütun sayısı olarak depolanır. Daha fazla disk eklendiğinde, sütun sayısının yalnızca bir katı olabilir.

Yedekleme Sunucusuna bir birim eklemek için **Yönetim** bölmesinde, depolama alanını yeniden tarayanın ve ardından **Ekle'yi**seçin. Yedekleme Sunucusu Depolaması için eklenecek tüm birimlerin listesi görüntülenir. Kullanılabilir birimler seçili birimler listesine eklendikten sonra, bunları yönetmenize yardımcı olacak şekilde onlara dostça bir ad verebilirsiniz. Yedekleme Sunucusu'nun Modern Yedekleme Depolama'nın avantajlarını kullanabilmesi için bu birimleri ReFS olarak biçimlendirmek için **Tamam'ı**seçin.

![Kullanılabilir Birim Ekle](./media/backup-mabs-add-storage/mabs-add-storage-7.png)

## <a name="set-up-workload-aware-storage"></a>İş yüküne duyarlı depolama yı ayarlama

İş yüküne duyarlı depolama ile, belirli iş yüklerini tercihen depolayan birimleri seçebilirsiniz. Örneğin, yalnızca sık, yüksek hacimli yedekleme gerektiren iş yüklerini depolamak için saniyede çok sayıda giriş/çıkış işlemi (IOPS) destekleyen pahalı birimler ayarlayabilirsiniz. Bir örnek işlem günlükleri ile SQL Server olduğunu. VM'ler gibi daha az sıklıkta yedeklenen diğer iş yükleri düşük maliyetli birimlere yedeklenebilir.

### <a name="update-dpmdiskstorage"></a>Update-DPMDiskStorage

Azure Yedekleme Sunucusu'ndaki depolama havuzundaki bir birimin özelliklerini güncelleyen PowerShell cmdlet Update-DPMDiskStorage'ı kullanarak iş yüküne duyarlı depolama alanı ayarlayabilirsiniz.

Söz dizimi:

`Parameter Set: Volume`

```powershell
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```

Aşağıdaki ekran görüntüsü PowerShell penceresinde Ki Update-DPMDiskStorage cmdlet'i gösterir.

![PowerShell penceresindeki Update-DPMDiskStorage komutu](./media/backup-mabs-add-storage/mabs-add-storage-8.png)

PowerShell kullanarak yaptığınız değişiklikler Yedekleme Sunucusu Yönetici Konsolu'na yansıtılır.

![Yönetici Konsolundaki diskler ve birimler](./media/backup-mabs-add-storage/mabs-add-storage-9.png)

## <a name="migrate-legacy-storage-to-modern-backup-storage"></a>Eski depolama alanını Modern Yedekleme Depolamasına geçirin

Backup Server V2'ye yükselttikten veya yükledikten ve işletim sistemini Windows Server 2016'ya yükselttikten sonra, koruma gruplarınızı Modern Yedekleme Depolama'yı kullanacak şekilde güncelleyin. Varsayılan olarak, koruma grupları değiştirilmez. Başlangıçta kuruldukları gibi çalışmaya devam ederler.

Modern Yedekleme Depolama Alanı kullanmak için koruma gruplarını güncelleştirmek isteğe bağlıdır. Koruma grubunu güncelleştirmek için, verileri koruma seçeneğini kullanarak tüm veri kaynaklarının korunmasını durdurun. Ardından, veri kaynaklarını yeni bir koruma grubuna ekleyin.

1. Yönetici Konsolunda **Koruma** özelliğini seçin. Koruma **Grubu Üyesi** listesinde, üyeye sağ tıklayın ve ardından **üyenin korumasını durdur'u**seçin.

   ![Üyenin korumasını durdurun](https://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-stop-protection1.png)

2. Gruptan **Kaldır** iletişim kutusunda, kullanılan disk alanını ve depolama havuzu için kullanılabilir boş alanı gözden geçirin. Varsayılan, diskteki kurtarma noktalarını bırakmak ve ilişkilendirildikleri bekletme ilkesine göre sürelerinin dolmasına izin vermektir. **Tamam**'a tıklayın.

   Kullanılan disk alanını hemen boş depolama havuzuna döndürmek istiyorsanız, bu üyeyle ilişkili yedekleme verilerini (ve kurtarma noktalarını) silmek için **disk üzerindeki** yinelemeyi sil kutusunu seçin.

   ![Grup iletişim kutusundan kaldırma](https://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-retain-data.png)

3. Modern Yedekleme Depolama kullanan bir koruma grubu oluşturun. Korumasız veri kaynaklarını ekleyin.

## <a name="add-disks-to-increase-legacy-storage"></a>Eski depolama alanını artırmak için disk ekleme

Backup Server ile eski depolama alanını kullanmak istiyorsanız, eski depolama alanını artırmak için disk eklemeniz gerekebilir.

Disk depolama eklemek için:

1. Yönetici Konsolunda **Yönetim** > **Diski Depolama** > **Ekle'yi**seçin.

    ![Disk Depolama iletişim kutusu ekle](https://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-add-disk-storage.png)

2. Disk **Depolama Ekle** iletişim kutusunda **disk ekle'yi**seçin.

3. Kullanılabilir diskler listesinde, eklemek istediğiniz diskleri seçin, **Ekle'yi**seçin ve ardından **Tamam'ı**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Backup Server'ı yükledikten sonra sunucunuzu nasıl hazırlayacağınızı öğrenin veya iş yükünü korumaya başlayın.

- [Yedekleme Sunucusu iş yüklerini hazırlama](backup-azure-microsoft-azure-backup.md)
- [VMware sunucusuyedeklemek için Yedek Sunucu'yı kullanma](backup-azure-backup-server-vmware.md)
- [SQL Server'ı yedeklemek için Yedekleme Sunucusu'nı kullanın](backup-azure-sql-mabs.md)
