---
title: Apache Hive ile & işlemini analiz edin - Azure HDInsight
description: Azure HDInsight'ta Apache Hive'ı kullanarak JSON belgelerini nasıl kullanacağınızı ve bunları nasıl analiz edeceğinizi öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/06/2020
ms.openlocfilehash: db7c7ae9889d26479f51a7714e7e9fb04b444628
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80757117"
---
# <a name="process-and-analyze-json-documents-by-using-apache-hive-in-azure-hdinsight"></a>Azure HDInsight'ta Apache Hive kullanarak JSON belgelerini işleme ve analiz

Azure HDInsight'ta Apache Hive'ı kullanarak JavaScript Nesne Gösterimi (JSON) dosyalarını nasıl işleyip analiz edeceğinizi öğrenin. Bu makalede, aşağıdaki JSON belgekullanır:

```json
{
  "StudentId": "trgfg-5454-fdfdg-4346",
  "Grade": 7,
  "StudentDetails": [
    {
      "FirstName": "Peggy",
      "LastName": "Williams",
      "YearJoined": 2012
    }
  ],
  "StudentClassCollection": [
    {
      "ClassId": "89084343",
      "ClassParticipation": "Satisfied",
      "ClassParticipationRank": "High",
      "Score": 93,
      "PerformedActivity": false
    },
    {
      "ClassId": "78547522",
      "ClassParticipation": "NotSatisfied",
      "ClassParticipationRank": "None",
      "Score": 74,
      "PerformedActivity": false
    },
    {
      "ClassId": "78675563",
      "ClassParticipation": "Satisfied",
      "ClassParticipationRank": "Low",
      "Score": 83,
      "PerformedActivity": true
    }
  ]
}
```

Dosya ' da `wasb://processjson@hditutorialdata.blob.core.windows.net/`bulunabilir. HDInsight ile Azure Blob depolamasını nasıl kullanacağınız hakkında daha fazla bilgi için, [HDInsight'ta Apache Hadoop ile HDFS uyumlu Azure Blob depolama sını kullanın'](../hdinsight-hadoop-use-blob-storage.md)a bakın. Dosyayı kümenizin varsayılan kapsayıcısına kopyalayabilirsiniz.

Bu makalede, Apache Hive konsolu kullanın. Hive konsolu nasıl açılacağını anlatan talimatlar için, [HDInsight'ta Apache Hadoop ile Apache Ambari Hive View'ı kullanın.](apache-hadoop-use-hive-ambari-view.md)

> [!NOTE]  
> Kovan Görünümü artık HDInsight 4.0'da kullanılamıyor.

## <a name="flatten-json-documents"></a>Flatten JSON belgeleri

Bir sonraki bölümde listelenen yöntemler, JSON belgesinin tek bir satırdan oluşmasını gerektirir. JSON belgesini bir dize düzleştirmek gerekir. JSON belgeniz zaten düzleştirilmişse, bu adımı atlayabilir ve doğrudan JSON verilerini çözümleme yle ilgili bir sonraki bölüme gidebilirsiniz. JSON belgesini düzleştirmek için aşağıdaki komut dosyasını çalıştırın:

```sql
DROP TABLE IF EXISTS StudentsRaw;
CREATE EXTERNAL TABLE StudentsRaw (textcol string) STORED AS TEXTFILE LOCATION "wasb://processjson@hditutorialdata.blob.core.windows.net/";

DROP TABLE IF EXISTS StudentsOneLine;
CREATE EXTERNAL TABLE StudentsOneLine
(
  json_body string
)
STORED AS TEXTFILE LOCATION '/json/students';

INSERT OVERWRITE TABLE StudentsOneLine
SELECT CONCAT_WS(' ',COLLECT_LIST(textcol)) AS singlelineJSON
      FROM (SELECT INPUT__FILE__NAME,BLOCK__OFFSET__INSIDE__FILE, textcol FROM StudentsRaw DISTRIBUTE BY INPUT__FILE__NAME SORT BY BLOCK__OFFSET__INSIDE__FILE) x
      GROUP BY INPUT__FILE__NAME;

SELECT * FROM StudentsOneLine
```

Ham JSON dosyası ' `wasb://processjson@hditutorialdata.blob.core.windows.net/`nda yer alır. **StudentsRaw** Hive tablosu, düzlenmemiş ham JSON belgesine işaret edilir.

