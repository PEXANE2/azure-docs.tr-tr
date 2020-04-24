---
title: Windows sistem durumunu Azure 'a yedekleme
description: Windows Server ve/veya Windows bilgisayarlarının sistem durumunu Azure 'a yedeklemeyi öğrenin.
ms.reviewer: saurse
ms.topic: conceptual
ms.date: 05/23/2018
ms.openlocfilehash: 4089815f8f76d9868f8fa56f8b2eab3de89541d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "82128195"
---
# <a name="back-up-windows-system-state-in-resource-manager-deployment"></a>Kaynak Yöneticisi dağıtımında Windows sistem durumunu yedekleme

Bu makalede, Windows Server sistem eyaletinizi Azure 'a nasıl yedekleyeceğiniz açıklanmaktadır. Temel bilgiler konusunda size kılavuzluk etmek üzere tasarlanmıştır.

Azure Backup hakkında daha fazla bilgi edinmek istiyorsanız bu [genel bakışı](backup-overview.md) okuyun.

Azure aboneliğiniz yoksa istediğiniz Azure hizmetine erişmenizi sağlayan [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="create-a-recovery-services-vault"></a>Kurtarma hizmetleri kasası oluşturma

Windows Server sistem durumunu yedeklemek için, verileri depolamak istediğiniz bölgede bir kurtarma hizmetleri Kasası oluşturmanız gerekir. Ayrıca, depolama alanınızın nasıl çoğaltılmasını istediğinizi belirlemeniz gerekir.

### <a name="to-create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturmak için

1. Önceden yapmadıysanız Azure aboneliğinizi kullanarak [Azure portalında](https://portal.azure.com/) oturum açın.
2. Hub menüsünde **Tüm hizmetler**'e tıklayın ve kaynak listesinde **Kurtarma Hizmetleri** yazıp **Kurtarma Hizmetleri kasaları** seçeneğine tıklayın.

    ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-azure-system-state/open-rs-vault-list.png)

    Abonelikte kurtarma hizmetleri kasaları varsa kasalar listelenir.
3. **Kurtarma Hizmetleri kasaları** menüsünde **Ekle**'ye tıklayın.

    ![Kurtarma Hizmetleri Kasası oluşturma 2. adım](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    Kurtarma Hizmetleri kasası dikey penceresi açılır ve sizden bir **Ad**, **Abonelik**, **Kaynak Grubu** ve **Konum** sağlamanızı ister.

    ![Kurtarma Hizmetleri Kasası Oluşturma 3. adım](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. **Ad** alanına, kasayı tanımlayacak kolay bir ad girin. Adın Azure aboneliği için benzersiz olması gerekir. 2 ila 50 karakterden oluşan bir ad yazın. Ad bir harf ile başlamalıdır ve yalnızca harf, rakam ve kısa çizgi içerebilir.

5. **Abonelik** bölümündeki açılır menüyü kullanarak Azure aboneliğini seçin. Yalnızca bir abonelik kullanıyorsanız bu abonelik görüntülenir ve sonraki adıma atlayabilirsiniz. Hangi aboneliğin kullanılacağından emin değilseniz varsayılan (veya önerilen) aboneliği kullanın. Yalnızca kuruluş hesabınızın birden çok Azure aboneliği ile ilişkili olması durumunda birden çok seçenek olur.

6. **Kaynak grubu** bölümünde:

    * bir Kaynak grubu oluşturmak istiyorsanız, **Yeni oluştur**’u seçin.
    Veya
    * **Var olanı kullan**’ı seçin ve açılır menüyü kullanarak mevcut Kaynak gruplarının listesine bakın.

   Kaynak grupları hakkında eksiksiz bilgiler için bkz. [Azure Resource Manager’a genel bakış](../azure-resource-manager/management/overview.md).

7. Kasa için coğrafi bölgeyi seçmek üzere **Konum**'a tıklayın. Bu seçim, yedekleme verilerinizin gönderildiği coğrafi bölgeyi belirler.

8. Kurtarma Hizmetleri kasası dikey penceresinin alt kısmındaki **Oluştur**’a tıklayın.

    Kurtarma Hizmetleri kasasının oluşturulması birkaç dakika sürebilir. Portalın sağ üst kısmından durum bildirimlerini izleyin. Kasanız oluşturulduktan sonra Kurtarma Hizmetleri kasaları listesinde görünür. Birkaç dakika sonra kasayı görmezseniz **Yenile**’ye tıklayın.

    ![Yenile düğmesine tıklayın](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

    Kasanızı Kurtarma Hizmetleri kasaları listesinde gördükten sonra, depolama yedekliliğini ayarlamaya hazır olursunuz.

### <a name="set-storage-redundancy-for-the-vault"></a>Kasa için depolama artıklığı ayarlama

Kurtarma Hizmetleri kasası oluşturduğunuzda, depolama yedekliliğinin istediğiniz şekilde yapılandırıldığından emin olun.

1. **Kurtarma Hizmetleri kasaları** dikey penceresinden yeni kasaya tıklayın.

    ![Kurtarma Hizmetleri kasası listesinden yeni kasayı seçin](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

    Kasayı seçtiğinizde **Kurtarma Hizmetleri kasası** dikey penceresi daralır ve Ayarlar dikey penceresi (*en üstünde kasanın adı bulunur*) ve ile kasa ayrıntıları dikey penceresi açılır.

    ![Yeni kasa için depolama yapılandırmasını görüntüleme](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)
2. Yeni kasanın Ayarlar dikey penceresinde dikey kaydırma çubuğunu kullanarak Yönet bölümüne inin ve **Yedekleme Altyapısı**’na tıklayın.
    Yedekleme Altyapısı dikey penceresi açılır.
3. Yedekleme Altyapısı dikey penceresinde, **Yedekleme Yapılandırması**’na tıklayarak **Yedekleme Yapılandırması** dikey penceresini açın.

    ![Yeni kasa için depolama yapılandırması ayarlama](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)
4. Kasanız için uygun depolama çoğaltma seçeneğini belirleyin.

    ![depolama yapılandırması seçenekleri](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    Varsayılan olarak, kasanız coğrafi olarak yedekli depolamaya sahiptir. Azure'ı birincil yedek depolama uç noktası olarak kullanıyorsanız, **Coğrafi olarak yedekli** seçeneğini kullanmaya devam edin. Azure’u birincil yedek depolama uç noktası olarak kullanmıyorsanız, Azure depolama maliyetlerini azaltan **Yerel olarak yedekli** seçeneğini belirleyin. [Coğrafi olarak yedekli](../storage/common/storage-redundancy-grs.md) ve [yerel olarak yedekli](../storage/common/storage-redundancy-lrs.md) depolama seçenekleri hakkında daha fazla bilgiyi [Depolama yedekliliğine genel bakış](../storage/common/storage-redundancy.md) bölümünden edinebilirsiniz.

Artık bir kasa oluşturduğunuza göre, Windows sistem durumunu yedeklemek için yapılandırın.

## <a name="configure-the-vault"></a>Kasa yapılandırma

1. Kurtarma Hizmetleri kasası dikey penceresinin (yeni oluşturduğunuz kasa için) Başlarken bölümünde **Yedekle**’ye tıklayın, ardından **Yedeklemeye Başlama** dikey penceresinde **Yedekleme hedefi**’ne tıklayın.

    ![Yedekleme hedefi dikey penceresini açma](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

    **Yedekleme Hedefi** dikey penceresi açılır.

    ![Yedekleme hedefi dikey penceresini açma](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. **İş yükünüz nerede çalışıyor?** açılır menüsünden **Şirket içi**’ni seçin.

    Windows Server veya Windows bilgisayarınız Azure üzerinde olmayan fiziksel bir makine olduğu için **Şirket içi** seçeneğini belirlersiniz.

3. **Neleri yedeklemek istiyorsunuz?** menüsünde, **sistem durumu**' nu seçin ve **Tamam**' ı tıklatın.

    ![Dosya ve klasörleri yedekleme](./media/backup-azure-system-state/backup-goal-system-state.png)

    Tamam'a tıkladıktan sonra **Yedekleme hedefi**’nin yanında bir onay işareti görünür ve **Altyapıyı hazırlama** dikey penceresi açılır.

    ![Yedekleme hedefi yapılandırılmıştır, bundan sonra altyapıyı hazırlayın](./media/backup-try-azure-backup-in-10-mins/backup-goal-configed.png)

4. **Altyapıyı hazırlama** dikey penceresinde **Windows Server veya Windows İstemcisi için Aracı'yı indir** seçeneğine tıklayın.

    ![altyapıyı hazırlama](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

    Windows Server Essential kullanıyorsanız Windows Server Essential aracısını indirmeyi seçin. Açılır menü, MARSAgentInstaller.exe dosyasını çalıştırma veya kaydetme seçeneğini sunar.

    ![MARSAgentInstaller iletişim kutusu](./media/backup-try-azure-backup-in-10-mins/mars-installer-run-save.png)

5. İndirme açılır penceresinde **Kaydet**'e tıklayın.

    Varsayılan olarak, **MARSagentinstaller.exe** dosyası İndirilenler klasörünüze kaydedilir. Yükleyici tamamlandığında yükleyiciyi çalıştırmak veya klasörü açmak isteyip istemediğinizi soran bir açılır pencere görüntülenir.

    ![altyapıyı hazırlama](./media/backup-try-azure-backup-in-10-mins/mars-installer-complete.png)

    Aracıyı yüklemeniz henüz gerekli değildir. Kasa kimlik bilgilerini indirdikten sonra aracıyı yükleyebilirsiniz.

6. **Altyapıyı hazırlama** dikey penceresinde **İndir**'e tıklayın.

    ![kasa kimlik bilgilerini indirme](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

    Kasa kimlik bilgileri, İndirmeler klasörünüze indirilir. Kasa kimlik bilgilerini indirme tamamlandıktan sonra kimlik bilgilerini açmak veya kaydetmek isteyip istemediğinizi soran bir açılır pencere görüntülenir. **Kaydet**’e tıklayın. Yanlışlıkla **Aç**’a tıklarsanız, kasa kimlik bilgilerini açmaya çalışan iletişim kutusu başarısız olur. Kasa kimlik bilgilerini açamazsınız. Sonraki adıma geçin. Kasa kimlik bilgileri İndirmeler klasöründedir.

    ![kasa kimlik bilgilerini indirme tamamlandı](./media/backup-try-azure-backup-in-10-mins/vault-credentials-downloaded.png)
   > [!NOTE]
   > Kasa kimlik bilgilerinin yalnızca aracıyı kullanmayı düşündüğünüz Windows Server 'ın yerel bir konumuna kaydedilmesi gerekir.
   >

[!INCLUDE [backup-upgrade-mars-agent.md](../../includes/backup-upgrade-mars-agent.md)]

## <a name="install-and-register-the-agent"></a>Aracıyı yükleme ve kaydetme

> [!NOTE]
> Azure portal üzerinden yedeklemeyi etkinleştirme olanağı henüz mevcut değildir. Windows Server sistem durumunu yedeklemek için Microsoft Azure Kurtarma Hizmetleri aracısını kullanın.
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
     > Parolayı kaybeder veya unutursanız Microsoft, yedekleme verilerini kurtarmanıza yardımcı olamaz. Dosyayı güvenli bir konuma kaydedin. Bu dosya, bir yedeklemeyi geri yüklemek için gereklidir.
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

2. Kurtarma Hizmetleri aracısında, **Yedeklemeyi Zamanla**'ya tıklayın.

    ![Windows Server yedeklemesini zamanlama](./media/backup-try-azure-backup-in-10-mins/schedule-first-backup.png)

3. Yedeklemeyi Zamanlama Sihirbazı'nın Başlarken sayfasında **İleri**'ye tıklayın.

4. Yedeklenecek Öğeleri Seçin sayfasında **Öğe Ekle**'ye tıklayın.

5. **Sistem durumu** ' nu seçin ve ardından **Tamam**' a tıklayın.

6. **İleri**’ye tıklayın.

7. Sonraki sayfalarda sistem durumu yedeklemeleriniz için gerekli yedekleme sıklığını ve bekletme ilkesini seçin.

8. Onay sayfasında bilgileri gözden geçirin ve ardından **Son**'a tıklayın.

9. Sihirbaz yedekleme zamanlamasını oluşturduktan sonra **Kapat**'a tıklayın.

### <a name="to-back-up-windows-server-system-state-for-the-first-time"></a>Windows Server sistem durumunu ilk kez yedeklemek için

1. Windows Server için yeniden başlatma gerektiren bekleyen güncelleştirmeler bulunmadığından emin olun.

2. Kurtarma Hizmetleri aracısında, ağ üzerinden ilk doldurma işlemini tamamlamak için **Şimdi Yedekle**'ye tıklayın.

    ![Windows Server Yedekleme şimdi](./media/backup-try-azure-backup-in-10-mins/backup-now.png)

3. Görüntülenen **yedekleme öğesi Seç** ekranında **sistem durumu** ' nu seçin ve **İleri**' ye tıklayın.

4. Onay sayfasında, Şimdi Yedekle Sihirbazı'nın makineyi yedeklemek için kullanacağı ayarları gözden geçirin. Ardından **Yedekle**'ye tıklayın.

5. Sihirbazı kapatmak için **Kapat**'a tıklayın. Yedekleme işlemi tamamlanmadan önce sihirbazı kapatırsanız, sihirbaz arka planda çalışmaya devam eder.
    > [!NOTE]
    > MARS Aracısı, her sistem durumu yedeklemesinden önce ön denetimlerin parçası olarak yalnızca SFC/verifyonly 'yi tetikler. Bu, sistem durumunun bir parçası olarak Yedeklenen dosyaların Windows sürümüne karşılık gelen doğru sürümlere sahip olduğundan emin olmak için gereklidir. [Bu makalede](https://docs.microsoft.com/windows-server/administration/windows-commands/sfc)sistem dosyası DENETLEYICISI (Sfc) hakkında daha fazla bilgi edinin.
    >

İlk yedekleme tamamlandıktan sonra, Yedekleme konsolunda **İş tamamlandı** durumu görünür.

  ![IR tamamlandı](./media/backup-try-azure-backup-in-10-mins/ircomplete.png)

## <a name="questions"></a>Sorularınız mı var?

Sorularınız varsa veya dahil edilmesini istediğiniz herhangi bir özellik varsa [bize geri bildirim gönderin](https://feedback.azure.com/forums/258995-azure-backup).

## <a name="next-steps"></a>Sonraki adımlar

* [Windows makinelerini yedekleme](backup-windows-with-mars-agent.md) konusunda daha fazla bilgi edinin.
* Windows Server sistem durumlarınızı yedeklediğinize göre, [kasalarınızı ve sunucularınızı yönetebilirsiniz](backup-azure-manage-windows-server.md).
* Bir yedeklemeyi geri yüklemeniz gerekirse [dosyaları bir Windows makinesine geri yüklemek](backup-azure-restore-windows-server.md) için bu makaleyi kullanın.
