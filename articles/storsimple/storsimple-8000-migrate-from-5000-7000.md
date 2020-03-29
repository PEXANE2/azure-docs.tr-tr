---
title: StorSimple 5000-7000 serisi verileri 8000 serisi cihaza geçirin| Microsoft Dokümanlar
description: Geçiş özelliğine genel bir bakış ve ön koşullar sağlar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: twooley
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/23/2018
ms.author: alkohli
ms.openlocfilehash: 967c03f3c4201bdcf1529fdda93717b6eb74e771
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60631683"
---
# <a name="migrate-data-from-storsimple-5000-7000-series-to-8000-series-device"></a>StorSimple 5000-7000 serisindeki verileri 8000 serisi cihaza geçirme

> [!IMPORTANT]
> - 31 Temmuz 2019'da StorSimple 5000/7000 serisi destek sonu (EOS) durumuna ulaşacak. StorSimple 5000/7000 serisi müşterilerin belgede açıklanan alternatiflerden birine geçiş yapmanızı öneririz.
> - Geçiş şu anda yardımcı bir işlemdir. StorSimple 5000-7000 serisi aygıtınızdaki verileri 8000 serisi bir aygıta aktarmak istiyorsanız, Microsoft Desteği ile geçiş zamanlamanız gerekir. Microsoft Destek daha sonra geçiş aboneliğinizi etkinleştirecektir. Daha fazla bilgi için [Destek biletinin](storsimple-8000-contact-microsoft-support.md)nasıl açılacağını görün.
> - Hizmet isteğini dosyaladıktan sonra, geçiş planını yürütmek ve geçişi gerçekten başlatmak birkaç hafta sürebilir.
> - Microsoft Destek'e başvurmadan önce, makalede belirtilen [Geçiş ön koşullarının](#migration-prerequisites) gözden geçirilip tamamlandığından emin olun.

## <a name="overview"></a>Genel Bakış

Bu makalede, StorSimple 5000-7000 serisi müşterilerin verilerini StorSimple 8000 serisi fiziksel aygıta veya 8010/8020 bulut cihazına geçirmelerine olanak tanıyan geçiş özelliği tanıtıştır. Bu makalede, 5000-7000 serisi eski bir cihazdan 8000 serisi fiziksel veya bulut aygıtına veri geçirmek için gereken adımların indirilebilir adım adım walkthrough'una da bağlantılar.

Bu makale, hem şirket içi 8000 serisi cihaz hem de StorSimple Cloud Appliance için geçerlidir.


## <a name="migration-feature-versus-host-side-migration"></a>Ana bilgisayar tarafı geçişine karşı geçiş özelliği

Geçiş özelliğini kullanarak veya ana bilgisayar tarafı geçişi gerçekleştirerek verilerinizi taşıyabilirsiniz. Bu bölümde artıları ve eksileri de dahil olmak üzere her yöntemin özellikleri açıklanmaktadır. Verilerinizi geçirmek için hangi yöntemi takip etmek istediğinizi bulmak için bu bilgileri kullanın.

Geçiş özelliği, 7000/5000 serisinden 8000 serisine kadar bir olağanüstü durum kurtarma (DR) işlemini simüle eder. Bu özellik, verileri Azure'da 5000/7000 seri biçiminden 8000 seri biçimine geçirmenize olanak tanır. Geçiş işlemi StorSimple Geçiş aracı kullanılarak başlatılır. Araç, 8000 serisi aygıttaki yedekleme meta verilerinin karşıdan yüksesini ve dönüşümlerini başlatır ve ardından aygıttaki birimleri ortaya çıkarmak için en son yedeklemeyi kullanır.

