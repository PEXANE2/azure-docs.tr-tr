---
title: Yeni ve güncelleştirilmiş dosyaları artımlı olarak kopyalamak için veri aracı
description: Bir Azure veri fabrikası oluşturun ve ardından LastModifiedDate'e dayalı olarak yeni dosyaları artımlı olarak yüklemek için Veri Kopyalama aracını kullanın.
services: data-factory
author: dearandyxu
ms.author: yexu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.devlang: na
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 3/18/2020
ms.openlocfilehash: 743f9dd8f7998178a75d716f4da22efee2b3bc79
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80131227"
---
# <a name="incrementally-copy-new-and-changed-files-based-on-lastmodifieddate-by-using-the-copy-data-tool"></a>Copy Data aracını kullanarak LastModifiedDate'e dayalı yeni ve değiştirilmiş dosyaları artımlı olarak kopyalayın

Bu eğitimde, bir veri fabrikası oluşturmak için Azure portalını kullanırsınız. Ardından, Azure Blob depolama alanından Azure Blob depolamasına **kadar olan LastModifiedDate'ini** temel alan, yalnızca yeni ve değiştirilmiş dosyaları artımlı olarak kopyalayan bir ardışık kaynak oluşturmak için Verileri Kopyala aracını kullanırsınız.

Bunu yaparak, ADF kaynak deposundan tüm dosyaları tarayın, lastmodifieddate tarafından dosya filtresi uygulamak ve hedef deposuna son kez sadece son kez yeni ve güncelleştirilmiş dosya kopyalamak.  ADF'nin büyük miktarda dosyayı taramasına izin verirseniz ancak yalnızca birkaç dosyayı hedefe kopyalamanıza izin verirseniz, dosya taraması nedeniyle uzun sürenin de zaman alacağını unutmayın.   

