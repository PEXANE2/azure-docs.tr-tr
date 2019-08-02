---
title: Azure günlük tümleştirmesi ile çalışmaya başlama | Microsoft Docs
description: Azure günlük tümleştirme hizmetini yüklemeyi ve günlükleri Azure depolama, Azure denetim günlükleri ve Azure Güvenlik Merkezi uyarılarından tümleştirme hakkında bilgi edinin.
services: security
documentationcenter: na
author: Barclayn
manager: barbkess
editor: TomShinder
ms.assetid: 53f67a7c-7e17-4c19-ac5c-a43fabff70e1
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 05/28/2019
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: 861a72b6abdc3d06cc4737630c92d34eee2e8545
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68727680"
---
# <a name="azure-log-integration-with-azure-diagnostics-logging-and-windows-event-forwarding"></a>Azure günlük tümleştirmesi Azure Tanılama günlük kaydı ve Windows olay iletimi


>[!IMPORTANT]
> Azure günlük tümleştirme özelliği 06/15/2019 tarafından kullanım dışı bırakılacak. AzLog İndirmeleri 27 Haz 2018 ' de devre dışı bırakıldı. İleriye doğru iletme hakkında yönergeler için gönderiyi [SıEM araçlarıyla tümleştirme Için Azure Izleyicisini kullanma](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/) 

Yalnızca güvenlik olayınızdan ve olay yönetimi (SıEM) satıcınızdan bir [Azure izleyici](/azure/monitoring-and-diagnostics/monitoring-get-started) Bağlayıcısı yoksa Azure günlük tümleştirmesi ' ni kullanmanız gerekir.

Azure günlük tümleştirmesi, Azure günlüklerini SıEM 'niz için kullanılabilir hale getirir. böylece tüm varlıklarınız için birleştirilmiş bir güvenlik panosu oluşturabilirsiniz.
Bir Azure Izleyici bağlayıcısının durumu hakkında daha fazla bilgi için SıEM satıcınız ile iletişime geçin.

> [!IMPORTANT]
> Birincil İlginiz sanal makine günlüklerini topluyorsa, en SıEM satıcıları çözümünde bu seçeneği içerir. SıEM satıcısının bağlayıcısının kullanılması her zaman tercih edilen alternatiftir.

Bu makale, Azure günlük tümleştirmesini kullanmaya başlamanıza yardımcı olur. Azure günlük tümleştirme hizmeti 'ni yükleme ve hizmeti Azure Tanılama ile tümleştirme konusuna odaklanır. Azure günlük tümleştirme hizmeti daha sonra bir Azure altyapısında hizmet olarak dağıtılan sanal makinelerden Windows Güvenlik olayı kanalından Windows olay günlüğü bilgilerini toplar. Bu, şirket içi sistemde kullanabileceğiniz *olay iletmeyle* benzerdir.

> [!NOTE]
> Azure günlük tümleştirmesinin çıkışını SıEM ile tümleştirmek SıEM tarafından yapılır. Daha fazla bilgi için bkz. Şirket [ıçı SıEM Ile Azure günlük tümleştirmesini](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/)tümleştirme.

Azure günlük tümleştirme hizmeti, Windows Server 2008 R2 veya üstünü çalıştıran bir fiziksel veya sanal bilgisayar üzerinde çalışır (Windows Server 2016 veya Windows Server 2012 R2 tercih edilir).

Fiziksel bir bilgisayar, şirket içinde veya barındırma sitesinde çalıştırılabilir. Azure günlük tümleştirme hizmetini bir sanal makinede çalıştırmayı seçerseniz, sanal makine şirket içinde veya Microsoft Azure gibi bir genel bulutta bulunabilir.

Azure günlük tümleştirme hizmetini çalıştıran fiziksel veya sanal makine, Azure genel bulutuna ağ bağlantısı gerektirir. Bu makale, gerekli yapılandırma hakkında ayrıntılı bilgi sağlar.

## <a name="prerequisites"></a>Önkoşullar

Azure günlük tümleştirmesi 'nin yüklenmesi için en azından aşağıdaki öğeler gerekir:

