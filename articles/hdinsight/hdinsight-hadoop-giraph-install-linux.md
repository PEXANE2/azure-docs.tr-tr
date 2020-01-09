---
title: Azure HDInsight 'ta Giraph 'yi yükleyip kullanma
description: Betik eylemleri kullanarak HDInsight kümelerine Giraph 'yi yüklemeyi öğrenin. Giraph 'yi kullanarak Azure bulutundaki Apache Hadoop grafik işleme gerçekleştirebilirsiniz.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 12/26/2019
ms.openlocfilehash: 1f6fd88ec492f26f6819dff099ec8fe53364ba0b
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2019
ms.locfileid: "75552262"
---
# <a name="install-apache-giraph-on-hdinsight-hadoop-clusters-and-use-giraph-to-process-large-scale-graphs"></a>HDInsight Hadoop kümelerine Apache Giraph 'yi yükler ve büyük ölçekli grafikleri işlemek için Giraph kullanma

Bir HDInsight kümesine Apache Giraph 'yi yüklemeyi öğrenin. HDInsight 'ın betik eylemi özelliği, bir bash betiği çalıştırarak kümenizi özelleştirmenizi sağlar. Betikler, küme oluşturma sırasında ve sonrasında kümeleri özelleştirmek için kullanılabilir.

## <a name="what-is-giraph"></a>Giraph nedir?

