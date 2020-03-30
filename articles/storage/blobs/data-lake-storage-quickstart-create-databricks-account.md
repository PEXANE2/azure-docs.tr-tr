---
title: "Hızlı başlatma: Azure Veri Tuğlaları kullanarak Azure Veri Gölü Depolama Gen2'deki verileri analiz edin | Microsoft Dokümanlar"
description: Azure portalını ve Azure Veri Gölü Depolama Gen2 depolama hesabını kullanarak Azure Veri Tuğlaları'nda bir Spark işi çalıştırmayı öğrenin.
author: normesta
ms.author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: quickstart
ms.date: 02/17/2020
ms.reviewer: jeking
ms.openlocfilehash: 346795b79a78589d949b035a803a67a9e5a2e8e5
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77470743"
---
# <a name="quickstart-analyze-data-with-databricks"></a>Quickstart: Databricks ile verileri analiz edin

Bu hızlı başlangıçta, bir depolama hesabında depolanan veriler üzerinde analitik gerçekleştirmek için Azure Databricks'i kullanarak bir Apache Spark işi çalıştırın. Spark işinin bir parçası olarak, demografiye dayalı ücretsiz/ücretli kullanım hakkında bilgi edinmek için bir radyo kanalı abonelik verilerini analiz eceksiniz.

## <a name="prerequisites"></a>Ön koşullar

* Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)

* Azure Veri Gölü Gen2 depolama hesabınızın adı. [Bir Azure Veri Gölü Depolama Gen2 depolama hesabı oluşturun.](data-lake-storage-quickstart-create-account.md)

* **Depolama Blob Veri Oluşturkanı**olarak atanmış bir role sahip bir Azure hizmet müdürünün kiracı kimliği, uygulama kimliği ve parolası. [Bir hizmet ilkesi oluşturun.](../../active-directory/develop/howto-create-service-principal-portal.md)

  > [!IMPORTANT]
  > Rolü Veri Gölü Depolama Gen2 depolama hesabı kapsamındaki atama. Bir rolü üst kaynak grubuna veya aboneye atayabilirsiniz, ancak bu rol atamaları depolama hesabına yayılana kadar izinlerle ilgili hatalar alırsınız.

## <a name="create-an-azure-databricks-workspace"></a>Azure Databricks çalışma alanı oluşturma

Bu bölümde Azure portalını kullanarak bir Azure Databricks çalışma alanı oluşturursunuz.

1. Azure portalında, **bir kaynak** > Oluştur**Analytics** > **Azure Databricks'i**seçin.

    ![Azure portalında veri tuğlaları](./media/data-lake-storage-quickstart-create-databricks-account/azure-databricks-on-portal.png "Azure portalında veri tuğlaları")

2. **Azure Databricks Hizmeti** bölümünde, Databricks çalışma alanı oluşturmak için değerler sağlayın.

    ![Azure Databricks çalışma alanı oluşturma](./media/data-lake-storage-quickstart-create-databricks-account/create-databricks-workspace.png "Azure Databricks çalışma alanı oluşturma")

    Aşağıdaki değerleri sağlayın:

    |Özellik  |Açıklama  |
    |---------|---------|
    |**Çalışma alanı adı**     | Databricks çalışma alanınız için bir ad sağlayın        |
    |**Abonelik**     | Açılan listeden Azure aboneliğinizi seçin.        |
    |**Kaynak grubu**     | Yeni bir kaynak grubu oluşturmayı veya mevcut bir kaynak grubunu kullanmayı seçin. Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Daha fazla bilgi için bkz. [Azure Kaynak Grubuna genel bakış](../../azure-resource-manager/management/overview.md). |
    |**Konum**     | **Batı ABD 2**'yi seçin. Tercih ettiğiniz başka bir genel bölgeyi seçebilirsiniz.        |
    |**Fiyatlandırma Katmanı**     |  **Standart** veya **Premium** arasında seçim yapın. Bu katmanlar hakkında daha fazla bilgi için bkz. [Databricks fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/databricks/).       |

3. Hesabın oluşturulması birkaç dakika sürer. İşlem durumunu izlemek için en üstteki ilerleme çubuğunu görüntüleyin.

4. **Panoya sabitle**’yi ve sonra **Oluştur**’u seçin.

## <a name="create-a-spark-cluster-in-databricks"></a>Databricks’te Spark kümesi oluşturma

1. Azure portalında, oluşturduğunuz Databricks çalışma alanına gidin ve sonra **Çalışma Alanını Başlat**’ı seçin.

