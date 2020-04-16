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
ms.openlocfilehash: 3098ca0d3d5e41c298d3058ffa84fcf129648281
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81399480"
---
# <a name="incrementally-copy-new-and-changed-files-based-on-lastmodifieddate-by-using-the-copy-data-tool"></a>Copy Data aracını kullanarak LastModifiedDate'e dayalı yeni ve değiştirilmiş dosyaları artımlı olarak kopyalayın

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Bu eğitimde, bir veri fabrikası oluşturmak için Azure portalını kullanırsınız. Daha sonra, Azure Blob depolamadan Azure Blob depolamasına kadar yalnızca yeni ve değiştirilmiş dosyaları artımlı olarak kopyalayan bir ardışık kaynak oluşturmak için Verileri Kopyala aracını kullanırsınız. Hangi `LastModifiedDate` dosyaların kopyalanamayacağına karar vermek için kullanır.

Buradaki adımları tamamladıktan sonra, Azure Veri Fabrikası kaynak mağazadaki tüm dosyaları tarar, dosya filtresini `LastModifiedDate`uygular ve yalnızca yeni veya son kez güncelleştirilen dosyaları hedef depoya kopyalar. Veri Fabrikası çok sayıda dosya tararsa, yine de uzun süreler beklemeniz gerektiğini unutmayın. Kopyalanan veri miktarı azaldığında bile dosya taraması zaman alır.