* Bir **Azure aboneliği**. Bir aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) için kaydolabilirsiniz.
* Windows Azure Tanılama (WAD) günlüğü için kullanılabilen bir **depolama hesabı** . Önceden yapılandırılmış bir depolama hesabı kullanabilir veya yeni bir depolama hesabı oluşturabilirsiniz. Bu makalenin ilerleyen bölümlerinde, depolama hesabının nasıl yapılandırılacağı anlatılmaktadır.

  > [!NOTE]
  > Senaryonuza bağlı olarak bir depolama hesabı gerekli olmayabilir. Bu makalede ele alınan Azure Tanılama senaryosunda bir depolama hesabı gereklidir.

* **İki sistem**: 
  * Azure günlük tümleştirme hizmetini çalıştıran bir makine. Bu makine, daha sonra SıEM 'nize aktarılan tüm günlük bilgilerini toplar. Bu sistem:
    * Şirket içinde bulunabilir veya Microsoft Azure içinde barındırılabilir.  
    * Windows Server 2008 R2 SP1 veya üzeri bir x64 sürümü çalıştırıyor ve Microsoft .NET 4.5.1 yüklü olmalıdır. Yüklü .NET sürümünü öğrenmek için bkz. [hangi .NET Framework sürümlerinin yüklendiğini belirleme](https://msdn.microsoft.com/library/hh925568).  
    * Azure Tanılama günlük kaydı için kullanılan Azure depolama hesabı ile bağlantı olmalıdır. Bu makalenin ilerleyen kısımlarında, bağlantının nasıl doğrulanacağı anlatılmaktadır.
  * İzlemek istediğiniz bir makine. Bu, bir [Azure sanal makinesi](/azure/virtual-machines/virtual-machines-windows-overview)olarak çalışan bir VM 'dir. Bu makineden günlüğe kaydetme bilgileri Azure günlük tümleştirme hizmeti makinesine gönderilir.

Azure portal kullanarak sanal makine oluşturma hakkında hızlı bir gösterim için aşağıdaki videoya göz atın:<br /><br />


> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Create-a-Virtual-Machine/player]

## <a name="deployment-considerations"></a>Dağıtma konuları

Sınama sırasında, en düşük işletim sistemi gereksinimlerini karşılayan herhangi bir sistemi kullanabilirsiniz. Bir üretim ortamında, yük ölçeği büyütmeyi veya ölçeklendirmeyi planlamanız gerekebilir.

Azure günlük tümleştirme hizmeti 'nin birden çok örneğini çalıştırabilirsiniz. Ancak, hizmetin fiziksel veya sanal makine başına yalnızca bir örneğini çalıştırabilirsiniz. Ayrıca, WAD için Azure Tanılama depolama hesaplarının yükünü dengeleyebilirsiniz. Örneklere sağlanacak aboneliklerin sayısı kapasiteye göre belirlenir.

> [!NOTE]
> Şu anda Azure günlük tümleştirme makinelerinin (yani, Azure günlük tümleştirme hizmeti çalıştıran makineler) örneklerinin ne zaman Ölçeklendirilecek ve depolama hesapları ya da aboneliklerde belirli önerilere sahip olduğumuz durumlar yoktur. Bu alanların her birinde performans gözlemlerinizi temel alarak ölçekleme kararları alın.

Performansı artırmaya yardımcı olmak için Azure günlük tümleştirme hizmeti 'ni ölçeklendirme seçeneğiniz de vardır. Aşağıdaki performans ölçümleri, Azure günlük tümleştirme hizmetini çalıştırmak için seçtiğiniz makineleri boyutlandıralmanıza yardımcı olabilir:

* 8 işlemci (çekirdek) makinede, Azure günlük tümleştirmesinin tek bir örneği günde 24.000.000 olay (saat başına yaklaşık 1.000.000 olay) işleyebilir.
* 4 işlemcili (çekirdek) makinede, Azure günlük tümleştirmesinin tek bir örneği günde 1.500.000 olay (saat başına yaklaşık 62.500 olay) işleyebilir.

## <a name="install-azure-log-integration"></a>Azure günlük tümleştirmesini yükler

