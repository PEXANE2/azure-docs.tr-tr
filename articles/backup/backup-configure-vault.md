---
title: MARS Aracısı ile Windows makinelerini yedekleme
description: Windows makinelerini yedeklemek için Azure Backup Microsoft Kurtarma Hizmetleri (MARS) aracısını kullanın.
ms.topic: conceptual
ms.date: 06/04/2019
ms.openlocfilehash: 990929cc95d1c25117873ca39415d33370456b91
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025546"
---
# <a name="back-up-windows-machines-with-the-azure-backup-mars-agent"></a>Azure Backup MARS aracısı ile Windows makinelerini yedekleme

Bu makalede, Azure Backup Aracısı olarak da bilinen [Azure Backup](backup-overview.md) hizmeti ve MICROSOFT Azure kurtarma HIZMETLERI (mars) Aracısı kullanılarak Windows makinelerini nasıl yedekleyeceğiniz açıklanmaktadır.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
>
> * Önkoşulları doğrulayın ve bir kurtarma hizmetleri Kasası oluşturun.
> * MARS Aracısını indirip ayarlama
> * Bir yedekleme ilkesi ve zamanlama oluşturun.
> * İsteğe bağlı yedekleme gerçekleştirin.

## <a name="about-the-mars-agent"></a>MARS Aracısı hakkında

MARS Aracısı, şirket içi makinelerden ve Azure VM 'lerinden dosyaları, klasörleri ve sistem durumunu Azure 'daki bir yedek kurtarma hizmetleri kasasına yedeklemek için Azure Backup tarafından kullanılır. Aracıyı aşağıdaki gibi çalıştırabilirsiniz:

* Doğrudan Azure 'da bir yedekleme kurtarma hizmetleri kasasına yedekleyebilmeleri için aracıyı doğrudan şirket içi Windows makinelerinde çalıştırın.
* SANAL makinede belirli dosya ve klasörleri yedeklemek için Windows çalıştıran Azure VM 'lerinde (Azure VM yedekleme uzantısıyla yan yana) aracıyı çalıştırın.
* Aracıyı bir Microsoft Azure Backup sunucusu (MABS) veya System Center Data Protection-Manager (DPM) sunucusu üzerinde çalıştırın. Bu senaryoda, makineler ve iş yükleri MABS/DPM 'ye yedeklenir ve ardından MABS/DPM, MARS Aracısı 'nı kullanarak Azure 'daki bir kasaya yedekler.
Yedekleyebileceğiniz, aracının yüklü olduğu yere bağlıdır.

> [!NOTE]
> Azure VM 'lerini yedeklemeye yönelik birincil yöntem, VM 'de bir Azure Backup uzantısı kullanmaktır. Bu, tüm VM 'yi yedekler. SANAL makinede belirli dosya ve klasörleri yedeklemek istiyorsanız, bu uzantının yanı sıra MARS aracısını yüklemek ve kullanmak isteyebilirsiniz. [Daha fazla bilgi edinin](backup-architecture.md#architecture-built-in-azure-vm-backup).

![Yedekleme işlemi adımları](./media/backup-configure-vault/initial-backup-process.png)

## <a name="before-you-start"></a>Başlamadan önce

