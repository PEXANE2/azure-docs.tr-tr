---
title: Azure Data Factory verileri Azure 'a kopyalama Veri Gezgini
description: Bu konuda, Azure Data Factory kopyalama aracını kullanarak Azure Veri Gezgini 'a veri alma (yükleme) hakkında bilgi edinirsiniz.
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: jasonh
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/15/2019
ms.openlocfilehash: 5eb05df7ed97839ef80798a752565234d180f0e2
ms.sourcegitcommit: 263a69b70949099457620037c988dc590d7c7854
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71268831"
---
# <a name="copy-data-to-azure-data-explorer-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Veri Gezgini veri kopyalama 

Azure Veri Gezgini, uygulamalar, Web siteleri ve IoT cihazları gibi birçok kaynaktan büyük miktarlarda veri akışı için gerçek zamanlı analizler için hızlı ve tam olarak yönetilen bir veri analizi hizmetidir. Verileri tekrarlayarak, ürünleri iyileştirmek, müşteri deneyimlerini geliştirmek, cihazları izlemek ve işlemleri yükseltmek için desenleri ve anormallikleri belirler. Yeni soruları keşfedin ve dakikalar içinde yanıt alın. Azure Data Factory, tam olarak yönetilen bulut tabanlı bir veri tümleştirme hizmetidir. Hizmeti kullanarak Azure Veri Gezgini veritabanınızı mevcut sisteminizdeki verilerle doldurabilir ve analiz çözümlerinizi oluştururken zamandan tasarruf edebilirsiniz.

Azure Data Factory Azure Veri Gezgini 'a veri yüklemek için aşağıdaki avantajları sunmaktadır:

* **Kolayca ayarlanır**: Komut dosyası gerekmeden, sezgisel beş adımlı bir sihirbaz.
* **Zengin veri deposu desteği**: Zengin bir şirket içi ve bulut tabanlı veri depoları kümesi için yerleşik destek. Ayrıntılı bir liste için [desteklenen veri depoları](/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats)tablosuna bakın.
* **Güvenli ve uyumlu**: Veriler HTTPS veya ExpressRoute üzerinden aktarılır. Küresel hizmet varlığı, verilerinizin hiçbir şekilde coğrafi sınır bırakmamasını sağlar.
* **Yüksek performans**: En fazla 1 GB/sn veri yükleme hızı Azure Veri Gezgini. Ayrıntılar için bkz. [etkinlik performansını kopyalama](/azure/data-factory/copy-activity-performance).

Bu makalede, Amazon S3 'ten Azure Veri Gezgini 'e veri yüklemek için Data Factory Veri Kopyalama aracının nasıl kullanılacağı gösterilmektedir. [Azure Blob depolama](/azure/data-factory/connector-azure-blob-storage), [Azure SQL VERITABANı](/azure/data-factory/connector-azure-sql-database), [Azure SQL veri ambarı](/azure/data-factory/connector-azure-sql-data-warehouse), [Google BigQuery](/azure/data-factory/connector-google-bigquery),[Oracle](/azure/data-factory/connector-oracle)ve [dosya sistemi](/azure/data-factory/connector-file-system)gibi diğer veri depolarından veri kopyalamak için benzer adımları izleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* [Azure Veri Gezgini kümesi ve veritabanı](create-cluster-database-portal.md)
* Veri kaynağı.

## <a name="create-a-data-factory"></a>Data factory oluştur

1. Portal > **analiz** > **Data Factory**sol üst köşesinde **kaynak oluştur** düğmesini (+) seçin.

   ![Yeni bir veri fabrikası oluşturma](media/data-factory-load-data/create-adf.png)

