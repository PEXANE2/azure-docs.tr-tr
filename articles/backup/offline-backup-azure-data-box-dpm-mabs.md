---
title: DPM ve MABS için Azure Data Box çevrimdışı yedekleme
description: DPM 'den ve MABS 'den çevrimdışı olarak ilk yedekleme verilerini temel almak için Azure Data Box kullanabilirsiniz.
ms.topic: conceptual
ms.date: 08/12/2020
ms.openlocfilehash: 3755a4cc77455b37ae1545247992a3c8c4f25653
ms.sourcegitcommit: 3fb5e772f8f4068cc6d91d9cde253065a7f265d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89177695"
---
# <a name="offline-seeding-using-azure-data-box-for-dpm-and-mabs-preview"></a>DPM ve MABS için Azure Data Box kullanarak çevrimdışı dengeli dağıtım (Önizleme)

> [!NOTE]
> Bu özellik Data Protection Manager (DPM) 2019 UR2 ve üzeri için geçerlidir.<br><br>
> Bu özellik şu anda Microsoft Azure Backup Server (MABS) için önizleme aşamasındadır. MABS ile çevrimdışı dağıtım için Azure Data Box kullanmak istiyorsanız, bizden bize ulaşın [systemcenterfeedback@microsoft.com](mailto:systemcenterfeedback@microsoft.com) .

Bu makalede, DPM ve MABS 'den bir Azure kurtarma hizmetleri kasasına çevrimdışı olarak ilk yedekleme verilerini tohum için Azure Data Box nasıl kullanabileceğiniz açıklanmaktadır.

