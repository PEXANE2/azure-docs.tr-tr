---
title: Visual Studio & Hortonworks için Veri Gölü araçları - Azure HDInsight
description: Yerel bir VM'de çalışan Hortonworks sanal alanıyla Visual Studio için Azure Veri Gölü araçlarını nasıl kullanacağınızı öğrenin. Bu araçlarla, zAnda hive ve pig işleri oluşturabilir ve çalıştırabilir ve iş çıktısını ve geçmişini görüntüleyebilirsiniz.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/07/2018
ms.openlocfilehash: e128aaf6e1726b7a1341fefc6df3cdafd3beb880
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73500161"
---
# <a name="use-the-azure-data-lake-tools-for-visual-studio-with-the-hortonworks-sandbox"></a>Hortonworks Sandbox ile Visual Studio için Azure Veri Gölü araçlarını kullanın

Azure Veri Gölü, genel Apache Hadoop kümeleriyle çalışmak için araçlar içerir. Bu belge, yerel bir sanal makinede çalışan Hortonworks Sandbox ile Veri Gölü araçlarını kullanmak için gereken adımları sağlar.

Hortonworks Sandbox'ı kullanmak, Hadoop ile geliştirme ortamınızda yerel olarak çalışmanızı sağlar. Bir çözüm geliştirip ölçekte dağıtmak istedikten sonra, bir HDInsight kümesine geçebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* Hortonworks Sandbox, geliştirme ortamınızda sanal bir makinede çalışıyor. Bu belge Oracle VirtualBox'ta çalışan sanal alanla yazılmış ve test edilmiştir. Kum havuzunu kurma hakkında daha fazla bilgi için [Hortonworks kum havuzuna başlayın'](hadoop/apache-hadoop-emulator-get-started.md) a bakın. Belge.

* Visual Studio.

* .NET 2.7.1 veya sonrası [için Azure SDK.](https://azure.microsoft.com/downloads/)

* [Visual Studio için Azure Veri Gölü araçları.](https://www.microsoft.com/download/details.aspx?id=49504)

## <a name="configure-passwords-for-the-sandbox"></a>Sandbox için parolaları yapılandırma

Hortonworks Sandbox'ın çalıştığından emin olun. Ardından [Hortonworks Sandbox belgesinde Başlat'taki](hadoop/apache-hadoop-emulator-get-started.md#set-sandbox-passwords) adımları izleyin. Bu adımlar, SSH `root` hesabının ve Apache Ambari `admin` hesabının parolasını yapılandırır. Bu parolalar Visual Studio'dan kum havuzuna bağlandığınızda kullanılır.

## <a name="connect-the-tools-to-the-sandbox"></a>Araçları kum havuzuna bağlayın

1. Visual Studio'yı açın, **Görünüm'i**seçin ve ardından **Server Explorer'ı**seçin.

2. **Server Explorer'dan** **HDInsight** girişine sağ tıklayın ve ardından **HDInsight Emülatöre Bağlan'ı**seçin.

    ![Sunucu Explorer, HDInsight Emülatörconnect ile vurgulanır](./media/hdinsight-hadoop-emulator-visual-studio/connect-hdinsight-emulator.png)

3. **Connect'ten HDInsight Emulator iletişim kutusuna,** Ambari için yapılandırdığınız parolayı girin.

    ![Ambari şifre metin kutusu vurgulanmış iletişim kutusuekran görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/enter-ambari-password.png)

    Devam etmek için **İleri**’yi seçin.

4. Hesap için yapılandırdığınız parolayı girmek için Parola alanını kullanın. **Password** `root` Diğer alanları varsayılan değerde bırakın.

    ![Kök parola metin kutusu vurgulanmış iletişim kutusunun ekran görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/enter-root-password1.png)

    Devam etmek için **İleri**’yi seçin.

5. Hizmetlerin onaylanmasının tamamlanmasını bekleyin. Bazı durumlarda, doğrulama başarısız olabilir ve yapılandırmayı güncelleştirmenizi isteyebilir. Doğrulama başarısız olursa, **Güncelleştirme'yi**seçin ve hizmetin tamamlanması için yapılandırma ve doğrulamayı bekleyin.

    ![İletişim kutusunun ekran görüntüsü, Güncelleştirme düğmesi vurgulanmış](./media/hdinsight-hadoop-emulator-visual-studio/fail-and-update-window.png)

    > [!NOTE]  
    > Güncelleştirme işlemi, Hortonworks Sandbox yapılandırmasını Visual Studio için Veri Gölü araçlarının beklediği şekilde değiştirmek için Ambari'yi kullanır.

6. Doğrulama tamamlandıktan sonra yapılandırmayı tamamlamak için **Finish'i** seçin.
    ![İletişim kutusunun ekran görüntüsü, Bitiş düğmesi vurgulanmış](./media/hdinsight-hadoop-emulator-visual-studio/finished-connect-dialog.png)

     >[!NOTE]  
     > Geliştirme ortamınızın hızına ve sanal makineye ayrılan bellek miktarına bağlı olarak, hizmetleri yapılandırmak ve doğrulamak birkaç dakika sürebilir.

Bu adımları takip ettikten sonra, şimdi SERVER Explorer'da **HDInsight** bölümü altında bir **HDInsight yerel küme** girişine sahipsiniz.

## <a name="write-an-apache-hive-query"></a>Apache Hive sorgusu yazma

Hive yapılandırılmış verilerle çalışmak için SQL benzeri bir sorgu dili (HiveQL) sağlar. Yerel kümeye karşı isteğe bağlı sorguları nasıl çalıştırılaoynayacağınızı öğrenmek için aşağıdaki adımları kullanın.

1. **Server**Explorer'da, daha önce eklediğiniz yerel kümenin girişini sağ tıklatın ve ardından **Kovan Sorgusu Yaz'ı**seçin.

    ![Sunucu Gezgini ekran görüntüsü, Bir Kovan Sorgusu Yaz vurgulanan](./media/hdinsight-hadoop-emulator-visual-studio/write-apache-hive-query.png)

    Yeni bir sorgu penceresi görüntülenir. Burada hızlı bir şekilde yazabilir ve yerel kümeye bir sorgu gönderebilirsiniz.

2. Yeni sorgu penceresinde aşağıdaki komutu girin:

        select count(*) from sample_08;

    Sorguyu çalıştırmak için pencerenin üst kısmında **Gönder'i** seçin. Diğer değerleri **(Toplu Iş ve** sunucu adı) varsayılan değerlere bırakın.

    ![Sorgu penceresinin ekran görüntüsü, Gönder düğmesi vurgulanmış](./media/hdinsight-hadoop-emulator-visual-studio/query-window-submit-hive.png)

    **Gelişmiş'i**seçmek için **Gönder'in** yanındaki açılır menüyü de kullanabilirsiniz. Gelişmiş seçenekler, işi gönderirken ek seçenekler sunmanıza olanak sağlar.

    ![Komut Dosyası Gönder iletişim kutusu kovanı ekran görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/advanced-apache-hive.png)

3. Sorguyu gönderdikten sonra iş durumu görüntülenir. İş durumu, Hadoop tarafından işlenirken iş hakkında bilgi görüntüler. **İş Durumu,** işin durumunu sağlar. Durum düzenli aralıklarla güncelleştirilir veya durumu el ile yenilemek için yenileme simgesini kullanabilirsiniz.

    ![İş Durumu vurgulandığı İş Görünümü iletişim kutusunun ekran görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/job-view-dialog-box1.png)

    İş **Durumu** **Bitti'de**değişiklik yaptıktan sonra, Yönlendirilmiş Asiklik Grafik (DAG) görüntülenir. Bu diyagram, Hive sorgusunu işlerken Tez tarafından belirlenen yürütme yolunu açıklar. Tez, yerel kümedeki Hive'ın varsayılan yürütme motorudur.

    > [!NOTE]  
    > Apache Tez, Linux tabanlı HDInsight kümelerini kullanırken de varsayılan değerdir. Windows tabanlı HDInsight'ta varsayılan değildir. Burada kullanmak için, kovan `set hive.execution.engine = tez;` sorgunuzun başına satırı eklemeniz gerekir.

    Çıktıyı görüntülemek için **İş Çıktısı** bağlantısını kullanın. Bu durumda, 823, sample_08 tablosunda satır sayısıdır. **İş Günlüğü** ve **Download YARN Log** bağlantılarını kullanarak iş ile ilgili tanılama bilgilerini görüntüleyebilirsiniz.

4. **Ayrıca, Toplu İşlem** alanını **Interactive**olarak değiştirerek Kovan işlerini etkileşimli olarak çalıştırabilirsiniz. Sonra **Yürüt''ün'u**seçin.

    ![Vurgulanan Etkileşimli ve Yürüt düğmesinin ekran görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/hdi-interactive-query.png)

    Etkileşimli bir sorgu, işlem sırasında oluşturulan çıkış günlüğünü **HiveServer2 Çıktı** penceresine aktarıyor.

    > [!NOTE]  
    > Bilgiler, bir iş bittikten sonra **İş Günlüğü** bağlantısından edinilebilen bilgilerle aynıdır.

    ![HiveServer2 çıktısının ekran görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/hiveserver2-output-box.png)

## <a name="create-a-hive-project"></a>Hive projesi oluşturma

Ayrıca, birden çok Hive komut dosyası içeren bir proje de oluşturabilirsiniz. İlgili komut dosyalarınız varsa veya komut dosyalarını bir sürüm denetim sisteminde depolamak istediğinizde projeyi kullanın.

1. Visual Studio'da **Dosya**, **Yeni**ve ardından **Project'i**seçin.

2. Projeler listesinden **Şablonları**genişletin, **Azure Veri Gölü'nü**genişletin ve ardından **HIVE (HDInsight) seçeneğini belirleyin.** Şablonlar listesinden **Kovan Örneği'ni**seçin. Bir ad ve konum girin ve ardından **Tamam'ı**seçin.

    ![Azure Veri Gölü, Kovan Örneği ve Tamam ile Yeni Proje penceresi](./media/hdinsight-hadoop-emulator-visual-studio/new-apache-hive-project.png)

**Hive Örnek** proje iki komut dosyası, **WebLogAnalysis.hql** ve **SensorDataAnalysis.hql**içerir. Bu komut dosyalarını pencerenin üst kısmındaki aynı **Gönder** düğmesini kullanarak gönderebilirsiniz.

## <a name="create-an-apache-pig-project"></a>Apache Pig projesi oluşturma

Hive yapılandırılmış verilerle çalışmak için SQL benzeri bir dil sağlarken, Pig veriler üzerinde dönüşümler gerçekleştirerek çalışır. Pig dönüşümleri bir boru hattı geliştirmenize olanak sağlayan bir dil (Domuz Latince) sağlar. Yerel kümeyle Domuz'u kullanmak için aşağıdaki adımları izleyin:

1. Visual Studio'u açın ve **Dosya**, **Yeni**ve ardından **Project'i**seçin. Projeler listesinden **Şablonları**genişletin, **Azure Veri Gölü'nü**genişletin ve ardından **Pig (HDInsight) seçeneğini belirleyin.** Şablonlar listesinden Domuz **Uygulaması'nı**seçin. Bir ad, konum girin ve sonra **Tamam'ı**seçin.

    ![Azure Veri Gölü, Domuz, Domuz Uygulaması ve Ok vurgulanmış Yeni Proje penceresinin ekran görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/new-apche-pig-project.png)

2. Bu proje ile oluşturulan **komut dosyası.pig** dosyasının içeriği olarak aşağıdaki metni girin.

        a = LOAD '/demo/data/Website/Website-Logs' AS (
            log_id:int,
            ip_address:chararray,
            date:chararray,
            time:chararray,
            landing_page:chararray,
            source:chararray);
        b = FILTER a BY (log_id > 100);
        c = GROUP b BY ip_address;
        DUMP c;

    Pig Hive'dan farklı bir dil kullanırken, işleri nasıl **çalıştırdığınız, Gönder** düğmesi aracılığıyla her iki dil arasında tutarlıdır. **Gönder'in** yanındaki açılır açılır nokta seçilirken Pig için gelişmiş bir gönder iletişim kutusu görüntülenir.

    ![Script gönder iletişim kutusu domuz unun ekran görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/advanced-apache-pig1.png)

3. İş durumu ve çıktı, bir Hive sorgusu yla aynı şekilde de görüntülenir.

    ![Tamamlanmış bir Pig işinin ekran görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/completed-apache-pig.png)

## <a name="view-jobs"></a>İşleri görüntüle

Data Lake araçları ayrıca Hadoop'ta çalıştırılabilen işler hakkındaki bilgileri kolayca görüntülemenize olanak sağlar. Yerel kümede çalıştırılabilen işleri görmek için aşağıdaki adımları kullanın.

1. **Sunucu Gezgini'nden,** yerel kümeyi sağ tıklatın ve ardından **İş İllerini Görüntüle'yi**seçin. Kümeye gönderilen işlerin listesi görüntülenir.

    ![Görünüm İşleri vurgulanmış sunucu gezgini ekran görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/server-explorer-view-jobs.png)

2. İş listesinden iş ayrıntılarını görüntülemek için birini seçin.

    ![İş Tarayıcısı ekran görüntüsü, bir iş vurgulanan ile](./media/hdinsight-hadoop-emulator-visual-studio/hdi-view-job-details.png)

    Görüntülenen bilgiler, çıktıyı ve günlük bilgilerini görüntülemek için bağlantılar da dahil olmak üzere bir Hive veya Pig sorgusuçalıştırdıktan sonra gördükleriniz inanın benzer.

3. Ayrıca buradan işi değiştirebilir ve yeniden gönderebilirsiniz.

## <a name="view-hive-databases"></a>Kovan veritabanlarını görüntüleyin

1. **Server Explorer'da** **HDInsight yerel küme** girişini genişletin ve ardından **Kovan Veritabanlarını**genişletin. Yerel kümedeki **Varsayılan** ve **xademo** veritabanları görüntülenir. Veritabanını genişletmek, veritabanı içindeki tabloları gösterir.

    ![Sunucu Gezgini ekran görüntüsü, veritabanları genişletilmiş](./media/hdinsight-hadoop-emulator-visual-studio/expanded-databases-view.png)

2. Tabloyu genişletmek, bu tablonun sütunlarını görüntüler. Verileri hızla görüntülemek için bir tabloyu sağ tıklatın ve **En İyi 100 Satırı Görüntüle'yi**seçin.

    ![Sunucu Gezgini, tablo genişletilmiş ve Seçilen En İyi 100 Satırı Görüntüle](./media/hdinsight-hadoop-emulator-visual-studio/hdi-view-top-100-rows.png)

### <a name="database-and-table-properties"></a>Veritabanı ve tablo özellikleri

Veritabanı veya tablonun özelliklerini görüntüleyebilirsiniz. **Özellikler'i** seçmek, özellikler penceresinde seçili öğenin ayrıntılarını görüntüler. Örneğin, aşağıdaki ekran görüntüsünde gösterilen bilgilere bakın:

![Özellikler penceresinin ekran görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/hdi-properties-window.png)

### <a name="create-a-table"></a>Bir tablo oluşturma

Tablo oluşturmak için veritabanını sağ tıklatın ve ardından **Tablo Oluştur'u**seçin.

![Create Table vurgulanmış sunucu gezgininin ekran görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/server-explorer-create-table.png)

Daha sonra bir form kullanarak tablo oluşturabilirsiniz. Aşağıdaki ekran görüntüsünün alt kısmında, tablooluşturmak için kullanılan ham HiveQL görebilirsiniz.

![Tablo oluşturmak için kullanılan formun ekran görüntüsü](./media/hdinsight-hadoop-emulator-visual-studio/create-table-form-box.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Hortonworks Sandbox ipleri öğrenme](https://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [Apaçi Hadoop öğretici - HDP ile başlarken](https://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)
