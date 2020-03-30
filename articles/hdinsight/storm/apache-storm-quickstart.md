---
title: 'Hızlı başlangıç: Apache Storm topolojisi oluşturma/yönetme - Azure HDInsight'
description: Hızlı başlangıç ta, Azure HDInsight'ta bir Apache Storm topolojisini nasıl oluşturup izleyeceğinizi öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.date: 06/14/2019
ms.author: hrasheed
ms.custom: mvc
ms.openlocfilehash: 24173b553f30f652caf20b1ec7500fd9c4d2f7a0
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "73241232"
---
# <a name="quickstart-create-and-monitor-an-apache-storm-topology-in-azure-hdinsight"></a>Hızlı başlangıç: Azure HDInsight'ta Bir Apache Storm topolojisi oluşturun ve izleyin

Apache Storm, veri akışlarını işlemeye yönelik ölçeklenebilir, hataya dayanıklı, dağıtılmış ve gerçek zamanlı bir işlem sistemidir. Azure HDInsight’ta Storm ile büyük veri analizini gerçek zamanlı olarak gerçekleştiren bulut tabanlı bir Storm kümesi oluşturabilirsiniz.

Bu hızlı başlangıçta, apache [storm-starter](https://github.com/apache/storm/tree/v2.0.0/examples/storm-starter) projesinden bir örnek kullanarak bir Apache Storm topolojisi oluşturup mevcut bir Apache Storm kümesini izlersiniz.

## <a name="prerequisites"></a>Ön koşullar

* HDInsight'ta bir Apache Storm kümesi. Bkz. [Azure portalını kullanarak Apache Hadoop kümeleri oluşturun](../hdinsight-hadoop-create-linux-clusters-portal.md) ve Küme türü için **Fırtına'yı**seçin. **Storm**

* Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight'a (Apache Hadoop) bağlan'a](../hdinsight-hadoop-linux-use-ssh-unix.md)bakın.

## <a name="create-the-topology"></a>Topolojiyi oluşturun

1. Fırtına kümenize bağlanın. Fırtına kümenizin adını `CLUSTERNAME` değiştirerek aşağıdaki komutu düzenleme ve ardından komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. **WordCount** örneği, HDInsight kümenizde `/usr/hdp/current/storm-client/contrib/storm-starter/`. Topoloji rasgele cümleler oluşturur ve kelimelerin kaç kez oluştuğunu sayar. Kümedeki **sözcük sayısı** topolojisini başlatmak için aşağıdaki komutu kullanın:

    ```bash
    storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology wordcount
    ```

## <a name="monitor-the-topology"></a>Topolojiyi izleme

Storm, çalışan topolojilerle çalışmak için bir web arabirimi sağlar ve HDInsight kümenize dahildir.

Storm Kullanıcı Arabirimini kullanarak topolojiyi izlemek için aşağıdaki adımları kullanın:

1. Storm kullanıcı arabirimini görüntülemek için bir web tarayıcısı açarak `https://CLUSTERNAME.azurehdinsight.net/stormui` adresine gidin. `CLUSTERNAME` değerini kümenizin adıyla değiştirin.