1. **Yeni Veri Fabrikası** sayfasında, aşağıdaki alanlar için değerler sağlayın ve **Oluştur**' u seçin.

    ![Yeni veri fabrikası sayfası](media/data-factory-load-data/my-new-data-factory.png)

    **Ayar**  | **Alan açıklaması**
    |---|---|
    | **Name** | Data Factory 'niz için genel olarak benzersiz bir ad girin. *"Data Factory Name \"loadadxdemo\" kullanılamıyor"* hatasını alırsanız Veri Fabrikası için farklı bir ad girin. Data Factory yapıtların adlandırma kuralları için bkz. [Data Factory adlandırma kuralları](/azure/data-factory/naming-rules).|
    | **Abonelik** | Veri fabrikasının oluşturulacağı Azure aboneliğinizi seçin. |
    | **Kaynak Grubu** | **Yeni oluştur** ' u seçin ve yeni bir kaynak grubunun adını girin. Mevcut bir kaynak grubunuz varsa **var olanı kullan**' ı seçin. |
    | **Sürüm** | **V2** 'yi seçin |
    | **Location** | Veri fabrikasının konumunu seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Data Factory tarafından kullanılan veri depoları başka konumlarda veya bölgelerde olabilir. |
    | | |

1. Oluşturma işlemini izlemek için araç çubuğunda bildirimler ' i seçin. Oluşturma işlemi tamamlandıktan sonra, oluşturduğunuz veri fabrikasına gidin. **Data Factory** giriş sayfası açılır.

   ![Data factory giriş sayfası](media/data-factory-load-data/data-factory-home-page.png)

1. Uygulamayı ayrı bir sekmede başlatmak için **yazar & İzleyici** kutucuğunu seçin.

## <a name="load-data-into-azure-data-explorer"></a>Verileri Azure 'a yükleme Veri Gezgini

Veriler birçok [veri deposu](/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats) türünden Azure Veri Gezgini yüklenebilir. Bu konuda, Amazon S3 ' den veri yükleme ayrıntıları verilmektedir.

Azure Data Factory kullanarak Azure Veri Gezgini 'a veri yüklemek için iki yol vardır:

