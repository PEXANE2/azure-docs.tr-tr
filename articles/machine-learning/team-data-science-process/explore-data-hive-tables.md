---
title: Hive sorguları ile Hive tablolarındaki verileri araştırma-Team Data Science Process
description: HDInsight Hadoop kümesindeki Hive tablolarındaki verileri araştırmak için kullanılan örnek Hive betikleri kullanın.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76722178"
---
# <a name="explore-data-in-hive-tables-with-hive-queries"></a>Hive sorguları ile Hive tablosundaki verileri keşfedin

Bu makalede, bir HDInsight Hadoop kümesindeki Hive tablolarındaki verileri araştırmak için kullanılan örnek Hive betikleri sağlanmıştır.

Bu görev, [ekip veri bilimi işlemindeki](overview.md)bir adımdır.

## <a name="prerequisites"></a>Ön koşullar
Bu makalede sahip olduğunuz varsayılır:

* Bir Azure depolama hesabı oluşturuldu. Yönergelere ihtiyacınız varsa bkz. [Azure depolama hesabı oluşturma](../../storage/common/storage-account-create.md)
* HDInsight hizmeti ile özelleştirilmiş bir Hadoop kümesi sağlandı. Yönergelere ihtiyacınız varsa bkz. [Gelişmiş analiz için Azure HDInsight Hadoop kümelerini özelleştirme](customize-hadoop-cluster.md).
* Veriler Azure HDInsight Hadoop kümelerinde Hive tablolarına yüklendi. Bu yoksa, önce verileri Hive tablolarına yüklemek üzere [Hive tablolarına veri oluşturma ve yükleme](move-hive-tables.md) konusundaki yönergeleri izleyin.
* Kümeye uzaktan erişim etkinleştirildi. Yönergelere ihtiyacınız varsa bkz. [Hadoop kümesinin baş düğümüne erişme](customize-hadoop-cluster.md).
* Hive sorgularının nasıl gönderileceği hakkında yönergeler gerekiyorsa bkz. [Hive sorguları gönderme](move-hive-tables.md#submit)

## <a name="example-hive-query-scripts-for-data-exploration"></a>Veri araştırması için örnek Hive sorgu betikleri
1. Bölüm başına gözlemlerin sayısını Al`SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`
2. Gün başına gözlemlerin sayısını Al`SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`
3. Kategorik bir sütundaki düzeyleri al  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`
4. İki kategorik sütunun birleşimi içindeki düzeylerin sayısını Al`SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`
5. Sayısal sütunlara yönelik dağıtımı al  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`
6. İki tablonun birleştirilmesinden kayıtları Ayıkla
   
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

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a>Taxı seyahat verileri senaryoları için ek sorgu betikleri
[NYC TAXI seyahat veri](https://chriswhong.com/open-data/foil_nyc_taxi/) senaryolarına özgü sorgu örnekleri de [GitHub deposunda](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts)sunulmaktadır. Bu sorguların veri şeması zaten belirtilmiş ve çalıştırılmaya gönderilmeye hazırlanıyor.

