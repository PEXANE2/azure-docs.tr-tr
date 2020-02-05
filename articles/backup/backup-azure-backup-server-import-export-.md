---
title: DPM ve Azure Backup Sunucusu için çevrimdışı yedekleme
description: Azure Backup, Azure Içeri/dışarı aktarma hizmetini kullanarak ağdan veri göndermenizi sağlar. Bu makalede, DPM ve Azure Backup Sunucusu (MABS) için çevrimdışı yedekleme iş akışı açıklanmaktadır.
ms.reviewer: saurse
ms.topic: conceptual
ms.date: 1/28/2020
ms.openlocfilehash: 6be75062ab0ce06784d8cd7c833e0070476acf60
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77022588"
---
# <a name="offline-backup-workflow-for-dpm-and-azure-backup-server"></a>DPM ve Azure Backup Sunucusu için çevrimdışı yedekleme iş akışı

Azure Backup, Azure 'a verilerin ilk tam yedeklemesi sırasında ağ ve depolama maliyetlerini kaydetmekle ilgili çeşitli yerleşik verimlilik içerir. İlk tam yedeklemeler genellikle büyük miktarda veriyi aktarır ve yalnızca deltas/artımlarsa aktarımı yapan sonraki yedeklemelerle karşılaştırıldığında daha fazla ağ bant genişliği gerektirir. Azure Backup ilk yedeklemeleri sıkıştırır. Azure Backup çevrimdışı dağıtım sürecinde, sıkıştırılmış ilk yedekleme verilerini çevrimdışı olarak Azure 'a yüklemek için diskleri kullanabilir.

Azure Backup çevrimdışı dengeli işlem, Azure [içeri/dışarı aktarma hizmeti](../storage/common/storage-import-export-service.md) ile sıkı bir şekilde tümleştirilir ve diskleri kullanarak Azure 'a veri aktarmanızı sağlar. Yüksek gecikmeli ve düşük bant genişliğine sahip bir ağ üzerinden aktarılması gereken ilk yedekleme verilerinin terabayt (TBs) varsa, ilk yedekleme kopyasını bir veya daha fazla sabit sürücüye bir Azure veri merkezine göndermek için çevrimdışı dengeli dağıtım iş akışını kullanabilirsiniz. Bu makalede, System Center DPM ve Azure Backup Sunucusu için bu iş akışını tamamlamaya ilişkin genel bakış ve diğer ayrıntılar adımları sunulmaktadır.

> [!NOTE]
> Microsoft Azure Kurtarma Hizmetleri (MARS) Aracısı için çevrimdışı yedekleme işlemi, System Center DPM ve Azure Backup Sunucusu farklıdır. MARS Aracısı ile çevrimdışı yedekleme kullanma hakkında daha fazla bilgi için [Bu makaleye](backup-azure-backup-import-export.md)bakın. Çevrimdışı yedekleme, Azure Backup Aracısı kullanılarak gerçekleştirilen sistem durumu yedeklemeleri için desteklenmez.
>

## <a name="overview"></a>Genel Bakış

Azure Backup ve Azure Içeri/dışarı aktarma özelliğinin çevrimdışı dağıtımı sayesinde, diskleri kullanarak verileri Azure 'a çevrimdışı yüklemek basittir. Çevrimdışı yedekleme işlemi aşağıdaki adımları içerir:

> [!div class="checklist"]
>
> * Ağ üzerinden gönderilmek yerine yedekleme verileri bir *hazırlama konumuna* yazılır
> * *Hazırlama konumundaki* veriler, *AzureOfflineBackupDiskPrep* yardımcı programını kullanarak BIR veya daha fazla SATA diskine yazılır
> * Bir Azure Içeri aktarma işi otomatik olarak yardımcı program tarafından oluşturulur
> * Daha sonra SATA sürücüler en yakın Azure veri merkezine gönderilir
> * Yedekleme verilerinin Azure 'a yüklenmesi tamamlandıktan sonra, Azure Backup yedekleme verileri yedekleme kasasına kopyalanır ve artımlı yedeklemeler zamanlanır.

## <a name="supported-configurations"></a>Desteklenen yapılandırmalar

Çevrimdışı yedekleme, Şirket içindeki verileri Şirket içinden Microsoft Bulut dışına yedekleyen tüm Azure Backup dağıtım modelleriyle desteklenir. Buna şunlar dahildir