Büyük ilk DPM/MABS yedeklerinizi bir kurtarma hizmetleri kasasına çevrimdışı olarak (ağ kullanmadan) temel almak için [Azure Data Box](https://docs.microsoft.com/azure/databox/data-box-overview) kullanabilirsiniz. Bu işlem, büyük miktarlarda yedekleme verilerinin yüksek gecikmeli bir ağ üzerinden çevrimiçi olarak harcanmasına neden olacak zaman ve ağ bant genişliğini kaydeder. Bu özellik şu anda önizleme sürümündedir.

Azure Data Box tabanlı çevrimdışı yedekleme [, Azure içeri/dışarı aktarma hizmeti 'ni temel alan çevrimdışı yedekleme](backup-azure-backup-server-import-export.md)üzerinde iki ayrı avantaj sağlar:

- Azure ile uyumlu disklerinizi ve bağlayıcılarınızı temin etmeniz gerekmez. Azure Data Box seçili [Data Box SKU 'su](https://azure.microsoft.com/services/databox/data/)ile ilişkili diskleri sevk eder.

- Azure Backup (MARS Aracısı), Azure Data Box desteklenen SKU 'Lara doğrudan yedekleme verileri yazabilir. Bu özellik, ilk yedekleme verileriniz için bir hazırlama konumu sağlamanıza yönelik gereksinimi ortadan kaldırır. Ayrıca, bu verileri disklere göre biçimlendirmek ve kopyalamak için yardımcı programlara gerek kalmaz.

## <a name="supported-platforms"></a>Desteklenen platformlar

Aşağıdaki platformlar desteklenir:

- Windows Server 2019 64 bit (Standard, Datacenter, Essentials)
- Windows Server 2016 64 bit (Standard, Datacenter, Essentials)

## <a name="backup-data-size-and-supported-data-box-skus"></a>Yedekleme veri boyutu ve desteklenen Data Box SKU 'Lar

Aşağıdaki Data Box SKU 'Lar desteklenir:

| Sunucu başına yedekleme veri boyutu (MARS tarafından sıkıştırmadan sonra) \* | Desteklenen Azure Data Box SKU 'SU |
| --- | --- |
| \<= 7,2 TB | [Azure Data Box Disk](https://docs.microsoft.com/azure/databox/data-box-disk-overview) |
| > 7,2 TB ve <= 80 TB\*\* | [Azure Data Box (100 TB)](https://docs.microsoft.com/azure/databox/data-box-overview) |

\*Tipik sıkıştırma ücretleri% 10-20 arasında farklılık gösterir <br>
\*\*[SystemCenterFeedback@microsoft.com](mailto:SystemCenterFeedback@microsoft.com)Tek bir veri kaynağı için 80 TB 'den fazla ilk yedekleme verisi olmasını bekleseniz, öğesine ulaşın.

> [!IMPORTANT]
> Tek bir veri kaynağından ilk yedekleme verileri tek bir Azure Data Box veya Azure Data Box disk içinde bulunmalıdır ve aynı veya farklı SKU 'ların birden çok cihazı arasında paylaşılamaz. Ancak, bir Azure Data Box birden çok veri kaynağından ilk yedeklemeleri içerebilir.

## <a name="before-you-begin"></a>Başlamadan önce

DPM/MABS üzerinde çalışan MARS Aracısı [en son sürüme](https://aka.ms/azurebackup_agent) (2.0.9171.0 veya üzeri) yükseltilmelidir.

Aşağıdakilerden emin olun:

### <a name="azure-subscription-and-required-permissions"></a>Azure aboneliği ve gerekli izinler

- Geçerli bir Azure aboneliği.
- Çevrimdışı yedekleme ilkesini gerçekleştirmeyi amaçlayan Kullanıcı, Azure aboneliğinin sahibi olmalıdır.
- Verilerin hazırlanması gereken Data Box işi ve kurtarma hizmetleri Kasası aynı aboneliklerde kullanılabilir olmalıdır.
    > [!NOTE]
    > Hedef depolama hesabının ve kurtarma hizmetleri kasasının aynı bölgede olması önerilir. Ancak bu zorunlu değildir.

### <a name="order-and-receive-the-data-box-device"></a>Data Box cihazı sıralama ve alma

Çevrimdışı yedeklemeyi tetiklemeden önce gerekli Data Box cihazların *teslim edilmiş* durumda olduğundan emin olun. Gereksiniminize uygun SKU 'YU sıralamak için bkz. [yedekleme verileri boyutu ve desteklenen Data Box SKU 'ları](#backup-data-size-and-supported-data-box-skus) . Data Box cihazlarınızı sıralamak ve almak için [Bu makaledeki](https://docs.microsoft.com/azure/databox/data-box-disk-deploy-ordered) adımları izleyin.

> [!IMPORTANT]
> **Hesap türü**Için *blobstorage* ' ı seçmeyin. DPM/MABS sunucusu, *Blobstorage* seçildiğinde desteklenmeyen sayfa bloblarını destekleyen bir hesap gerektirir. Azure Data Box işiniz için hedef depolama hesabı oluştururken, **Hesap türü** olarak **Storage v2 'yi (genel amaçlı v2)** seçin.

![Azure veri kutusunu kurma](./media/offline-backup-azure-data-box-dpm-mabs/setup-azure-databox.png)

## <a name="setup-azure-data-box-devices"></a>Azure Data Box cihazları kurma

Azure Data Box cihazı aldıktan sonra, sipariş ettiğiniz Azure Data Box SKU 'suna bağlı olarak, DPM/MABS sunucusu için Data Box cihazlarını ayarlamak ve ilk yedekleme verilerini aktarmak üzere hazırlamak üzere aşağıdaki ilgili bölümlerde verilen adımları gerçekleştirin.

### <a name="setup-azure-data-box-disk"></a>Azure Data Box disk ayarlama

Bir veya daha fazla Azure Data Box disk sipariş ederseniz (her biri 8 TB 'a kadar), Data Box diskinizin paketini açmak, bağlanmak ve kilidini açmak için [burada](https://docs.microsoft.com/azure/databox/data-box-disk-deploy-set-up) bahsedilen adımları izleyin.

> [!NOTE]
> DPM/MABS sunucusunda bir USB bağlantı noktası yok olabilir. Böyle bir senaryoda, Azure Data Box diskinizi başka bir sunucuya/istemciye bağlayabilirsiniz ve cihazın kökünü bir ağ paylaşma olarak kullanıma sunabilirsiniz.

## <a name="setup-azure-data-box"></a>Kurulum Azure Data Box

Bir Azure Data Box sipariş ederseniz (100 TB 'a kadar), Data Box ayarlamak için [burada](https://docs.microsoft.com/azure/databox/data-box-deploy-set-up) bahsedilen adımları izleyin.

### <a name="mount-your-azure-data-box-as-local-system"></a>Azure Data Box yerel sistem olarak bağlama

DPM/MABS sunucusu sistem bağlamında çalışır ve bu nedenle Azure Data Box bağlandığı bağlama yoluna aynı ayrıcalık düzeyinin sağlanması gerekir. Data Box cihazınızı NFS protokolünü kullanarak yerel sistem olarak bağlayabilmeniz için aşağıdaki adımları izleyin.

1. DPM/MABS sunucusunda NFS Istemcisi özelliğini etkinleştirin.
Alternatif kaynak belirtin: *WIM: D: \Sources\ınstall.exe: 4*
2. **PsExec** ' den [https://download.sysinternals.com/files/PSTools.zip](https://download.sysinternals.com/files/PSTools.zip) DPM/mabs sunucusuna indirin.
3. Yükseltilmiş bir komut istemi açın ve geçerli dizin olarak *PSExec.exe* içeren dizin ile aşağıdaki komutu yürütün.

   ```cmd
   psexec.exe  -s  -i  cmd.exe
   ```

4. Yukarıdaki komutun sonucu olarak açılan komut penceresi yerel sistem bağlamında olur. Azure Sayfa Blobu paylaşımından bir ağ sürücüsü olarak Windows Server 'a bağlama adımlarını yürütmek için bu komut penceresini kullanın.
5. DPM/MABS sunucunuzu NFS aracılığıyla Data Box cihazına bağlamak ve Azure sayfa Bloblarını paylaşmak için yerel sistem komut isteminde aşağıdaki komutu yürütmek için [buradaki](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data-via-nfs#connect-to-data-box) adımları izleyin:

    ```cmd
    mount -o nolock \\<DeviceIPAddres>\<StorageAccountName_PageBlob X:
    ```

6. Bağlandıktan sonra, sunucunuza X: erişiminiz olup olmadığını kontrol edin. Bunu yapmak için, bu makalenin sonraki bölümüne devam edin.

## <a name="transfer-initial-backup-data-to-azure-data-box-devices"></a>İlk yedekleme verilerini Azure Data Box cihazlara aktarma

1. DPM/MABS sunucunuzda, [Yeni bir koruma grubu oluşturmak](https://docs.microsoft.com/system-center/dpm/create-dpm-protection-groups?view=sc-dpm-2019)için aşağıdaki adımları izleyin. Var olan koruma grubuna bir çevrimiçi koruma ekliyorsanız, mevcut koruma grubuna sağ tıklayın ve **çevrimiçi koruma ekle** ' yi seçin ve **8. adımdan**başlayın.
2. **Grup üyelerini seçin** sayfasında, yedeklemek istediğiniz bilgisayarları ve kaynakları belirtin.
3. **Veri koruma yöntemini seçin** sayfasında, kısa ve uzun süreli yedeklemeyi nasıl işlemek istediğinizi belirtin. **Çevrimiçi koruma** istiyorum ' u seçtiğinizden emin olun.

   ![Yeni koruma grubu oluştur](./media/offline-backup-azure-data-box-dpm-mabs/create-new-protection-group.png)

4. **Kısa vadeli hedefleri seçin** sayfasında, diskte kısa süreli depolamaya nasıl yedekleme yapmak istediğinizi belirtin.
5. **Disk ayırmayı İncele** sayfasında, koruma grubu için ayrılmış depolama havuzu disk alanını gözden geçirin.
6. **Çoğaltma oluşturma yöntemini seçin** sayfasında, **ağ üzerinden otomatik olarak** ' ı seçin.
7. **Tutarlılık denetimi seçenekleri seçin** sayfasında, tutarlılık denetimlerinin nasıl otomatikleştirilmesi istediğinizi seçin.
8. **Çevrimiçi koruma verilerini belirtin** sayfasında, çevrimiçi korumayı etkinleştirmek istediğiniz üyeyi seçin.

    ![Çevrimiçi koruma verilerini belirtin](./media/offline-backup-azure-data-box-dpm-mabs/specify-online-protection-data.png)

9. **Çevrimiçi yedekleme zamanlamasını belirtin** sayfasında, Azure 'a artımlı yedeklemelerin ne sıklıkta gerçekleşeceğini belirtin.
10. **Çevrimiçi saklama Ilkesini belirtin** sayfasında günlük/haftalık/aylık/yıllık yedeklerden oluşturulan kurtarma noktalarının Azure 'da nasıl korunacağını belirtin.
11. Sihirbazın **çevrimiçi çoğaltma ekranını seçin** ekranında, **Microsoft 'un sahip olduğu diskleri kullanarak aktar** seçeneğini belirleyin ve **İleri ' yi**seçin.

    ![İlk çevrimiçi çoğaltma seçin](./media/offline-backup-azure-data-box-dpm-mabs/choose-initial-online-replication.png)

    >[!NOTE]
    > **Microsoft 'un sahip olduğu diskleri kullanarak Aktarım** seçme seçeneği, özelliğin önizleme aşamasında olduğundan mabs v3 için kullanılamaz. [systemcenterfeedback@microsoft.com](mailto:systemcenterfeedback@microsoft.com)MABS v3 için bu özelliği kullanmak istiyorsanız, bizden bize ulaşın.

12. İstendiğinde, Azure aboneliğinde sahip erişimi olan kullanıcı kimlik bilgilerini kullanarak Azure 'da oturum açın. Başarılı bir oturum açma işleminden sonra aşağıdaki ekran görüntülenir:

    ![Başarıyla oturum açtıktan sonra](./media/offline-backup-azure-data-box-dpm-mabs/after-successful-login.png)

     DPM/MABS sunucusu daha sonra, *teslim edilen* durumda olan abonelikte Data Box işleri getirir.

     > [!NOTE]
     > İlk kez oturum açma normalden daha uzun sürer. Azure PowerShell modülü arka planda yüklenir ve Azure AD uygulaması da kaydedilir.
     >
     >  - Aşağıdaki PowerShell modülleri yüklendi:<br>
          -Azurerd. Profile     *5.8.3*<br>
          -Azurerd. Resources   *6.7.3*<br>
          -Azurerd. Storage     *5.2.0*<br>
          -Azure. Storage       *4.6.1*<br>
     >  - Azure AD uygulaması *AzureOfflineBackup_ \<object GUID of the user> *olarak kaydedilir.

13. Data Box diskinizin açılacağı, bağlandığı ve kilidinin açılacağı doğru veri kutusu sırasını seçin. **İleri**’yi seçin.

    ![Veri kutusunu seçin](./media/offline-backup-azure-data-box-dpm-mabs/select-databox.png)

14. **Veri kutusunu Algıla** ekranında Data Box cihazınızın yolunu girin ve **cihazı Algıla**' yı seçin.

    ![Ağ yolunu girin](./media/offline-backup-azure-data-box-dpm-mabs/enter-network-path.png)

    > [!IMPORTANT]
    > Azure Data Box diskinin kök dizinine ağ yolunu sağlayın. Bu dizin, aşağıda gösterildiği gibi *Pageblob* adlı bir dizin içermelidir:
    >
    > ![USB sürücü](./media/offline-backup-azure-data-box-dpm-mabs/usb-drive.png)
    >
    > Örneğin, diskin yolu `\\mydomain\myserver\disk1\` ve *Disk1* , *pageblob*ADLı bir dizin içeriyorsa, DPM/mabs sunucu Sihirbazı 'nda sağlanacak yol olur `\\mydomain\myserver\disk1\` .
    > [Azure Data Box 100 TB 'lik bir cihaz](https://docs.microsoft.com/azure/backup/offline-backup-azure-data-box#setup-azure-data-box)ayarlarsanız, cihazın ağ yolu olarak aşağıdakileri sağlayın `\\<DeviceIPAddress>\<StorageAccountName>_PageBlob` .

15. **İleri**’yi seçin. **Özet** sayfasında, ayarlarınızı gözden geçirin ve **Grup Oluştur**' u seçin.

    ![Veri kutusunu Algıla](./media/offline-backup-azure-data-box-dpm-mabs/detect-databox.png)

    Aşağıdaki ekran, koruma grubunun başarıyla oluşturulduğunu onaylar.

    ![Koruma grubu oluşturuldu](./media/offline-backup-azure-data-box-dpm-mabs/protection-group-created.png)

16. Yukarıdaki ekranda **Kapat** ' ı seçin.

    Bununla birlikte, verilerin ilk çoğaltması DPM/MABS diskinde oluşur. Koruma tamamlandığında, Grup durumu **koruma sayfasında koruma** durumunu **Tamam** olarak gösterir.

17. Azure Data Box cihazınıza çevrimdışı yedekleme kopyasını başlatmak için **koruma grubuna**sağ tıklayın ve ardından **Kurtarma noktası oluştur** seçeneğini belirleyin. Sonra **çevrimiçi koruma** seçeneğini belirleyin.

    ![Kurtarma noktası oluştur](./media/offline-backup-azure-data-box-dpm-mabs/create-recovery-point.png)

    ![Kurtarma noktası](./media/offline-backup-azure-data-box-dpm-mabs/recovery-point.png)

   DPM/MABS sunucusu, seçtiğiniz verileri Azure Data Box cihazına yedeklemeye başlayacaktır. Bu işlem, DPM/MABS sunucusu ile Azure Data Box Disk arasındaki verilerin boyutuna ve bağlantı hızına bağlı olarak birkaç saatten birkaç güne kadar sürebilir.

   **İzleme** bölmesinde işin durumunu izleyebilirsiniz. Verilerin yedeklenmesi tamamlandıktan sonra, aşağıdakine benzer bir ekran görürsünüz:

   ![Yönetici Konsolu](./media/offline-backup-azure-data-box-dpm-mabs/administrator-console.png)

## <a name="post-backup-steps"></a>Yedekleme sonrası adımlar

Azure Data Box Disk veri yedeklemesi başarılı olduktan sonra bu adımları izleyin.

- Azure Data Box diskini Azure 'a göndermek için [Bu makaledeki](https://docs.microsoft.com/azure/databox/data-box-disk-deploy-picked-up) adımları izleyin. Azure Data Box 100-TB bir cihaz kullandıysanız, Azure Data Box Azure 'a göndermek için [aşağıdaki adımları](https://docs.microsoft.com/azure/databox/data-box-deploy-picked-up) izleyin.
- Azure portal [Data Box Işini izleyin](https://docs.microsoft.com/azure/databox/data-box-disk-deploy-upload-verify) . Azure Data Box işi *tamamlandıktan*sonra DPM/mabs sunucusu, bir sonraki zamanlanmış yedekleme sırasında verileri depolama hesabından kurtarma hizmetleri kasasına otomatik olarak taşıdır. Ardından, bir kurtarma noktası başarıyla oluşturulduysa yedekleme işini *Iş tamamlandı* olarak işaretler.

  > [!NOTE]
  > DPM/MABS sunucusu, koruma grubu oluşturma sırasında zamanlandığı zamanlarda yedeklemeleri tetikler. Ancak, bu işler, iş tamamlanana kadar *Azure Data Box işin tamamlanmasını beklemek* için bayrak uygulanır.

- DPM/MABS sunucusu, ilk yedeklemeye karşılık gelen bir kurtarma noktasını başarıyla oluşturduktan sonra, Azure Data Box işiyle ilişkili depolama hesabını (veya belirli içeriği) silebilirsiniz.

## <a name="troubleshooting"></a>Sorun giderme

DPM sunucusundaki Microsoft Azure Backup (MAB) Aracısı, kiracınızda sizin için bir Azure AD uygulaması oluşturur. Bu uygulama, çevrimdışı dengeli dağıtım ilkesi yapılandırılırken oluşturulup karşıya yüklenen kimlik doğrulaması için bir sertifika gerektirir.

Sertifikayı oluşturmak ve Azure AD uygulamasına yüklemek için Azure PowerShell kullanırız.

### <a name="issue"></a>Sorun

Azure PowerShell cmdlet 'inde bilinen bir kod hatası nedeniyle çevrimdışı yedeklemeyi yapılandırma sırasında, MAB Aracısı tarafından oluşturulan aynı Azure AD uygulamasına birden çok sertifika ekleyemedik. Bu işlem, aynı veya farklı bir sunucu için çevrimdışı bir dengeli dağıtım ilkesi yapılandırdıysanız sizi etkiler.

### <a name="verify-if-the-issue-is-caused-by-this-specific-root-cause"></a>Sorunun bu özel kök nedenin kaynaklanıp kaynaklanmadığını doğrulayın

Hatanın Yukarıdaki [sorundan](#issue) dolayı olduğundan emin olmak için aşağıdaki adımlardan birini gerçekleştirin:

#### <a name="step-1"></a>1. Adım

Çevrimdışı yedeklemeyi yapılandırma sırasında DPM/MABS konsolunda aşağıdaki hata iletisini görüp görmenizi denetleyin:

![Azure kurtarma hizmetleri Aracısı](./media/offline-backup-azure-data-box-dpm-mabs/azure-recovery-services-agent.png)

#### <a name="step-2"></a>2. Adım

1. Yükleme yolundaki **geçici** klasörü açın (varsayılan Temp klasörü yolu *C:\Program Files\Microsoft Azure Recovery Services \Temp*' dir. *Cbuicurr* dosyasını bulun ve dosyayı açın.
2. *Cbuicurr* dosyasında, son satıra ilerleyin ve hatanın "müşteri hesabındaki BIR Azure AD uygulama kimlik bilgisi oluşturulamıyor. Özel durum: anahtar kimliği ile mevcut kimlik bilgisine güncelleştirme \<some guid> yapılmasına izin verilmiyor.

### <a name="workaround"></a>Geçici çözüm

Bu sorunu çözmek için, aşağıdaki adımları uygulayın ve ilke yapılandırmasını yeniden deneyin.

1. İçeri aktarma verme işinin oluşturulduğu abonelikte yönetici erişimi olan farklı bir hesap kullanarak DPM/MABS Server Kullanıcı arabiriminde görüntülenen Azure oturum açma sayfasında oturum açın.
2. Başka bir sunucuda, çevrimdışı dengeli dağıtım yapılandırılmamışsa ve uygulamaya bağımlı başka bir sunucu yoksa `AzureOfflineBackup_<Azure User Id>` , bu uygulamayı **Azure portal > Azure Active Directory > uygulama kayıtları**silin.

   > [!NOTE]
   > Uygulamanın `AzureOfflineBackup_<Azure User Id>` başka bir çevrimdışı dengeli dağıtım olup olmadığını ve aynı zamanda bu uygulamaya bağımlı başka bir sunucu olmadığını denetleyin. Ortak anahtarlar bölümü altındaki **ayarlar > anahtarlar** ' a gidin. Başka hiçbir **ortak anahtar** eklenmelidir. Başvuru için aşağıdaki ekran görüntüsüne bakın:
   >
   > ![Ortak anahtarlar](./media/offline-backup-azure-data-box-dpm-mabs/public-keys.png)

#### <a name="step-3"></a>3. Adım

Çevrimdışı yedeklemeyi yapılandırmaya çalıştığınız DPM/MABS sunucusundan aşağıdaki işlemleri yapın:

1. **Bilgisayar sertifikası uygulaması**  >  **Kişisel** sekmesini açın ve adına sahip sertifikayı arayın `CB_AzureADCertforOfflineSeeding_<ResourceId>` .
2. Yukarıdaki sertifikayı seçin, **Tüm görevler** ' e sağ tıklayın ve özel anahtar olmadan. cer biçiminde **dışarı aktarın** .
3. **2. noktada**belirtilen Azure çevrimdışı yedekleme uygulamasına gidin. **Ayarlar**  >  **anahtarlar**  >  **ortak anahtarı karşıya yükle** bölümünde, yukarıdaki adımda dışarıya aktarılmış sertifikayı karşıya yükleyin.

   ![Ortak anahtarları karşıya yükle](./media/offline-backup-azure-data-box-dpm-mabs/upload-public-keys.png)

4. Sunucusunda, **Çalıştır** penceresine **Regedit** yazarak kayıt defterini açın.
5. *Bilgisayar \ HKEY \_ Local \_ Machıne\software\microsoft\windows Azure Backup\Config\CloudBackupProvider*kayıt defterine gidin. **CloudBackupProvider** öğesine sağ tıklayın ve adında yeni bir dize değeri ekleyin `AzureADAppCertThumbprint_<Azure User Id>` .

    >[!NOTE]
    > Azure kullanıcı KIMLIĞINI almak için şu eylemlerden birini gerçekleştirin:
    >
    >- Azure bağlı PowerShell 'den `Get-AzureRmADUser -UserPrincipalName "Account Holder's email as defined in the portal"` komutunu çalıştırın.
    > - `Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\DbgSettings\OnlineBackup` *Currentuserıd*adlı kayıt defteri yoluna gidin.

6. Yukarıdaki adımda eklenen dizeye sağ tıklayın ve **Değiştir**' i seçin. Değerde, **2. noktada** verdiğiniz sertifikanın parmak Izini girip **Tamam**' ı seçin.
7. Parmak izi değerini almak için, sertifikaya çift tıklayın, ardından **Ayrıntılar**  ' ı seçin ve ardından parmak izi alanını görene kadar aşağı kaydırın. **Parmak izi** ' ni seçin ve değeri kopyalayın.

   ![Parmak izi değeri](./media/offline-backup-azure-data-box-dpm-mabs/certificate.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Kendi diskini kullanan çevrimdışı dengeli dağıtım (Azure Içeri/dışarı aktarma hizmeti kullanılarak)](backup-azure-backup-server-import-export.md)