2. **Topoloji Özeti**altında, **Ad** sütunundaki **sözcük sayısı** girişini seçin. Topoloji hakkında bilgiler görüntülenir.

    ![Storm-starter WordCount topoloji bilgilerini içeren Storm Panosu.](./media/apache-storm-quickstart/hdi-topology-summary.png)

    Yeni sayfa aşağıdaki bilgileri sağlar:

    |Özellik | Açıklama |
    |---|---|
    |Topoloji istatistikleri|Topoloji performansı hakkında temel bilgiler, zaman pencerelerine düzenlenmiştir. Belirli bir zaman penceresinin seçilmesi sayfanın diğer bölümlerinde gösterilen bilgiler için zaman penceresini değiştirir.|
    |Ağızlı|Her emzit tarafından döndürülen son hata da dahil olmak üzere, emzmeler hakkında temel bilgiler.|
    |Cıvata|Cıvatalar hakkında temel bilgiler.|
    |Topoloji yapılandırması|Topoloji yapılandırması hakkında detaylı bilgi.|
    |Etkinleştir|Devre dışı bırakılmış bir topolojinin işlemesini devam ettirer.|
    |Devre dışı bırak|Çalışan bir topolojiyi duraklatabilir.|
    |Yeniden dengeleme|Topolojinin paralelliğini ayarlar. Kümedeki düğüm sayısını değiştirdikten sonra çalışan topolojileri yeniden dengelemeniz gerekir. Yeniden dengeleme, kümede artan/azalan düğüm sayısını dengelemek üzere paralelliği ayarlamaya imkan tanır. Daha fazla bilgi için bkz: [Apache Storm topolojisinin paralelliğini anlamak.](https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html)|
    |Öldür|Belirtilen zaman anından sonra Fırtına topolojisini sonlandırır.|

3. Bu sayfada **Spout’lar** veya **Cıvatalar** bölümünden bir giriş seçin. Seçilen bileşen hakkında bilgiler görüntülenir.

    ![Seçili bileşenler hakkında bilgi içeren Storm Panosu.](./media/apache-storm-quickstart/hdi-component-summary.png)

    Yeni sayfada aşağıdaki bilgiler görüntülenir:

    |Özellik | Açıklama |
    |---|---|
    |Spout/Cıvata istatistikleri|Zaman pencerelerine düzenlenmiş bileşen performansı yla ilgili temel bilgiler. Belirli bir zaman penceresinin seçilmesi sayfanın diğer bölümlerinde gösterilen bilgiler için zaman penceresini değiştirir.|
    |Giriş istatistikleri (yalnızca cıvata)|Cıvata tarafından tüketilen verileri üreten bileşenler hakkında bilgi.|
    |Çıktı istatistikleri|Bu cıvata tarafından yayılan veriler hakkında bilgi.|
    |Executors|Bu bileşenin örnekleri hakkında bilgi.|
    |Hatalar|Bu bileşen tarafından üretilen hatalar.|

4. Spout veya cıvata ayrıntılarını görüntülerken bileşenin belirli bir örneğine ilişkin ayrıntıları görmek için **Yürütücüler** bölümündeki **Bağlantı Noktası** sütunundan bir giriş seçin.

        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["with"]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["nature"]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [snow]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [snow, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [white]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [white, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [seven]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [seven, 1493957]

    Bu örnekte **seven** kelimesi 1493957 kez geçmiştir. Bu sayı, bu topoloji başlatıldığından beri kelimeyle kaç kez karşılaşıldığını gösterir.

## <a name="stop-the-topology"></a>Topolojiyi durdurma

Word-count topolojisi için **Topoloji özeti** sayfasına geri dönün ve ardından **Topoloji eylemleri** bölümünden **Sonlandır** düğmesini seçin. İstendiğinde, topolojiyi durdurmadan önce beklenecek saniye sayısı için 10 girin. Zaman aşımı süresinden sonra panonun **Storm Kullanıcı Arabirimi** bölümünü ziyaret ettiğinizde topoloji bir daha görünmez.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Hızlı başlatmayı tamamladıktan sonra, kümeyi silmek isteyebilirsiniz. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır.

Bir kümeyi silmek için bkz: [Tarayıcınızı, PowerShell'i veya Azure CLI'yi kullanarak bir HDInsight kümesini sil.](../hdinsight-delete-cluster.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, apache [storm-starter](https://github.com/apache/storm/tree/v2.0.0/examples/storm-starter) projesinden bir örnek kullanarak bir Apache Storm topolojisi oluşturup mevcut bir Apache Storm kümesini izlediniz. Apache Storm topolojilerini yönetme ve izlemenin temellerini öğrenmek için bir sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
>[Azure HDInsight'ta Apache Storm topolojilerini dağıtın ve yönetin](./apache-storm-deploy-monitor-topology-linux.md)