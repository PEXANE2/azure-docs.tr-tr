---
title: Azure Data Lake Storage 2. Nesil'e verileri yükleme
description: Verileri Azure Veri Gölü Depolama Gen2'ye kopyalamak için Azure Veri Fabrikası'nı kullanın
services: data-factory
documentationcenter: ''
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 05/13/2019
ms.openlocfilehash: 90573f77c77d614923f882053145d2f84598953d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75440228"
---
# <a name="load-data-into-azure-data-lake-storage-gen2-with-azure-data-factory"></a>Azure Veri Fabrikası ile Verileri Azure Veri Gölü Depolama Gen2'ye yükleyin

Azure Veri Gölü Depolama Gen2, [Azure Blob depolamaalanına](../storage/blobs/storage-blobs-introduction.md)yerleşik olarak büyük veri analitiğine adanmış bir dizi özelliktir. Hem dosya sistemini hem de nesne depolama paradigmalarını kullanarak verilerinizle arayüz aranızda bulunmanızı sağlar.

Azure Veri Fabrikası (ADF), bulut tabanlı tam olarak yönetilen bir veri tümleştirme hizmetidir. Hizmeti, gölü zengin şirket içi ve bulut tabanlı veri depolarından gelen verilerle doldurmak ve analitik çözümlerinizi yaparken zamandan tasarruf etmek için kullanabilirsiniz. Desteklenen bağlayıcıların ayrıntılı bir listesi için Desteklenen [veri depolarının](copy-activity-overview.md#supported-data-stores-and-formats)tablosuna bakın.

Azure Veri Fabrikası, ölçeklendirilen, yönetilen bir veri hareketi çözümü sunar. ADF'nin ölçeklendirme mimarisi sayesinde, verileri yüksek bir iş seviyesinde yutabilir. Ayrıntılar için [bkz.](copy-activity-performance.md)

Bu makalede, _Amazon Web Hizmetleri S3 hizmetinden_ verileri _Azure Veri Gölü Depolama Gen2'ye_yüklemek için Veri Fabrikası Kopyalama Veri aracını nasıl kullanacağınızı gösterilmektedir. Diğer veri depolarından veri kopyalamak için benzer adımları izleyebilirsiniz.

>[!TIP]
>Azure Veri Gölü Depolama Gen1'deki verileri Gen2'ye kopyalamak için [bu özel gözden geçirme ye](load-azure-data-lake-storage-gen2-from-gen1.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği: Azure aboneliğiniz yoksa, başlamadan önce ücretsiz bir [hesap](https://azure.microsoft.com/free/) oluşturun.
* Veri Gölü Depolama Gen2 etkinleştirilmiş Azure Depolama hesabı: Depolama hesabınız [yoksa, bir hesap oluşturun.](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM)
* Veri içeren bir S3 kova ile AWS hesabı: Bu makalede Amazon S3 verileri kopyalamak için nasıl gösterir. Benzer adımları izleyerek diğer veri depolarını kullanabilirsiniz.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Sol menüde > kaynak**Veri + Analitik** > **Veri Fabrikası** **Oluştur'u**seçin:
   
   ![“Yeni” bölmesinde Data Factory seçimi](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

2. Yeni **veri fabrikası** sayfasında, aşağıdaki resimde gösterilen alanlar için değerler sağlayın: 
      
   ![Yeni veri fabrikası sayfası](./media/load-azure-data-lake-storage-gen2//new-azure-data-factory.png)
 
    * **Ad**: Azure veri fabrikanız için benzersiz bir ad girin. "Veri fabrikası adı \"LoadADLSDemo\" kullanılamıyor" hatasını alırsanız, veri fabrikası için farklı bir ad girin. Örneğin, _**adınızı**_**ADFTutorialDataFactory**kullanabilirsiniz. Veri fabrikasını yeniden oluşturmayı deneyin. Data Factory yapıtlarını adlandırma kuralları için bkz. [Data Factory adlandırma kuralları](naming-rules.md).
    * **Abonelik**: Veri fabrikasını oluşturmak için Azure aboneliğinizi seçin. 
    * **Kaynak Grubu**: Açılan listeden varolan bir kaynak grubu seçin veya **Yeni Oluştur** seçeneğini belirleyin ve bir kaynak grubunun adını girin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/management/overview.md).  
    * **Sürüm**: **V2**seçin.
    * **Konum**: Veri fabrikasının konumunu seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan veri depoları başka konumlarda ve bölgelerde olabilir. 

3. **Oluştur'u**seçin.
4. Oluşturma tamamlandıktan sonra veri fabrikanıza gidin. Aşağıdaki resimde gösterildiği gibi **Veri Fabrikası** ana sayfasını görürsünüz: 
   
   ![Data factory giriş sayfası](./media/load-azure-data-lake-storage-gen2/data-factory-home-page.png)

   Veri Tümleştirme Uygulamasını ayrı bir sekmede açmak için **Oluştur ve İzle** kutucuğunu seçin.

## <a name="load-data-into-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage 2. Nesil'e verileri yükleme

1. **Başlat'ın sayfasında,** Verileri **Kopyala** aracını başlatmak için Veri Kopyala'yı seçin: 

   ![Veri Kopyalama aracının kutucuğu](./media/load-azure-data-lake-storage-gen2/copy-data-tool-tile.png)
2. **Özellikler** sayfasında, **Görev adı** alanı için **CopyFromAmazonS3ToADLS** belirtin ve **Sonraki'yi**seçin:

    ![Özellikler sayfası](./media/load-azure-data-lake-storage-gen2/copy-data-tool-properties-page.png)
3. Kaynak **veri deposu** sayfasında + **Yeni bağlantı oluştur'u**tıklatın:

    ![Kaynak veri deposu sayfası](./media/load-azure-data-lake-storage-gen2/source-data-store-page.png)
    
    Bağlayıcı galerisinden **Amazon S3'ünü** seçin ve **Devam et'i** seçin
    
    ![Kaynak veri deposu s3 sayfası](./media/load-azure-data-lake-storage-gen2/source-data-store-page-s3.png)
    
4. Amazon **S3 bağlantı** sayfasını belirtin, aşağıdaki adımları yapın:

   1. Access **Tuşu Kimliği** değerini belirtin.
   2. Gizli **Erişim Anahtarı** değerini belirtin.
   3. Ayarları doğrulamak için **Bağlantı testi**'ne tıklayın ve **Son**'u seçin.
   4. Yeni bir bağlantı nın oluşturulduğunu göreceksiniz. **Sonraki'ni**seçin.
   
      ![Amazon S3 hesabını belirtin](./media/load-azure-data-lake-storage-gen2/specify-amazon-s3-account.png)
      
5. **Giriş dosyasını veya klasörünü seçin** sayfasında, üzerine kopyalamak istediğiniz klasöre ve dosyaya göz atın. Klasörü/dosyayı seçin, **Seç'i**seçin:

    ![Girdi dosyasını veya klasörünü seçin](./media/load-azure-data-lake-storage-gen2/choose-input-folder.png)

6. **Dosyaları yinelemeli olarak kopyala** ve **İkili kopya** seçeneklerini işaretleyerek kopyalama davranışını belirtin. **Sonraki'ni**Seçin :

    ![Çıktı klasörünü belirtin](./media/load-azure-data-lake-storage-gen2/specify-binary-copy.png)
    
7. Hedef **veri deposu** sayfasında **+ Yeni bağlantı oluştur'u**tıklatın ve ardından **Azure Veri Gölü Depolama Gen2'yi**seçin ve Devam et seçeneğini **belirleyin:**

    ![Hedef veri deposu sayfası](./media/load-azure-data-lake-storage-gen2/destination-data-storage-page.png)

8. Azure **Veri Gölü Depolama bağlantı** sayfasını belirt inde, aşağıdaki adımları yapın:

   1. "Depolama hesabı adı" açılır listesinden Veri Gölü Depolama Gen2 özellikli hesabınızı seçin.
   2. **Son**'u seçerek bağlantıyı oluşturun. Ardından **İleri'yi**seçin.
   
   ![Azure Veri Gölü Depolama Gen2 hesabını belirtin](./media/load-azure-data-lake-storage-gen2/specify-adls.png)

9. Çıktı **dosyasını veya klasörsayfasını seç,** çıktı klasöradı olarak **copyfroms3'ü** girin ve **İleri'yi**seçin. ADF, varsa kopyalama sırasında ilgili ADLS Gen2 dosya sistemini ve alt klasörleri oluşturur.

    ![Çıktı klasörünü belirtin](./media/load-azure-data-lake-storage-gen2/specify-adls-path.png)

10. **Ayarlar** sayfasında, varsayılan ayarları kullanmak için **Sonraki'ni** seçin:

    ![Ayarlar sayfası](./media/load-azure-data-lake-storage-gen2/copy-settings.png)
11. **Özet** sayfasında, ayarları gözden geçirin ve **Sonraki'ni**seçin:

    ![Özet sayfası](./media/load-azure-data-lake-storage-gen2/copy-summary.png)
12. Dağıtım **sayfasında,** ardışık izlemeyi izlemek için **Monitör'ü** seçin:

    ![Dağıtım sayfası](./media/load-azure-data-lake-storage-gen2/deployment-page.png)
13. Soldaki **İzleyici** sekmesinin otomatik olarak seçildiğine dikkat edin. **Eylemler** sütunu, etkinlik çalıştırma ayrıntılarını görüntülemek ve ardışık hattı yeniden çalıştırmak için bağlantılar içerir:

    ![İşlem hattı çalıştırmalarını izleme](./media/load-azure-data-lake-storage-gen2/monitor-pipeline-runs.png)

14. Ardışık işlem hattı çalışmasıyla ilişkili etkinlik çalışanlarını görüntülemek **için, Eylemler** sütunundaki **Etkinlik Çalışır'ı Görüntüle** bağlantısını seçin. İşlem hattında yalnızca bir etkinlik (kopyalama etkinliği) olduğundan tek bir girdi görürsünüz. Ardışık hatlar çalışır görünümüne geri dönmek için, üstteki **Ardışık Hatlar** bağlantısını seçin. Listeyi yenilemek için **Yenile**’yi seçin. 

    ![Etkinlik çalıştırmalarını izleme](./media/load-azure-data-lake-storage-gen2/monitor-activity-runs.png)

15. Her kopyalama etkinliğinin yürütme ayrıntılarını izlemek için, etkinlik izleme görünümündeki **Eylemler** altında **Ayrıntılar** bağlantısını (gözlük görüntüsü) seçin. Kaynaktan lavaboya kopyalanan veri hacmi, veri aktarımı, ilgili süreye sahip yürütme adımları ve kullanılan yapılandırmalar gibi ayrıntıları izleyebilirsiniz:

    ![Etkinlik çalıştırma ayrıntılarını izleme](./media/load-azure-data-lake-storage-gen2/monitor-activity-run-details.png)

16. Verilerin Veri Gölü Depolama Gen2 hesabınıza kopyalandığından doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
* [Azure Data Lake Storage 2. Nesil bağlayıcısı](connector-azure-data-lake-storage.md)
