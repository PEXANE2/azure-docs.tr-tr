---
title: Azure VM 'lerini bir kurtarma hizmetleri kasasında yedekleme
description: Azure VM 'Leri bir kurtarma hizmetleri kasasında Azure Backup kullanarak nasıl yedekleyeceğiniz açıklanmaktadır.
ms.topic: conceptual
ms.date: 04/03/2019
ms.openlocfilehash: aeadd7bc798f690c67eef38c6dc645204ff39115
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78363868"
---
# <a name="back-up-azure-vms-in-a-recovery-services-vault"></a>Azure VM 'lerini bir kurtarma hizmetleri kasasında yedekleme

Bu makalede, Azure VM 'lerinin [Azure Backup](backup-overview.md) hizmeti kullanılarak bir kurtarma hizmetleri kasasında nasıl yedekleneceği açıklanır.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
>
> * Azure VM 'lerini hazırlayın.
> * Kasa oluşturun.
> * VM 'Leri bulun ve bir yedekleme ilkesi yapılandırın.
> * Azure VM 'Leri için yedeklemeyi etkinleştirin.
> * İlk yedeklemeyi çalıştırın.

> [!NOTE]
> Bu makalede, bir kasasının nasıl ayarlanacağı ve yedeklenecek VM 'Lerin nasıl ayarlanacağı açıklanır. Birden çok VM 'yi yedeklemek istiyorsanız bu yararlı olur. Alternatif olarak, [tek bir Azure VM](backup-azure-vms-first-look-arm.md) 'YI doğrudan VM ayarlarından yedekleyebilirsiniz.

## <a name="before-you-start"></a>Başlamadan önce