Kurulum yordamını çalıştırın. Microsoft 'a telemetri bilgilerinin sağlanması gerekip gerekmediğini seçin.

Azure günlük tümleştirme hizmeti, üzerinde yüklü olduğu makineden telemetri verilerini toplar.  

Toplanan telemetri verileri şunları içerir:

* Azure günlük tümleştirmesinin yürütülmesi sırasında oluşan özel durumlar.
* İşlenen sorguların ve olayların sayısıyla ilgili ölçümler.
* Hangi Azlog. exe komut satırı seçeneklerinin kullanıldığı hakkında istatistikler. 

> [!NOTE]
> Microsoft 'un telemetri verilerini toplamasına izin vermeniz önerilir. Telemetri verilerinin toplanmasını **Microsoft 'un telemetri verilerini toplamasına Izin ver** onay kutusunu temizleyerek devre dışı bırakabilirsiniz.
>

![Telemetri onay kutusu seçiliyken yükleme bölmesinin ekran görüntüsü](./media/azure-log-integration-get-started/telemetry.png)


Yükleme işlemi aşağıdaki videoda ele alınmıştır:<br /><br />

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Install-Azure-Log-Integration/player]

## <a name="post-installation-and-validation-steps"></a>Yükleme sonrası ve doğrulama adımları

Temel kurulumu tamamladıktan sonra yükleme sonrası ve doğrulama adımlarını gerçekleştirmeye hazırsınız demektir:

1. PowerShell'i yönetici olarak açın. Ardından, C:\Program Files\Microsoft Azure günlük tümleştirmesi ' ne gidin.
2. Azure günlük tümleştirme cmdlet 'lerini içeri aktarın. Cmdlet 'leri içeri aktarmak için betiği `LoadAzlogModule.ps1`çalıştırın. Yazın ve ardından ENTER tuşuna basın (öğesinin kullanımını aklınızda bulunan **.\\**  Bu komutta). `.\LoadAzlogModule.ps1` Aşağıdaki şekilde görünen gibi bir şey görmeniz gerekir:

   ![LoadAzlogModule. ps1 komutunun çıktısının ekran görüntüsü](./media/azure-log-integration-get-started/loaded-modules.png)
3. Sonra, Azure günlük tümleştirmesini belirli bir Azure ortamını kullanacak şekilde yapılandırın. *Azure ortamı* , birlikte çalışmak istediğiniz Azure bulut veri merkezi türüdür. Şu anda birkaç Azure ortamı olsa da, ilgili seçenekler **Azurecyüksek** veya **AzureUSGovernment**. PowerShell 'i yönetici olarak çalıştırarak, C:\Program Files\Microsoft Azure log ıntegration\yolunda olduğunuzdan emin olun. Ardından şu komutu çalıştırın:

   `Set-AzlogAzureEnvironment -Name AzureCloud`( **Azurecyüksek**)
  
   ABD kamu Azure bulutunu kullanmak istiyorsanız **-Name** değişkeni için **AzureUSGovernment** kullanın. Şu anda diğer Azure bulutları desteklenmez.  

   > [!NOTE]
   > Komut başarılı olduğunda geri bildirim almazsınız. 

4. Bir sistemi izleyebilmeniz için önce Azure Tanılama için kullanılan depolama hesabının adı gerekir. Azure portal, **sanal makineler**' e gidin. İzlenecek bir Windows sanal makinesi bulun. **Özellikler** bölümünde **Tanılama ayarları**' nı seçin.  Sonra **Aracı**' ı seçin. Belirtilen depolama hesabı adını unutmayın. Daha sonraki bir adım için bu hesap adına ihtiyacınız vardır.

   ![Azure Tanılama ayarları bölmesinin ekran görüntüsü](./media/azure-log-integration-get-started/storage-account-large.png) 

   ![Konuk düzeyinde izlemeyi etkinleştir düğmesinin ekran görüntüsü](./media/azure-log-integration-get-started/azure-monitoring-not-enabled-large.png)

   > [!NOTE]
   > Sanal makine oluşturulduğunda izleme etkinleştirilmediyse, önceki görüntüde gösterildiği gibi etkinleştirebilirsiniz.

