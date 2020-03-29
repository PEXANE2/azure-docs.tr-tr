---
title: Azure Veri Kutusu'ni kullanarak çevrimdışı yedekleme
description: Mars Aracısı'ndan Kurtarma Hizmetleri kasasına büyük ilk yedekleme verilerini çevrimdışı tohumlamak için Azure Veri Kutusu'nu nasıl kullanabileceğinizi öğrenin.
ms.topic: conceptual
ms.date: 1/27/2020
ms.openlocfilehash: a031a8cac357e7d212f8f6a3a5dbec749fbccc21
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78672960"
---
# <a name="azure-backup-offline-backup-by-using-azure-data-box"></a>Azure Veri Kutusu'ni kullanarak Azure Yedekleme çevrimdışı yedekleme

Büyük ilk Microsoft Azure Kurtarma Hizmetleri (MARS) yedeklemelerinizi çevrimdışı (ağ kullanmadan) Kurtarma Hizmetleri kasasına tohumlamak için [Azure Veri Kutusu'ni](https://docs.microsoft.com/azure/databox/data-box-overview) kullanabilirsiniz. Bu işlem, aksi takdirde yüksek gecikmeli bir ağ üzerinden çevrimiçi büyük miktarda yedekleme veri taşıma tüketilen olacak zaman ve ağ bant genişliği kaydeder. Bu geliştirme şu anda önizlemededir. Azure Veri Kutusu'na dayalı çevrimdışı yedekleme, [Azure İçe Alma/Dışa Aktarma hizmetine dayalı çevrimdışı yedeklemeye](https://docs.microsoft.com/azure/backup/backup-azure-backup-import-export)göre iki farklı avantaj sağlar:

* Azure uyumlu kendi disklerinizi ve konektörlerinizi satın almanıza gerek yoktur. Azure Veri Kutusu, seçili [Veri Kutusu SKU](https://azure.microsoft.com/services/databox/data/)ile ilişkili diskleri aktarır.
* Azure Yedekleme (MARS Aracısı), desteklenen Azure Veri Kutusu'nun SNU'larına yedekleme verilerini doğrudan yazabilir. Bu özellik, ilk yedekleme verileriniz için bir hazırlama konumu sağlama gereksinimini ortadan kaldırır. Ayrıca, bu verileri biçimlendirmek ve disklere kopyalamak için yardımcı programlara da ihtiyacınız yoktur.

## <a name="azure-data-box-with-the-mars-agent"></a>MARS Aracılı Azure Veri Kutusu

Bu makalede, MARS Aracısı'ndan Kurtarma Hizmetleri kasasına büyük ilk yedekleme verilerini çevrimdışı tohumlamak için Azure Veri Kutusu'nu nasıl kullanabileceğiniz açıklanmaktadır.

## <a name="supported-platforms"></a>Desteklenen platformlar

Azure Veri Kutusu'nu kullanarak MARS Aracısı'ndan veri tohumlaması işlemi aşağıdaki Windows SUS'larda desteklenir.

| **İşletim Sistemi**                                 | **Sku**                                                      |
| -------------------------------------- | ------------------------------------------------------------ |
| **İş İstasyonu**                        |                                                              |
| Windows 10 64 bit                     | Enterprise, Pro, Home                                       |
| Windows 8.1 64 bit                    | Enterprise, Pro                                             |
| Windows 8 64 bit                      | Enterprise, Pro                                             |
| Windows 7 64 bit                      | Ultimate, Enterprise, Professional, Home Premium, Home Basic, Starter |
| **Sunucu**                             |                                                              |
| Windows Server 2019 64 bit            | Standard, Datacenter, Essentials                            |
| Windows Server 2016 64 bit            | Standard, Datacenter, Essentials                            |
| Windows Server 2012 R2 64 bit         | Standard, Datacenter, Foundation                            |
| Windows Server 2012 64 bit            | Datacenter, Foundation, Standard                            |
| Windows Storage Server 2016 64 bit    | Standard, Workgroup                                         |
| Windows Storage Server 2012 R2 64 bit | Standard, Workgroup, Essential                              |
| Windows Storage Server 2012 64 bit    | Standard, Workgroup                                         |
| Windows Server 2008 R2 SP1 64 bit     | Standard, Enterprise, Datacenter, Foundation                |
| Windows Server 2008 SP2 64 bit        | Standard, Enterprise, Datacenter                            |

## <a name="backup-data-size-and-supported-data-box-skus"></a>Yedekleme veri boyutu ve desteklenen Veri Kutusu SUs'ları

| Yedekleme veri boyutu (MARS tarafından sıkıştırma sonrası)* sunucu başına | Desteklenen Azure Veri Kutusu SKU                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| <=7.2 TB                                                    | [Azure Veri Kutusu diski](https://docs.microsoft.com/azure/databox/data-box-disk-overview) |
| >7,2 TB ve <=80 TB**                                      | [Azure Veri Kutusu (100 TB)](https://docs.microsoft.com/azure/databox/data-box-overview) |

*Tipik kompresyon oranları %10 ile %20 arasında değişmektedir. <br>
**Tek bir MARS sunucusu için 80 TB'den fazla ilk [AskAzureBackupTeam@microsoft.com](mailto:AskAzureBackupTeam@microsoft.com)yedekleme verisi olmasını bekliyorsanız, başvurun.

>[!IMPORTANT]
>Tek bir sunucudan gelen ilk yedekleme verilerinin tek bir Azure Veri Kutusu örneği veya Azure Veri Kutusu diski içinde yer alması gerekir ve aynı veya farklı SK'lere ait birden çok aygıt arasında paylaşılamaz. Ancak bir Azure Veri Kutusu aygıtı birden çok sunucudan ilk yedeklemeleri içerebilir.

## <a name="prerequisites"></a>Ön koşullar

### <a name="azure-subscription-and-required-permissions"></a>Azure aboneliği ve gerekli izinler

* İşlem için bir Azure aboneliği gerekir.
* İşlem, çevrimdışı yedekleme ilkesini gerçekleştirmek üzere atanan kullanıcının Azure aboneliğinin sahibi olmasını gerektirir.
* Veri Kutusu işi ve Kurtarma Hizmetleri kasasının (verilerin tohumlanması gereken) aynı aboneliklerde olması gerekir.
* Azure Veri Kutusu işi ve Kurtarma Hizmetleri kasasıyla ilişkili hedef depolama hesabının aynı bölgede olduğunu öneririz. Ancak, bu gerekli değildir.

### <a name="get-azure-powershell-370"></a>Azure PowerShell 3.7.0'ı alın

*Bu süreç için en önemli ön koşuldur.* Azure PowerShell sürümünü 3.7.0 olarak yüklemeden önce aşağıdaki denetimleri gerçekleştirin.

#### <a name="step-1-check-the-powershell-version"></a>Adım 1: PowerShell sürümünü kontrol edin

1. Windows PowerShell'i açın ve aşağıdaki komutu çalıştırın:

    ```powershell
    Get-Module -ListAvailable AzureRM*
    ```

1.  Çıktı 3.7.0'dan yüksek bir sürüm görüntülerse, "Adım 2" yapın. Aksi takdirde, "Adım 3"e atlayın.

#### <a name="step-2-uninstall-the-powershell-version"></a>Adım 2: PowerShell sürümünü kaldırın

PowerShell'in geçerli sürümünü kaldırın.

1. PowerShell'de aşağıdaki komutu çalıştırarak bağımlı modülleri kaldırın:

    ```powershell
    foreach ($module in (Get-Module -ListAvailable AzureRM*).Name |Get-Unique)  { write-host "Removing Module $module" Uninstall-module $module }
    ```

2. Bağımlı modüllerin başarılı bir şekilde silinmesini sağlamak için aşağıdaki komutu çalıştırın:

    ```powershell
    Get-Module -ListAvailable AzureRM*
    ```

#### <a name="step-3-install-powershell-version-370"></a>Adım 3: PowerShell sürüm 3.7.0 yükleyin

AzureRM modülü bulunmadığını doğruladıktan sonra aşağıdaki yöntemlerden birini kullanarak sürüm 3.7.0'ı yükleyin:

* GitHub'dan [bu bağlantıyı](https://github.com/Azure/azure-powershell/releases/tag/v3.7.0-March2017)kullanın.

Veya şunları yapabilirsiniz:

* PowerShell penceresinde aşağıdaki komutu çalıştırın:

    ```powershell
    Install-Module -Name AzureRM -RequiredVersion 3.7.0
    ```

Azure PowerShell bir msi dosyası kullanılarak da yüklenmiş olabilir. Kaldırmak için Denetim Masası'ndaki **Programları Kaldır** seçeneğini kullanarak kaldırın.

### <a name="order-and-receive-the-data-box-device"></a>Veri Kutusu aygıtını sipariş edin ve alın

MARS ve Azure Veri Kutusu'nu kullanarak çevrimdışı yedekleme işlemi, MARS Aracısı'nı kullanarak çevrimdışı yedeklemeyi tetiklemeden önce Veri Kutusu aygıtları teslim durumunda olmasını gerektirir. Gereksiniminiz için en uygun SKU'yu sipariş etmek için [Yedekleme veri boyutu na ve desteklenen Veri Kutusu SKU'larına](#backup-data-size-and-supported-data-box-skus)bakın. Öğretici' deki adımları izleyin: Veri Kutusu aygıtlarınızı sipariş etmek ve almak için [bir Azure Veri Kutusu diski sipariş](https://docs.microsoft.com/azure/databox/data-box-disk-deploy-ordered) edin.

> [!IMPORTANT]
> **Hesap türü**için *BlobStorage'ı* seçmeyin. MARS Aracısı, *BlobStorage* seçildiğinde desteklenmeyen sayfa lekelerini destekleyen bir hesap gerektirir. Azure Veri Kutusu işiniz için hedef depolama hesabı oluştururken **Hesap türü** olarak Depolama V2 'yi **(genel amaçlı v2)** seçin.

![Örnek ayrıntılarda hesap türünü seçin](./media/offline-backup-azure-data-box/instance-details.png)

## <a name="install-and-set-up-the-mars-agent"></a>MARS Aracısını yükleyin ve kurun

1. MARS Aracısı'nın önceki yüklemelerini kaldırdığınızdan emin olun.
1. [Bu web sitesinden](https://aka.ms/azurebackup_agent)en son MARS Agent indirin.
1. *MARSAgentInstaller.exe'yi*çalıştırın ve [yalnızca aracıyı yüklemek ve](https://docs.microsoft.com/azure/backup/install-mars-agent#install-and-register-the-agent) yedeklemelerinizin depolenmesini istediğiniz Kurtarma Hizmetleri kasasına kaydettirmek için gereken adımları *yapın.*

   > [!NOTE]
   > Kurtarma Hizmetleri kasası, Azure Veri Kutusu işiyle aynı abonelikte olmalıdır.

   Aracı Kurtarma Hizmetleri kasasına kaydolduktan sonra, sonraki bölümlerdeki adımları izleyin.

## <a name="set-up-azure-data-box-devices"></a>Azure Veri Kutusu aygıtlarını ayarlama

Sipariş ettiğiniz Azure Veri Kutusu SKU'ya bağlı olarak, aşağıdaki uygun bölümlerde kapsanan adımları yapın. Adımlar, ilk yedekleme verilerini tanımlamak ve aktarmak için Mars Aracısı için Veri Kutusu aygıtlarını nasıl ayarlayacağınızı ve hazırlayacağınızı gösterir.

### <a name="set-up-azure-data-box-disks"></a>Azure Veri Kutusu disklerini ayarlama

Bir veya daha fazla Azure Veri Kutusu diski (her biri 8 TB'a kadar) sipariş ettiyseniz, [Veri Kutusu diskinizi açmak, bağlamak ve kilidini açmak](https://docs.microsoft.com/azure/databox/data-box-disk-deploy-set-up)için burada belirtilen adımları izleyin.

>[!NOTE]
>MARS Agent'ı olan sunucunun USB bağlantı noktası olmaması mümkündür. Bu durumda, Azure Veri Kutusu diskinizi başka bir sunucuya veya istemciye bağlayabilir ve aygıtın kökünü ağ paylaşımı olarak ortaya çıkarabilirsiniz.

### <a name="set-up-azure-data-box"></a>Azure Veri Kutusu'nı ayarlama

Bir Azure Veri Kutusu örneği (en fazla 100 TB) sipariş ettiyseniz, [Veri Kutusu örneğini ayarlamak için](https://docs.microsoft.com/azure/databox/data-box-deploy-set-up)aşağıdaki adımları izleyin.

#### <a name="mount-your-azure-data-box-instance-as-a-local-system"></a>Azure Veri Kutusu örneğini Yerel Sistem olarak monte edin

MARS Aracısı Yerel Sistem bağlamında çalıştığından, Azure Veri Kutusu örneğinin bağlı olduğu montaj yoluna aynı ayrıcalık düzeyinin sağlanması nı gerektirir. 

NFS protokolünü kullanarak Veri Kutusu aygıtınızı Yerel Sistem olarak monte edebilmenizi sağlamak için:

1. MARS Aracısı yüklü olan Windows sunucusundaki NFS özelliğinin istemcisini etkinleştirin. Alternatif kaynak *WIM:D:\Kaynaklar\Install.wim:4*belirtin.
1. PSExec'i <https://download.sysinternals.com/files/PSTools.zip> MARS Agent yüklü sunucuya indirin.
1. Yükseltilmiş bir komut istemiaçın ve geçerli dizini olarak *PSExec.exe* içeren dizinile aşağıdaki komutu çalıştırın.

    ```cmd
    psexec.exe  -s  -i  cmd.exe
    ```

   Önceki komutun bir sonucu olarak açılan komut penceresi Yerel Sistem bağlamındadır. Windows sunucunuzda ağ sürücüsü olarak Azure sayfa blob paylaşımını monte etmek için adımları yürütmek için bu komut penceresini kullanın.
1. Sunucunuzu Mars Agent ile NFS üzerinden Veri Kutusu cihazına bağlamak için [Veri Kutusuna Bağlan'daki](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data-via-nfs#connect-to-data-box) adımları izleyin. Azure sayfa blobs paylaşımını takmak için Yerel Sistem komutu komutu komutunu çalıştırın.

    ```cmd
    mount -o nolock \\<DeviceIPAddress>\<StorageAccountName_PageBlob X:  
    ```

   Paylaşım monte edildikten sonra, X:'e sunucunuzdan erişip erişebildiğinizi kontrol edin. Eğer yapabilirseniz, bu makalenin bir sonraki bölümü ile devam edin.

## <a name="transfer-initial-backup-data-to-azure-data-box-devices"></a>İlk yedekleme verilerini Azure Veri Kutusu aygıtlarına aktarma

1. Microsoft **Azure Yedekleme** uygulamasını sunucunuzda açın.
1. **Eylemler** bölmesine Yedek **Zamanla'yı**seçin.

    ![Zamanlama Yedekleme'yi seçin](./media/offline-backup-azure-data-box/schedule-backup.png)

1. **Zamanlama Yedekleme Sihirbazı'ndaki**adımları izleyin.

1. **Öğeleri Ekle** düğmesini seçerek öğe ekleyin. Sipariş ettiğiniz ve aldığınız [Azure Veri Kutusu SKU tarafından desteklenen boyut sınırları](#backup-data-size-and-supported-data-box-skus) içinde öğelerin toplam boyutunu tutun.

    ![Yedeklemeye öğe ekleme](./media/offline-backup-azure-data-box/add-items.png)

1. Dosyalar ve **Klasörler** ve **Sistem Durumu**için uygun yedekleme çizelgesi ve bekletme ilkesini seçin. Sistem durumu yalnızca Windows Sunucuları için geçerlidir, Windows istemcileri için geçerli değildir.
1. Sihirbazın **İlk Yedekleme Türünü (Dosya ve Klasörler) seç** sayfasında, Microsoft Azure Veri Kutusu **disklerini kullanarak Aktarım** seçeneğini seçin ve **İleri'yi**seçin.

    ![İlk yedekleme türünü seçin](./media/offline-backup-azure-data-box/initial-backup-type.png)

1. Azure aboneliğinde Sahip erişimi olan kullanıcı kimlik bilgilerini kullanarak istendiğinde Azure'da oturum açın. Bunu başardıktan sonra, bu benzer bir sayfa görmeniz gerekir.

    ![Kaynakları oluşturma ve gerekli izinleri uygulama](./media/offline-backup-azure-data-box/creating-resources.png)

   MARS Aracısı daha sonra Teslim durumundaki abonelikteki Veri Kutusu işlerini getirir.

    ![Abonelik kimliği için Veri Kutusu'nun işlerini getir](./media/offline-backup-azure-data-box/fetching-databox-jobs.png)

1. Veri Kutusu diskinizi açtığınız, bağlayıp kilidini açtığınız doğru Veri Kutusu sırasını seçin. **Sonraki'ni**seçin.

    ![Veri Kutusu siparişlerini seçin](./media/offline-backup-azure-data-box/select-databox-order.png)

1. **Veri Kutusu Aygıt Algılama** sayfasında Cihazı **Algıla'yı** seçin. Bu eylem, MARS Aracısı'nın yerel olarak bağlı Azure Veri Kutusu disklerini tarayıp algılamasını sağlar.

    ![Veri Kutusu Cihaz Algılama](./media/offline-backup-azure-data-box/databox-device-detection.png)

    Azure Veri Kutusu örneğini ağ paylaşımı olarak bağladıysanız (USB bağlantı noktalarının kullanılamaması veya 100-TB Veri Kutusu aygıtını sipariş edip monte ettiğiniz için), algılama ilk başta başarısız olur. Veri Kutusu aygıtına ağ yolunu girme seçeneği size verilir.

    ![Ağ yolunu girin](./media/offline-backup-azure-data-box/enter-network-path.png)

    >[!IMPORTANT]
    > Ağ yolunu Azure Veri Kutusu diskinin kök dizinine sağlayın. Bu dizin *PageBlob*adına göre bir dizin içermelidir.
    >
    >![Azure Veri Kutusu diskinin kök dizini](./media/offline-backup-azure-data-box/root-directory.png)
    >
    >Örneğin, `\\mydomain\myserver\disk1\` diskin yolu ise ve *disk1* *PageBlob*adlı bir dizin içeriyorsa, MARS `\\mydomain\myserver\disk1\`Aracısı sihirbazı sayfasına girdiğiniz yol .
    >
    >Bir [Azure Veri Kutusu 100-TB aygıtı ayarlarsanız,](#set-up-azure-data-box-devices)aygıta giden ağ yolu olarak girin. `\\<DeviceIPAddress>\<StorageAccountName>_PageBlob`

1. Azure Veri Kutusu'nu kullanarak çevrimdışı yedekleme yapılandırmasıyla yedekleme ve bekletme ilkesini kaydetmek için **Sonraki**ve Sonraki Sayfada **Bitir'i** seçin.

   Aşağıdaki sayfa, ilkenin başarıyla kaydettiğini doğrular.

    ![İlke başarıyla kaydedilir](./media/offline-backup-azure-data-box/policy-saved.png)

1. Önceki sayfada **Kapat'ı** seçin.

1. MARS Agent konsolunun **Eylemler** bölmesinde **Şimdi Geri Al'ı** seçin. Sihirbaz sayfasında **Yukarı Dön'yü** seçin.

    ![Şimdi Yedekle Sihirbazı](./media/offline-backup-azure-data-box/backup-now.png)

MARS Aracısı seçtiğiniz verileri Azure Veri Kutusu aygıtına yedeklemeye başlar. Bu işlem birkaç saat ile birkaç gün arasında sürebilir. Zaman miktarı, MARS Aracısı ile sunucu arasındaki dosya sayısına ve bağlantı hızına bağlıdır.

Verilerin yedeklemesi tamamlandıktan sonra MARS Aracısı'nda buna benzeyen bir sayfa görürsünüz.

![Gösterilen yedekleme ilerlemesi](./media/offline-backup-azure-data-box/backup-progress.png)

## <a name="post-backup-steps"></a>Yedekleme sonrası adımlar

Bu bölümde, verilerin Azure Veri Kutusu Diski'ne yedeklemesi başarılı olduktan sonra atılacak adımlar açıklanmaktadır.

* [Azure Veri Kutusu diskini Azure'a sevk](https://docs.microsoft.com/azure/databox/data-box-disk-deploy-picked-up)etmek için bu makaledeki adımları izleyin. Bir Azure Veri Kutusu 100-TB aygıtı kullandıysanız, [Azure Veri Kutusu aygıtını Azure'a sevk](https://docs.microsoft.com/azure/databox/data-box-deploy-picked-up)etmek için aşağıdaki adımları izleyin.

* [Azure portalında Veri Kutusu işini izleyin.](https://docs.microsoft.com/azure/databox/data-box-disk-deploy-upload-verify) Azure Veri Kutusu işi tamamlandıktan sonra, MARS Aracısı verileri otomatik olarak depolama hesabından bir sonraki zamanlanan yedekleme sırasında Kurtarma Hizmetleri kasasına taşır. Daha sonra bir kurtarma noktası başarıyla oluşturulursa, *Tamamlanmamış Olarak* yedekleme işi işaretler.

    >[!NOTE]
    >MARS Aracısı, ilke oluşturma sırasında zamanlanan zamanlarda yedeklemeleri tetikler. Bu işler, iş bitene kadar "Azure Veri Kutusu işinin tamamlanmasını bekler" olarak işaretlenir.

* MARS Aracısı ilk yedeklemeye karşılık gelen bir kurtarma noktasını başarıyla oluşturduktan sonra, depolama hesabını veya Azure Veri Kutusu işiyle ilişkili belirli içerikleri silebilirsiniz.

## <a name="troubleshooting"></a>Sorun giderme

Microsoft Azure Yedekleme (MAB) Aracısı, kiracınızda sizin için bir Azure Etkin Dizin (Azure AD) uygulaması oluşturur. Bu uygulama, çevrimdışı tohumlama ilkesini yapılandırdığınızda oluşturulan ve yüklenen kimlik doğrulama için bir sertifika gerektirir. Sertifikayı oluşturmak ve Azure AD uygulamasına yüklemek için Azure PowerShell'i kullanıyoruz.

### <a name="problem"></a>Sorun

Çevrimdışı yedeklemeyi yapılandırdığınızda, Azure PowerShell cmdlet'indeki bir hata nedeniyle bir sorunla karşılaşabilirsiniz. MAB Aracısı tarafından oluşturulan aynı Azure REKLAM uygulamasına birden çok sertifika ekleyemiyor olabilirsiniz. Aynı veya farklı bir sunucu için çevrimdışı tohumlama ilkesini yapılandırırsanız, bu sorun sizi etkiler.

### <a name="verify-if-the-problem-is-caused-by-this-specific-root-cause"></a>Sorunun bu belirli kök nedenden kaynaklanmasından kaynaklanırsa doğrulama

Sorununuzun daha önce açıklananla aynı olup olmadığını görmek için aşağıdaki adımlardan birini yapın.

#### <a name="step-1"></a>1. Adım

Çevrimdışı yedeklemeyi yapılandırdığınızda aşağıdaki hata iletisinin MAB konsolunda görünip görünmeip görünmeyip görünmeyip görünmeyip görüntüleninceye cereyan edip olmadığını denetleyin.

![Geçerli Azure hesabı için Çevrimdışı Yedekleme ilkesi oluşturulamıyor](./media/offline-backup-azure-data-box/unable-to-create-policy.png)

#### <a name="step-2"></a>2. Adım

1. Yükleme yolunda **Geçici** klasörü açın. Varsayılan geçici klasör yolu *C:\Program Files\Microsoft Azure Kurtarma Hizmetleri Aracısı\Geçicidir.* *CBUICurr* dosyasını arayın ve dosyayı açın.

1. *CBUICurr* dosyasında, son satıra gidin ve sorunun bu hata iletisindekiyle aynı `Unable to create an Azure AD application credential in customer's account. Exception: Update to existing credential with KeyId <some guid> is not allowed`olup olmadığını denetleyin: .

### <a name="workaround"></a>Geçici çözüm

Bu sorunu gidermek için geçici çözüm olarak, aşağıdaki adımları yapın ve ilke yapılandırmasını yeniden deneyin.

#### <a name="step-1"></a>1. Adım

İthalat veya dışa aktarma işini oluşturabilecek abonelikte yönetici erişimine sahip farklı bir hesap kullanarak MAB UI'da görünen PowerShell'de oturum açın.

#### <a name="step-2"></a>2. Adım

Başka hiçbir sunucuda çevrimdışı tohumlama yapılandırılmamışsa `AzureOfflineBackup_<Azure User Id>` ve başka hiçbir sunucu uygulamaya bağlı değilse, bu uygulamayı silin. **Azure portalı** > **Azure Active Directory** > **App kayıtlarını**seçin.

>[!NOTE]
> Uygulamanın `AzureOfflineBackup_<Azure User Id>` başka bir çevrimdışı tohumlama yapılandırılan ve ayrıca başka bir sunucu bu uygulamaya bağlı olup olmadığını görmek için denetleyin. **Ortak Anahtarlar** bölümünün altındaki **Ayarlar** > **Tuşları'na** gidin. Başka ortak anahtarlar eklenmemelidir. Başvuru için aşağıdaki ekran görüntüsüne bakın.
>
>![Ortak anahtarlar](./media/offline-backup-azure-data-box/public-keys.png)

#### <a name="step-3"></a>3. Adım

Çevrimdışı yedekleme için yapılandırmaya çalıştığınız sunucudan aşağıdaki işlemleri gerçekleştirin.

1. **Bilgisayar sertifikası uygulamasını** > yönet'e gidin**Kişisel** sekmesine gidin `CB_AzureADCertforOfflineSeeding_<ResourceId>`ve adı olan sertifikayı arayın.

2. Sertifikayı seçin, **Tüm Görevler'i**sağ tıklatın ve .cer biçiminde özel bir anahtar olmadan **Dışa Aktar'ı** seçin.

3. Adım 2'de belirtilen Azure çevrimdışı yedekleme uygulamasına gidin. **Ayarlar** > **Tuşlarını** > Genel**Anahtar Yükle'yi**seçin. Önceki adımda dışa aktardığınız sertifikayı yükleyin.

    ![Ortak anahtarı yükleme](./media/offline-backup-azure-data-box/upload-public-key.png)

4. Sunucuda, çalışma penceresine **regedit** girerek kayıt defterini açın.

5. *Kayıt defteribilgisayar\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Yedekleme\Config\CloudBackupProvider gidin.* **CloudBackupProvider'ı**sağ tıklatın ve adı `AzureADAppCertThumbprint_<Azure User Id>`ile yeni bir dize değeri ekleyin.

    >[!NOTE]
    > Azure kullanıcı kimliğini almak için aşağıdaki eylemlerden birini gerçekleştirin:
    >
    >* Azure'a bağlı PowerShell'den `Get-AzureRmADUser -UserPrincipalName "Account Holder's email as defined in the portal"` komutu çalıştırın.
    > * Kayıt defteri yoluna gidin *Bilgisayar\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Yedekleme\DbgSettings\OnlineBackup* adı *CurrentUserId*ile.

6. Önceki adımda eklenen dize sağ tıklatın ve **Değiştir'i**seçin. Değer olarak, adım 2'de dışa aktarılan sertifikanın parmak izini sağlayın. **Tamam'ı**seçin.

7. Parmak izinin değerini almak için sertifikayı çift tıklatın. **Ayrıntılar** sekmesini seçin ve parmak izi alanını görene kadar aşağı kaydırın. **Thumbprint'i**seçin ve değeri kopyalayın.

    ![Sertifikanın parmak izi alanı](./media/offline-backup-azure-data-box/thumbprint-field.png)

## <a name="questions"></a>Sorular

Karşılaştığınız herhangi bir sorun la ilgili herhangi [AskAzureBackupTeam@microsoft.com](mailto:AskAzureBackupTeam@microsoft.com)bir soru veya açıklama için, iletişim.
