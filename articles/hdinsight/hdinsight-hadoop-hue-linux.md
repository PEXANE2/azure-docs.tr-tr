---
title: HDInsight Linux tabanlı kümelerde Hadoop ile Hue - Azure
description: Hue'yu HDInsight kümelerine nasıl yükleyin ve istekleri Hue'ye yönlendirmek için tünel kazmayı kullanmayı öğrenin. Depolama alanına göz atmak ve Kovan veya Domuz'u çalıştırmak için Hue'yu kullanın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 11/28/2019
ms.openlocfilehash: 69acfd4f2edab9be1b1dcfbb52eafbd00aec712f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75934574"
---
# <a name="install-and-use-hue-on-hdinsight-hadoop-clusters"></a>HDInsight Hadoop kümelerinde Hue'yi yükleyin ve kullanın

Hue'yu HDInsight kümelerine nasıl yükleyin ve istekleri Hue'ye yönlendirmek için tünel kazmayı kullanmayı öğrenin.

## <a name="what-is-hue"></a>Hue nedir?

Hue, Apache Hadoop kümesiyle etkileşim kurmak için kullanılan bir Web uygulamaları kümesidir. Bir Hadoop kümesiyle ilişkili depolama alanına göz atmak için Hue'yi kullanabilirsiniz (WASB, HDInsight kümeleri durumunda), Kovan işleri ve Pig komut dosyaları çalıştırın, vb. Aşağıdaki bileşenler, HDInsight Hadoop kümesindeki Hue yüklemeleri ile kullanılabilir.

* Balmumu Kovan Editörü
* Apaçi Domuz
* Metastore yöneticisi
* Apaçi Oozie
* FileBrowser (WASB varsayılan kapsayıcı için görüşmeler)
* İş Tarayıcısı

