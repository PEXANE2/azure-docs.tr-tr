---
title: Azure Data Lake Storage 2. Nesil'e verileri yükleme
description: Verileri Azure Data Lake Storage 2. kopyalamak için Azure Data Factory kullanma
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
ms.openlocfilehash: 96674f059e9cbc21c5c8c64eff8c94c810c4aa32
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81417785"
---
# <a name="load-data-into-azure-data-lake-storage-gen2-with-azure-data-factory"></a>Azure Data Factory ile Azure Data Lake Storage 2. verileri yükleme

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Azure Data Lake Storage 2., [Azure Blob depolamada](../storage/blobs/storage-blobs-introduction.md)yerleşik olan, büyük veri analizi için adanmış bir özellik kümesidir. Hem dosya sistemi hem de nesne depolama paradigmalarına kullanarak verilerinize arabirim oluşturmanızı sağlar.

Azure Data Factory (ADF), tam olarak yönetilen bulut tabanlı bir veri tümleştirme hizmetidir. Bu hizmeti kullanarak Gölü zengin şirket içi ve bulut tabanlı veri mağazalarındaki verilerle doldurabilir ve analiz çözümlerinizi derlerken zamandan tasarruf edebilirsiniz. Desteklenen bağlayıcıların ayrıntılı bir listesi için [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats)tablosuna bakın.

Azure Data Factory, genişleme, yönetilen bir veri taşıma çözümü sağlar. ADF 'nin ölçek genişletme mimarisi nedeniyle, verileri yüksek bir aktarım hızına alabilir. Ayrıntılar için bkz. [etkinlik performansını kopyalama](copy-activity-performance.md).

Bu makalede _Amazon Web Services S3 hizmetinden_ _Azure Data Lake Storage 2._'a veri yüklemek için Data Factory veri kopyalama aracının nasıl kullanılacağı gösterilmektedir. Diğer veri deposu türlerinden veri kopyalamak için benzer adımları izleyebilirsiniz.

