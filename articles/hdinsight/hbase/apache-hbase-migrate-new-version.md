---
title: Bir HBase kümesini yeni bir sürüme geçirin - Azure HDInsight
description: Apache HBase kümelerini Azure HDInsight'ta daha yeni bir sürüme nasıl geçirilir?
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/02/2020
ms.openlocfilehash: 30cda7a83feddaeb41385252a61d1dc68a881a47
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75646515"
---
# <a name="migrate-an-apache-hbase-cluster-to-a-new-version"></a>Bir Apache HBase kümesini yeni bir sürüme geçirme

Bu makalede, Azure HDInsight'taki Apache HBase kümenizi daha yeni bir sürümle güncelleştirmek için gereken adımlar açıklanmaktadır.

Yükseltme sırasında kesinti dakika sırasına göre en az olmalıdır. Bu kapalı kalma süresi, tüm bellek içi verileri sifonu çekme adımlarını, ardından yeni kümedeki hizmetleri yapılandırma ve yeniden başlatma süresinden kaynaklanır. Sonuçlarınız düğüm sayısına, veri miktarına ve diğer değişkenlere bağlı olarak değişir.

## <a name="review-apache-hbase-compatibility"></a>Apache HBase uyumluluğunu gözden geçirin

Apache HBase'i yükseltmeden önce kaynak ve hedef kümeler üzerindeki HBase sürümlerinin uyumlu olduğundan emin olun. Daha fazla bilgi için, [HDInsight ile kullanılabilen Apache Hadoop bileşenleri ve sürümlerine](../hdinsight-component-versioning.md)bakın.

