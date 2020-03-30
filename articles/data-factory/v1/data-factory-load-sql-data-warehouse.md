---
title: Terabaytlardır veriyi SQL Veri Ambarına yükleme
description: Azure Veri Fabrikası ile 15 dakikanın altında 1 TB verinin Azure SQL Veri Ambarına nasıl yüklenebileceğini gösterir
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.assetid: a6c133c0-ced2-463c-86f0-a07b00c9e37f
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 079d29c241cfbbdcc991f024c07b07b378670c10
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80130883"
---
# <a name="load-1-tb-into-azure-sql-data-warehouse-under-15-minutes-with-data-factory"></a>Veri Fabrikası ile Azure SQL Veri Ambarına 15 dakikanın altında 1 TB yükleyin
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [Veri Fabrikası'nı kullanarak Verileri Azure SQL Veri Ambarına kopyala'ya](../connector-azure-sql-data-warehouse.md)bakın.


[Azure SQL Veri Ambarı,](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) hem ilişkisel hem de ilişkisel olmayan büyük hacimli verileri işleyebilen bulut tabanlı, ölçeklendirilebilen bir veritabanıdır.  Büyük ölçüde paralel işleme (MPP) mimarisi üzerine inşa edilen SQL Veri Ambarı, kurumsal veri ambarı iş yükleri için optimize edilmiştir.  Depolamayı ölçeklendirme ve bağımsız olarak hesaplama esnekliğiile bulut elastikiyeti sunar.

Azure SQL Veri Ambarı ile başlarken artık **Azure Veri Fabrikası'nı**kullanmak her zamankinden daha kolay.  Azure Veri Fabrikası, bir SQL Veri Ambarını mevcut sisteminizdeki verilerle doldurmak ve SQL Veri Ambarını değerlendirirken ve analitiğinizi oluşturarak değerli zamandan tasarruf etmek için kullanılabilen, tam olarak yönetilen bulut tabanlı bir veri tümleştirme hizmetidir Çözümleri. Azure Veri Fabrikası'nı kullanarak Azure SQL Veri Ambarı'na veri yüklemenin temel avantajları şunlardır:

* **Kurulumu kolay:** Komut dosyası gerektirmeden 5 adımlı sezgisel sihirbaz.
* **Zengin veri deposu desteği:** şirket içi ve bulut tabanlı veri depolarından oluşan zengin bir dizi için yerleşik destek.
* **Güvenli ve uyumlu**: veriler HTTPS veya ExpressRoute üzerinden aktarılır ve küresel hizmet varlığı verilerinizin coğrafi sınırdan asla çıkmamasını sağlar
* **PolyBase – Polybase kullanarak benzersiz performans,** verileri Azure SQL Veri Ambarı'na taşımanın en etkili yoludur. Hazırlama blob özelliğini kullanarak, Polybase'in varsayılan olarak desteklediği Azure Blob depolamasının yanı sıra her türlü veri deposundan yüksek yük hızları elde edebilirsiniz.

Bu makalede, Azure Blob Depolama'dan 1-TB verilerini 15 dakikadan daha az bir mesafede, 1,2 GBps'nin üzerinde bir iş seviyesinde Azure SQL Veri Ambarına yüklemek için Veri Fabrikası Kopyalama Sihirbazı'nı nasıl kullanacağınızı gösterir.

Bu makalede, Kopya Sihirbazı'nı kullanarak verileri Azure SQL Veri Ambarı'na taşımak için adım adım yönergeler sağlanmaktadır.

