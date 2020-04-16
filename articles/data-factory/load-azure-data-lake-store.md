---
title: Verileri Azure Veri Gölü Depolama Gen1'e yükleyin
description: Verileri Azure Veri Gölü Depolama Gen1'e kopyalamak için Azure Veri Fabrikası'nı kullanın
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 01/17/2018
ms.openlocfilehash: 1b1b19814709451bdbbea97462c459149484e71f
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81415857"
---
# <a name="load-data-into-azure-data-lake-storage-gen1-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri Azure Veri Gölü Depolama Gen1'e yükleyin

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

[Azure Veri Gölü Depolama Gen1](../data-lake-store/data-lake-store-overview.md) (daha önce Azure Veri Gölü Deposu olarak da bilinir) büyük veri analitik iş yükleri için kurumsal çapında hiper ölçekli bir depodur. Veri Gölü Depolama Gen1, herhangi bir boyut, tür ve yutma hızına ait verileri yakalamanızı sağlar. Veriler operasyonel ve araştırmacı analizler için tek bir yerde yakalanır.

Azure Veri Fabrikası, tam olarak yönetilen bulut tabanlı bir veri tümleştirme hizmetidir. Hizmeti, gölü mevcut sisteminizden alınan verilerle doldurmak ve analitik çözümlerinizi yaparken zamandan tasarruf etmek için kullanabilirsiniz.

Azure Veri Fabrikası, Veri Gölü Depolama Gen1'e veri yüklemek için aşağıdaki avantajları sunar:

