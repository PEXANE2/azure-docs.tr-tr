---
title: VM ayarlarından bir Azure VM 'yi yedekleme
description: Bu makalede, Azure Backup hizmetiyle tek bir Azure VM veya birden fazla Azure VM 'yi nasıl yedekleyeceğinizi öğrenin.
ms.topic: conceptual
ms.date: 06/13/2019
ms.openlocfilehash: 72d6e5657add3e815bb0d77fadbdbc716712bee5
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76705454"
---
# <a name="back-up-an-azure-vm-from-the-vm-settings"></a>VM ayarlarından bir Azure VM 'yi yedekleme

Bu makalede, Azure VM 'lerinin [Azure Backup](backup-overview.md) hizmetiyle nasıl yedekleneceği açıklanır. Azure VM 'Leri, birkaç yöntem kullanarak yedekleyebilirsiniz:

- Tek Azure VM: Bu makaledeki yönergeler, bir Azure VM 'yi doğrudan VM ayarlarından nasıl yedekleyeceğiniz açıklanır.
- Birden çok Azure VM: bir kurtarma hizmetleri Kasası ayarlayabilir ve birden çok Azure VM için yedeklemeyi yapılandırabilirsiniz. Bu senaryo için [Bu makaledeki](backup-azure-arm-vms-prepare.md) yönergeleri izleyin.

## <a name="before-you-start"></a>Başlamadan önce

