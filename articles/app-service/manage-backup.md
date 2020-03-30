---
title: Uygulama yedekleme
description: Azure Uygulama Hizmeti'nde uygulamalarınızın yedeklerini nasıl oluşturabilirsiniz öğrenin. El ile veya zamanlanmış yedeklemeleri çalıştırın. Ekli veritabanını ekleyerek yedeklemeleri özelleştirin.
ms.assetid: 6223b6bd-84ec-48df-943f-461d84605694
ms.topic: article
ms.date: 10/16/2019
ms.custom: seodec18
ms.openlocfilehash: 783737729601bfef3bee8741a097d4319349f18e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79259337"
---
# <a name="back-up-your-app-in-azure"></a>Uygulamanızı Azure’a yedekleme
[Azure Uygulama Hizmeti'ndeki](overview.md) Yedekleme ve Geri Yükleme özelliği, uygulama yedeklemelerini el ile veya zamanlamada kolayca oluşturmanıza olanak tanır. Yedeklemeleri belirsiz bir süreye kadar saklanacak şekilde yapılandırabilirsiniz. Varolan uygulamayı üzerine yazarak veya başka bir uygulamaya geri yükleyerek uygulamayı önceki bir durum görüntüsünü geri yükleyebilirsiniz.

Bir uygulamayı yedeklemeden geri yükleme hakkında daha fazla bilgi için [Azure'da bir uygulamayı geri yükle'ye](web-sites-restore.md)bakın.

<a name="whatsbackedup"></a>

## <a name="what-gets-backed-up"></a>Ne yedeklenir
Uygulama Hizmeti, aşağıdaki bilgileri kullanmak üzere uygulamanızı yapılandırmak tasladığınız bir Azure depolama hesabına ve kapsayıcıya yedekleyebilir. 

* Uygulama yapılandırması
* Dosya içeriği
* Uygulamanıza bağlı veritabanı

Aşağıdaki veritabanı çözümleri yedekleme özelliği ile desteklenir: 

- [SQL Database](https://azure.microsoft.com/services/sql-database/)
- [MySQL için Azure Veritabanı](https://azure.microsoft.com/services/mysql)
- [PostgreSQL için Azure Veritabanı](https://azure.microsoft.com/services/postgresql)
- [MySQL uygulama içi](https://azure.microsoft.com/blog/mysql-in-app-preview-app-service/)
 

> [!NOTE]
> Her yedekleme, uygulamanızın tam bir çevrimdışı kopyasıdır, artımlı bir güncelleştirme değildir.
>

<a name="requirements"></a>

## <a name="requirements-and-restrictions"></a>Gereksinimler ve kısıtlamalar
* Yedekleme ve Geri Yükleme özelliği, Uygulama Hizmeti planının **Standart** katmanda veya **Premium** katmanda olmasını gerektirir. Daha yüksek bir katman kullanmak için Uygulama Hizmeti planınızı ölçeklendirme hakkında daha fazla bilgi için [azure'da bir uygulamayı ölçeklendir'e](manage-scale-up.md)bakın. **Premium** katman, **Standart** katmandan daha fazla günlük yedekleme sağlar.
* Yedeklemek istediğiniz uygulamayla aynı abonelikte bir Azure depolama hesabına ve kapsayıcıya ihtiyacınız var. Azure depolama hesapları hakkında daha fazla bilgi için Azure [depolama hesabına genel bakış'a](https://docs.microsoft.com/azure/storage/common/storage-account-overview)bakın.
* Yedeklemeler en fazla 10 GB uygulama ve veritabanı içeriği olabilir. Yedekleme boyutu bu sınırı aşarsa, bir hata alırsınız.
* MySQL için SSL etkin Azure Veritabanı yedeklemeleri desteklenmez. Bir yedekleme yapılandırılırsa, başarısız yedekleme alırsınız.
* PostgreSQL için SSL etkin Azure Veritabanı yedeklemeleri desteklenmez. Bir yedekleme yapılandırılırsa, başarısız yedekleme alırsınız.
* Uygulama içi MySQL veritabanları herhangi bir yapılandırma olmadan otomatik olarak yedeklenir. Bağlantı dizeleri ekleme gibi uygulama içi MySQL veritabanları için el ile ayarlar yaparsanız, yedeklemeler düzgün çalışmayabilir.
* Yedeklemeleriniz için hedef olarak güvenlik duvarı etkin bir depolama hesabı kullanmak desteklenmez. Bir yedekleme yapılandırılırsa, başarısız yedekleme alırsınız.


<a name="manualbackup"></a>

## <a name="create-a-manual-backup"></a>El ile yedekleme oluşturun
1. Azure [portalında](https://portal.azure.com)uygulamanızın sayfasına gidin, **Yedekler'i**seçin. **Yedeklemesayfası** görüntülenir.

    ![Yedekleme sayfası](./media/manage-backup/access-backup-page.png)

    > [!NOTE]
    > Aşağıdaki iletiyi görürseniz, yedeklemelere devam etmeden önce Uygulama Hizmeti planınızı yükseltmek için bu iletiyi tıklatın.
    > Daha fazla bilgi için [bkz.](manage-scale-up.md)
    > ![Depolama hesabı seçin](./media/manage-backup/upgrade-plan.png)
    > 
    > 

2. **Yedekleme** sayfasında, Yedekleme'yi seçin **yapılandırılmez. Uygulamanızın yedeklemesini yapılandırmak için burayı tıklatın.**

    ![Yapıla'yı tıklatın](./media/manage-backup/configure-start.png)

3. Yedekleme **Yapılandırması** sayfasında, depolama hesabını yapılandırmak için **yapılandırılmamış Depolama'yı** tıklatın.

    ![Depolama hesabı seçin](./media/manage-backup/configure-storage.png)

4. Bir **Depolama Hesabı** ve **Kapsayıcı**seçerek yedek hedefinizi seçin. Depolama hesabı, yedeklemek istediğiniz uygulamayla aynı aboneye ait olmalıdır. İsterseniz, ilgili sayfalarda yeni bir depolama hesabı veya yeni bir kapsayıcı oluşturabilirsiniz. Bittiğinde, **Seç'i**tıklatın.

5. Hala açık bırakılan **Yedekleme Yapılandırması** sayfasında Yedek **Veritabanı'nı**yapılandırabilir, ardından yedeklemelere (SQL veritabanı veya MySQL) eklemek istediğiniz veritabanlarını seçebilir ve **ardından Tamam'ı**tıklatın.

    ![Depolama hesabı seçin](./media/manage-backup/configure-database.png)

    > [!NOTE]
    > Bir veritabanının bu listede görünmesi için bağlantı dizesi uygulamanızın **Uygulama ayarları** sayfasının **Bağlantı dizeleri** bölümünde bulunması gerekir. 
    >
    > Uygulama içi MySQL veritabanları herhangi bir yapılandırma olmadan otomatik olarak yedeklenir. Bağlantı dizeleri ekleme gibi uygulama içi MySQL veritabanları için el ile ayarlar yaparsanız, yedeklemeler düzgün çalışmayabilir.
    > 
    > 

6. Yedekleme **Yapılandırması** sayfasında **Kaydet'i**tıklatın.
7. **Yedeklemeler** sayfasında **Yedek'i**tıklatın.

    ![BackUpNow düğmesi](./media/manage-backup/manual-backup.png)

    Yedekleme işlemi sırasında bir ilerleme iletisi görürsünüz.

Depolama hesabı ve kapsayıcı yapılandırıldıktan sonra, istediğiniz zaman el ile yedekleme başlatabilirsiniz.

<a name="automatedbackups"></a>

## <a name="configure-automated-backups"></a>Otomatik yedeklemeleri yapılandırma
1. Yedekleme **Yapılandırması** sayfasında, **Zamanlanmış yedeklemeyi** **A'ya**ayarlayın. 

    ![Otomatik yedeklemeleri etkinleştirme](./media/manage-backup/scheduled-backup.png)

2. Yedekleme zamanlamasını istediğiniz gibi yapılandırın ve **Tamam'ı**seçin.

<a name="partialbackups"></a>

## <a name="configure-partial-backups"></a>Kısmi Yedeklemeleri Yapılandırma
Bazen uygulamanızdaki her şeyi yedeklemek istemezsiniz. İşte birkaç örnek:

* Uygulamanızın eski blog gönderileri veya resimler gibi hiç değişmemiş statik içerik içeren [haftalık yedeklemelerini ayarlarsınız.](#configure-automated-backups)
* Uygulamanızda 10 GB'ın üzerinde içerik bulunur (bu, aynı anda yedekleyebileceğiniz maksimum tutardır).
* Günlük dosyalarını yedeklemek istemezsin.

Kısmi yedeklemeler, yedeklemek istediğiniz dosyaları tam olarak seçmenize olanak tanır.

> [!NOTE]
> Yedekleme bireysel veritabanları 4GB max olabilir ama yedekleme toplam maksimum boyutu 10GB

### <a name="exclude-files-from-your-backup"></a>Dosyaları yedeklemenizden hariç tutma
Günlük dosyalarıve bir kez yedek olan ve değişmeyecek statik görüntüler içeren bir uygulamanız olduğunu varsayalım. Bu gibi durumlarda, bu klasörlerin ve dosyaların gelecekteki yedeklemelerinizde depolanmalarını hariç tutabilirsiniz. Dosyaları ve klasörleri yedeklemelerinizden hariç `_backup.filter` tutmak için `D:\home\site\wwwroot` uygulamanızın klasöründe bir dosya oluşturun. Bu dosyada hariç tutmak istediğiniz dosya ve klasörlerin listesini belirtin. 

Dosyalarınıza ' a `https://<app-name>.scm.azurewebsites.net/DebugConsole`yön vererek erişebilirsiniz. İstenirse, Azure hesabınızda oturum açın.

Yedeklemelerinizden hariç tutmak istediğiniz klasörleri tanımlayın. Örneğin, vurgulanan klasörü ve dosyaları filtrelemek istiyorsunuz.

![Görüntüler Klasörü](./media/manage-backup/kudu-images.png)

Çağrılan `_backup.filter` bir dosya oluşturun ve önceki listeyi dosyaya koyun, ancak kaldırın. `D:\home` Satır başına bir dizin veya dosya listele. Yani dosyanın içeriği olmalıdır:

 ```
\site\wwwroot\Images\brand.png
\site\wwwroot\Images\2014
\site\wwwroot\Images\2013
```

[FtP](deploy-ftp.md) veya `D:\home\site\wwwroot\` başka bir yöntemi kullanarak dosyayı sitenizin dizinine yükleyin. `_backup.filter` İsterseniz, doğrudan Kudu `DebugConsole` kullanarak dosyayı oluşturabilir ve içeriği oraya ekleyebilirsiniz.

Yedeklemeleri, normalde yaptığınız gibi, el [ile](#create-a-manual-backup) veya [otomatik olarak](#configure-automated-backups)çalıştırın. Şimdi, belirtilen tüm dosya ve `_backup.filter` klasörler zamanlanan veya el ile başlatılan gelecekteki yedeklemeler dışında tutulur. 

> [!NOTE]
> Sitenizin kısmi yedeklemelerini, normal bir [yedeklemeyi geri yüklediğiniz](web-sites-restore.md)gibi geri yükleyin. Geri yükleme işlemi doğru olanı yapar.
> 
> Tam yedekleme geri yüklendiğinde, sitedeki tüm içerik yedeklemede ne varsa onunla değiştirilir. Bir dosya sitedeyse, ancak yedeklemede değilse silinir. Ancak kısmi bir yedekleme geri yüklendiğinde, kara listeye alınan dizinlerden birinde veya kara listeye alınan herhangi bir dosyada bulunan herhangi bir içerik olduğu gibi bırakılır.
> 


<a name="aboutbackups"></a>

## <a name="how-backups-are-stored"></a>Yedeklemeler nasıl depolanır?
Uygulamanız için bir veya daha fazla yedekleme yaptıktan sonra, yedeklemeler depolama hesabınızın **Kapsayıcılar** sayfasında ve uygulamanızda görünür. Depolama hesabında, her yedekleme yedekleme`.zip` verilerini içeren bir `.xml` dosya ve dosya içeriğinin `.zip` bir bildirimini içeren bir dosyadan oluşur. Bir uygulama geri yüklemesi gerçekleştirmeden yedeklemelerinize erişmek istiyorsanız, bu dosyaların fermuarını açabilir ve göz atabilirsiniz.

Uygulamanın veritabanı yedeklemesi .zip dosyasının kökünde depolanır. Bir SQL veritabanı için bu bir BACPAC dosyasıdır (dosya uzantısı yoktur) ve içe aktarılabilir. BACPAC dışa aktarımını temel alan bir SQL veritabanı oluşturmak [için](https://technet.microsoft.com/library/hh710052.aspx)bkz.

> [!WARNING]
> **Web sitesi yedekleme** kapsayıcınızdaki dosyalardan herhangi birini değiştirmek, yedeklemenin geçersiz olmasına ve bu nedenle geri ödenmez olmasına neden olabilir.
> 
> 

## <a name="automate-with-scripts"></a>Betiklerle otomatikleştirme

[Azure CLI](/cli/azure/install-azure-cli) veya [Azure PowerShell'i](/powershell/azure/overview)kullanarak yedekleme yönetimini komut dosyalarıyla otomatikleştirebilirsiniz.

Örnekler için bkz:

- [Azure CLI örnekleri](samples-cli.md)
- [Azure PowerShell örnekleri](samples-powershell.md)

<a name="nextsteps"></a>

## <a name="next-steps"></a>Sonraki Adımlar
Bir uygulamayı yedeklemeden geri yükleme hakkında bilgi için [Azure'da bir uygulamayı geri yükleme'ye](web-sites-restore.md)bakın. 
