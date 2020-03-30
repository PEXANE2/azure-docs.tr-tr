---
title: VM ayarlarından bir Azure VM'i yedekleme
description: Bu makalede, Azure Yedekleme hizmetiyle tekil bir Azure VM'yi veya birden çok Azure VM'yi nasıl yedekleyeceğimiz öğrenin.
ms.topic: conceptual
ms.date: 06/13/2019
ms.openlocfilehash: 72d6e5657add3e815bb0d77fadbdbc716712bee5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76705454"
---
# <a name="back-up-an-azure-vm-from-the-vm-settings"></a>VM ayarlarından bir Azure VM'i yedekleme

Bu makalede, [Azure Yedekleme](backup-overview.md) hizmetiyle Azure VM'leri nasıl yedeklenenizi açıklamaktadır. Birkaç yöntem kullanarak Azure VM'lerini yedekleyebilirsiniz:

- Tek Azure VM: Bu makaledeki yönergeler, bir Azure VM'inin doğrudan VM ayarlarından nasıl yedeklenebildiğini açıklar.
- Birden çok Azure VM'si: Bir Kurtarma Hizmetleri kasası ayarlayabilir ve birden çok Azure VM'si için yedekleme yi yapılandırabilirsiniz. Bu senaryo için [bu makaledeki](backup-azure-arm-vms-prepare.md) yönergeleri izleyin.

## <a name="before-you-start"></a>Başlamadan önce