>[!TIP]
>Azure Data Lake Storage 1. verileri Gen2 'e kopyalamak için [Bu özel izlenecek yolu](load-azure-data-lake-storage-gen2-from-gen1.md)inceleyin.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği: bir Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* Data Lake Storage 2. etkin Azure depolama hesabı: depolama hesabınız yoksa [bir hesap oluşturun](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM).
* Veri içeren bir S3 demetini olan AWS hesabı: Bu makalede, Amazon S3 'ten nasıl veri kopyalanacağı gösterilmektedir. Benzer adımları izleyerek başka veri depoları da kullanabilirsiniz.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Sol taraftaki menüden > **veri ve analiz** >  **kaynak oluştur**' u seçin**Data Factory**:
   
   ![“Yeni” bölmesinde Data Factory seçimi](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

2. **Yeni Veri Fabrikası** sayfasında, aşağıdaki görüntüde gösterilen alanlar için değerler girin: 
      
   ![Yeni veri fabrikası sayfası](./media/load-azure-data-lake-storage-gen2//new-azure-data-factory.png)
 
    * **Ad**: Azure Data Factory 'niz için genel olarak benzersiz bir ad girin. "Data Factory Name \"loadadlsdemo\" kullanılamıyor" hatasını alırsanız Veri Fabrikası için farklı bir ad girin. Örneğin _**, Name**_**ADFTutorialDataFactory**adını kullanabilirsiniz. Data Factory 'yi yeniden oluşturmayı deneyin. Data Factory yapıtlarını adlandırma kuralları için bkz. [Data Factory adlandırma kuralları](naming-rules.md).
    * **Abonelik**: veri fabrikasının oluşturulacağı Azure aboneliğinizi seçin. 
    * **Kaynak grubu**: açılan listeden var olan bir kaynak grubunu seçin veya **Yeni oluştur** seçeneğini belirleyin ve bir kaynak grubunun adını girin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/management/overview.md).  
    * **Sürüm**: **v2**'yi seçin.
    * **Konum**: veri fabrikasının konumunu seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Data Factory tarafından kullanılan veri depoları diğer konumlarda ve bölgelerde olabilir. 

3. **Oluştur**’u seçin.
4. Oluşturma işlemi tamamlandıktan sonra, veri fabrikanıza gidin. Aşağıdaki görüntüde gösterildiği gibi **Data Factory** giriş sayfasını görürsünüz: 
   
   ![Data factory giriş sayfası](./media/load-azure-data-lake-storage-gen2/data-factory-home-page.png)

   Veri Tümleştirme Uygulamasını ayrı bir sekmede açmak için **Oluştur ve İzle** kutucuğunu seçin.

## <a name="load-data-into-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage 2. Nesil'e verileri yükleme

1. **Başlarken** sayfasında, veri kopyalama aracını başlatmak için **veri kopyalama** kutucuğunu seçin: 

   ![Veri Kopyalama aracının kutucuğu](./media/load-azure-data-lake-storage-gen2/copy-data-tool-tile.png)
2. **Özellikler** sayfasında, **görev adı** alanı Için **CopyFromAmazonS3ToADLS** belirtin ve **İleri**' yi seçin:

    ![Özellikler sayfası](./media/load-azure-data-lake-storage-gen2/copy-data-tool-properties-page.png)
3. **Kaynak veri deposu** sayfasında **+ Yeni bağlantı oluştur**' a tıklayın:

    ![Kaynak veri deposu sayfası](./media/load-azure-data-lake-storage-gen2/source-data-store-page.png)
    
    Bağlayıcı galerisinden **Amazon S3** ' i seçin ve **devam** ' ı seçin.
    
    ![Kaynak veri deposu S3 sayfası](./media/load-azure-data-lake-storage-gen2/source-data-store-page-s3.png)
    
4. **Amazon S3 bağlantısını belirtin** sayfasında aşağıdaki adımları uygulayın:

   1. **Erişim anahtarı kimlik** değerini belirtin.
   2. Gizli dizi **erişim anahtarı** değerini belirtin.
   3. Ayarları doğrulamak için **Bağlantı testi**'ne tıklayın ve **Son**'u seçin.
   4. Yeni bir bağlantı oluşturulur. **İleri**’yi seçin.
   
      ![Amazon S3 hesabını belirtin](./media/load-azure-data-lake-storage-gen2/specify-amazon-s3-account.png)
      
5. **Giriş dosyasını veya klasörünü seçin** sayfasında, üzerine kopyalamak istediğiniz klasöre ve dosyaya göz atın. Klasör/dosya seçin **, Seç ' i seçin.**

    ![Girdi dosyasını veya klasörünü seçin](./media/load-azure-data-lake-storage-gen2/choose-input-folder.png)

6. **Dosyaları yinelemeli olarak kopyala** ve **İkili kopya** seçeneklerini işaretleyerek kopyalama davranışını belirtin. **İleri ' yi**seçin:

    ![Çıkış klasörünü belirtin](./media/load-azure-data-lake-storage-gen2/specify-binary-copy.png)
    
7. **Hedef veri deposu** sayfasında **+ Yeni bağlantı oluştur**' a ve ardından **Azure Data Lake Storage 2.**' ı seçip **devam**' ı seçin:

    ![Hedef veri deposu sayfası](./media/load-azure-data-lake-storage-gen2/destination-data-storage-page.png)

8. **Azure Data Lake Storage bağlantısını belirtin** sayfasında aşağıdaki adımları uygulayın:

   1. "Depolama hesabı adı" açılan listesinden Data Lake Storage 2. özellikli hesabınızı seçin.
   2. **Son**'u seçerek bağlantıyı oluşturun. Ardından **İleri**' yi seçin.
   
   ![Azure Data Lake Storage 2. hesabı belirtin](./media/load-azure-data-lake-storage-gen2/specify-adls.png)

9. **Çıktı dosyasını veya klasörünü seçin** sayfasında, çıkış klasörü adı olarak **Copyfroms3** girin ve **İleri**' yi seçin. ADF, varsa kopyalama sırasında karşılık gelen ADLS 2. dosya sistemi ve alt klasörleri oluşturur.

    ![Çıkış klasörünü belirtin](./media/load-azure-data-lake-storage-gen2/specify-adls-path.png)

10. **Ayarlar** sayfasında, varsayılan ayarları kullanmak için **İleri** ' yi seçin:

    ![Ayarlar sayfası](./media/load-azure-data-lake-storage-gen2/copy-settings.png)
11. **Özet** sayfasında, ayarları gözden geçirin ve **İleri**' yi seçin:

    ![Özet sayfası](./media/load-azure-data-lake-storage-gen2/copy-summary.png)
12. **Dağıtım sayfasında**, işlem hattını Izlemek için **izleyici** ' yi seçin:

    ![Dağıtım sayfası](./media/load-azure-data-lake-storage-gen2/deployment-page.png)
13. Soldaki **İzleyici** sekmesinin otomatik olarak seçildiğine dikkat edin. Eylemler sütunu, etkinlik çalıştırma ayrıntılarını görüntüleme ve **işlem** hattını yeniden çalıştırma bağlantılarını içerir:

    ![İşlem hattı çalıştırmalarını izleme](./media/load-azure-data-lake-storage-gen2/monitor-pipeline-runs.png)

14. Işlem hattı çalıştırmasıyla ilişkili etkinlik çalıştırmalarını görüntülemek için **Eylemler** sütunundaki **etkinlik çalıştırmalarını görüntüle** bağlantısını seçin. İşlem hattında yalnızca bir etkinlik (kopyalama etkinliği) olduğundan tek bir girdi görürsünüz. İşlem hattı çalıştırmaları görünümüne geri dönmek için üstteki işlem **hatları** bağlantısını seçin. Listeyi yenilemek için **Yenile**’yi seçin. 

    ![Etkinlik çalıştırmalarını izleme](./media/load-azure-data-lake-storage-gen2/monitor-activity-runs.png)

15. Her kopyalama etkinliğinin yürütme ayrıntılarını izlemek için, etkinlik izleme görünümündeki **Eylemler** altında **Ayrıntılar** bağlantısını (gözlük resmi) seçin. Kaynaktan havuza, veri işleme, karşılık gelen süre ile yürütme adımlarına ve kullanılan yapılandırmalara kadar olan veri hacmi gibi ayrıntıları izleyebilirsiniz:

    ![Etkinlik çalıştırma ayrıntılarını izle](./media/load-azure-data-lake-storage-gen2/monitor-activity-run-details.png)

16. Verilerin Data Lake Storage 2. hesabınıza kopyalandığını doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
* [Azure Data Lake Storage 2. Nesil bağlayıcısı](connector-azure-data-lake-storage.md)