* **Kurulumu kolay**: Komut dosyası gerektirmeyen sezgisel 5 adımlı sihirbaz.
* **Zengin veri deposu desteği**: Şirket içi ve bulut tabanlı veri depolarından oluşan zengin bir dizi için yerleşik destek. Ayrıntılı bir liste için [Desteklenen veri depolarının](copy-activity-overview.md#supported-data-stores-and-formats)tablosuna bakın.
* **Güvenli ve uyumlu**: Veriler HTTPS veya ExpressRoute üzerinden aktarılır. Genel hizmet varlığı, verilerinizin coğrafi sınırdan asla çıkmamasını sağlar.
* **Yüksek performans**: Data Lake Storage Gen1'e 1 GB/s'ye kadar veri yükleme hızı. Ayrıntılar için [bkz.](copy-activity-performance.md)

Bu makalede, Amazon S3 verileri Veri _Gölü Depolama Gen1 içine yüklemek_için Veri Fabrikası Kopyalama Veri aracını nasıl kullanacağınızı gösterir. Diğer veri depolarından veri kopyalamak için benzer adımları izleyebilirsiniz.

> [!NOTE]
> Daha fazla bilgi için Azure [Veri Fabrikası'nı kullanarak Veri Gölü Depolama Gen1'e veya Veri Gölü Depolama Gen1'den veri kopyalama'ya](connector-azure-data-lake-store.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği: Azure aboneliğiniz yoksa, başlamadan önce ücretsiz bir [hesap](https://azure.microsoft.com/free/) oluşturun.
* Veri Gölü Depolama Gen1 hesabı: Veri Gölü Depolama Gen1 hesabınız yoksa, [Veri Gölü Depolama Gen1 hesabı oluştur'daki](../data-lake-store/data-lake-store-get-started-portal.md#create-a-data-lake-storage-gen1-account)yönergeleri görün.
* Amazon S3: Bu makale, Amazon S3 verileri kopyalamak için nasıl gösterir. Benzer adımları izleyerek diğer veri depolarını kullanabilirsiniz.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Sol menüde, **bir kaynak** > **Analiz** > **Veri Fabrikası**Oluştur'u seçin:
   
   ![“Yeni” bölmesinde Data Factory seçimi](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

2. Yeni **veri fabrikası** sayfasında, aşağıdaki resimde gösterilen alanlar için değerler sağlayın: 
      
   ![Yeni veri fabrikası sayfası](./media/load-data-into-azure-data-lake-store//new-azure-data-factory.png)
 
    * **Ad**: Azure veri fabrikanız için benzersiz bir ad girin. "Veri fabrikası adı \"LoadADLSG1Demo\" kullanılamıyor" hatasını alırsanız, veri fabrikası için farklı bir ad girin. Örneğin, _**adınızı**_**ADFTutorialDataFactory**kullanabilirsiniz. Veri fabrikasını yeniden oluşturmayı deneyin. Data Factory yapıtlarını adlandırma kuralları için bkz. [Data Factory adlandırma kuralları](naming-rules.md).
    * **Abonelik**: Veri fabrikasını oluşturmak için Azure aboneliğinizi seçin. 
    * **Kaynak Grubu**: Açılan listeden varolan bir kaynak grubu seçin veya **Yeni Oluştur** seçeneğini belirleyin ve bir kaynak grubunun adını girin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/management/overview.md).  
    * **Sürüm**: **V2**seçin.
    * **Konum**: Veri fabrikasının konumunu seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan veri depoları başka konumlarda ve bölgelerde olabilir. Bu veri depoları Azure Veri Gölü Depolama Gen1, Azure Depolama, Azure SQL Veritabanı ve benzeri içerir.

3. **Oluştur**’u seçin.
4. Oluşturma tamamlandıktan sonra veri fabrikanıza gidin. Aşağıdaki resimde gösterildiği gibi **Veri Fabrikası** ana sayfasını görürsünüz: 
   
   ![Data factory giriş sayfası](./media/load-data-into-azure-data-lake-store/data-factory-home-page.png)

   Veri Tümleştirme Uygulamasını ayrı bir sekmede açmak için **Oluştur ve İzle** kutucuğunu seçin.

## <a name="load-data-into-data-lake-storage-gen1"></a>Verileri Veri Gölü Depolama Gen1'e yükleyin

1. **Başlat'ın sayfasında,** Verileri **Kopyala** aracını başlatmak için Veri Kopyala'yı seçin: 

   ![Veri Kopyalama aracının kutucuğu](./media/load-data-into-azure-data-lake-store/copy-data-tool-tile.png)
2. **Özellikler** sayfasında, **Görev adı** alanı için **CopyFromAmazonS3ToADLS** belirtin ve **Sonraki'yi**seçin:

    ![Özellikler sayfası](./media/load-data-into-azure-data-lake-store/copy-data-tool-properties-page.png)
3. Kaynak **veri deposu** sayfasında + **Yeni bağlantı oluştur'u**tıklatın:

    ![Kaynak veri deposu sayfası](./media/load-data-into-azure-data-lake-store/source-data-store-page.png)
    
    **Amazon S3'ünü**seçin ve **Devam et'i** seçin
    
    ![Kaynak veri deposu s3 sayfası](./media/load-data-into-azure-data-lake-store/source-data-store-page-s3.png)
    
4. Amazon **S3 bağlantı** sayfasını belirtin, aşağıdaki adımları yapın: 
   1. Access **Tuşu Kimliği** değerini belirtin.
   2. Gizli **Erişim Anahtarı** değerini belirtin.
   3. **Bitiş'i**seçin.
   
      ![Amazon S3 hesabını belirtin](./media/load-data-into-azure-data-lake-store/specify-amazon-s3-account.png)
   
   4. Yeni bir bağlantı göreceksiniz. **İleri**’yi seçin.
   
   ![Amazon S3 hesabını belirtin](./media/load-data-into-azure-data-lake-store/specify-amazon-s3-account-created.png)
   
5. **Giriş dosyasını veya klasörünü seçin** sayfasında, üzerine kopyalamak istediğiniz klasöre ve dosyaya göz atın. Klasörü/dosyayı seçin, **Seç'i**seçin ve sonra **İleri'yi**seçin :

    ![Girdi dosyasını veya klasörünü seçin](./media/load-data-into-azure-data-lake-store/choose-input-folder.png)

6. **Dosyaları özyinelemeli olarak** kopyala ve **Ikili kopya** (dosyaları olduğu gibi kopyala) seçeneklerini seçerek kopyalama davranışını seçin. **Sonraki'ni**Seçin :

    ![Çıktı klasörünü belirtin](./media/load-data-into-azure-data-lake-store/specify-binary-copy.png)
    
7. Hedef **veri deposu** sayfasında **+ Yeni bağlantı oluştur'u**tıklatın ve ardından **Azure Veri Gölü Depolama Gen1'i**seçin ve Devam et seçeneğini **belirleyin:**

    ![Hedef veri deposu sayfası](./media/load-data-into-azure-data-lake-store/destination-data-storage-page.png)

8. Yeni **Bağlantılı Hizmet (Azure Veri Gölü Depolama Gen1)** sayfasında aşağıdaki adımları yapın: 

   1. **Data Lake Store hesap adı**için Veri Gölü Depolama Gen1 hesabınızı seçin.
   2. **Kiracı'yı**belirtin ve Bitiş'i seçin.
   3. **İleri**’yi seçin.
   
   > [!IMPORTANT]
   > Bu iznizde, Veri Gölü Depolama Gen1 hesabınızın kimliğini doğrulamak için Azure kaynakları için yönetilen bir kimlik kullanırsınız. [Bu yönergeleri](connector-azure-data-lake-store.md#managed-identity)izleyerek MSI'a Veri Gölü Depolama Gen1'de uygun izinleri verdiğinden emin olun.
   
   ![Veri Gölü Depolama Gen1 hesabını belirtin](./media/load-data-into-azure-data-lake-store/specify-adls.png)
9. Çıktı **dosyasını veya klasörünü seç** sayfasında, çıkış klasörü adı olarak **copyfroms3'ü** girin ve **Sonraki'ni**seçin: 

    ![Çıktı klasörünü belirtin](./media/load-data-into-azure-data-lake-store/specify-adls-path.png)

10. **Ayarlar** sayfasında **İleri'yi**seçin:

    ![Ayarlar sayfası](./media/load-data-into-azure-data-lake-store/copy-settings.png)
11. **Özet** sayfasında, ayarları gözden geçirin ve **Sonraki'ni**seçin:

    ![Özet sayfası](./media/load-data-into-azure-data-lake-store/copy-summary.png)
12. Dağıtım **sayfasında,** ardışık izlemeyi izlemek için **Monitör'ü** (görev) seçin:

    ![Dağıtım sayfası](./media/load-data-into-azure-data-lake-store/deployment-page.png)
13. Soldaki **İzleyici** sekmesinin otomatik olarak seçildiğine dikkat edin. **Eylemler** sütunu, etkinlik çalıştırma ayrıntılarını görüntülemek ve ardışık hattı yeniden çalıştırmak için bağlantılar içerir:

    ![İşlem hattı çalıştırmalarını izleme](./media/load-data-into-azure-data-lake-store/monitor-pipeline-runs.png)
14. Ardışık işlem hattı çalışmasıyla ilişkili etkinlik çalışanlarını görüntülemek **için, Eylemler** sütunundaki **Etkinlik Çalışır'ı Görüntüle** bağlantısını seçin. İşlem hattında yalnızca bir etkinlik (kopyalama etkinliği) olduğundan tek bir girdi görürsünüz. Ardışık hatlar çalışır görünümüne geri dönmek için, üstteki **Ardışık Hatlar** bağlantısını seçin. Listeyi yenilemek için **Yenile**’yi seçin. 

    ![Etkinlik çalıştırmalarını izleme](./media/load-data-into-azure-data-lake-store/monitor-activity-runs.png)

15. Her kopyalama etkinliğinin yürütme ayrıntılarını izlemek için, etkinlik izleme görünümünde **Eylemler** altında **Ayrıntılar** bağlantısını seçin. Kaynaktan lavaboya kopyalanan veri hacmi, veri aktarımı, ilgili süreye sahip yürütme adımları ve kullanılan yapılandırmalar gibi ayrıntıları izleyebilirsiniz:

    ![Etkinlik çalıştırma ayrıntılarını izleme](./media/load-data-into-azure-data-lake-store/monitor-activity-run-details.png)

16. Verilerin Veri Gölü Depolama Gen1 hesabınıza kopyalandığından doğrulayın: 

    ![Veri Gölü Depolama Gen1 çıktısını doğrulayın](./media/load-data-into-azure-data-lake-store/adls-copy-result.png)

## <a name="next-steps"></a>Sonraki adımlar

Data Lake Storage Gen1 desteği hakkında bilgi edinmek için aşağıdaki makaleye ilerleyin: 

> [!div class="nextstepaction"]
>[Azure Veri Gölü Depolama Gen1 konektörü](connector-azure-data-lake-store.md)
