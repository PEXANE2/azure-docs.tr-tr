---
title: Windows sistem durumunu Azure 'a yedekleme
description: Windows Server bilgisayarlarının sistem durumunu Azure 'a nasıl yedekleyeceğinizi öğrenin.
ms.topic: conceptual
ms.date: 05/23/2018
ms.openlocfilehash: 10d37bbafab6aa96986a5d7af88a509d4c8eac25
ms.sourcegitcommit: 3246e278d094f0ae435c2393ebf278914ec7b97b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89376501"
---
# <a name="back-up-windows-system-state-to-azure"></a>Windows sistem durumunu Azure 'a yedekleme

Bu makalede, Windows Server sistem eyaletinizi Azure 'a nasıl yedekleyeceğiniz açıklanmaktadır. Temel bilgiler konusunda size kılavuzluk etmek üzere tasarlanmıştır.

Azure Backup hakkında daha fazla bilgi edinmek istiyorsanız bu [genel bakışı](backup-overview.md) okuyun.

Azure aboneliğiniz yoksa istediğiniz Azure hizmetine erişmenizi sağlayan [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="set-storage-redundancy-for-the-vault"></a>Kasa için depolama artıklığı ayarlama

Kurtarma Hizmetleri kasası oluşturduğunuzda, depolama yedekliliğinin istediğiniz şekilde yapılandırıldığından emin olun.

1. **Kurtarma Hizmetleri kasaları** bölmesinden yeni kasayı seçin.

    ![Kurtarma Hizmetleri kasası listesinden yeni kasayı seçin](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

    Kasayı seçtiğinizde, **Kurtarma Hizmetleri Kasası** bölmesi daraltır ve ayarlar bölmesi (*en üstte kasanın adı bulunur*) ve kasa ayrıntıları bölmesi açılır.

    ![Yeni kasa için depolama yapılandırmasını görüntüleme](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)
2. Yeni kasasının ayarlar bölmesinde, dikey slaydı kullanarak Yönet bölümüne gidin ve **Yedekleme altyapısı**' nı seçin.
    Yedekleme altyapısı bölmesi açılır.
3. Yedekleme altyapısı bölmesinde **yedekleme yapılandırması ' nı seçerek** **yedekleme yapılandırması** bölmesini açın.

    ![Yeni kasa için depolama yapılandırması ayarlama](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)
4. Kasanız için uygun depolama çoğaltma seçeneğini belirleyin.

    ![Depolama yapılandırma seçenekleri](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    Varsayılan olarak, kasanız coğrafi olarak yedekli depolamaya sahiptir. Azure'ı birincil yedek depolama uç noktası olarak kullanıyorsanız, **Coğrafi olarak yedekli** seçeneğini kullanmaya devam edin. Azure’u birincil yedek depolama uç noktası olarak kullanmıyorsanız, Azure depolama maliyetlerini azaltan **Yerel olarak yedekli** seçeneğini belirleyin. [Coğrafi olarak yedekli](../storage/common/storage-redundancy.md) ve [yerel olarak yedekli](../storage/common/storage-redundancy.md) depolama seçenekleri hakkında daha fazla bilgiyi [Depolama yedekliliğine genel bakış](../storage/common/storage-redundancy.md) bölümünden edinebilirsiniz.

Artık bir kasa oluşturduğunuza göre, Windows sistem durumunu yedeklemek için yapılandırın.

## <a name="configure-the-vault"></a>Kasa yapılandırma

1. Kurtarma Hizmetleri Kasası bölmesinde (yeni oluşturduğunuz kasa için), Başlarken bölümünde **Yedekle**' yi seçin ve sonra **yedeklemeyi kullanmaya** başlama bölmesinde yedekleme **hedefi**' ni seçin.

    ![Yedekleme ayarlarını açın](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

    **Yedekleme hedefi** bölmesi açılır.

    ![Yedekleme hedefi bölmesini Aç](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. **İş yükünüz nerede çalışıyor?** açılır menüsünden **Şirket içi**’ni seçin.

    Windows Server veya Windows Bilgisayarınız Azure 'da olmayan bir fiziksel makine olduğundan **Şirket içi** seçeneğini belirleyin.

3. **Neleri yedeklemek istiyorsunuz?** menüsünde, **sistem durumu**' nu seçin ve **Tamam**' ı seçin.

    ![Dosya ve klasörleri yedekleme](./media/backup-azure-system-state/backup-goal-system-state.png)

    **Tamam**' ı seçtikten sonra, **yedekleme hedefi**' nin yanında bir onay Işareti görünür ve **altyapıyı hazırla** bölmesi açılır.

    ![Yedekleme hedefi yapılandırılmıştır, bundan sonra altyapıyı hazırlayın](./media/backup-try-azure-backup-in-10-mins/backup-goal-configed.png)

4. **Altyapıyı hazırla** bölmesinde, **Windows Server veya Windows Istemcisi için aracıyı indir**' i seçin.

    ![Altyapıyı hazırla](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

    Windows Server 'ı temel kullanıyorsanız, Windows Server için aracıyı yüklemeyi seçin. Açılır menü, MARSAgentInstaller.exe dosyasını çalıştırma veya kaydetme seçeneğini sunar.

    ![MARSAgentInstaller iletişim kutusu](./media/backup-try-azure-backup-in-10-mins/mars-installer-run-save.png)

5. İndirme açılan menüsünde **Kaydet**' i seçin.

    Varsayılan olarak, **MARSagentinstaller.exe** dosyası İndirilenler klasörünüze kaydedilir. Yükleyici tamamlandığında, yükleyiciyi çalıştırmak veya klasörü açmak isteyip istemediğinizi soran bir açılır pencere görürsünüz.

    ![MARS yükleyicisi tamamlanmıştır](./media/backup-try-azure-backup-in-10-mins/mars-installer-complete.png)

    Aracıyı yüklemeniz henüz gerekli değildir. Kasa kimlik bilgilerini indirdikten sonra aracıyı yükleyebilirsiniz.

6. **Altyapıyı hazırla** bölmesinde **İndir**' i seçin.

    ![kasa kimlik bilgilerini indirme](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

    Kasa kimlik bilgileri, **indirmeler** klasörünüze indirilir. Kasa kimlik bilgileri indirmeyi tamamladıktan sonra, kimlik bilgilerini açmak veya kaydetmek isteyip istemediğinizi soran bir açılır pencere görürsünüz. **Kaydet**’i seçin. Yanlışlıkla **Aç**' ı seçerseniz, kasa kimlik bilgilerini açmaya yönelik iletişim kutusunun başarısız olmasına izin verin. Kasa kimlik bilgilerini açamazsınız. Sonraki adımla devam edin. Kasa kimlik bilgileri **indirmeler** klasöründedir.

    ![kasa kimlik bilgilerini indirme tamamlandı](./media/backup-try-azure-backup-in-10-mins/vault-credentials-downloaded.png)
   > [!NOTE]
   > Kasa kimlik bilgilerinin yalnızca aracıyı kullanmayı düşündüğünüz Windows Server 'da yerel bir konuma kaydedilmesi gerekir.
   >

[!INCLUDE [backup-upgrade-mars-agent.md](../../includes/backup-upgrade-mars-agent.md)]

## <a name="install-and-register-the-agent"></a>Aracıyı yükleme ve kaydetme

> [!NOTE]
> Azure portal aracılığıyla yedeklemenin etkinleştirilmesi mümkün değildir. Windows Server sistem durumunu yedeklemek için Microsoft Azure Kurtarma Hizmetleri aracısını kullanın.
>

1. İndirilenler klasöründen (veya diğer kayıtlı konumdan) **MARSagentinstaller.exe** dosyasını bulun ve dosyaya çift tıklayın.

    Yükleyici, Kurtarma Hizmetleri aracısı ayıklama, yükleme ve kaydetme sırasında bir dizi ileti sunar.

    ![Kurtarma Hizmetleri aracısı yükleyici kimlik bilgilerini çalıştırma](./media/backup-try-azure-backup-in-10-mins/mars-installer-registration.png)

2. Microsoft Azure Kurtarma Hizmetleri Aracısı Kurulum Sihirbazı'nı tamamlayın. Sihirbazı tamamlamak için şunları yapmanız gerekir:

   * Yükleme ve önbellek klasörü için bir konum seçin.
   * İnternet'e bağlanmak için bir ara sunucu kullanıyorsanız ara sunucu bilgilerinizi sağlayın.
   * Kimliği doğrulanmış bir ara sunucu kullanıyorsanız kullanıcı adı ve parola bilgilerinizi sağlayın.
   * İndirilen kasa kimlik bilgilerini sağlayın
   * Şifreleme parolasını güvenli bir konuma kaydedin.

     > [!NOTE]
     > Parolayı kaybeder veya unutursanız, Microsoft, yedekleme verilerini kurtarmaya yardımcı olamaz. Dosyayı güvenli bir konuma kaydedin. Bir yedeklemeyi geri yüklemek gereklidir.
     >
     >

Aracı artık yüklenmiş ve makineniz kasaya kaydedilmiştir. Yedeklemenizi yapılandırıp zamanlamak için hazırsınız.

## <a name="back-up-windows-server-system-state"></a>Windows Server Sistem Durumunu yedekleme

İlk yedekleme iki görevi içerir:

* Yedeklemeyi zamanlama
* Sistem durumunu ilk kez yedekleme

İlk yedeklemeyi tamamlamak için Microsoft Azure Kurtarma Hizmetleri aracısını kullanın.

> [!NOTE]
> Windows Server 2008 R2 'de sistem durumu ' nu Windows Server 2016 ' de yedekleyebilirsiniz. Sistem durumu yedeklemesi, istemci SKU 'Larında desteklenmez. Sistem durumu, Windows istemcileri veya Windows Server 2008 SP2 makineleri için bir seçenek olarak gösterilmez.
>
>

### <a name="to-schedule-the-backup-job"></a>Yedekleme işini zamanlamak için

1. Microsoft Azure Kurtarma Hizmetleri aracısını açın. Bunu, makinenizde **Microsoft Azure Backup** aramasını yaparak bulabilirsiniz.

    ![Azure Kurtarma Hizmetleri aracısını başlatma](./media/backup-try-azure-backup-in-10-mins/snap-in-search.png)

2. Kurtarma Hizmetleri aracısında, **yedeklemeyi zamanla**' yı seçin.

    ![Windows Server yedeklemesini zamanlama](./media/backup-try-azure-backup-in-10-mins/schedule-first-backup.png)

3. Yedekleme zamanlaması sihirbazının **Başlarken** sayfasında **İleri**' yi seçin.

4. **Yedeklenecek öğeleri seçin** sayfasında **öğe Ekle**' yi seçin.

5. **Sistem durumu** ' nu seçin ve ardından **Tamam**' ı seçin.

6. **İleri**’yi seçin.

7. Sonraki sayfalarda sistem durumu yedeklemeleriniz için gerekli yedekleme sıklığını ve bekletme ilkesini seçin.

8. Onay sayfasında, bilgileri gözden geçirin ve ardından **son**' u seçin.

9. Sihirbaz Yedekleme zamanlamasını oluşturmayı tamamladıktan sonra **Kapat**' ı seçin.

### <a name="to-back-up-windows-server-system-state-for-the-first-time"></a>Windows Server sistem durumunu ilk kez yedeklemek için

1. Windows Server için yeniden başlatma gerektiren bekleyen güncelleştirmeler bulunmadığından emin olun.

2. Kurtarma Hizmetleri aracısında, ağ üzerinden ilk dengeli dağıtımı gerçekleştirmek için **Şimdi Yedekle** ' yi seçin.

    ![Windows Server Yedekleme şimdi](./media/backup-try-azure-backup-in-10-mins/backup-now.png)

3. Görüntülenen **yedekleme öğesi seçin** ekranında **sistem durumu** ' nu seçin ve ileri ' **yi**seçin.

4. Onay sayfasında, Şimdi Yedekle Sihirbazı'nın makineyi yedeklemek için kullanacağı ayarları gözden geçirin. Ardından **Yedekle**' yi seçin.

5. Sihirbazı kapatmak için **Kapat** ' ı seçin. Yedekleme işlemi tamamlanmadan önce sihirbazı kapatırsanız, sihirbaz arka planda çalışmaya devam eder.
    > [!NOTE]
    > MARS Aracısı, `SFC /verifyonly` her sistem durumu yedeklemesinden önce ön denetimlerin parçası olarak tetiklenir. Bu, sistem durumunun bir parçası olarak Yedeklenen dosyaların Windows sürümüne karşılık gelen doğru sürümlere sahip olduğundan emin olmak için gereklidir. [Bu makalede](/windows-server/administration/windows-commands/sfc)sistem dosyası DENETLEYICISI (Sfc) hakkında daha fazla bilgi edinin.
    >

İlk yedekleme tamamlandıktan sonra, Yedekleme konsolunda **İş tamamlandı** durumu görünür.

  ![IR tamamlandı](./media/backup-try-azure-backup-in-10-mins/ircomplete.png)

## <a name="questions"></a>Sorularınız mı var?

Sorularınız varsa [bize geri bildirim gönderin](https://feedback.azure.com/forums/258995-azure-backup).

## <a name="next-steps"></a>Sonraki adımlar

* [Windows makinelerini yedekleme](backup-windows-with-mars-agent.md) konusunda daha fazla bilgi edinin.
* Windows Server sistem durumlarınızı yedeklediğinize göre, [kasalarınızı ve sunucularınızı yönetebilirsiniz](backup-azure-manage-windows-server.md).
* Bir yedeklemeyi geri yüklemeniz gerekirse [dosyaları bir Windows makinesine geri yüklemek](backup-azure-restore-windows-server.md) için bu makaleyi kullanın.
