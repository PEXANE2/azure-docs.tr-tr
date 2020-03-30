---
title: Hive sorguları ile Kovan tablolarında verileri keşfedin - Takım Veri Bilimi Süreci
description: BIR HDInsight Hadoop kümesindeki Hive tablolarında verileri keşfetmek için kullanılan örnek Hive komut dosyalarını kullanın.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 75dce2b5a83d13fe4a7d166595456e9a8d6324ba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76722178"
---
# <a name="explore-data-in-hive-tables-with-hive-queries"></a>Hive sorguları ile Hive tablosundaki verileri keşfedin

Bu makalede, bir HDInsight Hadoop kümesindeki Hive tablolarında verileri keşfetmek için kullanılan örnek Hive komut dosyaları sağlanmaktadır.

Bu [görev, Ekip Veri Bilimi Sürecinde](overview.md)bir adımdır.

## <a name="prerequisites"></a>Ön koşullar
Bu makalede, sahip olduğunuzu varsayar:

* Bir Azure depolama hesabı oluşturdu. Yönergelere ihtiyacınız [varsa,](../../storage/common/storage-account-create.md) bkz.
* HDInsight hizmeti ile özelleştirilmiş bir Hadoop kümesi sağlanmış. Talimatlara ihtiyacınız varsa, [Gelişmiş Analitik için Azure HDInsight Hadoop Kümelerini Özelleştir'e](customize-hadoop-cluster.md)bakın.
* Veriler Azure HDInsight Hadoop kümelerinde Hive tablolarına yüklendi. Yoksa, Önce Kovan tablolarına veri yüklemek için [Veri Oluştur ve Kovan tablolarına veri yükleyin](move-hive-tables.md) yönergeleri izleyin.
* Kümeye uzaktan erişim etkin. Talimatlara ihtiyacınız varsa, [Hadoop Kümesi'nin Baş Düğümüne Eriş'e](customize-hadoop-cluster.md)bakın.
* Hive sorgularını nasıl göndereceğinize ilişkin talimatlara ihtiyacınız varsa, [Kovan Sorgularını Nasıl Gönderirsiniz](move-hive-tables.md#submit)

## <a name="example-hive-query-scripts-for-data-exploration"></a>Veri arama için örnek Hive sorgu komut dosyaları
1. Bölüm başına gözlem sayısını alın`SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`
2. Günlük gözlem sayısını alın`SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`
3. Düzeyleri kategorik bir sütunda alma  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`
4. İki kategorik sütunun birleşimiyle düzey sayısını alma`SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`
5. Sayısal sütunlar için dağıtımı alın  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`
6. İki tabloya katılmadan kayıtları ayıklama
   
        SELECT
            a.<common_columnname1> as <new_name1>,
            a.<common_columnname2> as <new_name2>,
            a.<a_column_name1> as <new_name3>,
            a.<a_column_name2> as <new_name4>,
            b.<b_column_name1> as <new_name5>,
            b.<b_column_name2> as <new_name6>
        FROM
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <a_column_name1>,
                <a_column_name2>,
            FROM <databasename>.<tablename1>
            ) a
            join
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <b_column_name1>,
                <b_column_name2>,
            FROM <databasename>.<tablename2>
            ) b
            ON a.<common_columnname1>=b.<common_columnname1> and a.<common_columnname2>=b.<common_columnname2>

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a>Taksi yolculuğu veri senaryoları için ek sorgu komut dosyaları
[NYC Taksi Gezisi Veri](https://chriswhong.com/open-data/foil_nyc_taxi/) senaryolarına özgü sorgu örnekleri [github deposunda](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts)da verilmektedir. Bu sorgular zaten veri şeması belirtilen ve çalıştırmak için gönderilmeye hazırdır.

