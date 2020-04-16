---
title: Azure Veri Fabrikası ve Azure Veri Paylaşımı'nı kullanarak veri tümleştirmesi
description: Azure Veri Fabrikası ve Azure Veri Paylaşımı'nı kullanarak verileri kopyalama, dönüştürme ve paylaşma
author: djpmsft
ms.author: daperlov
ms.service: data-factory
ms.topic: tutorial
ms.custom: seo-lt-2019
ms.date: 01/08/2020
ms.openlocfilehash: 7d453b2724c308e48366d653a51d9e6aa8e82c96
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81415918"
---
# <a name="data-integration-using-azure-data-factory-and-azure-data-share"></a>Azure Veri Fabrikası ve Azure Veri Paylaşımı'nı kullanarak veri tümleştirmesi

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Müşteriler modern veri ambarı ve analiz projelerine girişirken, yalnızca daha fazla veri değil, aynı zamanda veri mülklerinde daha fazla görünürlük gerektirir. Bu atölye, Azure Veri Fabrikası ve Azure Veri Paylaşımı'ndaki geliştirmelerin Azure'da veri tümleştirmesini ve yönetimini nasıl basitleştirttişe ne kadar önemse deiçine giriyor. 

Kodsuz ETL/ELT'yi etkinleştirmekten verileriniz üzerinde kapsamlı bir görünüm oluşturmaya kadar, Azure Veri Fabrikası'ndaki geliştirmeler veri mühendislerinize işletmenize güvenle daha fazla veri ve dolayısıyla daha fazla değer getirme leri için güç verecektir. Azure Veri Paylaşımı, iş paylaşımına yönetimli bir şekilde iş yapmanıza olanak tanır.

Bu atölyede, Azure Sql veritabanından (SQL DB) azure veri gölü deposu gen2 (ADLS gen2) verilerini almak için Azure Veri Fabrikası'nı (ADF) kullanırsınız. Verileri göle indirgedikten sonra, veri akışlarını, veri fabrikasının yerel dönüşüm hizmetini eşleme yoluyla dönüştürecek ve azure Synapse Analytics'e (eski adıyla SQL DW) dönüştüreceksiniz. Ardından, Azure Veri Paylaşımı'nı kullanarak tabloyu dönüştürülmüş verilerle ve bazı ek verilerle paylaşırsınız. 