* Azure VM yedekleme mimarisini [gözden geçirin](backup-architecture.md#architecture-built-in-azure-vm-backup) .
* [Hakkında bilgi edinin](backup-azure-vms-introduction.md) Azure VM yedeklemesi ve yedekleme uzantısı.
* Yedeklemeyi yapılandırmadan önce [destek matrisini gözden geçirin](backup-support-matrix-iaas.md) .

Ayrıca, bazı durumlarda yapmanız gerekebilecek birkaç şey vardır:

* VM **ARACıSıNı VM 'ye yükleme**: Azure Backup makinede çalışan Azure VM aracısına bir uzantı yükleyerek Azure VM 'lerini yedekler. VM 'niz bir Azure Marketi görüntüsünden oluşturulduysa, aracı yüklenir ve çalışır. Özel bir VM oluşturursanız veya şirket içi bir makineyi geçirirseniz, [aracıyı el ile yüklemeniz](#install-the-vm-agent)gerekebilir.

## <a name="create-a-vault"></a>Kasa oluşturma

 Kasa, zaman içinde oluşturulan yedeklemeleri ve kurtarma noktalarını depolar ve yedeklenen makinelerle ilişkili yedekleme ilkelerini depolar. Aşağıdaki gibi bir kasa oluşturun:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Arama bölümünde, **Kurtarma Hizmetleri**yazın. **Hizmetler**' in altında, **Kurtarma Hizmetleri kasaları**' na tıklayın.

     ![Kurtarma Hizmetleri kasalarını ara](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png)

3. **Kurtarma Hizmetleri kasaları** menüsünde **+ Ekle**' ye tıklayın.

     ![Kurtarma Hizmetleri Kasası oluşturma 2. adım](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

4. **Kurtarma Hizmetleri kasasında**kasayı tanımlamak için bir kolay ad yazın.
    * Adın Azure aboneliği için benzersiz olması gerekir.
    * Bu, 2 ile 50 karakter içerebilir.
    * Bir harfle başlamalı ve yalnızca harf, rakam ve kısa çizgi içerebilir.
5. Kasanın oluşturulması gereken Azure aboneliğini, kaynak grubunu ve coğrafi bölgeyi seçin. Sonra **Oluştur**’a tıklayın.
    * Kasanın oluşturulması biraz zaman alabilir.
    * Portalın sağ üst bölümündeki durum bildirimlerini izleyin.

Kasa oluşturulduktan sonra kurtarma hizmetleri kasaları listesinde görünür. Kasanızı görmüyorsanız **Yenile**' yi seçin.

![Yedekleme kasalarının listesi](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

>[!NOTE]
> Azure Backup artık Azure Backup hizmeti tarafından oluşturulan kaynak grubu adının özelleştirilmesine izin veriyor. Daha fazla bilgi için bkz. [sanal makineler için Azure Backup kaynak grubu](backup-during-vm-creation.md#azure-backup-resource-group-for-virtual-machines).

### <a name="modify-storage-replication"></a>Depolama çoğaltmasını değiştirme

Varsayılan olarak, [kasalar coğrafi olarak yedekli depolama (GRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs)kullanır.

* Kasa birincil yedekleme mekanizmanız ise GRS kullanmanızı öneririz.
* Bir ucuz seçeneği için [yerel olarak yedekli depolama (LRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) kullanabilirsiniz.

Depolama çoğaltma türünü aşağıdaki gibi değiştirin:

1. Yeni kasada, **Ayarlar** bölümünde **Özellikler** ' e tıklayın.
2. **Özellikler**' de, **yedekleme yapılandırması**altında **Güncelleştir**' e tıklayın.
3. Depolama çoğaltma türünü seçin ve **Kaydet**' e tıklayın.

      ![Yeni kasa için depolama yapılandırması ayarlama](./media/backup-try-azure-backup-in-10-mins/full-blade.png)

> [!NOTE]
   > Kasa ayarlandıktan ve yedekleme öğeleri içerdiğinde depolama çoğaltma türünü değiştiremezsiniz. Bunu yapmak istiyorsanız kasayı yeniden oluşturmanız gerekir.

## <a name="apply-a-backup-policy"></a>Yedekleme ilkesi uygulama

Kasa için bir yedekleme ilkesi yapılandırın.

1. Kasada **genel bakış** bölümünde **+ Yedekle** ' ye tıklayın.

   ![Yedekleme düğmesi](./media/backup-azure-arm-vms-prepare/backup-button.png)

2. **Yedekleme hedefi** 'nde > **Iş yükünüz nerede çalışıyor?** **Azure**' u seçin. **Neleri yedeklemek istiyorsunuz?** **sanal makine** >  **Tamam ' ı**seçin. Bu, VM uzantısını kasaya kaydeder.

   ![Yedekleme ve yedekleme hedef bölmeleri](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

3. **Yedekleme ilkesinde**, kasa ile ilişkilendirmek istediğiniz ilkeyi seçin.
    * Varsayılan ilke, VM 'yi günde bir kez yedekler. Günlük yedeklemeler 30 gün boyunca tutulur. Anlık kurtarma anlık görüntüleri iki gün boyunca tutulur.
    * Varsayılan ilkeyi kullanmak istemiyorsanız, **Yeni oluştur**' u seçin ve sonraki yordamda açıklandığı gibi özel bir ilke oluşturun.

      ![Varsayılan yedekleme ilkesi](./media/backup-azure-arm-vms-prepare/default-policy.png)

4. **Sanal makineler Seç**bölümünde, ilkeyi kullanarak yedeklemek Istediğiniz VM 'leri seçin. Daha sonra, **Tamam**'a tıklayın.

   * Seçilen VM 'Ler doğrulanacak.
   * Yalnızca kasala aynı bölgedeki VM 'Leri seçebilirsiniz.
   * VM 'Ler yalnızca tek bir kasada yedeklenebilir.

     !["Sanal makine seçin" bölmesi](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

5. **Yedekleme**'de **yedeklemeyi etkinleştir**' e tıklayın. Bu, ilkeyi kasaya ve VM 'lere dağıtır ve yedekleme uzantısını Azure VM 'de çalışan VM aracısına kurar.

     !["Yedeklemeyi etkinleştir" düğmesi](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Yedeklemeyi etkinleştirdikten sonra:

* Yedekleme hizmeti, sanal makinenin çalışıp çalışmadığını, yedekleme uzantısını da yüklüyor.
* İlk yedekleme, yedekleme zamanlamanıza uygun olarak çalışacaktır.
* Yedeklemeler çalıştırıldığında şunları unutmayın:
  * Çalıştıran bir VM, uygulamayla tutarlı bir kurtarma noktası yakalamak için en büyük şansınız vardır.
  * Ancak, VM kapatılmış olsa bile. Bu tür bir VM, çevrimdışı bir VM olarak bilinir. Bu durumda, kurtarma noktası kilitlenmeyle tutarlı olacaktır.
* Azure VM 'lerinin yedeklenmesini sağlamak için açık giden bağlantı gerekli değildir.

### <a name="create-a-custom-policy"></a>Özel ilke oluşturma

Yeni bir yedekleme ilkesi oluşturmayı seçtiyseniz, ilke ayarlarını girin.

1. **İlke adı**' nda anlamlı bir ad belirtin.
2. **Yedekleme zamanlaması**' nda, yedeklemelerin ne zaman alınacağını belirtin. Azure VM 'Leri için günlük veya haftalık yedeklemeler gerçekleştirebilirsiniz.
3. **Anlık geri yükleme**' de anlık geri yükleme için anlık görüntüleri yerel olarak ne kadar süreyle bekletmek istediğinizi belirtin.
    * ' Yi geri yüklerken, yedeklenen VM diskleri depolama alanından, ağ üzerinden kurtarma depolama konumuna kopyalanır. Anlık geri yükleme sayesinde, yedekleme verilerinin kasaya aktarılmasını beklemeden, bir yedekleme işi sırasında alınan yerel olarak depolanmış anlık görüntülerden yararlanabilirsiniz.
    * Anlık geri yüklemenin anlık görüntülerini bir ila beş gün arasında tutabilirsiniz. Varsayılan ayar iki gün olur.
4. **Bekletme aralığı**' nda, günlük veya haftalık yedekleme noktalarınızı ne kadar süreyle saklamak istediğinizi belirtin.
5. **Aylık yedekleme noktasını bekletmek**için günlük veya haftalık yedeklemelerinizin aylık bir yedeklemesini korumak isteyip istemediğinizi belirtin.
6. İlkeyi kaydetmek için **Tamam** ' ı tıklatın.

    ![Yeni yedekleme ilkesi](./media/backup-azure-arm-vms-prepare/new-policy.png)

> [!NOTE]
   > Azure Backup, Azure VM yedeklemeleri için günışığından yararlanma değişiklikleri için otomatik saat ayarlamasını desteklemez. Zaman değişikliği gerçekleştiğinde, yedekleme ilkelerini gerektiği şekilde el ile değiştirin.

## <a name="trigger-the-initial-backup"></a>İlk yedeklemeyi tetikleme

İlk yedekleme zamanlamaya uygun olarak çalışır, ancak bunu hemen aşağıdaki gibi çalıştırabilirsiniz:

1. Kasa menüsünde, **yedekleme öğeleri**' ne tıklayın.
2. **Yedekleme öğeleri**' nde **Azure sanal makine**' ye tıklayın.
3. **Yedekleme öğeleri** listesinde üç noktaya (...) tıklayın.
4. **Şimdi Yedekle**'ye tıklayın.
5. **Şimdi Yedekle**' de, kurtarma noktasının tutulacağı son günü seçmek için Takvim denetimini kullanın. Daha sonra, **Tamam**'a tıklayın.
6. Portal bildirimlerini izleyin. İş ilerlemesini kasa panosunda izleyebilirsiniz > **yedekleme işleri** > **devam**ediyor. VM’nizin boyutuna bağlı olarak, ilk yedeklemenin oluşturulması biraz zaman alabilir.

## <a name="verify-backup-job-status"></a>Yedekleme işinin durumunu doğrulama

Her VM yedeklemesinin yedekleme işi ayrıntıları iki aşamadan oluşur, **anlık görüntü** aşaması ve ardından **verileri kasaya aktar** aşamasına gelir.<br/>
Anlık görüntü aşaması, **anlık geri yüklemeler** için disklerle birlikte depolanan bir kurtarma noktasının kullanılabilirliğini garanti eder ve Kullanıcı tarafından yapılandırılan anlık görüntü bekletmesine bağlı olarak en fazla beş gün boyunca kullanılabilir. Verileri kasaya aktarma, uzun süreli saklama için kasada bir kurtarma noktası oluşturur. Verileri kasaya aktarma işlemi yalnızca anlık görüntü aşaması tamamlandıktan sonra başlar.

  ![Yedekleme Işi durumu](./media/backup-azure-arm-vms-prepare/backup-job-status.png)

Arka uçta çalışan, **yedekleme işi** ayrıntıları dikey penceresinden aşağıda verilen şekilde denetlenen bir ön uç yedekleme işi olan Iki **alt görev** vardır:

  ![Yedekleme Işi durumu](./media/backup-azure-arm-vms-prepare/backup-job-phase.png)

**Verileri kasaya aktar** aşamasına kadar, disklerin boyutuna, disk başına dalgalanma ve diğer birçok etkene göre tamamlanması çok gün sürebilir.

İş durumu, aşağıdaki senaryolara bağlı olarak değişebilir:

**Görüntüye** | **Verileri kasaya aktar** | **İş durumu**
--- | --- | ---
Tamamlandı | Devam ediyor | Devam ediyor
Tamamlandı | Atlandı | Tamamlandı
Tamamlandı | Tamamlandı | Tamamlandı
Tamamlandı | Başarısız | Uyarıyla tamamlandı
Başarısız | Başarısız | Başarısız

Artık bu özellik ile aynı VM için iki yedek paralel çalışabilir, ancak her iki aşamada (anlık görüntü, verileri kasaya aktar) yalnızca bir alt görev çalışıyor olabilir. Bu nedenle, senaryolarda devam eden bir yedekleme işi olduğundan, bu ayırma işleviyle birlikte yedeklemenin başarısız olmasına neden olur. Sonraki günün yedeklemeleri, daha önceki bir günün yedekleme işi devam ediyorsa, **kasaya veri aktarımı** atlanırken anlık görüntü tamamlanabilir.
Kasada oluşturulan artımlı kurtarma noktası, kasada oluşturulan son kurtarma noktasındaki tüm karmaşıklığı yakalar. Kullanıcı üzerinde bir ücret etkisi yoktur.

## <a name="optional-steps"></a>İsteğe bağlı adımlar

### <a name="install-the-vm-agent"></a>VM aracısını yükler

Azure Backup, makinede çalışan Azure VM aracısına bir uzantı yükleyerek Azure VM 'lerini yedekler. VM 'niz bir Azure Marketi görüntüsünden oluşturulduysa, aracı yüklenir ve çalışır. Özel bir sanal makine oluşturursanız veya şirket içi bir makineyi geçirirseniz, aracıyı tabloda özetlenen şekilde el ile yüklemeniz gerekebilir.

**'Nın** | **Ayrıntılar**
--- | ---
**Windows** | 1. aracı MSI dosyasını [indirip yükleyin](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) .<br/><br/> 2. makinede yönetici izinleriyle Install.<br/><br/> 3. yüklemeyi doğrulayın. VM 'deki *C:\windowsazure\packages* ' de, **Waappagent. exe** > **Özellikler**' e sağ tıklayın. **Ayrıntılar** sekmesinde **ürün sürümü** 2.6.1198.718 veya üzeri olmalıdır.<br/><br/> Aracıyı güncelleştiriyorsanız, hiçbir yedekleme işlemi olmadığından emin olun ve [aracıyı yeniden yükleyin](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409).
**Linux** | Dağıtım paketi deposundan bir RPM veya bir DEB paketini kullanarak uygulamasını yükler. Bu, Azure Linux aracısını yüklemek ve yükseltmek için tercih edilen yöntemdir. Tüm [onaylı dağıtım sağlayıcıları](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) , Azure Linux Aracısı paketini görüntülerle ve depolarında tümleştirin. Aracı [GitHub](https://github.com/Azure/WALinuxAgent)'da kullanılabilir ancak buradan yüklemeyi önermiyoruz.<br/><br/> Aracıyı güncelleştiriyorsanız, yedekleme işlemlerinin çalışmakta olmadığından emin olun ve ikili dosyaları güncelleştirin.

>[!NOTE]
> **Azure Backup artık, Azure sanal makine yedekleme çözümünü kullanarak Seçmeli disk yedeklemesini ve geri yüklemeyi desteklemektedir.**
>
>Günümüzde, sanal makine yedekleme çözümünü kullanarak bir VM 'deki tüm disklerin (Işletim sistemi ve veri) yedeklenmesini destekler Azure Backup. Disk dışlama işlevselliğiyle, bir VM 'deki birçok veri diskinden bir veya birkaçını yedekleme seçeneği alırsınız. Bu, yedekleme ve geri yükleme gereksinimleriniz için verimli ve ekonomik bir çözüm sunar. Her kurtarma noktası, yedekleme işlemine dahil edilen disklerin verilerini içerir. Bu, geri yükleme işlemi sırasında verilen kurtarma noktasından geri yüklenen disklerin bir alt kümesine sahip etmenize olanak tanır. Bu, hem anlık görüntüden hem de kasadan geri yükleme için geçerlidir.
>
>**Önizlemeye kaydolmak için AskAzureBackupTeam@microsoft.com adresinden bize yazın**

## <a name="next-steps"></a>Sonraki adımlar

* [Azure VM aracılarıyla](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md) veya [Azure VM yedeklemeyle](backup-azure-vms-troubleshoot.md)ilgili sorunları giderin.
* [Geri yükle](backup-azure-arm-restore-vms.md) Azure VM 'Leri.
