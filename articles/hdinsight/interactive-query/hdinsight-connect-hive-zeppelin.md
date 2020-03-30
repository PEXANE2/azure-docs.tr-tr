---
title: "Hızlı başlangıç: Apache Zeppelin ile Azure HDInsight'ta Apache Hive"
description: Bu hızlı başlangıçta, Apache Hive sorgularını çalıştırmak için Apache Zeppelin'in nasıl kullanılacağını öğrenirsiniz.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.custom: hdinsightactive
ms.date: 12/03/2019
ms.openlocfilehash: 49b576fd511d17616880e5d981fd3f649de797df
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79367933"
---
# <a name="quickstart-execute-apache-hive-queries-in-azure-hdinsight-with-apache-zeppelin"></a>Hızlı başlangıç: Apache Zeppelin ile Azure HDInsight'ta Apache Hive sorgularını yürütün

Bu hızlı başlangıçta, Azure HDInsight'ta [Apache Hive](https://hive.apache.org/) sorgularını çalıştırmak için Apache Zeppelin'i nasıl kullanacağınızı öğrenirsiniz. HDInsight Interactive Query kümeleri, etkileşimli Hive sorgularını çalıştırmak için kullanabileceğiniz [Apache Zeppelin](https://zeppelin.apache.org/) dizüstü bilgisayarlarını içerir.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

HDInsight İnteraktif Sorgu kümesi. Bkz. HDInsight kümesi oluşturmak için [Küme Oluştur'a](../hadoop/apache-hadoop-linux-tutorial-get-started.md) bakın.  **Etkileşimli Sorgu** küme türünü seçtiğinizden emin olun.

## <a name="create-an-apache-zeppelin-note"></a>Apache Zeppelin Notu Oluşturma

1. Aşağıdaki `CLUSTERNAME` URL'de `https://CLUSTERNAME.azurehdinsight.net/zeppelin`kümenizin adı ile değiştirin. Ardından URL'yi bir web tarayıcısı girin.

2. Küme giriş kullanıcı adınızı ve şifrenizi girin. Zeppelin sayfasından yeni bir not oluşturabilir veya varolan notları açabilirsiniz. **HiveSample** bazı örnek Hive sorguları içerir.  

    ![HDInsight İnteraktif Sorgu zeplin](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin.png)

3. **Yeni not oluştur'u**seçin.

4. Yeni **not oluştur** iletişim kutusundan aşağıdaki değerleri yazın veya seçin:

    - Not Adı: Notun adını girin.
    - Varsayılan yorumlayıcı: Açılan listeden **jdbc'yi** seçin.

5. **Not Oluştur'u**seçin.

6. Kod bölümüne aşağıdaki Hive sorgusunu girin ve shift **+ Enter**tuşuna basın:

    ```hive
    %jdbc(hive)
    show tables
    ```

    ![HDInsight İnteraktif Sorgu zeppelin sorgu çalışır](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin-query.png)

    İlk satırdaki **%jdbc(kovan)** deyimi not defterine Hive JDBC yorumlayıcısını kullanmasını söyler.

    Sorgu **kovan sampletable**denilen bir Kovan tablosu döndürecektir.

    Aşağıdaki iki ek Hive sorguları size **kovan sampletable**karşı çalıştırabilirsiniz:

    ```hive
    %jdbc(hive)
    select * from hivesampletable limit 10

    %jdbc(hive)
    select ${group_name}, count(*) as total_count
    from hivesampletable
    group by ${group_name=market,market|deviceplatform|devicemake}
    limit ${total_count=10}
    ```

    Geleneksel Hive ile karşılaştırıldığında, sorgu sonuçları nın daha hızlı geri gelmesi gerekir.

### <a name="additional-examples"></a>Ek örnekler

1. Bir tablo oluşturun. Aşağıdaki kodu Zeppelin Not Defteri'nde uygulayın:

    ```hql
    %jdbc(hive)
    CREATE EXTERNAL TABLE log4jLogs (
        t1 string,
        t2 string,
        t3 string,
        t4 string,
        t5 string,
        t6 string,
        t7 string)
    ROW FORMAT DELIMITED
    FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE;
    ```

1. Verileri yeni tabloya yükleyin. Aşağıdaki kodu Zeppelin Not Defteri'nde uygulayın:

    ```hql
    %jdbc(hive)
    LOAD DATA
    INPATH 'wasbs:///example/data/sample.log'
    INTO TABLE log4jLogs;
    ```

1. Tek bir kayıt ekleyin. Aşağıdaki kodu Zeppelin Not Defteri'nde uygulayın:

    ```hql
    %jdbc(hive)
    INSERT INTO TABLE log4jLogs2
    VALUES ('A', 'B', 'C', 'D', 'E', 'F', 'G');
    ```

Ek sözdizimi için [Hive dil kılavuzunu](https://cwiki.apache.org/confluence/display/Hive/LanguageManual) gözden geçirin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Hızlı başlatmayı tamamladıktan sonra, kümeyi silmek isteyebilirsiniz. HDInsight ile verileriniz Azure Depolama'da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Kullanılmamış olsa bile bir HDInsight kümesi için de ücretlendirilirsiniz. Küme ücretleri depolama ücretlerinden kat kat daha fazla olduğundan, kümeleri kullanılmadıklarında silmek ekonomik açıdan mantıklıdır.

Bir kümeyi silmek için bkz: [Tarayıcınızı, PowerShell'i veya Azure CLI'yi kullanarak bir HDInsight kümesini sil.](../hdinsight-delete-cluster.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure HDInsight'ta Apache Hive sorgularını çalıştırmak için Apache Zeppelin'i kullanmayı öğrendiniz. Hive sorguları hakkında daha fazla bilgi edinmek için, bir sonraki makalede Visual Studio ile sorguları nasıl yürüteceğinizi gösterecektir.

> [!div class="nextstepaction"]
> [Visual Studio için Veri Gölü Araçlarını kullanarak Azure HDInsight'a bağlanın ve Apache Hive sorguları çalıştırın](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)
