---
title: Azure Kopya Veri aracını kullanarak verileri kopyalama
description: Azure Blob depolama alanındaki bir konumda bulunan verileri başka bir konuma kopyalamak için bir Azure veri fabrikası oluşturun ve Veri Kopyalama aracını kullanın.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
manager: anandsub
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: quickstart
ms.date: 03/18/2020
ms.openlocfilehash: 8a05ae29d62a5afacbc0007850266688e989b62b
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81419434"
---
# <a name="quickstart-use-the-copy-data-tool-to-copy-data"></a>Hızlı başlatma: Verileri kopyalamak için Verileri Kopyalama aracını kullanma

> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Geçerli sürüm](quickstart-create-data-factory-copy-data-tool.md)

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Bu hızlı başlangıçta Azure portalını kullanarak bir veri fabrikası oluşturursunuz. Sonra, Veri Kopyalama aracını kullanarak Azure Blob depolama alanındaki bir klasörde bulunan verileri başka bir klasöre kopyalarsınız. 

> [!NOTE]
> Azure Data Factory'yi kullanmaya yeni başlıyorsanız, bu hızlı başlangıçtaki işlemleri gerçekleştirmeden önce [Azure Data Factory'ye giriş](data-factory-introduction.md) konusuna bakın. 

[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)] 

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. **Microsoft Edge** veya **Google Chrome** web tarayıcısını açın. Şu anda Data Factory kullanıcı arabirimi yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenmektedir.
1. [Azure portalına](https://portal.azure.com)gidin. 
1. Azure portalı menüsünden, bir > kaynak**Analitik** > **Veri Fabrikası** **Oluştur'u**seçin:

    ![Yeni veri fabrikası oluşturma](./media/doc-common-process/new-azure-data-factory-menu.png)

1. **Yeni veri fabrikası** sayfasında **Ad** için **ADFTutorialDataFactory** girin. 
 
   Azure veri fabrikasının adı *genel olarak benzersiz*olmalıdır. Aşağıdaki hatayı görürseniz, veri fabrikasının adını (örneğin, ** &lt;adınız&gt;ADFTutorialDataFactory)** değiştirin ve yeniden oluşturmayı deneyin. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - adlandırma kuralları](naming-rules.md) makalesini inceleyin.
  
   ![Bir ad kullanılamadığında alınan hata](./media/doc-common-process/name-not-available-error.png)
1. **Abonelik** için, veri fabrikasını oluşturmak istediğiniz Azure aboneliğini seçin. 
1. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
   - **Var olanı kullan**’ı ve ardından listeden var olan bir kaynak grubunu seçin. 
   - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.   
         
   Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/management/overview.md).  
1. **Sürüm** bölümünde **V2**'yi seçin.
1. **Konum** için, veri fabrikasının konumunu seçin.

   Listede yalnızca Data Factory tarafından desteklenen ve Azure Data Factory meta verilerinizin depolanacağı konumlar gösterilir. İlişkili veri depoları (Azure Depolama ve Azure SQL Veritabanı gibi) ve Veri Fabrikası'nın kullandığı hesaplamalar (Azure HDInsight gibi) diğer bölgelerde çalıştırılabilir.

1. **Oluştur**’u seçin.

1. Oluşturma tamamlandıktan sonra Veri **Fabrikası** sayfasını görürsünüz. Azure Data Factory kullanıcı arabirimi (UI) uygulamasını ayrı bir sekmede başlatmak için **Yazar ve İzleyici** kutucuğunu seçin.
   
   ![Veri fabrikasının “Yazar ve İzleyici” kutucuğuna sahip ana sayfası](./media/doc-common-process/data-factory-home-page.png)

## <a name="start-the-copy-data-tool"></a>Veri Kopyalama aracını başlatma

1. **Başlayalım** sayfasında, Veri Kopyalama aracını başlatmak için **Veri Kopyala** kutucuğunu seçin. 

   ![“Veri kopyala” kutucuğu](./media/doc-common-process/get-started-page.png)

1. Veri Kopyalama aracının **Özellikler** sayfasında işlem hattı için bir ad ve açıklama belirtip **İleri**'yi seçebilirsiniz. 

   ![“Özellikler” sayfası](./media/quickstart-create-data-factory-copy-data-tool/copy-data-tool-properties-page.png)
