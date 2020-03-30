---
title: DPM ve Azure Backup Sunucusu için çevrimdışı yedekleme
description: Azure Yedekleme ile, Azure İçe Alma/Dışa Aktarma hizmetini kullanarak verileri ağdan gönderebilirsiniz. Bu makalede, DPM ve Azure Yedekleme Sunucusu için çevrimdışı yedekleme iş akışı açıklanmaktadır.
ms.reviewer: saurse
ms.topic: conceptual
ms.date: 1/28/2020
ms.openlocfilehash: 080b0bc53b2058bd186e90f354b8f5bcda510414
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78197105"
---
# <a name="offline-backup-workflow-for-dpm-and-azure-backup-server"></a>DPM ve Azure Yedekleme Sunucusu için çevrimdışı yedekleme iş akışı

Azure Yedekleme, Azure'a verinin ilk tam yedeklemeleri sırasında ağ ve depolama maliyetlerinden tasarruf sağlayan birkaç yerleşik verimlilikzorundadır. İlk tam yedeklemeler genellikle büyük miktarda veri aktarımı sağlar ve yalnızca deltaları/artımlı aygıtları aktaran sonraki yedeklemelerle karşılaştırıldığında daha fazla ağ bant genişliği gerektirir. Azure Yedekleme ilk yedeklemeleri sıkıştırır. Azure Yedekleme, çevrimdışı tohumlama işlemi sayesinde sıkıştırılmış ilk yedekleme verilerini Azure'a çevrimdışı yüklemek için diskler kullanabilir.

Azure Yedekleme'nin çevrimdışı tohumlama [işlemi, Azure İçe Alma/Dışa Aktarma hizmetiyle](../storage/common/storage-import-export-service.md)sıkı bir şekilde tümleştirilir. Bu hizmeti diskleri kullanarak verileri Azure'a aktarmak için kullanabilirsiniz. Yüksek gecikmeli ve düşük bant genişliğine sahip bir ağ üzerinden aktarılması gereken ilk yedekleme verilerinin terabaytları (TB'leri) varsa, bir veya daha fazla sabit sürücüdeki ilk yedekleme kopyasını bir Azure veri merkezine nakletmek için çevrimdışı tohumlama iş akışını kullanabilirsiniz. Bu makalede, Sistem Merkezi Veri Koruma Yöneticisi (DPM) ve Microsoft Azure Yedekleme Sunucusu (MABS) için bu iş akışını tamamlayan genel bir bakış ve diğer adımlar sağlanmaktadır.

> [!NOTE]
> Microsoft Azure Kurtarma Hizmetleri (MARS) Aracısı için çevrimdışı yedekleme işlemi DPM ve MABS'den farklıdır. MARS Aracısı ile çevrimdışı yedekleme kullanma hakkında daha fazla bilgi için [Azure Yedekleme'de Çevrimdışı yedekleme iş akışına](backup-azure-backup-import-export.md)bakın. Çevrimdışı yedekleme, Azure Yedekleme Aracısı kullanılarak yapılan sistem durumu yedeklemeleri için desteklenmez.
>

## <a name="overview"></a>Genel Bakış

Azure Yedekleme'nin çevrimdışı tohumlama özelliği ve Azure İçe Alma/Dışa Aktarma hizmeti sayesinde, diskleri kullanarak verileri Azure'a çevrimdışı yüklemek kolaydır. Çevrimdışı yedekleme işlemi aşağıdaki adımları içerir:

> [!div class="checklist"]
>
> * Yedekleme verileri ağ üzerinden gönderilmek yerine bir hazırlama konumuna yazılır.
> * Hazırlama yerindeki veriler, *AzureOfflineBackupDiskPrep* yardımcı programı kullanılarak bir veya daha fazla SATA diskine yazılır.
> * Azure alma işi yardımcı program tarafından otomatik olarak oluşturulur.
> * SATA sürücüleri daha sonra en yakın Azure veri merkezine gönderilir.
> * Yedekleme verilerinin Azure'a yüklenmesi tamamlandıktan sonra, Azure Yedekleme yedekleme verilerini yedekleme kasasına kopyalar ve artımlı yedeklemeler zamanlanır.

