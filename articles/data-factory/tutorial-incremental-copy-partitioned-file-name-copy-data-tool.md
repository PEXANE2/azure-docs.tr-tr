---
title: Zaman bölümlenmiş dosya adına göre yeni dosyaları aşamalı olarak kopyalama
description: Azure veri fabrikası oluşturun ve ardından yalnızca zaman bölümlenmiş dosya adına bağlı olarak yeni dosyaları artımlı olarak yüklemek için Veri Kopyala aracını kullanın.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.devlang: na
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 3/17/2020
ms.openlocfilehash: 50e82362f39da2d3f3ecfad2de1ed07b68f8a6a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79501608"
---
# <a name="incrementally-copy-new-files-based-on-time-partitioned-file-name-by-using-the-copy-data-tool"></a>Veri Kopyalama aracını kullanarak zaman bölümlenmiş dosya adına göre yeni dosyaları artımlı olarak kopyalayın

Bu öğreticide, Azure portalını kullanarak bir veri fabrikası oluşturursunuz. Ardından, Azure Blob depolamadan Azure Blob depolamasına zaman bölümlenmiş dosya adını temel alan yeni dosyaları artımlı olarak kopyalayan bir ardışık kaynak oluşturmak için Verileri Kopyala aracını kullanırsınız.

> [!NOTE]
> Azure Data Factory kullanmaya yeni başlıyorsanız bkz. [Azure Data Factory'ye giriş](introduction.md).

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Veri Kopyalama aracını kullanarak bir işlem hattı oluşturun.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* **Azure depolama hesabı**: _Kaynak_ ve _lavabo_ veri deposu olarak Blob depolamasını kullanın. Azure depolama hesabınız yoksa [Depolama hesabı oluşturma](../storage/common/storage-account-create.md) bölümündeki yönergelere bakın.

### <a name="create-two-containers-in-blob-storage"></a>Blob depolama alanında iki kapsayıcı oluşturma

Bu adımları gerçekleştirerek Blob depolama nızı öğreticiye hazırlayın.

1. **Kaynak**adlı bir kapsayıcı oluşturun.  Kapsayıcınızda **2020/03/17/03** olarak bir klasör yolu oluşturun. Boş bir metin dosyası oluşturun ve **dosya1.txt**olarak adlandırın. Dosya1.txt'yi depolama hesabınızdaki klasör yolu **kaynağı/2020/03/17/03'e** yükleyin.  Bu görevleri [Azure Depolama Gezgini](https://storageexplorer.com/) gibi çeşitli araçlar kullanarak gerçekleştirebilirsiniz.

    ![dosya yükleme](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/upload-file.png)

    > [!NOTE]
    > Lütfen klasör adını UTC saatinizle ayarlayın.  Örneğin, geçerli UTC saati 17 Mart 2020 tarihinde 03:38 ise, **kaynak/{Year}/{Month}/{Day}/{Hour}/** olarak klasör yolunu **kaynak/2020/03/17/03/** olarak oluşturabilirsiniz.

2. **Hedef**adlı bir kapsayıcı oluşturun. Bu görevleri [Azure Depolama Gezgini](https://storageexplorer.com/) gibi çeşitli araçlar kullanarak gerçekleştirebilirsiniz.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Sol menüde > kaynak**Veri + Analitik** > **Veri Fabrikası** **Oluştur'u**seçin:

   ![“Yeni” bölmesinde Data Factory seçimi](./media/doc-common-process/new-azure-data-factory-menu.png)

2. **Yeni veri fabrikası** sayfasında **Ad** bölümüne **ADFTutorialDataFactory** girin.

    Veri fabrikanızın adı _genel olarak benzersiz_ olmalıdır. Aşağıdaki hata iletisini alabilirsiniz:

   ![Yeni veri fabrikası hata iletisi](./media/doc-common-process/name-not-available-error.png)

   Ad değeriyle ilgili bir hata iletisi alırsanız, veri fabrikası için farklı bir ad girin. Örneğin, _**adınız**_**ADFTutorialDataFactory** adını kullanın. Data Factory yapıtlarını adlandırma kuralları için bkz. [Data Factory adlandırma kuralları](naming-rules.md).
3. Yeni veri fabrikasının oluşturulacağı Azure **aboneliğini** seçin.
4. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:

    a. **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin.

    b. **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin. 
         
    Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/management/overview.md).

5. **Sürüm** bölümünde **V2**'yi seçin.
6. **Konum** bölümünde veri fabrikasının konumunu seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikanız tarafından kullanılan veri depoları (örneğin, Azure Depolama ve SQL Veritabanı) ve işlemler (örneğin, Azure HDInsight) başka konumlarda ve bölgelerde olabilir.
7. **Oluştur'u**seçin.
8. Oluşturma işlemi tamamlandıktan sonra **Data Factory** giriş sayfası görüntülenir.
9. Azure Data Factory kullanıcı arabirimini (UI) ayrı bir sekmede açmak için **Yazar ve İzleyici** kutucuğunu seçin.

    ![Data factory giriş sayfası](./media/doc-common-process/data-factory-home-page.png)


## <a name="use-the-copy-data-tool-to-create-a-pipeline"></a>Veri Kopyalama aracını kullanarak işlem hattı oluşturma

1. **Başlatalım** sayfasında, Verileri Kopyala aracını başlatmak için **Verileri Kopyala** başlığını seçin.

   ![Veri Kopyalama aracının kutucuğu](./media/doc-common-process/get-started-page.png)

2. **Özellikler** sayfasında aşağıdaki adımları izleyin:

    a. **Görev adı altında,** **DeltaCopyFromBlobPipeline**girin.

    b. **Görev cadence veya Görev zamanlaması**altında, **zamanlamaya göre düzenli olarak Çalıştır'ı**seçin.

    c. **Tetikleyici türü**altında, **Yuvarlanan Pencere'yi**seçin.

    d. **Yineleme**altında, **1 Saat(ler)** girin.

    e. **Sonraki'ni**seçin.

    Data Factory kullanıcı arabirimi, belirtilen görev adına sahip bir işlem hattı oluşturur.

    ![Özellikler sayfası](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/copy-data-tool-properties-page.png)
3. **Kaynak veri deposu** sayfasında aşağıdaki adımları tamamlayın:

    a. Bağlantı eklemek için **+ Yeni bağlantı oluştur'u** tıklatın
    
    b. Galeriden Azure Blob Depolama'yı seçin ve ardından Devam et'i seçin.
    
    c. Yeni **Bağlantılı Hizmet (Azure Blob Depolama)** sayfasında, bağlantılı hizmet için bir ad girin. Azure aboneliğinizi seçin ve **Depolama hesabı adı** listesinden depolama hesabınızı seçin. Bağlantıyı test edin ve ardından **Oluştur'u**seçin.

    ![Kaynak veri deposu sayfası](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/source-data-store-page-linkedservice.png)

    d. **Kaynak veri deposu** sayfasında yeni oluşturulan bağlantılı hizmeti seçin ve sonra **İleri'yi**tıklatın.

4. **Girdi dosyasını veya klasörünü seçin** sayfasında aşağıdaki adımları uygulayın:

    a. Göz atın ve **kaynak** kapsayıcı seçin, sonra **seçin**.

    ![Girdi dosyası veya klasörü seçin](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/choose-input-file-folder.png)

    b. **Dosya yükleme davranışı**altında, **Artımlı yükleme seçin: zaman bölümlenmiş klasör/dosya adları.**

    c. Dinamik klasör yolunu **kaynak/{year}/{month}/{day}/{hour}/** olarak yazın ve aşağıdaki ekran görüntüsünde gösterildiği gibi biçimi değiştirin. **İkili kopyayı** denetleyin ve **İleri'yi**tıklatın.

    ![Girdi dosyası veya klasörü seçin](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/check-binary-copy.png)     

5. Hedef **veri deposu** sayfasında, veri kaynağı deposuyla aynı depolama hesabı olan **AzureBlobStorage'ı**seçin ve **ardından İleri'yi**tıklatın.

    ![Hedef veri deposu sayfası](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/destination-data-store-page-select-linkedservice.png)
6. Çıktı **dosyasını veya klasörünü seç** sayfasında aşağıdaki adımları yapın:

    a. Göz atın ve **hedef** klasörü seçin, sonra **Seç'i**tıklatın.

    b. Dinamik klasör yolunu **hedef/{year}/{month}/{day}/{hour}/** olarak yazın ve biçimi aşağıdaki gibi değiştirin:

    ![Çıktı dosyasını veya klasörünü seçin](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/output-file-name.png)

    c. **İleri**'ye tıklayın.

    ![Çıktı dosyasını veya klasörünü seçin](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/click-next-after-output-folder.png)
7. **Ayarlar** sayfasında **İleri**’yi seçin.

8. **Özet** sayfasında ayarları gözden geçirin ve **İleri**’yi seçin.

    ![Özet sayfası](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/summary-page.png)

9. **Dağıtım** sayfasında, işlem hattını (görev) izlemek için **İzleyici**’yi seçin.
    ![Dağıtım sayfası](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/deployment-page.png)

10. Soldaki **İzleyici** sekmesinin otomatik olarak seçildiğine dikkat edin.  Otomatik olarak tetiklendiğinde (yaklaşık bir saat sonra) boru hattının çalışmasını beklemeniz gerekir. Çalıştığında, etkinlik çalıştırma ayrıntılarını görüntülemek veya ardışık hattı yeniden çalıştırmak için boru hattı adı bağlantısı **DeltaCopyFromBlobPipeline'ı** tıklatın. Listeyi yenilemek için **Yenile**’yi seçin.

    ![İşlem hattı çalıştırmalarını izleme](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs1.png)
11. İşlem hattında yalnızca bir etkinlik (kopyalama etkinliği) olduğundan tek bir girdi görürsünüz. **Kaynak** ve **hedef** sütunların sütun genişliğini (gerekirse) daha fazla ayrıntı görüntülemek için ayarlayarak, kaynak dosyanın (file1.txt) *kaynak/2020/03/17/03/* adresinden *hedef/2020/03/17/03/* olarak aynı dosya adı ile kopyalandığını görebilirsiniz. 

    ![İşlem hattı çalıştırmalarını izleme](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs2.png)

    Dosyaları tetmekhttps://storageexplorer.com/) için Azure Depolama Gezgini'ni kullanarak da aynı şeyi doğrulayabilirsiniz.

    ![İşlem hattı çalıştırmalarını izleme](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs3.png)

12. **File2.txt**olarak yeni adı olan başka bir boş metin dosyası oluşturun. File2.txt dosyasını depolama hesabınızdaki klasör yolu **kaynağı/2020/03/17/04'e** yükleyin. Bu görevleri [Azure Depolama Gezgini](https://storageexplorer.com/) gibi çeşitli araçlar kullanarak gerçekleştirebilirsiniz.

    > [!NOTE]
    > Yeni bir klasör yolunun oluşturulması gerektiğini biliyor olabilirsiniz. Lütfen klasör adını UTC saatinizle ayarlayın.  Örneğin, geçerli UTC saati 17 Mart 2020 tarihinde 04:20 ise, **{Year}/{Month}/{Day}/{Hour}/** kuralıyla klasör yolunu **kaynak/2020/03/17/04/** olarak oluşturabilirsiniz.

13. **Pipeline Runs** görünümüne geri dönmek için, **Tüm Ardışık Hatlar çalışır'ı**seçin ve aynı ardışık etkinleştirilmiş ardışık ardışık bir saat sonra otomatik olarak yeniden tetiklenmesini bekleyin.  

    ![İşlem hattı çalıştırmalarını izleme](./media/tutorial-incremental-copy-partitioned-file-name-copy-data-tool/monitor-pipeline-runs5.png)

14. Geldiğinde ikinci boru hattı çalıştırmak için yeni **DeltaCopyFromBlobPipeline** bağlantısını seçin ve ayrıntıları gözden geçirmek için aynı şeyi yapın. Kaynak dosyanın (file2.txt) **kaynak/2020/03/17/04/** **adresinden hedef/2020/03/17/04/** olarak aynı dosya adı ile kopyalandığını göreceksiniz. Ayrıca, hedef kapsayıcıdaki dosyaları tetmekhttps://storageexplorer.com/) için Azure Depolama **destination** Gezgini'ni kullanarak aynı şeyi doğrulayabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar
Azure üzerinde bir Spark kümesi kullanarak veri dönüştürme hakkında bilgi edinmek için aşağıdaki öğreticiye geçin:

> [!div class="nextstepaction"]
>[Bulutta Spark kümesi kullanarak verileri dönüştürme](tutorial-transform-data-spark-portal.md)