1. **Kaynak veri deposu** sayfasında aşağıdaki adımları tamamlayın:

    a. Bağlantı eklemek için **+ Yeni bağlantı oluştur**'a tıklayın.

    b. Kaynak bağlantı için oluşturmak istediğiniz bağlantılı hizmet türünü seçin. Bu eğitimde Azure **Blob Depolama'yı**kullanıyoruz. Galeriden seçin ve sonra **Devam'ı**seçin.
    
    ![Blob'u seçin](./media/quickstart-create-data-factory-copy-data-tool/select-blob-source.png)

    c. Yeni **Bağlantılı Hizmet (Azure Blob Depolama)** sayfasında, bağlantılı hizmetinizin adını belirtin. **Depolama hesabı adı** listesinden depolama hesabınızı seçin, bağlantıyı test edin ve sonra **Oluştur'u**seçin. 

    ![Azure Blob depolama hesabını yapılandırın](./media/quickstart-create-data-factory-copy-data-tool/configure-blob-storage.png)

    d. Kaynak olarak yeni oluşturulan bağlantılı hizmeti seçin ve sonra **İleri'yi**tıklatın.


1. **Girdi dosyasını veya klasörünü seçin** sayfasında aşağıdaki adımları uygulayın:

   a. **adftutorial/input** klasörüne gitmek için **Gözat'ı** tıklatın, **emp.txt** dosyasını seçin ve sonra **Seç'i**tıklatın. 

   d. Dosyayı olduğu gibi kopyalamak için **İkili kopya** onay kutusunu seçin ve ardından **İleri'yi**seçin. 

   ![“Girdi dosyası veya klasörü seçin” sayfası](./media/quickstart-create-data-factory-copy-data-tool/select-binary-copy.png)


1. Hedef **veri deposu** sayfasında, oluşturduğunuz **Azure Blob Depolama** bağlantılı hizmeti seçin ve ardından **İleri'yi**seçin. 

1. Çıktı **dosyasını veya klasörü** seçin sayfasında klasör yolu için **adftutorial/çıktı** girin ve sonra **İleri'yi**seçin. 

   ![“Çıktı dosyasını veya klasörünü seçin” sayfası](./media/quickstart-create-data-factory-copy-data-tool/configure-sink-path.png) 

1. **Ayarlar** sayfasında varsayılan yapılandırmaları kullanmak için **İleri**'yi seçin. 

1. **Özet** sayfasında tüm ayarları gözden geçirin ve **İleri**'yi seçin. 

1. **Dağıtım tamamlandı** sayfasında, oluşturduğunuz işlem hattını izlemek için **İzleyici**’yi seçin. 

    ![“Dağıtım tamamlandı” sayfası](./media/quickstart-create-data-factory-copy-data-tool/deployment-page.png)

1. Uygulama **Monitör** sekmesine geçer. Bu sekmede ardışık hattın durumunu görürsünüz. Listeyi yenilemek için **Yenile'yi** seçin. Etkinlik çalıştırma ayrıntılarını görüntülemek veya ardışık hattı yeniden çalıştırmak için **PIPELINE NAME** altındaki bağlantıyı tıklatın. 
   
    ![Boru hattını yenile](./media/quickstart-create-data-factory-copy-data-tool/refresh-pipeline.png)

1. Etkinlik çalışır sayfasında, kopyalama işlemi hakkında daha fazla bilgi için **ACTIVITY NAME** sütununaltındaki **Ayrıntılar** bağlantısını (gözlük simgesi) seçin. Özelliklerle ilgili ayrıntılar için bkz. [Kopyalama Etkinliğine genel bakış](copy-activity-overview.md). 

1. Pipeline Runs görünümüne geri dönmek için, kırıntı menüsündeki **TÜM ardışık hat lar çalışır** bağlantısını seçin. Görünümü yenilemek için **Yenile**’yi seçin. 

1. **adftutorial** kapsayıcısının **çıktı** klasöründe **emp.txt** dosyasının oluşturulduğunu doğrulayın. Çıktı klasörü yoksa, Veri Fabrikası hizmeti otomatik olarak oluşturur. 

1. Bağlantılı hizmetleri, veri kümelerini ve işlem hatlarını düzenlemek için sol panelde **İzleyici** sekmesinin üzerindeki **Yazar** sekmesine geçin. Bunları Data Factory kullanıcı arabiriminde düzenleme hakkında bilgi edinmek için bkz. [Azure portalını kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-portal.md).

    ![Yazar sekmesini seçin](./media/quickstart-create-data-factory-copy-data-tool/select-author.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekteki işlem hattı, verileri bir konumdan Azure Blob depolama alanındaki başka bir konuma kopyalar. Data Factory’yi daha fazla senaryoda kullanma hakkında bilgi almak için [öğreticileri](tutorial-copy-data-portal.md) okuyun. 