1. Yedeklemenin nasıl çalıştığını [öğrenin](backup-architecture.md#how-does-azure-backup-work) ve destek gereksinimlerini [doğrulayın.](backup-support-matrix.md#azure-vm-backup-support)
2. Azure VM yedeklemesi hakkında [genel bir bakış alın.](backup-azure-vms-introduction.md)

### <a name="azure-vm-agent-installation"></a>Azure VM aracı yüklemesi

Azure VM'leri yedeklemek için Azure Yedekleme, makinede çalışan VM aracısına bir uzantı yükler. VM'niz Bir Azure pazar yeri görüntüsünden oluşturulduysa, aracı çalışıyor olur. Bazı durumlarda, örneğin özel bir VM oluşturursanız veya bir makineyi şirket içinden geçirin. aracıyı el ile yüklemeniz gerekebilir.

- VM aracısını el ile yüklemeniz gerekiyorsa, [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) veya [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) VM'leri için yönergeleri izleyin.
- Aracı yüklendikten sonra, yedeklemeyi etkinleştirdiğinizde Azure Yedekleme yedekleme uzantısını aracıya yükler. Kullanıcı müdahalesi olmadan uzantıyı günceller ve yamalar.

## <a name="back-up-from-azure-vm-settings"></a>Azure VM ayarlarından yedekleme

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. **Tüm hizmetleri** tıklatın ve Filtre'de **Sanal makineleri**yazın ve ardından Sanal **makineleri**tıklatın.
3. VM'ler listesinden yedeklemek istediğiniz VM'yi seçin.
4. VM menüsünde **Yedekleme'yi**tıklatın.
5. **Kurtarma Hizmetleri kasasında**aşağıdakileri yapın:
   - Zaten bir kasanız varsa, **varolan seç'i**tıklatın ve bir kasa seçin.
   - Kasanız yoksa, **yeni oluştur'u**tıklatın. Kasa için bir ad belirtin. VM ile aynı bölge ve kaynak grubunda oluşturulur. Yedeklemeyi doğrudan VM ayarlarından etkinleştirdiğinizde bu ayarları değiştiremezsiniz.

   ![Yedekleme Sihirbazını Etkinleştirme](./media/backup-azure-vms-first-look-arm/vm-menu-enable-backup-small.png)

6. **Yedekleme ilkesini seç'te**aşağıdakileri yapın:

   - Varsayılan ilkeyi bırakın. Bu, VM'yi belirtilen saatte günde bir kez yedekler ve yedekleri 30 gün boyunca kasada tutar.
   - Varsa varolan bir yedekleme ilkesini seçin.
   - Yeni bir ilke oluşturun ve ilke ayarlarını tanımlayın.  

   ![Yedekleme ilkesini seçme](./media/backup-azure-vms-first-look-arm/set-backup-policy.png)

7. **Yedeklemeyi Etkinleştir'i**tıklatın. Bu, yedekleme ilkesini VM ile ilişkilendirer.

    ![Yedeklemeyi Etkinleştir düğmesi](./media/backup-azure-vms-first-look-arm/vm-management-menu-enable-backup-button.png)

8. Portal bildirimlerinde yapılandırma ilerlemesini izleyebilirsiniz.
9. İş tamamlandıktan sonra, VM menüsünde **Yedekleme'yi**tıklatın. Sayfada VM için yedekleme durumu, kurtarma noktaları, çalışan işler ve verilen uyarılar hakkında bilgiler gösterilmektedir.

   ![Yedekleme durumu](./media/backup-azure-vms-first-look-arm/backup-item-view-update.png)

10. Yedeklemeyi etkinleştirdikten sonra, ilk yedekleme çalışır. İlk yedeklemeyi hemen başlatabilir veya yedekleme zamanlamasına uygun olarak başlayana kadar bekleyebilirsiniz.
    - İlk yedekleme tamamlanana kadar, **Son yedekleme durumu** Uyarı olarak gösterir **(Bekleyen ilk yedekleme)**.
    - Bir sonraki zamanlanmış yedeklemenin ne zaman çalışacağını görmek için yedekleme ilkesi adını tıklatın.

## <a name="run-a-backup-immediately"></a>Yedeklemeyi hemen çalıştırın

1. Bir yedeklemeyi hemen çalıştırmak için, VM menüsünde Yedek > **Yedekleme'yi şimdi**tıklatın. **Backup**

    ![Yedeklemeyi çalıştır](./media/backup-azure-vms-first-look-arm/backup-now-update.png)

2. **Yedekleme Şimdi** kurtarma noktası > ve **Tamam**korunacak kadar seçmek için takvim denetimini kullanın.

    ![Yedekleme bekletme günü](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

3. Portal bildirimleri yedekleme işinin tetiklendiğini size bildirir. Yedekleme ilerlemesini izlemek için **tüm işleri görüntüle'yi**tıklatın.

## <a name="back-up-from-the-recovery-services-vault"></a>Kurtarma Hizmetleri kasasından yedekleme

Azure Yedekleme Kurtarma Hizmetleri kasası kurarak ve kasada yedeklemeyi etkinleştirerek Azure VM'leri için yedeklemeyi etkinleştirmek için bu makaledeki yönergeleri izleyin.

>[!NOTE]
> **Azure Yedekleme artık seçici disk yedeklemesini destekler ve Azure Sanal Makine yedekleme çözümlerini kullanarak geri yüklemeyi destekler.**
>
>Bugün Azure Yedekleme, Sanal Makine yedekleme çözümlerini kullanarak VM'deki tüm diskleri (İşletim Sistemi ve verileri) birlikte yedeklemeyi destekler. Diski dışlama işleviyle, VM'deki birçok veri diskinden bir veya birkaçını yedekleme seçeneğine sahip olursunuz. Bu, yedekleme ve geri yükleme gereksinimleriniz için verimli ve uygun maliyetli bir çözüm sağlar. Her kurtarma noktası yedekleme işlemine dahil edilen disklerin verilerini içerir ve bu da geri yükleme işlemi sırasında verilen kurtarma noktasından geri yüklenen disklerin bir alt kümesini geri yüklemenize olanak tanır. Bu, hem anlık görüntüden hem de kasadan geri yüklemek için geçerlidir.
>
>**Önizleme için kaydolmak için, bize yazınAskAzureBackupTeam@microsoft.com**

## <a name="next-steps"></a>Sonraki adımlar

- Bu makaledeki yordamlardan herhangi biriyle ilgili sorunlar yaşıyorsanız, [sorun giderme kılavuzuna](backup-azure-vms-troubleshoot.md)başvurun.
- Yedeklemelerinizi yönetme [hakkında bilgi edinin.](backup-azure-manage-vms.md)