* [Nasıl yapılacağını öğrenin](backup-architecture.md#architecture-direct-backup-of-on-premises-windows-server-machines-or-azure-vm-files-or-folders) Azure Backup Windows makinelerini MARS aracısıyla yedekler.
* İkincil bir MABS veya DPM sunucusu üzerinde MARS Aracısı 'nı çalıştıran yedekleme mimarisi [hakkında bilgi edinin](backup-architecture.md#architecture-back-up-to-dpmmabs) .
* Nelerin desteklendiğini ve MARS aracısıyla neleri yedeklenebileceklerini [gözden geçirin](backup-support-matrix-mars-agent.md) .
* Yedeklemek istediğiniz makinelerde internet erişimini doğrulayın.
* Bir sunucuyu veya istemcisini Azure 'a yedeklemek için bir Azure hesabınızın olması gerekir. Hiç bir hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir [Hesap](https://azure.microsoft.com/free/) oluşturabilirsiniz.

### <a name="verify-internet-access"></a>İnternet erişimini doğrulama

Makinenizin internet erişimi sınırlı ise, makinedeki veya proxy 'deki güvenlik duvarı ayarlarının bu URL 'Lere ve IP adreslerine izin verildiğinden emin olun:

#### <a name="urls"></a>Adresleri

* www\.msftncsi.com
* *.Microsoft.com
* *.WindowsAzure.com
* *.microsoftonline.com
* *.windows.net

#### <a name="ip-addresses"></a>IP adresleri

* 20.190.128.0/18
* 40.126.0.0/18

Yukarıda listelenen tüm URL ve IP adreslerine erişim, bağlantı noktası 443 ' de HTTPS protokolünü kullanır.

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Bir kurtarma hizmetleri Kasası, zaman içinde oluşturduğunuz tüm yedeklemeleri ve kurtarma noktalarını depolar ve yedeklenen makinelere uygulanan yedekleme ilkesini içerir. Aşağıdaki gibi bir kasa oluşturun:

1. Azure aboneliğinizi kullanarak [Azure Portal](https://portal.azure.com/) oturum açın.

2. **Kurtarma Hizmetleri kasalarını**arayın ve seçin.

    ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-configure-vault/open-recovery-services-vaults.png)

3. **Kurtarma Hizmetleri kasaları** menüsünde **+ Ekle**' ye tıklayın.

    ![Kurtarma Hizmetleri Kasası oluşturma 2. adım](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

4. **Ad** alanına, kasayı tanımlayacak kolay bir ad girin.

   * Adın Azure aboneliği için benzersiz olması gerekir.
   * Bu, 2 ile 50 karakter içerebilir.
   * Bir harfle başlamalı ve yalnızca harf, rakam ve kısa çizgi içerebilir.

5. Kasanın oluşturulması gereken Azure aboneliğini, kaynak grubunu ve coğrafi bölgeyi seçin. Yedekleme verileri kasaya gönderilir. Sonra **Oluştur**’a tıklayın.

    ![Kurtarma Hizmetleri Kasası Oluşturma 3. adım](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

   * Kasanın oluşturulması biraz zaman alabilir.
   * Portalın sağ üst bölümündeki durum bildirimlerini izleyin. Birkaç dakika sonra kasayı görmüyorsanız **Yenile**' ye tıklayın.

     ![Yenile düğmesine tıklayın](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)

### <a name="set-storage-redundancy"></a>Depolama artıklığı ayarlama

Azure Backup kasa için depolamayı otomatik olarak işler. Bu depolamanın nasıl çoğaltılacağı belirtmeniz gerekir.

1. **Kurtarma Hizmetleri kasaları** dikey penceresinden yeni kasaya tıklayın. **Ayarlar** bölümünde, **Özellikler**' e tıklayın.
2. **Özellikler**' de, **yedekleme yapılandırması**altında **Güncelleştir**' e tıklayın.

3. Depolama çoğaltma türünü seçin ve **Kaydet**' e tıklayın.

      ![Yeni kasa için depolama yapılandırması ayarlama](./media/backup-try-azure-backup-in-10-mins/recovery-services-vault-backup-configuration.png)

* Azure 'u birincil yedek depolama uç noktası olarak kullanıyorsanız, varsayılan **coğrafi olarak yedekli** ayarını kullanmaya devam edebilirsiniz.
* Azure’u birincil yedek depolama uç noktası olarak kullanmıyorsanız, Azure depolama maliyetlerini azaltan **Yerel olarak yedekli** seçeneğini belirleyin.
* [Coğrafi](../storage/common/storage-redundancy-grs.md) ve [Yerel](../storage/common/storage-redundancy-lrs.md) artıklık hakkında daha fazla bilgi edinin.

## <a name="download-the-mars-agent"></a>MARS aracısını indirin

Yedeklemek istediğiniz makinelere yükleme için MARS aracısını indirin.

* Aracıyı herhangi bir makineye zaten yüklediyseniz, en son sürümü çalıştırdığınızdan emin olun.
* En son sürüm portalda kullanılabilir veya [doğrudan karşıdan yükleme](https://aka.ms/azurebackup_agent) kullanılıyor

1. Kasadaki **Başlarken**bölümünde **Yedekle**' ye tıklayın.

    ![Yedekleme hedefi dikey penceresini açma](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

2. **İş yükünüz nerede çalışıyor?** , **Şirket içi**seçeneğini belirleyin. MARS aracısını bir Azure VM 'ye yüklemek istiyor olsanız bile bu seçeneği seçmeniz gerekir.
3. **Neleri yedeklemek istiyorsunuz?** **dosya ve klasör** ve/veya **sistem durumu**' nu seçin. Birçok farklı seçenek mevcuttur, ancak bunlar yalnızca ikincil bir yedekleme sunucusu çalıştırıyorsanız desteklenir. **Altyapıyı hazırla**' ya tıklayın.

      ![Dosya ve klasörleri yedekleme](./media/backup-try-azure-backup-in-10-mins/set-file-folder.png)

4. **Altyapıyı hazırla**bölümünde, **Kurtarma Hizmetleri ARACıSı yükleme**altında Mars aracısını indirin.

    ![altyapıyı hazırlama](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

5. İndirme açılır penceresinde **Kaydet**'e tıklayın. Varsayılan olarak, **MARSagentinstaller.exe** dosyası İndirilenler klasörünüze kaydedilir.

6. Şimdi, **zaten indirmeyi veya en son kurtarma hizmetleri aracısını kullanmayı**denetleyin ve ardından kasa kimlik bilgilerini indirin.

    ![kasa kimlik bilgilerini indirme](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

7. **Save (Kaydet)** düğmesine tıklayın. Dosya, Indirme klasörünüze indirilir. Kasa kimlik bilgileri dosyasını açamazsınız.

## <a name="install-and-register-the-agent"></a>Aracıyı yükleme ve kaydetme

1. Yedeklemek istediğiniz makinelerde **Marsagentınstaller. exe** dosyasını çalıştırın.
2. MARS Aracısı Kurulum Sihirbazı > **yükleme ayarları**' nda, aracıyı yüklemek istediğiniz yeri ve önbellek için kullanılacak bir konumu belirtin. Ardından **İleri**'ye tıklayın.
   * Azure Backup, veri anlık görüntülerini Azure 'a göndermeden önce depolamak için önbelleği kullanır.
   * Önbellek konumu, yedekleyeceğiniz verilerin boyutunun en az %5 ' i kadar boş alan içermelidir.

    ![MARS Sihirbazı yükleme ayarları](./media/backup-configure-vault/mars1.png)

3. **Ara sunucu yapılandırması**' nda, Windows makinesinde çalışan aracının internet 'e nasıl bağlanacağını belirtin. Ardından **İleri**'ye tıklayın.

   * Özel bir proxy kullanıyorsanız, proxy ayarlarını ve gerekirse kimlik bilgilerini belirtin.
   * Aracının [Bu URL 'lere](#verify-internet-access)erişmesi gerektiğini unutmayın.

     ![MARS Sihirbazı internet erişimi](./media/backup-configure-vault/mars2.png)

4. **Yükleme** ' de önkoşul denetimini gözden geçirin ve **yükleme**' ye tıklayın.
5. Aracı yüklendikten sonra, **kayda devam et ' e**tıklayın.
6. **Sunucu kaydetme sihirbazı** > **kasa kimliği**' nde, indirdiğiniz kimlik bilgileri dosyasına gidip seçin. Ardından **İleri**'ye tıklayın.

    ![Register-kasa kimlik bilgileri](./media/backup-configure-vault/register1.png)

7. **Şifreleme ayarında**, makinenin yedeklemelerini şifrelemek ve şifresini çözmek için kullanılacak bir parola belirtin.

    * Şifreleme parolasını güvenli bir konuma kaydedin.
    * Parolayı kaybeder veya unutursanız, Microsoft, yedekleme verilerini kurtarmaya yardımcı olamaz. Dosyayı güvenli bir konuma kaydedin. Bir yedeklemeyi geri yüklemeniz gerekir.

8. **Finish (Son)** düğmesine tıklayın. Aracı artık yüklenmiş ve makineniz kasaya kaydedilmiştir. Yedeklemenizi yapılandırıp zamanlamak için hazırsınız.

## <a name="create-a-backup-policy"></a>Yedekleme ilkesi oluşturma

Yedekleme ilkesi, kurtarma noktaları oluşturmak için verilerin ne zaman anlık görüntülerinin alınacağını ve kurtarma noktalarının ne kadar süreyle saklanacağını belirtir.

* MARS Aracısı 'nı kullanarak bir yedekleme ilkesi yapılandırırsınız.
* Azure Backup otomatik olarak gün ışığından yararlanma süresini (DST) hesaba almaz. Bu, gerçek zaman ve zamanlanan yedekleme zamanı arasında bir uyuşmazlık oluşmasına neden olabilir.

Aşağıdaki şekilde bir ilke oluşturun:
1. MARS aracısını indirip kaydettikten sonra, aracı konsolunu başlatın. Bunu, makinenizde **Microsoft Azure Backup** aramasını yaparak bulabilirsiniz.  
2. **Eylemler**' de, **yedeklemeyi zamanla**' ya tıklayın.

    ![Windows Server yedeklemesini zamanlama](./media/backup-configure-vault/schedule-first-backup.png)
3. Yedekleme zamanlaması Sihirbazı > **Başlarken** **İleri**' ye tıklayın.
4. **Yedeklenecek öğeleri seçin**' de, **öğe Ekle**' ye tıklayın.

    ![Yedeklenecek öğeleri seçin](./media/backup-azure-manage-mars/select-item-to-backup.png)

5. **Öğeleri seç**' de, ne yedeklemek istediğinizi seçin ve **Tamam**' a tıklayın.

    ![Yedeklenecek seçili öğeler](./media/backup-azure-manage-mars/selected-items-to-backup.png)

6. **Yedeklenecek öğeleri seçin** sayfasında **İleri**' ye tıklayın.
7. **Yedekleme zamanlamasını belirtin** sayfasında, günlük veya haftalık yedeklemeleri ne zaman almak istediğinizi belirtin. Ardından **İleri**'ye tıklayın.

    - Bir yedekleme çekilirken bir kurtarma noktası oluşturulur.
    - Ortamınızda oluşturulan kurtarma noktası sayısı yedekleme zamanlamanıza bağlıdır.


8. Günde en fazla üç kez günlük yedeklemeler zamanlayabilirsiniz. Örneğin ekran görüntüsünde, biri gece yarısı ve diğeri 6:00 PM olmak üzere iki günlük yedek gösterilmektedir.

    ![Günlük zamanlama](./media/backup-configure-vault/day-schedule.png)


9. Haftalık yedeklemeleri de çalıştırabilirsiniz. Örneğin ekran görüntüsünde, 9:30 ve 1:00 ' de Çarşamba & her alternatif Pazar için alınan yedeklemeler gösterilmektedir.

    ![Haftalık zamanlama](./media/backup-configure-vault/week-schedule.png)


10. **Bekletme Ilkesi Seç** sayfasında, verilerinizin geçmiş kopyalarını nasıl depolayakullanacağınızı belirtin. Ardından **İleri**'ye tıklayın.

    - Bekletme ayarları hangi kurtarma noktalarının depolanacağını ve ne kadar süreyle depolanması gerektiğini belirtir.
    - Örneğin, günlük bir bekletme ayarı ayarladığınızda, günlük bekletme için belirtilen zamanda en son kurtarma noktasının belirtilen gün sayısı boyunca tutulacaksınız. Ya da başka bir örnek olarak, her ayın 30. gününde oluşturulan kurtarma noktasının 12 ay boyunca depolanması gerektiğini göstermek için bir aylık bekletme ilkesi belirtebilirsiniz.
    - Günlük ve haftalık kurtarma noktası bekletme genellikle yedekleme zamanlaması ile saatle çakışan. Yedekleme zamanlamaya göre tetiklendiğinde, yedekleme tarafından oluşturulan kurtarma noktası günlük veya haftalık bekletme ilkesinde belirtilen süre için depolanır.
    - Örnek olarak, aşağıdaki ekran görüntüsünde:

        -   Gece yarısı ve 6:00 PM 'de günlük yedeklemeler yedi gün boyunca tutulur.
        -   Gece yarısı ve 6:00 PM 'de Cumartesi günü gerçekleştirilen yedeklemeler dört hafta boyunca tutulur.
        -   Gece yarısı ve 6:00 PM 'deki ayın son haftasında Cumartesi tarihinde gerçekleştirilen yedeklemeler 12 ay boyunca tutulur.
        -   Mart 'ın son haftasında Cumartesi günü alınan yedeklemeler 10 yıl boyunca tutulur.

        ![Bekletme örneği](./media/backup-configure-vault/retention-example.png)


11. İlk yedekleme **türünü seçin** bölümünde, ağ üzerinden ilk yedeklemeyi almak mi yoksa çevrimdışı yedekleme mi kullanmak istediğinize karar verin (çevrimdışı yedekleme hakkında daha fazla bilgi için bu [makaleye](offline-backup-azure-data-box.md)bakın). İlk yedeklemeyi ağ üzerinden almak için, **ağ üzerinden otomatik olarak** ' ı seçin ve **İleri**' ye tıklayın.

    ![ilk yedekleme türü](./media/backup-azure-manage-mars/choose-initial-backup-type.png)


12. **Onaylama**bölümünde bilgileri gözden geçirin ve ardından **son**' a tıklayın.

    ![Yedekleme türünü Onayla](./media/backup-azure-manage-mars/confirm-backup-type.png)


13. Sihirbaz yedekleme zamanlamasını oluşturduktan sonra **Kapat**'a tıklayın.

    ![Yedekleme Işlemini değiştirmeyi Onayla](./media/backup-azure-manage-mars/confirm-modify-backup-process.png)

Aracının yüklü olduğu her makinede bir ilke oluşturmanız gerekir.

### <a name="perform-the-initial-backup-offline"></a>İlk yedeklemeyi çevrimdışı yap

Bir ilk yedeklemeyi otomatik olarak ağ üzerinden veya çevrimdışı çalıştırabilirsiniz. Bir ilk yedekleme için çevrimdışı dengeli dağıtım, çok sayıda ağ bant genişliğinin aktarılmasına gerek alacak büyük miktarlardır. Çevrimdışı bir aktarımı şu şekilde yapabilirsiniz:

1. Yedekleme verilerini bir hazırlama konumuna yazarsınız.
2. Hazırlama konumundaki verileri bir veya daha fazla SATA diskine kopyalamak için AzureOfflineBackupDiskPrep aracını kullanabilirsiniz.
3. Araç bir Azure Içeri aktarma işi oluşturur. Azure içeri aktarma ve dışarı aktarma hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/storage/common/storage-import-export-service) .
4. SATA disklerini bir Azure veri merkezine gönderirsiniz.
5. Veri merkezinde, disk verileri bir Azure depolama hesabına kopyalanır.
6. Azure Backup, verileri depolama hesabından kasaya kopyalar ve artımlı yedeklemeler zamanlanır.

Çevrimdışı dengeli dağıtım hakkında [daha fazla bilgi edinin](offline-backup-azure-data-box.md) .

### <a name="enable-network-throttling"></a>Ağ azaltmayı etkinleştir

Ağ genişliğini azaltmayı etkinleştirerek, MARS Aracısı tarafından ağ bant genişliğinin nasıl kullanıldığını kontrol edebilirsiniz. Çalışma saatleri sırasında verileri yedeklemeniz gerekiyorsa, ancak yedekleme ve geri yükleme etkinliği için ne kadar bant genişliği kullanıldığını denetlemek istediğinizde kısıtlama yararlı olur.

* Azure Backup ağ azaltma, yerel işletim sisteminde [hizmet kalitesi (QoS)](https://docs.microsoft.com/windows-server/networking/technologies/qos/qos-policy-top) kullanır.
* Yedekleme için ağ daraltma, Windows Server 2012 ve Windows 8 ' de Windows 8 ve sonraki sürümlerde kullanılabilir. İşletim sistemleri en son hizmet paketlerini çalıştırıyor olmalıdır.

Ağ azaltmayı şu şekilde etkinleştirin:

1. MARS aracısında **Özellikleri Değiştir**' e tıklayın.
2. **Daraltma** sekmesinde, **yedekleme işlemleri için internet bant genişliği kullanımını azaltmayı etkinleştir**' i işaretleyin.

    ![Ağ azaltma](./media/backup-configure-vault/throttling-dialog.png)
3. İş sırasında ve çalışma saatleri dışında izin verilen bant genişliğini belirtin. Bant genişliği değerleri 512 kbps ile başlar ve 1.023 MBps 'e kadar gider. Daha sonra, **Tamam**'a tıklayın.

## <a name="run-an-on-demand-backup"></a>İsteğe bağlı yedekleme çalıştırma

1. MARS aracısında **Şimdi Yedekle**' ye tıklayın.

    ![Windows Server şimdi yedekle](./media/backup-configure-vault/backup-now.png)

2. MARS Aracısı sürümü 2.0.9169.0 veya daha yeniyse, özel bir saklama alanı ayarlanabilir. Yedeklemeyi aynı şekilde **sakla** bölümünde, sunulan takvimden bir tarih seçin:

   ![Yedekleme takvimini sakla](./media/backup-configure-vault/mars-ondemand.png)

3. **Onaylama**bölümünde ayarları gözden geçirin ve **Yedekle**' ye tıklayın.
4. Sihirbazı kapatmak için **Kapat**'a tıklayın. Yedekleme tamamlanmadan önce bunu yaparsanız, sihirbaz arka planda çalışmaya devam eder.
5. İlk yedekleme tamamlandıktan sonra, Yedekleme konsolunda **İş tamamlandı** durumu görünür.

## <a name="on-demand-backup-policy-retention-behavior"></a>İsteğe bağlı yedekleme ilkesi saklama davranışı

>[!NOTE]
>Yalnızca 2.0.9169.0 'den eski MARS Aracısı sürümleri için geçerlidir
>

* Daha fazla bilgi için [yedekleme Ilkesi oluşturma](backup-configure-vault.md#create-a-backup-policy) ' nın 8. adımına bakın.

| Yedekleme zamanlaması seçeneği | Yedeklenen veriler ne kadar süreyle bekletilecektir?
| -- | --
| Her zaman bir yedekleme zamanlayın: * gün | **Varsayılan saklama**: "günlük yedeklemeler için gün cinsinden bekletme" ile eşdeğerdir <br/><br/> **Özel durum**: uzun süreli saklama (hafta, ay, yıl) için günlük zamanlanmış bir yedekleme kümesi başarısız olursa, bu başarısız zamanlanan yedekleme, uzun süreli saklama için kabul edilir. Aksi halde, bir sonraki zamanlanmış yedekleme uzun süreli saklama için kabul edilir.<br/><br/> **Örnek**: (deyin) 8:00 Perşembe günü gerçekleştirilen zamanlanmış yedekleme başarısız olursa ve aynı yedekleme haftalık/aylık/yıllık bekletme için değerlendiriliyorsa, sonraki zamanlanan yedeklemeden (deyin) önce tetiklenen ilk isteğe bağlı yedekleme, 00 8:00:8:00 ' e uygun olarak haftalık/aylık/yıllık bekletme için otomatik olarak etiketlenir.
| Her zaman bir yedekleme zamanlayın: * haftalık | **Varsayılan bekletme**: 1 gün <br/> Haftalık yedekleme ilkesiyle bir veri kaynağı için alınan isteğe bağlı yedeklemeler, veri kaynağı için en son yedeklemeler olsalar bile bir sonraki günde silinir. <br/><br/> **Özel durum**: uzun süreli saklama (hafta, ay, yıl) için haftalık zamanlanmış bir yedekleme kümesi başarısız olursa, bu başarısız zamanlanan yedekleme, uzun süreli saklama için kabul edilir. Aksi halde, bir sonraki zamanlanmış yedekleme uzun süreli saklama için kabul edilir. <br/><br/> **Örnek**: (deyin) 8:00 Perşembe günü gerçekleştirilen zamanlanmış yedekleme başarısız oluyor ve aynı yedekleme aylık/yıllık bekletme için değerlendiriliyorsa, sonraki zamanlanmış yedeklemeden (deyin) önce tetiklenen ilk isteğe bağlı yedekleme, 00 8:00:8:00 ' e uygun olarak aylık/yıllık saklama için otomatik olarak etiketlenir

## <a name="next-steps"></a>Sonraki adımlar

[Dosyaları geri yüklemeyi öğrenin](backup-azure-restore-windows-server.md).
