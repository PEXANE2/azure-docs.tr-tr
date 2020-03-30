---
title: Azure Veri Fabrikası'ndan Azure Veri Gezgini'ne verileri kopyalama
description: Bu makalede, Azure Veri Fabrikası kopyalama aracını kullanarak Azure Veri Gezgini'ne nasıl veri öttürecek (yüklenir) öğrenirsiniz.
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: jasonh
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/15/2019
ms.openlocfilehash: 860b1a579d9c8cee6c6e80ae4c4e7fdd7949d5c7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71300596"
---
# <a name="copy-data-to-azure-data-explorer-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri Azure Veri Gezgini'ne kopyalama 

Azure Veri Gezgini, hızlı, tam olarak yönetilen, veri analizi hizmetidir. Uygulamalar, web siteleri ve IoT aygıtları gibi birçok kaynaktan akış yapan büyük hacimli veriler üzerinde gerçek zamanlı analiz sunar. Azure Veri Gezgini ile verileri yinelemeli bir şekilde keşfedebilir ve ürünleri geliştirmek, müşteri deneyimlerini geliştirmek, cihazları izlemek ve işlemleri artırmak için desenleri ve anormallikleri belirleyebilirsiniz. Yeni sorular keşfetmenize ve dakikalar içinde yanıt almanıza yardımcı olur. 

Azure Veri Fabrikası, tam olarak yönetilen, bulut tabanlı, veri tümleştirme hizmetidir. Azure Veri Gezgini veritabanınızı mevcut sisteminizdeki verilerle doldurmak için kullanabilirsiniz. Analitik çözümler oluşturuyorken zamandan tasarruf etmenizi sağlayabilir.

Verileri Azure Veri Gezgini'ne yüklediğinizde, Veri Fabrikası aşağıdaki avantajları sağlar:

* **Kolay kurulum**: Komut dosyası gerektirmeden sezgisel, beş adımlı bir sihirbaz alın.
* **Zengin veri deposu desteği**: Şirket içi ve bulut tabanlı veri depolarından oluşan zengin bir set için yerleşik destek alın. Ayrıntılı bir liste için [Desteklenen veri depolarının](/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats)tablosuna bakın.
* **Güvenli ve uyumlu**: Veriler HTTPS veya Azure ExpressRoute üzerinden aktarılır. Genel hizmet varlığı, verilerinizin coğrafi sınırdan asla çıkmamasını sağlar.
* **Yüksek performans**: Azure Veri Gezgini'ne saniyede 1 gigabayt (GBp) kadar veri yükleme hızı. Daha fazla bilgi için [bkz.](/azure/data-factory/copy-activity-performance)