Bu laboratuvarda kullanılan veriler New York taksi verileri. Azure SQL veritabanınıza almak için [taksi veri bacpac dosyasını](https://github.com/djpmsft/ADF_Labs/blob/master/sample-data/taxi-data.bacpac)indirin.

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

* **Azure SQL Veritabanı**: SQL DB'niz yoksa, [SQL DB hesabı oluşturmayı](https://docs.microsoft.com/azure/sql-database/sql-database-single-database-get-started?tabs=azure-portal) öğrenin

* **Azure Veri Gölü Depolama Gen2 depolama hesabınız**: AdLS Gen2 depolama hesabınız yoksa, [bir ADLS Gen2 depolama hesabı oluşturmayı](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-quickstart-create-account)öğrenin.

* **Azure Synapse Analytics (eski adıyla SQL DW)**: Azure Synapse Analytics (eski adıyla SQL DW) yoksa, Azure [Synapse Analytics örneğini](https://docs.microsoft.com/azure/sql-data-warehouse/create-data-warehouse-portal)nasıl oluşturabileceğinizi öğrenin.

* **Azure Veri Fabrikası**: Bir veri fabrikası oluşturmadıysanız, [nasıl bir veri fabrikası oluşturabileceğinizi](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal)görün.

* **Azure Veri Paylaşımı**: Bir veri paylaşımı oluşturmadıysanız, nasıl [veri paylaşımı oluşturabileceğinizi](https://docs.microsoft.com/azure/data-share/share-your-data#create-a-data-share-account)görün.

## <a name="set-up-your-azure-data-factory-environment"></a>Azure Veri Fabrikası ortamınızı ayarlama

Bu bölümde, Azure portalından Azure Veri Fabrikası kullanıcı deneyimine (ADF UX) nasıl erişeceğinizi öğreneceksiniz. ADF UX'ye girince, kullandığımız her veri mağazası için üç bağlantılı hizmeti yapılandıracaksınız: Azure SQL DB, ADLS Gen2 ve Azure Synapse Analytics.

Azure Veri Fabrikası'nda bağlantılı hizmetler, bağlantı bilgilerini dış kaynaklara tanımlar. Azure Veri Fabrikası şu anda 85'in üzerinde konektörü destekliyor.

### <a name="open-the-azure-data-factory-ux"></a>Azure Veri Fabrikası UX'yi açın

1. Azure [portalını](https://portal.azure.com) Microsoft Edge veya Google Chrome'da açın.
1. Sayfanın üst kısmındaki arama çubuğunu kullanarak 'Veri Fabrikaları'nı arayın

    ![Portal](media/lab-data-flow-data-share/portal1.png)
1. Kaynak bıçağını açmak için veri fabrikası kaynağınızı tıklatın.

    ![Portal](media/lab-data-flow-data-share/portal2.png)
1. ADF UX'yi açmak için **Yazar ve Monitör'e** tıklayın. ADF UX'ye adf.azure.com'dan da ulaşılabilir.

    ![Portal](media/lab-data-flow-data-share/portal3.png)
1. ADF UX'nin ana sayfasına yönlendirileceksiniz. Bu sayfa hızlı başlar, öğretim videoları ve veri fabrikası kavramları öğrenmek için öğreticiler bağlantılar içerir. Yazmaya başlamak için sol yan çubuktaki kalem simgesine tıklayın.

    ![Portal](media/lab-data-flow-data-share/configure1.png)

### <a name="create-an-azure-sql-database-linked-service"></a>Azure SQL veritabanına bağlı bir hizmet oluşturma

1. Yazma sayfası, ardışık hatlar, veri kümeleri, veri akışları, tetikleyiciler ve bağlantılı hizmetler gibi veri fabrikası kaynakları oluşturduğunuz sayfadır. Bağlantılı bir hizmet oluşturmak için sağ alt köşedeki **Bağlantılar** düğmesini tıklatın.

    ![Portal](media/lab-data-flow-data-share/configure2.png)
1. Bağlantılar sekmesinde, yeni bir bağlantılı hizmet eklemek için **Yeni'yi** tıklatın.

    ![Portal](media/lab-data-flow-data-share/configure3.png)
1. Yapılandıracağınız ilk bağlantılı hizmet bir Azure SQL DB'dir. Veri deposu listesini filtrelemek için arama çubuğunu kullanabilirsiniz. **Azure SQL Veritabanı** döşemesini tıklatın ve devam'ı tıklatın.

    ![Portal](media/lab-data-flow-data-share/configure4.png)
1. SQL DB yapılandırma bölmesine bağlı hizmet adınız olarak 'SQLDB' girin. Veri fabrikasının veritabanınıza bağlanmasına izin vermek için kimlik bilgilerinizi girin. SQL kimlik doğrulaması kullanıyorsanız, sunucu adını, veritabanını, kullanıcı adınızı ve parolanızı girin. Bağlantı bilgilerinizin doğru olduğunu Test **bağlantısını**tıklatarak doğru luğunu zedebilirsiniz. Tamamlandığında **Oluştur'u** tıklatın.

    ![Portal](media/lab-data-flow-data-share/configure5.png)

### <a name="create-an-azure-synapse-analytics-linked-service"></a>Azure Synapse Analytics bağlantılı bir hizmet oluşturun

1. Azure Synapse Analytics bağlantılı bir hizmet eklemek için aynı işlemi yineleyin. Bağlantılar sekmesinde **Yeni'yi**tıklatın. Azure **Synapse Analytics (eski adıyla SQL DW)** döşemesini seçin ve devam et'i tıklatın.

    ![Portal](media/lab-data-flow-data-share/configure6.png)
1. Bağlantılı hizmet yapılandırma bölmesine, bağlı hizmet adınız olarak 'SQLDW' girin. Veri fabrikasının veritabanınıza bağlanmasına izin vermek için kimlik bilgilerinizi girin. SQL kimlik doğrulaması kullanıyorsanız, sunucu adını, veritabanını, kullanıcı adınızı ve parolanızı girin. Bağlantı bilgilerinizin doğru olduğunu Test **bağlantısını**tıklatarak doğru luğunu zedebilirsiniz. Tamamlandığında **Oluştur'u** tıklatın.

    ![Portal](media/lab-data-flow-data-share/configure7.png)

### <a name="create-an-azure-data-lake-storage-gen2-linked-service"></a>Azure Veri Gölü Depolama Gen2 bağlantılı bir hizmet oluşturma

1. Bu laboratuvar için gereken son bağlantılı hizmet bir Azure Veri Gölü Depolama gen2'dir.  Bağlantılar sekmesinde **Yeni'yi**tıklatın. Azure **Veri Gölü Depolama Gen2** döşemesini seçin ve devam'ı tıklatın.

    ![Portal](media/lab-data-flow-data-share/configure8.png)
1. Bağlantılı hizmet yapılandırma bölmesine, bağlı hizmet adınız olarak 'ADLSGen2' girin. Hesap tuşu kimlik doğrulamasını kullanıyorsanız, **Depolama hesabı adı** açılır tarafından adls gen2 depolama hesabınızı seçin. Bağlantı bilgilerinizin doğru olduğunu Test **bağlantısını**tıklatarak doğru luğunu zedebilirsiniz. Tamamlandığında **Oluştur'u** tıklatın.

    ![Portal](media/lab-data-flow-data-share/configure9.png)

### <a name="turn-on-data-flow-debug-mode"></a>Veri akışı hata ayıklama modunu açma

Veri *akışını eşleme kullanarak verileri dönüştür*bölümünde, eşleme veri akışları oluşturuyor olacaksınız. Eşleme veri akışları oluşturmadan önce en iyi yöntem, hata ayıklama modunu açmaktır ve bu da dönüşüm mantığını etkin bir kıvılcım kümesinde saniyeler içinde test etmenizi sağlar.

Hata ayıklama'yı açmak için, fabrikanın üst çubuğundaki **Veri akışı hata ayıklama** kaydırıcısını tıklatın. Onay iletişim kutusu açılırken tamam'ı tıklatın. Kümenin başlaması yaklaşık 5-7 dakika sürer. Başlatma sırasında *kopyalama etkinliğini kullanarak Azure SQL DB'deki verileri ADLS gen2'ye* almaya devam edin.

![Portal](media/lab-data-flow-data-share/configure10.png)

## <a name="ingest-data-using-the-copy-activity"></a>Kopyalama etkinliğini kullanarak verileri yutma

Bu bölümde, bir tabloyu Azure SQL DB'den ADLS gen2 depolama hesabına alan bir kopyalama etkinliği içeren bir ardışık işlem oluşturursunuz. Bir ardışık yapıyı nasıl ekleyeceğinizi, veri kümesini yapılandırmayı ve ADF UX üzerinden bir boru hattını nasıl hata ayıkladığınızı öğreneceksiniz. Bu bölümde kullanılan yapılandırma deseni, ilişkisel veri deposundan dosya tabanlı veri deposuna kopyalama için uygulanabilir.

Azure Veri Fabrikası'nda, bir ardışık işlem, bir görevi birlikte gerçekleştiren etkinliklerin mantıksal bir gruplandırmasIdır. Etkinlik, verilerinizde gerçekleşecek bir işlem tanımlar. Veri kümesi, bağlantılı bir hizmette kullanmak istediğiniz verilere işaret verir.

### <a name="create-a-pipeline-with-a-copy-activity"></a>Kopyalama etkinliği içeren bir ardışık hatlar oluşturma

1. Fabrika kaynakları bölmesinde, yeni kaynak menüsünü açmak için artı simgesine tıklayın. **Pipeline'ı**seçin.

    ![Portal](media/lab-data-flow-data-share/copy1.png)
1. Boru hattı tuvalinin **Genel** sekmesinde, boru hattınızı 'IngestAndTransformTaxiData' gibi açıklayıcı bir şey olarak adlandırın.

    ![Portal](media/lab-data-flow-data-share/copy2.png)
1. Boru hattı tuvalinin etkinlik **bölmesinde, Taşı ve Dönüştür** akordeonu açın ve Kopya **veri** etkinliğini tuvale sürükleyin. Kopyalama etkinliğine 'IngestIntoADLS' gibi açıklayıcı bir ad verin.

    ![Portal](media/lab-data-flow-data-share/copy3.png)

### <a name="configure-azure-sql-db-source-dataset"></a>Azure SQL DB kaynak veri kümesini yapılandırma

1. Kopyalama etkinliğinin **Kaynak** sekmesine tıklayın. Yeni bir veri kümesi oluşturmak için **Yeni'yi**tıklatın. Kaynağın tablo 'dbo olacak. TripData' bulunan bağlantılı hizmet 'SQLDB' daha önce yapılandırıldı.

    ![Portal](media/lab-data-flow-data-share/copy4.png)
1. Azure **SQL Veritabanı'nı** arayın ve devam et'i tıklatın.

    ![Portal](media/lab-data-flow-data-share/copy5.png)
1. Veri setinizi 'TripData' olarak adlandırın. Bağlantılı hizmetiniz olarak 'SQLDB'yi seçin. Tablo adı 'dbo'yu seçin. TripData' tablo adı açılır. Schema **From connection/store 'dan**alma. Bittiğinde Tamam'ı tıklatın.

    ![Portal](media/lab-data-flow-data-share/copy6.png)

Kaynak veri kümenizi başarıyla oluşturdunuz. Kaynak ayarlarında, kullanım sorgusu alanında varsayılan değer **Tablosu'nun** seçildiğinden emin olun.

### <a name="configure-adls-gen-2-sink-dataset"></a>ADLS Gen 2 lavabo veri kümesini yapılandırma

1. Kopyalama etkinliğinin **Lavabo** sekmesine tıklayın. Yeni bir veri kümesi oluşturmak için **Yeni'yi**tıklatın.

    ![Portal](media/lab-data-flow-data-share/copy7.png)
1. Azure **Veri Gölü Depolama Gen2'yi** arayın ve devam et'i tıklatın.

    ![Portal](media/lab-data-flow-data-share/copy8.png)
1. Select format bölmesinde, csv dosyasına yazarken **DelimitedText'i** seçin. Devam'ı tıklatın.

    ![Portal](media/lab-data-flow-data-share/copy9.png)
1. Lavabo veri setinizi 'TripDataCSV' olarak adlandırın. Bağlantılı hizmetiniz olarak 'ADLSGen2'yi seçin. csv dosyanızı yazmak istediğiniz yere girin. Örneğin, verilerinizi kapsayıcıda `trip-data.csv` `staging-container`dosyaya yazabilirsiniz. Çıktı verilerinizin **üstbilgileri** olmasını istediğiniz gibi İlk satırı üstbilgi olarak doğru olarak ayarlayın. Hedefte henüz dosya bulunmadığından, **Alma şemasını** **Yok**olarak ayarlayın. Bittiğinde Tamam'ı tıklatın.

    ![Portal](media/lab-data-flow-data-share/copy10.png)

### <a name="test-the-copy-activity-with-a-pipeline-debug-run"></a>Bir ardışık bilgi ayıklama çalışmasıyla kopyalama etkinliğini test edin

1. Kopyalama etkinliğinizin doğru çalıştığını doğrulamak için, hata ayıklama çalışmasını yürütmek için ardışık hatlar tuvalinin üst **kısmındahata** ayıklama'yı tıklatın. Hata ayıklama çalışması, veri fabrikası hizmetine yayımlamadan önce ardışık ardışık hattınızı uçdan uca veya bir kesme noktasına kadar sınamamanızı sağlar.

    ![Portal](media/lab-data-flow-data-share/copy11.png)
1. Hata ayıklama çalışmanızı izlemek için, boru hattı tuvalinin **Çıktı** sekmesine gidin. İzleme ekranı her 20 saniyede bir veya yenileme düğmesini el ile tıklattığınızda otomatik olarak yenilenir. Kopyalama etkinliği, **Eylemler** sütunundaki göz gözlüğü simgesine tıklayarak erişilebilen özel bir izleme görünümüne sahiptir.

    ![Portal](media/lab-data-flow-data-share/copy12.png)
1. Kopyalama izleme görünümü, etkinliğin yürütme ayrıntılarını ve performans özelliklerini verir. Veri okuma/yazma, satır okuma/yazma, dosya okuma/yazma ve iş mesuliyonu gibi bilgileri görebilirsiniz. Her şeyi doğru şekilde yapılandırmışsanız, ADLS lavabonuzda bir dosyaya yazılmış 49.999 satır görmeniz gerekir.

    ![Portal](media/lab-data-flow-data-share/copy13.png)
1. Bir sonraki bölüme geçmeden önce, tüm bunları fabrikanın üst çubuğunda **yayımla'yı** tıklatarak veri fabrikası hizmetindeki değişikliklerinizi yayımlamanız önerilir. Azure Veri Fabrikası, bu laboratuvarda kapsanmıyor olsa da tam git tümleştirmeyi destekler. Git tümleştirmesi sürüm denetimine, depoda yinelemeli kaydetmeye ve bir veri fabrikasında işbirliğine olanak tanır. Daha fazla bilgi için [Azure Veri Fabrikası'nda kaynak denetimine](https://docs.microsoft.com/azure/data-factory/source-control#troubleshooting-git-integration)bakın.

    ![Portal](media/lab-data-flow-data-share/publish1.png)

## <a name="transform-data-using-mapping-data-flow"></a>Veri akışlarını eşlemeyi kullanarak verileri dönüştürme

Artık verileri Azure Veri Gölü Depolama'ya başarıyla kopyaladığınıza göre, bu verileri birleştirme ve bir veri ambarına toplama zamanı gelmiştir. Azure Veri Fabrikası'nın görsel olarak tasarlanmış dönüşüm hizmeti olan haritalama veri akışını kullanacağız. Veri akışlarını eşleme, kullanıcıların dönüşüm mantığını kodsuz geliştirmelerine ve Bunları ADF hizmeti tarafından yönetilen kıvılcım kümelerinde yürütmelerine olanak sağlar.

Bu adımda oluşturulan veri akışı bir önceki bölümde oluşturulan 'TripDataCSV' veri kümesini bir tablo 'dbo ile birleştirir. TripFares' dört anahtar sütuna dayalı olarak 'SQLDB'de depolanır. Daha sonra veriler, belirli `payment_type` alanların ortalamasını hesaplamak için sütuna göre toplanır ve bir Azure Synapse Analytics tablosuna yazılır.

### <a name="add-a-data-flow-activity-to-your-pipeline"></a>Ardışık hattınıza veri akışı etkinliği ekleme

1. Boru hattı tuvalinin etkinlik bölmesinde, **Taşı ve Dönüştür** akordeonu açın ve Veri **akışı** etkinliğini tuvale sürükleyin.

    ![Portal](media/lab-data-flow-data-share/dataflow1.png)
1. Açılan yan bölmede, **yeni veri akışı oluştur'u** seçin ve veri akışını **eşlemeyi**seçin. **Tamam**'a tıklayın.

    ![Portal](media/lab-data-flow-data-share/dataflow2.png)
1. Dönüşüm mantığınızı oluşturarak veri akışı tuvaline yönlendirileceksiniz. Genel sekmede, veri akışınızı 'JoinAndAggregateData' olarak adlandırın.

    ![Portal](media/lab-data-flow-data-share/dataflow3.png)

### <a name="configure-your-trip-data-csv-source"></a>Seyahat verilerinizi csv kaynağını yapılandırma

1. Yapmak istediğiniz ilk şey, iki kaynak dönüşümünüzü yapılandırmaktır. İlk kaynak 'TripDataCSV' DelimitedText veri kümesini işaret edecektir. Kaynak dönüşümü eklemek için tuvaldeki **Kaynak Ekle** kutusunu tıklatın.

    ![Portal](media/lab-data-flow-data-share/dataflow4.png)
1. Kaynağınızı 'TripDataCSV' olarak adlandırın ve kaynak açılır tarafından 'TripDataCSV' veri kümesini seçin. Hatırlarsanız, bu veri kümesini oluştururken başlangıçta bir şema almadın, çünkü orada veri yoktu. Şimdi `trip-data.csv` olduğundan, veri kümesi ayarları sekmesine gitmek için **Edet'i** tıklatın.

    ![Portal](media/lab-data-flow-data-share/dataflow5.png)
1. Sekme **Şema'ya** gidin ve **Şemayı Aktar'ı**tıklatın. Doğrudan dosya deposundan almak için **Bağlantıdan/Mağaza'dan** seçin. Tip dize 14 sütun görünmelidir.

    ![Portal](media/lab-data-flow-data-share/dataflow6.png)
1. Veri akışı 'JoinAndAggregateData' geri dön. Hata ayıklama kümeniz başladıysa (hata ayıklama kaydırıcısının yanındaki yeşil daireyle gösterilir), **Veri Önizleme** sekmesindeki verilerin anlık görüntüsünü alabilirsiniz. Veri önizlemesini almak için **Yenile'yi** tıklatın.

    ![Portal](media/lab-data-flow-data-share/dataflow7.png)

> [!Note]
> Veri önizlemesi veri yazmaz.

### <a name="configure-your-trip-fares-sql-db-source"></a>Seyahat ücretlerinizi SQL DB kaynağınızı yapılandırın

1. Eklediğiniz ikinci kaynak SQL DB tablosu 'dbo'yu işaret edecektir. TripFares'in. 'TripDataCSV' kaynağınızın altında başka bir **Kaynak Ekle** kutusu olacaktır. Yeni bir kaynak dönüşümü eklemek için tıklatın.

    ![Portal](media/lab-data-flow-data-share/dataflow8.png)
1. Bu kaynağa 'TripFaresSQL' adını ver. Yeni bir SQL DB veri kümesi oluşturmak için kaynak veri kümesi alanının yanında **Yeni'yi** tıklatın.

    ![Portal](media/lab-data-flow-data-share/dataflow9.png)
1. Azure **SQL Veritabanı** döşemesini seçin ve devam et'i tıklatın. *Not: Veri fabrikasındaki konektörlerin çoğunun veri akışını nakışlamada desteklenmediğini fark edebilirsiniz. Bu kaynaklardan birinden gelen verileri dönüştürmek için, kopyalama etkinliğini kullanarak desteklenen bir kaynağa dönüştürün.*

    ![Portal](media/lab-data-flow-data-share/dataflow10.png)
1. Veri setinizi 'TripFares' olarak arayın. Bağlantılı hizmetiniz olarak 'SQLDB'yi seçin. Tablo adı 'dbo'yu seçin. TripFares'in masa adı açılır. Schema **From connection/store 'dan**alma. Bittiğinde Tamam'ı tıklatın.

    ![Portal](media/lab-data-flow-data-share/dataflow11.png)
1. Verilerinizi doğrulamak için **Veri Önizleme** sekmesinden bir veri önizlemesi getirin.

    ![Portal](media/lab-data-flow-data-share/dataflow12.png)

### <a name="inner-join-tripdatacsv-and-tripfaressql"></a>İç join TripDataCSV ve TripFaresSQL

1. Yeni bir dönüşüm eklemek için, 'TripDataCSV'nin sağ alt köşesindeki artı simgesini tıklatın. **Birden Çok giriş/çıktı**altında **Katıl'ı**seçin.

    ![Portal](media/lab-data-flow-data-share/join1.png)
1. Join transformation'ınızı 'InnerJoinWithTripFares' olarak adlandırın. Sağ akış açılır tarafından 'TripFaresSQL' seçeneğini belirleyin. Birleştirme türü olarak **İç'i** seçin. Veri akışını eşlemedeki farklı birleştirme türleri hakkında daha fazla bilgi edinmek için [birleştirme türlerine](https://docs.microsoft.com/azure/data-factory/data-flow-join#join-types)bakın.

    Birleştirme koşulları açılır yoluyla her akıştan hangi sütunlarla eşleşip eşleştirmek **istediğinizi** seçin. Ek birleştirme koşulu eklemek için, varolan bir koşulun yanındaki artı simgesini tıklatın. Varsayılan olarak, tüm birleştirme koşulları bir AND işleci ile birleştirilir, bu da bir eşleşme için tüm koşulların karşılanması gerektiği anlamına gelir. Bu laboratuvarda, sütunlar `medallion`üzerinde `hack_license`maç `vendor_id`istiyorum , , ve`pickup_datetime`

    ![Portal](media/lab-data-flow-data-share/join2.png)
1. Bir veri önizlemesi ile birlikte 25 sütuna başarıyla katıldığınızı doğrulayın.

    ![Portal](media/lab-data-flow-data-share/join3.png)

### <a name="aggregate-by-payment_type"></a>payment_type göre toplam

1. Birleştirme dönüşümünüzü tamamladıktan sonra, 'InnerJoinWithTripFares'in yanındaki artı simgesine tıklayarak toplu bir dönüşüm ekleyin. **Schema değiştirici**altında **Toplam'ı** seçin.

    ![Portal](media/lab-data-flow-data-share/agg1.png)
1. Toplu dönüşümünüzü 'AggregateByPaymentType' olarak adlandırın. Sütuna göre grup olarak seçin. `payment_type`

    ![Portal](media/lab-data-flow-data-share/agg2.png)
1. **Agregalar** sekmesine gidin. Burada iki toplama belirteceksiniz:
    * Ödeme türüne göre gruplanan ortalama ücret
    * Ödeme türüne göre gruplanan toplam seyahat mesafesi

    İlk olarak, ortalama ücret ifadesini oluşturursunuz. Ekle veya bir **sütun seçin**etiketli metin kutusuna ,'average_fare' girin.

    ![Portal](media/lab-data-flow-data-share/agg3.png)
1. Toplama ifadesi girmek için, **ifadeyi gir**etiketli mavi kutuyu tıklatın. Bu, giriş şeması, yerleşik işlevler ve işlemler ve kullanıcı tanımlı parametreler kullanarak veri akışı ifadeleri oluşturmak için kullanılan bir araç olan veri akışı ifade oluşturucusunu açar. İfade oluşturucu yetenekleri hakkında daha fazla bilgi [için, ifade oluşturucu belgeleri](https://docs.microsoft.com/azure/data-factory/concepts-data-flow-expression-builder)bakın.

    Ortalama ücreti almak için, `avg()` `total_amount` sütun dökümunu bir ortasayı yada `toInteger()`. Veri akışı ifade dilinde, bu `avg(toInteger(total_amount))`olarak tanımlanır. **Kaydet'i** tıklatın ve bittiğinde bitirin.

    ![Portal](media/lab-data-flow-data-share/agg4.png)
1. Ek bir toplama ifadesi eklemek için, `average_fare`yanındaki artı simgesine tıklayın. **Sütun Ekle'yi**seçin.

    ![Portal](media/lab-data-flow-data-share/agg5.png)
1. Ekle veya bir **sütun seçin**etiketli metin kutusuna 'total_trip_distance' girin. Son adımda olduğu gibi, ifadeyi girmek için ifade oluşturucuyu açın.

    Toplam yolculuk mesafesini almak `sum()` için, `trip_distance` sütun dökümunu bir tamsayıya toplamak `toInteger()`için toplama işlevini kullanın. Veri akışı ifade dilinde, bu `sum(toInteger(trip_distance))`olarak tanımlanır. **Kaydet'i** tıklatın ve bittiğinde bitirin.

    ![Portal](media/lab-data-flow-data-share/agg6.png)
1. **Veri Önizleme** sekmesinde dönüşüm mantığınızı test edin. Gördüğünüz gibi, öncekinden önemli ölçüde daha az satır ve sütun vardır. Yalnızca üç grup tarafından ve bu dönüşümde tanımlanan toplama sütunları aşağı devam ediyor. Örnekte yalnızca beş ödeme türü grubu olduğundan, yalnızca beş satır çıktı alınır.

    ![Portal](media/lab-data-flow-data-share/agg7.png)

### <a name="configure-you-azure-synapse-analytics-sink"></a>Azure Synapse Analytics lavabosu yapılandırma

1. Dönüşüm mantığımızı tamamladığımızda, verilerimizi Bir Azure Synapse Analytics tablosuna batırmaya hazırız. **Hedef** bölümüne bir lavabo dönüşümü ekleyin.

    ![Portal](media/lab-data-flow-data-share/sink1.png)
1. Lavabonuzu 'SQLDWSink' olarak adlandırın. Yeni bir Azure Synapse Analytics veri kümesi oluşturmak için lavabo veri kümesi alanının yanında **Yeni'yi** tıklatın.

    ![Portal](media/lab-data-flow-data-share/sink2.png)

1. Azure **Synapse Analytics (eski adıyla SQL DW)** döşemesini seçin ve devam et'i tıklatın.

    ![Portal](media/lab-data-flow-data-share/sink3.png)
1. Veri setinizi 'AggregatedTaxiData' olarak arayın. Bağlantılı hizmetiniz olarak 'SQLDW'yi seçin. **Yeni tablo oluştur'u** seçin ve yeni tablo dbo'yu adlandırın. Toplu Taksi Verileri. Tamamlandığında Tamam'ı tıklatın

    ![Portal](media/lab-data-flow-data-share/sink4.png)
1. Lavabonun **Ayarlar** sekmesine gidin. Yeni bir tablo oluşturduğumuziçin tablo eyleminin altında **yeniden oluşturma tablosunu** seçmemiz gerekir. Sırayla mı yoksa toplu olarak mı eklediğimizi seve **seve**etkinleştir'in.

    ![Portal](media/lab-data-flow-data-share/sink5.png)

Veri akışınızı başarıyla oluşturdunuz. Şimdi bir boru hattı faaliyetinde çalıştırma zamanı.

### <a name="debug-your-pipeline-end-to-end"></a>Ardışık hatınızın uçuca hata ayıklama

1. **IngestAndTransformData** ardışık boru hattı için sekmeye geri dön. 'IngestIntoADLS' kopyalama etkinliğindeki yeşil kutuya dikkat edin. 'JoinAndAggregateData' veri akışı etkinliğine sürükleyin. Bu, veri akışı etkinliğinin yalnızca kopya başarılı olduğunda çalışmasına neden olan bir 'başarı' oluşturur.

    ![Portal](media/lab-data-flow-data-share/pipeline1.png)
1. Kopyalama etkinliği için yaptığımız gibi, hata ayıklama çalışmasını yürütmek için **Hata Ayıklama'yı** tıklatın. Hata ayıklama çalıştırmaları için veri akışı etkinliği, yeni bir küme döndürmek yerine etkin hata ayıklama kümesini kullanır. Bu boru hattının uygulanması bir dakikadan biraz fazla sürecek.

    ![Portal](media/lab-data-flow-data-share/pipeline2.png)
1. Kopyalama etkinliği gibi, veri akışı da etkinliğin tamamlanmasıyla ilgili gözlük simgesi tarafından erişilen özel bir izleme görünümüne sahiptir.

    ![Portal](media/lab-data-flow-data-share/pipeline3.png)
1. İzleme görünümünde, her yürütme aşamasında yürütme süreleri ve satırları ile birlikte basitleştirilmiş bir veri akışı grafiği görebilirsiniz. Doğru yapılırsa, bu etkinlikte 49.999 satırı beş satıra toplamanız gerekir.

    ![Portal](media/lab-data-flow-data-share/pipeline4.png)
1. Yürütmesi hakkında bölümleme bilgileri ve yeni/güncelleştirilmiş/bırakılan sütunlar gibi ek ayrıntılar almak için bir dönüşümü tıklatabilirsiniz.

    ![Portal](media/lab-data-flow-data-share/pipeline5.png)

Bu laboratuvarın veri fabrikası bölümünü tamamladınız. Kaynaklarınızı tetikleyicilerle operasyonel hale getirmek istiyorsanız yayımlayın. Kopyalama etkinliğini kullanarak Azure SQL Veritabanı'ndan Azure Veri Gölü Depolamasına veri yutmuş ve ardından bu verileri bir Azure Synapse Analytics'e toplayan bir ardışık ardışık işlem aygıtı çalıştırdın. SQL Server'ın kendisine bakarak verilerin başarıyla yazıldığını doğrulayabilirsiniz.

## <a name="share-data-using-azure-data-share"></a>Azure Veri Paylaşımı'nı kullanarak veri paylaşma

Bu bölümde, Azure portalını kullanarak yeni bir veri paylaşımını nasıl ayarlayabileceğinizi öğreneceksiniz. Bu, Azure Veri Gölü Deposu Gen2 ve Azure SQL Veri Ambarı'ndan veri kümeleri içeren yeni bir veri paylaşımı oluşturmayı içerir. Ardından, veri tüketicilerine kendileriyle paylaşılan verileri otomatik olarak yenileme seçeneği sunan bir anlık görüntü çizelgesi yapılandırırsınız. Ardından, alıcıları veri paylaşımınıza davet elersiniz. 

Bir veri paylaşımı oluşturduktan sonra şapkaları değiştirip *veri tüketicisi*olursunuz. Veri tüketicisi olarak, veri paylaşımı davetini kabul etme, verilerin alınmasını istediğiniz yeri yapılandırma ve veri kümelerini farklı depolama konumlarına eşleme akışında gezinirsiniz. Ardından, sizinle paylaşılan verileri belirtilen hedefe kopyalayan bir anlık görüntü tetiklersiniz. 

### <a name="sharing-data-data-provider-flow"></a>Veri paylaşımı (Veri Sağlayıcı akışı)

1. Azure portalını Microsoft Edge veya Google Chrome'da açın.

1. Sayfanın üst kısmındaki arama çubuğunu kullanarak **Veri Paylaşımlarını** arayın

    ![Portal](media/lab-data-flow-data-share/portal-ads.png)

1. Addaki 'Sağlayıcı' ile veri paylaşımı hesabını seçin. Örneğin, **DataProvider0102**. 

1. **Verilerinizi paylaşmaya başlat'ı** seçin

    ![Paylaşmaya başla](media/lab-data-flow-data-share/ads-start-sharing.png)

1. Yeni veri paylaşımınızı yapılandırmaya başlamak için **+Oluştur'u** seçin. 

1. **Paylaşım adı altında,** seçtiğiniz bir ad belirtin. Bu, veri tüketiciniz tarafından görülecek olan paylaşım adıdır, bu nedenle TaxiData gibi açıklayıcı bir ad verdiğinizden emin olun.

1. **Açıklama**altında , veri paylaşımının içeriğini açıklayan bir cümle koyun. Veri paylaşımı, Azure SQL Veri Ambarı ve Azure Veri Gölü Deposu da dahil olmak üzere çeşitli mağazalarda depolanan dünya çapındaki taksi yolculuğu verilerini içerir. 

1. **Kullanım Koşulları**uyarınca, veri tüketicinizin uymasını istediğiniz bir dizi koşul belirtin. Bazı örnekler arasında "Bu verileri kuruluşunuz dışında dağıtmayın" veya "Yasal sözleşmeye başvurun" verilebilir. 

    ![Ayrıntıları paylaşın](media/lab-data-flow-data-share/ads-details.png)

1. **Devam**'ı seçin. 

1. **Veri Seti Ekle'yi** seçin 

    ![Veri kümesi ekle](media/lab-data-flow-data-share/add-dataset.png)

1. ADF dönüşümlerinizin geldiği Azure SQL Veri Ambarı'ndan bir tablo seçmek için **Azure SQL Veri Ambarı'nı** seçin.

    ![Veri kümesi ekle](media/lab-data-flow-data-share/add-dataset-sql.png)

> [!NOTE]
> Azure SQL Veri Ambarı artık Azure Synapse Analytics olarak bilinir

1. Devam etmeden önce çalıştırmak için bir komut dosyası verilecektir. Sağlanan komut dosyası, Azure Veri Paylaşımı MSI'sinin kendi adına kimlik doğrulaması için SQL veritabanında bir kullanıcı oluşturur. 

> [!IMPORTANT]
> Komut dosyasını çalıştırmadan önce, kendinizi SQL Server için Active Directory Admin olarak ayarlamanız gerekir. 

1. Yeni bir sekme açın ve Azure portalına gidin. Veri paylaşmak istediğiniz veritabanında bir kullanıcı oluşturmak için sağlanan komut dosyasını kopyalayın. Bunu, AAD kimlik doğrulaması kullanarak Sorgu Gezgini 'ni (önizleme) kullanarak EDW veritabanına giriş yaparak yapın. 

    Oluşturulan kullanıcının parantez içinde yer alacak şekilde komut dosyasını değiştirmeniz gerekir. Örneğin:
    
    harici oturum açmadan kullanıcı [dataprovider-xxxx] oluşturun;  exec sp_addrolemember db_owner, [dataprovider-xxxx];
    
1. Veri paylaşımınıza veri kümeleri eklediğiniz Azure Veri Paylaşımı'na geri dön. 

1. SQL Veri Ambarı için **EDW'yi** seçin ve tablo için **AggregatedTaxiData'yı** seçin. 

1. **Veri Kümesi Ekle'yi** seçin

    Artık veri setimizin bir parçası olan bir SQL tablomuz var. Ardından, Azure Veri Gölü Deposu'ndan ek veri kümeleri ekleriz. 

1. **Veri Seti Ekle'yi** seçin ve Azure Veri Gölü **Deposu Gen2'yi** seçin

    ![Veri kümesi ekle](media/lab-data-flow-data-share/add-dataset-adls.png)

1. **Sonraki'ni** Seçin

1. *Wwtaxidata*genişletin. *Boston Taksi Verilerini*Genişletin. Dosya düzeyine kadar paylaşabileceğinize dikkat edin. 

1. Tüm klasörü veri paylaşımınıza eklemek için *Boston Taksi Verileri* klasörünü seçin. 

1. **Veri Seti Ekle'yi** seçin

1. Eklenen veri kümelerini gözden geçirin. Veri paylaşımınıza bir SQL tablosu ve bir ADLSGen2 klasörü eklenmelidir. 

1. **Devam Et'i** seçin

1. Bu ekranda, veri paylaşımınıza alıcı ekleyebilirsiniz. Eklediğiniz alıcılar, veri paylaşımınız için davet alır. Bu laboratuvarın amacı için, 2 e-posta adresi eklemeniz gerekir:

    1. Içinde bulunan Azure aboneliğinin e-posta adresi. 

        ![Alıcı ekleme](media/lab-data-flow-data-share/add-recipients.png)

    1. Adlı *janedoe@fabrikam.com*kurgusal veri tüketicisi ekleyin.

1. Bu ekranda, veri tüketiciniz için anlık görüntü ayarı yapılandırabilirsiniz. Bu, sizin tanımladığınız bir aralıkta verilerinizin düzenli güncelleştirmelerini almalarını sağlar. 

1. **Anlık Görüntü Çizelgesi'ni** denetleyin ve *Yinelenen* Düşüş'ü kullanarak verilerinizin saatlik yenilenmesini yapılandırın.  

1. **Oluştur**’u seçin.

    Artık etkin bir veri paylaşımınız var. Veri paylaşımı oluştururken veri sağlayıcısı olarak görebileceğiniz leri gözden geçirmenizi sağlar. 

1. Oluşturduğunuz **Veri**Sağlayıcısı başlıklı veri paylaşımını seçin. **Veri Paylaşımı'nda**Gönderilen **Paylaşımlar'ı** seçerek bu sayfaya gidebilirsiniz. 

1. Anlık Görüntü çizelgesine tıklayın. İsterseniz anlık görüntü zamanlamasını devre dışı kullanabilirsiniz. 

1. Ardından, **Veri Kümeleri** sekmesini seçin. Oluşturulduktan sonra bu veri paylaşımına ek veri kümeleri ekleyebilirsiniz. 

1. **Abonelikleri Paylaş** sekmesini seçin. Veri tüketiciniz davetinizi henüz kabul etmediğinden, henüz paylaşım aboneliği yok.

1. **Davetler** sekmesine gidin. Burada bekleyen davet(ler) listesini görürsünüz. 

    ![Bekleyen davetiyeler](media/lab-data-flow-data-share/pending-invites.png)

1. Daveti *janedoe@fabrikam.com*seçin. Sil’i seçin. Alıcınız daveti henüz kabul etmediyse, bunu artık yapamaz. 

1. **Geçmiş** sekmesini seçin. Veri tüketiciniz henüz davetinizi kabul etmediği ve anlık görüntü tetiklemediği için henüz hiçbir şey görüntülenmez. 

### <a name="receiving-data-data-consumer-flow"></a>Veri alma (Veri tüketici akışı)

Artık veri paylaşımımızı gözden geçirdiğimize göre, bağlam ı değiştirmeye ve veri tüketici şapkamızı takmaya hazırız. 

Artık Microsoft Azure'dan gelen kutunuzda bir Azure Veri Paylaşımı daveti olması gerekir. Outlook Web Erişimi'ni başlatın (outlook.com) ve Azure aboneliğiniz için sağlanan kimlik bilgilerini kullanarak oturum açın.

Almış olması gereken e-postada "Daveti > görüntüle"ye tıklayın. Bu noktada, veri sağlayıcılarının veri paylaşımına davetini kabul ederken veri tüketici seçimlerini simüle eceyeceniz. 

![E-posta daveti](media/lab-data-flow-data-share/email-invite.png)

Bir abonelik seçmeniz istenebilir. Bu laboratuvar için çalıştığınız aboneliği seçtiğinizden emin olun. 

1. *DataProvider*başlıklı davetiyeye tıklayın. 

1. Bu Davet ekranında, daha önce veri sağlayıcısı olarak yapılandırdığınız veri paylaşımıyla ilgili çeşitli ayrıntılar fark edeceksiniz. Ayrıntıları gözden geçirin ve sağlanırsa kullanım koşullarını kabul edin.

1. Laboratuvarınız için zaten var olan Abonelik ve Kaynak Grubu'nu seçin. 

1. **Veri paylaşımı hesabı**için **DataConsumer'ı**seçin. Ayrıca yeni bir veri paylaşım hesabı da oluşturabilirsiniz. 

1. Alınan **paylaşım adının**yanında, varsayılan paylaşım adının veri sağlayıcısı tarafından belirtilen ad olduğunu fark esiniz. Paylaşıma, almak üzere olduğunuz verileri açıklayan samimi bir ad verin (örneğin **TaxiDataShare**).

    ![Davet kabul eder](media/lab-data-flow-data-share/consumer-accept.png)

1. Şimdi kabul **etmeyi ve yapılandırmayı** seçebilir veya **daha sonra kabul edip yapılandırabilirsiniz.** Şimdi kabul etmeyi ve yapılandırmayı seçerseniz, tüm verilerin kopyalanması gereken bir depolama hesabı belirtirsiniz. Daha sonra kabul etmeyi ve yapılandırmayı seçerseniz, paylaşımdaki veri kümelerinin eşlenenleri çözülmez ve bunları el ile eşlebilmeniz gerekir. Bunu daha sonra seçeceğiz. 

1. **Daha sonra Kabul Et'i seçin ve yapılandırın.** 

    Bu seçeneği yapılandırmada, bir paylaşım aboneliği oluşturulur, ancak hiçbir hedef eşlenmediğinden verilerin inmek için hiçbir yeri yoktur. 

    Daha sonra, veri paylaşımı için veri kümesi eşlemeleri yapılandıracağız. 

1. Alınan Paylaşım'ı (adım 5'te belirttiğiniz ad) seçin.

    **Tetikleyici anlık görüntüsü** soluk, ancak paylaşım Etkin. 

1. **Datasets** sekmesini seçin. Her veri kümesinin Eşlenmemiş olduğuna dikkat edin, bu da verileri kopyalamak için bir hedefi olmadığı anlamına gelir. 

    ![eşlenmemiş veri kümeleri](media/lab-data-flow-data-share/unmapped.png)

1. SQL Veri Ambarı Tablosu'nu seçin ve ardından **Hedefe + Harita'yı**seçin.

1. Ekranın sağ tarafında, **Hedef Veri Türü** açılır seçeneğini belirleyin. 

    SQL verilerini çok çeşitli veri depolarına haritalandırabilirsiniz. Bu durumda, bir Azure SQL Veritabanı'nın eşlemesini yapacağız.

    ![eşleme](media/lab-data-flow-data-share/mapping-options.png)
    
    (İsteğe bağlı) Hedef veri türü olarak **Azure Veri Gölü Deposu Gen2'yi** seçin. 
    
    (İsteğe bağlı) Çalıştığınız Abonelik, Kaynak Grubu ve Depolama hesabını seçin. 
    
    (İsteğe bağlı) Verileri csv veya parke formatında veri gölünüze almayı seçebilirsiniz. 

1. Hedef **veri türünün**yanında Azure SQL Veritabanı'nı seçin. 

1. Çalıştığınız Abonelik, Kaynak Grubu ve Depolama hesabını seçin. 

    ![sql haritası](media/lab-data-flow-data-share/map-to-sqldb.png)

1. Devam etmeden önce, sağlanan komut dosyasını çalıştırarak SQL Server'da yeni bir kullanıcı oluşturmanız gerekir. İlk olarak, panonuza sağlanan komut dosyasını kopyalayın. 

1. Yeni bir Azure portalı sekmesi açın. Bir anda geri dönmeniz gerektiğinden varolan sekmenizi kapatmayın. 

1. Açtığınız yeni sekmede **SQL veritabanlarına**gidin.

1. SQL veritabanını seçin (aboneliğinizde yalnızca bir tane olmalıdır). SQL Veri Ambarı'nı seçmemeye dikkat edin. 

1. Sorgu düzenleyicisini seçin **(önizleme)**

1. Sorgu düzenleyicisinde oturum açmak için AAD kimlik doğrulamasını kullanın. 

1. Veri paylaşımınızda sağlanan sorguyu çalıştırın (adım 14'te panoya kopyalanır). 

    Bu komut, Azure Veri Paylaşımı hizmetinin Azure Hizmetleri için Yönetilen Kimlikler'i kullanmasına olanak tanır ve verileri kopyalayabilmek için SQL Server'da kimlik doğrulaması yapılır. 

1. Özgün sekmeye geri dön ve **hedeflemek için Harita'yı**seçin.

1. Ardından, veri kümesinin bir parçası olan Azure Veri Gölü Gen2 klasörünü seçin ve bir Azure Blob Depolama hesabına eşleyin. 

    ![depolama](media/lab-data-flow-data-share/storage-map.png)

    Tüm veri kümeleri eşlendikten sonra, artık veri sağlayıcısından veri almaya başlamaya hazırsınız. 

    ![Eşlenmiş](media/lab-data-flow-data-share/all-mapped.png)
    
1. **Ayrıntılar'ı**seçin. 

    Veri paylaşımının kopyalanması gereken hedefleri **olduğundan, Tetikleyici anlık görüntüsünün** artık soluk olmadığına dikkat edin.

1. Tetikle anlık görüntü -tam kopya > seçin. 

    ![Tetikleyici](media/lab-data-flow-data-share/trigger-full.png)

    Bu, verileri yeni veri paylaşım hesabınıza kopyalamaya başlar. Gerçek bir dünya senaryosunda, bu veriler üçüncü bir taraftan geliyor olurdu. 

    Verilerin rastlaması yaklaşık 3-5 dakika sürer. **Geçmiş** sekmesine tıklayarak ilerlemeyi izleyebilirsiniz. 

    Beklerken, özgün veri paylaşımına (DataProvider) gidin ve **Paylaşım Abonelikleri** ve **Geçmiş** sekmesinin durumunu görüntüleyin. Artık etkin bir abonelik olduğunu ve bir veri sağlayıcısı olarak veri tüketicisinin kendileriyle paylaşılan verileri almaya başladığındada da izleyebilirsiniz. 

1. Veri tüketicisinin veri paylaşımına geri gidin. Tetikleyicinin durumu başarılı olduktan sonra, verilerin ilgili mağazalara düştüğünü görmek için hedef SQL veritabanına ve veri gölüne gidin. 

Tebrikler, laboratuarı tamamladınız!


