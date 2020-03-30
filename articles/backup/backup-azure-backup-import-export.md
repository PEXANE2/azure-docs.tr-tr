---
title: Azure İçe Alma/Dışa Aktarma hizmetiyle çevrimdışı yedeklemeyi tohumla
description: Azure İçe Alma/Dışa Aktarma hizmetini kullanarak verileri ağdan göndermek için Azure Yedekleme'yi nasıl kullanabileceğinizi öğrenin. Bu makalede, Azure İçe Alma/Dışa Aktarma hizmetini kullanarak ilk yedekleme verilerinin çevrimdışı tohumlama açıklanmaktadır.
ms.reviewer: saurse
ms.topic: conceptual
ms.date: 05/17/2018
ms.openlocfilehash: 1359616e074f36a1324a418d5b2c889076ced52d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78206767"
---
# <a name="offline-backup-workflow-in-azure-backup"></a>Azure Yedekleme'de çevrimdışı yedekleme iş akışı

Azure Yedekleme, Azure'a verinin ilk tam yedeklemeleri sırasında ağ ve depolama maliyetlerinden tasarruf sağlayan birkaç yerleşik verimlilikzorundadır. İlk tam yedeklemeler genellikle büyük miktarda veri aktarımı sağlar ve yalnızca deltaları/artımlı aygıtları aktaran sonraki yedeklemelerle karşılaştırıldığında daha fazla ağ bant genişliği gerektirir. Azure Yedekleme, çevrimdışı tohumlama işlemi sayesinde çevrimdışı yedekleme verilerini Azure'a yüklemek için diskleri kullanabilir.

Azure Yedekleme çevrimdışı tohumlama işlemi, [Azure İçe Alma/Dışa Aktarma hizmetiyle](../storage/common/storage-import-export-service.md)sıkı bir şekilde tümleştirilir. Bu hizmeti, ilk yedekleme verilerini diskleri kullanarak Azure'a aktarmak için kullanabilirsiniz. Yüksek gecikmeli ve düşük bant genişliğine sahip bir ağ üzerinden aktarılması gereken ilk yedekleme verilerinin terabaytları (TB'leri) varsa, ilk yedekleme kopyasını bir veya daha fazla sabit sürücüye bir veya daha fazla sabit sürücüye nakletmek için çevrimdışı tohumlama iş akışını kullanabilirsiniz. Aşağıdaki resim, iş akışındaki adımlara genel bir bakış sağlar.

  ![Çevrimdışı alma iş akışı sürecine genel bakış](./media/backup-azure-backup-import-export/offlinebackupworkflowoverview.png)

Çevrimdışı yedekleme işlemi şu adımları içerir:

1. Yedekleme verilerini ağ üzerinden göndermek yerine, yedekleme verilerini bir hazırlama konumuna yazın.
1. Bir veya daha fazla SATA diskine hazırlama konumundaki verileri yazmak için *AzureOfflineBackupDiskPrep* yardımcı programını kullanın.
1. Hazırlık çalışmalarının bir parçası olarak, *AzureOfflineBackupDiskPrep* yardımcı programı bir Azure alma işi oluşturur. SATA sürücülerini en yakın Azure veri merkezine gönderin ve etkinlikleri bağlamak için içe aktarma işine başvurun.
1. Azure veri merkezinde, diskler üzerindeki veriler bir Azure depolama hesabına kopyalanır.
1. Azure Yedekleme, depolama hesabındaki yedekleme verilerini Kurtarma Hizmetleri kasasına kopyalar ve artımlı yedeklemeler zamanlanır.

## <a name="supported-configurations"></a>Desteklenen yapılandırmalar

Aşağıdaki Azure Yedekleme özellikleri veya iş yükleri aşağıdakiler için çevrimdışı yedekleme kullanımını destekler:

> [!div class="checklist"]
>
> * Azure Yedekleme Aracısı olarak da adlandırılan Microsoft Azure Kurtarma Hizmetleri (MARS) Aracısı ile dosya ve klasörlerin yedeklemesi.
> * Sistem Merkezi Veri Koruma Yöneticisi (DPM) ile tüm iş yüklerinin ve dosyaların yedeklemi.
> * Microsoft Azure Yedekleme Sunucusu ile tüm iş yüklerinin ve dosyaların yedeklemi.
 
   > [!NOTE]
   > Çevrimdışı yedekleme, Azure Yedekleme Aracısı kullanılarak yapılan sistem durumu yedeklemeleri için desteklenmez.

[!INCLUDE [backup-upgrade-mars-agent.md](../../includes/backup-upgrade-mars-agent.md)]

