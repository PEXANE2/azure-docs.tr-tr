---
title: Azure SQL Veri Ambarı’na veri yükleme
description: Verileri Azure SQL Veri Ambarı'na kopyalamak için Azure Veri Fabrikası'nı kullanın
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 04/16/2020
ms.openlocfilehash: 1a764f392402acf9aa405468470d0fb6f680d755
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81461117"
---
# <a name="load-data-into-azure-sql-data-warehouse-by-using-azure-data-factory"></a>Azure Data Factory kullanarak verileri Azure SQL Veri Ambarı’na yükleme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

[Azure SQL Veri Ambarı,](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) hem ilişkisel hem de ilişkisel olmayan büyük hacimli verileri işleyebilen bulut tabanlı, ölçeklendirilebilen bir veritabanıdır. SQL Veri Ambarı, kurumsal veri ambarı iş yükleri için en iyi duruma getirilmiş büyük ölçüde paralel işleme (MPP) mimarisi üzerine kurulmuştur. Depolamayı ölçeklendirme ve bağımsız olarak hesaplama esnekliğiile bulut elastikiyeti sunar.

Azure SQL Veri Ambarı'nı kullanmaya başlamak, Azure Veri Fabrikası'nı kullandığınızda her zamankinden daha kolay. Azure Veri Fabrikası, tam olarak yönetilen bulut tabanlı bir veri tümleştirme hizmetidir. Hizmeti, bir SQL Veri Ambarı'nı mevcut sisteminizden gelen verilerle doldurmak ve analitik çözümlerinizi yaparken zamandan tasarruf etmek için kullanabilirsiniz.

Azure Veri Fabrikası, Verileri Azure SQL Veri Ambarı'na yüklemek için aşağıdaki avantajları sunar:

* **Kurulumu kolay**: Komut dosyası gerektirmeyen sezgisel 5 adımlı sihirbaz.
* **Zengin veri deposu desteği**: Şirket içi ve bulut tabanlı veri depolarından oluşan zengin bir dizi için yerleşik destek. Ayrıntılı bir liste için [Desteklenen veri depolarının](copy-activity-overview.md#supported-data-stores-and-formats)tablosuna bakın.
* **Güvenli ve uyumlu**: Veriler HTTPS veya ExpressRoute üzerinden aktarılır. Genel hizmet varlığı, verilerinizin coğrafi sınırdan asla çıkmamasını sağlar.
* **PolyBase : Polybase kullanarak benzersiz performans,** verileri Azure SQL Veri Ambarı'na taşımanın en etkili yoludur. Azure Blob depolama ve Data Lake Store dahil olmak üzere her türlü veri deposundan yüksek yük hızları elde etmek için hazırlama blob özelliğini kullanın. (Polybase varsayılan olarak Azure Blob depolama ve Azure Veri Gölü Deposu'nü destekler.) Ayrıntılar için [bkz.](copy-activity-performance.md)

Bu makalede, _Azure SQL Veritabanından Azure SQL Veri Ambarı'na veri yüklemek için_Veri Fabrikası Kopyalama Veri aracını nasıl kullanacağınızı gösterir. Diğer veri depolarından veri kopyalamak için benzer adımları izleyebilirsiniz.

