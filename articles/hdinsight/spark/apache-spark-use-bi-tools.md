---
title: 'Öğretici: Power BI ile Azure HDInsight Apache Spark verilerini analiz edin'
description: Öğretici - Apache Spark verilerini saklı HDInsight kümelerini görselleştirmek için Microsoft Power BI'yi kullanın
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: tutorial
ms.custom: hdinsightactive,mvc
ms.date: 04/21/2020
ms.openlocfilehash: dd0b4d1b0998bd4b13a17fb8345b87924bc27e1d
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81869986"
---
# <a name="tutorial-analyze-apache-spark-data-using-power-bi-in-hdinsight"></a>Öğretici: HDInsight'ta Power BI kullanarak Apache Spark verilerini analiz edin

Bu eğitimde, Azure HDInsight'taki bir Apache Spark kümesindeki verileri görselleştirmek için Microsoft Power BI'yi nasıl kullanacağınızı öğreneceksiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Power BI kullanarak Spark verilerini görselleştirme

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* [Öğretici: Azure HDInsight içindeki bir Apache Spark kümesinde veri yükleme ve sorgu çalıştırma](./apache-spark-load-data-run-query.md) makalesini tamamlayın.

* [Güç BI Masaüstü](https://powerbi.microsoft.com/en-us/desktop/).

* İsteğe bağlı: [Güç BI deneme aboneliği.](https://app.powerbi.com/signupredirect?pbi_source=web)

## <a name="verify-the-data"></a>Verileri doğrulama

[Önceki öğreticide](apache-spark-load-data-run-query.md) oluşturduğunuz `hvac` [Jupyter Notebook,](https://jupyter.org/) tablo oluşturmak için kod içerir. Bu tablo, tüm HDInsight Spark kümelerinde bulunan CSV dosyasına `\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv`dayanmaktadır. Verileri doğrulamak için aşağıdaki yordamı kullanın.

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

1. Power BI Desktop’ı açın. Açılırsa başlangıç sıçrama ekranını kapatın.

2. Ana **Sayfa** sekmesinden Veri Daha Fazla **Al'a** > **gidin...**

    ![HDInsight Apache Spark'tan Power BI Desktop'a veri alın](./media/apache-spark-use-bi-tools/hdinsight-spark-power-bi-desktop-get-data.png "Apache Spark BI'den Power BI'ye veri alın")

3. Arama `Spark` kutusuna girin, **Azure HDInsight Spark'ı**seçin ve ardından **Bağlan'ı**seçin.

    ![Apache Spark BI'den Power BI'ye veri alın](./media/apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "Apache Spark BI'den Power BI'ye veri alın")

4. Küme URL'nizi (formda) `mysparkcluster.azurehdinsight.net` **Sunucu** metin kutusuna girin.

5. **Veri bağlantı modu**altında **DirectQuery'yi**seçin. Sonra **Tamam**’ı seçin.

    Spark ile herhangi bir veri bağlantısı modunu kullanabilirsiniz. DirectQuery kullanırsanız, değişiklikler tüm veri kümesi yenilenmeden raporlara yansıtılır. Verileri içeri aktarırsanız, değişiklikleri görmek için veri kümesini yenilemeniz gerekir. DirectQuery’nin nasıl ve ne zaman kullanılacağı hakkında daha fazla bilgi için bkz. [Power BI’da DirectQuery kullanma](https://powerbi.microsoft.com/documentation/powerbi-desktop-directquery-about/).

6. HDInsight giriş hesap bilgilerini girin, ardından **Bağlan'ı**seçin. Varsayılan hesap adı *admin*’dir.

7. Tabloyu `hvac` seçin, verilerin önizlemesini görmek için bekleyin ve ardından **Yükle'yi**seçin.

    ![Spark küme kullanıcı adı ve şifre](./media/apache-spark-use-bi-tools/apache-spark-bi-select-table.png "Spark küme kullanıcı adı ve şifre")

    Power BI Desktop, Spark kümesine bağlanmak ve `hvac` tablosundan verileri yüklemek için gereken bilgilere sahiptir. Tablo ve sütunları, **Alanlar** bölmesinde gösterilir.

8. Her bina için hedef sıcaklık ile gerçek sıcaklık arasındaki farkı görselleştirin:

    1. **GÖRSELLEŞTİRMELER** bölmesinde **Alan Grafiği**’ni seçin.

    2. **BuildingID** alanını **Eksen**’e, **ActualTemp** ve **TargetTemp** alanlarını ise **Değer**’e sürükleyin.

        ![değer sütunları ekleme](./media/apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "değer sütunları ekleme")

        Diyagram şuna benzer:

        ![alan grafiği toplamı](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph-sum.png "alan grafiği toplamı")

        Varsayılan olarak görselleştirme, **ActualTemp** ve **TargetTemp** değerlerinin toplamını gösterir. Visualizations bölmesinde **ActualTemp** ve **TragetTemp'in** yanındaki aşağı oku seçin, **Sum'un** seçildiğini görebilirsiniz.

    3. Visualizations bölmesinde **ActualTemp** ve **TragetTemp'in** yanındaki aşağı okları seçin, her bina için ortalama gerçek ve hedef sıcaklık elde etmek için **Ortalama'yı** seçin.

        ![değerlerin ortalaması](./media/apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "değerlerin ortalaması")

        Veri görselleştirmeniz, ekran görüntüsünde gösterilene benzer olmalıdır. İlgili verilere ilişkin araç ipuçları almak üzere imlecinizi görselleştirmenin üzerine getirin.

        ![alan grafiği](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "alan grafiği")

9. **Dosya** > **Kaydet'e**gidin, dosyanın adını `BuildingTemperature` girin ve ardından **Kaydet'i**seçin.

### <a name="publish-the-report-to-the-power-bi-service-optional"></a>Raporu Power BI Hizmetinde yayımlama (isteğe bağlı)

Power BI hizmeti, raporları ve panoları kuruluşunuzda paylaşmanıza olanak tanır. Bu bölümde, ilk olarak veri kümesini ve raporu yayımlayacaksınız. Ardından, raporu bir panoya sabitleyeceksiniz. Panolar genellikle bir rapordaveri bir alt kümesi odaklanmak için kullanılır. Raporunda yalnızca bir görselleştirme var, ancak adımları gözden geçirmek yine de yararlıdır.

1. Power BI Desktop’ı açın.

1. **Giriş** sekmesinden **Yayımla'yı**seçin.

    ![Power BI Desktop'tan yayımlama](./media/apache-spark-use-bi-tools/apache-spark-bi-publish.png "Power BI Desktop'tan yayımlama")

1. Veri kümenizi yayımlamak ve rapor vermek için bir çalışma alanı seçin ve ardından **Seç'i**seçin. Aşağıdaki görüntüde varsayılan **Çalışma Alanım** seçilidir.

    ![Veri kümesini yayımlamak için çalışma alanını seçin ve](./media/apache-spark-use-bi-tools/apache-spark-bi-select-workspace.png "Veri kümesini yayımlamak için çalışma alanını seçin ve")

1. Yayımlama başarılı olduktan sonra **Power BI'de 'BuildingTemperature.pbix' açık'ı**seçin.

    ![Başarı yayımlayın, kimlik bilgilerini girmek için tıklayın](./media/apache-spark-use-bi-tools/apache-spark-bi-publish-success.png "Başarı yayımlayın, kimlik bilgilerini girmek için tıklayın")

1. Power BI hizmetinde **kimlik bilgilerini girin'i**seçin.

    ![Power BI hizmetine kimlik bilgilerini girin](./media/apache-spark-use-bi-tools/apache-spark-bi-enter-credentials.png "Power BI hizmetine kimlik bilgilerini girin")

1. **Kimlik bilgilerini edit'i**seçin.

    ![Power BI hizmetinde kimlik bilgilerini edin](./media/apache-spark-use-bi-tools/apache-spark-bi-edit-credentials.png "Power BI hizmetinde kimlik bilgilerini edin")

1. HDInsight giriş hesabı bilgilerini girin ve ardından **Oturum Aç'ı**seçin. Varsayılan hesap adı *admin*’dir.

    ![Spark kümesinde oturum açma](./media/apache-spark-use-bi-tools/apache-spark-bi-sign-in.png "Spark kümesinde oturum açma")

1. Sol bölmede, Çalışma **Alanları** > **My Workspace** > **REPORTS'a**gidin, ardından **BuildingTemperature'ı**seçin.

    ![Sol bölmedeki raporlar altında listelenen rapor](./media/apache-spark-use-bi-tools/apache-spark-bi-service-left-pane.png "Sol bölmedeki raporlar altında listelenen rapor")

    **BinaSıcaklığı** değerinin sol bölmedeki **VERİ KÜMELERİ** altında da listelendiğini göreceksiniz.

    Power BI Desktop’ta oluşturduğunuz görsel artık Power BI hizmetinde kullanılabilir.

1. İmlecinizi görselleştirmenin üzerine takın ve sağ üst köşedeki pin simgesini seçin.

    ![Power BI hizmetinde rapor](./media/apache-spark-use-bi-tools/apache-spark-bi-service-report.png "Power BI hizmetinde rapor")

1. "Yeni pano"yu seçin, `Building temperature`adı girin, ardından **Pin'i**seçin.

    ![Yeni panoya sabitleme](./media/apache-spark-use-bi-tools/apache-spark-bi-pin-dashboard.png "Yeni panoya sabitleme")

1. Raporda, **panoya Git'i**seçin.

Görseliniz panoya sabitlenir. Rapora başka görseller ekleyebilir ve bu görselleri aynı panoya sabitleyebilirsiniz. Raporlar ve panolar hakkında daha fazla bilgi için Power [BI'deki Raporlar](https://powerbi.microsoft.com/documentation/powerbi-service-reports/) ve Power [BI'deki Panolar'a](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/)bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticiyi tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile verileriniz Azure Depolama'da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Kullanılmamış olsa bile bir HDInsight kümesi için de ücretlendirilirsiniz. Küme ücretleri depolama ücretlerinden kat kat daha fazla olduğundan, kümeleri kullanılmadıklarında silmek ekonomik açıdan mantıklıdır.

Bir kümeyi silmek için bkz: [Tarayıcınızı, PowerShell'i veya Azure CLI'yi kullanarak bir HDInsight kümesini sil.](../hdinsight-delete-cluster.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure HDInsight'taki bir Apache Spark kümesindeki verileri görselleştirmek için Microsoft Power BI'yi nasıl kullanacağınızı öğrendiniz. Bir sonraki makaleye bir makine öğrenme uygulaması oluşturabilirsiniz görmek için ilerleyin.

> [!div class="nextstepaction"]
> [Makine öğrenimi uygulaması oluşturma](./apache-spark-ipython-notebook-machine-learning.md)