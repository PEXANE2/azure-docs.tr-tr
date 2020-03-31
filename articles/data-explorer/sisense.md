---
title: Sisense'i kullanarak Azure Veri Gezgini'nden gelen verileri görselleştirin
description: Bu makalede, Azure Veri Gezgini'ni Sisense için bir veri kaynağı olarak nasıl ayarlayabilirsiniz ve verileri görselleştirin.
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 5/29/2019
ms.openlocfilehash: f0840b90e1036c23fa58d94515bfeb035299c07f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66358191"
---
# <a name="visualize-data-from-azure-data-explorer-in-sisense"></a>Sisense'de Azure Veri Gezgini'nden gelen verileri görselleştirin

Sisense, son derece etkileşimli kullanıcı deneyimleri sunan analitik uygulamalar oluşturmanıza olanak tanıyan bir analitik iş zekası platformudur. İş zekası ve pano raporlama yazılımı, verilere birkaç tıklamayla erişmenizi ve birleştirmenizi sağlar. Yapılandırılmış ve yapılandırılmamış veri kaynaklarına bağlanabilir, en az komut dosyası ve kodlama ile birden çok kaynaktan tablolara katılabilir ve etkileşimli web panoları ve raporlar oluşturabilirsiniz. Bu makalede, Azure Veri Gezgini'ni Sisense için veri kaynağı olarak nasıl ayarlayabileceğinizi ve örnek kümedeki verileri nasıl görselleştireceğinizi öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için aşağıdakilere ihtiyacınız var:

* [Sisense uygulamasını indirin ve kurun](https://documentation.sisense.com/latest/getting-started/download-install.htm) 

* StormEvents örnek verilerini içeren bir küme ve veritabanı oluşturun. Daha fazla bilgi için [Bkz. Hızlı Başlangıç: Azure Veri Gezgini kümesi ve veritabanı oluşturun](create-cluster-database-portal.md) ve örnek verileri Azure Veri [Gezgini'ne alın.](ingest-sample-data.md)

    [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

## <a name="connect-to-sisense-dashboards-using-azure-data-explorer-jdbc-connector"></a>Azure Data Explorer JDBC konektörünü kullanarak Sisense panolarına bağlanma

1. Aşağıdaki kavanoz dosyalarının en son sürümlerini indirin ve *kopyalayın. \Sisense\DataConnectors\jdbcdrivers\adx* 

    * aktivasyon-1.1.jar
    * adal4j-1.6.0.jar
    * commons-codec-1.10.jar
    * commons-koleksiyonları4-4.1.jar
    * commons-lang3-3.5.jar
    * gson-2.8.0.jar
    * jcip-ek açıklamalar-1.0-1.jar
    * json-smart-1.3.1.jar
    * lang-tag-1.4.4.jar
    * posta-1.4.7.jar
    * mssql-jdbc-7.2.1.jre8.jar
    * nimbus-jose-jwt-7.0.1.jar
    * oauth2-oidc-sdk-5.24.1.jar
    * slf4j-api-1.7.21.jar
    
1. **Sisense** uygulamasını açın.
1. Yeni bir ElastiCube modeli oluşturmak için **Veri** sekmesini seçin ve **+ElastiCube'u** seçin.
    
    ![ElastiCube'u seçin](media/sisense/data-select-elasticube.png)

1. **Yeni ElastiCube Model ekle**, ElastiCube modeli adı ve **Kaydet**.
   
    ![Yeni ElastiCube modeli ekle](media/sisense/add-new-elasticube-model.png)

1. + **Veri**seçin.

    ![Veri düğmesini seçin](media/sisense/select-data.png)

1. **Bağlayıcıyı Seç** sekmesinde **Genel JDBC** bağlayıcısı'nı seçin.

    ![JDBC konektörünü seçin](media/sisense/select-connector.png)

1. **Bağlan** sekmesinde, **Genel JDBC** bağlayıcısı için aşağıdaki alanları tamamlayın ve **İleri'yi**seçin.

    ![JDBC konektör ayarları](media/sisense/jdbc-connector.png)

    |Alan |Açıklama |
    |---------|---------|
    |Bağlantı Dizesi     |   `jdbc:sqlserver://<cluster_name.region>.kusto.windows.net:1433;database=<database_name>;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.kusto.windows.net;loginTimeout=30;authentication=ActiveDirectoryPassword`      |
    |JDBC JARs klasörü  |    `..\Sisense\DataConnectors\jdbcdrivers\adx`     |
    |Sürücü Sınıf Adı    |   `com.microsoft.sqlserver.jdbc.SQLServerDriver`      |
    |User Name   |    AAD kullanıcı adı     |
    |Parola     |   AAD kullanıcı parolası      |

1. Verileri **Seç** sekmesinde, izinlere sahip olduğunuz ilgili veritabanını seçmek için **Veritabanını Seç'i** arayın. Bu örnekte, *test1'i*seçin.

    ![veritabanını seçin](media/sisense/select-database.png)

1. *Test* (veritabanı adı) bölmesinde:
    1. Tabloyu önizlemek ve tablo sütun adlarını görmek için tablo adını seçin. Gereksiz sütunları kaldırabilirsiniz.
    1. Bu tabloyu seçmek için ilgili tablonun onay kutusunu seçin. 
    1. **Done** (Bitti) öğesini seçin.

    ![tabloyu seçme](media/sisense/select-table-see-columns.png)    

1. Veri kümenizi oluşturmak için **Oluştur'u** seçin. 

    * **Yapı** penceresinde **Yapı'yı**seçin.

      ![Pencere oluşturma](media/sisense/build-window.png)

    * Yapı işlemi tamamlanana kadar bekleyin ve ardından **Başarılı Olan Yap'ı**seçin.

      ![Yapı başarılı](media/sisense/build-succeeded.png)

## <a name="create-sisense-dashboards"></a>Sisense panoları oluşturma

1. **Analytics** sekmesinde, **+**  > bu veri kümesinde panooluşturmak için **Yeni Pano'yu** seçin.

    ![Yeni pano](media/sisense/new-dashboard.png)

1. Bir pano seçin ve **Oluştur'u**seçin. 

    ![Pano oluşturma](media/sisense/create-dashboard.png)

1. **Yeni Widget**altında, yeni bir widget oluşturmak için **+ Veri seçin.** 

    ![StormEvents panosuna alan ekleme](media/sisense/storm-dashboard-add-field.png)  

1. Grafiğinize ek sütunlar eklemek için **+ Daha Fazla Veri Ekle'yi** seçin. 

    ![Grafiğe daha fazla veri ekleme](media/sisense/add-more-data.png)

1. Başka bir widget oluşturmak için **+ Widget'ı** seçin. Panonuzu yeniden düzenlemek için widget'ları sürükleyin ve bırakın.

    ![Storm panosu](media/sisense/final-dashboard.png)

Artık verilerinizi görsel analitikle keşfedebilir, ek panolar oluşturabilir ve işletmeniz üzerinde etki yaratmak için verilerinizi eyleme geçirilebilir öngörülere dönüştürebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini için sorgu yazma](write-queries.md)