1. Yedeklemenin nasıl çalıştığını [öğrenin](backup-architecture.md#how-does-azure-backup-work) ve destek gereksinimlerini [doğrulayın](backup-support-matrix.md#azure-vm-backup-support) .
2. Azure VM yedeklemesine [genel bakış alın](backup-azure-vms-introduction.md) .

### <a name="azure-vm-agent-installation"></a>Azure VM Aracısı yüklemesi

Azure VM 'lerini yedeklemek için, Azure Backup makinede çalışan VM aracısına bir uzantı yüklüyor. VM 'niz bir Azure Marketi görüntüsünden oluşturulduysa, aracı çalışır. Bazı durumlarda, örneğin, özel bir VM oluşturuyorsanız veya bir makineyi Şirket içinden geçirirseniz. aracıyı el ile yüklemeniz gerekebilir.

- VM aracısını el ile yüklemeniz gerekiyorsa, [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) veya [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) VM 'leri için yönergeleri izleyin.
- Aracı yüklendikten sonra, yedeklemeyi etkinleştirdiğinizde Azure Backup yedekleme uzantısını aracıya yüklenir. Uzantıyı Kullanıcı müdahalesi olmadan güncelleştirir ve düzeltme eklerini ekler.

## <a name="back-up-from-azure-vm-settings"></a>Azure VM ayarlarından yedekleme

1. [Azure Portal](https://portal.azure.com/)’ında oturum açın.
2. **Tüm hizmetler** ' e tıklayın ve filtreye, **sanal makineler**yazın ve ardından **sanal makineler**' e tıklayın.
3. VM 'Ler listesinden yedeklemek istediğiniz VM 'yi seçin.
4. VM menüsünde **Yedekle**' ye tıklayın.
5. **Kurtarma Hizmetleri kasasında**şunları yapın:
   - Zaten bir kasanız varsa, **Varolanı Seç**' e tıklayın ve bir kasa seçin.
   - Kasanız yoksa **Yeni oluştur**' a tıklayın. Kasa için bir ad belirtin. Bu, VM ile aynı bölgede ve kaynak grubunda oluşturulur. Yedeklemeyi doğrudan VM ayarlarından etkinleştirdiğinizde bu ayarları değiştiremezsiniz.

   ![Yedekleme Sihirbazını Etkinleştirme](./media/backup-azure-vms-first-look-arm/vm-menu-enable-backup-small.png)

6. **Yedekleme Ilkesini seçin**bölümünde şunları yapın:

   - Varsayılan ilkeyi bırakın. Bu, VM 'yi belirtilen zamanda günde bir kez yedekler ve kasadaki yedeklemeleri 30 gün boyunca tutar.
   - Varsa, var olan bir yedekleme ilkesi seçin.
   - Yeni bir ilke oluşturun ve ilke ayarlarını tanımlayın.  

   ![Yedekleme ilkesini seçme](./media/backup-azure-vms-first-look-arm/set-backup-policy.png)

7. **Yedeklemeyi etkinleştir**' e tıklayın. Bu, yedekleme ilkesini VM ile ilişkilendirir.

    ![Yedeklemeyi Etkinleştir düğmesi](./media/backup-azure-vms-first-look-arm/vm-management-menu-enable-backup-button.png)

8. Yapılandırma ilerlemesini Portal bildirimlerinde izleyebilirsiniz.
9. İş tamamlandıktan sonra VM menüsünde **Yedekle**' ye tıklayın. Sayfada VM için yedekleme durumu, kurtarma noktaları, çalışan işler ve verilen uyarılar hakkında bilgi gösterilir.

   ![Yedekleme durumu](./media/backup-azure-vms-first-look-arm/backup-item-view-update.png)

10. Yedeklemeyi etkinleştirdikten sonra bir ilk yedekleme çalışır. İlk yedeklemeyi hemen başlatabilir veya yedekleme zamanlamasına uygun şekilde çalışmaya başlayabilirsiniz.
    - İlk Yedekleme tamamlanana kadar, son yedekleme **durumu** **uyarı (ilk yedekleme beklemede)** olarak gösterilir.
    - Sonraki zamanlanmış yedeklemenin ne zaman çalışacağını görmek için yedekleme ilkesi adına tıklayın.

## <a name="run-a-backup-immediately"></a>Yedeklemeyi hemen Çalıştır

1. Bir yedeklemeyi hemen çalıştırmak için, VM menüsünde yedekleme > **Şimdi**yedekle **'** ye tıklayın.

    ![Yedeklemeyi çalıştır](./media/backup-azure-vms-first-look-arm/backup-now-update.png)

2. **Şimdi yedekleme** 'de, kurtarma noktası > ve **Tamam**' a kadar olacak şekilde Takvim denetimini kullanın.

    ![Yedekleme bekletme günü](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

3. Portal bildirimleri, yedekleme işinin tetiklendiğini bilmenizi sağlar. Yedekleme ilerlemesini izlemek için **Tüm Işleri görüntüle**' ye tıklayın.

## <a name="back-up-from-the-recovery-services-vault"></a>Kurtarma Hizmetleri kasasından yedekleme

Azure Backup kurtarma hizmetleri Kasası ayarlayarak ve kasadaki yedeklemeyi etkinleştirerek Azure VM 'lerinin yedeklenmesini etkinleştirmek için bu makaledeki yönergeleri izleyin.

>[!NOTE]
> **Azure Backup artık, Azure sanal makine yedekleme çözümünü kullanarak Seçmeli disk yedeklemesini ve geri yüklemeyi desteklemektedir.**
>
>Günümüzde, sanal makine yedekleme çözümünü kullanarak bir VM 'deki tüm disklerin (Işletim sistemi ve veri) yedeklenmesini destekler Azure Backup. Disk dışlama işlevselliğiyle, bir VM 'deki birçok veri diskinden bir veya birkaçını yedekleme seçeneği alırsınız. Bu, yedekleme ve geri yükleme gereksinimleriniz için verimli ve ekonomik bir çözüm sunar. Her kurtarma noktası, yedekleme işlemine dahil edilen disklerin verilerini içerir. Bu, geri yükleme işlemi sırasında verilen kurtarma noktasından geri yüklenen disklerin bir alt kümesine sahip etmenize olanak tanır. Bu, hem anlık görüntüden hem de kasadan geri yükleme için geçerlidir.
>
>**Önizlemeye kaydolmak için AskAzureBackupTeam@microsoft.com adresinden bize yazın**

## <a name="next-steps"></a>Sonraki adımlar

- Bu makaledeki yordamlardan herhangi biriyle ilgili zorluklarla karşılaşıyorsanız [sorun giderme kılavuzuna](backup-azure-vms-troubleshoot.md)başvurun.
- Yedeklemelerinizi yönetme [hakkında bilgi edinin](backup-azure-manage-vms.md) .