[Apache Giraph](https://giraph.apache.org/) , Hadoop kullanarak grafik işleme gerçekleştirmenize olanak tanır ve Azure HDInsight ile kullanılabilir. Nesneler arasındaki model ilişkilerini grafik olarak tanımlar. Örneğin, Internet gibi büyük bir ağdaki yönlendiriciler veya sosyal ağlardaki insanlar arasındaki ilişkiler arasındaki bağlantılar. Grafik işleme, bir grafikteki nesneler arasındaki ilişkilerle ilgili nedenleri sağlar; örneğin:

* Mevcut ilişkilerinizi temel alarak olası arkadaşları tanımlama.

* Bir ağdaki iki bilgisayar arasındaki en kısa yolu tanımlama.

* Web sayfalarının sayfa sırasını hesaplama.

> [!WARNING]  
> HDInsight kümesiyle birlikte sunulan bileşenler tam olarak desteklenir-Microsoft Desteği bu bileşenlerle ilgili sorunları yalıtmaya ve çözmeye yardımcı olur.
>
> Giraph gibi özel bileşenler, sorunu gidermeye yardımcı olmak için ticari olarak makul destek alır. Microsoft Desteği sorunu çözebiliyor olabilir. Aksi takdirde, bu teknolojinin derin uzmanlığı bulunan açık kaynaklı topluluklara başvurmalısınız. Örneğin, şu şekilde kullanılabilecek birçok topluluk sitesi vardır: [HDInsight Için MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [https://stackoverflow.com](https://stackoverflow.com). Ayrıca Apache projelerinin [https://apache.org](https://apache.org)proje siteleri vardır, örneğin: [Hadoop](https://hadoop.apache.org/).

## <a name="what-the-script-does"></a>Betiğin ne yapar

Bu betik aşağıdaki eylemleri gerçekleştirir:

* `/usr/hdp/current/giraph`için Giraph 'yi yüklüyor.

* `giraph-examples.jar` dosyasını kümeniz için varsayılan depolama alanına (için) kopyalar: `/example/jars/giraph-examples.jar`.

## <a name="install-giraph-using-script-actions"></a>Betik eylemlerini kullanarak Giraph 'yi yükler

Bir HDInsight kümesine Giraph yüklemek için örnek betik `https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh` adresinden edinilebilir

Bu bölüm, Azure portal kullanarak küme oluştururken örnek betiğin nasıl kullanılacağına ilişkin yönergeler sağlar.

> [!NOTE]  
> Bir betik eylemi, aşağıdaki yöntemlerden herhangi biri kullanılarak uygulanabilir:
> * Azure PowerShell
> * Azure CLI
> * HDInsight .NET SDK 'Sı
> * Azure Resource Manager şablonları
> 
> Zaten çalışan kümelere betik eylemleri de uygulayabilirsiniz. Daha fazla bilgi için bkz. [HDInsight kümelerini betik eylemleriyle özelleştirme](hdinsight-hadoop-customize-cluster-linux.md).

1. [Linux tabanlı HDInsight kümeleri oluşturma](hdinsight-hadoop-create-linux-clusters-portal.md)' daki adımları kullanarak bir küme oluşturmaya başlayın, ancak oluşturma işleminin tamammayın. **Klasik oluşturma deneyimini** ve **özel (boyut, ayarlar, uygulamalar)** kullanmanız gerekir.

1. **Küme boyutu** bölümünde, bu örnek için **çalışan düğümlerinin sayısının** en az 2 olduğundan emin olun.

1. **Betik eylemleri** bölümünde aşağıdaki bilgileri sağlayın:

    |Özellik |Değer |
    |---|---|
    |Betik türü|-Özel|
    |Ad|Giraph 'yi yükler|
    |Bash betiği URI 'SI|`https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh`|
    |Düğüm türleri|Baş|
    |Parametreler|Boş bırakın|

    Daha fazla bilgi için bkz. [küme oluşturma sırasında betik eylemi kullanma](./hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation).

1. [Linux tabanlı HDInsight kümeleri oluşturma](hdinsight-hadoop-create-linux-clusters-portal.md)bölümünde açıklandığı gibi Kümeyi oluşturmaya devam edin.

## <a name="how-do-i-use-giraph-in-hdinsight"></a>HDInsight 'ta Giraph 'yi kullanmak Nasıl yaparım? mı?

Küme oluşturulduktan sonra, Giraph ile birlikte gelen Simplekısaltestpathshesaplama örneğini çalıştırmak için aşağıdaki adımları kullanın. Bu örnek, bir grafikteki nesneler arasındaki en kısa yolu bulmak için temel bir [Pregel](https://people.apache.org/~edwardyoon/documents/pregel.pdf) uygulamasını kullanır.

1. Kümenize bağlanmak için [SSH komutunu](./hdinsight-hadoop-linux-use-ssh-unix.md) kullanın. CLUSTERNAME öğesini kümenizin adıyla değiştirerek aşağıdaki komutu düzenleyin ve ardından şu komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. **Tiny_graph. txt**adlı bir dosya oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    nano tiny_graph.txt
    ```

    Bu dosyanın içeriği olarak aşağıdaki metni kullanın:

    ```text
    [0,0,[[1,1],[3,3]]]
    [1,0,[[0,1],[2,2],[3,1]]]
    [2,0,[[1,2],[4,4]]]
    [3,0,[[0,3],[1,1],[4,4]]]
    [4,0,[[3,4],[2,4]]]
    ```

    Bu veriler, `[source_id, source_value,[[dest_id], [edge_value],...]]`biçimini kullanarak, yönlendirilmiş bir grafikteki nesneler arasındaki ilişkiyi açıklar. Her satır, bir `source_id` nesnesi ile bir veya daha fazla `dest_id` nesnesi arasındaki ilişkiyi temsil eder. `edge_value` `source_id` ve `dest\_id`arasındaki bağlantının gücü veya uzaklığı olarak düşünülebilir.

    Çizilmiş ve nesneler arasındaki mesafe olarak değer (veya ağırlık) kullanıldığında, veriler aşağıdaki diyagram gibi görünebilir:

    ![tiny_graph. txt, aralarında değişen mesafe çizgileri olan daireler olarak çizilir](./media/hdinsight-hadoop-giraph-install-linux/hdinsight-giraph-graph.png)

3. Dosyayı kaydetmek için **CTRL + X**, sonra **Y**ve son olarak, dosya adını kabul etmek için **ENTER** tuşunu kullanın.

4. HDInsight kümeniz için verileri birincil depolamaya depolamak için aşağıdakileri kullanın:

    ```bash
    hdfs dfs -put tiny_graph.txt /example/data/tiny_graph.txt
    ```

5. Şu komutu kullanarak Simplekısaltestpathshesaplama örneğini çalıştırın:

    ```bash
    yarn jar /usr/hdp/current/giraph/giraph-examples.jar org.apache.giraph.GiraphRunner org.apache.giraph.examples.SimpleShortestPathsComputation -ca mapred.job.tracker=headnodehost:9010 -vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat -vip /example/data/tiny_graph.txt -vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat -op /example/output/shortestpaths -w 2
    ```

    > [!IMPORTANT]
    > `-w` geçirilen değer, çalışan düğümlerin gerçek sayısından küçük veya bu değere eşit olmalıdır.

    Bu komutla kullanılan parametreler aşağıdaki tabloda açıklanmıştır:

   | Parametre | Neler yapar? |
   | --- | --- |
   | Van |Örnekleri içeren jar dosyası. |
   | org. Apache. Giraph. Gioyphrunner |Örnekleri başlatmak için kullanılan sınıf. |
   | org. Apache. Giraph. Examples. Simplekısaltestpathshesaplama |Kullanılan örnek. Bu örnekte, 1 KIMLIĞI ve grafikteki diğer tüm kimlikler arasındaki en kısa yolu hesaplar. |
   | -CA mapred. job. Tracker |Küme için baş düğümüne. |
   | -VIF |Giriş verileri için kullanılacak giriş biçimi. |
   | -VIP |Giriş veri dosyası. |
   | -VOF |Çıkış biçimi. Bu örnekte, ID ve değer düz metin olarak. |
   | -Op |Çıkış konumu. |
   | -w 2 |Kullanılacak çalışanların sayısı. Bu örnekte, 2. |

    Bunlar hakkında daha fazla bilgi ve Giraph örnekleri ile kullanılan diğer parametreler için bkz. [Giraph hızlı](https://giraph.apache.org/quick_start.html)başlangıcı.

6. İş tamamlandıktan sonra sonuçlar **/example/output/kısaltestpaths** dizininde depolanır. Çıkış dosyası adları, **bölüm-d** ile başlar ve birinci, ikinci ve bu gibi bir sayı ile biter. Çıktıyı görüntülemek için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -text /example/output/shortestpaths/*
    ```

    Çıktı aşağıdaki metne benzer şekilde görünür:

    ```output
    0    1.0
    4    5.0
    2    2.0
    1    0.0
    3    1.0
    ```

    Simplekısaltestpathhesaplama örneği, nesne KIMLIĞI 1 ile başlamak ve diğer nesnelerin en kısa yolunu bulmak için sabit kodlanmış bir örnektir. Çıktı `destination_id` ve `distance`biçimindedir. `distance`, nesne KIMLIĞI 1 ile hedef KIMLIĞI arasındaki kenarlarının değeri (veya ağırlığı) değeridir.

    Bu verileri görselleştirerek, KIMLIK 1 ve diğer tüm nesneler arasındaki en kısa yolu taşıyarak sonuçları doğrulayabilirsiniz. KIMLIK 1 ve KIMLIK 4 arasındaki en kısa yol 5 ' tir. Bu değer 1 ile 3 arasındaki toplam uzaklığın ve KIMLIK 3 ve 4 ' ü arasındadır.

    ![Nesnelerin arasına çizilmiş en kısa yolların bulunduğu daireler](./media/hdinsight-hadoop-giraph-install-linux/hdinsight-giraph-graph-out.png)

## <a name="next-steps"></a>Sonraki adımlar

[HDInsight kümelerinde ton 'U yükleyip kullanın](hdinsight-hadoop-hue-linux.md).
