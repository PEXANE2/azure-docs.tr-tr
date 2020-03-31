---
title: Kurtarma Hizmetleri kasasında Azure VM'leri yedekleme
description: Azure Yedekleme'yi kullanarak Kurtarma Hizmetleri kasasında Azure VM'leri nasıl yedeklenenizi açıklar
ms.topic: conceptual
ms.date: 04/03/2019
ms.openlocfilehash: aeadd7bc798f690c67eef38c6dc645204ff39115
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79273520"
---
# <a name="back-up-azure-vms-in-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasasında Azure VM'leri yedekleme

Bu makalede, [Azure Yedekleme](backup-overview.md) hizmetini kullanarak Bir Kurtarma Hizmetleri kasasında Azure VM'leri nasıl yedeklenenize açıklanmaktadır.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
>
> * Azure VM'leri hazırlayın.
> * Bir kasa oluşturun.
> * VM'leri keşfedin ve bir yedekleme ilkesi yapılandırın.
> * Azure VM'leri için yedeklemeyi etkinleştirin.
> * İlk yedeklemeyi çalıştırın.

> [!NOTE]
> Bu makalede, bir kasa nın nasıl ayarlanır ve yedeklene bilmek için VM'leri seçilir. Birden çok VM yedeklemek istiyorsanız yararlıdır. Alternatif olarak, [tek bir Azure VM'yi](backup-azure-vms-first-look-arm.md) doğrudan VM ayarlarından yedekleyebilirsiniz.

## <a name="before-you-start"></a>Başlamadan önce

