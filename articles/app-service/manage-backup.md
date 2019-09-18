---
title: Uygulama yedekleme-Azure App Service
description: Azure App Service ' de uygulamalarınızın yedeklerini oluşturmayı öğrenin.
services: app-service
documentationcenter: ''
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: 6223b6bd-84ec-48df-943f-461d84605694
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 8784a06306f59015b95293d90ff5509dcfcae045
ms.sourcegitcommit: ca359c0c2dd7a0229f73ba11a690e3384d198f40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71057929"
---
# <a name="back-up-your-app-in-azure"></a>Uygulamanızı Azure’a yedekleme
[Azure App Service](overview.md) yedekleme ve geri yükleme özelliği, uygulama yedeklemelerini el ile veya bir zamanlamaya göre kolayca oluşturmanızı sağlar.  Yedeklemeler, sınırsız bir zaman miktarına kadar tutulacak şekilde yapılandırılabilir. Mevcut uygulamanın üzerine yazarak veya başka bir uygulamaya geri yükleyerek uygulamayı önceki bir anlık görüntüye geri yükleyebilirsiniz.

Bir uygulamayı yedekten geri yükleme hakkında bilgi için bkz. [Azure 'da bir uygulamayı geri yükleme](web-sites-restore.md).

<a name="whatsbackedup"></a>

## <a name="what-gets-backed-up"></a>Yedeklenecek
App Service, uygulamanızı kullanacak şekilde yapılandırdığınız bir Azure depolama hesabına ve kapsayıcısına aşağıdaki bilgileri yedekleyebilir. 

* Uygulama yapılandırması
* Dosya içeriği
* Uygulamanıza bağlı veritabanı

Aşağıdaki veritabanı çözümleri yedekleme özelliği ile desteklenir: 
   - [SQL Veritabanı](https://azure.microsoft.com/services/sql-database/)
   - [MySQL için Azure Veritabanı](https://azure.microsoft.com/services/mysql)
   - [PostgreSQL için Azure Veritabanı](https://azure.microsoft.com/services/postgresql)
   - [uygulama içi MySQL](https://azure.microsoft.com/en-us/blog/mysql-in-app-preview-app-service/)
 

> [!NOTE]
>  Her yedekleme, artımlı güncelleştirme değil, uygulamanızın tamamen çevrimdışı bir kopyasıdır.
>  

<a name="requirements"></a>

## <a name="requirements-and-restrictions"></a>Gereksinimler ve kısıtlamalar
* Yedekleme ve geri yükleme özelliği, App Service planının **Standart** katmanda veya **Premium** katmanda olmasını gerektirir. App Service planınızı daha yüksek bir katman kullanmak üzere ölçeklendirme hakkında daha fazla bilgi için bkz. [Azure 'da bir uygulamayı ölçeklendirme](manage-scale-up.md).  
  **Premium** katman, **Standart** katmandan daha fazla sayıda günlük arka UPS sağlar.
* Yedeklemek istediğiniz uygulamayla aynı abonelikte bir Azure depolama hesabına ve kapsayıcısına ihtiyacınız vardır. Azure depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](https://docs.microsoft.com/azure/storage/common/storage-account-overview).
* Yedeklemeler 10 GB 'a kadar uygulama ve veritabanı içeriği olabilir. Yedekleme boyutu bu sınırı aşarsa bir hata alırsınız.
* SSL için MySQL için Azure veritabanı yedeklemeleri desteklenmez. Bir yedekleme yapılandırılmışsa, başarısız yedeklemeler alırsınız.
* PostgreSQL için SSL etkin Azure veritabanı yedeklemeleri desteklenmez. Bir yedekleme yapılandırılmışsa, başarısız yedeklemeler alırsınız.
* Uygulama içi MySQL veritabanları, herhangi bir yapılandırma olmadan otomatik olarak yedeklenir. Uygulama içi MySQL veritabanları için bağlantı dizeleri ekleme gibi el ile ayarları yaparsanız yedeklemeler düzgün çalışmayabilir.
* Yedeklemeleriniz için hedef olarak Güvenlik Duvarı etkin bir depolama hesabı kullanılması desteklenmez. Bir yedekleme yapılandırılmışsa, başarısız yedeklemeler alırsınız.


<a name="manualbackup"></a>

## <a name="create-a-manual-backup"></a>El ile yedekleme oluşturun
1. [Azure Portal](https://portal.azure.com)uygulamanızın sayfasına gidin, **yedeklemeler**' i seçin. **Yedeklemeler** sayfası görüntülenir.
   
    ![Yedeklemeler sayfası][ChooseBackupsPage]
   
   > [!NOTE]
   > Aşağıdaki iletiyi görürseniz, yedeklemelere devam edebilmeniz için App Service planınızı yükseltmek üzere tıklayın.
   > Daha fazla bilgi için bkz. [Azure 'da bir uygulamayı ölçeklendirme](manage-scale-up.md).  
   > ![Depolama hesabı seçin](./media/web-sites-backup/01UpgradePlan1.png)
   > 
   > 

2. **Yedekleme** sayfasında, **Yapılandır**
![' a tıklayın.](./media/web-sites-backup/ClickConfigure1.png)
3. **Yedekleme yapılandırması** sayfasında, depolama ' ya **tıklayın. Bir depolama** hesabı yapılandırmak için yapılandırılmadı.
   
    ![Depolama hesabı seç][ChooseStorageAccount]
4. Bir **depolama hesabı** ve **kapsayıcı**seçerek yedekleme hedefini seçin. Depolama hesabı, yedeklemek istediğiniz uygulamayla aynı aboneliğe ait olmalıdır. İsterseniz, ilgili sayfalarda yeni bir depolama hesabı veya yeni bir kapsayıcı oluşturabilirsiniz. İşiniz bittiğinde **Seç**' e tıklayın.
   
    ![Depolama hesabı seç](./media/web-sites-backup/02ChooseStorageAccount1-1.png)
5. Hala açık olan **yedekleme yapılandırması** sayfasında, **yedekleme veritabanını**yapılandırabilir ve ardından yedeklemelere eklemek istediğiniz veritabanlarını (SQL veritabanı veya MySQL) seçip **Tamam**' a tıklayın.  
   
    ![Depolama hesabı seç](./media/web-sites-backup/03ConfigureDatabase1.png)
   
   > [!NOTE]
   > Bu listede bir veritabanının görünmesi için, uygulamanızın **uygulama ayarları** sayfasının **bağlantı dizeleri** bölümünde bağlantı dizesinin bulunması gerekir. 
   >
   > Uygulama içi MySQL veritabanları, herhangi bir yapılandırma olmadan otomatik olarak yedeklenir. Uygulama içi MySQL veritabanları için bağlantı dizeleri ekleme gibi el ile ayarları yaparsanız yedeklemeler düzgün çalışmayabilir.
   > 
   > 
6. **Yedekleme yapılandırması** sayfasında **Kaydet**' e tıklayın.    
7. **Yedeklemeler** sayfasında, **Yedekle**' ye tıklayın.
   
    ![Şimdi Backupbutton][BackUpNow]
   
    Yedekleme işlemi sırasında bir ilerleme durumu iletisi görürsünüz.

Depolama hesabı ve kapsayıcısı yapılandırıldıktan sonra dilediğiniz zaman el ile yedekleme başlatabilirsiniz.  

<a name="automatedbackups"></a>

## <a name="configure-automated-backups"></a>Otomatik yedeklemeleri yapılandırma
1. **Yedekleme yapılandırması** sayfasında, **Zamanlanmış yedeklemeyi** **Açık**olarak ayarlayın. 
   
    ![Depolama hesabı seç](./media/web-sites-backup/05ScheduleBackup1.png)
2. Yedekleme zamanlaması seçenekleri görünür, **Zamanlanmış yedeklemeyi** **Açık**olarak ayarlar, ardından Yedekleme zamanlamasını Istediğiniz gibi yapılandırır ve **Tamam**' a tıklayın.
   
    ![Otomatik yedeklemeleri etkinleştir][SetAutomatedBackupOn]

<a name="partialbackups"></a>

## <a name="configure-partial-backups"></a>Kısmi yedeklemeleri yapılandırma
Bazen uygulamanızdaki her şeyi yedeklemek istemezsiniz. İşte birkaç örnek:

* Uygulamanızda, eski blog gönderileri veya görüntüleri gibi hiçbir değişiklik olmayan statik içerik içeren [haftalık yedeklemeler ayarlarsınız](#configure-automated-backups) .
* Uygulamanız 10 GB 'lık içeriğe sahiptir (bir seferde yedekleyebileceğiniz maksimum miktar).
* Günlük dosyalarını yedeklemek istemezsiniz.

Kısmi yedeklemeler, tam olarak yedeklemek istediğiniz dosyaları seçmenize olanak sağlar.

> [!NOTE]
> Yedekteki ayrı veritabanları 4GB'A fazla olabilir, ancak yedeklemenin toplam en büyük boyutu 10 GB 'dir

### <a name="exclude-files-from-your-backup"></a>Dosyaları yedeklemeinizden hariç tut
Bir kez yedekleme ve değişiklik yapamamayan günlük dosyalarını ve statik görüntüleri içeren bir uygulamanız olduğunu varsayalım. Böyle durumlarda, bu klasörleri ve dosyaları gelecekteki yedeklemelerde depolanmak üzere dışlayabilirsiniz. Yedeklemelerinizden dosya ve klasörleri dışlamak için uygulamanızın `_backup.filter` `D:\home\site\wwwroot` klasöründe bir dosya oluşturun. Bu dosyada dışlamak istediğiniz dosya ve klasörlerin listesini belirtin. 

Dosyalarınıza erişmenin kolay bir yolu kudu kullanmaktır. Web uygulamanızın kudu 'ye erişmesi için **Gelişmiş Araçlar-> go** ayarı seçeneğine tıklayın.

![Portalı kullanarak kudu][kudu-portal]

Yedeklemelerinizden dışlamak istediğiniz klasörleri belirler.  Örneğin, vurgulanan klasör ve dosyaları filtrelemek istiyorsunuz.

![Görüntüler klasörü][ImagesFolder]

Adlı `_backup.filter` bir dosya oluşturun ve önceki listeyi dosyaya yerleştirin, ancak kaldırın `D:\home`. Satır başına bir dizin veya dosya listeleyin. Bu nedenle, dosyanın içeriği şu olmalıdır:
 ```bash
    \site\wwwroot\Images\brand.png
    \site\wwwroot\Images\2014
    \site\wwwroot\Images\2013
```

`_backup.filter` [](deploy-ftp.md) FTP`D:\home\site\wwwroot\` 'yi veya başka bir yöntemi kullanarak sitenizin dizinine dosyayı yükleyin. İsterseniz, kudu `DebugConsole` kullanarak dosyayı doğrudan oluşturabilir ve buraya içerik ekleyebilirsiniz.

Yedeklemeleri, normalde [el ile](#create-a-manual-backup) veya [otomatik olarak](#configure-automated-backups)yaptığınız gibi çalıştırın. Şimdi, içinde `_backup.filter` belirtilen tüm dosyalar ve klasörler, zamanlanan veya el ile başlatılan yedeklemelerin dışında tutulur. 

> [!NOTE]
> Sitenizin kısmi yedeklerini [düzenli bir yedeklemeyi geri](web-sites-restore.md)yüklediğiniz şekilde geri yükleyebilirsiniz. Geri yükleme işlemi doğru şeyi yapar.
> 
> Tam yedekleme geri yüklendiğinde, sitedeki tüm içerikler, yedeklemedeki herhangi bir şey ile değiştirilmiştir. Bir dosya sitede yer alıyorsa, ancak yedeklemede silinir. Ancak kısmi bir yedekleme geri yüklendiğinde, kara listelenen dizinlerden birinde bulunan içerik ya da kara listelenen herhangi bir dosya olduğu gibi kalır.
> 


<a name="aboutbackups"></a>

## <a name="how-backups-are-stored"></a>Yedeklemeler nasıl depolanır
Uygulamanız için bir veya daha fazla yedekleme yaptıktan sonra yedeklemeler, depolama hesabınızın **kapsayıcılar** sayfasında ve uygulamanızda görünür. Depolama hesabında, her yedekleme, yedekleme verilerini`.zip` `.xml` ve `.zip` dosya içeriklerinin bildirimini içeren bir dosyayı içeren bir dosyadan oluşur. Uygulama geri yükleme işlemi yapmadan yedeklemelerinize erişmek istiyorsanız bu dosyaları açabilir ve bunlara gözatamazsınız.

Uygulamanın veritabanı yedeklemesi,. zip dosyasının kökünde saklanır. Bir SQL veritabanı için bu bir BACPAC dosyasıdır (dosya uzantısı yoktur) ve içeri aktarılabilir. BACPAC dışarı aktarmaya dayalı bir SQL veritabanı oluşturmak için, bkz. [Yeni bir kullanıcı veritabanı oluşturmak IÇIN bacpac dosyasını Içeri aktarma](https://technet.microsoft.com/library/hh710052.aspx).

> [!WARNING]
> **Websitebackups** kabınızda herhangi bir dosyanın değiştirilmesi yedeklemenin geçersiz olmasına ve bu nedenle geri yüklenebilir olmasına neden olabilir.
> 
> 

## <a name="automate-with-scripts"></a>Betiklerle otomatikleştirme

[Azure CLI](/cli/azure/install-azure-cli) veya [Azure PowerShell](/powershell/azure/overview)kullanarak yedekleme yönetimini betiklerle otomatik hale getirebilirsiniz.

Örnekler için bkz.:

- [Azure CLI örnekleri](samples-cli.md)
- [Azure PowerShell örnekleri](samples-powershell.md)

<a name="nextsteps"></a>

## <a name="next-steps"></a>Sonraki Adımlar
Bir uygulamayı yedekten geri yükleme hakkında bilgi için bkz. [Azure 'da bir uygulamayı geri yükleme](web-sites-restore.md). 


<!-- IMAGES -->
[ChooseBackupsPage]: ./media/web-sites-backup/01ChooseBackupsPage1.png
[ChooseStorageAccount]: ./media/web-sites-backup/02ChooseStorageAccount-1.png
[BackUpNow]: ./media/web-sites-backup/04BackUpNow1.png
[SetAutomatedBackupOn]: ./media/web-sites-backup/06SetAutomatedBackupOn1.png
[SaveIcon]: ./media/web-sites-backup/10SaveIcon.png
[ImagesFolder]: ./media/web-sites-backup/11Images.png
[LogsFolder]: ./media/web-sites-backup/12Logs.png
[GhostUpgradeWarning]: ./media/web-sites-backup/13GhostUpgradeWarning.png
[kudu-portal]:./media/web-sites-backup/kudu-portal.PNG

