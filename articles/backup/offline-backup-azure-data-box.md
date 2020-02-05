---
title: Azure Data Box kullanarak çevrimdışı yedekleme
description: MARS aracısından bir Azure kurtarma hizmetleri kasasına çevrimdışı olan büyük ilk yedekleme verilerini temel alarak Azure Data Box nasıl kullanabileceğinizi öğrenin.
ms.topic: conceptual
ms.date: 1/27/2020
ms.openlocfilehash: 553939df8aa7a1b097b2cd6c7f29365302101324
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77027036"
---
# <a name="azure-backup-offline-backup-using-azure-data-box"></a>Azure Data Box kullanarak çevrimdışı yedekleme Azure Backup

Büyük ilk MARS yedeklerinizi bir Azure kurtarma hizmetleri kasasına çevrimdışı olarak (ağ kullanmadan) temel almak için [Azure Data Box](https://docs.microsoft.com/azure/databox/data-box-overview) hizmetini kullanabilirsiniz.  Bu, zaman ve ağ bant genişliğini kaydeder, aksi takdirde büyük miktarlarda yedekleme verilerinin yüksek gecikmeli bir ağ üzerinden çevrimiçi olarak taşınması önerilir. Bu geliştirme şu anda önizleme aşamasındadır. Azure Data Box tabanlı çevrimdışı yedekleme, [Azure içeri/dışarı aktarma hizmeti tabanlı çevrimdışı yedekleme](https://docs.microsoft.com/azure/backup/backup-azure-backup-import-export)üzerinde iki ayrı avantaj sağlar.

1. Azure ile uyumlu disklerinizi ve bağlayıcılarınızı temin etmeniz gerekmez. Azure Data Box hizmeti, seçili [Data Box SKU 'su](https://azure.microsoft.com/services/databox/data/) ile ilişkili diskleri sevk eder

2. Azure Backup (MARS Aracısı), Azure Data Box desteklenen SKU 'Lara doğrudan yedekleme verileri yazabilir. Bu, ilk yedekleme verileriniz için bir hazırlama konumu sağlama ihtiyacını ve bu verileri disklere biçimlendirme ve bu verilerin kopyalanması için gereken yardımcı programlar gereksinimini ortadan kaldırır.

## <a name="azure-data-box-with-mars-agent"></a>MARS Aracısı ile Azure Data Box

Bu makalede, büyük ilk yedekleme verilerini MARS aracısından bir Azure kurtarma hizmetleri kasasına çevrimdışına almak için Azure Data Box nasıl kullanabileceğiniz açıklanmaktadır. Makale aşağıdaki bölümlere ayrılmıştır:

* Desteklenen platformlar
* Desteklenen Data Box SKU 'Lar
* Önkoşullar
* MARS Aracısı kurulumu
* Kurulum Azure Data Box
* Azure Data Box yedekleme veri aktarımı
* Yedekleme sonrası adımlar

## <a name="supported-platforms"></a>Desteklenen platformlar

Azure Data Box kullanarak MARS aracısından verileri temel alan işlem aşağıdaki Windows SKU 'Larında desteklenir:

| **OS**                                 | **SKU**                                                      |
| -------------------------------------- | ------------------------------------------------------------ |
| **Unuzu**                        |                                                              |
| Windows 10 64 bit                     | Enterprise, Pro, Home                                       |
| Windows 8.1 64 bit                    | Enterprise, Pro                                             |
| Windows 8 64 bit                      | Enterprise, Pro                                             |
| Windows 7 64 bit                      | Ultimate, Enterprise, Professional, Home Premium, Home Basic, Starter |
| **Sunucu**                             |                                                              |
| Windows Server 2019 64 bit            | Standart, veri merkezi, temel parçalar                            |
| Windows Server 2016 64 bit            | Standart, veri merkezi, temel parçalar                            |
| Windows Server 2012 R2 64 bit         | Standard, Datacenter, Foundation                            |
| Windows Server 2012 64 bit            | Veri Merkezi, temel, standart                            |
| Windows Storage Server 2016 64 bit    | Standart, çalışma grubu                                         |
| Windows Storage Server 2012 R2 64 bit | Standart, çalışma grubu, temel                              |
| Windows Storage Server 2012 64 bit    | Standart, çalışma grubu                                         |
| Windows Server 2008 R2 SP1 64 bit     | Standard, Enterprise, Datacenter, Foundation                |
| Windows Server 2008 SP2 64 bit        | Standard, Enterprise, Datacenter                            |

## <a name="backup-data-size-and-supported-data-box-skus"></a>Yedekleme veri boyutu ve desteklenen Data Box SKU 'Lar

| Yedekleme veri boyutu (MARS tarafından sıkıştırmayı Gönder) * sunucu başına | Desteklenen Azure Data Box SKU 'SU                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| < = 7,2 TB                                                    | [Azure Data Box Disk](https://docs.microsoft.com/azure/databox/data-box-disk-overview) |
| > 7,2 TB ve < = 80 TB * *                                      | [Azure Data Box (100 TB)](https://docs.microsoft.com/azure/databox/data-box-overview) |

\* Tipik sıkıştırma ücretleri% 10-20 arasında farklılık gösterir <br>
\* * Tek bir MARS sunucusu için 80 TB 'den fazla ilk yedekleme verisi olmasını bekleseniz [AskAzureBackupTeam@microsoft.com](mailto:AskAzureBackupTeam@microsoft.com) 'a ulaşın.

>[!IMPORTANT]
>Tek bir sunucudan ilk yedekleme verileri tek bir Azure Data Box veya Azure Data Box Disk içermeli ve aynı veya farklı SKU 'ların birden çok cihazı arasında paylaşılamaz. Ancak, Azure Data Box bir cihaz birden çok sunucudan ilk yedeklemeleri içerebilir.

## <a name="pre-requisites"></a>Önkoşullar

### <a name="azure-subscription-and-required-permissions"></a>Azure aboneliği ve gerekli izinler

* İşlem bir Azure aboneliği gerektiriyor
* İşlem, çevrimdışı yedekleme ilkesini gerçekleştirmek üzere atanan kullanıcının Azure aboneliğinin bir "sahibi" olması gerekir
* Data Box işi ve kurtarma hizmetleri kasasının (verilerin sağlaması gereken) aynı aboneliklerde olması gerekir.
* Azure Data Box işiyle ve kurtarma hizmetleri kasasıyla ilişkili hedef depolama hesabının aynı bölgede olması önerilir. Ancak, bu gerekli değildir.

### <a name="get-azure-powershell-370"></a>Azure PowerShell 3.7.0 al

**Bu işlem için en önemli önkoşul budur**. Azure PowerShell (ver) yüklenmeden önce. 3.7.0), aşağıdaki denetimleri gerçekleştirin * *

#### <a name="step-1-check-powershell-version"></a>1\. Adım: PowerShell sürümünü denetleme

* Windows PowerShell’i açın ve aşağıdaki komutu çalıştırın:

    ```powershell
    Get-Module -ListAvailable AzureRM*
    ```

* Çıktı, **3.7.0**'den daha yüksek bir sürüm görüntülerse aşağıdaki 2. adımı gerçekleştirin. Aksi takdirde 3. adıma geçin.

#### <a name="step-2-uninstall-the-powershell-version"></a>2\. Adım: PowerShell sürümünü kaldırma

Aşağıdaki eylemleri gerçekleştirerek PowerShell 'in geçerli sürümünü kaldırın:

* PowerShell 'de aşağıdaki komutu çalıştırarak bağımlı modülleri kaldırın:

    ```powershell
    foreach ($module in (Get-Module -ListAvailable AzureRM*).Name |Get-Unique)  { write-host "Removing Module $module" Uninstall-module $module }
    ```

* Tüm bağımlı modüllerin başarıyla silinmesini sağlamak için aşağıdaki komutu çalıştırın:

    ```powershell
    Get-Module -ListAvailable AzureRM*
    ```

#### <a name="step-3-install-powershell-version-370"></a>3\. Adım: PowerShell sürüm 3.7.0 'Yi yükler

Azurera modüllerinin bulunmadığını doğruladıktan sonra, aşağıdaki yöntemlerden birini kullanarak sürüm 3.7.0 ' yi yükleyebilirsiniz:

* GitHub 'dan, [bağlantısı](https://github.com/Azure/azure-powershell/releases/tag/v3.7.0-March2017)

VEYA

* PowerShell penceresinde aşağıdaki komutu çalıştırın:

    ```powershell
    Install-Module -Name AzureRM -RequiredVersion 3.7.0
    ```

Azure PowerShell, MSI dosyası kullanılarak da yüklenmiş olabilir. Kaldırmak için, Denetim Masası 'ndaki programları Kaldır seçeneğini kullanarak kaldırın.

### <a name="order-and-receive-the-data-box-device"></a>Data Box cihazı sıralama ve alma

MARS ve Azure Data Box kullanan çevrimdışı yedekleme işlemi, MARS Aracısı kullanılarak çevrimdışı yedeklemeyi tetiklemeden önce gerekli Data Box cihazların "teslim edildi" durumunda olmasını gerektirir. Gereksiniminize uygun SKU 'YU sıralamak için [yedekleme veri boyutuna ve desteklenen Data Box SKU 'lara](#backup-data-size-and-supported-data-box-skus) bakın. Data Box cihazlarınızı sıralamak ve almak için [Bu makaledeki](https://docs.microsoft.com/azure/databox/data-box-disk-deploy-ordered) adımları izleyin.

>[!IMPORTANT]
>Hesap türü için BlobStorage ' ı seçmeyin. MARS Aracısı, BlobStorage seçildiğinde desteklenmeyen sayfa bloblarını destekleyen bir hesap gerektirir. Azure Data Box işiniz için hedef depolama hesabı oluştururken, *depolama v2* 'yi (*genel amaçlı v2*) hesap türü olarak seçmenizi önemle tavsiye ederiz.

![Örnek ayrıntılarında hesap türünü seçin](./media/offline-backup-azure-data-box/instance-details.png)

## <a name="install-and-setup-the-mars-agent"></a>MARS aracısını yükleme ve kurma

* MARS aracısının önceki tüm yüklemelerini kaldırdığınızdan emin olun.

* En son MARS aracısını [buradan](https://aka.ms/azurebackup_agent)indirin.

* *Marsagentınstaller. exe* ' yi çalıştırın ve ***yalnızca** yedeklemelerin depolanmasını Istediğiniz kurtarma hizmetleri kasasına [aracıyı yüklemek ve kaydetmek](https://docs.microsoft.com/azure/backup/backup-configure-vault#install-and-register-the-agent) için adımları uygulayın.

  >[!NOTE]
  > Kurtarma Hizmetleri Kasası, Azure Data Box işle aynı abonelikte olmalıdır.

* Aracı kurtarma hizmetleri kasasına kaydedildikten sonra aşağıdaki bölümlerde bulunan adımları izleyin.

## <a name="setup-azure-data-box-devices"></a>Azure Data Box cihazı ayarla

Siparişi verdiğiniz Azure Data Box SKU 'suna bağlı olarak, ilk yedekleme verilerini tanımlamak ve aktarmak üzere MARS aracısının Data Box cihazlarını ayarlamak ve hazırlamak için aşağıdaki uygun bölümlerde anlatılan adımları uygulayın.

### <a name="setup-azure-data-box-disk"></a>Kurulum Azure Data Box Disk

Bir veya daha fazla Azure Data Box disk sipariş ederseniz (her biri 8 TB 'a kadar), [Data Box disk paketini açmak, bağlanmak ve kilidini açmak](https://docs.microsoft.com/azure/databox/data-box-disk-deploy-set-up)için burada bahsedilen adımları izleyin.

>[!NOTE]
>MARS aracısına sahip sunucuda USB bağlantı noktası yok olabilir. Bu durumda, Azure Data Box Disk başka bir sunucuya/istemciye bağlayabilirsiniz ve cihazın kökünü bir ağ paylaşma olarak kullanıma sunabilirsiniz.

### <a name="setup-azure-data-box"></a>Kurulum Azure Data Box

Bir Azure Data Box sipariş ederseniz (100 TB 'a kadar), [Data Box kurmak için](https://docs.microsoft.com/azure/databox/data-box-deploy-set-up)burada bahsedilen adımları izleyin.

#### <a name="mount-your-azure-data-box-as-local-system"></a>Azure Data Box yerel sistem olarak bağlama

MARS Aracısı yerel sistem bağlamında çalışır ve bu nedenle Azure Data Box bağlandığı bağlama yoluna aynı ayrıcalık düzeyinin sağlanması gerekir. Data Box cihazınızı NFS protokolünü kullanarak yerel sistem olarak bağlayabilmeniz için aşağıdaki adımları izleyin:

* Windows Server 'da (MARS Aracısı yüklü olan) NFS Istemcisi özelliğini etkinleştirin.<br>
  Alternatif kaynak belirtin: *WIM: D: \Sources\ınstall.exe: 4*

* <https://download.sysinternals.com/files/PSTools.zip> 'den PSExec 'yi MARS aracısının yüklü olduğu sunucuya indirin.

* Yükseltilmiş bir komut istemi açın ve şu komutu, geçerli dizin olarak PSExec. exe dosyasını içeren dizin ile yürütün:

    ```cmd
    psexec.exe  -s  -i  cmd.exe
    ```

* Yukarıdaki komutun sonucu olarak açılan komut penceresi yerel sistem bağlamında olur. Azure Sayfa Blobu paylaşımından bir ağ sürücüsü olarak Windows Server 'a bağlama adımlarını yürütmek için bu komut penceresini kullanın.

* Sunucunuzu Data Box cihaza NFS aracılığıyla bağlamak için [buradaki](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data-via-nfs#connect-to-data-box) adımları Izleyin ve Azure sayfa blob 'larını bağlamak Için yerel sistem komut isteminde aşağıdaki komutu yürütün:

    ```cmd
    mount -o nolock \\<DeviceIPAddress>\<StorageAccountName_PageBlob X:  
    ```

* Bağlandıktan sonra, sunucunuza X: erişiminiz olup olmadığını kontrol edin. Yanıt Evet ise, bu makalenin sonraki bölümüne devam edin.

## <a name="transfer-initial-backup-data-to-azure-data-box-devices"></a>Ilk yedekleme verilerini Azure Data Box cihaza aktar

* **Microsoft Azure Backup** uygulamanızı sunucunuzda açın.

* **Eylemler** bölmesinde **yedeklemeyi zamanla** ' ya tıklayın.

    ![Yedeklemeyi Zamanla ' ya tıklayın.](./media/offline-backup-azure-data-box/schedule-backup.png)

* **Yedekleme zamanlaması Sihirbazı** 'ndaki adımları izleyin

* Öğelerin toplam boyutunun, sipariş ettiğiniz ve aldığınız [Azure Data Box SKU 'nun desteklediği boyut sınırları](#backup-data-size-and-supported-data-box-skus) dahilinde olması gibi **öğeleri ekleyin** .

    ![Yedeklenecek öğeleri ekle](./media/offline-backup-azure-data-box/add-items.png)

* Dosyalar ve klasörler ve sistem durumu için uygun yedekleme zamanlamasını ve bekletme ilkesini seçin (sistem durumu yalnızca Windows Istemcileri için geçerlidir, Windows Istemcileri için geçerli değildir)

* Sihirbazın **Ilk yedekleme türünü (dosyalar ve klasörler) seçin** ekranında, **Microsoft Azure Data Box disklerini kullanarak aktar** seçeneğini belirleyin ve **İleri** ' ye tıklayın.

    ![İlk yedekleme türünü seçin](./media/offline-backup-azure-data-box/initial-backup-type.png)

* Azure aboneliğinde sahip erişimi olan kullanıcı kimlik bilgilerini kullanarak sorulduğunda Azure 'da oturum açın. Bunu yaptıktan sonra, aşağıdakine benzer bir ekran görmeniz gerekir:

    ![Kaynak oluşturma ve gerekli izinleri uygulama](./media/offline-backup-azure-data-box/creating-resources.png)

* MARS Aracısı daha sonra abonelikte "teslim edilen" durumundaki Data Box işleri getirir.

    ![Abonelik KIMLIĞI için veri kutusu işleri getiriliyor](./media/offline-backup-azure-data-box/fetching-databox-jobs.png)

* Data Box diskinizin açılacağı, bağlandığı ve kilidinin bulunduğu doğru veri kutusu sırasını seçin. **İleri**’ye tıklayın.

    ![Data Box sırasını seçin](./media/offline-backup-azure-data-box/select-databox-order.png)

* **Data Box cihaz algılama** ekranında **cihazı Algıla** ' ya tıklayın. Bu, MARS Aracısı 'nın yerel olarak bağlı Azure Data Box diskleri taramasını ve bunları aşağıda gösterildiği gibi tespit etmelerini sağlar:

    ![Cihaz algılamayı Data Box](./media/offline-backup-azure-data-box/databox-device-detection.png)

    Azure Data Box bir ağ paylaşımıyla (USB bağlantı noktası kullanılamamasından veya 100 TB Data Box cihazı sipariş ettiğiniz ve bağladığınız için) bağlandıysanız, algılama ilk başta başarısız olur, ancak Data Box cihazının ağ yolunu, s olarak girme seçeneği sunar Aşağıdan Hown:

    ![Ağ yolunu girin](./media/offline-backup-azure-data-box/enter-network-path.png)

    >[!IMPORTANT]
    > Azure Data Box diskinin kök dizinine ağ yolunu sağlayın. Bu dizin, aşağıda gösterildiği gibi *Pageblob* adlı bir dizin içermelidir:
    >
    >![Azure Data Box diskin kök dizini](./media/offline-backup-azure-data-box/root-directory.png)
    >
    >Örneğin, diskin yolu `\\mydomain\myserver\disk1\` ve *Disk1* , *pageblob*adlı BIR dizin içeriyorsa, Mars aracısı sihirbazında sağlanacak yol `\\mydomain\myserver\disk1\`
    >
    >[Azure Data Box 100 TB 'lik bir cihaz](#setup-azure-data-box)ayarlarsanız, cihazın ağ yolu olarak aşağıdakileri sağlayın `\\<DeviceIPAddress>\<StorageAccountName>_PageBlob`

* **İleri** ' ye tıklayın ve Azure Data Box kullanarak yedekleme ve bekletme ilkesini çevrimdışı yedekleme yapılandırmasıyla kaydetmek için sonraki ekranda **son** ' a tıklayın.

* Aşağıdaki ekran, ilkenin başarıyla kaydedildiğini onaylar:

    ![İlke başarıyla kaydedildi](./media/offline-backup-azure-data-box/policy-saved.png)

* Yukarıdaki ekranda **Kapat** ' a tıklayın.

* MARS Aracısı konsolunun **Eylemler** bölmesinde ***Şimdi Yedekle** ' ye tıklayın ve sihirbaz ekranında aşağıda gösterildiği gibi **Yedekle** ' ye tıklayın:

    ![Şimdi Yedekle Sihirbazı](./media/offline-backup-azure-data-box/backup-now.png)

* MARS Aracısı, seçtiğiniz verileri Azure Data Box cihazına yedeklemeye başlayacaktır. Bu işlem, MARS Aracısı ve Azure Data Box Disk ile sunucu arasındaki bağlantı hızına bağlı olarak birkaç saatten birkaç güne kadar sürebilir.

* Verilerin yedeklenmesi tamamlandıktan sonra, MARS Aracısı üzerinde aşağıdakine benzer bir ekran görürsünüz:

    ![Yedekleme ilerlemesi gösteriliyor](./media/offline-backup-azure-data-box/backup-progress.png)

## <a name="post-backup-steps"></a>Yedekleme sonrası adımlar

Bu bölümde, verilerin yedeklenmesi Azure Data Box Disk başarılı olduktan sonra gerçekleştirilecek adımlar açıklanmaktadır.

* [Azure Data Box diskini Azure 'a göndermek](https://docs.microsoft.com/azure/databox/data-box-disk-deploy-picked-up)için bu makaledeki adımları izleyin. Azure Data Box 100-TB bir cihaz kullandıysanız, [Azure Data Box Azure 'a göndermek](https://docs.microsoft.com/azure/databox/data-box-deploy-picked-up)için aşağıdaki adımları izleyin.

* Azure portal [Data Box Işini izleyin](https://docs.microsoft.com/azure/databox/data-box-disk-deploy-upload-verify) . Azure Data Box işi "Tamam" olduktan sonra, MARS Aracısı verileri otomatik olarak depolama hesabından kurtarma hizmetleri kasasından bir sonraki zamanlanmış yedekleme sırasında taşır. Ardından, bir kurtarma noktası başarıyla oluşturulduysa yedekleme işini "Iş tamamlandı" olarak işaretler.

    >[!NOTE]
    >MARS Aracısı, ilke oluşturma sırasında zamanlandığı zamanlarda yedeklemeleri tetikler. Ancak bu işler, iş tamamlanana kadar "Azure Data Box işin tamamlanmasını bekleniyor" olarak bayrak eklenir.

* MARS Aracısı ilk yedeklemeye karşılık gelen bir kurtarma noktasını başarıyla oluşturduktan sonra, Azure Data Box işiyle ilişkili depolama hesabını (veya belirli içeriği) silebilirsiniz.

## <a name="troubleshooting"></a>Sorun giderme

Microsoft Azure Backup (MAB) Aracısı, kiracınızda sizin için bir Azure AD uygulaması oluşturur. Bu uygulama, çevrimdışı dengeli dağıtım ilkesi yapılandırılırken oluşturulup karşıya yüklenen kimlik doğrulaması için bir sertifika gerektirir. Sertifikayı oluşturmak ve Azure AD uygulamasına yüklemek için Azure PowerShell kullanırız.

### <a name="issue"></a>Sorun

Çevrimdışı yedeklemeyi yapılandırma sırasında, Azure PowerShell cmdlet 'inde bir hata nedeniyle, MAB Aracısı tarafından oluşturulan aynı Azure AD uygulamasına birden çok sertifika ekleyemeyebilirsiniz bir sorun olabilir. Bu işlem, aynı veya farklı bir sunucu için çevrimdışı dengeli dağıtım ilkesi yapılandırdıysanız sizi etkiler.

### <a name="how-to-verify-if-the-issue-is-caused-by-this-specific-root-cause"></a>Sorunun bu özel kök nedenin kaynaklanıp kaynaklanmadığını doğrulama

Hatanın yukarıdaki sorundan dolayı olduğundan emin olmak için aşağıdaki adımlardan birini gerçekleştirin:

#### <a name="step-1"></a>Adım 1

Çevrimdışı yedeklemeyi yapılandırma sırasında MAB konsolunda aşağıdaki hata iletisini görmenizi denetleyin:

![Geçerli Azure hesabı için çevrimdışı yedekleme ilkesi oluşturulamıyor](./media/offline-backup-azure-data-box/unable-to-create-policy.png)

#### <a name="step-2"></a>Adım 2

* Yükleme yolundaki **geçici** klasörü açın (varsayılan Temp klasörü yolu *C:\Program Files\Microsoft Azure Recovery Services \Temp*' dir). **Cbuicurr** dosyasını bulun ve dosyayı açın.

* **Cbuicurr** dosyasında, son satıra ilerleyin ve hatanın `Unable to create an Azure AD application credential in customer's account. Exception: Update to existing credential with KeyId <some guid> is not allowed`olup olmadığını denetleyin.

### <a name="workaround"></a>Geçici çözüm

Bu sorunu çözmek için geçici bir çözüm olarak, aşağıdaki adımları uygulayın ve ilke yapılandırmasını yeniden deneyin.

#### <a name="first-step"></a>İlk adım

Yük aktarma işi oluşturulan abonelikte yönetici erişimine sahip farklı bir hesap kullanarak MAB Kullanıcı arabiriminde görünen PowerShell 'de oturum açın.

#### <a name="second-step"></a>İkinci adım

Başka bir sunucuda, çevrimdışı dengeli dağıtım yapılandırılmamışsa ve başka bir sunucu `AzureOfflineBackup_<Azure User Id>` uygulamasına bağlı değilse, bu uygulamayı **Azure portal** > **Azure Active Directory** > **uygulama kayıtları silin.**

>[!NOTE]
> Uygulama `AzureOfflineBackup_<Azure User Id>` başka bir çevrimdışı dengeli dağıtım olup olmadığını ve aynı zamanda bu uygulamaya bağımlı başka bir sunucu olmadığını kontrol edin. **Ortak anahtarlar** bölümünde **Ayarlar** > **anahtarlar** ' a gidin, başka bir ortak anahtar eklenmelidir. Başvuru için aşağıdaki ekran görüntüsüne bakın:
>
>![Ortak anahtarlar](./media/offline-backup-azure-data-box/public-keys.png)

#### <a name="third-step"></a>Üçüncü adım

Çevrimdışı yedeklemeyi yapılandırmaya çalıştığınız sunucudan aşağıdaki eylemleri gerçekleştirin:

1. **Bilgisayar sertifikası uygulamasını yönet** > **Kişisel** sekmesini açın ve ada sahip sertifikayı arayın `CB_AzureADCertforOfflineSeeding_<ResourceId>`

2. Yukarıdaki sertifikayı seçin, **Tüm görevler** ' e sağ tıklayın ve özel anahtar olmadan. cer biçiminde **dışarı aktarın** .

3. **2. noktada**belirtilen Azure çevrimdışı yedekleme uygulamasına gidin. **Ayarlar** > **anahtarlar** > **karşıya yükleme ortak anahtarı ' nda,** Yukarıdaki adımda dışarıya alınan sertifikayı karşıya yükleyin.

    ![Ortak anahtarı karşıya yükle](./media/offline-backup-azure-data-box/upload-public-key.png)

4. Sunucusunda, Çalıştır penceresine **Regedit** yazarak kayıt defterini açın.

5. Kayıt defteri bilgisayarına git *\ HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider.* **CloudBackupProvider** ' a sağ tıklayın ve ada sahip yeni bir dize değeri ekleyin `AzureADAppCertThumbprint_<Azure User Id>`

    >[!NOTE]
    > Azure Kullanıcı kimliğini almak için aşağıdaki eylemlerden birini gerçekleştirin:
    >
    >1. Azure bağlı PowerShell 'den `Get-AzureRmADUser -UserPrincipalName “Account Holder’s email as defined in the portal”` komutunu çalıştırın.
    > 2. Kayıt defteri yoluna gidin: *bilgisayar \ HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows Azure Backup\DbgSettings\OnlineBackup*; Ad: *Currentuserıd*

6. Yukarıdaki adımda eklenen dizeye sağ tıklayın ve **Değiştir**' i seçin. Değerde, **1. noktada** verdiğiniz sertifikanın parmak Izini girip **Tamam**' a tıklayın.

7. Parmak izi değerini almak için, sertifikaya çift tıklayın, ardından **Ayrıntılar** sekmesini seçin ve parmak izi alanını görene kadar aşağı kaydırın. **Parmak izi** ' ne tıklayın ve değeri kopyalayın.

    ![Sertifikanın parmak izi alanı](./media/offline-backup-azure-data-box/thumbprint-field.png)

## <a name="questions"></a>Sorular

Tüm sorular veya açıklamalar için, karşılaştığı sorunlarla ilgili olarak [AskAzureBackupTeam@microsoft.com](mailto:AskAzureBackupTeam@microsoft.com) ulaşın
