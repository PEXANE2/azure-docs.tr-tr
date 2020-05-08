---
title: HBase kümesini yeni sürüme geçirme-Azure HDInsight
description: Apache HBase kümelerini Azure HDInsight 'ta daha yeni bir sürüme geçirme.
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/02/2020
ms.openlocfilehash: d7fb7b6b409a4e24be97ee61fc7ba1f0c0a93202
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82792641"
---
# <a name="migrate-an-apache-hbase-cluster-to-a-new-version"></a>Apache HBase kümesini yeni bir sürüme geçirme

Bu makalede, Azure HDInsight 'ta Apache HBase kümenizi daha yeni bir sürüme güncelleştirmek için gereken adımlar açıklanmaktadır.

Yükseltme sırasında kapalı kalma süresi, dakika düzeninde en az olmalıdır. Bu kesinti süresi, tüm bellek içi verileri temizleme, sonra yeni kümedeki hizmetleri yapılandırma ve yeniden başlatma zamanına neden olur. Sonuçlarınız, düğüm sayısına, veri miktarına ve diğer değişkenlere göre değişir.

## <a name="review-apache-hbase-compatibility"></a>Apache HBase uyumluluğunu gözden geçir

Apache HBase 'i yükseltmeden önce kaynak ve hedef kümelerindeki HBase sürümlerinin uyumlu olduğundan emin olun. Daha fazla bilgi için bkz. [HDInsight ile kullanılabilen bileşenler ve sürümler Apache Hadoop](../hdinsight-component-versioning.md).