> [!WARNING]  
> HDInsight kümesiyle sağlanan bileşenler tam olarak desteklenir ve Microsoft Desteği bu bileşenlerle ilgili sorunları yalıtmak ve çözmeye yardımcı olur.
>
> Özel bileşenler, sorunu daha fazla gidermenize yardımcı olmak için ticari olarak makul destek alır. Bu, sorunun çözülmesiyle veya bu teknoloji için derin uzmanlık bulunan açık kaynak teknolojileri için kullanılabilir kanallardan bağlantı kurmanızı istemesiyle sonuçlanabilir. Örneğin, kullanılabilir birçok topluluk sitesi vardır, örneğin: [HDInsight için MSDN forumu](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [https://stackoverflow.com](https://stackoverflow.com). Ayrıca Apache projeleri, [https://apache.org](https://apache.org)örneğin proje siteleri var: [Hadoop](https://hadoop.apache.org/).

## <a name="install-hue-using-script-actions"></a>Komut Dosyası Eylemlerini Kullanarak Ton Yükleme

Komut Dosyası Eyleminiz için aşağıdaki tablodaki bilgileri kullanın. Komut Dosyası Eylemleri'ni kullanma yla ilgili belirli yönergeler için [KOMUT Dosyası Eylemleri ile HDInsight kümelerini özelleştir'e](hdinsight-hadoop-customize-cluster-linux.md) bakın.

> [!NOTE]  
> Hue'yu HDInsight kümelerine yüklemek için önerilen kafa düğümü boyutu en az A4 'dür (8 çekirdek, 14 GB bellek).

|Özellik |Değer |
|---|---|
|Komut dosyası türü:|- Özel|
|Adı|Ton Yükle|
|Bash script URI|`https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh`|
|Düğüm türü(ler):|Head|

## <a name="use-hue-with-hdinsight-clusters"></a>HDInsight kümeleriyle Hue'yi kullanma

SSH Tünelleme, kümedeki Tona erişebilmek için tek yoldur. SSH üzerinden tünel yapmak, trafiğin doğrudan Hue'nin çalıştığı kümenin headnode'una gitmesini sağlar. Küme sağlama yı bitirdikten sonra, Bir HDInsight kümesinde Hue'yu kullanmak için aşağıdaki adımları kullanın.

> [!NOTE]  
> Aşağıdaki talimatları takip etmek için Firefox web tarayıcısını kullanmanızı öneririz.

1. [Apache Ambari web Kullanıcı Arabirimi, ResourceManager, JobHistory, NameNode, Oozie ve diğer web Kullanıcı Arabirimi'ne erişmek için SSH Tünel](hdinsight-linux-ambari-ssh-tunnel.md) oluşturmayı kullanarak istemci sisteminizden HDInsight kümesine bir SSH tüneli oluşturun ve ardından Web tarayıcınızı proxy olarak kullanacak şekilde yapılandırın.

1. Kümenize bağlanmak için [ssh komutunu](./hdinsight-hadoop-linux-use-ssh-unix.md) kullanın. CLUSTERNAME'yi kümenizin adıyla değiştirerek aşağıdaki komutu düzenleme ve ardından komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Bağlandıktan sonra, birincil başlığın tam nitelikli etki alanı adını elde etmek için aşağıdaki komutu kullanın:

    ```bash
    hostname -f
    ```

    Bu, aşağıdakilere benzer bir ad döndürecektir:

        myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net

    Bu, Hue web sitesinin bulunduğu birincil başlık başlığının ana adıdır.

1. 'de `http://HOSTNAME:8888`Hue portalını açmak için tarayıcıyı kullanın. HOSTNAME'yi önceki adımda elde ettiğiniz adla değiştirin.

   > [!NOTE]  
   > İlk kez oturum açtığınızda, Hue portalında oturum açmak için bir hesap oluşturmanız istenir. Burada belirttiğiniz kimlik bilgileri portalla sınırlı olacaktır ve kümeyi sağlarken belirttiğiniz yönetici veya SSH kullanıcı kimlik bilgileriyle ilgili değildir.

    ![HDInsight ton portalı giriş penceresi](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-login.png "Hue portalı için kimlik bilgilerini belirtin")

### <a name="run-a-hive-query"></a>Hive sorgusu çalıştırma

1. Hue portalından **Sorgu Düzenleyicileri'ni**seçin ve ardından Hive düzenleyicisini açmak için **Hive'ı** seçin.

    ![HDInsight ton portalı kullanım kovan editörü](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-use-hive.png "Kovan kullanın")

2. **Assist** sekmesinde, **Veritabanı**altında **kovan örneği tablosunu**görmeniz gerekir. Bu HDInsight tüm Hadoop kümeleri ile sevk bir örnek tablodur. Sağ bölmeye örnek bir sorgu girin ve ekran yakalamada gösterildiği gibi aşağıdaki bölmedeki **Sonuçlar** sekmesinde çıktıyı görün.

    ![HDInsight hue portal kovan sorgusu](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-hive-query.png "Kovan sorgusunu çalıştır")

    Sonucun görsel bir temsilini görmek için **Grafik** sekmesini de kullanabilirsiniz.

### <a name="browse-the-cluster-storage"></a>Küme depolama alanına göz atın

1. Hue portalından, menü çubuğunun sağ üst köşesinde **Dosya Tarayıcısı'nı** seçin.
2. Varsayılan olarak dosya tarayıcısı **/user/myuser** dizininde açılır. Kümeyle ilişkili Azure depolama kapsayıcısının köküne gitmek için kullanıcı dizininden hemen önce ileri eğik çizgiyi seçin.

    ![HDInsight renk portalı dosya tarayıcısı](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-file-browser.png "Dosya tarayıcısı kullanma")

3. Kullanılabilir işlemleri görmek için bir dosyaveya klasöre sağ tıklayın. Dosyaları geçerli dizine yüklemek için sağ köşedeki **Yükle** düğmesini kullanın. Yeni dosyalar veya dizinler oluşturmak için **Yeni** düğmesini kullanın.

> [!NOTE]  
> Hue dosya tarayıcısı yalnızca HDInsight kümesiyle ilişkili varsayılan kapsayıcının içeriğini gösterebilir. Kümeyle ilişkili olabileceğiniz ek depolama hesapları/kapsayıcıları dosya tarayıcısı kullanılarak erişilemez. Ancak, kümeyle ilişkili ek kapsayıcılar Hive işleri için her zaman erişilebilir olacaktır. Örneğin, Hive düzenleyicisinde `dfs -ls wasbs://newcontainer@mystore.blob.core.windows.net` komutu girerseniz, ek kapsayıcıların içeriğini de görebilirsiniz. Bu komutta, **yeni kapsayıcı** bir küme ile ilişkili varsayılan kapsayıcı değildir.

## <a name="important-considerations"></a>Önemli noktalar

1. Hue'yu yüklemek için kullanılan komut dosyası, yalnızca kümenin birincil headnode'una yükler.

1. Kurulum sırasında, yapılandırmayı güncelleştirmek için birden fazla Hadoop hizmeti (HDFS, YARN, MR2, Oozie) yeniden başlatılır. Komut dosyası Hue yüklemeyi bitirdikten sonra, diğer Hadoop hizmetlerinin başlatılması biraz zaman alabilir. Bu, başlangıçta Hue'nin performansını etkileyebilir. Tüm hizmetler başladığında, Hue tamamen işlevsel olacaktır.

1. Hue, Hive için geçerli varsayılan olan Apache Tez işlerini anlamıyor. MapReduce'ı Kovan yürütme motoru olarak kullanmak istiyorsanız, komut dosyanızdaki aşağıdaki komutu kullanmak için komut dosyasını güncelleştirin:

        set hive.execution.engine=mr;

1. Linux kümeleri ile, Kaynak Yöneticisi ikincil de çalışıyor olabilir iken hizmetlerinbirincil başlık üzerinde çalışan bir senaryo olabilir. Böyle bir senaryo, kümedeki RUNNING işlerinin ayrıntılarını görüntülemek için Hue'yu kullanırken hatalara (aşağıda gösterilmiştir) neden olabilir. Ancak, iş tamamlandığında iş ayrıntılarını görüntüleyebilirsiniz.

   ![Ton portalı hatası örnek ileti](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-error.png "Ton portalı hatası")

   Bunun nedeni bilinen bir sorundur. Geçici çözüm olarak Ambari'yi değiştirin, böylece etkin Kaynak Yöneticisi de birincil başlıküzerinde çalışır.

1. HDInsight kümeleri Azure Depolama'yı kullanarak `wasbs://`kullanırken Hue WebHDFS'yi anlıyor. Yani, komut dosyası eylem ile kullanılan özel komut dosyası WASB konuşmak için bir WebHDFS uyumlu bir hizmettir WebWasb yükler. Bu nedenle, Hue portalı yerlerde HDFS diyor olsa bile **(Dosya Tarayıcısı**üzerinde farenizi hareket ettirdiğinizde olduğu gibi), WASB olarak yorumlanmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

[HDInsight kümelerine R yükleyin.](hdinsight-hadoop-r-scripts-linux.md) HDInsight Hadoop kümelerine R yüklemek için küme özelleştirmesini kullanın. R, istatistiksel bilgi işlem için açık kaynak kodlu bir dil ve ortamdır. Yüzlerce yerleşik istatistiksel işlev ve işlevsel ve nesne yönelimli programlama yönlerini birleştiren kendi programlama dili sağlar. Ayrıca kapsamlı grafik yetenekleri sağlar.
