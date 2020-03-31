---
title: Apache Phoenix ile Azure HDInsight'ta Apache Base sorgularını çalıştırın
description: Apache Zeppelin'i Phoenix ile Apache Base sorgularını çalıştırmak için nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/14/2019
ms.openlocfilehash: 28eeb446e55213f1ffa0a638878f6432fd15a05a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72392247"
---
# <a name="use-apache-zeppelin-to-run-apache-phoenix-queries-over-apache-hbase-in-azure-hdinsight"></a>Azure HDInsight'ta Apache HBase üzerinden Apache Phoenix sorgularını çalıştırmak için Apache Zeppelin'i kullanın

Apache Phoenix açık kaynak kodlu, kitlesel paralel ilişkisel veritabanı katmanı HBase üzerine inşa edilmiştir. Phoenix HBase üzerinde sorguları gibi SQL kullanmanıza olanak sağlar. Phoenix, SQL tablolarını, dizinleri, görünümleri ve dizileri oluşturmanızı, silmenizi, değiştirmenizi sağlamak için altında JDBC sürücülerini kullanır.  Satırları tek tek ve toplu olarak güncelleştirmek için Phoenix'i de kullanabilirsiniz. Phoenix, sorguları derlemek için MapReduce'ı kullanmak yerine NOSQL yerel derlemesi kullanarak HBase'in üstünde düşük gecikmeli uygulamalar oluşturulmasını sağlar.

Apache Zeppelin, etkileşimli veri analitiği ve SQL ve Scala gibi dilleri kullanarak veri odaklı, işbirlikçi belgeler oluşturmanıza olanak tanıyan açık kaynaklı web tabanlı bir dizüstü bilgisayardır. Veri geliştiricilerin, veri bilimcileri & veri işleme için kod geliştirmelerine, düzenlemelerine, yürütmelerine ve paylaşmamıza yardımcı olur. Komut satırına atıfta bulunmaksızın veya küme ayrıntılarına ihtiyaç duymadan sonuçları görselleştirmenizi sağlar.

HDInsight kullanıcıları Phoenix tablolarını sorgulamak için Apache Zeppelin'i kullanabilir. Apache Zeppelin HDInsight kümesi ile entegre edilmiştir ve kullanmak için ek adım yoktur. JDBC tercümanı ile bir Zeppelin Notebook oluşturmanız ve Phoenix SQL sorgularınızı yazmaya başlamanız yeterlidir

## <a name="prerequisites"></a>Ön koşullar

HDInsight'ta bir Apache HBase kümesi. Bkz. [Apache HBase ile başlayın.](./apache-hbase-tutorial-get-started-linux.md)

## <a name="create-an-apache-zeppelin-note"></a>Apache Zeppelin Notu Oluşturma

1. Aşağıdaki `CLUSTERNAME` URL'de `https://CLUSTERNAME.azurehdinsight.net/zeppelin`kümenizin adı ile değiştirin. Ardından URL'yi bir web tarayıcısı girin. Küme giriş kullanıcı adınızı ve şifrenizi girin.

1. Zeppelin sayfasından **yeni not oluştur'u**seçin.

    ![HDInsight İnteraktif Sorgu zeplin](./media/apache-hbase-phoenix-zeppelin/hbase-zeppelin-create-note.png)

1. Yeni **not oluştur** iletişim kutusundan aşağıdaki değerleri yazın veya seçin:

    - Not Adı: Notun adını girin.
    - Varsayılan yorumlayıcı: Açılan listeden **jdbc'yi** seçin.

    Ardından **Not Oluştur'u**seçin.

1. Not defteri üstbilginin bağlı bir durum gösterdiğinden emin olun. Sağ üst köşedeki yeşil bir noktayla gösterilir.

    ![Zeppelin not defteri durumu](./media/apache-hbase-phoenix-zeppelin/hbase-zeppelin-connected.png "Zeppelin not defteri durumu")

1. Bir HBase tablosu oluşturun. Aşağıdaki komutu girin ve sonra **Shift + Enter**tuşuna basın :

    ```sql
    %jdbc(phoenix)
    CREATE TABLE Company (
        company_id INTEGER PRIMARY KEY,
        name VARCHAR(225)
    );
    ```

    İlk satırdaki **%jdbc(phoenix)** deyimi not defterine Phoenix JDBC yorumlayıcısını kullanmasını söyler.

1. Oluşturulan tabloları görüntüleyin.

    ```sql
    %jdbc(phoenix)
    SELECT DISTINCT table_name
    FROM SYSTEM.CATALOG
    WHERE table_schem is null or table_schem <> 'SYSTEM';
    ```

1. Tabloya değerleri ekleyin.

    ```sql
    %jdbc(phoenix)
    UPSERT INTO dbo.Company VALUES(1, 'Microsoft');
    UPSERT INTO dbo.Company (name, company_id) VALUES('Apache', 2);
    ```

1. Tabloyu sorgula.

    ```sql
    %jdbc(phoenix)
    SELECT * FROM dbo.Company;
    ```

1. Bir kaydı silin.

    ```sql
    %jdbc(phoenix)
    DELETE FROM dbo.Company WHERE COMPANY_ID=1;
    ```

1. Masayı bırak.

    ```sql
    %jdbc(phoenix)
    DROP TABLE dbo.Company;
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Apache Phoenix artık Azure HDInsight'ta Zeppelin'i destekliyor](https://blogs.msdn.microsoft.com/ashish/2018/08/17/apache-phoenix-now-supports-zeppelin-in-azure-hdinsight/)
- [Apache Phoenix dilbilgisi](https://phoenix.apache.org/language/index.html)
