---
title: 'Tutorial: Analyze Azure HDInsight Apache Spark data with Power BI'
description: Tutorial - Use Microsoft Power BI to visualize Apache Spark data stored HDInsight clusters
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,mvc
ms.topic: tutorial
ms.date: 10/03/2019
ms.openlocfilehash: 3fd1405d8421d71f52d9cd215dd055ce1595abd0
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74327267"
---
# <a name="tutorial-analyze-apache-spark-data-using-power-bi-in-hdinsight"></a>Tutorial: Analyze Apache Spark data using Power BI in HDInsight

In this tutorial, you learn how to use [Microsoft Power BI](https://powerbi.microsoft.com/) to visualize data in an Apache Spark cluster in [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Power BI kullanarak Spark verilerini görselleştirme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* Complete the article [Tutorial: Load data and run queries on an Apache Spark cluster in Azure HDInsight](./apache-spark-load-data-run-query.md).

* [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).

* Optional: [Power BI trial subscription](https://app.powerbi.com/signupredirect?pbi_source=web).

## <a name="verify-the-data"></a>Verileri doğrulama

The [Jupyter Notebook](https://jupyter.org/) that you created in the [previous tutorial](apache-spark-load-data-run-query.md) includes code to create an `hvac` table. This table is based on the CSV file available on all HDInsight Spark clusters at `\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv`. Verileri doğrulamak için aşağıdaki yordamı kullanın.

1. Jupyter not defterinden aşağıdaki kodu yapıştırın ve sonra **SHIFT + ENTER** tuşuna basın. Kod, tabloların varlığını doğrular.

    ```PySpark
    %%sql
    SHOW TABLES
    ```

    Çıkış aşağıdakine benzer olacaktır:

    ![Spark’ta tabloları gösterme](./media/apache-spark-use-bi-tools/apache-spark-show-tables.png)

    Bu öğreticiye başlamadan önce not defterini kapattıysanız, `hvactemptable` silinir ve bu nedenle çıktıya eklenmez.  Yalnızca meta veri deposunda depolanmış Hive tablolarına (**isTemporary** sütunu altında **False** ile gösterilir) BI araçlarından erişilebilir. Bu öğreticide, oluşturduğunuz **hvac** tablosuna bağlanacaksınız.

2. Aşağıdaki kodu boş bir hücreye yapıştırın ve sonra **SHIFT + ENTER** tuşlarına basın. Kod, tablodaki verileri doğrular.

    ```PySpark
    %%sql
    SELECT * FROM hvac LIMIT 10
    ```

    Çıkış aşağıdakine benzer olacaktır:

    ![Spark içinde hvac tablosunun satırlarını gösterme](./media/apache-spark-use-bi-tools/apache-spark-select-limit.png)

3. Not defterindeki **Dosya** menüsünden **Kapat ve Durdur**’u seçin. Kaynakları serbest bırakmak için not defterini kapatın.

## <a name="visualize-the-data"></a>Verileri görselleştirme

Bu bölümde Power BI kullanarak, Spark kümesi verilerinden görselleştirme, rapor ve panolar oluşturacaksınız.

### <a name="create-a-report-in-power-bi-desktop"></a>Power BI Desktop'ta rapor oluşturma

Spark ile çalışmanın ilk adımları, Power BI Desktop’ta kümeye bağlanmak, kümeden veri yüklemek ve bu verileri temel alarak basit bir görselleştirme oluşturmaktır.

> [!NOTE]  
> Bu makalede gösterilen bağlayıcı şu anda önizleme aşamasındadır. Geri bildirimlerinizi [Power BI Topluluğu](https://community.powerbi.com/) sitesi ya da [Power BI Ideas](https://ideas.powerbi.com/forums/265200-power-bi-ideas) sayfasından gönderin.

1. Power BI Desktop’ı açın. Close the start-up splash screen if it opens.

2. From the **Home** tab, navigate to **Get Data** > **More..** .

    ![Get data into Power BI Desktop from HDInsight Apache Spark](./media/apache-spark-use-bi-tools/hdinsight-spark-power-bi-desktop-get-data.png "Get data into Power BI from Apache Spark BI")

3. Enter `Spark` in the search box, select **Azure HDInsight Spark**, and then select **Connect**.

    ![Get data into Power BI from Apache Spark BI](./media/apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "Get data into Power BI from Apache Spark BI")

4. Enter your cluster URL (in the form `mysparkcluster.azurehdinsight.net`) in the **Server** text box.

5. Under **Data connectivity mode**, select **DirectQuery**. Sonra **Tamam**’ı seçin.

    Spark ile herhangi bir veri bağlantısı modunu kullanabilirsiniz. DirectQuery kullanırsanız, değişiklikler tüm veri kümesi yenilenmeden raporlara yansıtılır. Verileri içeri aktarırsanız, değişiklikleri görmek için veri kümesini yenilemeniz gerekir. DirectQuery’nin nasıl ve ne zaman kullanılacağı hakkında daha fazla bilgi için bkz. [Power BI’da DirectQuery kullanma](https://powerbi.microsoft.com/documentation/powerbi-desktop-directquery-about/).

6. Enter the HDInsight login account information, then select **Connect**. Varsayılan hesap adı *admin*’dir.

7. Select the `hvac` table, wait to see a preview of the data, and then select **Load**.

    ![Spark cluster user name and password](./media/apache-spark-use-bi-tools/apache-spark-bi-select-table.png "Spark cluster user name and password")

    Power BI Desktop, Spark kümesine bağlanmak ve `hvac` tablosundan verileri yüklemek için gereken bilgilere sahiptir. Tablo ve sütunları, **Alanlar** bölmesinde gösterilir.

8. Her bina için hedef sıcaklık ile gerçek sıcaklık arasındaki farkı görselleştirin:

    1. **GÖRSELLEŞTİRMELER** bölmesinde **Alan Grafiği**’ni seçin.

    2. **BuildingID** alanını **Eksen**’e, **ActualTemp** ve **TargetTemp** alanlarını ise **Değer**’e sürükleyin.

        ![add value columns](./media/apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "add value columns")

        Diyagram şuna benzer:

        ![area graph sum](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph-sum.png "area graph sum")

        Varsayılan olarak görselleştirme, **ActualTemp** ve **TargetTemp** değerlerinin toplamını gösterir. Select the down arrow next to **ActualTemp** and **TragetTemp** in the Visualizations pane, you can see **Sum** is selected.

    3. Select the down arrows next to **ActualTemp** and **TragetTemp** in the Visualizations pane, select **Average** to get an average of actual and target temperatures for each building.

        ![average of values](./media/apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "average of values")

        Veri görselleştirmeniz, ekran görüntüsünde gösterilene benzer olmalıdır. İlgili verilere ilişkin araç ipuçları almak üzere imlecinizi görselleştirmenin üzerine getirin.

        ![area graph](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "area graph")

9. Navigate to **File** > **Save**, enter the name `BuildingTemperature` for the file, then select **Save**.

### <a name="publish-the-report-to-the-power-bi-service-optional"></a>Raporu Power BI Hizmetinde yayımlama (isteğe bağlı)

Power BI hizmeti, raporları ve panoları kuruluşunuzda paylaşmanıza olanak tanır. Bu bölümde, ilk olarak veri kümesini ve raporu yayımlayacaksınız. Ardından, raporu bir panoya sabitleyeceksiniz. Dashboards are typically used to focus on a subset of data in a report. You have only one visualization in your report, but it's still useful to go through the steps.

1. Power BI Desktop’ı açın.

1. **Giriş** sekmesinden **Yayımla**’ya tıklayın.

    ![Power BI Desktop'tan yayımlama](./media/apache-spark-use-bi-tools/apache-spark-bi-publish.png "Power BI Desktop'tan yayımlama")

1. Veri kümenizin ve raporunuzun yayımlanacağı çalışma alanını seçin ve **Seç**’e tıklayın. Aşağıdaki görüntüde varsayılan **Çalışma Alanım** seçilidir.

    ![Select workspace to publish dataset and report to](./media/apache-spark-use-bi-tools/apache-spark-bi-select-workspace.png "Select workspace to publish dataset and report to") 

1. Yayımlama başarılı olduktan sonra **'BuildingTemperature.pbix' dosyasını Power BI’da aç** öğesine tıklayın.

    ![Publish success, click to enter credentials](./media/apache-spark-use-bi-tools/apache-spark-bi-publish-success.png "Publish success, click to enter credentials") 

1. Power BI hizmetinde **Kimlik bilgilerini girin**’e tıklayın.

    ![Enter credentials in Power BI service](./media/apache-spark-use-bi-tools/apache-spark-bi-enter-credentials.png "Enter credentials in Power BI service")

1. **Kimlik bilgilerini düzenle**’ye tıklayın.

    ![Edit credentials in Power BI service](./media/apache-spark-use-bi-tools/apache-spark-bi-edit-credentials.png "Edit credentials in Power BI service")

1. HDInsight oturum açma hesabı bilgilerini girin, ardından **Oturum Aç**’a tıklayın. Varsayılan hesap adı *admin*’dir.

    ![Sign in to Spark cluster](./media/apache-spark-use-bi-tools/apache-spark-bi-sign-in.png "Sign in to Spark cluster")

1. Sol bölmede, **Çalışma Alanları** > **Çalışma Alanım** > **RAPORLAR**’a gidin, ardından **BinaSıcaklığı**’na tıklayın.

    ![Report listed under reports in left pane](./media/apache-spark-use-bi-tools/apache-spark-bi-service-left-pane.png "Report listed under reports in left pane")

    **BinaSıcaklığı** değerinin sol bölmedeki **VERİ KÜMELERİ** altında da listelendiğini göreceksiniz.

    Power BI Desktop’ta oluşturduğunuz görsel artık Power BI hizmetinde kullanılabilir. 

1. İmlecinizi görselleştirmenin üzerine getirin ve ardından sağ üst köşedeki iğne simgesine tıklayın.

    ![Report in the Power BI service](./media/apache-spark-use-bi-tools/apache-spark-bi-service-report.png "Report in the Power BI service")

1. "Yeni pano" öğesini seçin, `Building temperature` adını girin, ardından **Sabitle**’ye tıklayın.

    ![Pin to new dashboard](./media/apache-spark-use-bi-tools/apache-spark-bi-pin-dashboard.png "Pin to new dashboard")

1. Raporda **Panoya git**’e tıklayın.

Görseliniz panoya sabitlenir. Rapora başka görseller ekleyebilir ve bu görselleri aynı panoya sabitleyebilirsiniz. For more information about reports and dashboards, see [Reports in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-reports/) and [Dashboards in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticiyi tamamladıktan sonra kümeyi silmek isteyebilirsiniz. With HDInsight, your data is stored in Azure Storage, so you can safely delete a cluster when it isn't in use. You're also charged for an HDInsight cluster, even when it isn't in use. Since the charges for the cluster are many times more than the charges for storage, it makes economic sense to delete clusters when they aren't in use.

To delete a cluster, see [Delete an HDInsight cluster using your browser, PowerShell, or the Azure CLI](../hdinsight-delete-cluster.md).

## <a name="next-steps"></a>Sonraki adımlar

In this tutorial, you learned how to use [Microsoft Power BI](https://powerbi.microsoft.com/) to visualize data in an Apache Spark cluster in [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/). Advance to the next article to see you can create a machine learning application.

> [!div class="nextstepaction"]
> [Create a machine learning application](./apache-spark-ipython-notebook-machine-learning.md)