2. Azure Databricks portalına yönlendirilirsiniz. Portaldan **Yeni** > **Küme'yi**seçin.

    ![Azure'da Veri Tuğlaları](./media/data-lake-storage-quickstart-create-databricks-account/databricks-on-azure.png "Azure'da Veri Tuğlaları")

3. **Yeni küme** sayfasında, bir küme oluşturmak için değerleri girin.

    ![Azure'da Veri Tuğlaları Kıvılcım kümesi Oluşturma](./media/data-lake-storage-quickstart-create-databricks-account/create-databricks-spark-cluster.png "Azure'da Veri Tuğlaları Kıvılcım kümesi Oluşturma")

    Aşağıdaki alanlara değerleri girin ve diğer alanlar için varsayılan değerleri kabul edin:

    - Küme için bir ad girin.
     
    - **120 dakika işlem yapılmadığında sonlandır** onay kutusunu seçtiğinizden emin olun. Küme kullanılmazsa kümenin sonlandırılması için biz süre (dakika cinsinden) belirtin.

4. **Küme oluştur**’u seçin. Küme çalışmaya başladıktan sonra kümeye not defterleri ekleyebilir ve Spark işleri çalıştırabilirsiniz.

Küme oluşturma hakkında daha fazla bilgi için bkz. [Azure Databricks üzerinde Spark kümesi oluşturma](https://docs.azuredatabricks.net/user-guide/clusters/create.html).

## <a name="create-storage-account-container"></a>Depolama hesabı kapsayıcısı oluşturma

Bu bölümde, Azure Databricks çalışma alanında bir not defteri oluşturacak ve ardından depolama hesabını yapılandırmak için kod parçacıklarını çalıştıracaksınız.

1. [Azure portalında](https://portal.azure.com), oluşturduğunuz Azure Databricks çalışma alanına gidin ve sonra **Çalışma Alanını Başlat**’ı seçin.

2. Sol bölmede **Çalışma Alanı**’nı seçin. **Çalışma Alanı** açılır listesinden **Oluştur** > **Not Defteri**’ni seçin.

    ![Databricks'te not defteri oluşturma](./media/data-lake-storage-quickstart-create-databricks-account/databricks-create-notebook.png "Databricks'te not defteri oluşturma")

3. **Not Defteri Oluştur** iletişim kutusunda, not defterinizin adını girin. Dil olarak **Scala**’yı seçin ve daha önce oluşturduğunuz Spark kümesini seçin.

    ![Databricks'te not defteri oluşturma](./media/data-lake-storage-quickstart-create-databricks-account/databricks-notebook-details.png "Databricks'te not defteri oluşturma")

    **Oluştur'u**seçin.

4. Aşağıdaki kod bloğunu ilk hücreye kopyalayıp yapıştırın, ancak bu kodu henüz çalıştırmayın.

   ```scala
   spark.conf.set("fs.azure.account.auth.type.<storage-account-name>.dfs.core.windows.net", "OAuth")
   spark.conf.set("fs.azure.account.oauth.provider.type.<storage-account-name>.dfs.core.windows.net", "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider")
   spark.conf.set("fs.azure.account.oauth2.client.id.<storage-account-name>.dfs.core.windows.net", "<appID>")
   spark.conf.set("fs.azure.account.oauth2.client.secret.<storage-account-name>.dfs.core.windows.net", "<password>")
   spark.conf.set("fs.azure.account.oauth2.client.endpoint.<storage-account-name>.dfs.core.windows.net", "https://login.microsoftonline.com/<tenant-id>/oauth2/token")
   spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "true")
   dbutils.fs.ls("abfss://<container-name>@<storage-account-name>.dfs.core.windows.net/")
   spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "false")

   ```
5. Bu kod bloğunda, `storage-account-name` `appID`bu `password`kod `tenant-id` bloğundaki ,, ve yer tutucu değerlerini, hizmet ilkesini oluşturduğunuzda topladığınız değerlerle değiştirin. `container-name` Yer tutucu değerini, kapsayıcıya vermek istediğiniz ada ayarlayın.

6. Bu bloktaki kodu çalıştırmak için **SHIFT + ENTER** tuşlarına basın.

## <a name="ingest-sample-data"></a>Örnek verileri ekleme

Bu bölüme başlamadan önce aşağıdaki önkoşulları tamamlamanız gerekir:

Aşağıdaki kodu bir not defteri hücresine girin:

    %sh wget -P /tmp https://raw.githubusercontent.com/Azure/usql/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json

Hücrede, kodu çalıştırmak için **SHIFT + ENTER** tuşuna basın.

Şimdi bunun altındaki yeni bir hücrede, aşağıdaki kodu girin ve parantez içinde görünen değerleri daha önce kullandığınız değerlerle değiştirin:

    dbutils.fs.cp("file:///tmp/small_radio_json.json", "abfss://<container-name>@<storage-account-name>.dfs.core.windows.net/")

Hücrede, kodu çalıştırmak için **SHIFT + ENTER** tuşuna basın.

## <a name="run-a-spark-sql-job"></a>Spark SQL işi çalıştırma

Verilerde bir Spark SQL işi çalıştırmak için aşağıdaki görevleri gerçekleştirin.

1. **small_radio_json.json** adlı örnek JSON veri dosyasındaki verileri kullanarak geçici tablo oluşturmak için bir SQL deyimi çalıştırın. Aşağıdaki kod parçacığında yer tutucu değerlerini kapsayıcınızın adı ve depolama hesabı adı ile değiştirin. Daha önceden oluşturduğunuz not defterini kullanarak kod parçacığını not defterindeki yeni bir kod hücresine yapıştırın ve sonra SHIFT + ENTER tuşlarına basın.

    ```sql
    %sql
    DROP TABLE IF EXISTS radio_sample_data;
    CREATE TABLE radio_sample_data
    USING json
    OPTIONS (
     path  "abfss://<container-name>@<storage-account-name>.dfs.core.windows.net/small_radio_json.json"
    )
    ```

    Komut başarıyla tamamlandıktan sonra, JSON dosyasındaki tüm verileri Databricks kümesinde tablo olarak görüntüleyebilirsiniz.

    `%sql` dili sihirli komutu, not defteri başka bir türde olsa bile not defterinden bir SQL kodu çalıştırmanızı sağlar. Daha fazla bilgi için bkz. [Bir not defterinde dilleri karıştırma](https://docs.azuredatabricks.net/user-guide/notebooks/index.html#mixing-languages-in-a-notebook).

2. Çalıştırdığınız sorguyu daha iyi anlamak için örnek JSON verilerinin bir anlık görüntüsüne bakalım. Kod hücresine aşağıdaki kod parçacığını yapıştırın ve **SHIFT + ENTER** tuşuna basın.

    ```sql
    %sql
    SELECT * from radio_sample_data
    ```

3. Aşağıdaki ekran görüntüsünde gösterildiği gibi bir tablo çıktısı görürsünüz (yalnızca bazı sütunlar gösterilmiştir):

    ![Örnek JSON verileri](./media/data-lake-storage-quickstart-create-databricks-account/databricks-sample-csv-data.png "Örnek JSON verileri")

    Diğer ayrıntıların yanı sıra, örnek veriler bir radyo kanalının hedef kitlesinin cinsiyetini (sütun adı, **cinsiyet)** ve aboneliklerinin ücretsiz veya ücretli olup olmadığını (sütun adı, **düzey)** yakalar.

4. Bu durumda, bu verilerin her bir cinsiyet, ücretsiz hesaba sahip kullanıcı sayısı ve ücretli hesabı olan abone sayısı için gösterilecek görsel bir açıklamasını oluşturursunuz. Tablo çıktısının alt kısmında bulunan **Çubuk grafik** simgesine ve sonra **Çizim Seçenekleri**’ne tıklayın.

    ![Çubuk grafiği oluşturma](./media/data-lake-storage-quickstart-create-databricks-account/create-plots-databricks-notebook.png "Çubuk grafiği oluşturma")

5. **Çizimi Özelleştir** menüsünde, değerleri ekran görüntüsünde gösterilen şekilde sürükleyip bırakın.

    ![Çubuk grafiği özelleştir](./media/data-lake-storage-quickstart-create-databricks-account/databricks-notebook-customize-plot.png "Çubuk grafiği özelleştir")

    - **Anahtarlar**’ı **cinsiyet** olarak ayarlayın.
    - **Seri gruplandırmalar**’ı **düzey** olarak ayarlayın.
    - **Değerler**’ı **düzey** olarak ayarlayın.
    - **Toplama**’yı **SAYI** olarak ayarlayın.

6. **Uygula**’ya tıklayın.

7. Çıktı aşağıdaki ekran görüntüsünde gösterildiği gibi görsel açıklamayı gösterir:

     ![Çubuk grafiği özelleştir](./media/data-lake-storage-quickstart-create-databricks-account/databricks-sql-query-output-bar-chart.png "Çubuk grafiği özelleştir")

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makaleyle bitirdikten sonra kümeyi sonlandırabilirsiniz. Azure Databricks çalışma alanında **Kümeler**'i seçin ve sonlandırmak istediğiniz kümeyi bulun. Fare imlecini **Eylemler** sütununun altındaki üç noktanın üzerine götürün ve **Sonlandır** simgesini seçin.

![Databricks kümesini durdurma](./media/data-lake-storage-quickstart-create-databricks-account/terminate-databricks-cluster.png "Databricks kümesini durdurma")

Kümeyi oluştururken ** \_ \_ dakika** dakika devam onay kutusunu seçmeniz koşuluyla, kümeyi el ile sonlandırmazsanız otomatik olarak durur. Bu seçeneği belirlerseniz küme belirtilen zaman boyunca devre dışı olması halinde durdurulur.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Databricks’te bir Spark kümesi oluşturdunuz ve Data Lake Storage 2. Nesil etkin bir depolama hesabındaki verileri kullanarak bir Spark işi çalıştırdınız.

Azure Databricks kullanılarak bir ETL işleminin (verileri ayıklama, dönüştürme ve yükleme) nasıl gerçekleştirileceğini öğrenmek için sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
>[Azure Databricks'i kullanarak veri ayıklayın, dönüştürün ve yükleyin.](../../azure-databricks/databricks-extract-load-sql-data-warehouse.md)

- Diğer veri kaynaklarından gelen verileri Azure Veri Tuğlalarına nasıl aktarılabilirsiniz, [bkz.](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html)

- Azure Veri Gölü Depolama Gen2'ye bir Azure Veri Tuğlaları çalışma alanından erişmenin diğer yolları hakkında bilgi edinmek için [bkz.](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake-gen2.html)
