---
title: Uygulamayı yedeklemeden geri yükleme
description: Uygulamanızı bir yedeklemeden nasıl geri yükleyin öğrenin. Belirli bağlantılı veritabanları tek bir işlemde uygulamayla birlikte geri yüklenebilir.
ms.assetid: 4444dbf7-363c-47e2-b24a-dbd45cb08491
ms.topic: article
ms.date: 07/06/2016
ms.custom: seodec18
ms.openlocfilehash: 1295080d0eec7a4e88029cdadd85863f5f40d034
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74689239"
---
# <a name="restore-an-app-in-azure"></a>Uygulamanızı Azure’a geri yükleme
Bu makalede, [Azure Uygulama Hizmeti'nde](../app-service/overview.md) daha önce yedeklediğiniz bir uygulamayı nasıl geri yükleyebilirsiniz (bkz. [uygulamanızı Azure'da yedekle).](manage-backup.md) İsteğe bağlı bağlantılı veritabanlarıyla uygulamanızı önceki bir duruma geri yükleyebilir veya orijinal uygulamanızın yedeklerinden birine dayalı yeni bir uygulama oluşturabilirsiniz. Azure Uygulama Hizmeti yedekleme ve geri yükleme için aşağıdaki veritabanlarını destekler:
- [SQL Database](https://azure.microsoft.com/services/sql-database/)
- [MySQL için Azure Veritabanı](https://azure.microsoft.com/services/mysql)
- [PostgreSQL için Azure Veritabanı](https://azure.microsoft.com/services/postgresql)
- [MySQL uygulama içi](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)

**Yedeklemelerden** geri ödeme, Standart ve **Premium** katmanında çalışan uygulamalar için kullanılabilir. Uygulamanızı ölçeklendirme hakkında bilgi için [Azure'da bir uygulamayı ölçeklendir'e](manage-scale-up.md)bakın. **Premium** katman, **Standart** katmandan daha fazla günlük yedekleme yapılmasına olanak tanır.

<a name="PreviousBackup"></a>

## <a name="restore-an-app-from-an-existing-backup"></a>Varolan bir yedeklemeden uygulamayı geri yükleme
1. Azure portalında uygulamanızın **Ayarlar** sayfasında Yedekler sayfasını görüntülemek için **Yedekler'i** tıklatın. **Backups** Sonra **Geri Yükle'yi**tıklatın.
   
    ![Şimdi geri yüklemeyi seçin][ChooseRestoreNow]
2. Geri **Yükleme** sayfasında, önce yedek kaynağı seçin.
   
    ![](./media/web-sites-restore/021ChooseSource1.png)
   
    **Uygulama yedekleme** seçeneği, geçerli uygulamanın mevcut tüm yedeklemelerini gösterir ve kolayca birini seçebilirsiniz.
    **Depolama** seçeneği, aboneliğinizdeki mevcut Azure Depolama hesabından ve kapsayıcısından herhangi bir yedek ZIP dosyasını seçmenize olanak tanır.
    Başka bir uygulamanın yedeğini geri yüklemeye çalışıyorsanız, **Depolama** seçeneğini kullanın.
3. Ardından, **geri yükleme hedefinde**uygulamanın hedefini belirleyin.
   
    ![](./media/web-sites-restore/022ChooseDestination1.png)
   
   > [!WARNING]
   > **Overwrite'ı**seçerseniz, geçerli uygulamanızdaki tüm varolan veriler silinir ve üzerine yazılır. **Tamam'ı**tıklatmadan önce, tam olarak ne yapmak istediğinizden emin olun.
   > 
   > 
   
   > [!WARNING]
   > Uygulama Hizmeti siz bunu geri alırken veritabanına veri yazıyorsa, BIRINCIL ANAHTAR ihlali ve veri kaybı gibi belirtilere neden olabilir. Veritabanını geri yüklemeye başlamadan önce Uygulama Hizmetini durdurmanız önerilir.
   > 
   > 
   
    Uygulama yedeklemesini aynı kaynak grubundaki başka bir uygulamaya geri yüklemek için **Varolan Uygulama'yı** seçebilirsiniz. Bu seçeneği kullanmadan önce, kaynak grubunuzda veritabanı yapılandırmasını uygulama yedeklemesinde tanımlanan uygulama yapılandırmasına yansıtan başka bir uygulama oluşturmuş olmalısınız. Ayrıca, içeriğinizi geri yüklemek için **yeni** bir uygulama oluşturabilirsiniz.

4. **Tamam**'a tıklayın.

<a name="StorageAccount"></a>

## <a name="download-or-delete-a-backup-from-a-storage-account"></a>Bir depolama hesabından yedekleme indirme veya silme
1. Azure portalının ana **Gözat** sayfasından **Depolama hesaplarını**seçin. Varolan depolama hesaplarınızın listesi görüntülenir.
2. İndirmek veya silmek istediğiniz yedeklemeyi içeren depolama hesabını seçin. Depolama hesabının sayfası görüntülenir.
3. Depolama hesabı sayfasında, istediğiniz kapsayıcıyı seçin
   
    ![Kapsayıcıları Görüntüle][ViewContainers]
4. İndirmek veya silmek istediğiniz yedekleme dosyasını seçin.
   
    ![Görünüm Konteynerleri](./media/web-sites-restore/03ViewFiles.png)
5. Ne yapmak istediğinize bağlı olarak **İndir** veya **Sil'i** tıklatın.  

<a name="OperationLogs"></a>

## <a name="monitor-a-restore-operation"></a>Geri yükleme işlemini izleme
Uygulama geri yükleme işleminin başarısı veya başarısızlığı yla ilgili ayrıntıları görmek için Azure portalındaki **Etkinlik Günlüğü** sayfasına gidin.  
 

İstenilen geri yükleme işlemini bulmak için aşağı kaydırın ve seçmek için tıklatın.

Ayrıntılar sayfası geri yükleme işlemiyle ilgili kullanılabilir bilgileri görüntüler.

## <a name="automate-with-scripts"></a>Betiklerle otomatikleştirme

[Azure CLI](/cli/azure/install-azure-cli) veya [Azure PowerShell'i](/powershell/azure/overview)kullanarak yedekleme yönetimini komut dosyalarıyla otomatikleştirebilirsiniz.

Örnekler için bkz:

- [Azure CLI örnekleri](samples-cli.md)
- [Azure PowerShell örnekleri](samples-powershell.md)

<!-- ## Next Steps
You can backup and restore App Service apps using REST API. -->


<!-- IMAGES -->
[ChooseRestoreNow]: ./media/web-sites-restore/02ChooseRestoreNow1.png
[ViewContainers]: ./media/web-sites-restore/03ViewContainers.png