> [!NOTE]
> İlk kez Data Factory kullanıyorsanız bkz. [Azure Data Factory'ye giriş](introduction.md).

Bu öğreticide, şu görevleri tamamlayasınız:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Veri Kopyalama aracını kullanarak bir işlem hattı oluşturun.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* **Azure Depolama hesabı**: Kaynak ve lavabo veri depoları için Blob depolamasını kullanın. Azure Depolama hesabınız yoksa, [depolama hesabı oluştur'daki](../storage/common/storage-account-create.md)yönergeleri izleyin.

## <a name="create-two-containers-in-blob-storage"></a>Blob depolama alanında iki kapsayıcı oluşturma

Bu adımları tamamlayarak Blb depolamanızı öğreticiye hazırlayın:

1. **Kaynak**adlı bir kapsayıcı oluşturun. Bu görevi gerçekleştirmek için [Azure Depolama Gezgini](https://storageexplorer.com/)gibi çeşitli araçlar kullanabilirsiniz.

2. **Hedef**adlı bir kapsayıcı oluşturun.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Sol bölmede **Kaynak oluştur**'u seçin. **Analitik** > **Veri Fabrikası'nı**seçin:

   ![Veri Fabrikası'nı seçin](./media/doc-common-process/new-azure-data-factory-menu.png)

2. **Yeni veri fabrikası** sayfasında **Ad** bölümüne **ADFTutorialDataFactory** girin.

   Veri fabrikanızın adı genel olarak benzersiz olmalıdır. Bu hata iletisi alabilirsiniz:

   ![Ad yok hata iletisi](./media/doc-common-process/name-not-available-error.png)

   Ad değeriyle ilgili bir hata iletisi alırsanız, veri fabrikası için farklı bir ad girin. Örneğin, _**adınız**_**ADFTutorialDataFactory** adını kullanın. Data Factory yapıtlarını adlandırma kuralları için bkz. [Data Factory adlandırma kuralları](naming-rules.md).
3. **Abonelik**altında, yeni veri fabrikasını oluşturacağınız Azure aboneliğini seçin.
4. **Kaynak Grubu**altında, şu adımlardan birini atın:

    * **Varolan Kullan'ı** seçin ve ardından listede varolan bir kaynak grubunu seçin.

    * **Yeni Oluştur'u** seçin ve kaynak grubu için bir ad girin.
         
    Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/management/overview.md).

5. **Sürüm** bölümünde **V2**'yi seçin.
6. **Konum**altında, veri fabrikasının konumunu seçin. Listede yalnızca desteklenen konumlar görünür. Veri depoları (örneğin, Azure Depolama ve Azure SQL Veritabanı) ve veri fabrikanızın kullandığı hesaplamalar (örneğin, Azure HDInsight) başka konumlarda ve bölgelerde olabilir.
8. **Oluştur**’u seçin.
9. Veri fabrikası oluşturulduktan sonra, veri fabrikası ana sayfası görüntülenir.
10. Azure Veri Fabrikası kullanıcı arabirimini (UI) ayrı bir sekmede açmak için **Yazar & Monitör** döşemesini seçin:

    ![Data factory giriş sayfası](./media/doc-common-process/data-factory-home-page.png)

## <a name="use-the-copy-data-tool-to-create-a-pipeline"></a>Veri Kopyalama aracını kullanarak işlem hattı oluşturma

1. **Başlatalım** sayfasında, Verileri **Kopyala** aracını açmak için Veri Kopyala'yı seçin:

   ![Veri döşemesi kopyala](./media/doc-common-process/get-started-page.png)

2. **Özellikler** sayfasında aşağıdaki adımları izleyin:

    a. **Görev adı altında,** **DeltaCopyFromBlobPipeline**girin.

    b. **Görev cadence veya Görev zamanlaması**altında, **zamanlamaya göre düzenli olarak Çalıştır'ı**seçin.

    c. **Tetik leme türü**altında, **Yuvarlanma penceresini**seçin.

    d. **Yineleme**altında, **15 Dakika(lar)** girin.

    e. **İleri**’yi seçin.

    Veri Fabrikası belirtilen görev adını içeren bir ardışık hatlar oluşturur.

    ![Veri özellikleri sayfasını kopyalama](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/copy-data-tool-properties-page.png)

3. Kaynak **veri deposu** sayfasında aşağıdaki adımları tamamlayın:

    a. Bağlantı eklemek için **yeni bağlantı oluştur'u** seçin.

    b. Galeriden **Azure Blob Depolama'yı** seçin ve ardından **Devam et'i**seçin:

    ![Azure Blog Depolama'yı seçin](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/source-data-store-page-select-blob.png)

    c. Yeni **Bağlantılı Hizmet (Azure Blob Depolama)** sayfasında, **Depolama hesabı ad** listesinden depolama hesabınızı seçin. Bağlantıyı test edin ve ardından **Oluştur'u**seçin.

    d. Yeni bağlantılı hizmeti seçin ve sonra **İleri'yi**seçin:

   ![Yeni bağlantılı hizmeti seçin](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/source-data-store-page-select-linkedservice.png)

4. **Girdi dosyasını veya klasörünü seçin** sayfasında aşağıdaki adımları uygulayın:

    a. Kaynak **klasöre** göz atın ve seçin ve sonra **Seç'i**seçin.

    ![Girdi dosyası veya klasörü seçin](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/choose-input-file-folder.png)

    b. **Dosya yükleme davranışı**altında, **Artımlı yük seçin: LastModifiedDate**.

    c. **İkili kopyayı** seçin ve **sonra İleri'yi**seçin:

     ![Giriş dosyasını veya klasör sayfasını seçin](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/check-binary-copy.png)

5. Hedef **veri deposu** sayfasında, oluşturduğunuz **AzureBlobStorage** hizmetini seçin. Bu, kaynak veri deposuyla aynı depolama hesabıdır. Ardından **İleri'yi**seçin.

6. **Çıktı dosyasını veya klasörünü seçin** sayfasında aşağıdaki adımları uygulayın:

    a. Göz atın ve **hedef** klasörü seçin ve sonra **Seçin**seçin:

    ![Çıktı dosyasını veya klasör sayfasını seçin](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/choose-output-file-folder.png)

    b. **İleri**’yi seçin.

7. **Ayarlar** sayfasında **İleri**’yi seçin.

8. **Özet** sayfasında, ayarları gözden geçirin ve **sonra İleri'yi**seçin.

    ![Özet sayfası](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/summary-page.png)

9. **Dağıtım** sayfasında, işlem hattını (görev) izlemek için **İzleyici**’yi seçin.

    ![Dağıtım sayfası](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/deployment-page.png)

10. Soldaki **İzleyici** sekmesinin otomatik olarak seçildiğine dikkat edin. Uygulama **Monitör** sekmesine geçer. Boru hattının durumunu görüyorsunuz. Listeyi yenilemek için **Yenile**’yi seçin. Etkinlik çalıştırma ayrıntılarını görüntülemek veya ardışık hattı yeniden çalıştırmak için **PIPELINE NAME** altındaki bağlantıyı seçin.

    ![Listeyi yenileyin ve etkinlik çalıştırma ayrıntılarını görüntüleyin](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs1.png)

11. Ardışık alanda yalnızca bir etkinlik (kopyalama etkinliği) vardır, bu nedenle yalnızca bir giriş görürsünüz. Kopyalama işlemi yle ilgili ayrıntılar için **ACTIVITY NAME** sütunundaki **Ayrıntılar** bağlantısını (gözlük simgesi) seçin. Özellikler le ilgili ayrıntılar [için](copy-activity-overview.md)bkz.

    ![Ardışık hatlar'daki kopyalama etkinliği](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs2.png)

    Blob depolama hesabınızdaki kaynak kapsayıcıda dosya olmadığından, hesaptaki hedef kapsayıcıya kopyalanan dosyaları görmezsiniz:

    ![Kaynak kapsayıcıda veya hedef kapsayıcıda dosya yok](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs3.png)

12. Boş bir metin dosyası oluşturun ve **dosya1.txt**. Bu metin dosyasını depolama hesabınızdaki kaynak kapsayıcıya yükleyin. [Azure Depolama Gezgini](https://storageexplorer.com/)gibi bu görevleri gerçekleştirmek için çeşitli araçlar kullanabilirsiniz.

    ![file1.txt oluşturun ve kaynak kapsayıcıya yükleyin](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs3-1.png)

13. **Pipeline çalışır** görünümüne geri dönmek **için, Tüm ardışık hatlar çalışır**seçin ve aynı ardışık otomatik olarak yeniden tetiklenecek bekleyin.  

    ![Tüm ardışık hatlar çalışır seçin](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs4.png)

14. İkinci ardışık iş tonu tamamlandığında, etkinlik çalıştırma ayrıntılarını gözden geçirmek için daha önce bahsedilen adımları izleyin.  

    Bir dosyanın (file1.txt) kaynak kapsayıcıdan Blob depolama hesabınızın hedef kapsayıcısına kopyalandığını görürsünüz:

    ![file1.txt kaynak kapsayıcıdan hedef kapsayıcıya kopyalanmıştır](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs6.png)

15. Başka bir boş metin dosyası oluşturun ve **dosya2.txt**adını. Bu metin dosyasını Blob depolama hesabınızdaki kaynak kapsayıcıya yükleyin.

16. İkinci metin dosyası için 13 ve 14 adımlarını yineleyin. Bu ardışık işlem sırasında kaynak kapsayıcıdan depolama hesabınızın hedef konteynerine yalnızca yeni dosyanın (file2.txt) kopyalandığını görürsünüz.  

    Dosyaları tkişik etmek için Azure Depolama [Gezgini](https://storageexplorer.com/) kullanılarak yalnızca bir dosyanın kopyalandığını da doğrulayabilirsiniz:

    ![Azure Depolama Gezgini kullanarak dosyaları tarayın](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs8.png)


## <a name="next-steps"></a>Sonraki adımlar
Azure'da bir Apache Spark kümesi kullanarak verileri nasıl dönüştüreceğinizi öğrenmek için aşağıdaki öğreticiye gidin:

> [!div class="nextstepaction"]
>[Apache Spark kümesini kullanarak buluttaki verileri dönüştürme](tutorial-transform-data-spark-portal.md)