|      | Artıları                                                                                                                                     |Simgeler                                                                                                                                                              |
|------|-------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1.   | Geçiş işlemi, 5000/7000 serisinde alınan yedeklemelerin geçmişini korur.                                               | Kullanıcılar verilere erişmeye çalıştıklarında, bu geçiş verileri Azure'dan karşıdan indirir ve böylece veri indirme maliyetleri ne olur.                                     |
| 2.   | Ana bilgisayar tarafında hiçbir veri geçirilir.                                                                                                     | İşlem, yedeklemenin başlangıcı ile 8000 serisinde su yüzüne çıkan en son yedekleme arasında kapalı kalma süresi ne kadar dır (geçiş aracı kullanılarak tahmin edilebilir). |
| 3.   | Bu işlem, 8000 serisi aygıttaki tüm ilkeleri, bant genişliği şablonlarını, şifrelemeyi ve diğer ayarları korur.                      | Kullanıcı erişimi yalnızca kullanıcılar tarafından erişilen verileri geri getirir ve tüm veri kümesini yeniden sulandıramaz.                                                  |
| 4.   | Bu işlem, Azure'daki tüm eski yedeklemeleri dönüştürmek için ek süre gerektirir ve bu yedeklemeler üretimi etkilemeden eşzamanlı olarak yapılır | Geçiş yalnızca bulut yapılandırma düzeyinde yapılabilir.  Bulut yapılandırmasındaki tek tek birimler ayrı olarak geçirilemez                       |

Ana bilgisayar geçişi, 8000 serisinin bağımsız olarak ayarlanmasına ve verilerin 5000/7000 serisi aygıttan 8000 serisi aygıta kopyalanmasına olanak tanır. Bu, verileri bir depolama aygıtından diğerine geçirmekle eşdeğerdir. Verileri kopyalamak için Diskboss, robocopy gibi çeşitli araçlar kullanılır.

|      | Artıları                                                                                                                      |Simgeler                                                                                                                                                                                                      |
|------|---------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1.   | Geçişe hacim bazında aşamalı olarak yaklaşılabilir.                                               | Önceki yedeklemeler (5000/7000 serisi yle alınan) 8000 serisi cihazda kullanılamaz.                                                                                                       |
| 2.   | Azure'da verilerin tek bir depolama hesabında birleştirilmesine olanak tanır.                                                       | 8000 serisindeki bulutun ilk yedeklemesi daha uzun sürer, çünkü 8000 serisindeki tüm verilerin Azure'a yedeklemesi gerekir.                                                                     |
| 3.   | Başarılı bir geçişin ardından, cihazın tüm verileri yereldir. Verilere erişirken gecikme yok. | Veriler 5000/7000 aygıtından silinene kadar Azure depolama tüketimi artar.                                                                                                        |
| 4.   |                                                                                                                           | 7000/5000 serisi aygıtında büyük miktarda veri varsa, geçiş sırasında bu verilerin azure'dan indirilmesi gerekir ve bu da Azure'dan veri indirmeyle ilgili maliyetlere ve gecikmelere neden olur |

