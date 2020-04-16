---
title: Verileri Azure Blob depolamadan SQL'e Kopyala veri aracını kullanarak kopyalama
description: Azure veri fabrikası oluşturun ve ardından Azure Blob depolamadan sql veritabanına veri kopyalamak için Veri Kopyalama aracını kullanın.
services: data-factory
documentationcenter: ''
author: linda33wj
ms.author: jingwang
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: tutorial
ms.custom: seo-lt-2019
ms.date: 03/03/2020
ms.openlocfilehash: be9395b908461b54b607fff32747ca0d9f20f45c
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81418669"
---
# <a name="copy-data-from-azure-blob-storage-to-a-sql-database-by-using-the-copy-data-tool"></a>Verileri Azure Blob depolamasından Kopyaveri aracını kullanarak SQL Veritabanına kopyalama

> [!div class="op_single_selector" title1="Kullanmakta olduğunuz Veri Fabrikası hizmetinin sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Geçerli sürüm](tutorial-copy-data-tool.md)

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Bu öğreticide, Azure portalını kullanarak bir veri fabrikası oluşturursunuz. Ardından, Azure Blob depolamasundaki verileri SQL Veritabanı'na kopyalayan bir ardışık iş aktonu oluşturmak için Veri Kopyala aracını kullanırsınız.

> [!NOTE]
> Azure Data Factory kullanmaya yeni başlıyorsanız bkz. [Azure Data Factory'ye giriş](introduction.md).

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:
> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Veri Kopyalama aracını kullanarak bir işlem hattı oluşturun.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* **Azure Depolama hesabı**: _Kaynak_ veri deposu olarak Blob depolamasını kullanın. Azure Depolama hesabınız yoksa, [depolama hesabı oluştur'daki](../storage/common/storage-account-create.md)yönergelere bakın.
* **Azure SQL Veritabanı**: _Lavabo_ veri deposu olarak bir SQL Veritabanı kullanın. SQL Veritabanınız yoksa, [SQL Veritabanı Oluştur'daki](../sql-database/sql-database-get-started-portal.md)yönergeler'e bakın.

### <a name="create-a-blob-and-a-sql-table"></a>Bir blob ve SQL tablosu oluşturma

Bu adımları gerçekleştirerek Blob depolamanızı ve SQL Veritabanınızı öğreticiye hazırlayın.

#### <a name="create-a-source-blob"></a>Kaynak blob oluşturma

1. **Başlat Not Defteri**. Aşağıdaki metni kopyalayıp diskinizde **inputEmp.txt** adlı bir dosyaya kaydedin:

    ```
    FirstName|LastName
    John|Doe
    Jane|Doe
    ```

1. **adfv2tutorial** adlı bir kapsayıcı oluşturun ve inputEmp.txt dosyasını kapsayıcıya yükleyin. Bu görevleri gerçekleştirmek için Azure portalını veya [Azure Depolama Gezgini](https://storageexplorer.com/) gibi çeşitli araçları kullanabilirsiniz.

#### <a name="create-a-sink-sql-table"></a>Havuz SQL tablosu oluşturma

1. SQL Veritabanınızda **dbo.emp** adlı bir tablo oluşturmak için aşağıdaki SQL komut dosyasını kullanın:

    ```sql
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50)
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);
    ```

2. Azure hizmetlerinin SQL Server’a erişmesine izin ver. SQL Veritabanı'nı çalıştıran sunucunuz için **azure hizmetlerine ve kaynaklarına erişmesine izin** ver ayarı etkinleştirin. Bu ayar, Data Factory’nin veritabanı örneğinize veri yazmasına imkan tanır. Bu ayarı doğrulamak ve açmak için, Azure **hizmetlerine ve kaynaklarına erişmesine** izin vermek için Azure hizmetlerine ve kaynaklarına **>** Güvenlik > Güvenlik Duvarları'nı ve sanal ağları > Azure SQL sunucusuna gidin.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Sol menüde, **bir kaynak** > **Analiz** > **Veri Fabrikası**Oluştur'u seçin:

    ![Yeni veri fabrikası oluşturma](./media/doc-common-process/new-azure-data-factory-menu.png)
1. **Yeni veri fabrikası** sayfasında **Ad** bölümüne **ADFTutorialDataFactory** girin.

    Veri fabrikanızın adı _genel olarak benzersiz_ olmalıdır. Aşağıdaki hata iletisini alabilirsiniz:

    ![Yeni veri fabrikası hata iletisi](./media/doc-common-process/name-not-available-error.png)

    Ad değeriyle ilgili bir hata iletisi alırsanız, veri fabrikası için farklı bir ad girin. Örneğin, _**adınız**_**ADFTutorialDataFactory** adını kullanın. Data Factory yapıtlarını adlandırma kuralları için bkz. [Data Factory adlandırma kuralları](naming-rules.md).
1. Yeni veri fabrikasının oluşturulacağı Azure **aboneliğini** seçin.
1. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:

    a. **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin.

    b. **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.
    
    Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/management/overview.md).

1. **Sürüm** bölümünde **V2**'yi seçin.
1. **Konum** bölümünde veri fabrikasının konumunu seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikanız tarafından kullanılan veri depoları (örneğin, Azure Depolama ve SQL Veritabanı) ve işlemler (örneğin, Azure HDInsight) başka konumlarda ve bölgelerde olabilir.
1. **Oluştur**’u seçin.

1. Oluşturma işlemi tamamlandıktan sonra **Data Factory** giriş sayfası görüntülenir.

    ![Data factory giriş sayfası](./media/doc-common-process/data-factory-home-page.png)
1. Azure Data Factory kullanıcı arabirimini (UI) ayrı bir sekmede açmak için **Yazar ve İzleyici** kutucuğunu seçin.

## <a name="use-the-copy-data-tool-to-create-a-pipeline"></a>Veri Kopyalama aracını kullanarak işlem hattı oluşturma

1. **Başlayalım** sayfasında, Veri Kopyalama aracını açmak için **Veri Kopyala** kutucuğunu seçin.

    ![Veri Kopyalama aracının kutucuğu](./media/doc-common-process/get-started-page.png)
1. **Özellikler** sayfasındaki **Görev adı** bölümüne **CopyFromBlobToSqlPipeline** adını girin. Ardından **İleri'yi**seçin. Data Factory kullanıcı arabirimi, belirtilen görev adına sahip bir işlem hattı oluşturur.
    ![İşlem hattı oluşturma](./media/tutorial-copy-data-tool/create-pipeline.png)

1. **Kaynak veri deposu** sayfasında aşağıdaki adımları tamamlayın:

    a. Bağlantı eklemek için **+ Yeni bağlantı oluştur'u** tıklatın

    b. Galeriden **Azure Blob Depolama'yı** seçin ve ardından **Devam et'i**seçin.

    c. Yeni **Bağlantılı Hizmet** sayfasında, Azure aboneliğinizi seçin ve **Depolama hesabı adı** listesinden depolama hesabınızı seçin. Bağlantıyı test edin ve ardından **Oluştur'u**seçin.

    d. Kaynak olarak yeni oluşturulan bağlantılı hizmeti seçin ve **İleri**'ye tıklayın.

    ![Kaynak olarak bağlantılı hizmeti seçin](./media/tutorial-copy-data-tool/select-source-linked-service.png)

1. **Girdi dosyasını veya klasörünü seçin** sayfasında aşağıdaki adımları uygulayın:

    a. **Göz at**'a tıklayarak **adfv2tutorial/input** klasörüne gidin, **inputEmp.txt** dosyasını seçin ve **Seç**'e tıklayın.

    b. Sonraki adıma geçmek için **İleri**'ye tıklayın.

1. Dosya **biçimi ayarları** sayfasında, İlk satır için onay kutusunu *üstbilgi olarak*etkinleştirin. Aracın sütun ve satır sınırlayıcıları otomatik olarak algıladığını unutmayın. **İleri**’yi seçin. Ayrıca verileri önizleyebilir ve bu sayfadaki giriş verilerinin şemasını görüntüleyebilirsiniz.

    ![Dosya biçimi ayarları](./media/tutorial-copy-data-tool/file-format-settings-page.png)
1. **Hedef veri deposu** sayfasında aşağıdaki adımları tamamlayın:

    a. Bağlantı eklemek için **+ Yeni bağlantı oluştur'u** tıklatın

    b. Galeriden **Azure SQL Veritabanı'nı** seçin ve ardından **Devam et'i**seçin.

    c. Yeni **Bağlantılı Hizmet** sayfasında, açılan listeden sunucu adınızı ve DB adınızı seçin ve kullanıcı adını ve parolayı belirtin, ardından **Oluştur'u**seçin.

    ![Azure SQL veritabanını yapılandırma](./media/tutorial-copy-data-tool/config-azure-sql-db.png)

    d. Kaynak olarak yeni oluşturulan bağlantılı havuz hizmetini seçin ve **İleri**'ye tıklayın.

1. **Tablo eşleme** sayfasında **[dbo].[emp]** tablosunu seçip **İleri**’yi seçin.

1. Sütun **eşleme** sayfasında, giriş dosyasındaki ikinci ve üçüncü sütunların **emp** tablosunun **FirstName** ve **Soyadı** sütunlarına eşlenmiş olduğuna dikkat edin. Hata olmadığından emin olmak için eşlemi ayarlayın ve sonra **İleri'yi**seçin.

    ![Sütun eşleme sayfası](./media/tutorial-copy-data-tool/column-mapping.png)

1. **Ayarlar** sayfasında **İleri**’yi seçin.
1. **Özet** sayfasında ayarları gözden geçirin ve **İleri**’yi seçin.
1. **Dağıtım** sayfasında, işlem hattını (görev) izlemek için **İzleyici**’yi seçin.
 
    ![İşlem hattını izleme](./media/tutorial-copy-data-tool/monitor-pipeline.png)

1. Pipeline çalışır sayfasında, listeyi yenilemek için **Yenile'yi** seçin. Etkinlik çalıştırma ayrıntılarını görüntülemek veya ardışık hattı yeniden çalıştırmak için **PIPELINE NAME** altındaki bağlantıyı tıklatın. 
    ![Boru hattı çalıştır](./media/tutorial-copy-data-tool/pipeline-run.png)

1. Etkinlik çalışır sayfasında, kopyalama işlemi hakkında daha fazla bilgi için **ACTIVITY NAME** sütununaltındaki **Ayrıntılar** bağlantısını (gözlük simgesi) seçin. Pipeline Runs görünümüne geri dönmek için, kırıntı menüsündeki **TÜM ardışık hat lar çalışır** bağlantısını seçin. Görünümü yenilemek için **Yenile**’yi seçin.

    ![Etkinlik çalıştırmalarını izleme](./media/tutorial-copy-data-tool/activity-monitoring.png)


1. Verilerin SQL Veritabanınızdaki **dbo.emp** tablosuna eklenmiş olduğunu doğrulayın.


1. Düzenleyici moduna geçmek için soldaki **Yazar** sekmesini seçin. Düzenleyici kullanılarak araç üzerinden oluşturulan bağlı hizmetleri, veri kümelerini ve işlem hatlarını güncelleştirebilirsiniz. Bu varlıkları Data Factory kullanıcı arabiriminde düzenlemeyle ilgili ayrıntılar için [bu öğreticinin Azure portalı sürümüne](tutorial-copy-data-portal.md) bakın.

    ![Yazar sekmesini seçin](./media/tutorial-copy-data-tool/author-tab.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekteki ardışık işlem, Blob depolamadan bir SQL Veritabanına verileri kopyalar. Şunları öğrendiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Veri Kopyalama aracını kullanarak bir işlem hattı oluşturun.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

Verileri şirket içinden buluta kopyalamayı öğrenmek için aşağıdaki öğreticiye geçin:

>[!div class="nextstepaction"]
>[Şirket içinden buluta veri kopyalama](tutorial-hybrid-copy-data-tool.md)