5. Şimdi Azure günlük tümleştirme makinesine geri dönün. Azure günlük tümleştirmesi 'ni yüklediğiniz sistemden depolama hesabıyla bağlantı olduğunu doğrulayın. Azure günlük tümleştirme hizmetini çalıştıran bilgisayarın, izlenen sistemlerin her birinde Azure Tanılama tarafından günlüğe kaydedilen bilgileri almak için depolama hesabına erişmesi gerekir. Bağlantıyı doğrulamak için: 
   1. [Azure Depolama Gezgini indirin](https://storageexplorer.com/).
   2. Kurulumu tamamladıktan sonra.
   3. Yükleme tamamlandığında **İleri**' yi seçin. **Başlatma Microsoft Azure Depolama Gezgini** onay kutusunu seçili bırakın.  
   4. Azure'da oturum açın.
   5. Azure Tanılama için yapılandırdığınız depolama hesabını görebildiğinizi doğrulayın: 

   ![Depolama Gezgini 'de depolama hesaplarının ekran görüntüsü](./media/azure-log-integration-get-started/storage-explorer.png)

   1. Depolama hesapları altında birkaç seçenek görüntülenir. **Tablolar**' ın altında **Wadwindowseventlogstable**adlı bir tablo görmeniz gerekir.

   Sanal makine oluşturulduğunda izleme etkinleştirilmediyse, daha önce açıklandığı gibi etkinleştirebilirsiniz.


## <a name="integrate-windows-vm-logs"></a>Windows VM günlüklerini tümleştirme

Bu adımda, Azure günlük tümleştirme hizmetini çalıştıran makineyi, günlük dosyalarını içeren depolama hesabına bağlanacak şekilde yapılandırırsınız.

Bu adımı gerçekleştirmek için birkaç şeyin olması gerekir:  
* **Yaforynameforsource**: Azure Tanılama 'tan bilgi depolamak için sanal makine için yapılandırdığınız depolama hesabına uygulayabileceğiniz kolay bir addır.
* **StorageAccountName**: Azure Tanılama yapılandırdığınızda belirttiğiniz depolama hesabının adı.  
* **StorageKey**: Azure Tanılama bilgisinin bu sanal makine için depolandığı depolama hesabının depolama anahtarı.  

Depolama anahtarını almak için aşağıdaki adımları izleyin:
1. [Azure Portal](https://portal.azure.com) gidin.
2. Gezinti bölmesinde **tüm hizmetler**' i seçin.
3. **Filtre** kutusuna **depolama**girin. Sonra **depolama hesapları**' nı seçin.

   ![Tüm hizmetlerde depolama hesaplarını gösteren ekran görüntüsü](./media/azure-log-integration-get-started/filter.png)

4. Depolama hesaplarının bir listesi görüntülenir. Günlük depolama alanına atadığınız hesaba çift tıklayın.

   ![Depolama hesaplarının bir listesini gösteren ekran görüntüsü](./media/azure-log-integration-get-started/storage-accounts.png)

5. **Ayarlar** altında **Erişim anahtarları**'nı seçin.

   ![Menüdeki erişim tuşları seçeneğini gösteren ekran görüntüsü](./media/azure-log-integration-get-started/storage-account-access-keys.png)

6. **KEY1**kopyalayın ve ardından aşağıdaki adım için erişebileceğiniz güvenli bir konuma kaydedin.
7. Azure günlük tümleştirmesini yüklediğiniz sunucuda, yönetici olarak bir komut Istemi penceresi açın. (Bir komut Istemi penceresini, PowerShell değil, yönetici olarak da seçtiğinizden emin olun).
8. C:\Program Files\Microsoft Azure günlük tümleştirmesi ' ne gidin.
9. Şu komutu çalıştırın: `Azlog source add <FriendlyNameForTheSource> WAD <StorageAccountName> <StorageKey>`.
 
   Örnek:
  
   `Azlog source add Azlogtest WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==`

   Abonelik KIMLIĞININ olay XML dosyasında gösterilmesini istiyorsanız, abonelik KIMLIĞINI kolay ada ekleyin:

   `Azlog source add <FriendlyNameForTheSource>.<SubscriptionID> WAD <StorageAccountName> <StorageKey>`
  
   Örnek:
  
   `Azlog source add Azlogtest.YourSubscriptionID WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==`

> [!NOTE]
> 60 dakikaya kadar bekleyip depolama hesabından çekilen olayları görüntüleyin. Azure günlük tümleştirmesi ' nde olayları görüntülemek için **Olay Görüntüleyicisi** > **Windows** > tarafından**iletilen olayları**günlüğe kaydet ' i seçin.

Aşağıdaki videoda önceki adımlar ele alınmaktadır:<br /><br />

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Enable-Diagnostics-and-Storage/player]


## <a name="if-data-isnt-showing-up-in-the-forwarded-events-folder"></a>Veriler Iletilen olaylar klasöründe görüntülenmiyorsa
Veriler bir saatten sonra Iletilen olaylar klasöründe gösterilmiyorsa, şu adımları uygulayın:

1. Azure günlük tümleştirme hizmetini çalıştıran makineyi kontrol edin. Azure 'un erişebileceği doğrulayın. Bağlantıyı test etmek için, bir tarayıcıda [Azure Portal](https://portal.azure.com)gitmeye çalışın.
2. Kullanıcı hesabı Azlog dosyası, Users\azlog klasörü için yazma iznine sahip olduğundan emin olun.
   1. Dosya Gezgini'ni açın.
   2. C:\Users. adresine gidin.
   3. C:\users\azlog öğesine sağ tıklayın
   4. **Güvenlik**' i seçin.
   5. **NT Service\Azlog**öğesini seçin. Hesap için izinleri denetleyin. Bu sekmede hesap eksikse veya uygun izinler gösterilmiyorsa, bu sekmede hesap izinlerini verebilirsiniz.
3. Komutunu `Azlog source list`çalıştırdığınızda, komutta `Azlog source add` eklenen depolama hesabının çıktıda listelendiğinden emin olun.
4. Azure günlük tümleştirme hizmeti 'nden herhangi bir hatanın raporlanmasında olup olmadığını görmek için **Olay Görüntüleyicisi** > **Windows günlükleri** > **uygulaması**' na gidin.

Yükleme ve yapılandırma sırasında herhangi bir sorunla karşılaşırsanız, bir [destek isteği](../../azure-supportability/how-to-create-azure-support-request.md)oluşturabilirsiniz. Hizmet için **günlük tümleştirmesi**' ni seçin.

Başka bir destek seçeneği de [Azure günlük TÜMLEŞTIRMESI MSDN Forumu](https://social.msdn.microsoft.com/Forums/home?forum=AzureLogIntegration)' dur. MSDN forumundaki topluluk, soruları yanıtlayarak ve Azure günlük tümleştirmesinden en iyi şekilde yararlanmak için ipuçları ve püf noktaları paylaşarak destek verebilir. Azure günlük tümleştirme ekibi de bu forumu izler. Her zaman yardımcı olurlar.

## <a name="integrate-azure-activity-logs"></a>Azure etkinlik günlüklerini tümleştirme

Azure etkinlik günlüğü oluşan Azure'da abonelik düzeyindeki olayların bir anlayış sağlayan bir abonelik günlüktür. Bu, Azure Resource Manager işlemsel verilerden hizmet durumu olaylarında güncelleştirmelere kadar olan bir veri aralığı içerir. Azure Güvenlik Merkezi uyarıları Bu günlüğe de dahildir.
> [!NOTE]
> Bu makaledeki adımları denemeden önce, [kullanmaya başlama](azure-log-integration-get-started.md) makalesini gözden geçirmeniz ve burada bulunan adımları gerçekleştirmeniz gerekir.

### <a name="steps-to-integrate-azure-activity-logs"></a>Azure etkinlik günlüklerini tümleştirme adımları

1. Komut istemi 'ni açın ve şu komutu çalıştırın:```cd c:\Program Files\Microsoft Azure Log Integration```
2. Şu komutu çalıştırın:```azlog createazureid```

    Bu komut sizden Azure oturum açmanızı ister. Bu komut daha sonra Azure AD kiracılarında, oturum açmış kullanıcının yönetici, ortak yönetici veya sahip olduğu Azure aboneliklerini barındıran bir Azure Active Directory hizmet sorumlusu oluşturur. Oturum açan kullanıcı yalnızca Azure AD kiracısındaki bir Konuk Kullanıcı ise, komut başarısız olur. Azure kimlik doğrulaması, Azure AD aracılığıyla yapılır. Azure günlük tümleştirmesi için bir hizmet sorumlusu oluşturmak, Azure aboneliklerinden okumak üzere erişim verilen Azure AD kimliğini oluşturur.
3. Önceki adımda oluşturulan Azure günlük tümleştirme hizmeti sorumlusunu, abonelik için etkinlik günlüğünü oku öğesine yetkilendirmek üzere aşağıdaki komutu çalıştırın. Komutu çalıştırmak için abonelikte bir sahip olmanız gerekir.

   ```Azlog.exe authorize subscriptionId```Örneğinde

   ```AZLOG.exe authorize ba2c2367-d24b-4a32-17b5-4443234859```

4. Azure Active Directory Denetim günlüğü JSON dosyalarının oluşturulduğunu onaylamak için aşağıdaki klasörleri denetleyin:
   - C:\Users\azlog\AzureResourceManagerJson
   - C:\Users\azlog\AzureResourceManagerJsonLD

> [!NOTE]
> JSON dosyalarındaki bilgileri güvenlik bilgileri ve olay yönetimi (SıEM) sisteminize getirme hakkında belirli yönergeler için SıEM satıcınıza başvurun.

Topluluk Yardımı [Azure günlük TÜMLEŞTIRMESI MSDN Forumu](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration)aracılığıyla kullanılabilir. Bu forum, Azure günlük tümleştirme topluluğundaki kişilerin, soruları, yanıtları, ipuçları ve püf noktalarını kullanarak birbirini desteklemesini sağlar. Ayrıca, Azure günlük tümleştirme ekibi bu forumu izler ve her zaman yardımcı olur.

Ayrıca, bir [destek isteği](../../azure-supportability/how-to-create-azure-support-request.md)açabilirsiniz. Destek istediğiniz hizmet olarak günlük tümleştirmesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

Azure günlük tümleştirmesi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın: Bu makaledeki adımları denemeden önce, kullanmaya başlama makalesini gözden geçirmeniz ve burada bulunan adımları gerçekleştirmeniz gerekir.

* [Azure günlük tümleştirmesine giriş](azure-log-integration-overview.md). Bu makalede Azure günlük tümleştirmesi, temel özellikleri ve nasıl çalıştığı açıklanır.
* [Iş ortağı yapılandırma adımları](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/). Bu blog gönderisi, Azure günlük tümleştirmesinin, iş ortağı çözümleri ile birlikte çalışmak üzere nasıl yapılandırılacağını gösterir. SıEM bileşenlerini yapılandırma hakkında geçerli kılavuzumuzu açıklar. Ek ayrıntılar için SıEM satıcınızla görüşün.
* [Azure günlük tümleştirmesi hakkında sık sorulan sorular (SSS)](azure-log-integration-faq.md). Bu SSS, Azure günlük tümleştirmesi hakkında sık sorulan soruları yanıtlar.
* Azure [Güvenlik Merkezi uyarılarını Azure günlük tümleştirmesi Ile tümleştirme](/azure/security-center/security-center-integrating-alerts-with-log-integration). Bu makalede, Azure Tanılama ve Azure etkinlik günlükleri tarafından toplanan Güvenlik Merkezi uyarılarını ve sanal makine güvenlik olaylarını nasıl eşitleyecağınızı gösterilmektedir. Günlükleri Azure Izleyici günlüklerinizi veya SıEM çözümünüzü kullanarak eşitlersiniz.
* [Azure tanılama ve Azure denetim günlükleri Için yeni özellikler](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/). Bu blog gönderisi sizi Azure denetim günlükleri ve Azure kaynaklarınızın işlemleriyle ilgili Öngörüler almanıza yardımcı olabilecek diğer özellikleri tanıtır.
