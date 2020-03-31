---
title: Quickstart - Azure portalLarını kullanarak Azure Databricks'te Bir Kıvılcım işi çalıştırma
description: Bu hızlı başlangıç, Bir Azure Databricks çalışma alanı, bir Apache Spark kümesi oluşturmak ve bir Spark işini çalıştırmak için Azure portalının nasıl kullanılacağını gösterir.
services: azure-databricks
ms.service: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.workload: big-data
ms.topic: quickstart
ms.date: 03/23/2020
ms.custom: mvc
ms.openlocfilehash: d6af521238a034bc22612335119f08284b87eb4b
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80132700"
---
# <a name="quickstart-run-a-spark-job-on-azure-databricks-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portalını kullanarak Databricks üzerinde bir Spark işi çalıştırma

Bu hızlı başlangıçta, Bir Apache Spark kümesiyle bir Azure Databricks çalışma alanı oluşturmak için Azure portalını kullanırsınız. Kümede bir iş çalıştırır ve Boston güvenlik verilerinden gerçek zamanlı raporlar oluşturmak için özel grafikler kullanırsınız.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com)oturum açın.

> [!Note]
> Bu öğretici **Azure Ücretsiz Deneme Aboneliği**kullanılarak gerçekleştirilemez.
> Ücretsiz bir hesabınız varsa, profilinize gidin ve aboneliğinizi istediğiniz **kadar öde**olarak değiştirin. Daha fazla bilgi için bkz. [Ücretsiz Azure hesabı](https://azure.microsoft.com/free/). Ardından, [harcama sınırını kaldırın](https://docs.microsoft.com/azure/billing/billing-spending-limit#why-you-might-want-to-remove-the-spending-limit)ve bölgenizdeki VCPU'lar için kota artışı [isteyin.](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request) Azure Databricks çalışma alanınızı oluşturduğunuzda, çalışma alanına ücretsiz Premium Azure Databricks DBUs'a 14 gün süreyle erişim sağlamak için **Deneme (Premium - 14 Gün Ücretsiz DBUs)** fiyatlandırma katmanını seçebilirsiniz.

## <a name="create-an-azure-databricks-workspace"></a>Azure Databricks çalışma alanı oluşturma

Bu bölümde Azure portalını kullanarak bir Azure Databricks çalışma alanı oluşturursunuz.

1. Azure portalında, **bir kaynak** > Oluştur**Analytics** > **Azure Databricks'i**seçin.

    ![Azure portalında veri tuğlaları](./media/quickstart-create-databricks-workspace-portal/azure-databricks-on-portal.png "Azure portalında veri tuğlaları")

2. **Azure Databricks Hizmeti** bölümünde, Databricks çalışma alanı oluşturmak için değerler sağlayın.

    ![Azure Databricks çalışma alanı oluşturma](./media/quickstart-create-databricks-workspace-portal/create-databricks-workspace.png "Azure Databricks çalışma alanı oluşturma")

    Aşağıdaki değerleri sağlayın:
    
    |Özellik  |Açıklama  |
    |---------|---------|
    |**Çalışma alanı adı**     | Databricks çalışma alanınız için bir ad sağlayın        |
    |**Abonelik**     | Açılan listeden Azure aboneliğinizi seçin.        |
    |**Kaynak grubu**     | Yeni bir kaynak grubu oluşturmayı veya mevcut bir kaynak grubunu kullanmayı seçin. Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Daha fazla bilgi için bkz. [Azure Kaynak Grubuna genel bakış](../azure-resource-manager/management/overview.md). |
    |**Konum**     | **Batı ABD 2**'yi seçin. Kullanılabilir diğer bölgeler için bkz. [Bölgeye göre kullanılabilir Azure hizmetleri](https://azure.microsoft.com/regions/services/).        |
    |**Fiyatlandırma Katmanı**     |  **Standart,** **Premium**veya **Deneme**arasında seçim yapın. Bu katmanlar hakkında daha fazla bilgi için bkz. [Databricks fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/databricks/).       |

3. **Gözden Geçir + Oluştur'u**seçin ve ardından **Oluşturun.** Çalışma alanının oluşturulması birkaç dakika sürer. Çalışma alanı oluşturma sırasında, **Bildirimler'de**dağıtım durumunu görüntüleyebilirsiniz. Bu işlem tamamlandıktan sonra, kullanıcı hesabınız çalışma alanına otomatik olarak yönetici kullanıcı olarak eklenir.

    ![Databricks dağıtım döşemesi](./media/quickstart-create-databricks-workspace-portal/databricks-deployment-tile.png "Databricks dağıtım döşemesi")

    Çalışma alanı dağıtımı başarısız olduğunda, çalışma alanı yine de başarısız durumda oluşturulur. Başarısız çalışma alanını silin ve dağıtım hatalarını çözen yeni bir çalışma alanı oluşturun. Başarısız çalışma alanını sildiğinizde, yönetilen kaynak grubu ve başarıyla dağıtılan kaynaklar da silinir.

## <a name="create-a-spark-cluster-in-databricks"></a>Databricks’te Spark kümesi oluşturma

> [!NOTE]
> Azure Databricks kümesini oluşturmak için ücretsiz hesap oluşturmak istiyorsanız kümeyi oluşturmadan önce profilinize gidin ve aboneliğini **kullandıkça öde** modeline geçirin. Daha fazla bilgi için bkz. [Ücretsiz Azure hesabı](https://azure.microsoft.com/free/).

1. Azure portalında, oluşturduğunuz Databricks çalışma alanına gidin ve sonra **Çalışma Alanını Başlat**’a tıklayın.

2. Azure Databricks portalına yönlendirilirsiniz. Portaldan Yeni **Küme'yi**tıklatın.

    ![Azure'da Veri Tuğlaları](./media/quickstart-create-databricks-workspace-portal/databricks-on-azure.png "Azure'da Veri Tuğlaları")

3. **Yeni küme** sayfasında, bir küme oluşturmak için değerleri girin.

    ![Azure'da Veri Tuğlaları Kıvılcım kümesi Oluşturma](./media/quickstart-create-databricks-workspace-portal/create-databricks-spark-cluster.png "Azure'da Veri Tuğlaları Kıvılcım kümesi Oluşturma")

    Aşağıdakiler dışında diğer tüm varsayılan değerleri kabul edin:

   * Küme için bir ad girin.
   * Bu makale için, **5,3** çalışma süresine sahip bir küme oluşturun.
   * Dakikalar süren inaktivite onay kutusunun **ardından \_ \_ Sonlandırma'yı seçtiğinizden** emin olun. Küme kullanılmazsa kümenin sonlandırılması için biz süre (dakika cinsinden) belirtin.
    
     **Küme oluştur**’u seçin. Küme çalışmaya başladıktan sonra kümeye not defterleri ekleyebilir ve Spark işleri çalıştırabilirsiniz.

Küme oluşturma hakkında daha fazla bilgi için bkz. [Azure Databricks üzerinde Spark kümesi oluşturma](/azure/databricks/clusters/create).

## <a name="run-a-spark-sql-job"></a>Spark SQL işi çalıştırma

Databricks'te bir not defteri oluşturmak, not defterini Azure Açık Veri Kümelerinden gelen verileri okuyacak şekilde yapılandırmak ve ardından veriler üzerinde bir Spark SQL işi çalıştırmak için aşağıdaki görevleri gerçekleştirin.

1. Sol bölmede Azure **Veri Tuğlaları'nı**seçin. Ortak **Görevler'den** **Yeni Not Defteri'ni**seçin.

    ![Databricks'te not defteri oluşturma](./media/quickstart-create-databricks-workspace-portal/databricks-create-notebook.png "Databricks'te not defteri oluşturma")

2. Not **Defteri Oluştur** iletişim kutusunda bir ad girin, dil olarak **Python'u** seçin ve daha önce oluşturduğunuz Kıvılcım kümesini seçin.

    ![Databricks'te not defteri oluşturma](./media/quickstart-create-databricks-workspace-portal/databricks-notebook-details.png "Databricks'te not defteri oluşturma")

    **Oluştur'u**seçin.

3. Bu adımda, [Azure Açık Veri Kümelerinden](https://azure.microsoft.com/services/open-datasets/catalog/boston-safety-data/#AzureDatabricks)Boston Güvenlik Verileri içeren bir Spark DataFrame oluşturun ve verileri sorgulamak için SQL'i kullanın.

   Aşağıdaki komut Azure depolama erişim bilgilerini ayarlar. Bu PySpark kodunu ilk hücreye yapıştırın ve kodu çalıştırmak için **Shift+Enter'ı** kullanın.

   ```python
   blob_account_name = "azureopendatastorage"
   blob_container_name = "citydatacontainer"
   blob_relative_path = "Safety/Release/city=Boston"
   blob_sas_token = r"?st=2019-02-26T02%3A34%3A32Z&se=2119-02-27T02%3A34%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=XlJVWA7fMXCSxCKqJm8psMOh0W4h7cSYO28coRqF2fs%3D"
   ```

   Aşağıdaki komut, Spark'ın Blob depolamadan uzaktan okumasını sağlar. Bu PySpark kodunu bir sonraki hücreye yapıştırın ve kodu çalıştırmak için **Shift+Enter'u** kullanın.

   ```python
   wasbs_path = 'wasbs://%s@%s.blob.core.windows.net/%s' % (blob_container_name, blob_account_name, blob_relative_path)
   spark.conf.set('fs.azure.sas.%s.%s.blob.core.windows.net' % (blob_container_name, blob_account_name), blob_sas_token)
   print('Remote blob path: ' + wasbs_path)
   ```

   Aşağıdaki komut bir DataFrame oluşturur. Bu PySpark kodunu bir sonraki hücreye yapıştırın ve kodu çalıştırmak için **Shift+Enter'u** kullanın.

   ```python
   df = spark.read.parquet(wasbs_path)
   print('Register the DataFrame as a SQL temporary view: source')
   df.createOrReplaceTempView('source')
   ```

4. Bir SQL deyimi çalıştırın **kaynak**olarak adlandırılan geçici görünümden en üst 10 veri satırları döndürün. Bu PySpark kodunu bir sonraki hücreye yapıştırın ve kodu çalıştırmak için **Shift+Enter'u** kullanın.

   ```python
   print('Displaying top 10 rows: ')
   display(spark.sql('SELECT * FROM source LIMIT 10'))
   ```

5. Aşağıdaki ekran görüntüsünde gösterildiği gibi bir tablo çıktısı görürsünüz (yalnızca bazı sütunlar gösterilmiştir):

    ![Örnek veriler](./media/quickstart-create-databricks-workspace-portal/databricks-sample-csv-data.png "Örnek JSON verileri")

6. Artık, diğer kaynaklar yerine Citizens Connect App ve City Worker App kullanarak kaç güvenlik olayı rapor edildigini göstermek için bu verilerin görsel bir temsilini oluşturursunuz. Tablo çıktısının altından Çubuk **grafik** simgesini seçin ve ardından **Çizim Seçenekleri'ni**tıklatın.

    ![Çubuk grafiği oluşturma](./media/quickstart-create-databricks-workspace-portal/create-plots-databricks-notebook.png "Çubuk grafiği oluşturma")

8. **Çizimi Özelleştir** menüsünde, değerleri ekran görüntüsünde gösterilen şekilde sürükleyip bırakın.

    ![Pasta grafiğini özelleştirme](./media/quickstart-create-databricks-workspace-portal/databricks-notebook-customize-plot.png "Çubuk grafiği özelleştir")

   * **Anahtarları** **kaynağa**ayarlayın.
   * **Değerleri** **<\id>** ayarlayın.
   * **Toplama**’yı **SAYI** olarak ayarlayın.
   * **Ekran türünü** **Pasta grafiğine**ayarlayın.

     **Uygula**’ya tıklayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Makaleyi tamamladıktan sonra kümeyi sonlandırabilirsiniz. Bunu yapmak için Azure Databricks çalışma alanında sol bölmedeki **Kümeler**’i seçin. Sonlandırmak istediğiniz küme için imleci **Eylemler** sütunu altındaki üç noktanın üzerine taşıyın ve **Sonlandır** simgesini seçin.

![Databricks kümesini durdurma](./media/quickstart-create-databricks-workspace-portal/terminate-databricks-cluster.png "Databricks kümesini durdurma")

Kümeyi oluştururken **etkinlik dakikalarından \_ \_ sonra Sonlandırma** onay kutusunu seçmeniz koşuluyla, kümeyi el ile sonlandırmazsanız otomatik olarak durur. Böyle bir durumda, belirtilen süre boyunca etkin olmaması durumunda küme otomatik olarak durdurulur.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Veri Tuğlaları'nda bir Kıvılcım kümesi oluşturdunuz ve Azure Açık Veri Kümelerinden gelen verileri kullanarak bir Spark işi çalıştırdın. Diğer veri kaynaklarından Azure Databricks’e verileri aktarma hakkında bilgi almak için [Spark veri kaynakları](/azure/databricks/data/data-sources/index) bölümüne de bakabilirsiniz. Azure Databricks kullanılarak bir ETL işleminin (verileri ayıklama, dönüştürme ve yükleme) nasıl gerçekleştirileceğini öğrenmek için sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
>[Azure Databricks kullanarak verileri ayıklama, dönüştürme ve yükleme](databricks-extract-load-sql-data-warehouse.md)