> [!NOTE]  
> [HBase kitabındaki](https://hbase.apache.org/book.html#upgrading)sürüm uyumluluk matrisini incelemenizi şiddetle öneririz. Herhangi bir kırılma uyumsuzlukları HBase sürüm notları açıklanmalıdır.

Burada bir örnek sürüm uyumluluk matrisi verilmiştir. Y uyumluluğu gösterir ve N olası bir uyumsuzluğu gösterir:

| Uyumluluk türü | Ana sürüm| Küçük sürüm | Patch |
| --- | --- | --- | --- |
| İstemci-Sunucu tel uyumluluğu | N | E | E |
| Sunucu-Sunucu uyumluluğu | N | E | E |
| Dosya biçimi uyumluluğu | N | E | E |
| İstemci API uyumluluğu | N | E | E |
| İstemci ikili uyumluluğu | N | N | E |
| **Sunucu tarafı sınırlı API uyumluluğu** |  |  |  |
| Dengeli | N | E | E |
| Gelişen | N | N | E |
| Kararsız | N | N | N |
| Bağımlılık uyumluluğu | N | E | E |
| Operasyonel uyumluluk | N | N | E |

## <a name="upgrade-with-same-apache-hbase-major-version"></a>Aynı Apache HBase ana sürümüyle yükseltme

Azure HDInsight'ta Apache HBase kümenizi yükseltmek için aşağıdaki adımları tamamlayın:

1. Uygulamanızın HBase uyumluluk matrisi ve sürüm notlarında gösterildiği gibi yeni sürümle uyumlu olduğundan emin olun. UYGULAMANIZI HDInsight ve HBase'in hedef sürümünü çalıştıran bir kümede test edin.

1. Aynı depolama hesabını kullanarak, ancak farklı bir kapsayıcı adı ile [yeni bir hedef HDInsight kümesi ayarlayın:](../hdinsight-hadoop-provision-linux-clusters.md)

    ![Aynı Depolama hesabını kullanın, ancak farklı bir Kapsayıcı oluşturun](./media/apache-hbase-migrate-new-version/same-storage-different-container.png)

1. Yükselttiğiniz küme olan kaynak HBase kümenizi temize çıkar. HBase gelen verileri bir bellek deposuna yazar, _memstore_olarak adlandırılır. Memstore belirli bir boyuta ulaştıktan sonra, HBase kümenin depolama hesabında uzun süreli depolama için diske yıkayın. Eski küme silirken, memstores geri dönüştürülür ve potansiyel olarak veri kaybeder. Her tablo için memstore'u el ile diske yıkamak için aşağıdaki komut dosyasını çalıştırın. Bu komut dosyasının en son [GitHub](https://raw.githubusercontent.com/Azure/hbase-utils/master/scripts/flush_all_tables.sh)sürümü Azure'un GitHub'ındadır.

    ```bash
    #!/bin/bash
    
    #-------------------------------------------------------------------------------#
    # SCRIPT TO FLUSH ALL HBASE TABLES.
    #-------------------------------------------------------------------------------#
    
    LIST_OF_TABLES=/tmp/tables.txt
    HBASE_SCRIPT=/tmp/hbase_script.txt
    TARGET_HOST=$1
    
    usage ()
    {
        if [[ "$1" == "-h" ]] || [[ "$1" == "--help" ]]
        then
            cat << ...
    
    Usage: 
    
    $0 [hostname]
    
    Providing hostname is optional and not required when the script is executed within HDInsight cluster with access to 'hbase shell'.
    
    However hostname should be provided when executing the script as a script-action from HDInsight portal.
    
    For Example:
    
        1.  Executing script inside HDInsight cluster (where 'hbase shell' is 
            accessible):
    
            $0 
    
            [No need to provide hostname]
    
        2.  Executing script from HDinsight Azure portal:
    
            Provide Script URL.
    
            Provide hostname as a parameter (i.e. hn0, hn1, hn2.. or wn2 etc.).
    ...
            exit
        fi
    }
    
    validate_machine ()
    {
        THIS_HOST=`hostname`
    
        if [[ ! -z "$TARGET_HOST" ]] && [[ $THIS_HOST  != $TARGET_HOST* ]]
        then
            echo "[INFO] This machine '$THIS_HOST' is not the right machine ($TARGET_HOST) to execute the script."
            exit 0
        fi
    }
    
    get_tables_list ()
    {
    hbase shell << ... > $LIST_OF_TABLES 2> /dev/null
        list
        exit
    ...
    }
    
    add_table_for_flush ()
    {
        TABLE_NAME=$1
        echo "[INFO] Adding table '$TABLE_NAME' to flush list..."
        cat << ... >> $HBASE_SCRIPT
            flush '$TABLE_NAME'
    ...
    }
    
    clean_up ()
    {
        rm -f $LIST_OF_TABLES
        rm -f $HBASE_SCRIPT
    }
    
    ########
    # MAIN #
    ########
    
    usage $1
    
    validate_machine
    
    clean_up
    
    get_tables_list
    
    START=false
    
    while read LINE 
    do 
        if [[ $LINE == TABLE ]] 
        then
            START=true
            continue
        elif [[ $LINE == *row*in*seconds ]]
        then
            break
        elif [[ $START == true ]]
        then
            add_table_for_flush $LINE
        fi
    
    done < $LIST_OF_TABLES
    
    cat $HBASE_SCRIPT
    
    hbase shell $HBASE_SCRIPT << ... 2> /dev/null
    exit
    ...
    
    ```

1. Eski HBase kümesine alımını durdurun.

1. Memstore'daki son verilerin temizlenmesini sağlamak için önceki komut dosyasını yeniden çalıştırın.

1. Eski kümedeki`https://OLDCLUSTERNAME.azurehdidnsight.net` [Apache Ambari'ye](https://ambari.apache.org/) oturum açın ve HBase hizmetlerini durdurun. Hizmetleri durdurmak istediğinizi onaylamanız istendiğinde, HBase için bakım modunu açmak için kutuyu işaretleyin. Ambari'ye bağlanma ve kullanma hakkında daha fazla bilgi için [Ambari Web Kullanıcı Arabirimi'ni kullanarak HDInsight kümelerini yönet'e](../hdinsight-hadoop-manage-ambari.md)bakın.

    ![Ambari'de, Hizmet Eylemleri Altında HBase > Stop > Hizmetler'i tıklatın](./media/apache-hbase-migrate-new-version/stop-hbase-services1.png)

    ![HBase onay kutusu için Bakım Açma Modunu kontrol edin ve onaylayın](./media/apache-hbase-migrate-new-version/turn-on-maintenance-mode.png)

1. Yeni HDInsight kümesinde Ambari'de oturum açın. HDFS ayarını `fs.defaultFS` özgün küme tarafından kullanılan kapsayıcı adını işaret etmek için değiştirin. Bu ayar **HDFS > Configs > Advanced > Advanced core-site**altındadır.

    ![Ambari'de, Hizmetler > HDFS > Configs > Advanced'e tıklayın](./media/apache-hbase-migrate-new-version/hdfs-advanced-settings.png)

    ![Ambari'de konteyner adını değiştirin](./media/apache-hbase-migrate-new-version/change-container-name.png)

1. Gelişmiş Yazma özelliğine sahip HBase kümelerini kullanmıyorsanız, bu adımı atlayın. Yalnızca Gelişmiş Yazma özelliğine sahip HBase kümeleri için gereklidir.

   Özgün `hbase.rootdir` kümenin kapsayıcısına işaret etmek için yolu değiştirin.

    ![Ambari'de HBase rootdir için konteyner adını değiştirin](./media/apache-hbase-migrate-new-version/change-container-name-for-hbase-rootdir.png)

1. HDInsight 3,6'dan 4,0'a yükseltiyorsanız, aşağıdaki adımları izleyin, aksi takdirde adım 10'a geçin:
    1. Ambari'de gerekli tüm hizmetleri **Services** > Hizmetler Gerekli Tüm Ünü**Yeniden Başlat'ı**seçerek yeniden başlatın.
    1. HBase hizmetini durdurun.
    1. Zookeeper düğümüne SSH ve Zookeeper HBase `rmr /hbase-unsecure` kök znode kaldırmak için [zkCli](https://github.com/go-zkcli/zkcli) komutunu yürütmek.
    1. HBase'i yeniden başlatın.

1. 4.0 dışında başka bir HDInsight sürümüne yükseltiyorsanız aşağıdaki adımları izleyin:
    1. Yaptığınız değişiklikleri kaydedin.
    1. Ambari tarafından belirtildiği gibi gerekli tüm hizmetleri yeniden başlatın.

1. Uygulamanızı yeni kümeye yönlendirin.

    > [!NOTE]  
    > Yükseltme yaparken uygulamanızın statik DNS'si değişir. Bu DNS'yi zor kodlamak yerine, etki alanı adınızın DNS ayarlarında kümenin adını gösteren bir CNAME yapılandırabilirsiniz. Başka bir seçenek, uygulamanız için yeniden dağıtmadan güncelleştirebileceğiniz bir yapılandırma dosyası kullanmaktır.

1. Her şeyin beklendiği gibi çalışıp çalışmamasını görmek için yutma işlemine başlayın.

1. Yeni küme tatmin ediciyse, özgün kümeyi silin.

## <a name="next-steps"></a>Sonraki adımlar

[Apache HBase](https://hbase.apache.org/) ve HDInsight kümelerini yükseltme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [HDInsight kümesini daha yeni bir sürüme yükseltme](../hdinsight-upgrade-cluster.md)
* [Apache Ambari Web Kullanıcı Arabirimi'ni kullanarak Azure HDInsight'ı izleyin ve yönetin](../hdinsight-hadoop-manage-ambari.md)
* [Apache Hadoop bileşenleri ve versiyonları](../hdinsight-component-versioning.md)
* [Apache Ambari kullanarak yapılandırmaları optimize edin](../hdinsight-changing-configs-via-ambari.md#apache-hbase-optimization-with-the-ambari-web-ui)