* Azure Data Factory Kullanıcı arabirimi- [ **Yazar** sekmesi](/azure/data-factory/quickstart-create-data-factory-portal#create-a-data-factory)
* Bu makalede kullanılan [ **veri kopyalama** Azure Data Factory araç](/azure/data-factory/quickstart-create-data-factory-copy-data-tool) .

### <a name="copy-data-from-amazon-s3-source"></a>Amazon S3 (kaynak) konumundan veri kopyalama

1. **Haydi** başlayın sayfasında, veri kopyalama aracını başlatmak için **veri kopyalama** kutucuğunu seçin.

   ![Veri kopyalama araç kutucuğu](media/data-factory-load-data/copy-data-tool-tile.png)

1. **Özellikler** sayfasında, **görev adı** ' nı belirtin ve **İleri**' yi seçin.

    ![Kaynak özelliklerinden Kopyala](media/data-factory-load-data/copy-from-source.png)

1. **Kaynak veri deposu** sayfasında **+ Yeni bağlantı oluştur**' a tıklayın.

    ![Kaynak verileri bağlantı oluştur](media/data-factory-load-data/source-create-connection.png)

1. **Amazon S3**' i seçin ve ardından **devam** ' ı seçin.

    ![Yeni bağlı hizmet](media/data-factory-load-data/amazons3-select-new-linked-service.png)

1. **Yeni bağlı hizmet (Amazon S3)** sayfasında, aşağıdaki adımları uygulayın:

    ![Amazon S3 bağlı hizmetini belirtin](media/data-factory-load-data/amazons3-new-linked-service-properties.png)

    * Yeni bağlı hizmetinizin **adını** belirtin.
    * Açılan listeden **tümleştirme çalışma zamanı değeri aracılığıyla Bağlan** ' ı seçin.
    * **Erişim anahtarı kimlik** değerini belirtin.
    * Gizli dizi **erişim anahtarı** değerini belirtin.
    * Oluşturduğunuz bağlı hizmet bağlantısını test etmek için **Bağlantıyı Sına** ' yı seçin.
    * **Son**’u seçin.
    
    > [!NOTE]
    > Amazon S3 ' de, gezinti çubuğunda Amazon Kullanıcı adınızı seçin ve ardından **erişim anahtarınızı**bulmak Için **güvenlik kimlik bilgilerimi** seçin. 

1. **Kaynak veri deposu** sayfasında, yeni AmazonS31 bağlantınızı görürsünüz. **İleri**’yi seçin.

   ![Kaynak veri deposunun bağlantısı oluşturuldu](media/data-factory-load-data/source-data-store-created-connection.png)

1. **Girdi dosyasını veya klasörünü seçin** sayfasında:

    1. Kopyalamak istediğiniz klasöre/dosyaya gidin. Klasörü/dosyayı seçin.
    1. Kopyalama davranışını gerektiği şekilde seçin. **İkili kopyayı** işaretlenmemiş olarak bırakın.
    1. **İleri**’yi seçin.

    ![Girdi dosyasını veya klasörünü seçin](media/data-factory-load-data/source-choose-input-file.png)

1. **Dosya biçimleri ayarları** sayfasında, dosyanız için ilgili ayarları seçin ve **İleri**' ye tıklayın.

   ![Girdi dosyasını veya klasörünü seçin](media/data-factory-load-data/source-file-format-settings.png)

### <a name="copy-data-into-azure-data-explorer-destination"></a>Verileri Azure Veri Gezgini 'a (hedef) kopyalama

Azure Veri Gezgini yeni bağlı hizmet, verileri aşağıda belirtilen Azure Veri Gezgini hedef tablosuna (havuz) kopyalamak için oluşturulur.

#### <a name="create-the-azure-data-explorer-linked-service"></a>Azure Veri Gezgini bağlı hizmetini oluşturma

1. **Hedef veri deposu** sayfasında, mevcut bir veri deposu bağlantısını kullanabilir veya **+ Yeni bağlantı oluştur**' a tıklayarak yeni bir veri deposu belirtebilirsiniz.

    ![Hedef veri deposu sayfası](media/data-factory-load-data/destination-create-connection.png)

1. **Yeni bağlı hizmet** sayfasında **Azure Veri Gezgini**' yi seçin ve ardından **devam** ' ı seçin.

    ![Azure Veri Gezgini seçin-Yeni bağlı hizmet](media/data-factory-load-data/adx-select-new-linked-service.png)

1. **Yeni bağlı hizmet (Azure Veri Gezgini)** sayfasında, aşağıdaki adımları uygulayın:

    ![ADX yeni bağlı hizmet](media/data-factory-load-data/adx-new-linked-service.png)

   * Azure Veri Gezgini bağlı hizmeti için **ad** ' ı seçin.
   * **Hesap seçim yönteminde**: 
        * **Azure aboneliği** radyo düğmesini seçin ve **Azure abonelik** hesabınızı seçin. Sonra, **kümenizi**seçin. Açılan listede yalnızca kullanıcıya ait kümeler listelenir.
        * Alternatif olarak, **el ile** radyo düğmesi gir ' i seçin ve **uç**noktanızı girin.
    * **Kiracıyı**belirtin.
    * **Hizmet sorumlusu kimliğini**girin.
    * **Hizmet sorumlusu anahtar** düğmesini seçin ve **hizmet sorumlusu anahtarını**girin.
    * Açılır menüden **veritabanınızı** seçin. Alternatif olarak, **Düzenle** onay kutusunu seçin ve veritabanınızın adını girin.
    * Oluşturduğunuz bağlı hizmet bağlantısını test etmek için **Bağlantıyı Sına** ' yı seçin. Kuruluma bağlanıyorsanız, bir yeşil onay işareti **bağlantısı başarılı** olur.
    * Bağlı hizmet oluşturmayı tamamladıktan sonra **son** ' u seçin.

    > [!NOTE]
    > Hizmet sorumlusu, Azure Veri Gezgini hizmetine erişmek için Azure Data Factory tarafından kullanılır. Hizmet sorumlusu için [bir Azure Active Directory (Azure AD) hizmet sorumlusu oluşturun](/azure-stack/operator/azure-stack-create-service-principals#manage-an-azure-ad-service-principal). **Azure Key Vault** yöntemini kullanmayın.

#### <a name="configure-the-azure-data-explorer-data-connection"></a>Azure Veri Gezgini veri bağlantısını yapılandırma

1. **Hedef veri deposu** açılır. Oluşturduğunuz Azure Veri Gezgini veri bağlantısı kullanıma sunulmuştur. Bağlantıyı yapılandırmak için **İleri ' yi** seçin.

    ![ADX hedef veri deposu](media/data-factory-load-data/destination-data-store.png)

1. **Tablo eşlemesinde**, hedef tablo adını ayarlayın ve **İleri ' yi**seçin.

    ![Hedef veri kümesi tablo eşleme](media/data-factory-load-data/destination-dataset-table-mapping.png)

1. **Sütun eşleme** sayfasında:
    * İlk eşleme ADF tarafından [ADF şema eşlemesine](/azure/data-factory/copy-activity-schema-and-type-mapping) göre gerçekleştirilir
        * Azure Data Factory hedef tablosu için **sütun eşlemelerini** ayarlayın. Varsayılan eşleme, kaynaktan ADF hedef tablosundan görüntülenir.
        * Sütun eşlemenizi tanımlamak için ihtiyacınız olmayan sütunların seçimini kaldırın.
    * İkinci eşleme, bu tablo verileri Azure Veri Gezgini 'da gösterildiğinde oluşur. Eşleme, [CSV eşleme kurallarına](/azure/kusto/management/mappings#csv-mapping)göre gerçekleştirilir. Kaynak veriler CSV biçiminde olmasa bile, ADF verileri tablosal biçimine dönüştürdüğünden, bu nedenle CSV eşlemesi bu aşamada yalnızca ilgili eşleme olur.
        * **Azure Veri Gezgini (kusto) havuz özellikleri** ' nin altında, sütun eşlemesinin kullanılabilmesi için ilgili alım **eşleme adını** (isteğe bağlı) ekleyin.
        * Alma **eşleme adı** belirtilmemişse, **sütun eşlemeleri** bölümünde tanımlanan "ada göre" eşleme sırası oluşur. "Ad-adı" eşlemesi başarısız olursa, Azure Veri Gezgini verileri bir "sütun olarak konum" sırasıyla (varsayılan olarak eşler) almaya çalışacaktır.
    * **İleri**’yi seçin

    ![Hedef veri kümesi sütun eşleme](media/data-factory-load-data/destination-dataset-column-mapping.png)

1. **Ayarlar** sayfasında:
    * İlgili **hata toleransı ayarlarını**belirleyin.
    * **Performans ayarları**: Hazırlama geçerli değildir. **Gelişmiş ayarlar** maliyet konularını içerir. Belirli bir ihtiyaç yoksa, olarak bırakın.
    * **İleri**’yi seçin.

    ![Veri ayarlarını Kopyala](media/data-factory-load-data/copy-data-settings.png)

1. **Özet**bölümünde ayarları gözden geçirin ve **İleri**' yi seçin.

    ![Veri kopyalama Özeti](media/data-factory-load-data/copy-data-summary.png)

1. **Dağıtım sayfasında**:
    * **İzleyici sekmesine geçmek** için **izleyici** ' yi seçin ve işlem hattının durumunu (ilerleme, hatalar ve veri akışı) görüntüleyin.
    * Bağlı hizmetleri, veri kümelerini ve işlem hatlarını düzenlemek için **ardışık düzeni Düzenle** ' yi seçin.
    * Veri kopyalamayı tamamlama görevi için **son** ' u seçin

    ![Dağıtım sayfası](media/data-factory-load-data/deployment.png)

## <a name="next-steps"></a>Sonraki adımlar

* Azure Data Factory [Azure Veri Gezgini Bağlayıcısı](/azure/data-factory/connector-azure-data-explorer) hakkında bilgi edinin.

* [Data Factory Kullanıcı arabirimindeki](/azure/data-factory/quickstart-create-data-factory-portal)bağlı hizmetleri, veri kümelerini ve işlem hatlarını düzenlemeyle ilgili daha fazla bilgi edinin.

* Veri sorgulama için [Azure Veri Gezgini sorguları](/azure/data-explorer/web-query-data) hakkında bilgi edinin.