## <a name="prerequisites"></a>Ön koşullar

  > [!NOTE]
  > Aşağıdaki önkoşullar ve iş akışı yalnızca [en son Azure Kurtarma Hizmetleri Aracısını](https://aka.ms/azurebackup_agent)kullanarak dosyaların ve klasörlerin çevrimdışı yedeklemesi için geçerlidir. System Center DPM veya Azure Backup Server'ı kullanarak iş yükleri için çevrimdışı yedekleme yapmak [için DPM ve Azure Yedekleme Sunucusu için Çevrimdışı yedekleme iş akışına](backup-azure-backup-server-import-export-.md)bakın.

Çevrimdışı yedekleme iş akışını başlatmadan önce aşağıdaki ön koşulları tamamlayın:

* Kurtarma [Hizmetleri kasası](backup-azure-recovery-services-vault-overview.md)oluşturun. Kasa oluşturmak için Kurtarma [Hizmetleri oluştur'daki](tutorial-backup-windows-server-to-azure.md#create-a-recovery-services-vault)adımları izleyin.
* Azure [Yedekleme Aracısı'nın yalnızca en son sürümünün](https://aka.ms/azurebackup_agent) Windows Server veya Windows istemcisinde yüklü olduğundan ve bilgisayarın Kurtarma Hizmetleri kasasına kayıtlı olduğundan emin olun.
* Azure Yedekleme Aracısını çalıştıran bilgisayarda Azure PowerShell 3.7.0 gereklidir. Azure [PowerShell'in 3.7.0 sürümünü indirin ve yükleyin.](https://github.com/Azure/azure-powershell/releases/tag/v3.7.0-March2017)
* Azure Yedekleme Aracısını çalıştıran bilgisayarda, Microsoft Edge veya Internet Explorer 11'in yüklü olduğundan ve JavaScript'in etkin olduğundan emin olun.
* Kurtarma Hizmetleri kasası ile aynı abonelikte bir Azure depolama hesabı oluşturun.
* Azure Active Directory uygulamasını oluşturmak için [gerekli izinlere](../active-directory/develop/howto-create-service-principal-portal.md) sahip olduğundan emin olun. Çevrimdışı yedekleme iş akışı, Azure depolama hesabıyla ilişkili abonelikte bir Azure Etkin Dizin uygulaması oluşturur. Uygulamanın amacı, Azure Yedekleme'ye çevrimdışı yedekleme iş akışı için gerekli olan Azure İçe Aktarma/Dışa Aktarma hizmetine güvenli ve kapsamlı erişim sağlamaktır.
* *Microsoft.ImportExport* kaynak sağlayıcısını Azure depolama hesabı nı içeren abonelikle kaydedin. Kaynak sağlayıcısını kaydetmek için:
    1. Ana menüde **Abonelikler'i**seçin.
    1. Birden çok aboneye aboneyseniz, çevrimdışı yedekleme için kullanmayı planladığınız aboneliği seçin. Yalnızca bir abonelik kullanıyorsanız, aboneliğiniz görüntülenir.
    1. Abonelik menüsünde, sağlayıcılar listesini görüntülemek için **Kaynak sağlayıcıları** seçin.
    1. Sağlayıcılar listesinde, *Microsoft.ImportExport'a*gidin. **Durum** Kayıtlı **Değilse,** **Kaydol'u**seçin.

        ![Kaynak sağlayıcısını kaydetme](./media/backup-azure-backup-import-export/registerimportexport.png)

* İlk kopyanızı tutmak için yeterli disk alanı olan, bilgisayarda bir ağ paylaşımı veya herhangi bir ek sürücü olabilecek bir evreleme konumu oluşturulur. Örneğin, 500 GB'lık bir dosya sunucusunu yedeklemek istiyorsanız, hazırlama alanının en az 500 GB olduğundan emin olun. (Sıkıştırma nedeniyle daha küçük bir miktar kullanılır.)
* Diskleri Azure'a gönderirken yalnızca 2,5 inç SSD veya 2,5 inç veya 3,5 inç SATA II/III dahili sabit diskler kullanın. 10 TB'a kadar sabit diskler kullanabilirsiniz. Hizmetin desteklediği en son sürücü kümesi için [Azure İçE Alma/Dışa Aktarma hizmeti belgelerini](../storage/common/storage-import-export-requirements.md#supported-hardware) denetleyin.
* SATA sürücüleri, evreleme konumundan SATA sürücülerine yedekleme verilerinin kopyalanmasının yapıldığı bilgisayara *(kopya bilgisayar*olarak anılacaktır) bağlanmalıdır. BitLocker'ın kopya bilgisayarda etkin olduğundan emin olun.

## <a name="workflow"></a>İş akışı

Bu bölümde, verilerinizin bir Azure veri merkezine teslim edilebilmeleri ve Azure Depolama'ya yüklenebilmeleri için çevrimdışı yedekleme iş akışı açıklanmaktadır. Alma hizmeti veya işlemin herhangi bir yönü yle ilgili sorularınız varsa, [Azure İçe Alma/Dışa Aktarma hizmetine genel bakış belgelerine](../storage/common/storage-import-export-service.md)bakın.

## <a name="initiate-offline-backup"></a>Çevrimdışı yedekleme başlatma

1. Kurtarma Hizmetleri Aracısı'nda bir yedekleme zamanladiğinizde, bu sayfayı görürsünüz.

    ![Sayfayı içe aktarma](./media/backup-azure-backup-import-export/offlinebackup_inputs.png)

1. **Kendi disklerimi kullanarak Aktar**seçeneğini seçin.

    > [!NOTE]
    > İlk yedekleme verilerini çevrimdışına aktarmak için Azure Veri Kutusu seçeneğini kullanın. Bu seçenek, kendi Azure uyumlu disklerinizi satın almak için gereken çabayı kaydeder. Yedekleme verilerinin Kurtarma Hizmetleri Aracısı tarafından doğrudan yazAbileceği Microsoft'a özel, güvenli ve kurcalanmayan Azure Veri Kutusu aygıtları sunar.

1. **İleri'yi**seçin ve kutuları dikkatlice doldurun.

    ![Disk bilgilerinizi girin](./media/backup-azure-backup-import-export/your-disk-details.png)

   Doldurduğunuz kutular şunlardır:

    * **Evreleme Yeri**: İlk yedek kopyanın yazıldığı geçici depolama konumu. Hazırlama konumu bir ağ paylaşımında veya yerel bir bilgisayarda olabilir. Kopya bilgisayar ve kaynak bilgisayar farklıysa, hazırlama konumunun tam ağ yolunu belirtin.
    * **Azure Kaynak Yöneticisi Depolama Hesabı**: Herhangi bir Azure aboneliğinde Kaynak Yöneticisi türü depolama hesabının (genel amaçlı v1 veya genel amaçlı v2) adı.
    * **Azure Depolama Kapsayıcısı**: Kurtarma Hizmetleri kasasına kopyalanmadan önce yedekleme verilerinin içe aktarıldığı Azure depolama hesabındaki hedef depolama blobunun adı.
    * **Azure Abonelik Kimliği**: Azure depolama hesabının oluşturulduğu Azure aboneliğinin kimliği.
    * **Azure İçe Alma İş Adı**: Azure İçe Aktarma/Verme hizmeti ve Azure Yedekleme'nin disklere gönderilen verilerin Azure'a aktarılmasını izlediği benzersiz addır.
  
   Kutuları doldurduktan sonra **İleri'yi**seçin. Hazırlama **Konumunu** ve **Azure İçe Alma İş Adı** bilgilerini kaydedin. Diskleri hazırlamak için gereklidir.

1. İstendiğinde Azure aboneliğinizde oturum açın. Azure Yedekleme'nin Azure Active Directory uygulamasını oluşturabilmesi için oturum açmanız gerekir. Azure İçe Alma/Dışa Aktarma hizmetine erişmek için gerekli izinleri girin.

    ![Azure abonelik oturum açma sayfası](./media/backup-azure-backup-import-export/azure-login.png)

1. İş akışını tamamla. Azure Yedekleme Aracısı konsolunda **Şimdi Yukarı'yı seçin.**

    ![Şimdi Yedekle](./media/backup-azure-backup-import-export/backupnow.png)

1. Sihirbazın **Onay** sayfasında **Yukarı'yı seçin.** İlk yedekleme, kurulumun bir parçası olarak hazırlama alanına yazılır.

   ![Yedeklemeye hazır olduğunuzu doğrulayın](./media/backup-azure-backup-import-export/backupnow-confirmation.png)

    İşlem bittikten sonra, evreleme konumu disk hazırlama için kullanılmaya hazırdır.

   ![Şimdi Yedekle Sihirbazı sayfası](./media/backup-azure-backup-import-export/opbackupnow.png)

## <a name="prepare-sata-drives-and-ship-to-azure"></a>SATA sürücülerini hazırlayın ve Azure'a yönlendirin

*AzureOfflineBackupDiskPrep* yardımcı programı, en yakın Azure veri merkezine gönderilen SATA sürücülerini hazırlar. Bu yardımcı program, Azure Yedekleme Aracısı yükleme dizininde aşağıdaki şekilde kullanılabilir:

```*\Microsoft Azure Recovery Services Agent\Utils\\*```

1. Dizine gidin ve *AzureOfflineBackupDiskPrep* dizinini SATA sürücülerinin bağlı olduğu başka bir bilgisayara kopyalayın. Bağlı SATA sürücülerine sahip bilgisayarda şunları

   * Kopya bilgisayarı, "Çevrimdışı yedekleme başlat" bölümünde iş akışında sağlanan aynı ağ yolunu kullanarak çevrimdışı tohumlama iş akışı için hazırlama konumuna erişebilir.
   * BitLocker kopya bilgisayarda etkindir.
   * Azure PowerShell 3.7.0 yüklendi.
   * En son uyumlu tarayıcılar (Microsoft Edge veya Internet Explorer 11) yüklenir ve JavaScript etkinleştirilir.
   * Kopya bilgisayar Azure portalına erişebilir. Gerekirse, kopyalama bilgisayarı kaynak bilgisayarla aynı olabilir.

     > [!IMPORTANT]
     > Kaynak bilgisayar sanal bir makineyse, kopya bilgisayar kaynak bilgisayardan farklı bir fiziksel sunucu veya istemci makinesi olmalıdır.

1. Geçerli dizini olarak *AzureOfflineBackupDiskPrep* yardımcı dizini ile kopya bilgisayarda yükseltilmiş bir komut istemi açın. Şu komutu çalıştırın:

    ```.\AzureOfflineBackupDiskPrep.exe s:<Staging Location Path>```

    | Parametre | Açıklama |
    | --- | --- |
    | s:&lt;*Evreleme Konum Yolu*&gt; |Bu zorunlu giriş, "Çevrimdışı yedekleme başlat" bölümünde iş akışına girdiğiniz hazırlama konumuna giden yolu sağlamak için kullanılır. |
    | p:&lt;*PublishSettingsFile Yolu*&gt; |Bu isteğe bağlı giriş, "Çevrimdışı yedekleme başlat" bölümünde iş akışına girdiğiniz Azure yayımlama ayarları dosyasına giden yolu sağlamak için kullanılır. |

    Komutu çalıştırdığınızda, yardımcı program hazırlanması gereken sürücülere karşılık gelen Azure alma işinin seçimini ister. Yalnızca tek bir alma işi sağlanan evreleme konumuyla ilişkiliyse, bunun gibi bir sayfa görürsünüz.

    ![Azure disk hazırlama aracı girişi](./media/backup-azure-backup-import-export/diskprepconsole0_1.png) <br/>

1. Azure'a aktarmak için hazırlamak istediğiniz monte edilmiş disk için sondaki üst üste olmadan sürücü mektubunu girin.
1. İstendiğinde sürücünün biçimlendirmesi için onay sağlayın.
1. Azure aboneliğinizde oturum açmanız istenir. Kimlik bilgilerinizi girin.

    ![Azure abonelik oturum açma](./media/backup-azure-backup-import-export/signindiskprep.png) <br/>

    Araç daha sonra diski hazırlamaya ve yedekleme verilerini kopyalamaya başlar. Sağlanan diskin yedekleme verileri için yeterli alana sahip olmaması durumunda araç tarafından istendiğinde ek diskeklemeniz gerekebilir. <br/>

    Aracın başarılı bir şekilde yürütülmesinin sonunda, komut istemi üç bilgi sağlar:

   1. Sağladığınız bir veya daha fazla disk Azure'a gönderim için hazırdır.
   1. Alma işinizin oluşturulduğuna dair onay alırsınız. Alma işi sağladığınız adı kullanır.
   1. Araç, Azure veri merkezinin sevkiyat adresini görüntüler.

      ![Azure disk hazırlama tamamlandı](./media/backup-azure-backup-import-export/console2.png)<br/>

1. Komut yürütmesonunda, sevkiyat bilgilerini güncelleştirebilirsiniz.

1. Diskleri aracın sağladığı adrese gönderin. Gelecekteki başvuru için izleme numarasını saklayın.

   > [!IMPORTANT]
   > İki Azure içe aktarma işi aynı izleme numarasına sahip olamaz. Yardımcı program tarafından tek bir Azure alma işi altında hazırlanan sürücülerin tek bir pakette birlikte gönderildiğinden ve paket için tek bir benzersiz izleme numarası olduğundan emin olun. Ayrı Azure alma işlerinin bir parçası olarak hazırlanan sürücüleri tek bir pakette birleştirmeyin.

## <a name="update-shipping-details-on-the-azure-import-job"></a>Azure alma işiyle ilgili sevkiyat ayrıntılarını güncelleştirme

Aşağıdaki yordam, Azure içe aktarma iş sevkiyat ayrıntılarını güncelleştirir. Bu bilgiler hakkında ayrıntılar içerir:

* Diskleri Azure'a teslim eden operatörün adı.
* Diskleriniz için sevkiyat ayrıntılarını iade edin.

1. Azure aboneliğinizde oturum açın.
1. Ana menüde **Tüm hizmetleri**seçin. Tüm **hizmetler** iletişim kutusuna, **Içeri aktar'ı**girin. **İçe Alma/Dışa Aktarma işleri**gördüğünüzde, seçin.

    ![Sevkiyat bilgilerini girin](./media/backup-azure-backup-import-export/search-import-job.png)<br/>

    **İçe/Dışa Aktar işleri** menüsü açılır ve seçili abonelikteki tüm içe/dışa aktarma işlerinin listesi görüntülenir.

1. Birden çok aboneliğiniz varsa, yedekleme verilerini almak için kullanılan aboneliği seçin. Ardından, ayrıntılarını açmak için yeni oluşturulan içe aktarma işini seçin.

    ![Gönderi bilgilerini inceleyin](./media/backup-azure-backup-import-export/import-job-found.png)<br/>

1. Alma işi için **Ayarlar** menüsünde, **gönderi bilgilerini yönet'i**seçin. İade gönderim ayrıntılarını girin.

    ![Sevkiyat bilgilerini depolama](./media/backup-azure-backup-import-export/shipping-info.png)<br/>

1. Gönderi operatörünüzden izleme numarası aldığınızda, Azure içe aktarma işine genel bakış sayfasındaki banner'ı seçin ve aşağıdaki ayrıntıları girin.

   > [!IMPORTANT]
   > Azure içeri aktarma işini oluşturduktan itibaren iki hafta içinde taşıyıcı bilgilerini ve takip numarasını güncelleştirmeyi unutmayın. Bu bilgilerin iki hafta içinde doğrulanmaması, işin silinmesine ve sürücülerin işlenilmemesine neden olabilir.

   ![Bilgi güncelleştirme uyarısını izleme](./media/backup-azure-backup-import-export/joboverview.png)<br/>

   ![Taşıyıcı bilgileri ve takip numarası](./media/backup-azure-backup-import-export/tracking-info.png)

### <a name="time-to-process-the-drives"></a>Sürücüleri işleme zamanı

Bir Azure alma işini işlemek için gereken süre değişir. İşlem süresi, sevkiyat süresi, iş türü, kopyalanan verilerin türü ve boyutu ve sağlanan disklerin boyutu gibi etkenlere dayanır. Azure İçe Alma/Dışa Aktarma hizmetinde SLA yok. Diskler alındıktan sonra, hizmet yedekleme veri kopyasını Azure depolama hesabınıza 7 ila 10 gün içinde tamamlamaya çalışır.

### <a name="monitor-azure-import-job-status"></a>Azure alma iş durumunu izleme

Azure portalından alma işinizin durumunu izleyebilirsiniz. **İşlerini İçe Aktar sayfasına** gidin ve işinizi seçin. Alma işlerinin durumu hakkında daha fazla bilgi için Azure [İçe Aktarma/Verme hizmeti nedir?](../storage/common/storage-import-export-service.md)

### <a name="finish-the-workflow"></a>İş akışını sonla

Alma işi başarıyla tamamlandıktan sonra, ilk yedekleme verileri depolama hesabınızda kullanılabilir. Bir sonraki zamanlanan yedekleme sırasında Azure Yedekleme, depolama hesabındaki verilerin içeriğini Kurtarma Hizmetleri kasasına kopyalar.

   ![Verileri Kurtarma Hizmetleri kasasına kopyalama](./media/backup-azure-backup-import-export/copyingfromstorageaccounttoazurebackup.png)<br/>

Bir sonraki zamanlanmış yedekleme sırasında Azure Yedekleme artımlı bir yedekleme gerçekleştirir.

### <a name="clean-up-resources"></a>Kaynakları temizleme

İlk yedekleme tamamlandıktan sonra, Azure Depolama kapsayıcısına aktarılan verileri ve hazırlama konumundaki yedekleme verilerini güvenle silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Azure İçe Alma/Dışa Aktarma hizmeti iş akışıyla ilgili sorularınız için [bkz.](../storage/common/storage-import-export-service.md)
