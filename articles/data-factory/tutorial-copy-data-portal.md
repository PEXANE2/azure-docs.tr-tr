---
title: Veri fabrikası ardışık hattı oluşturmak için Azure portalını kullanma
description: Bu öğreticide işlem hattıyla veri fabrikası oluşturmak için Azure portalını kullanmaya yönelik adım adım yönergeler sağlanır. Ardışık iş, Azure Blob depolamaadresindeki verileri Azure SQL veritabanına kopyalamak için kopyalama etkinliğini kullanır.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: tutorial
ms.custom: seo-lt-2019
ms.date: 04/13/2020
ms.author: jingwang
ms.openlocfilehash: 9e81efd826c78bfa1af5624272ae9b190cb54348
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81313849"
---
# <a name="copy-data-from-azure-blob-storage-to-a-sql-database-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Blob depolama alanında SQL veritabanına veri kopyalama
Bu öğreticide, Azure Data Factory kullanıcı arabirimini (UI) kullanarak bir veri fabrikası oluşturursunuz. Bu veri fabrikasındaki ardışık işlem, Azure Blob depolamasından azure SQL veritabanına verileri kopyalar. Bu öğreticideki yapılandırma düzeni, dosya tabanlı bir veri deposundan ilişkisel bir veri deposuna kopyalama için geçerlidir. Kaynak ve havuz olarak desteklenen veri depolarının listesi için [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

> [!NOTE]
> - İlk kez Data Factory kullanıyorsanız bkz. [Azure Data Factory'ye giriş](introduction.md).

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Kopyalama etkinliğiyle işlem hattı oluşturma.
> * İşlem hattında test çalıştırması yapma.
> * İşlem hattını el ile tetikleme.
> * İşlem hattını bir zamanlamaya göre tetikleme.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

## <a name="prerequisites"></a>Ön koşullar
* **Azure aboneliği.** Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* **Azure depolama hesabı**. Blob depolama alanını *kaynak* veri deposu olarak kullanabilirsiniz. Depolama hesabınız yoksa, oluşturma adımları için bkz. [Azure depolama hesabı oluşturma](../storage/common/storage-account-create.md).
* **Azure SQL Veritabanı**. Veritabanını *havuz* veri deposu olarak kullanabilirsiniz. Azure SQL veritabanınız yoksa, oluşturmak için adımlar için [bir SQL veritabanı oluşturma'ya](../sql-database/sql-database-get-started-portal.md) bakın.

### <a name="create-a-blob-and-a-sql-table"></a>Bir blob ve SQL tablosu oluşturma

Aşağıdaki adımları uygulayarak öğretici için Blob depolama alanınızı ve SQL veritabanınızı hazırlayın.

#### <a name="create-a-source-blob"></a>Kaynak blob oluşturma

1. Not Defteri'ni başlatın. Aşağıdaki metni kopyalayın ve diskinizde **emp.txt** dosyası olarak kaydedin:

    ```
    FirstName,LastName
    John,Doe
    Jane,Doe
    ```

1. Blob depolama alanınızda **adftutorial** adlı bir kapsayıcı oluşturun. Bu kapsayıcıda **input** adlı bir klasör oluşturun. Sonra, **emp.txt** dosyasını **input** klasörüne yükleyin. Bu görevleri yerine getirmek için Azure Portal'ı veya [Azure Depolama Gezgini](https://storageexplorer.com/) gibi araçları kullanın.

#### <a name="create-a-sink-sql-table"></a>Havuz SQL tablosu oluşturma

1. SQL veritabanınızda **dbo.emp** tablosu oluşturmak için aşağıdaki SQL betiğini kullanın:

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

1. Azure hizmetlerinin SQL Server’a erişmesine izin ver. Data Factory’nin SQL Server’ınıza veri yazabilmesi için SQL Server’ınız için **Azure hizmetlerine erişime izin ver** ayarının **AÇIK** olduğundan emin olun. Bu ayarı doğrulamak ve açmak için Azure SQL sunucusuna gidin > Genel Bakış > Sunucu güvenlik duvarını ayarla> **Azure hizmetlerine Erişime İzin Ver** seçeneğini **ON**olarak ayarlayın.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Bu adımda, bir veri fabrikası oluşturacak ve veri fabrikasında bir işlem hattı oluşturmak için Data Factory kullanıcı arabirimini başlatacaksınız.

1. **Microsoft Edge** veya **Google Chrome'u**açın. Şu anda Data Factory kullanıcı arabirimi yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenmektedir.
2. Sol menüde, **kaynak** > **Analiz** > **Veri Fabrikası**Oluştur'u seçin.
3. **Yeni veri fabrikası** sayfasında **Ad** bölümüne **ADFTutorialDataFactory** girin.

   Azure veri fabrikasının adı *genel olarak benzersiz*olmalıdır. Ad değeriyle ilgili bir hata iletisi alırsanız, veri fabrikası için farklı bir ad girin. (örneğin, adınızADFTutorialDataFactory). Data Factory yapıtlarının adlandırma kuralları için bkz.[Data Factory adlandırma kuralları](naming-rules.md).

     ![Yeni veri fabrikası](./media/doc-common-process/name-not-available-error.png)
4. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğinizi** seçin.
5. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:

    a. **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin.

    b. **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin. 
         
    Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/management/overview.md). 
6. **Sürüm** bölümünde **V2**'yi seçin.
7. **Konum** bölümünden veri fabrikası için bir konum seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan veri depoları (örneğin, Azure Depolama ve SQL Veritabanı) ve işlemler (örneğin, Azure HDInsight) başka bölgelerde olabilir.
8. **Oluştur'u**seçin.
9. Oluşturma tamamlandıktan sonra bildirimler merkezinde bildirimi görürsünüz. Veri fabrikası sayfasına gitmek için **kaynağa** git'i seçin.
10. Data Factory Kullanıcı Arabirimini (UI) ayrı bir sekmede başlatmak için **Geliştir ve İzle**’yi seçin.


## <a name="create-a-pipeline"></a>İşlem hattı oluşturma
Bu adımda, veri fabrikasında kopyalama etkinliği ile bir işlem hattı oluşturacaksınız. Kopyalama etkinliği, verileri Blob depolama alanından SQL Veritabanı'na kopyalar. [Hızlı başlangıç eğitiminde](quickstart-create-data-factory-portal.md), şu adımları izleyerek bir işlem hattı oluşturdunuz:

1. Bağlı hizmeti oluşturma.
1. Giriş ve çıkış veri kümelerini oluşturma.
1. İşlem hattı oluşturma.

Bu öğreticide işlem hattını oluşturmaya başlayacaksınız. Daha sonra işlem hattını yapılandırmanız gerektiğinde bağlı hizmetleri ve veri kümelerini oluşturacaksınız.

1. **Kullanmaya başlama** sayfasında **İşlem hattı oluştur** seçeneğini belirleyin.

   ![İşlem hattı oluşturma](./media/doc-common-process/get-started-page.png)
1. İşlem hattının **Genel** sekmesinde **Ad** alanına **CopyPipeline** yazın.

1. **Etkinlikler** araç kutusunda, **Taşı ve Dönüştür** kategorisini genişletin ve Veri **Kopyalama** etkinliğini araç kutusundan boru hattı tasarımcısı yüzeyine sürükleyip bırakın. **Ad** için **CopyFromBlobToSql** adını belirtin.

    ![Kopyalama etkinliği](./media/tutorial-copy-data-portal/drag-drop-copy-activity.png)

### <a name="configure-source"></a>Kaynağı yapılandırma

>[!TIP]
>Bu öğreticide, kaynak veri deponuzun kimlik doğrulama türü olarak *Hesap anahtarını* kullanırsınız, ancak desteklenen diğer kimlik doğrulama yöntemlerini seçebilirsiniz: *Gerekirse SAS URI,**Servis Sorumlusu* ve *Yönetilen Kimlik.* Ayrıntılar için [bu makaledeki](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#linked-service-properties) ilgili bölümlere bakın.
>Veri depolarının sırlarını güvenli bir şekilde depolamak için Azure Anahtar Kasası kullanılması da önerilir. Ayrıntılı resimler için [bu makaleye](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault) bakın.

1. **Kaynak** sekmesine gidin. Kaynak veri kümesi oluşturmak için **+ Yeni'yi** seçin.

1. Yeni **Veri Kümesi** iletişim kutusunda **Azure Blob Depolama'yı**seçin ve ardından **Devam et'i**seçin. Kaynak veriler bir Blob depolama alanında olduğundan kaynak veri kümesi olarak **Azure Blob Depolama Alanı**'nı seçmeniz gerekir.

1. **Biçim'i Seç** iletişim kutusunda, verilerinizin biçim türünü seçin ve ardından **Devam et'i**seçin.

1. Özellikleri **Ayarla** iletişim kutusuna, Ad için **SourceBlobDataset'i** girin. İlk satır için onay kutusunu **üstbilgi olarak**seçin. Bağlantılı **hizmet** metin kutusunun altında **+ Yeni'yi**seçin.

1. Yeni **Bağlantılı Hizmet (Azure Blob Depolama)** iletişim kutusunda, ad olarak **AzureStorageLinkedService'i** girin, **Depolama hesabı adı** listesinden depolama hesabınızı seçin. Bağlantıyı test edin, bağlantılı hizmeti dağıtmak için **Oluştur'u** seçin.

1. Bağlı hizmet oluşturulduktan sonra, **Özellikleri Ayarla** sayfasına geri yönlendirilir. **Dosya yolu**’nun yanındaki **Gözat** seçeneğini belirleyin.

1. **adftutorial/input** klasörüne gidin, **emp.txt** dosyasını seçin ve sonra **Tamam'ı**seçin.

1. **Tamam'ı**seçin. Otomatik olarak boru hattı sayfasına doğru ilerler. **Kaynak** sekmesinde, **SourceBlobDataset'in** seçildiğini onaylayın. Bu sayfadaki verilerin önizlemesini görüntülemek için **Veri önizleme **‘yi seçin.

    ![Kaynak veri kümesi](./media/tutorial-copy-data-portal/source-dataset-selected.png)

### <a name="configure-sink"></a>Havuzu yapılandırma
>[!TIP]
>Bu öğreticide, lavabo veri deponuz için kimlik doğrulama türü olarak *SQL kimlik doğrulaması* kullanırsınız, ancak desteklenen diğer kimlik doğrulama yöntemlerini seçebilirsiniz: Gerekirse *Hizmet Sorumlusu* ve *Yönetilen Kimlik.* Ayrıntılar için [bu makaledeki](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-database#linked-service-properties) ilgili bölümlere bakın.
>Veri depolarının sırlarını güvenli bir şekilde depolamak için Azure Anahtar Kasası kullanılması da önerilir. Ayrıntılı resimler için [bu makaleye](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault) bakın.

1. **Havuz** sekmesine gidin ve havuz veri kümesi oluşturmak için **+Yeni** seçeneğini belirleyin.

1. Yeni **Dataset** iletişim kutusunda, bağlayıcılara filtre açmak için arama kutusuna "SQL" girişi yapın, **Azure SQL Veritabanı'nı**seçin ve ardından **Devam et'i**seçin. Bu öğreticide verileri bir SQL veritabanına kopyalayacaksınız.

1. Özellikleri **Ayarla** iletişim kutusuna, Ad için **OutputSqlDataset'i** girin. Bağlantılı **hizmet** açılır listesinden **+ Yeni'yi**seçin. Bağlı hizmeti bir veri kümesi ile ilişkilendirilmelidir. Bağlı hizmet, Data Factory’nin çalışma zamanında SQL veritabanına bağlanmak için kullandığı bağlantı dizesini içerir. Veri kümesi, verilerin kopyalanacağı kapsayıcıyı, klasörü ve dosyayı (isteğe bağlı) belirtilir.

1. Yeni **Bağlantılı Hizmet (Azure SQL Veritabanı)** iletişim kutusunda aşağıdaki adımları izleyin:

    a. **Ad** bölümüne **AzureSqlDatabaseLinkedService** girin.

    b. **Sunucu adı** bölümünde SQL Server örneğinizi seçin.

    c. **Veritabanı adı** bölümünde SQL veritabanınızı seçin.

    d. **Kullanıcı adı** bölümüne kullanıcının adını girin.

    e. **Parola** bölümüne kullanıcının parolasını girin.

    f. Bağlantıyı test etmek için **Bağlantıyı sına**’yı seçin.

    g. Bağlantılı hizmeti dağıtmak için **Oluştur'u** seçin.

    ![Yeni bağlı hizmeti kaydedin](./media/tutorial-copy-data-portal/new-azure-sql-linked-service-window.png)

1. Özellikleri **Ayarla** iletişim kutusuna otomatik olarak yönlendirin. **Tablo**’da **[dbo].[emp]** seçeneğini belirleyin. Sonra **Tamam**’ı seçin.

1. İşlem hattının bulunduğu sekmeye gidin ve **Havuz Veri Kümesi**’nde **OutputSqlDataset** seçeneğinin belirlendiğinden emin olun.

    ![İşlem hattı sekmesi](./media/tutorial-copy-data-portal/pipeline-tab-2.png)       

İsteğe bağlı [olarak, kopya etkinliğinde Schema eşlemesini](copy-activity-schema-and-type-mapping.md)izleyerek kaynağın şemasını hedefin ilgili şemasıyla eşleyebilirsiniz.

## <a name="validate-the-pipeline"></a>İşlem hattını doğrulama
İşlem hattını doğrulamak için araç çubuğundan **Doğrula**'yı seçin.

Sağ üstteki **Kodu** tıklatarak ardışık alanla ilişkili JSON kodunu görebilirsiniz.

## <a name="debug-and-publish-the-pipeline"></a>İşlem hattında hata ayıklama ve işlem hattını yayımlama
Yapıtları (bağlı hizmetler, veri kümeleri ve işlem hattı) Data Factory'de veya kendi Azure Repos Git deponuzda yayımlamadan önce işlem hattında hata ayıklayabilirsiniz.

1. İşlem hattında hata ayıklamak için araç çubuğunda **Hata Ayıkla**'yı seçin. Pencerenin altındaki **Çıkış** sekmesinde işlem hattı çalıştırmasının durumu görüntülenir.

1. Ardışık hatlar başarılı bir şekilde çalıştırıladıktan sonra, üst araç çubuğunda **Tümünü Yayımla'yı**seçin. Bu eylem, oluşturduğunuz varlıkları (veri kümeleri ve işlem hatları) Data Factory'de yayımlar.

1. **Başarıyla yayımlandı** iletisini görene kadar bekleyin. Bildirim iletilerini görmek için sağ üstteki **Bildirimleri Göster**'e (zil düğmesi) tıklayın.

## <a name="trigger-the-pipeline-manually"></a>İşlem hattını el ile tetikleme
Bu adımda, önceki adımda yayımladığınız işlem hattını el ile tetiklersiniz.

1. Araç çubuğunda **Tetikleyici**’yi ve sonra **Şimdi Tetikle**’yi seçin. Pipeline **Run** sayfasında **Tamam'ı**seçin.  

1. Soldaki **İzleyici** sekmesine gidin. El ile tetikleme tarafından tetiklenmiş bir işlem hattı çalıştırması görürsünüz. Etkinlik ayrıntılarını görüntülemek ve ardışık hattı yeniden çalıştırmak için **PIPELINE NAME** sütununaltındaki bağlantıları kullanabilirsiniz.

    [![İzleme boru hattı çalışır](./media/tutorial-copy-data-portal/monitor-pipeline-inline-and-expended.png)](./media/tutorial-copy-data-portal/monitor-pipeline-inline-and-expended.png#lightbox)

1. Ardışık hatlar çalışmasıyla ilişkili etkinlik çalıştırmalarını görmek için **PIPELINE NAME** sütunundaki **CopyPipeline** bağlantısını seçin. Bu örnekte, yalnızca bir etkinlik vardır, bu nedenle listede yalnızca bir giriş görürsünüz. Kopyalama işlemi yle ilgili ayrıntılar için **ACTIVITY NAME** sütunundaki **Ayrıntılar** bağlantısını (gözlük simgesi) seçin. Tüm **ardışık hatlar,** Pipeline Runs görünümüne geri dönmek için en üstte çalışır. Görünümü yenilemek için **Yenile**’yi seçin.

    [![Etkinlik çalıştırmalarını izleme](./media/tutorial-copy-data-portal/view-activity-runs-inline-and-expended.png)](./media/tutorial-copy-data-portal/view-activity-runs-inline-and-expended.png#lightbox)

1. SQL veritabanında **emp** tablosuna iki satır daha eklendiğinden emin olun.

## <a name="trigger-the-pipeline-on-a-schedule"></a>İşlem hattını bir zamanlamaya göre tetikleme
Bu zamanlamada, işlem hattı için bir zamanlayıcı tetikleyicisi oluşturacaksınız. Tetikleyici, işlem hattını saatlik veya günlük gibi belirli bir zamanlamaya göre çalıştırır. Burada belirtilen bitiş tarihine kadar her dakika çalıştırmak için tetikleyici ayarlayın.

1. Sol üstte, izleyici sekmesinin üzerindeki **Yazar** sekmesine gidin.

1. İşlem hattınıza gidin, araç çubuğunda **Tetikleyici**'ye tıklayıp **Yeni/Düzenle**'yi seçin.

1. **Tetikleyiciler ekle** iletişim kutusunda , + **Tetikleyici** alanını seç **için Yeni' yi** seçin.

1. **Yeni Tetikleyici** penceresinde aşağıdaki adımları uygulayın:

    a. **Ad** bölümüne **RunEveryMinute** girin.

    b. **Bitiş** bölümünde **Tarih** seçeneğini belirleyin.

    c. **Bitiş Tarihi** bölümde açılan listeden seçim yapın.

    d. **Geçerli gün** seçeneğini belirleyin. Varsayılan olarak, bitiş günü olarak bir sonraki gün ayarlanır.

    e. Bitiş **Saati** bölümünü geçerli datetime'ı birkaç dakika geçmiş olacak şekilde güncelleştirin. Tetikleyicinin etkinleştirilmesi için, önce sizin değişiklikleri yayımlamanız gerekir. Eğer bunu yalnızca birkaç dakika arayla ayarlarsanız ve o zamana kadar yayınlamazsanız, tetikleyici bir çalıştırma göremezsin.

    f. **Tamam'ı**seçin.

    g. **Etkinleştirilen** seçenek için **Evet'i**seçin.

    h. **Tamam'ı**seçin.

    > [!IMPORTANT]
    > Her bir işlem hattı çalıştırması ile bir maliyet ilişkilendirildiğinden bitiş tarihini uygun bir şekilde ayarlayın.

1. **Tetikleyiciyi Edit** sayfasında uyarıyı gözden geçirin ve ardından **Kaydet'i**seçin. Bu örnekteki işlem hattı hiçbir parametre almaz.

1. Değişikliği yayınlamak için **Tümünü Yayımla'yı** tıklatın.

1. Tetiklenen işlem hattı çalıştırmalarını görmek için sol taraftaki **İzleyici** sekmesine gidin.

    [![Tetiklenen işlem hattı çalıştırmaları](./media/tutorial-copy-data-portal/triggered-pipeline-runs-inline-and-expended.png)](./media/tutorial-copy-data-portal/triggered-pipeline-runs-inline-and-expended.png#lightbox)

1. **Pipeline Runs** görünümünden Tetikleyici **Çalışır** görünümüne geçmek için pencerenin sol tarafındaki **Tetik Çalışır'ı** seçin.

1. Listede tetikleyici çalıştırmalarını görürsünüz.

1. Belirtilen bitiş saatine kadar **emp** tablosuna dakikada iki satır (her işlem hattı çalıştırması için) eklendiğini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekteki işlem hattı, verileri bir konumdan Blob depolama alanındaki başka bir konuma kopyalar. Şunları öğrendiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Kopyalama etkinliğiyle işlem hattı oluşturma.
> * İşlem hattında test çalıştırması yapma.
> * İşlem hattını el ile tetikleme.
> * İşlem hattını bir zamanlamaya göre tetikleme.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.


Verileri şirket içinden buluta kopyalamayı öğrenmek için aşağıdaki öğreticiye geçin:

> [!div class="nextstepaction"]
>[Şirket içinden buluta veri kopyalama](tutorial-hybrid-copy-portal.md)