> [!NOTE]
> Azure Data Factory kullanmaya yeni başlıyorsanız bkz. [Azure Data Factory'ye giriş](introduction.md).

Bu öğreticide, aşağıdaki görevleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Veri Kopyalama aracını kullanarak bir işlem hattı oluşturun.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* **Azure depolama hesabı**: _Kaynak_ ve _lavabo_ veri deposu olarak Blob depolamasını kullanın. Azure depolama hesabınız yoksa [Depolama hesabı oluşturma](../storage/common/storage-account-create.md) bölümündeki yönergelere bakın.

### <a name="create-two-containers-in-blob-storage"></a>Blob depolama alanında iki kapsayıcı oluşturma

Bu adımları gerçekleştirerek Blob depolama nızı öğreticiye hazırlayın.

1. **Kaynak**adlı bir kapsayıcı oluşturun. Bu görevi gerçekleştirmek için Azure Depolama [Gezgini](https://storageexplorer.com/)gibi çeşitli araçlar kullanabilirsiniz.

2. **Hedef**adlı bir kapsayıcı oluşturun.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Sol menüde > kaynak**Veri + Analitik** > **Veri Fabrikası** **Oluştur'u**seçin:

   ![“Yeni” bölmesinde Data Factory seçimi](./media/doc-common-process/new-azure-data-factory-menu.png)

2. **Yeni veri fabrikası** sayfasında **Ad** bölümüne **ADFTutorialDataFactory** girin.

   Veri fabrikanızın adı _genel olarak benzersiz_ olmalıdır. Aşağıdaki hata iletisini alabilirsiniz:

   ![Yeni veri fabrikası hata iletisi](./media/doc-common-process/name-not-available-error.png)

   Ad değeriyle ilgili bir hata iletisi alırsanız, veri fabrikası için farklı bir ad girin. Örneğin, _**adınız**_**ADFTutorialDataFactory** adını kullanın. Data Factory yapıtlarını adlandırma kuralları için bkz. [Data Factory adlandırma kuralları](naming-rules.md).
3. Yeni veri fabrikasını oluşturacağınız Azure **aboneliğini** seçin.
4. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:

    * **Varolan alanı kullan'ı** seçin ve açılan listeden varolan bir kaynak grubu seçin.

    * **Yeni Oluştur'u** seçin ve bir kaynak grubunun adını girin. 
         
    Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/management/overview.md).

5. **Sürüm**altında, **V2**seçin.
6. **Konum** bölümünde veri fabrikasının konumunu seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri depolama (örneğin, Azure Depolama ve SQL Veritabanı) ve veri fabrikanızın kullandığı hesaplamalar (örneğin, Azure HDInsight) başka konumlarda ve bölgelerde olabilir.
8. **Oluştur'u**seçin.
9. Oluşturma işlemi tamamlandıktan sonra **Data Factory** giriş sayfası görüntülenir.
10. Azure Veri Fabrikası kullanıcı arabirimini (UI) ayrı bir sekmede açmak için **Yazar & Monitör** döşemesini seçin.

    ![Data factory giriş sayfası](./media/doc-common-process/data-factory-home-page.png)

## <a name="use-the-copy-data-tool-to-create-a-pipeline"></a>Veri Kopyalama aracını kullanarak işlem hattı oluşturma

1. **Başlatalım** sayfasında, Verileri Kopyala aracını açmak için **Verileri Kopyala** başlığını seçin.

   ![Veri Kopyalama aracının kutucuğu](./media/doc-common-process/get-started-page.png)

2. **Özellikler** sayfasında aşağıdaki adımları izleyin:

    a. **Görev adı altında,** **DeltaCopyFromBlobPipeline**girin.

    b. **Görev cadence** veya **Görev zamanlaması**altında, **zamanlamaya göre düzenli olarak Çalıştır'ı**seçin.

    c. **Tetikleyici Türü**altında, **Yuvarlanan Pencere'yi**seçin.

    d. **Yineleme**altında, **15 Dakika(lar)** girin.

    e. **Sonraki'ni**seçin.

    Data Factory kullanıcı arabirimi, belirtilen görev adına sahip bir işlem hattı oluşturur.

    ![Özellikler sayfası](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/copy-data-tool-properties-page.png)

3. **Kaynak veri deposu** sayfasında aşağıdaki adımları tamamlayın:

    a. Bağlantı eklemek için **+ Yeni bağlantı oluşturun'** seçeneğini belirleyin.

    b. Galeriden **Azure Blob Depolama'yı** seçin ve ardından **Devam et'i**seçin.

    ![Kaynak veri deposu sayfası](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/source-data-store-page-select-blob.png)

    c. Yeni **Bağlantılı Hizmet (Azure Blob Depolama)** sayfasında, **Depolama hesabı ad** listesinden depolama hesabınızı seçin. Bağlantıyı test edin ve ardından **Oluştur'u**seçin.

    d. Yeni oluşturulan bağlantılı hizmeti seçin ve sonra **İleri'yi**seçin.

   ![Kaynak veri deposu sayfası](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/source-data-store-page-select-linkedservice.png)

4. **Girdi dosyasını veya klasörünü seçin** sayfasında aşağıdaki adımları uygulayın:

    a. Kaynak **klasöre** göz atın ve seçin ve sonra **Seç'i**seçin.

    ![Girdi dosyası veya klasörü seçin](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/choose-input-file-folder.png)

    b. **Dosya yükleme davranışı**altında, **Artımlı yük seçin: LastModifiedDate**.

    c. **İkili kopyayı** denetleyin ve **İleri'yi**seçin.

     ![Girdi dosyası veya klasörü seçin](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/check-binary-copy.png)

5. Hedef **veri deposu** sayfasında, oluşturduğunuz **AzureBlobStorage'ı** seçin. Bu, kaynak veri deposuyla aynı depolama hesabıdır. Ardından **İleri'yi**seçin.

6. **Çıktı dosyasını veya klasörünü seçin** sayfasında aşağıdaki adımları uygulayın:

    a. Göz atın ve **hedef** klasörü seçin ve sonra **Seçin'i**seçin.

    ![Çıktı dosyasını veya klasörünü seçin](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/choose-output-file-folder.png)

    b. **Sonraki'ni**seçin.

7. **Ayarlar** sayfasında **İleri**’yi seçin.

8. **Özet** sayfasında, ayarları gözden geçirin ve **sonra İleri'yi**seçin.

    ![Özet sayfası](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/summary-page.png)

9. **Dağıtım** sayfasında, işlem hattını (görev) izlemek için **İzleyici**’yi seçin.

    ![Dağıtım sayfası](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/deployment-page.png)

10. Soldaki **İzleyici** sekmesinin otomatik olarak seçildiğine dikkat edin. Uygulama **Monitör** sekmesine geçer. Boru hattının durumunu görüyorsunuz. Listeyi yenilemek için **Yenile**’yi seçin. Etkinlik çalıştırma ayrıntılarını görüntülemek veya ardışık hattı yeniden çalıştırmak için **PIPELINE NAME** altındaki bağlantıyı tıklatın. 

    ![Listeyi yenile ve Etkinlik Çalıştırmalarını Görüntüle'yi seçin](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs1.png)

11. Ardışık alanda yalnızca bir etkinlik (kopyalama etkinliği) vardır, bu nedenle yalnızca bir giriş görürsünüz. Kopyalama işlemi yle ilgili ayrıntılar için **ACTIVITY NAME** sütunundaki **Ayrıntılar** bağlantısını (gözlük simgesi) seçin. Özelliklerle ilgili ayrıntılar için bkz. [Kopyalama Etkinliğine genel bakış](copy-activity-overview.md). 

    ![Kopyalama etkinliği boru hattında](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs2.png)

    Blob depolama hesabınızdaki **kaynak** kapsayıcıda dosya olmadığından, Blob depolama hesabınızda **hedef** kapsayıcıya kopyalanan hiçbir dosya görmezsiniz.

    ![Kaynak kapsayıcıda veya hedef kapsayıcıda dosya yok](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs3.png)

12. Boş bir metin dosyası oluşturun ve **dosya1.txt**. Bu metin dosyasını depolama hesabınızdaki **kaynak** kapsayıcıya yükleyin. Bu görevleri [Azure Depolama Gezgini](https://storageexplorer.com/) gibi çeşitli araçlar kullanarak gerçekleştirebilirsiniz.

    ![file1.txt oluşturun ve kaynak kapsayıcıya yükleyin](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs3-1.png)

13. **Pipeline Runs** görünümüne geri dönmek **için, Tüm ardışık hatlar çalışır'ı**seçin ve aynı ardışık etkinleştirilmiş ardışık ardışık ardışık yeniden tetiklenecek bekleyin.  

    ![Tüm Boru Hattı Çalıştırmalarını Seçin](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs4.png)

14. İkinci ardışık iş tonu tamamlandığında, etkinlik çalıştırma ayrıntılarını gözden geçirmek için yukarıda belirtilen adımları izleyin.  

    Bir dosyanın (file1.txt) **kaynak** kapsayıcıdan Blob depolama hesabınızın **hedef** kapsayıcısına kopyalandığını görürsünüz.

    ![File1.txt kaynak kapsayıcıdan hedef kapsayıcıya kopyalandı](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs6.png)

15. Başka bir boş metin dosyası oluşturun ve **dosya2.txt**adını. Bu metin dosyasını Blob depolama hesabınızdaki **kaynak** kapsayıcıya yükleyin.

16. Bu ikinci metin dosyası için 13 ve 14 adımlarını yineleyin. Yalnızca yeni dosyanın (file2.txt) **kaynak** kapsayıcıdan bir sonraki ardışık iş takındaki depolama hesabınızın **hedef** kapsayıcısına kopyalandığını görürsünüz.  

    Dosyaları tetmek için [Azure Depolama Gezgini'ni](https://storageexplorer.com/) kullanarak da bunu doğrulayabilirsiniz.

    ![Azure Depolama Gezgini'ni kullanarak dosyaları tarayın](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs8.png)


## <a name="next-steps"></a>Sonraki adımlar
Azure'da bir Apache Spark kümesi kullanarak verileri dönüştürme hakkında bilgi edinmek için aşağıdaki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
>[Apache Spark kümesini kullanarak buluttaki verileri dönüştürme](tutorial-transform-data-spark-portal.md)
