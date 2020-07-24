---
title: Uygulamayı yedekten geri yükle
description: Uygulamanızı bir yedekten geri yüklemeyi öğrenin. Belirli bağlı veritabanları, tek bir işlemde uygulamayla birlikte geri yüklenebilir.
ms.assetid: 4444dbf7-363c-47e2-b24a-dbd45cb08491
ms.topic: article
ms.date: 07/06/2016
ms.custom: seodec18
ms.openlocfilehash: c1b0ce62905424032c2100a1a032fa43ba97578f
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87084866"
---
# <a name="restore-an-app-in-azure"></a>Uygulamanızı Azure’a geri yükleme
Bu makalede, daha önce yedeklediğiniz [Azure App Service](../app-service/overview.md) bir uygulamayı nasıl geri yükleyeceğiniz gösterilmektedir (bkz. [Azure 'da uygulamanızı yedekleme](manage-backup.md)). Uygulamanızı bağlı veritabanları ile önceki bir duruma geri yükleyebilir veya orijinal uygulamanızın yedeklemelerinden birini temel alan yeni bir uygulama oluşturabilirsiniz. Azure App Service yedekleme ve geri yükleme için aşağıdaki veritabanlarını destekler:
- [SQL Veritabanı](https://azure.microsoft.com/services/sql-database/)
- [MySQL için Azure Veritabanı](https://azure.microsoft.com/services/mysql)
- [PostgreSQL için Azure Veritabanı](https://azure.microsoft.com/services/postgresql)
- [uygulama içi MySQL](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)

Yedeklerden geri yükleme, **Standart** ve **Premium** katmanda çalışan uygulamalar tarafından kullanılabilir. Uygulamanızı ölçeklendirme hakkında daha fazla bilgi için bkz. [Azure 'da bir uygulamayı](manage-scale-up.md)ölçeklendirme. **Premium** katman, **Standart** katmandan daha fazla sayıda günlük yedeklemenin gerçekleştirilmesine izin verir.

<a name="PreviousBackup"></a>

## <a name="restore-an-app-from-an-existing-backup"></a>Mevcut bir yedekten uygulama geri yükleme
1. Azure portal uygulamanızın **Ayarlar** **sayfasında yedeklemeler sayfasını göstermek** için **yedeklemeler** ' e tıklayın. Sonra **geri yükle**' ye tıklayın.
   
    ![Şimdi geri yükle ' yi seçin][ChooseRestoreNow]
2. **Geri yükle** sayfasında, önce yedekleme kaynağını seçin.
   
    ![Yedekleme kaynağının nerede seçdiğinin gösterildiği ekran görüntüsü.](./media/web-sites-restore/021ChooseSource1.png)
   
    **Uygulama yedekleme** seçeneği, geçerli uygulamanın tüm mevcut yedeklemelerini gösterir ve kolayca bir seçim yapabilirsiniz.
    **Depolama** seçeneği, mevcut herhangi bir Azure depolama hesabı ve aboneliğinizdeki kapsayıcıdan herhangi BIR yedek ZIP dosyasını seçmenizi sağlar.
    Başka bir uygulamanın yedeklemesini geri yüklemeye çalışıyorsanız, **depolama** seçeneğini kullanın.
3. Ardından, **geri yükleme hedefinde**uygulama geri yükleme hedefini belirtin.
   
    ![Uygulama geri yüklemesi için hedefin nerede belirtildiğinin gösterildiği ekran görüntüsü.](./media/web-sites-restore/022ChooseDestination1.png)
   
   > [!WARNING]
   > **Üzerine yaz**' ı seçerseniz, geçerli uygulamanızdaki tüm mevcut veriler silinir ve üzerine yazılır. **Tamam**' a tıklamadan önce, tam olarak yapmak istediğiniz gibi olduğundan emin olun.
   > 
   > 
   
   > [!WARNING]
   > App Service, verileri geri yüklerken veritabanına veri yazıyor, bu durum BIRINCIL anahtar ve veri kaybını ihlal etmek gibi belirtilerin oluşmasına neden olabilir. Veritabanını geri yüklemeye başlamadan önce önce App Service durdurmanız önerilir.
   > 
   > 
   
    Aynı kaynak grubundaki başka bir uygulamaya uygulama yedeklemesini geri yüklemek için **var olan uygulamayı** seçebilirsiniz. Bu seçeneği kullanmadan önce, kaynak grubunuzda zaten uygulama yedeklemesiyle tanımlanmış olan yansıtma veritabanı yapılandırması ile başka bir uygulama oluşturmuş olmanız gerekir. İçeriğinizi uygulamasına geri yüklemek için **Yeni** bir uygulama da oluşturabilirsiniz.

4. **Tamam** düğmesine tıklayın.

<a name="StorageAccount"></a>

## <a name="download-or-delete-a-backup-from-a-storage-account"></a>Bir depolama hesabından yedek yükleme veya silme
1. Azure portal ana **tarayıcı** sayfasında, **depolama hesapları**' nı seçin. Mevcut depolama hesaplarınızın bir listesi görüntülenir.
2. İndirmek veya silmek istediğiniz yedeği içeren depolama hesabını seçin. Depolama hesabı sayfası görüntülenir.
3. Depolama hesabı sayfasında, istediğiniz kapsayıcıyı seçin
   
    ![Kapsayıcıları görüntüle][ViewContainers]
4. İndirmek veya silmek istediğiniz yedekleme dosyasını seçin.
   
    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)
5. Ne yapmak istediğinize bağlı olarak **İndir** veya **Sil** ' e tıklayın.  

<a name="OperationLogs"></a>

## <a name="monitor-a-restore-operation"></a>Geri yükleme işlemini izleme
Uygulama geri yükleme işleminin başarısı veya başarısızlığı hakkındaki ayrıntıları görmek için Azure portal **etkinlik günlüğü** sayfasına gidin.  
 

İstediğiniz geri yükleme işlemini bulmak için aşağı kaydırın ve seçmek için tıklayın.

Ayrıntılar sayfasında geri yükleme işlemiyle ilgili kullanılabilir bilgiler görüntülenir.

## <a name="automate-with-scripts"></a>Betiklerle otomatikleştirme

[Azure CLI](/cli/azure/install-azure-cli) veya [Azure PowerShell](/powershell/azure/)kullanarak yedekleme yönetimini betiklerle otomatik hale getirebilirsiniz.

Örnekler için bkz.:

- [Azure CLI örnekleri](samples-cli.md)
- [Azure PowerShell örnekleri](samples-powershell.md)

<!-- ## Next Steps
You can backup and restore App Service apps using REST API. -->


<!-- IMAGES -->
[ChooseRestoreNow]: ./media/web-sites-restore/02ChooseRestoreNow1.png
[ViewContainers]: ./media/web-sites-restore/03ViewContainers.png