* Azure VM yedekleme mimarisini [gözden geçirin.](backup-architecture.md#architecture-built-in-azure-vm-backup)
* [Hakkında bilgi edinin](backup-azure-vms-introduction.md) Azure VM yedekleme ve yedekleme uzantısı.
* Yedeklemeyi yapılandırmadan önce [destek matrisini gözden geçirin.](backup-support-matrix-iaas.md)

Buna ek olarak, bazı durumlarda yapmanız gereken birkaç şey vardır:

* **VM'deki VM aracısını yükleyin**: Azure Yedekleme, makinede çalışan Azure VM aracısına bir uzantı yükleyerek Azure VM'leri yedekler. VM'niz Bir Azure pazar yeri görüntüsünden oluşturulduysa, aracı yüklenir ve çalışır. Özel bir VM oluşturursanız veya şirket içi bir makineyi geçiştiriyorsanız, [aracıyı el ile yüklemeniz](#install-the-vm-agent)gerekebilir.

## <a name="create-a-vault"></a>Kasa oluşturma

 Kasa, zaman içinde oluşturulan yedeklemeleri ve kurtarma noktalarını depolar ve yedeklenen makinelerle ilişkili yedekleme ilkelerini depolar. Aşağıdaki gibi bir kasa oluşturun:

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Aramada, **Kurtarma Hizmetleri**yazın. **Hizmetler**altında, **Kurtarma Hizmetleri kasalarını**tıklatın.

     ![Kurtarma Hizmetleri kasalarını ara](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png)

3. **Kurtarma Hizmetleri kasaları** menüsünde **+Ekle'yi**tıklatın.

     ![Kurtarma Hizmetleri Kasası oluşturma 2. adım](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

4. **Kurtarma Hizmetleri kasasında,** tonozu tanımlamak için dostça bir ad yazın.
    * Adın Azure aboneliği için benzersiz olması gerekir.
    * 2 ila 50 karakter içerebilir.
    * Bir harfle başlamalı ve yalnızca harfler, sayılar ve tireler içerebilir.
5. Kasanın oluşturulması gereken Azure aboneliğini, kaynak grubunu ve coğrafi bölgeyi seçin. Sonra **Oluştur**’a tıklayın.
    * Kasanın oluşturulması biraz zaman alabilir.
    * Portalın sağ üst alanında durum bildirimlerini izleyin.

Kasa oluşturulduktan sonra, Kurtarma Hizmetleri kasaları listesinde görünür. Kasanızı **görmüyorsanız, Yenile'yi**seçin.

![Yedekleme kasalarının listesi](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

>[!NOTE]
> Azure Yedekleme artık Azure Yedekleme hizmeti tarafından oluşturulan kaynak grup adının özelleştirilmesine olanak tanır. Daha fazla bilgi için [Sanal Makineler için Azure Yedekleme kaynak grubuna](backup-during-vm-creation.md#azure-backup-resource-group-for-virtual-machines)bakın.

### <a name="modify-storage-replication"></a>Depolama çoğaltma değiştirme

Varsayılan olarak, kasalar [coğrafi yedekli depolama (GRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs)kullanır.

* Kasa birincil yedekleme mekanizmanızsa, GRS kullanmanızı öneririz.
* Daha ucuz bir seçenek için [yerel olarak yedekli depolama (LRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) kullanabilirsiniz.

Depolama çoğaltma türünü aşağıdaki gibi değiştirin:

1. Yeni kasada **Ayarlar** **bölümündeki Özellikler'i** tıklatın.
2. **Özellikler'de**, **Yedekleme Yapılandırması**altında, **Güncelleştir'i**tıklatın.
3. Depolama çoğaltma türünü seçin ve **Kaydet'i**tıklatın.

      ![Yeni kasa için depolama yapılandırması ayarlama](./media/backup-try-azure-backup-in-10-mins/full-blade.png)

> [!NOTE]
   > Kasa ayarlandıktan ve yedek öğeler içeren depolama çoğaltma türünü değiştiremezsiniz. Bunu yapmak istiyorsan kasayı yeniden oluşturman gerekiyor.

## <a name="apply-a-backup-policy"></a>Yedekleme ilkesi uygulama

Kasa için bir yedekleme ilkesi ni yapılandırın.

1. Kasada, **Genel Bakış** bölümünde **+Yedekleme'yi** tıklatın.

   ![Yedekleme düğmesi](./media/backup-azure-arm-vms-prepare/backup-button.png)

2. **Yedekleme Hedefi'nde** > **iş yükünüz nerede çalışıyor?** **Azure** **Virtual machine** >  **OK** **Ne yedeklemek istiyorsunuz?** Bu kasadaki VM uzantısını kaydeder.

   ![Yedekleme ve Yedekleme Hedefi bölmeleri](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

3. **Yedekleme ilkesinde,** kasayla ilişkilendirmek istediğiniz ilkeyi seçin.
    * Varsayılan ilke, VM'yi günde bir kez yedekler. Günlük yedeklemeler 30 gün boyunca saklanır. Anlık kurtarma anlık görüntüleri iki gün boyunca saklanır.
    * Varsayılan ilkeyi kullanmak istemiyorsanız, **Yeni Oluştur'u**seçin ve bir sonraki yordamda açıklandığı gibi özel bir ilke oluşturun.

      ![Varsayılan yedekleme ilkesi](./media/backup-azure-arm-vms-prepare/default-policy.png)

4. **Sanal makineleri seç'te,** ilkeyi kullanarak yedeklemek istediğiniz Sanal Makineler'i seçin. Ardından **Tamam**'a tıklayın.

   * Seçili VM'ler doğrulanır.
   * VM'leri yalnızca kasayla aynı bölgede seçebilirsiniz.
   * VM'ler yalnızca tek bir kasada yedeklenebilir.

     !["Sanal makineleri seçin" bölmesi](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

5. **Yedekleme'de**, **Yedeklemeyi Etkinleştir'i**tıklatın. Bu işlem, ilkeyi kasaya ve VM'lere dağır ve Yedekleme uzantısını Azure VM üzerinde çalışan VM aracısına yükler.

     !["Yedeklemeyi etkinleştir" düğmesi](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Yedeklemeyi etkinleştirdikten sonra:

* Yedekleme hizmeti, VM çalışıyor olsun veya olmasın yedekleme uzantısını yükler.
* İlk yedekleme, yedekleme zamanlamanıza uygun olarak çalışır.
* Yedeklemeler çalışırken şunları unutmayın:
  * Çalışan bir VM, uygulama tutarlı bir kurtarma noktasını yakalamak için en yüksek şansa sahiptir.
  * Ancak, VM kapalı olsa bile yedeklenir. Böyle bir VM çevrimdışı VM olarak bilinir. Bu durumda, kurtarma noktası kilitlenme tutarlı olacaktır.
* Azure VM'lerinin yedekalınmasına izin vermek için açık giden bağlantı gerekmez.

### <a name="create-a-custom-policy"></a>Özel ilke oluşturma

Yeni bir yedekleme ilkesi oluşturmayı seçtiyseniz, ilke ayarlarını doldurun.

1. **İlke adına**anlamlı bir ad belirtin.
2. **Yedekleme zamanlamasında,** yedeklemelerin ne zaman alınması gerektiğini belirtin. Azure VM'ler için günlük veya haftalık yedeklemeler alabilirsiniz.
3. **Anında Geri Yükleme'de**anlık görüntüleri anında geri yükleme için yerel olarak ne kadar süreyle saklamak istediğinizi belirtin.
    * Geri yüklediğinizde, yedeklenen VM diskleri depolamadan ağ üzerinden kurtarma depolama konumuna kopyalanır. Anında geri yükleme ile, yedekleme işi sırasında alınan yerel olarak depolanan anlık görüntülerden, yedekleme verilerinin kasaya aktarılmasını beklemeden yararlanabilirsiniz.
    * Anlık görüntüleri bir ila beş gün arasında anında geri yükleme için saklayabilirsiniz. İki gün varsayılan ayardır.
4. **Bekletme aralığında,** günlük veya haftalık yedekleme noktalarınızı ne kadar süreyle tutmak istediğinizi belirtin.
5. **Aylık yedekleme noktasının bekletinmesinde,** günlük veya haftalık yedeklemelerinizin aylık yedeklemesini tutmak isteyip istemediğinizbelirtilir.
6. İlkeyi kaydetmek için **Tamam'ı** tıklatın.

    ![Yeni yedekleme ilkesi](./media/backup-azure-arm-vms-prepare/new-policy.png)

> [!NOTE]
   > Azure Yedekleme, Azure VM yedeklemeleri için gün ışığından tasarruf sağlayan değişiklikler için otomatik saat ayarlamayı desteklemez. Saat değişiklikleri gerçekleştiğinde, yedekleme ilkelerini gerektiği gibi el ile değiştirin.

## <a name="trigger-the-initial-backup"></a>İlk yedeklemeyi tetikle

İlk yedekleme zamanlamaya uygun olarak çalışır, ancak hemen aşağıdaki gibi çalıştırabilirsiniz:

1. Kasa menüsünde Yedek **öğeleri**tıklatın.
2. **Yedekleme Öğeleri'nde**Azure **Sanal Makine'yi**tıklatın.
3. Yedek **Öğeler** listesinde elipsleri (...) tıklatın.
4. **Şimdi Yedek'i**tıklatın.
5. **Yedekleme Şimdi,** kurtarma noktası nın korunması gereken son günü seçmek için takvim denetimini kullanın. Ardından **Tamam**'a tıklayın.
6. Portal bildirimlerini izleyin. Devam**Eden** **Yedekleme İşleri** > > kasa panosundaki iş ilerlemesini izleyebilirsiniz. VM’nizin boyutuna bağlı olarak, ilk yedeklemenin oluşturulması biraz zaman alabilir.

## <a name="verify-backup-job-status"></a>Yedekleme iş durumunu doğrula

Her VM yedeklemesi için Yedekleme iş ayrıntıları iki aşamadan oluşur, **Anlık Görüntü** aşaması ve ardından **veri tonoz aşamasına aktar.**<br/>
Anlık görüntü aşaması, **Anında Geri Yüklemeler** için disklerle birlikte depolanan bir kurtarma noktasının kullanılabilirliğini garanti eder ve kullanıcı tarafından yapılandırılan anlık görüntü tutma durumuna bağlı olarak en fazla beş gün kullanılabilir. Verileri kasaya aktarmak, uzun süreli saklama için kasada bir kurtarma noktası oluşturur. Verileri kasaya aktarma yalnızca anlık görüntü aşaması tamamlandıktan sonra başlar.

  ![Yedekleme İş Durumu](./media/backup-azure-arm-vms-prepare/backup-job-status.png)

Arka uçta çalışan iki **Alt Görev** vardır, biri aşağıda verilen **Yedekleme İş** ayrıntıları bıçaktan denetlenebilir ön uç yedekleme işi için:

  ![Yedekleme İş Durumu](./media/backup-azure-arm-vms-prepare/backup-job-phase.png)

**Tonoz aşamasına veri aktarımı,** disklerin boyutuna, disk başına karmaşaya ve diğer çeşitli etkenlere bağlı olarak tamamlanması birkaç gün sürebilir.

İş durumu aşağıdaki senaryolara bağlı olarak değişebilir:

**Anlık Görüntü** | **Verileri kasaya aktarma** | **İş Durumu**
--- | --- | ---
Tamamlandı | Devam ediyor | Devam ediyor
Tamamlandı | Atlandı | Tamamlandı
Tamamlandı | Tamamlandı | Tamamlandı
Tamamlandı | Başarısız | Uyarı ile tamamlandı
Başarısız | Başarısız | Başarısız

Şimdi bu yeteneği ile, aynı VM için, iki yedek paralel olarak çalıştırabilirsiniz, ancak her iki aşamada (anlık görüntü, tonoz veri aktarımı) sadece bir alt görev çalışıyor olabilir. Yani senaryolarda devam eden bir yedekleme işi bu ayrıştırma işlevi ile önlenecektir başarısız ertesi gün yedekleme sonuçlandı. Önceki günün yedekleme durumu devam ediyorsa, **verileri kasaya aktarırken** sonraki günün yedekleri anlık görüntü tamamlanmış olabilir.
Kasada oluşturulan artımlı kurtarma noktası, kasada oluşturulan son kurtarma noktasından tüm karmaşayı yakalar. Kullanıcı üzerinde herhangi bir maliyet etkisi yoktur.

## <a name="optional-steps"></a>İsteğe bağlı adımlar

### <a name="install-the-vm-agent"></a>VM aracısını yükleme

Azure Yedekleme, makinede çalışan Azure VM aracısına bir uzantı yükleyerek Azure VM'leri yedekler. VM'niz bir Azure Marketi görüntüsünden oluşturulduysa, aracı yüklenir ve çalışır. Özel bir VM oluşturursanız veya şirket içi bir makineyi geçiş tirseniz, tabloda özetlendiği gibi aracıyı el ile yüklemeniz gerekebilir.

**Vm** | **Şey**
--- | ---
**Windows** | 1. Aracı MSI dosyasını [indirin ve kurun.](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)<br/><br/> 2. Makineye yönetici izinleri ile yükleyin.<br/><br/> 3. Yüklemeyi doğrulayın. VM'deki *C:\WindowsAzure\Paketleri'nde* **WaAppAgent.exe** > **Properties'e**sağ tıklayın. **Ayrıntılar** **sekmesinde, Ürün Sürümü** 2.6.1198.718 veya daha yüksek olmalıdır.<br/><br/> Aracıyı güncelliyorsanız, yedekleme işlemlerinin çalışmadığından emin olun ve [aracıyı yeniden yükleyin.](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)
**Linux** | Dağıtımınızın paket deposundan bir RPM veya DEB paketi kullanarak yükleyin. Azure Linux aracısını yüklemek ve yükseltmek için tercih edilen yöntem budur. Onaylanan tüm [dağıtım sağlayıcıları,](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) Azure Linux aracı paketini görüntülerine ve depolarına entegre ederler. Aracı [GitHub'da](https://github.com/Azure/WALinuxAgent)kullanılabilir, ancak buradan yüklemenizi önermiyoruz.<br/><br/> Aracıyı güncelliyorsanız, yedekleme işlemlerinin çalışmadığından emin olun ve ikili leri güncelleştirin.

>[!NOTE]
> **Azure Yedekleme artık seçici disk yedeklemesini destekler ve Azure Sanal Makine yedekleme çözümlerini kullanarak geri yüklemeyi destekler.**
>
>Bugün Azure Yedekleme, Sanal Makine yedekleme çözümlerini kullanarak VM'deki tüm diskleri (İşletim Sistemi ve verileri) birlikte yedeklemeyi destekler. Diski dışlama işleviyle, VM'deki birçok veri diskinden bir veya birkaçını yedekleme seçeneğine sahip olursunuz. Bu, yedekleme ve geri yükleme gereksinimleriniz için verimli ve uygun maliyetli bir çözüm sağlar. Her kurtarma noktası yedekleme işlemine dahil edilen disklerin verilerini içerir ve bu da geri yükleme işlemi sırasında verilen kurtarma noktasından geri yüklenen disklerin bir alt kümesini geri yüklemenize olanak tanır. Bu, hem anlık görüntüden hem de kasadan geri yüklemek için geçerlidir.
>
>**Önizleme için kaydolmak için, bize yazınAskAzureBackupTeam@microsoft.com**

## <a name="next-steps"></a>Sonraki adımlar

* [Azure VM aracıları](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md) veya [Azure VM yedeklemesi](backup-azure-vms-troubleshoot.md)ile ilgili sorunları giderin.
* [Geri Yükleme](backup-azure-arm-restore-vms.md) Azure VM'ler.