> [!NOTE]  
> [HBase defterindeki](https://hbase.apache.org/book.html#upgrading)sürüm uyumluluğu matrisini incelemenizi önemle tavsiye ederiz. Tüm önemli uyumsuzluklar, HBase sürümü sürüm notlarında açıklanmalıdır.

Örnek bir sürüm uyumluluğu matrisi aşağıda verilmiştir. Y uyumluluğu gösterir ve N olası bir uyumsuzluğu gösterir:

| Uyumluluk türü | Ana sürüm| İkincil sürüm | Patch |
| --- | --- | --- | --- |
| İstemci-sunucu hat uyumluluğu | N | E | E |
| Sunucu-sunucu uyumluluğu | N | E | E |
| Dosya biçimi uyumluluğu | N | E | E |
| İstemci API 'SI uyumluluğu | N | E | E |
| İstemci ikili uyumluluğu | N | N | E |
| **Sunucu tarafı sınırlı API uyumluluğu** |  |  |  |
| Dengeli | N | E | E |
| Mektedir | N | N | E |
| Gelmesine | N | N | N |
| Bağımlılık uyumluluğu | N | E | E |
| İşletimsel uyumluluk | N | N | E |

## <a name="upgrade-with-same-apache-hbase-major-version"></a>Aynı Apache HBase ana sürümüyle yükselt

Azure HDInsight 'ta Apache HBase kümenizi yükseltmek için aşağıdaki adımları izleyin:

1. HBase uyumluluk matrisinde ve sürüm notlarında gösterildiği gibi uygulamanızın yeni sürümle uyumlu olduğundan emin olun. Uygulamanızı HDInsight ve HBase 'in hedef sürümünü çalıştıran bir kümede test edin.

1. Aynı depolama hesabını kullanarak, ancak farklı bir kapsayıcı adıyla [Yeni bir hedef HDInsight kümesi ayarlayın](../hdinsight-hadoop-provision-linux-clusters.md) :

    ![Aynı depolama hesabını kullanın, ancak farklı bir kapsayıcı oluşturun](./media/apache-hbase-migrate-new-version/same-storage-different-container.png)

1. Yükseltmekte olduğunuz küme olan kaynak HBase kümenizi temizler. HBase, gelen verileri _memstore_olarak adlandırılan bir bellek içi depoya yazar. Memstore belirli bir boyuta ulaştığında, HBase onu kümenin depolama hesabındaki uzun vadeli depolama için diske boşaltır. Eski küme silinirken, memmağazaların geri dönüştürülmesi, muhtemelen verilerin kaybedilmesi. Her tablo için memstore 'yi diske el ile temizlemek için aşağıdaki betiği çalıştırın. Bu betiğin en son sürümü Azure [GitHub](https://raw.githubusercontent.com/Azure/hbase-utils/master/scripts/flush_all_tables.sh)' dır.

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

1. Eski HBase kümesine alımı durdurun.

1. Memstore içindeki son verilerin temizlendiğinden emin olmak için önceki betiği yeniden çalıştırın.

1. Eski kümede (`https://OLDCLUSTERNAME.azurehdidnsight.net`) [Apache ambarı](https://ambari.apache.org/) 'Nda oturum açın ve HBase hizmetlerini durdurun. Hizmetleri durdurmak istediğinizi onaylamanız istendiğinde, HBase için bakım modunu açmak üzere kutuyu işaretleyin. Ambarı 'na bağlanma ve kullanma hakkında daha fazla bilgi için bkz. [ambarı Web Kullanıcı arabirimini kullanarak HDInsight kümelerini yönetme](../hdinsight-hadoop-manage-ambari.md).

    ![Ambarı ' nda hizmetler > HBase > Durdur ' a tıklayın.](./media/apache-hbase-migrate-new-version/stop-hbase-services1.png)

    ![HBase için bakım modunu aç onay kutusunu işaretleyin ve ardından onaylayın](./media/apache-hbase-migrate-new-version/turn-on-maintenance-mode.png)

1. Yeni HDInsight kümesinde ambarı 'nda oturum açın. `fs.defaultFS` Bu ayarı, özgün küme tarafından kullanılan kapsayıcı adını gösterecek şekilde değiştirin. Bu ayar, Gelişmiş **> Gelişmiş çekirdek sitesi > > config**'ler altında.

    ![Ambarı 'nda hizmetler >, gelişmiş > > config 'ler ' e tıklayın](./media/apache-hbase-migrate-new-version/hdfs-advanced-settings.png)

    ![Ambarı 'nda kapsayıcı adını değiştirin](./media/apache-hbase-migrate-new-version/change-container-name.png)

1. Gelişmiş yazma özelliği ile HBase kümeleri kullanmıyorsanız, bu adımı atlayın. Yalnızca gelişmiş yazma özelliği olan HBase kümeleri için gereklidir.

   `hbase.rootdir` Yolu orijinal kümenin kapsayıcısına işaret etmek üzere değiştirin.

    ![Ambarı 'nda, HBase rootdir için kapsayıcı adını değiştirin](./media/apache-hbase-migrate-new-version/change-container-name-for-hbase-rootdir.png)

1. HDInsight 3,6 ' i 4,0 sürümüne yükseltiyorsanız aşağıdaki adımları izleyin, aksi halde adım 10 ' a atlayın:
    1. **Hizmetler** > **yeniden başlatma gerekli**' i seçerek, tüm gerekli hizmetleri yeniden başlatın.
    1. HBase hizmetini durdurun.
    1. Zookeeper düğümüne SSH yazın ve [Zkclı](https://github.com/go-zkcli/zkcli) komutunu `rmr /hbase-unsecure` yürütün. Bu, HBase kök znode değerini Zookeeper 'dan kaldırın.
    1. HBase 'i yeniden başlatın.

1. 4,0 dışında başka bir HDInsight sürümüne yükseltiyorsanız şu adımları izleyin:
    1. Yaptığınız değişiklikleri kaydedin.
    1. Ambarı tarafından belirtilen tüm gerekli hizmetleri yeniden başlatın.

1. Uygulamanızı yeni kümeye işaret edin.

    > [!NOTE]  
    > Yükseltme sırasında uygulamanıza yönelik statik DNS değişir. Bu DNS 'in sabit kodlanması yerine, etki alanı adınızın DNS ayarlarında kümenin adına işaret eden bir CNAME yapılandırabilirsiniz. Başka bir seçenek de, uygulamanız için yeniden dağıtım olmadan güncelleştirebilmeniz gereken bir yapılandırma dosyası kullanmaktır.

1. Her şeyin beklendiği gibi çalışıp çalışmadığını görmek için alımı başlatın.

1. Yeni küme tatmin edici ise, özgün kümeyi silin.

## <a name="next-steps"></a>Sonraki adımlar

[Apache HBase](https://hbase.apache.org/) hakkında daha fazla bilgi edinmek ve HDInsight kümelerini yükseltmek için aşağıdaki makalelere bakın:

* [HDInsight kümesini daha yeni bir sürüme yükseltme](../hdinsight-upgrade-cluster.md)
* [Apache ambarı Web Kullanıcı arabirimini kullanarak Azure HDInsight 'ı izleme ve yönetme](../hdinsight-hadoop-manage-ambari.md)
* [Bileşenler ve sürümler Apache Hadoop](../hdinsight-component-versioning.md)
* [Apache HBase 'i iyileştirme](../optimize-hbase-ambari.md)