> [!div class="checklist"]
>
> * Microsoft Azure Kurtarma Hizmetleri (MARS) Aracısı veya Azure Backup Aracısı ile dosya ve klasörlerin yedeklenmesi.
> * Tüm iş yüklerini ve dosyaları System Center Data Protection Manager (SC DPM) ile yedekleme
> * Tüm iş yüklerini ve dosyaları Microsoft Azure Backup sunucusuna yedekleme

## <a name="prerequisites"></a>Ön koşullar

Çevrimdışı yedekleme iş akışını başlatmadan önce aşağıdaki önkoşulların karşılandığından emin olun

* Bir [Kurtarma Hizmetleri Kasası](backup-azure-recovery-services-vault-overview.md) oluşturuldu. Bir tane oluşturmak için, [Bu makaledeki](tutorial-backup-windows-server-to-azure.md#create-a-recovery-services-vault) adımlara bakın
* Azure Backup Aracısı veya Azure Backup Sunucusu ya da SC DPM, geçerli olduğu şekilde Windows Server/Windows istemcisine yüklenmiştir ve bilgisayar kurtarma hizmetleri kasasına kaydedilir. Yalnızca [Azure Backup en son sürümünün](https://go.microsoft.com/fwlink/?linkid=229525) kullanıldığından emin olun.
* Verilerinizi yedeklemeyi planladığınız bilgisayarda [Azure yayımlama ayarları dosyasını indirin](https://portal.azure.com/#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) . Yayımlama ayarları dosyasını yüklediğiniz abonelik, kurtarma hizmetleri kasasını içeren abonelikten farklı olabilir. Aboneliğiniz sogeign Azure bulutlarında ise, Azure yayımlama ayarları dosyasını indirmek için aşağıdaki bağlantıları uygun şekilde kullanın.

    | Sovereign bulut bölgesi | Azure yayımlama ayarları dosyası bağlantısı |
    | --- | --- |
    | Birleşik Devletler | [Bağlantı](https://portal.azure.us#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) |
    | Çin | [Bağlantı](https://portal.azure.cn/#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) |

* Yayımlama ayarları dosyasını indirdiğiniz abonelikte aşağıda gösterildiği gibi, *Kaynak Yöneticisi* dağıtım modeli olan bir Azure depolama hesabı oluşturulmuştur:

  ![Kaynak Yöneticisi geliştirmeyle depolama hesabı oluşturma](./media/backup-azure-backup-import-export/storage-account-resource-manager.png)

* İlk kopyanızı tutmak için yeterli disk alanına sahip bir ağ paylaşımının veya bilgisayarda, iç veya dış olan herhangi bir ek sürücü olabilen bir hazırlama konumu oluşturulur. Örneğin, bir 500 GB dosya sunucusunu yedeklemeye çalışıyorsanız, hazırlama alanının en az 500 GB olduğundan emin olun. (Sıkıştırma nedeniyle daha küçük bir miktar kullanılır.)
* Azure 'a gönderilecek disklere yönelik olarak, yalnızca 2,5 inç SSD veya 2,5-inç veya 3,5-inç SATA II/III iç sabit sürücünün kullanıldığından emin olun. Sabit sürücüleri 10 TB 'a kadar kullanabilirsiniz. Hizmetin desteklediği en son sürücü kümesi için [Azure içeri/dışarı aktarma hizmeti belgelerini](../storage/common/storage-import-export-requirements.md#supported-hardware) denetleyin.
* SATA sürücülerin, *hazırlama KONUMUNDAN* SATA sürücülere olan yedekleme verilerinin kopyasının yapıldığı bir bilgisayara ( *kopya bilgisayar*olarak adlandırılır) bağlanması gerekir. *Kopya bilgisayarda* BitLocker 'ın etkinleştirildiğinden emin olun

## <a name="prepare-the-server-for-the-offline-backup-process"></a>Sunucuyu çevrimdışı yedekleme işlemi için hazırlama

>[!NOTE]
> MARS Aracısı yüklemenizde *AzureOfflineBackupCertGen. exe* gibi listelenen yardımcı programları bulamıyorsanız, bunlara erişim sağlamak için AskAzureBackupTeam@microsoft.com yazın.

* Sunucuda yükseltilmiş bir komut istemi açın ve aşağıdaki komutu çalıştırın:

    ```cmd
    AzureOfflineBackupCertGen.exe CreateNewApplication SubscriptionId:<Subs ID>
    ```

    Araç, yoksa, bir Azure çevrimdışı yedekleme AD uygulaması oluşturacaktır.

    Bir uygulama zaten varsa, bu yürütülebilir dosya sertifikayı Kiracıdaki uygulamaya el ile yüklemeniz istenir. Sertifikayı uygulamaya el ile yüklemek için [Bu bölümde](#manually-upload-offline-backup-certificate) aşağıdaki adımları izleyin.

* AzureOfflineBackup. exe aracı bir OfflineApplicationParams. xml dosyası oluşturacaktır.  Bu dosyayı MABS veya DPM ile sunucuya kopyalayın.
* DPM/Azure Backup (MABS) sunucusuna [en son Mars aracısını](https://aka.ms/azurebackup_agent) yükler.
* Sunucuyu Azure 'a kaydedin.
* Şu komutu çalıştırın:

    ```cmd
    AzureOfflineBackupCertGen.exe AddRegistryEntries SubscriptionId:<subscriptionid> xmlfilepath:<path of the OfflineApplicationParams.xml file>  storageaccountname:<storageaccountname configured with Azure Data Box>
    ```

* Yukarıdaki komut dosyayı oluşturur `C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch\MicrosoftBackupProvider\OfflineApplicationParams_<Storageaccountname>.xml`

## <a name="manually-upload-offline-backup-certificate"></a>Çevrimdışı yedekleme sertifikasını el ile karşıya yükle

Çevrimdışı yedekleme sertifikasını çevrimdışı yedekleme için daha önce oluşturulmuş bir Azure Active Directory uygulamasına el ile yüklemek için aşağıdaki adımları izleyin.

1. Azure Portal’da oturum açın.
2. **Azure Active Directory** > gidin **uygulama kayıtları**
3. **Sahip olunan uygulamalar** sekmesine gidin ve aşağıda gösterildiği gibi görünen ad biçimiyle `AzureOfflineBackup _<Azure User Id` bir uygulama bulun:

    ![Uygulamanın sahip olduğu uygulamalar sekmesinde bul](./media/backup-azure-backup-import-export/owned-applications.png)

4. Uygulamaya tıklayın. Sol bölmedeki **Yönet** sekmesinde **Sertifikalar & gizlilikler**' a gidin.
5. Önceden var olan sertifikaları veya ortak anahtarları denetleyin. Hiçbiri yoksa, uygulamanın **genel bakış** sayfasında **Sil** düğmesine tıklayarak uygulamayı güvenle silebilirsiniz. Bundan sonra, [sunucuyu çevrimdışı yedekleme Işlemine hazırlama](#prepare-the-server-for-the-offline-backup-process) adımlarını yeniden deneyebilir ve aşağıdaki adımları atlayabilirsiniz. Aksi takdirde, çevrimdışı yedeklemeyi yapılandırmak istediğiniz DPM/Azure Backup Sunucusu (MABS) sunucusundan aşağıdaki adımları yürütün.
6. **Bilgisayar sertifikası uygulamasını yönet** > **Kişisel** sekmesini açın ve ada sahip sertifikayı arayın `CB_AzureADCertforOfflineSeeding_<ResourceId>`
7. Yukarıdaki sertifikayı seçin, **Tüm görevler** ' e sağ tıklayın ve ardından özel anahtar olmadan. cer biçiminde **dışarı aktarın**.
8. Azure portal Azure çevrimdışı yedekleme uygulamasına gidin.
9.  > sertifikalarını **Yönet** ' e tıklayın **& gizli** dizileri > **sertifikayı karşıya yükleyin**ve önceki adımda dışarıya aktarılmış sertifikayı karşıya yükleyin.

    ![Sertifikayı karşıya yükle](./media/backup-azure-backup-import-export/upload-certificate.png)
10. Sunucusunda, Çalıştır penceresine **Regedit** yazarak kayıt defterini açın.
11. *\ HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider*kayıt defteri giriş bilgisayarına gidin.
12. **CloudBackupProvider** ' a sağ tıklayın ve ada sahip yeni bir dize değeri ekleyin `AzureADAppCertThumbprint_<Azure User Id>`

    >[!NOTE]
    > Note: Azure Kullanıcı kimliğini bulmak Için aşağıdaki adımlardan birini gerçekleştirin:
    >
    >1. Azure bağlı PowerShell 'den `Get-AzureRmADUser -UserPrincipalName “Account Holder’s email as appears in the portal”` komutunu çalıştırın.
    >2. Kayıt defteri yoluna gidin: `Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\DbgSettings\OnlineBackup; Name: CurrentUserId;`

13. Önceki adımda eklenen dizeye sağ tıklayın ve **Değiştir**' i seçin. Değerde, 7. adımda verdiğiniz sertifikanın parmak izini girip **Tamam**' a tıklayın.
14. Parmak izi değerini almak için, sertifikaya çift tıklayın, ardından **Ayrıntılar** sekmesini seçin ve parmak izi alanını görene kadar aşağı kaydırın. **Parmak izi** ' ne tıklayın ve değeri kopyalayın.

    ![Parmak izi alanından değeri Kopyala](./media/backup-azure-backup-import-export/thumbprint-field.png)

15. Çevrimdışı yedekleme işlemine devam etmek için [Iş akışı](#workflow) bölümüne devam edin.

## <a name="workflow"></a>İş Akışı

Bu bölümdeki bilgiler, verilerinizin bir Azure veri merkezine teslim edilebilmesi ve Azure depolama 'ya yüklenebilmesi için çevrimdışı yedekleme iş akışını tamamlamanıza yardımcı olur. Içeri aktarma hizmeti veya işlemin herhangi bir yönü hakkında sorularınız varsa, daha önce başvurulan [hizmete genel bakış](../storage/common/storage-import-export-service.md) belgelerine bakın.

### <a name="initiate-offline-backup"></a>Çevrimdışı Yedeklemeyi Başlat

1. Bir yedekleme zamanladığınızda, aşağıdaki ekranı görürsünüz (Windows Server, Windows istemcisi veya System Center Data Protection Manager).

    ![İçeri aktarma ekranı](./media/backup-azure-backup-import-export/offlineBackupscreenInputs.png)

    System Center Data Protection Manager ilgili ekran aşağıda verilmiştir: <br/>
    ![SC DPM ve Azure Backup sunucusu içeri aktarma ekranı](./media/backup-azure-backup-import-export/dpmoffline.png)

    Girişlerin açıklaması aşağıdaki gibidir:

   * **Hazırlama konumu**: ilk yedekleme kopyasının yazıldığı geçici depolama konumu. Hazırlama konumu bir ağ paylaşımında veya yerel bir bilgisayarda olabilir. Kopya bilgisayar ve kaynak bilgisayar farklıysa, hazırlama konumunun tam ağ yolunu belirtmenizi öneririz.
   * **Azure Içeri aktarma Işi adı**: Azure içeri aktarma hizmeti 'nin ve Azure Backup disklere gönderilen verilerin Azure 'a aktarılmasını izleyen benzersiz ad.
   * **Azure yayımlama ayarları**: yayımlama ayarları dosyasına yerel yol belirtin.
   * **Azure ABONELIK kimliği**: Azure yayımlama ayarları dosyasını Indirdiğiniz aboneliğin Azure abonelik kimliği.
   * Azure **depolama hesabı**: Azure yayımlama ayarları dosyasıyla ilişkili Azure aboneliğindeki depolama hesabının adı.
   * **Azure depolama kapsayıcısı**: yedekleme verilerinin Içeri aktarıldığı Azure depolama hesabındaki hedef depolama blobunun adı.

     Diskleri hazırlamak için gereken *hazırlama konumunu* ve *Azure Içeri aktarma işi adını* kaydedin.  

2. İş akışını tamamlar ve çevrimdışı yedekleme kopyasını başlatmak için Azure Backup aracı yönetim konsolundan **Şimdi Yedekle** ' ye tıklayın. İlk yedekleme, bu adımın bir parçası olarak hazırlama alanına yazılır.

    ![Şimdi Yedekle](./media/backup-azure-backup-import-export/backupnow.png)

    System Center Data Protection Manager veya Azure Backup sunucusu 'nda karşılık gelen iş akışını gerçekleştirmek için, **koruma grubuna**sağ tıklayın ve ardından **Kurtarma noktası oluştur** seçeneğini belirleyin. Sonra **çevrimiçi koruma** seçeneğini belirleyin.

    ![SC DPM ve Azure Backup Server Şimdi Yedekle](./media/backup-azure-backup-import-export/dpmbackupnow.png)

    İşlem bittikten sonra, hazırlama konumu, disk hazırlığı için kullanılabilir hale gelir.

    ![Yedekleme ilerleme durumu](./media/backup-azure-backup-import-export/opbackupnow.png)

### <a name="prepare-sata-drives-and-ship-to-azure"></a>SATA sürücüleri hazırlayın ve Azure 'a gönderin

En yakın Azure veri merkezine gönderilen SATA sürücüleri hazırlamak için *AzureOfflineBackupDiskPrep* yardımcı programı kullanılır. Bu yardımcı program, kurtarma hizmetleri aracısının yükleme dizininde aşağıdaki yolda bulunur:

`*\\Microsoft Azure Recovery Services Agent\Utils\*`

1. Dizinine gidin ve **AzureOfflineBackupDiskPrep** dizinini, HAZıRLANMAKTA olan SATA sürücülerin bağlı olduğu bir kopya bilgisayara kopyalayın. Aşağıdaki gibi, kopyalama bilgisayarına dikkat edin:

   * Kopya bilgisayar **, çevrimdışı dağıtım için hazırlama iş akışında** sağlanan aynı ağ yolunu kullanarak çevrimdışı dengeli dağıtım iş akışının hazırlama konumuna erişebilir.
   * BitLocker, kopyalama bilgisayarında etkinleştirilmiştir.
   * Kopya bilgisayar Azure portal erişebilir.

     Gerekirse, kopya bilgisayar kaynak bilgisayarla aynı olabilir.

     > [!IMPORTANT]
     > Kaynak bilgisayar bir sanal makinedir, kopya bilgisayar olarak farklı bir fiziksel sunucu veya istemci makine kullanılması zorunludur.

2. Geçerli dizin olarak *AzureOfflineBackupDiskPrep* yardımcı program dizini ile kopyalama bilgisayarında yükseltilmiş bir komut istemi açın ve aşağıdaki komutu çalıştırın:

    `*.\AzureOfflineBackupDiskPrep.exe*   s:<*Staging Location Path*>   [p:<*Path to AzurePublishSettingsFile*>]`

    | Parametre | Açıklama |
    | --- | --- |
    | s:&lt;*hazırlama konumu yolu*&gt; |**Çevrimdışı yedekleme 'Yi Başlat** iş akışına girdiğiniz hazırlama konumunun yolunu sağlamak için kullanılan zorunlu giriş. |
    | p: *PublishSettingsFile dosyasının yolunu*&lt;&gt; |**Çevrimdışı yedekleme 'Yi Başlat** iş akışında girdiğiniz **Azure yayımlama ayarları** dosyasının yolunu sağlamak için kullanılan isteğe bağlı giriş. |

    > [!NOTE]
    > Bilgisayar ve kaynak bilgisayar farklı olduğunda Azuyeniden yayımcı Settingfile&gt; değerinin &lt;yolu zorunludur.
    >
    >

    Komutunu çalıştırdığınızda, yardımcı program, hazırlanması gereken sürücülere karşılık gelen Azure Içeri aktarma işinin seçimini ister. Yalnızca tek bir içeri aktarma işi belirtilen hazırlama konumuyla ilişkiliyse, aşağıdakine benzer bir ekran görürsünüz.

    ![Azure disk hazırlama aracı girişi](./media/backup-azure-backup-import-export/azureDiskPreparationToolDriveInput.png) <br/>

3. Azure 'a aktarmaya hazırlamak istediğiniz bağlı disk için sondaki iki nokta olmadan sürücü harfini girin. İstendiğinde sürücünün biçimlendirmesi için onay sağlayın.

    Araç daha sonra diski hazırlamaya ve yedekleme verilerini kopyalamaya başlar. Belirtilen diskte yedekleme verileri için yeterli alan olmadığında araç tarafından istendiğinde ek diskler eklemeniz gerekebilir. <br/>

    Aracın başarıyla yürütülmesi sırasında, verdiğiniz bir veya daha fazla disk Azure 'a gönderim için hazırlandı. Ayrıca, **çevrimdışı Yedeklemeyi Başlat** iş akışı sırasında verdiğiniz ada sahip bir içeri aktarma işi Azure 'da oluşturulur. Son olarak, araç, disklerin sevk edilmesi gereken Azure veri merkezine sevkiyat adresini görüntüler.

    ![Azure disk hazırlığı Tamam](./media/backup-azure-backup-import-export/azureDiskPreparationToolSuccess.png)<br/>

4. Komut yürütmesinin sonunda, sevkiyat bilgilerini aşağıda gösterildiği gibi güncelleştirme seçeneğini de görebilirsiniz:

    ![Gönderi bilgilerini Güncelleştir seçeneği](./media/backup-azure-backup-import-export/updateshippingutility.png)<br/>

5. Ayrıntıları hemen girebilirsiniz. Araç, bir dizi girişi içeren süreç boyunca size rehberlik eder. Ancak, izleme numarası veya gönderme ile ilgili diğer ayrıntılar gibi bilgileriniz yoksa oturumu sonlandırabilir. Daha sonra bu makalede sevkiyat ayrıntılarını güncelleştirme adımları verilmiştir.

6. Diskleri aracın verdiği adrese gönderin ve daha sonra başvurmak üzere izleme numarasını saklayın.

   > [!IMPORTANT]
   > İki Azure Içeri aktarma Işi aynı izleme numarasına sahip olamaz. Tek bir Azure Içeri aktarma Işi altında yardımcı program tarafından hazırlanan sürücülerin tek bir pakette birlikte ve paket için tek bir benzersiz izleme numarası bulunduğundan emin olun. **Farklı** Azure Içeri aktarma işlerinin bir parçası olarak hazırlanan sürücüleri tek bir pakette birleştirmeyin.

7. İzleme numarası bilgisine sahip olduğunuzda, kaynak bilgisayara gidin ve Içeri aktarma Işleminin tamamlanması bekleniyor ve geçerli dizin olarak *AzureOfflineBackupDiskPrep* yardımcı program dizini ile yükseltilmiş bir komut isteminde aşağıdaki komutu çalıştırın:

   `*.\AzureOfflineBackupDiskPrep.exe*  u:`

   İsteğe bağlı olarak, *kopya bilgisayar*gibi farklı bir bilgisayardan, geçerli dizin olarak *AzureOfflineBackupDiskPrep* yardımcı program dizini ile aşağıdaki komutu çalıştırabilirsiniz:

   `*.\AzureOfflineBackupDiskPrep.exe*  u:  s:<*Staging Location Path*>   p:<*Path to AzurePublishSettingsFile*>`

    | Parametre | Açıklama |
    | --- | --- |
    | larınız | Azure Içeri aktarma işinin sevkiyat ayrıntılarını güncelleştirmek için kullanılan zorunlu giriş |
    | s:&lt;*hazırlama konumu yolu*&gt; | Kaynak bilgisayarda komut çalıştırılmayan zorunlu giriş. **Çevrimdışı yedekleme Başlat** iş akışına girdiğiniz hazırlama konumunun yolunu sağlamak için kullanılır. |
    | p: *PublishSettingsFile dosyasının yolunu*&lt;&gt; | Kaynak bilgisayarda komut çalıştırılmayan zorunlu giriş. **Çevrimdışı yedekleme Başlat** iş akışında girdiğiniz **Azure yayımlama ayarları** dosyasının yolunu sağlamak için kullanılır. |

    Yardımcı program, kaynak bilgisayarın beklediği Içeri aktarma işini veya komut farklı bir bilgisayarda çalıştırıldığında hazırlama konumuyla ilişkili içeri aktarma işlerini otomatik olarak algılar. Daha sonra, aşağıda gösterildiği gibi bir dizi giriş aracılığıyla sevkiyat bilgilerini güncelleştirme seçeneği sağlar:

    ![Sevkiyat bilgilerini girme](./media/backup-azure-backup-import-export/shippinginputs.png)<br/>

8. Tüm girişler sağlandığında, ayrıntıları dikkatlice gözden geçirin ve verdiğiniz teslim bilgilerini *Evet*yazarak işleyin.

    ![Sevkiyat bilgilerini gözden geçirme](./media/backup-azure-backup-import-export/reviewshippinginformation.png)<br/>

9. Sevkiyat bilgilerini başarıyla güncelleştirirken yardımcı program, sizin tarafınızdan girilen sevkıyat ayrıntılarının aşağıda gösterildiği gibi depolandığı yerel bir konum sağlar

    ![Sevkiyat bilgilerini depolama](./media/backup-azure-backup-import-export/storingshippinginformation.png)<br/>

   > [!IMPORTANT]
   > *AzureOfflineBackupDiskPrep* yardımcı programını kullanarak, sürücülerin sevkiyat bilgilerini sağlamanın iki haftası Içinde Azure veri merkezine ulaşmasını sağlayın. Bunun yapılmaması, sürücülerin işlenmesine neden olabilir.  

Yukarıdaki adımları tamamladıktan sonra, Azure veri merkezi sürücüleri almaya ve yedek verileri sürücülerden oluşturulan klasik Azure depolama hesabına aktarmaya yönelik olarak daha fazla işlem yapmaya hazırlanmalıdır.

### <a name="time-to-process-the-drives"></a>Sürücüleri işleme süresi

Bir Azure içeri aktarma işinin işlenmesi için gereken süre, Sevkiyat zamanı, iş türü, kopyalandığı verilerin türü ve boyutu ile verilen disklerin boyutu gibi farklı faktörlere bağlı olarak farklılık gösterir. Azure Içeri/dışarı aktarma hizmeti bir SLA 'ya sahip değil, ancak diskler alındıktan sonra hizmet, Azure depolama hesabınıza 7 ila 10 gün sonra yedek veri kopyalamayı tamamlayacak kadar çaba harcar. Sonraki bölümde, Azure içeri aktarma işinin durumunu nasıl izleyebilmeniz ayrıntılı bir şekilde anlatılmaktadır.

### <a name="monitoring-azure-import-job-status"></a>Azure Içeri aktarma Işi durumunu izleme

Sürücüleriniz aktarım sırasında veya Azure veri merkezinde depolama hesabına kopyalanmak olsa da, Azure Backup Aracısı veya SC DPM veya kaynak bilgisayardaki Azure Backup sunucu konsolu, zamanlanan yedeklemeleriniz için aşağıdaki iş durumunu gösterir.

  `Waiting for Azure Import Job to complete. Please check on Azure Management portal for more information on job status`

Içeri aktarma Işinin durumunu denetlemek için aşağıdaki adımları izleyin.

1. Kaynak bilgisayarda yükseltilmiş bir komut istemi açın ve aşağıdaki komutu çalıştırın:

     `AzureOfflineBackupDiskPrep.exe u:`

2. Çıktı, aşağıda gösterildiği gibi Içeri aktarma Işinin geçerli durumunu gösterir:

    ![Içeri aktarma Işi durumu denetleniyor](./media/backup-azure-backup-import-export/importjobstatusreporting.png)<br/>

Azure içeri aktarma işinin çeşitli durumları hakkında daha fazla bilgi için [Bu makaleye](../storage/common/storage-import-export-view-drive-status.md) bakın

### <a name="complete-the-workflow"></a>İş akışını doldurun

İçeri aktarma işi bittikten sonra, depolama hesabınızda ilk yedekleme verileri kullanılabilir. Sonraki zamanlanmış yedekleme sırasında Azure Backup, verilerin içeriğini depolama hesabından kurtarma hizmetleri kasasına aşağıda gösterildiği gibi kopyalar:

   ![Verileri kurtarma hizmetleri kasasına kopyalama](./media/backup-azure-backup-import-export/copyingfromstorageaccounttoazurebackup.png)<br/>

Sonraki zamanlanmış yedekleme sırasında, Azure Backup ilk yedekleme kopyası üzerinde artımlı yedekleme gerçekleştirir.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Içeri/dışarı aktarma iş akışı hakkında herhangi bir soru için, [BLOB depolamaya veri aktarmak için Microsoft Azure içeri/dışarı aktarma hizmetini kullanma](../storage/common/storage-import-export-service.md)konusuna bakın.