## <a name="supported-configurations"></a>Desteklenen yapılandırmalar

Şirket içi microsoft bulutuna verileri yedekleyen Azure Yedekleme'nin tüm dağıtım modelleri için çevrimdışı yedekleme desteklenir. Bu modeller şunlardır:

> [!div class="checklist"]
>
> * Mars Aracısı veya Azure Yedekleme Aracısı ile dosya ve klasörlerin yedeklemi.
> * DPM ile tüm iş yüklerinin ve dosyaların yedekli.
> * MABS ile tüm iş yüklerinin ve dosyaların yedeklemi.

## <a name="prerequisites"></a>Ön koşullar

Çevrimdışı yedekleme iş akışını başlatmadan önce aşağıdaki ön koşulların karşılandığından emin olun:

* [Kurtarma Hizmetleri kasası](backup-azure-recovery-services-vault-overview.md) oluşturuldu. Bir tane oluşturmak için Kurtarma [Hizmetleri Oluştur kasasındaki](tutorial-backup-windows-server-to-azure.md#create-a-recovery-services-vault) adımları izleyin
* Azure Yedekleme Aracısı veya MABS veya DPM, uygulanabilir olduğu şekilde Windows Server'a veya bir Windows istemcisine yüklendi ve bilgisayar Kurtarma Hizmetleri kasasına kayıtlı. [Azure Yedekleme'nin](https://go.microsoft.com/fwlink/?linkid=229525) yalnızca en son sürümünün kullanıldığından emin olun.
* Verilerinizi yedeklemeyi planladığınız bilgisayara [Azure yayımlama ayarları dosyasını indirin.](https://portal.azure.com/#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) Yayımlama ayarları dosyasını indirdiğiniz abonelik, Kurtarma Hizmetleri kasasını içeren abonelikten farklı olabilir. Aboneliğiniz egemen Azure bulutları üzerindeyse, Azure yayımlama ayarları dosyasını indirmek için aşağıdaki bağlantıları kullanın.

    | Egemen bulut bölgesi | Azure yayımlama ayarları dosya bağlantısı |
    | --- | --- |
    | Amerika Birleşik Devletleri | [Bağlantı](https://portal.azure.us#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) |
    | Çin | [Bağlantı](https://portal.azure.cn/#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) |

* Yayımlama ayarları dosyasını indirdiğiniz abonelikte Kaynak Yöneticisi dağıtım modeline sahip bir Azure depolama hesabı oluşturuldu.

  ![Kaynak Yöneticisi geliştirme ile depolama hesabı oluşturma](./media/backup-azure-backup-import-export/storage-account-resource-manager.png)

* İlk kopyanızı tutmak için yeterli disk alanı olan, bilgisayarda bir ağ paylaşımı veya herhangi bir ek sürücü olabilecek bir evreleme konumu oluşturulur. Örneğin, 500 GB'lık bir dosya sunucusunu yedeklemek istiyorsanız, hazırlama alanının en az 500 GB olduğundan emin olun. (Sıkıştırma nedeniyle daha küçük bir miktar kullanılır.)
* Azure'a gönderilen diskler için yalnızca 2,5 inç SSD veya 2,5 inç veya 3,5 inç SATA II/III dahili sabit disklerin kullanıldığından emin olun. 10 TB'a kadar sabit diskler kullanabilirsiniz. Hizmetin desteklediği en son sürücü kümesi için [Azure İçE Alma/Dışa Aktarma hizmeti belgelerini](../storage/common/storage-import-export-requirements.md#supported-hardware) denetleyin.
* SATA sürücüleri, evreleme konumundan SATA sürücülerine yedekleme verilerinin kopyalanmasının yapıldığı bilgisayara *(kopya bilgisayar*olarak anılacaktır) bağlanmalıdır. BitLocker'ın kopya bilgisayarda etkin olduğundan emin olun.

## <a name="prepare-the-server-for-the-offline-backup-process"></a>Sunucuyu çevrimdışı yedekleme işlemi için hazırlama

>[!NOTE]
> *AzureOfflineBackupCertGen.exe*gibi listelenen yardımcı programları MARS Aracısı yüklemenizde bulamıyorsanız, bunlara AskAzureBackupTeam@microsoft.com erişmek için yazın.

* Sunucuda yükseltilmiş bir komut istemi açın ve aşağıdaki komutu çalıştırın:

    ```cmd
    AzureOfflineBackupCertGen.exe CreateNewApplication SubscriptionId:<Subs ID>
    ```

    Araç, yoksa Bir Azure Çevrimdışı Yedekleme Etkin Dizin Uygulaması oluşturur.

    Bir uygulama zaten varsa, bu yürütülebilir kiracı uygulamaya sertifikayı el ile yüklemenizi ister. Sertifikayı uygulamaya el ile yüklemek için [bu bölümdeki](#manually-upload-an-offline-backup-certificate) adımları izleyin.

* *AzureOfflineBackup.exe* aracı bir *ÇevrimdışıApplicationParams.xml* dosyası oluşturur. Bu dosyayı MABS veya DPM ile sunucuya kopyalayın.
* DPM örneğine veya Azure Yedekleme sunucusuna [en son MARS Aracısını](https://aka.ms/azurebackup_agent) yükleyin.
* Sunucuyu Azure'a kaydedin.
* Şu komutu çalıştırın:

    ```cmd
    AzureOfflineBackupCertGen.exe AddRegistryEntries SubscriptionId:<subscriptionid> xmlfilepath:<path of the OfflineApplicationParams.xml file>  storageaccountname:<storageaccountname configured with Azure Data Box>
    ```

* Önceki komut dosyayı `C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch\MicrosoftBackupProvider\OfflineApplicationParams_<Storageaccountname>.xml`oluşturur.

## <a name="manually-upload-an-offline-backup-certificate"></a>Çevrimdışı yedekleme sertifikasını el ile yükleme

Çevrimdışı yedekleme sertifikasını çevrimdışı yedekleme için önceden oluşturulmuş bir Azure Etkin Dizin uygulamasına el ile yüklemek için aşağıdaki adımları izleyin.

1. Azure Portal’da oturum açın.
1. Azure **Active Directory** > **App kayıtlarına**gidin.
1. Sahip **olunan uygulamalar** sekmesinde, görüntü adı `AzureOfflineBackup _<Azure User Id`biçimine sahip bir uygulama bulun.

    ![Sahip olunan uygulamalar sekmesinde uygulamayı bulma](./media/backup-azure-backup-import-export/owned-applications.png)

1. Uygulamayı seçin. Sol bölmede **Yönet** altında, **sertifikalar & sırları**gidin.
1. Önceden varolan sertifikaları veya ortak anahtarları denetleyin. Yoksa, uygulamanın **Genel Bakış** **sayfasındasil** düğmesini seçerek uygulamayı güvenle silebilirsiniz. Ardından, [sunucuyu çevrimdışı yedekleme](#prepare-the-server-for-the-offline-backup-process) işlemi için hazırlamak için adımları yeniden deneyebilir ve aşağıdaki adımları atlayabilirsiniz. Aksi takdirde, çevrimdışı yedekleme yapılandırmak istediğiniz DPM örneğinden veya Azure Yedekleme sunucusundan bu adımları izlemeye devam edin.
1. Bilgisayar **sertifikası uygulamasını yönet'i** > seçin**Kişisel** sekmesi. `CB_AzureADCertforOfflineSeeding_<ResourceId>`
1. Sertifikayı seçin, **Tüm Görevler'i**sağ tıklatın ve sonra .cer biçiminde özel bir anahtar olmadan **Dışa Aktar'ı**seçin.
1. Azure portalındaki Azure çevrimdışı yedekleme uygulamasına gidin.
1. Sertifikaları **yönet** > **& sırları** > **Yükleme sertifikasını**seçin. Önceki adımda dışa aktarılan sertifikayı yükleyin.

    ![Sertifikayı güncelleştirin](./media/backup-azure-backup-import-export/upload-certificate.png)

1. Sunucuda, çalışma penceresine **regedit** girerek kayıt defterini açın.
1. Kayıt defteri girişi *Bilgisayar\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Yedekleme\Config\CloudBackupProvider*gidin.
1. **CloudBackupProvider'ı**sağ tıklatın ve adı `AzureADAppCertThumbprint_<Azure User Id>`ile yeni bir dize değeri ekleyin.

    >[!NOTE]
    > Azure kullanıcı kimliğini bulmak için aşağıdaki adımlardan birini yapın:
    >
    >* Azure'a bağlı PowerShell'den `Get-AzureRmADUser -UserPrincipalName “Account Holder’s email as appears in the portal”` komutu çalıştırın.
    >* Kayıt defteri yoluna `Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\DbgSettings\OnlineBackup; Name: CurrentUserId;`git.

1. Önceki adımda eklenen dize sağ tıklatın ve **Değiştir'i**seçin. Değer olarak, adım 7'de dışa aktarılan sertifikanın parmak izini sağlayın. Sonra **Tamam**’ı seçin.
1. Parmak izinin değerini almak için sertifikayı çift tıklatın. **Ayrıntılar** sekmesini seçin ve parmak izi alanını görene kadar aşağı kaydırın. **Thumbprint'i**seçin ve değeri kopyalayın.

    ![Parmak izi alanından değeri kopyalama](./media/backup-azure-backup-import-export/thumbprint-field.png)

1. Çevrimdışı yedekleme işlemine devam etmek için [İş Akışı](#workflow) bölümüne devam edin.

## <a name="workflow"></a>İş akışı

Bu bölümdeki bilgiler, verilerinizin bir Azure veri merkezine teslim edilebilmeleri ve Azure Depolama'ya yüklenebilmeleri için çevrimdışı yedekleme iş akışını tamamlamanıza yardımcı olur. Alma hizmeti veya işlemin herhangi bir yönü hakkında sorularınız varsa, daha önce başvurulan [Alma hizmeti genel bakış belgelerine](../storage/common/storage-import-export-service.md) bakın.

### <a name="initiate-offline-backup"></a>Çevrimdışı yedekleme başlatma

1. Bir yedekleme zamanlarken, aşağıdaki sayfayı Windows Server,Bir Windows istemcisi veya DPM'de görürsünüz.

    ![Sayfayı içe aktarma](./media/backup-azure-backup-import-export/offlineBackupscreenInputs.png)

    İşte DPM'deki ilgili sayfa. <br/>
    
    ![DPM ve Azure Yedekleme Sunucusu alma sayfası](./media/backup-azure-backup-import-export/dpmoffline.png)

    Doldurduğunuz kutular şunlardır:

   * **Evreleme Yeri**: İlk yedek kopyanın yazıldığı geçici depolama konumu. Hazırlama konumu bir ağ paylaşımında veya yerel bir bilgisayarda olabilir. Kopya bilgisayar ve kaynak bilgisayar farklıysa, hazırlama konumunun tam ağ yolunu belirtin.
   * **Azure Yayımlama Ayarları**: Yayımlama ayarları dosyasına giden yerel yol.
   * **Azure İçe Alma İş Adı**: Azure İçe Aktarma/Verme hizmeti ve Azure Yedekleme'nin disklere gönderilen verilerin Azure'a aktarılmasını izlediği benzersiz addır.
   * **Azure Abonelik Kimliği**: Azure yayımlama ayarları dosyasını indirdiğiniz abonelik için Azure abonelik kimliği.
   * **Azure Depolama Hesabı**: Azure yayımayarları dosyasıyla ilişkili Azure aboneliğindeki depolama hesabının adı.
   * **Azure Depolama Kapsayıcısı**: Yedekleme verilerinin aktarıldığı Azure depolama hesabındaki hedef depolama blobunun adı.

   Hazırlama **Konumunu** ve sağladığınız **Azure İçe Alma İş Adı** bilgilerini kaydedin. Diskleri hazırlamak için gereklidir.

1. İş akışını tamamla. Çevrimdışı yedekleme kopyasını başlatmak için Azure Yedekleme Aracısı yönetim konsolunda **Şimdi Yedekle'yi** seçin. İlk yedekleme bu adımın bir parçası olarak evreleme alanına yazılır.

    ![Şimdi yedekleme](./media/backup-azure-backup-import-export/backupnow.png)

    DPM veya Azure Yedekleme Sunucusu'nda karşılık gelen iş akışını tamamlamak için **Koruma Grubu'na**sağ tıklayın. Kurtarma **noktası oluştur** seçeneğini seçin. Ardından **Çevrimiçi Koruma** seçeneğini seçin.

    ![DPM ve MABS Şimdi Yedekleme](./media/backup-azure-backup-import-export/dpmbackupnow.png)

    İşlem bittikten sonra, evreleme konumu disk hazırlama için kullanılmaya hazırdır.

    ![Yedekleme ilerleme](./media/backup-azure-backup-import-export/opbackupnow.png)

### <a name="prepare-sata-drives-and-ship-to-azure"></a>SATA sürücülerini hazırlayın ve Azure'a yönlendirin

*AzureOfflineBackupDiskPrep* yardımcı programı, en yakın Azure veri merkezine gönderilen SATA sürücülerini hazırlamak için kullanılır. Bu yardımcı program, Kurtarma Hizmetleri Aracısı'nın yükleme dizininde aşağıdaki şekilde kullanılabilir:

`*\\Microsoft Azure Recovery Services Agent\Utils\*`

1. Dizine gidin ve *AzureOfflineBackupDiskPrep* dizinini SATA sürücülerinin bağlı olduğu bir kopya bilgisayara kopyalayın. Sağlamak:

   * Kopya bilgisayarı, "Çevrimdışı yedekleme başlat" bölümünde iş akışında sağlanan aynı ağ yolunu kullanarak çevrimdışı tohumlama iş akışı için hazırlama konumuna erişebilir.
   * BitLocker kopya bilgisayarda etkindir.
   * Kopya bilgisayar Azure portalına erişebilir. Gerekirse, kopyalama bilgisayarı kaynak bilgisayarla aynı olabilir.

     > [!IMPORTANT]
     > Kaynak bilgisayar sanal bir makineyse, kopya bilgisayar olarak farklı bir fiziksel sunucu veya istemci makinesi kullanmak zorunludur.

1. Geçerli dizini olarak *AzureOfflineBackupDiskPrep* yardımcı dizini ile kopya bilgisayarda yükseltilmiş bir komut istemi açın. Şu komutu çalıştırın:

    `*.\AzureOfflineBackupDiskPrep.exe*   s:<*Staging Location Path*>   [p:<*Path to AzurePublishSettingsFile*>]`

    | Parametre | Açıklama |
    | --- | --- |
    | s:&lt;*Evreleme Konum Yolu*&gt; |Bu zorunlu giriş, "Çevrimdışı yedekleme başlat" bölümünde iş akışına girdiğiniz hazırlama konumuna giden yolu sağlamak için kullanılır. |
    | p:&lt;*PublishSettingsFile Yolu*&gt; |Bu isteğe bağlı giriş, "Çevrimdışı yedekleme başlat" bölümünde iş akışına girdiğiniz Azure yayımlama ayarları dosyasına giden yolu sağlamak için kullanılır. |

    > [!NOTE]
    > &lt;Kopyalama bilgisayarı ve kaynak&gt; bilgisayar farklı olduğunda AzurePublishSettingFile değeri zorunludur.
    >
    >

    Komutu çalıştırdığınızda, yardımcı program hazırlanması gereken sürücülere karşılık gelen Azure alma işinin seçimini ister. Yalnızca tek bir alma işi sağlanan evreleme konumuyla ilişkiliyse, bunun gibi bir sayfa görürsünüz.

    ![Azure disk hazırlama aracı girişi](./media/backup-azure-backup-import-export/azureDiskPreparationToolDriveInput.png) <br/>

1. Azure'a aktarmak için hazırlamak istediğiniz monte edilmiş disk için sondaki üst üste olmadan sürücü mektubunu girin. İstendiğinde, sürücünün biçimlendirmesi için onay sağlayın.

    Araç daha sonra diski hazırlamaya ve yedekleme verilerini kopyalamaya başlar. Sağlanan diskin yedekleme verileri için yeterli alana sahip olmaması durumunda araç tarafından istendiğinde ek diskeklemeniz gerekebilir. <br/>

    Araç başarıyla bittikten sonra, sağladığınız bir veya daha fazla disk Azure'a gönderim için hazırlanır. Azure'da "Çevrimdışı yedekleme başlat" bölümünde iş akışı sırasında sağladığınız adı içeren bir alma işi de oluşturulur. Son olarak, araç gönderi adresini disklerin gönderilmesi gereken Azure veri merkezine görüntüler.

    ![Azure disk hazırlama tamamlandı](./media/backup-azure-backup-import-export/azureDiskPreparationToolSuccess.png)<br/>

1. Komut yürütmesonunda, gönderi bilgilerini güncelleştirme seçeneğini de görürsünüz.

    ![Gönderi bilgilerini güncelleştirme seçeneği](./media/backup-azure-backup-import-export/updateshippingutility.png)<br/>

1. Detayları hemen girebilirsiniz. Araç, bir dizi giriş içeren işlem boyunca size yol gösteriyor. Takip numarası veya sevkiyatla ilgili diğer ayrıntılar gibi bilgileriniz yoksa, oturumu sonlandırmayabilirsiniz. Sevkiyat ayrıntılarını daha sonra güncelleştirmek için adımlar bu makalede sağlanmıştır.

1. Diskleri aracın sağladığı adrese gönderin. Gelecekteki başvuru için izleme numarasını saklayın.

   > [!IMPORTANT]
   > İki Azure içe aktarma işi aynı izleme numarasına sahip olamaz. Yardımcı program tarafından tek bir Azure alma işi altında hazırlanan sürücülerin tek bir pakette birlikte gönderildiğinden ve paket için tek bir benzersiz izleme numarası olduğundan emin olun. Farklı Azure alma işlerinin bir parçası olarak hazırlanan sürücüleri tek bir pakette birleştirmeyin.

1. İzleme numarası bilgilerine sahip olduğunuzda, alma işinin tamamlanmasını bekleyen kaynak bilgisayara gidin. Geçerli dizini olarak *AzureOfflineBackupDiskPrep* yardımcı dizini ile yükseltilmiş bir komut isteminde aşağıdaki komutu çalıştırın.

   `*.\AzureOfflineBackupDiskPrep.exe*  u:`

   Aşağıdaki komutu isteğe bağlı olarak, kopyalama bilgisayarı gibi farklı bir bilgisayardan, geçerli dizini olarak *AzureOfflineBackupDiskPrep* yardımcı dizini yle çalıştırabilirsiniz.

   `*.\AzureOfflineBackupDiskPrep.exe*  u:  s:<*Staging Location Path*>   p:<*Path to AzurePublishSettingsFile*>`

    | Parametre | Açıklama |
    | --- | --- |
    | U: | Bu zorunlu giriş, bir Azure alma işinin sevkiyat ayrıntılarını güncelleştirmek için kullanılır. |
    | s:&lt;*Evreleme Konum Yolu*&gt; | Bu zorunlu giriş, komut kaynak bilgisayarda çalıştırılmadığı nda kullanılır. "Çevrimdışı yedekleme başlat" bölümünde iş akışına girdiğiniz hazırlama konumuna giden yolu sağlamak için kullanılır. |
    | p:&lt;*PublishSettingsFile Yolu*&gt; | Bu zorunlu giriş, komut kaynak bilgisayarda çalıştırılmadığı nda kullanılır. "Çevrimdışı yedekleme başlat" bölümünde iş akışına girdiğiniz Azure yayımlama ayarları dosyasına giden yolu sağlamak için kullanılır. |

    Yardımcı program, komut farklı bir bilgisayarda çalıştırıldığında kaynak bilgisayarın beklediğini veya hazırlama konumuyla ilişkili alma işlerini otomatik olarak algılar. Daha sonra, bir dizi giriş aracılığıyla gönderi bilgilerini güncelleştirme seçeneği sunar.

    ![Sevkiyat bilgilerini girin](./media/backup-azure-backup-import-export/shippinginputs.png)<br/>

1. Tüm girişler sağlandıktan sonra, ayrıntıları dikkatle gözden geçirin ve **evet**girerek sağladığınız sevkiyat bilgilerini işleme.

    ![Gönderi bilgilerini inceleyin](./media/backup-azure-backup-import-export/reviewshippinginformation.png)<br/>

1. Sevkiyat bilgileri başarıyla güncelleştirildikten sonra, yardımcı program girdiğiniz sevkiyat ayrıntılarının depolandığı yerel bir konum sağlar.

    ![Sevkiyat bilgilerini depolama](./media/backup-azure-backup-import-export/storingshippinginformation.png)<br/>

   > [!IMPORTANT]
   > *AzureOfflineBackupDiskPrep* yardımcı programını kullanarak gönderi bilgilerini sağladıktan sonra sürücülerin Azure veri merkezine iki hafta içinde erişmesini sağlayın. Aksi takdirde sürücüler işlenmesin neden olabilir. 

Önceki adımları tamamladıktan sonra, Azure veri merkezi sürücüleri almaya ve yedekleme verilerini sürücülerden oluşturduğunuz klasik türdeki Azure depolama hesabına aktarmak için bunları işlemeye hazırdır.

### <a name="time-to-process-the-drives"></a>Sürücüleri işleme zamanı

Bir Azure alma işini işlemek için gereken süre değişir. İşlem süresi, sevkiyat süresi, iş türü, kopyalanan verinin türü ve boyutu ve sağlanan disklerin boyutu gibi etkenlere bağlıdır. Azure İçe Alma/Dışa Aktarma hizmetinde SLA yok. Diskler alındıktan sonra, hizmet yedekleme veri kopyasını Azure depolama hesabınıza 7 ila 10 gün içinde tamamlamaya çalışır. Sonraki bölümde, Azure alma işinin durumunu nasıl izleyebilirsiniz.

### <a name="monitor-azure-import-job-status"></a>Azure alma iş durumunu izleme

Sürücüleriniz depolama hesabına kopyalanacak toplu haldeyken veya Azure veri merkezindeyken, kaynak bilgisayardaki Azure Yedekleme Aracısı veya DPM veya MABS konsolu, zamanlanmış yedeklemeleriniz için aşağıdaki iş durumunu gösterir:

  `Waiting for Azure Import Job to complete. Please check on Azure Management portal for more information on job status`

Alma iş durumunu denetlemek için:

1. Kaynak bilgisayarda yükseltilmiş bir komut istemi açın ve aşağıdaki komutu çalıştırın:

     `AzureOfflineBackupDiskPrep.exe u:`

1. Çıktı, alma işinin geçerli durumunu gösterir.

    ![Alma iş durumunu denetleme](./media/backup-azure-backup-import-export/importjobstatusreporting.png)<br/>

Azure alma işinin çeşitli durumları hakkında daha fazla bilgi için Azure [İçe Aktarma/Dışa Aktarma işlerinin durumunu görüntüleyin'](../storage/common/storage-import-export-view-drive-status.md)e bakın.

### <a name="finish-the-workflow"></a>İş akışını sonla

Alma işi bittikten sonra, ilk yedekleme verileri depolama hesabınızda kullanılabilir. Bir sonraki zamanlanan yedekleme sırasında Azure Yedekleme, depolama hesabındaki verilerin içeriğini Kurtarma Hizmetleri kasasına kopyalar.

   ![Verileri Kurtarma Hizmetleri kasasına kopyalama](./media/backup-azure-backup-import-export/copyingfromstorageaccounttoazurebackup.png)<br/>

Bir sonraki zamanlanmış yedekleme sırasında Azure Yedekleme, ilk yedekleme kopyası üzerinde artımlı yedekleme gerçekleştirir.

## <a name="next-steps"></a>Sonraki adımlar

* Azure İçe Alma/Dışa Aktarma hizmeti iş akışıyla ilgili sorularınız için [bkz.](../storage/common/storage-import-export-service.md)