**StudentsOneLine** Hive tablosu verileri HDInsight varsayılan dosya sisteminde **/json/students/** yolu altında saklar.

**INSERT** **deyimi, StudentOneLine** tablosunu düzleştirilmiş JSON verileriyle doldurur.

**SELECT** deyimi yalnızca bir satır döndürür.

**SELECT** deyiminin çıktısı aşağıdadır:

![HDInsight JSON belgesini düzleştirmek](./media/using-json-in-hive/hdinsight-flatten-json.png)

## <a name="analyze-json-documents-in-hive"></a>Hive'daki JSON belgelerini analiz edin

Hive, JSON belgelerinde sorguları çalıştırmak için üç farklı mekanizma sağlar veya kendi belgelerinizi yazabilirsiniz:

* kullanıcı tanımlı get_json_object işlevini (UDF) kullanın.
* UDFjson_tuple kullanın.
* Özel Serializer/Deserializer (SerDe) kullanın.
* Python veya diğer dilleri kullanarak kendi UDF yazın. Hive ile kendi Python kodunuzu nasıl çalıştıracağınız hakkında daha fazla bilgi [için, Apache Hive ve Apache Pig ile Python UDF'ye](./python-udf-hdinsight.md)bakın.

### <a name="use-the-get_json_object-udf"></a>UDFget_json_object kullanın

Hive, çalışma sırasında JSON'u sorgulayan [get_json_object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object) adında yerleşik bir UDF sağlar. Bu yöntem iki bağımsız değişken alır: tablo adı ve yöntem adı. Yöntem adı, düzleştirilmiş JSON belgesine ve ayrışdırılması gereken JSON alanına sahiptir. Bu UDF nasıl çalıştığını görmek için bir örnek bakalım.

Aşağıdaki sorgu, her öğrencinin adını ve soyadını döndürür:

```sql
SELECT
  GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
  GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
FROM StudentsOneLine;
```

Konsol penceresinde bu sorguyu çalıştırdığınızda çıktı:

![Apache Hive json nesne UDF alır](./media/using-json-in-hive/hdinsight-get-json-object.png)

UDF'get_json_object sınırlamaları vardır:

* Sorgudaki her alan sorgunun telafisini gerektirdiğinden, performansı etkiler.
* **GET\_JSON_OBJECT()** bir dizinin dize temsilini döndürür. Bu diziyi bir Kovan dizisine dönüştürmek için, "[" ve "]" kare köşeli ayraçlarını değiştirmek için normal ifadeler kullanmanız ve ardından diziyi almak için split'i de aramanız gerekir.

Bu dönüşüm, Hive wiki'nin **json_tuple**kullanmanızı önermesinin nedenidir.  

### <a name="use-the-json_tuple-udf"></a>UDFjson_tuple kullanın

Kovan tarafından sağlanan başka bir UDF [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple)denir , [hangi json _object get_](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object)daha iyi yapar . Bu yöntem anahtarları ve JSON dize bir dizi alır. Sonra bir değer tuple döndürür. Aşağıdaki sorgu json belgesinden öğrenci kimliğini ve notu döndürür:

```sql
SELECT q1.StudentId, q1.Grade
FROM StudentsOneLine jt
LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
  AS StudentId, Grade;
```

Hive konsolunda bu komut dosyasının çıktısı:

![Apache Hive json sorgu sonuçları](./media/using-json-in-hive/hdinsight-json-tuple.png)

UDF, json\_tuple'ın orijinal tablonun her satırına UDT işlevini uygulayarak sanal bir tablo oluşturmasını sağlayan Hive'da [yanal görünüm](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) sözdizimini kullanır. `json_tuple` Karmaşık JSON'lar **LATERAL VIEW'ın**tekrar tekrar kullanımı nedeniyle çok hantal hale gelir. Ayrıca, **JSON_TUPLE** iç içe JSONs idare edemez.

### <a name="use-a-custom-serde"></a>Özel bir SerDe kullanın

SerDe iç içe JSON belgeleri ayrıştırma için en iyi seçimdir. JSON şemasını tanımlamanızı sağlar ve sonra belgeleri ayrışdırmak için şemayı kullanabilirsiniz. Talimatlar için, [Microsoft Azure HDInsight ile özel bir JSON SerDe'nin nasıl kullanılacağına](https://web.archive.org/web/20190217104719/https://blogs.msdn.microsoft.com/bigdatasupport/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight/)bakın.

## <a name="summary"></a>Özet

Hive'da seçtiğiniz JSON işlecinin türü senaryonuza bağlıdır. Basit bir JSON belge ve bakmak için bir alan ile, Hive UDF **get_json_object**seçin. Eğer bakmak için birden fazla anahtar varsa, o zaman **json_tuple**kullanabilirsiniz. İç içe belgeler için **JSON SerDe'yi**kullanın.

## <a name="next-steps"></a>Sonraki adımlar

İlgili makaleler için bkz:

* [Örnek Apache log4j dosyasını analiz etmek için HDInsight'ta Apache Hadoop ile Apache Hive ve HiveQL kullanın](../hdinsight-use-hive.md)
* [HDInsight'ta İnteraktif Sorgu'u kullanarak uçuş gecikme verilerini analiz edin](../interactive-query/interactive-query-tutorial-analyze-flight-data.md)
* [HDInsight'ta Apache Hive kullanarak Twitter verilerini analiz edin](../hdinsight-analyze-twitter-data-linux.md)