Bu makale, yalnızca 5000/7000 ile 8000 serisi aygıttan geçiş özelliğine odaklanır. Ana bilgisayar tarafı geçişi hakkında daha fazla bilgi için [diğer depolama aygıtlarından Geçiş'e](https://download.microsoft.com/download/9/4/A/94AB8165-CCC4-430B-801B-9FD40C8DA340/Migrating%20Data%20to%20StorSimple%20Volumes_09-02-15.pdf)gidin.

## <a name="migration-prerequisites"></a>Geçiş ön koşulları

İşte eski 5000 veya 7000 serisi cihaz ve 8000 serisi StorSimple cihaz için geçiş ön koşulları vardır.

> [!IMPORTANT]
> Microsoft Destek'e bir hizmet isteği sunmadan önce geçiş ön koşullarıgözden geçirin ve tamamlayın.

### <a name="for-the-50007000-series-device-source"></a>5000/7000 serisi cihaz için (kaynak)

Geçişe başlamadan önce şunları emin olun:

* 5000 veya 7000 serisi kaynak cihazınız var; cihaz canlı veya aşağı olabilir.

    > [!IMPORTANT]
    > Geçiş işlemi boyunca bu aygıta seri erişime sahip olduğunuzu öneririz. Herhangi bir aygıt sorunu olması halinde, seri erişim sorun giderme konusunda yardımcı olabilir.

* 5000 veya 7000 serisi kaynak cihazınız v2.1.1.518 veya sonraki yazılım sürümünü çalıştırıyor. Önceki sürümler desteklenmez.
* 5000 veya 7000 serisinin çalıştığını doğrulamak için Web UI'nizin sağ üst köşesine bakın. Bu, cihazınızın çalıştırdığı yazılım sürümünü görüntülemelidir. Geçiş için, 5000 veya 7000 serisi v2.1.1.518 çalışıyor olmalıdır.

    ![Eski aygıtta yazılım sürümünü denetleme](media/storsimple-8000-migrate-from-5000-7000/check-version-legacy-device1.png)

    * Canlı cihazınız v2.1.1.518 veya daha yeni bir şekilde çalışmıyorsa, lütfen sisteminizi gerekli minimum sürüme yükseltin. Ayrıntılı talimatlar için [sisteminizi v2.1.1.518'e yükseltin'e](http://onlinehelp.storsimple.com/111_Appliance/6_System_Upgrade_Guides/Current_(v2.1.1)/000_Software_Patch_Upgrade_Guide_v2.1.1.518)bakın.
    * V2.1.1.518 çalıştırıyorsanız, kayıt defteri geri yükleme hataları için herhangi bir bildirim olup olmadığını görmek için web UI'ye gidin. Kayıt defteri geri yüklemesi başarısız olduysa, kayıt defteri geri yüklemeyi çalıştırın. Daha fazla bilgi [için, kayıt defteri geri yükleme](http://onlinehelp.storsimple.com/111_Appliance/2_User_Guides/1_Current_(v2.1.1)/1_Web_UI_User_Guide_WIP/2_Configuration/4_Cloud_Accounts/1_Cloud_Credentials#Restoring_Backup_Registry)çalıştırın nasıl gidin.
    * V2.1.1.518 çalışmayan bir aşağı aygıtınız varsa, v2.1.1.518 çalıştıran bir değiştirme aygıtına bir hata gerçekleştirin. Ayrıntılı talimatlar için 5000/7000 serisi StorSimple cihazınızın DR'sine bakın.
    * Bulut anlık görüntüsünü alarak cihazınızın verilerini yedekle.
    * Kaynak aygıtta çalışan diğer etkin yedekleme işlerini denetleyin. Buna StorSimple Veri Koruma Konsolu ana bilgisayardaki işler de dahildir. Geçerli işlerin tamamlanmasını bekleyin.


### <a name="for-the-8000-series-physical-device-target"></a>8000 serisi fiziksel cihaz (hedef) için

Geçişe başlamadan önce şunları emin olun:

* Hedef 8000 serisi cihazınız kayıtlı ve çalışıyor. Daha fazla bilgi için [StorSimple cihazınızı StorSimple Manager hizmetiyle nasıl dağıtılayabilirsiniz.](storsimple-8000-deployment-walkthrough-u2.md)
* 8000 serisi cihazınız da en son StorSimple 8000 Serisi Güncelleme 5 yüklü ve 6.3.9600.17845 veya daha sonraki sürümü çalışıyor. Aygıtınızda en son güncelleştirmeler yüklü değilse, geçişe devam etmeden önce en son güncelleştirmeleri yüklemeniz gerekir. Daha fazla bilgi için, [8000 serisi cihazınıza en son güncelleştirmeyi](storsimple-8000-install-update-5.md)nasıl yükleyebilirsiniz.
* Azure aboneliğiniz geçiş için etkinleştirildi. Aboneliğiniz etkinleştirilemiyorsa, aboneliğinizi geçiş için etkinleştirmek için Microsoft Destek'e başvurun.

### <a name="for-the-80108020-cloud-appliance-target"></a>8010/8020 bulut cihazı için (hedef)

Geçişe başlamadan önce şunları sağlayın:

* Hedef bulut cihazınız kayıtlı ve çalışıyor. Daha fazla bilgi için StorSimple Cloud Appliance'ı nasıl [dağıtıp yönetebilirsiniz.](storsimple-8000-cloud-appliance-u2.md)
* Bulut cihazınız en son StorSimple 8000 Serisi Güncelleme 5 yazılım sürümünü 6.3.9600.17845 olarak çalıştırıyor. Bulut cihazınız Güncelleme 5'i çalıştırmıyorsa, geçişe geçmeden önce yeni bir Update 5 bulut cihazı oluşturun. Daha fazla bilgi için [8010/8020 bulut cihazının](storsimple-8000-cloud-appliance-u2.md)nasıl oluşturulabildiğini öğrenin.

### <a name="for-the-computer-running-storsimple-migration-tool"></a>StorSimple Geçiş aracını çalıştıran bilgisayar için

StorSimple Geçiş aracı, StorSimple 5000-7000 serisindeki verileri 8000 serisi bir aygıta geçirmenizi sağlayan Kullanıcı Arabirimi tabanlı bir araçtır. StorSimple Geçiş aracını yüklemek için aşağıdaki gereksinimleri karşılayan bir bilgisayar kullanın.

Bilgisayar internet bağlantısı na sahiptir ve:

* Aşağıdaki işletim sistemini çalıştırıyor
    * Windows 10.
    * Windows Server 2012 R2 (veya üstü) StorSimple Geçiş aracını yüklemek için.
* .NET 4.5.2 yüklü.
* Aracı yüklemek ve kullanmak için en az 5 GB boş alana sahiptir.

> [!TIP]
> StorSimple aygıtınız bir Windows Server ana bilgisayarına bağlıysa, geçiş aracını Windows Server ana bilgisayarına yükleyebilirsiniz.

#### <a name="to-install-storsimple-migration-tool"></a>StorSimple Geçiş aracını yüklemek için

Bilgisayarınızda StorSimple Geçiş aracını yüklemek için aşağıdaki adımları gerçekleştirin.

1. _StorSimple8000SeriesMigrationTool_ klasörünü Windows bilgisayarınıza kopyalayın. Yazılımın kopyalandığı sürücünün yeterli alana sahip olduğundan emin olun.

    Araç config _dosyasını açın StorSimple8000SeriesMigrationTool.exe.config_ klasöründe. İşte dosyanın parçacığı.
    
    ```xml
        <add key="UserName" value="username@xyz.com" />
        <add key="SubscriptionName" value="YourSubscriptionName" />
        <add key="SubscriptionId" value="YourSubscriptionId" />
        <add key="TenantId" value="YourTenantId" />
        <add key="ResourceName" value="YourResourceName" />
        <add key="ResourceGroupName" value="YourResourceGroupName" />

    ```
2. Tuşlara karşılık gelen değerleri edin ve değiştirin:

    * `UserName`– Azure portalında oturum açmak için kullanıcı adı.
    * `SubscriptionName and SubscriptionId`– Azure aboneliğiniz için ad ve kimlik. StorSimple Device Manager hizmet açılış sayfanızda, **Genel**altında **Özellikler'i**tıklatın. Hizmetinizle ilişkili Abonelik adını ve Abonelik Kimliğini kopyalayın.
    * `ResourceName`– Azure portalındaki StorSimple Device Manager hizmetinizin adı. Ayrıca hizmet özellikleri altında gösterilir.
    * `ResourceGroup`– Azure portalındaki StorSimple Device Manager hizmetinizle ilişkili kaynak grubunun adı. Ayrıca hizmet özellikleri altında gösterilir.
    ![Hedef aygıt için servis özelliklerini denetleme](media/storsimple-8000-migrate-from-5000-7000/check-service-properties1.png)
    * `TenantId`– Azure portalında Azure Active Directory Kiracı Kimliği. Yönetici olarak Microsoft Azure'da oturum açın. Microsoft Azure portalında **Azure Etkin Dizini'ni**tıklatın. **Yönet**'in altında **Özellikler**’e tıklayın. Kiracı kimliği **Dizin Kimliği** kutusunda gösterilir.
    ![Azure Etkin Dizini için Kiracı Kimliğini Denetleme](media/storsimple-8000-migrate-from-5000-7000/check-tenantid-aad.png)

3.  Config dosyasına yapılan değişiklikleri kaydedin.
4.  Aracı başlatmak için _StorSimple8000SeriesMigrationTool.exe_ çalıştırın. Kimlik bilgileri istendiğinde, Azure portalında aboneliğinizle ilişkili kimlik bilgilerini sağlayın. 
5.  StorSimple Geçiş aracı Kullanıcı Arabirimi görüntülenir.
  

## <a name="next-steps"></a>Sonraki adımlar
[StorSimple 5000-7000 serisinden 8000 serisi bir cihaza verileri](https://gallery.technet.microsoft.com/Azure-StorSimple-50007000-c1a0460b)nasıl geçirilir ile ilgili adım adım yönergeleri indirin.