Bu makalede, Amazon Basit Depolama Hizmeti'nden (S3) verileri Azure Veri Gezgini'ne yüklemek için Veri Fabrikası Kopyalama Veri aracını kullanıyorsunuz. Aşağıdakiler gibi diğer veri depolarından veri kopyalamak için benzer bir işlem izleyebilirsiniz:
* [Azure Blob depolama](/azure/data-factory/connector-azure-blob-storage)
* [Azure SQL Veritabanı](/azure/data-factory/connector-azure-sql-database)
* [Azure SQL Veri Ambarı](/azure/data-factory/connector-azure-sql-data-warehouse)
* [Google BigQuery](/azure/data-factory/connector-google-bigquery)
* [Oracle](/azure/data-factory/connector-oracle)
* [Dosya sistemi](/azure/data-factory/connector-file-system)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* [Azure Veri Gezgini kümesi ve veritabanı.](create-cluster-database-portal.md)
* Bir veri kaynağı.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. [Azure portalında](https://ms.portal.azure.com)oturum açın.

1. Sol bölmede, bir kaynak > **Analitik** > **Veri Fabrikası** **Oluştur'u**seçin.

   ![Azure portalında veri fabrikası oluşturma](media/data-factory-load-data/create-adf.png)

1. Yeni **veri fabrikası** bölmesinde, aşağıdaki tablodaki alanlar için değerler sağlayın:

   !["Yeni veri fabrikası" bölmesi](media/data-factory-load-data/my-new-data-factory.png)  

   | Ayar  | Girilen değer  |
   |---|---|
   | **Adı** | Kutuya, veri fabrikanız için genel olarak benzersiz bir ad girin. Bir hata alırsanız, *Veri \"fabrikası adı\" LoadADXDemo kullanılamıyor,* veri fabrikası için farklı bir ad girin. Veri Fabrikası yapılarını adlandırma kuralları [için](/azure/data-factory/naming-rules)bkz.|
   | **Abonelik** | Açılan listede, veri fabrikasını oluşturmak için Azure aboneliğini seçin. |
   | **Kaynak Grubu** | **Yeni Oluştur'u**seçin ve ardından yeni bir kaynak grubunun adını girin. Zaten bir kaynak grubunuz varsa, **varolan kullan'ı**seçin. |
   | **Sürüm** | Açılan listede **V2'yi**seçin. |  
   | **Konum** | Açılan listede, veri fabrikasının konumunu seçin. Listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan veri depoları başka konumlarda veya bölgelerde bulunabilir. |

1. **Oluştur'u**seçin.

1. Oluşturma işlemini izlemek için araç çubuğunda **Bildirimler'i** seçin. Veri fabrikasını oluşturduktan sonra seçin.
   
   **Veri Fabrikası** bölmesi açılır.

   ![Veri Fabrikası bölmesi](media/data-factory-load-data/data-factory-home-page.png)

1. Uygulamayı ayrı bir bölmede açmak için **Yazar & Monitör** döşemesini seçin.

## <a name="load-data-into-azure-data-explorer"></a>Azure Veri Gezgini'ne veri yükleme

Azure Veri Gezgini'ne birçok [türde veri](/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats) yükleyebilirsiniz. Bu makalede, Amazon S3 veri yüklemek için nasıl anlatılmaktadır.

Verilerinizi aşağıdaki yollardan herhangi birinde yükleyebilirsiniz:

* Azure Veri Fabrikası kullanıcı arabiriminde, sol bölmede, Azure Veri Fabrikası Kullanıcı Arabirimi'ni kullanarak veri fabrikası oluştur'un "Veri fabrikası oluşturma" bölümünde gösterildiği gibi **Yazar** [simgesini](/azure/data-factory/quickstart-create-data-factory-portal#create-a-data-factory)seçin.
* Azure Veri Fabrikası Kopyalama Verileri aracında, [verileri kopyalamak için Verileri Kopyala aracını kullanın'](/azure/data-factory/quickstart-create-data-factory-copy-data-tool)da gösterildiği gibi.

### <a name="copy-data-from-amazon-s3-source"></a>Amazon S3'ten veri kopyalama (kaynak)

1. **Başlatılalım** bölmesinde, **Verileri Kopyala**seçeneğini seçerek Verileri Kopyala aracını açın.

   ![Verileri Kopyala aracı düğmesi](media/data-factory-load-data/copy-data-tool-tile.png)

1. **Özellikler** bölmesinde, Görev **adı** kutusunda bir ad girin ve sonra **İleri'yi**seçin.

    ![Veri Özelliklerini Kopyala bölmesi](media/data-factory-load-data/copy-from-source.png)

1. Kaynak **veri deposu** bölmesinde **yeni bağlantı oluştur'u**seçin.

    ![Veri Kopyalama "Kaynak veri deposu" bölmesi](media/data-factory-load-data/source-create-connection.png)

1. **Amazon S3'ünü**seçin ve ardından **Devam et'i**seçin.

    ![Yeni Bağlantılı Hizmet bölmesi](media/data-factory-load-data/amazons3-select-new-linked-service.png)

1. Yeni **Bağlantılı Hizmet (Amazon S3)** bölmesinde aşağıdakileri yapın:

    ![Amazon S3 bağlantılı hizmeti belirtin](media/data-factory-load-data/amazons3-new-linked-service-properties.png)

    a. **Ad** kutusuna, yeni bağlantılı hizmetinizin adını girin.

    b. **Tümleştirme çalışma zamanı** açılır listesi yle Bağlan'da değeri seçin.

    c. Access **Tuşu Kimliği** kutusuna değeri girin.
    
    > [!NOTE]
    > Amazon S3'te, erişim anahtarınızı bulmak için gezinti çubuğunda Amazon kullanıcı adınızı seçin ve ardından **Güvenlik Kimlik Bilgilerim'i**seçin.
    
    d. Gizli **Erişim Anahtarı** kutusuna bir değer girin.

    e. Oluşturduğunuz bağlantılı hizmet bağlantısını sınamak için **Test Bağlantısı'nı**seçin.

    f. **Bitiş'i**seçin.
    
      **Kaynak veri deposu** bölmesi yeni AmazonS31 bağlantınızı görüntüler. 

1. **Sonraki'ni**seçin.

   ![Kaynak veri deposu oluşturulan bağlantı](media/data-factory-load-data/source-data-store-created-connection.png)

1. Giriş **dosyasını veya klasör bölmesini seç'te** aşağıdakileri yapın:

    a. Kopyalamak istediğiniz dosyaya veya klasöre göz atın ve ardından seçin.

    b. İstediğiniz kopyalama davranışını seçin. **İkili kopya** onay kutusunun temizolduğundan emin olun.

    c. **Sonraki'ni**seçin.

    ![Girdi dosyasını veya klasörünü seçin](media/data-factory-load-data/source-choose-input-file.png)

1. Dosya **biçimi ayarları** bölmesinde, dosyanızın ilgili ayarlarını seçin. ve sonra **İleri'yi**seçin.

   !["Dosya biçimi ayarları" bölmesi](media/data-factory-load-data/source-file-format-settings.png)

### <a name="copy-data-into-azure-data-explorer-destination"></a>Verileri Azure Veri Gezgini'ne kopyalama (hedef)

Verileri bu bölümde belirtilen Azure Veri Gezgini hedef tablosuna (lavabo) kopyalamak için yeni Azure Veri Gezgini bağlantılı hizmet oluşturulur.

#### <a name="create-the-azure-data-explorer-linked-service"></a>Azure Veri Gezgini bağlantılı hizmeti oluşturun

Azure Veri Gezgini bağlantılı hizmeti oluşturmak için aşağıdakileri yapın;

1. Varolan bir veri deposu bağlantısını kullanmak veya **Hedef veri deposu** bölmesinde yeni bir veri deposu belirtmek için yeni bağlantı **oluştur'u**seçin.

    ![Hedef veri deposu bölmesi](media/data-factory-load-data/destination-create-connection.png)

1. Yeni **Bağlantılı Hizmet** bölmesinde **Azure Veri Gezgini'ni**seçin ve ardından **Devam et'i**seçin.

    ![Yeni bağlantılı hizmet bölmesi](media/data-factory-load-data/adx-select-new-linked-service.png)

1. Yeni **Bağlantılı Hizmet (Azure Veri Gezgini)** bölmesinde aşağıdakileri yapın:

    ![Azure Veri Gezgini Yeni Bağlantılı Hizmet bölmesi](media/data-factory-load-data/adx-new-linked-service.png)

   a. **Ad** kutusuna Azure Veri Gezgini bağlantılı hizmetiçin bir ad girin.

   b. **Hesap seçimi yöntemi**altında, aşağıdakilerden birini yapın: 

    * **Azure aboneliğinden** seçin ve ardından açılan listelerde **Azure aboneliğinizi** ve **Cluster'ınızı**seçin. 

        > [!NOTE]
        > **Küme** açılır denetimi yalnızca aboneliğinizle ilişkili kümeleri listeler.

    * **El ile Gir'i**seçin ve **ardından Bitiş Noktanızı**girin.

   c. **Kiracı** kutusuna kiracı adını girin.

   d. Hizmet **ana kimlik** kutusuna, hizmet ana kimliğini girin.

   e. **Servis ana anahtarını** seçin ve ardından **Hizmet ana anahtarı** kutusuna anahtarın değerini girin.

   f. **Veritabanı** açılır listesinde veritabanı adınızı seçin. Alternatif olarak, **Denetim** kutusunu seçin ve ardından veritabanı adını girin.

   g. Oluşturduğunuz bağlantılı hizmet bağlantısını sınamak için **Test Bağlantısı'nı**seçin. Bağlı hizmetinize bağlanabilirseniz, bölme de yeşil bir onay işareti ve **Bağlantı başarılı** bir ileti görüntüler.

   h. Bağlantılı hizmet oluşturmayı tamamlamak için **Bitiş'i** seçin.

    > [!NOTE]
    > Hizmet sorumlusu, Azure Veri Gezgini hizmetine erişmek için Azure Veri Fabrikası tarafından kullanılır. Bir hizmet ilkesi oluşturmak için [bir Azure Etkin Dizin (Azure AD) hizmet ilkesi oluşturmaya](/azure-stack/operator/azure-stack-create-service-principals#manage-an-azure-ad-service-principal)gidin. Azure Anahtar Kasası yöntemini kullanmayın.

#### <a name="configure-the-azure-data-explorer-data-connection"></a>Azure Veri Gezgini veri bağlantısını yapılandırma

Bağlantılı hizmet bağlantısını oluşturduktan sonra **Hedef veri deposu** bölmesi açılır ve oluşturduğunuz bağlantı kullanılabilir. Bağlantıyı yapılandırmak için aşağıdakileri yapın;

1. **Sonraki'ni**seçin.

    ![Azure Veri Gezgini "Hedef veri deposu" bölmesi](media/data-factory-load-data/destination-data-store.png)

1. Tablo **eşleme** bölmesinde, hedef tablo adını ayarlayın ve sonra **İleri'yi**seçin.

    ![Hedef veri kümesi "Tablo eşleme" bölmesi](media/data-factory-load-data/destination-dataset-table-mapping.png)

1. Sütun **eşleme** bölmesinde aşağıdaki eşlemeler gerçekleşir:

    a. İlk eşleme, Azure Veri Fabrikası [şema eşlenesine](/azure/data-factory/copy-activity-schema-and-type-mapping)göre Azure Veri Fabrikası tarafından gerçekleştirilir. Şunları yapın:

    * Azure Veri Fabrikası hedef tablosu için **Sütun eşlemelerini** ayarlayın. Varsayılan eşleme kaynağı kaynaktan Azure Veri Fabrikası hedef tablosuna görüntülenir.

    * Sütun eşlemenizi tanımlamanız gerekmeyen sütunların seçimini iptal edin.

    b. İkinci eşleme, bu tabular verileri Azure Veri Gezgini'ne yutulduğunda oluşur. Haritalama [CSV haritalama kurallarına](/azure/kusto/management/mappings#csv-mapping)göre gerçekleştirilir. Kaynak veriler CSV biçiminde olmasa bile, Azure Veri Fabrikası verileri bir tabular biçimine dönüştürür. Bu nedenle, CSV haritalama bu aşamada sadece ilgili haritalama olduğunu. Şunları yapın:

    * (İsteğe bağlı) **Azure Veri Gezgini (Kusto) lavabo özellikleri**altında, sütun eşlemenin kullanılabilmesi için ilgili Yutma **eşleme adını** ekleyin.

    * **Yutma eşleme adı** belirtilmemişse, Sütun **eşlemeleri** bölümünde tanımlanan *ad* eşleme sırası kullanılır. *Ad* eşleme başarısız olursa, Azure Veri Gezgini verileri *sütunlar arası konum* sırasına göre yutmaya çalışır (diğer bir deyişle, varsayılan konum olarak konuma göre eşlenir).

    * **Sonraki'ni**seçin.

    ![Hedef veri kümesi "Sütun eşleme" bölmesi](media/data-factory-load-data/destination-dataset-column-mapping.png)

1. **Ayarlar** bölmesinde aşağıdakileri yapın:

    a. **Hata toleransı ayarları**altında, ilgili ayarları girin.

    b. **Performans ayarları**altında, **etkinleştirme** geçerli değildir ve **Gelişmiş ayarlar** maliyet hususları içerir. Belirli bir gereksiniminiz yoksa, bu ayarları olduğu gibi bırakın.

    c. **Sonraki'ni**seçin.

    ![Kopya veri "Ayarlar" bölmesi](media/data-factory-load-data/copy-data-settings.png)

1. **Özet** bölmesinde, ayarları gözden geçirin ve sonra **İleri'yi**seçin.

    ![Kopya veri "Özet" bölmesi](media/data-factory-load-data/copy-data-summary.png)

1. Dağıtım **tam** bölmesi'nde aşağıdakileri yapın:

    a. **Monitör** sekmesine geçmek ve ardışık işakının durumunu görüntülemek için (diğer bir şekilde ilerleme, hatalar ve veri akışı), **Monitör'ü**seçin.

    b. Bağlantılı hizmetleri, veri kümelerini ve ardışık hatları da diseetmek için **Ardışık Veriyi Edit'i**seçin.

    c. Kopya veri görevini tamamlamak için **Finish'i** seçin.

    !["Dağıtım tamamlandı" bölmesi](media/data-factory-load-data/deployment.png)

## <a name="next-steps"></a>Sonraki adımlar

* Azure Veri Fabrikası'ndaki [Azure Veri Gezgini bağlayıcısı](/azure/data-factory/connector-azure-data-explorer) hakkında bilgi edinin.
* [Veri Fabrikası Web Hizmeti'nde](/azure/data-factory/quickstart-create-data-factory-portal)bağlantılı hizmetleri, veri kümelerini ve ardışık hatları düzenleme hakkında daha fazla bilgi edinin.
* Veri sorgulama için [Azure Veri Gezgini sorguları](/azure/data-explorer/web-query-data) hakkında bilgi edinin.