> [!NOTE]
> Daha fazla bilgi için Azure [Veri Fabrikası'nı kullanarak Azure SQL Veri Ambarı'na veya Azure SQL Veri Ambarından veri kopyalama'ya](connector-azure-sql-data-warehouse.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği: Azure aboneliğiniz yoksa, başlamadan önce ücretsiz bir [hesap](https://azure.microsoft.com/free/) oluşturun.
* Azure SQL Veri Ambarı: Veri ambarı, SQL veritabanından kopyalanan verileri tutar. Azure SQL Veri Ambarı yoksa, SQL Veri [Ambarı Oluştur'daki](../sql-data-warehouse/sql-data-warehouse-get-started-tutorial.md)yönergeleri görün.
* Azure SQL Veritabanı: Bu öğretici, Adventure Works LT örnek verileriyle birlikte azure SQL veritabanındaki verileri kopyalar. [Azure SQL veritabanı oluştur'daki](../sql-database/sql-database-get-started-portal.md)yönergeleri izleyerek bir SQL veritabanı oluşturabilirsiniz.
* Azure depolama hesabı: Azure Depolama, toplu kopyalama işleminde _hazırlama_ blob'u olarak kullanılır. Azure depolama hesabınız yoksa [Depolama hesabı oluşturma](../storage/common/storage-account-create.md) bölümündeki yönergelere bakın.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Sol menüde > kaynak**Veri + Analitik** > **Veri Fabrikası** **Oluştur'u**seçin:

2. Yeni **veri fabrikası** sayfasında, aşağıdaki öğeler için değerler sağlayın:

    * **Adı**: Ad için *LoadSQLDWDemo* girin. Veri fabrikanızın adı *global olarak benzersiz olmalıdır. "LoadSQLDWDemo" hatası alırsanız, veri fabrikası için farklı bir ad girin. Örneğin, _**adınızı**_**ADFTutorialDataFactory**kullanabilirsiniz. Veri fabrikasını yeniden oluşturmayı deneyin. Data Factory yapıtlarını adlandırma kuralları için bkz. [Data Factory adlandırma kuralları](naming-rules.md).
    * **Abonelik**: Veri fabrikasını oluşturmak için Azure aboneliğinizi seçin. 
    * **Kaynak Grubu**: Açılan listeden varolan bir kaynak grubu seçin veya **Yeni Oluştur** seçeneğini belirleyin ve bir kaynak grubunun adını girin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/management/overview.md).  
    * **Sürüm**: **V2**seçin.
    * **Konum**: Veri fabrikasının konumunu seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan veri depoları başka konumlarda ve bölgelerde olabilir. Bu veri depoları Azure Veri Gölü Deposu, Azure Depolama, Azure SQL Veritabanı ve benzeri bilgileri içerir.

3. **Oluştur**’u seçin.
4. Oluşturma tamamlandıktan sonra veri fabrikanıza gidin. Aşağıdaki resimde gösterildiği gibi **Veri Fabrikası** ana sayfasını görürsünüz:

   ![Data factory giriş sayfası](./media/doc-common-process/data-factory-home-page.png)

   Veri Tümleştirme Uygulamasını ayrı bir sekmede açmak için **Oluştur ve İzle** kutucuğunu seçin.

## <a name="load-data-into-azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı’na veri yükleme

1. **Başlayalım** sayfasında, Veri Kopyalama aracını açmak için **Veri Kopyala** kutucuğunu seçin.

1. **Özellikler** sayfasında, **Görev adı** alanı için **CopyFromSQLToSQLDW'yi** belirtin ve **İleri'yi**seçin.

    ![Özellikler sayfası](./media/load-azure-sql-data-warehouse/copy-data-tool-properties-page.png)

1. Kaynak **veri deposu** sayfasında aşağıdaki adımları tamamlayın:
    >[!TIP]
    >Bu öğreticide, kaynak veri deponuzun kimlik doğrulama türü olarak *SQL kimlik doğrulaması* kullanırsınız, ancak desteklenen diğer kimlik doğrulama yöntemlerini seçebilirsiniz: Gerekirse*Hizmet Sorumlusu* ve *Yönetilen Kimlik.* Ayrıntılar için [bu makaledeki](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-database#linked-service-properties) ilgili bölümlere bakın.
    >Veri depolarının sırlarını güvenli bir şekilde depolamak için Azure Anahtar Kasası kullanılması da önerilir. Ayrıntılı resimler için [bu makaleye](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault) bakın.

    a. tıklayın **+ Yeni bağlantı oluşturun.**

    b. Galeriden **Azure SQL Veritabanı'nı** seçin ve **Devam et'i**seçin. Bağlayıcılara filtre yazmak için arama kutusuna "SQL" yazabilirsiniz.

    ![Azure SQL veritabanını seçme](./media/load-azure-sql-data-warehouse/select-azure-sql-db-source.png)

    c. Yeni **Bağlantılı Hizmet** sayfasında, açılan listeden sunucu adınızı ve DB adınızı seçin ve kullanıcı adını ve parolayı belirtin. Ayarları doğrulamak için **Test bağlantısını** tıklatın ve ardından **Oluştur'u**seçin.

    ![Azure SQL veritabanını yapılandırma](./media/load-azure-sql-data-warehouse/configure-azure-sql-db.png)

    d. Kaynak olarak yeni oluşturulan bağlantılı hizmeti seçin ve **İleri**'ye tıklayın.

1. Verileri **kopyalamak veya özel bir sorgu** sayfası kullanmak için seç tablolarına, tablolara filtre etmek için **SalesLT'i** girin. Kopya için tüm tabloları kullanmak için **(Tümünü Seç)** kutusunu seçin ve ardından **İleri'yi**seçin.

    ![Kaynak tabloları seçin](./media/load-azure-sql-data-warehouse/select-source-tables.png)

1. Filtre **uygula** sayfasında ayarlarınızı belirtin veya **İleri'yi**seçin.

1. Hedef **veri depolama** sayfasında aşağıdaki adımları tamamlayın:
    >[!TIP]
    >Bu öğreticide, hedef veri deponuz için kimlik doğrulama türü olarak *SQL kimlik doğrulaması* kullanırsınız, ancak desteklenen diğer kimlik doğrulama yöntemlerini seçebilirsiniz: Gerekirse*Hizmet Sorumlusu* ve *Yönetilen Kimlik.* Ayrıntılar için [bu makaledeki](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-data-warehouse#linked-service-properties) ilgili bölümlere bakın.
    >Veri depolarının sırlarını güvenli bir şekilde depolamak için Azure Anahtar Kasası kullanılması da önerilir. Ayrıntılı resimler için [bu makaleye](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault) bakın.

    a. Bağlantı eklemek için **+ Yeni bağlantı oluştur'u** tıklatın

    b. Galeriden **Azure Synapse Analytics'i (eski adıyla SQL DW)** seçin ve **Devam et'i**seçin. Bağlayıcılara filtre yazmak için arama kutusuna "SQL" yazabilirsiniz.

    ![Azure SQL DW'yi seçin](./media/load-azure-sql-data-warehouse/select-azure-sql-dw-sink.png)

    c. Yeni **Bağlantılı Hizmet** sayfasında, açılan listeden sunucu adınızı ve DB adınızı seçin ve kullanıcı adını ve parolayı belirtin. Ayarları doğrulamak için **Test bağlantısını** tıklatın ve ardından **Oluştur'u**seçin.

    ![Azure SQL DW'yi yapılandırma](./media/load-azure-sql-data-warehouse/configure-azure-sql-dw.png)

    d. Kaynak olarak yeni oluşturulan bağlantılı havuz hizmetini seçin ve **İleri**'ye tıklayın.

1. Tablo **eşleme** sayfasında, içeriği gözden geçirin ve **İleri'yi**seçin. Akıllı bir tablo eşleme görüntüler. Kaynak tablolar, tablo adlarını temel alan hedef tablolara eşlenir. Hedefte bir kaynak tablo yoksa, Azure Veri Fabrikası varsayılan olarak aynı ada sahip bir hedef tablo oluşturur. Bir kaynak tabloyu varolan bir hedef tabloyla eşleyebilirsiniz.

   > [!NOTE]
   > SQL Server veya Azure SQL Veritabanı kaynak olduğunda SQL Veri Ambarı için otomatik tablo oluşturma uygulanır. Başka bir kaynak veri deposundan veri kopyalarsanız, veri kopyasını yürütmeden önce skeci azure SQL Veri Ambarı'nda önceden oluşturmanız gerekir.

   ![Tablo eşleme sayfası](./media/load-azure-sql-data-warehouse/table-mapping.png)

1. Sütun **eşleme** sayfasında, içeriği gözden geçirin ve **İleri'yi**seçin. Akıllı tablo eşleme sütun adını temel adatır. Veri Fabrikası'nın tabloları otomatik olarak oluşturmasına izin ederseniz, kaynak ve hedef depolar arasında uyumsuzluklar olduğunda veri türü dönüştürmesi oluşabilir. Kaynak ve hedef sütun arasında desteklenmeyen bir veri türü dönüştürmesi varsa, ilgili tablonun yanında bir hata iletisi görürsünüz.

    ![Sütun eşleme sayfası](./media/load-azure-sql-data-warehouse/schema-mapping.png)

1. **Ayarlar** sayfasında aşağıdaki adımları tamamlayın:

    a. **Hazırlama ayarları** bölümünde , **+ Yeni** evreleme depolamasını tıklatın. Depolama, PolyBase kullanarak verileri SQL Veri Ambarı'na yüklenmesinden önce evrelemek için kullanılır. Kopyalama tamamlandıktan sonra, Azure Blob Depolama'daki geçici veriler otomatik olarak temizlenir.

    b. Yeni **Bağlantılı Hizmet** sayfasında, depolama hesabınızı seçin ve bağlantılı hizmeti dağıtmak için **Oluştur'u** seçin.

    c. Gelişmiş **ayarlar** bölümünde, Kullanım **türü varsayılan** seçeneğini seçin ve **sonra İleri'yi**seçin.

    ![PolyBase'i yapılandır](./media/load-azure-sql-data-warehouse/configure-polybase.png)

1. **Özet** sayfasında, ayarları gözden geçirin ve **İleri'yi**seçin.

    ![Özet sayfası](./media/load-azure-sql-data-warehouse/summary-page.png)
1. Dağıtım **sayfasında,** ardışık izlemeyi (görev) izlemek için **Monitör'ü** seçin.

1. Soldaki **İzleyici** sekmesinin otomatik olarak seçildiğine dikkat edin. Ardışık iş aktüeden başarılı bir şekilde tamamlandığında, etkinlik çalıştırma ayrıntılarını görüntülemek ve ardışık ardışık ardışık ardışık lığı yeniden çalıştırmak için **PIPELINE NAME** sütununaltındaki **CopyFromSQLToSQLDW** bağlantısını seçin.

    [![İzleme boru hattı çalışır](./media/load-azure-sql-data-warehouse/pipeline-monitoring.png)](./media/load-azure-sql-data-warehouse/pipeline-monitoring.png#lightbox)
1. Ardışık hatlar çalışır görünümüne geri dönmek için, en üstteki **Tüm ardışık hatlar çalışır** bağlantısını seçin. Listeyi yenilemek için **Yenile**’yi seçin.

    ![Etkinlik çalıştırmalarını izleme](./media/load-azure-sql-data-warehouse/activity-monitoring.png)

1. Her kopyalama etkinliğinin yürütme ayrıntılarını izlemek için, etkinlik görünümünde **EtkİnLİk** İsİm'inin altındaki **Ayrıntılar** bağlantısını (gözlük simgesi) seçin. Kaynaktan lavaboya kopyalanan veri hacmi, veri aktarımı, ilgili süreye sahip yürütme adımları ve kullanılan yapılandırmalar gibi ayrıntıları izleyebilirsiniz.
    ![Etkinlik çalıştırma ayrıntılarını izleme](./media/load-azure-sql-data-warehouse/monitor-activity-run-details-1.png)

    ![Etkinlik çalıştırma ayrıntılarını izleme](./media/load-azure-sql-data-warehouse/monitor-activity-run-details-2.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure SQL Veri Ambarı desteği hakkında bilgi edinmek için aşağıdaki makaleye ilerleyin:

> [!div class="nextstepaction"]
>[Azure SQL Veri Ambarı bağlayıcısı](connector-azure-sql-data-warehouse.md)