> [!NOTE]
>  Verileri Azure SQL Veri Ambarı'na/Azure Veri Ambarından taşımadaki Veri Fabrikası'nın yetenekleri hakkında genel bilgi için Azure Veri Fabrikası makalesini [kullanarak verileri Azure SQL Veri Ambarı'na taşıyın ve buradan](data-factory-azure-sql-data-warehouse-connector.md) aktarın'a bakın.
>
> Ayrıca Visual Studio, PowerShell, vb kullanarak boru hatları oluşturabilirsiniz. Eğitime Bakın: Azure Veri Fabrikası'nda Kopyalama Etkinliğini kullanmak için adım adım yönergeleri kullanarak hızlı bir geçiş için [Azure Blob'dan Azure SQL Veritabanı'na verileri kopyalayın.](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)  
>
>

## <a name="prerequisites"></a>Ön koşullar
* Azure Blob Depolama: Bu deneme, TPC-H test veri kümesini depolamak için Azure Blob Depolama (GRS) kullanır.  Azure depolama hesabınız yoksa, depolama [hesabı oluşturmayı](../../storage/common/storage-account-create.md)öğrenin.
* [TPC-H](http://www.tpc.org/tpch/) verileri: TPC-H'yi test veri seti olarak kullanacağız.  Bunu yapmak için, veri `dbgen` kümesini oluşturmanıza yardımcı olan TPC-H araç setinden kullanmanız gerekir.  Ya [TPC Tools](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp) `dbgen` kaynak kodu indirebilir ve kendiniz derleyebilir, ya da [GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/TPCHTools)derlenmiş ikili indirebilirsiniz.  10 dosyaya yayılmış tablo için 1 TB düz dosya `lineitem` oluşturmak için aşağıdaki komutlarla dbgen.exe çalıştırın:

  * `Dbgen -s 1000 -S **1** -C 10 -T L -v`
  * `Dbgen -s 1000 -S **2** -C 10 -T L -v`
  * …
  * `Dbgen -s 1000 -S **10** -C 10 -T L -v`

    Şimdi oluşturulan dosyaları Azure Blob'a kopyalayın.  ADF Copy'yi kullanarak bunu nasıl yapacağınız için [Azure Veri Fabrikası'nı kullanarak verileri şirket içi bir dosya sistemine taşıyın](data-factory-onprem-file-system-connector.md) ve şirket içi dosya sistemine bakın.    
* Azure SQL Veri Ambarı: Bu deneme, verileri 6.000 DWUs ile oluşturulan Azure SQL Veri Ambarı'na yükler

    SQL [Veri Ambarı](../../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) veritabanı oluşturma hakkında ayrıntılı talimatlar için Azure SQL Veri Ambarı Oluştur'a bakın.  Polybase kullanarak SQL Veri Ambarı'na mümkün olan en iyi yük performansını elde etmek için, Performans ayarında izin verilen maksimum veri ambarbirimi (DWUs) sayısını seçeriz, bu da 6.000 DUs'dur.

  > [!NOTE]
  > Azure Blob'dan yükleme yaparken, veri yükleme performansı SQL Veri Ambarı'nda yapılandırdığınız DWUs sayısıyla doğru orantılıdır:
  >
  > 1.000 DWU SQL Veri Ambarı içine 1 TB yükleme 87 dakika (~ 200 MBps iş çıkış) 2.000 DWU SQL Veri Ambarı içine 1 TB yükleme 46 dakika (~ 380 MBps iş girdisi) 6.000 DWU WU SQL SQL Sql Veri Ambarı içine 1 TB yükleme 14 dakika (~ 1.2 GBps giriş) sürer
  >
  >

    6.000 DWUs'lu bir SQL Veri Ambarı oluşturmak için Performans kaydırıcısını sağa doğru taşıyın:

    ![Performans kaydırıcısı](media/data-factory-load-sql-data-warehouse/performance-slider.png)

    6.000 DWUs ile yapılandırılmamış varolan bir veritabanı için Azure portalLarını kullanarak ölçeklendirebilirsiniz.  Azure portalındaki veritabanına gidin ve aşağıdaki resimde gösterilen **Genel Bakış** panelinde bir **Ölçek** düğmesi vardır:

    ![Ölçek düğmesi](media/data-factory-load-sql-data-warehouse/scale-button.png)    

    Aşağıdaki paneli açmak için **Ölçekle** düğmesini tıklatın, kaydırıcıyı maksimum değere taşıyın ve **Kaydet** düğmesini tıklatın.

    ![Ölçek iletişim kutusu](media/data-factory-load-sql-data-warehouse/scale-dialog.png)

    Bu deneme, kaynak sınıfLarını `xlargerc` kullanarak verileri Azure SQL Veri Ambarı'na yükler.

    Mümkün olan en iyi iş elde etmek için, `xlargerc` kopyanın kaynak sınıfına ait bir SQL Veri Ambarı kullanıcısı kullanılarak gerçekleştirilmesi gerekir.  [Kullanıcı kaynağı sınıfı örneğini](../../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md)değiştir'i izleyerek bunu nasıl yapacağınızı öğrenin.  
* Aşağıdaki DDL deyimini çalıştırarak Azure SQL Veri Ambarı veritabanında hedef tablo şeması oluşturun:

    ```SQL  
    CREATE TABLE [dbo].[lineitem]
    (
        [L_ORDERKEY] [bigint] NOT NULL,
        [L_PARTKEY] [bigint] NOT NULL,
        [L_SUPPKEY] [bigint] NOT NULL,
        [L_LINENUMBER] [int] NOT NULL,
        [L_QUANTITY] [decimal](15, 2) NULL,
        [L_EXTENDEDPRICE] [decimal](15, 2) NULL,
        [L_DISCOUNT] [decimal](15, 2) NULL,
        [L_TAX] [decimal](15, 2) NULL,
        [L_RETURNFLAG] [char](1) NULL,
        [L_LINESTATUS] [char](1) NULL,
        [L_SHIPDATE] [date] NULL,
        [L_COMMITDATE] [date] NULL,
        [L_RECEIPTDATE] [date] NULL,
        [L_SHIPINSTRUCT] [char](25) NULL,
        [L_SHIPMODE] [char](10) NULL,
        [L_COMMENT] [varchar](44) NULL
    )
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    ```
  Ön koşul adımları tamamlandığında, kopyalama etkinliğini Kopyalama Sihirbazı'nı kullanarak yapılandırmaya hazırız.

## <a name="launch-copy-wizard"></a>Kopyalama Sihirbazı'nı başlatma
1. [Azure portalına](https://portal.azure.com)giriş yapın.
2. Sol üst köşeden **kaynak oluştur'u** tıklatın, **İstihbarat + analitik'i**tıklatın ve **Veri Fabrikası'nı**tıklatın.
3. Yeni **veri fabrikası** bölmesinde:

   1. **Adı**için **LoadIntoSQLDWDataFactory** girin.
       Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Hata alırsanız: **Veri fabrikası adı "LoadIntoSQLDWDataFactory" kullanılamıyor,** veri fabrikasının adını değiştirin (örneğin, adınızLoadIntoSQLDWDataFactory) ve yeniden oluşturmayı deneyin. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](data-factory-naming-rules.md) konusuna bakın.  
   2. Azure **aboneliğinizi**seçin.
   3. Kaynak Grubu için aşağıdaki adımlardan birini uygulayın:
      1. Var olan bir kaynak grubu seçmek için **Var olanı kullan**’ı seçin.
      2. Bir kaynak grubunun adını girmek için **Yeni oluştur**’u seçin.
   4. Veri fabrikası için bir **konum** seçin.
   5. Dikey pencerenin alt kısmındaki **Panoya sabitle** onay kutusunu seçin.  
   6. **Oluştur'u**tıklatın.
4. Oluşturma işlemi tamamlandıktan sonra, aşağıdaki görüntüde gösterildiği gibi **Data Factory** dikey penceresini görürsünüz:

   ![Data factory giriş sayfası](media/data-factory-load-sql-data-warehouse/data-factory-home-page-copy-data.png)
5. Data Factory giriş sayfasında **Veri kopyala** kutucuğuna tıklayarak **Kopyalama Sihirbazı**’nı başlatın.

   > [!NOTE]
   > Web tarayıcısının "Yetkilendiriliyor..." durumunda takıldığını görürseniz **Üçüncü taraf tanımlama bilgilerini ve site verilerini engelle** ayarını devre dışı bırakın/işaretini kaldırın (ya da) etkin durumda bırakıp **login.microsoftonline.com** için bir özel durum oluşturun ve ardından sihirbazı yeniden başlatmayı deneyin.
   >
   >

## <a name="step-1-configure-data-loading-schedule"></a>Adım 1: Veri yükleme zamanlamasını yapılandırma
İlk adım, veri yükleme zamanlamasını yapılandırmaktır.  

**Özellikler** sayfasında:

1. **Görev adı** için **CopyFromBlobToAzureSqlDataWarehouse** girin
2. **Şimdi Çalıştır** seçeneğini belirleyin.   
3. **İleri**'ye tıklayın.  

    ![Kopya Sihirbazı - Özellikler sayfası](media/data-factory-load-sql-data-warehouse/copy-wizard-properties-page.png)

## <a name="step-2-configure-source"></a>Adım 2: Kaynağı yapılandırma
Bu bölümde, kaynağı yapılandırma adımları gösterilmektedir: 1-TB TPC-H satır öğesi dosyalarını içeren Azure Blob.

1. Veri deposu olarak **Azure Blob Depolama'yı** seçin ve **İleri'yi**tıklatın.

    ![Kopya Sihirbazı - Kaynak sayfayı seçin](media/data-factory-load-sql-data-warehouse/select-source-connection.png)

2. Azure Blob depolama hesabının bağlantı bilgilerini doldurun ve **İleri'yi**tıklatın.

    ![Kopya Sihirbazı - Kaynak bağlantı bilgileri](media/data-factory-load-sql-data-warehouse/source-connection-info.png)

3. TPC-H satır öğesi dosyalarını içeren **klasörü** seçin ve **İleri'yi**tıklatın.

    ![Sihirbazı Kopyala - giriş klasörünü seçin](media/data-factory-load-sql-data-warehouse/select-input-folder.png)

4. **İleri'yi**tıklattığınızda, dosya biçimi ayarları otomatik olarak algılanır.  Sütun delimiter'in varsayılan virgül yerine '|' olduğundan emin olun.  Verileri önizledikten sonra **İleri'yi** tıklatın.

    ![Kopya Sihirbazı - dosya biçimi ayarları](media/data-factory-load-sql-data-warehouse/file-format-settings.png)

## <a name="step-3-configure-destination"></a>Adım 3: Hedefi yapılandırma
Bu bölümde, hedefin nasıl yapılandırılabildiğiniz gösterilmektedir: `lineitem` Azure SQL Veri Ambarı veritabanındaki tablo.

1. Hedef depo olarak **Azure SQL Veri Ambarı'nı** seçin ve **İleri'yi**tıklatın.

    ![Sihirbazı Kopyala - hedef veri deposu seçin](media/data-factory-load-sql-data-warehouse/select-destination-data-store.png)

2. Azure SQL Veri Ambarı için bağlantı bilgilerini doldurun.  Rolün `xlargerc` bir üyesi olan kullanıcıyı belirttiğinden emin olun (ayrıntılı yönergeler için **ön koşullar** bölümüne bakın) ve **İleri'yi**tıklatın.

    ![Kopya Sihirbazı - hedef bağlantı bilgileri](media/data-factory-load-sql-data-warehouse/destination-connection-info.png)

3. Hedef tabloyu seçin ve **İleri'yi**tıklatın.

    ![Kopya Sihirbazı - tablo eşleme sayfası](media/data-factory-load-sql-data-warehouse/table-mapping-page.png)

4. Şema eşleme sayfasında, "Sütun eşleme uygula" seçeneğini işaretlenmemiş olarak bırakın ve **İleri'yi**tıklatın.

## <a name="step-4-performance-settings"></a>Adım 4: Performans ayarları

**Izin polibase** varsayılan olarak denetlenir.  **İleri**'ye tıklayın.

![Kopya Sihirbazı - şema eşleme sayfası](media/data-factory-load-sql-data-warehouse/performance-settings-page.png)

## <a name="step-5-deploy-and-monitor-load-results"></a>Adım 5: Yük sonuçlarını dağıtma ve izleme
1. Dağıtmak için **Bitir** düğmesini tıklatın.

    ![Kopya Sihirbazı - özet sayfası](media/data-factory-load-sql-data-warehouse/summary-page.png)

2. Dağıtım tamamlandıktan sonra, `Click here to monitor copy pipeline` kopya çalıştırma ilerlemesini izlemek için tıklatın. **Etkinlik Windows** listesinde oluşturduğunuz kopya ardışık hattını seçin.

    ![Kopya Sihirbazı - özet sayfası](media/data-factory-load-sql-data-warehouse/select-pipeline-monitor-manage-app.png)

    Kaynaktan okunan ve hedefe yazılan veri hacmi, süre ve çalışma için ortalama iş hacmi de dahil olmak üzere, **etkinlik penceresi gezgininde** kopya çalıştırma ayrıntılarını doğru panelde görüntüleyebilirsiniz.

    Aşağıdaki ekran görüntüsünden de görebileceğiniz gibi, Azure Blob Depolama'dan SQL Veri Ambarı'na 1 TB kopyalayarak 14 dakika sürdü ve 1,22 GBps veri girişine etkili bir şekilde ulaştı!

    ![Kopya Sihirbazı - başarılı iletişim kutusu](media/data-factory-load-sql-data-warehouse/succeeded-info.png)

## <a name="best-practices"></a>En iyi uygulamalar
Azure SQL Veri Ambarı veritabanınızı çalıştırmak için en iyi birkaç uygulama şunlardır:

* KÜMELENMIŞ COLUMNSTORE INDEX'e yüklerken daha büyük bir kaynak sınıfı kullanın.
* Daha verimli birleştirmeler için, varsayılan yuvarlak robin dağıtımı yerine seçili bir sütuna göre karma dağıtımı kullanmayı düşünün.
* Daha hızlı yükleme hızları için, geçici veriler için yığın kullanmayı düşünün.
* Azure SQL Veri Ambarı yüklemeyi tamamladıktan sonra istatistikler oluşturun.

Ayrıntılar [için Azure SQL Veri Ambarı için en iyi uygulamalara](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-best-practices.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar
* [Veri Fabrikası Kopyalama Sihirbazı](data-factory-copy-wizard.md) - Bu makalede Kopyalama Sihirbazı hakkında ayrıntılı bilgi verilmektedir.
* [Etkinlik performansını ve ayarı kopyalama kılavuzunu kopyala](data-factory-copy-activity-performance.md) - Bu makalede, başvuru performansı ölçümleri ve ayarı kılavuzu bulunmaktadır.
