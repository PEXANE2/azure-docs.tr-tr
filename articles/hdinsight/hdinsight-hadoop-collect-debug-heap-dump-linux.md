---
title: HDInsight'ta Apache Hadoop hizmetleri için yığın dökümlerini etkinleştirin - Azure
description: Hata ayıklama ve analiz için Linux tabanlı HDInsight kümelerinden Apache Hadoop hizmetleri için yığın dökümlerini etkinleştirin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/02/2020
ms.openlocfilehash: 9134eb6922b0ed37bbe6051b138da2c7c082b175
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75658806"
---
# <a name="enable-heap-dumps-for-apache-hadoop-services-on-linux-based-hdinsight"></a>Linux tabanlı HDInsight'ta Apache Hadoop hizmetleri için yığın dökümlerini etkinleştirin

[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Yığın dökümleri, dökümün oluşturulduğu anda değişkenlerin değerleri de dahil olmak üzere uygulamanın belleğinin anlık görüntüsünü içerir. Bu nedenle, çalışma zamanında ortaya çıkan sorunları tanılamada yararlıdırlar.

## <a name="services"></a>Hizmetler

Aşağıdaki hizmetler için yığın dökümlerini etkinleştirebilirsiniz:

* **Apaçi hcatalog** - tempelton
* **Apache kovan** - kovanserver2, metastore, derbyserver
* **mapreduce** - iş geçmişisunucusu
* **Apache iplik** - resourcemanager, düğümyöneticisi, zaman çizelgesi sunucusu
* **Apache hdfs** - datanode, ikincil namenod, namenode

Ayrıca harita için yığın dökümleri etkinleştirebilir ve HDInsight tarafından çalıştırılen işlemleri azaltabilirsiniz.

## <a name="understanding-heap-dump-configuration"></a>Yığın dökümü yapılandırması anlama

Yığın dökümleri, bir hizmet başlatıldığında JVM'ye seçenekleri (bazen tercih ler veya parametreler olarak da bilinir) geçirerek etkinleştirilir. Çoğu [Apache Hadoop](https://hadoop.apache.org/) hizmeti için, bu seçenekleri geçirmek için hizmeti başlatmak için kullanılan kabuk komut dosyasını değiştirebilirsiniz.

Her komut dosyasında, JVM'ye geçirilen seçenekleri içeren ** \* \_OPTS**için bir dışa aktarma vardır. Örneğin, **hadoop-env.sh** komut dosyasında, ile `export HADOOP_NAMENODE_OPTS=` başlayan satır NameNode hizmeti için seçenekleri içerir.

Bu işlemler MapReduce hizmetinin alt işlemi olduğundan, harita ve azaltma işlemleri biraz farklıdır. Her harita veya azaltma işlemi bir alt kapsayıcıda çalışır ve JVM seçeneklerini içeren iki giriş vardır. Her ikisi de **mapred-site.xml**bulunan:

* **mapreduce.admin.map.child.java.opts**
* **mapreduce.admin.reduce.child.java.opts**

> [!NOTE]  
> Ambari kümedeki düğümler arasında çoğaltma değişikliklerini işlerken, hem komut dosyalarını hem de mapred-site.xml ayarlarını değiştirmek için [Apache Ambari'yi](https://ambari.apache.org/) kullanmanızı öneririz. Belirli adımlar için [Apache Ambari'yi kullanma](#using-apache-ambari) bölümüne bakın.

### <a name="enable-heap-dumps"></a>Yığın dökümlerini etkinleştirme

OutOfMemoryError oluştuğunda aşağıdaki seçenek yığın dökümleri sağlar:

    -XX:+HeapDumpOnOutOfMemoryError

Bu **+** seçeneğin etkin olduğunu gösterir. Varsayılan olarak devre dışı seçeneği kullanılır.

> [!WARNING]  
> Yığın dökümleri, döküm dosyaları büyük olabileceğinden, varsayılan olarak HDInsight'taki Hadoop hizmetleri için etkinleştirilmez. Sorun giderme için bunları etkinleştiriseniz, sorunu yeniden ve döküm dosyalarını topladıktan sonra bunları devre dışı kaldırmayı unutmayın.

### <a name="dump-location"></a>Döküm konumu

Döküm dosyasının varsayılan konumu geçerli çalışma dizinidir. Aşağıdaki seçeneği kullanarak dosyanın nerede depolanır denetleyebilirsiniz:

    -XX:HeapDumpPath=/path

Örneğin, dökümlerin /tmp dizininde depolanmasını kullanmak. `-XX:HeapDumpPath=/tmp`

### <a name="scripts"></a>Betikler

**OutOfMemoryError** oluştuğunda bir komut dosyası da tetikleyebilirsiniz. Örneğin, hatanın oluştuğunu bilmek için bir bildirimi tetikleme. __OutOfMemoryError'ta__bir komut dosyasını tetiklemek için aşağıdaki seçeneği kullanın:

    -XX:OnOutOfMemoryError=/path/to/script

> [!NOTE]  
> Apache Hadoop dağıtılmış bir sistem olduğundan, kullanılan tüm komut dosyası hizmetin çalıştığı kümedeki tüm düğümlere yerleştirilmelidir.
> 
> Komut dosyası, hizmetin çalıştığı hesap tarafından erişilebilen bir konumda da olmalı ve yürütme izinleri sağlamalı. Örneğin, komut dosyalarını depolamak `/usr/local/bin` ve `chmod go+rx /usr/local/bin/filename.sh` okuma ve yürütme izinleri vermek için kullanmak isteyebilirsiniz.

## <a name="using-apache-ambari"></a>Apache Ambari kullanma

Bir hizmet için yapılandırmayı değiştirmek için aşağıdaki adımları kullanın:

1. Bir web tarayıcısından, `https://CLUSTERNAME.azurehdinsight.net`kümenizin adı nerede' `CLUSTERNAME` ye gidin.

2. Soldaki listeyi kullanarak, değiştirmek istediğiniz hizmet alanını seçin. Örneğin, **HDFS**. Orta alanda **Configs** sekmesini seçin.

    ![Seçilen HDFS Configs sekmesi ile Ambari web görüntüsü](./media/hdinsight-hadoop-collect-debug-heap-dump-linux/hdi-service-config-tab.png)

3. **Filtreyi kullanarak...** girişini, **tercihleri**girin. Yalnızca bu metni içeren öğeler görüntülenir.

    ![Apache Ambari config filtreli liste](./media/hdinsight-hadoop-collect-debug-heap-dump-linux/hdinsight-filter-list.png)

4. Yığın dökümlerini etkinleştirmek istediğiniz hizmet için ** \_OPTS girişini bulun ve etkinleştirmek istediğiniz seçenekleri ekleyin. \*** Aşağıdaki resimde, **HADOOP\_NAMENODE\_OPTS** girişine ekledim: `-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/`

    ![Apache Ambari hadoop-namenode-opts](./media/hdinsight-hadoop-collect-debug-heap-dump-linux/hadoop-namenode-opts.png)

   > [!NOTE]  
   > Harita için yığın dökümlerini etkinleştirirken veya alt işlemi azaltırken **mapreduce.admin.map.child.java.opts** ve **mapreduce.admin.reduce.child.java.opts**adlı alanları arayın.

    Değişiklikleri kaydetmek için **Kaydet** düğmesini kullanın. Değişiklikleri açıklayan kısa bir not girebilirsiniz.

5. Değişiklikler uygulandıktan sonra, **gerekli yeniden başlatma** simgesi bir veya daha fazla hizmetin yanında görünür.

    ![gerekli simgeyi yeniden başlatın ve düğmeyi yeniden başlatın](./media/hdinsight-hadoop-collect-debug-heap-dump-linux/restart-required-icon.png)

6. Yeniden başlatması gereken her hizmeti seçin ve **Bakım Modunu Açmak**için Servis **Eylemleri** düğmesini kullanın. Bakım modu, yeniden başlattığınızda uyarılardan uyarıların üretilebini önler.

    ![HDI bakım modu menüsünü açın](./media/hdinsight-hadoop-collect-debug-heap-dump-linux/hdi-maintenance-mode.png)

7. Bakım modunu etkinleştirdikten sonra, **Tüm Efektli Tüm'ün** Yeniden Başlatılması için hizmetiçin **Yeniden Başlat** düğmesini kullanın

    ![Apache Ambari Etkilenen Tüm Girişi Yeniden Başlat](./media/hdinsight-hadoop-collect-debug-heap-dump-linux/hdi-restart-all-button.png)

   > [!NOTE]  
   > **Yeniden Başlat** düğmesi için girişler diğer hizmetler için farklı olabilir.

8. Hizmetler yeniden başlatıldıktan **sonra, Bakım Modunu Kapatmak**için **Servis Eylemleri** düğmesini kullanın. Bu Ambari hizmet için uyarılar için izleme devam etmek.