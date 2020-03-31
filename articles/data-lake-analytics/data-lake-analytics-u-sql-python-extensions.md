---
title: Azure Veri Gölü Analizi'nde Python ile U-SQL komut dosyalarını genişletme
description: Azure Veri Gölü Analizi'ni kullanarak U-SQL komut dosyalarında Python kodunu nasıl çalıştırılacaklarla çalıştırılamayı öğrenin
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: c1c74e5e-3e4a-41ab-9e3f-e9085da1d315
ms.topic: conceptual
ms.date: 06/20/2017
ms.openlocfilehash: 0a49cbdb4caf474d0628fea3679ce712d37886e7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60813407"
---
# <a name="extend-u-sql-scripts-with-python-code-in-azure-data-lake-analytics"></a>Azure Veri Gölü Analizi'nde Python koduyla U-SQL komut dosyalarını genişletme

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce Python uzantılarının Azure Veri Gölü Analizi hesabınıza yüklendiğinden emin olun.

* Azure portalında Veri Gölü Analizi Hesabı'nda size gidin
* Sol menüde, **BAŞLARKEN** **Altında Örnek Komut Dosyaları'na** tıklayın
* **U-SQL Uzantıları yükle'yi** tıklatın sonra **Tamam**

## <a name="overview"></a>Genel Bakış 

U-SQL için Python Uzantıları, geliştiricilerin Python kodunun büyük ölçüde paralel yürütülmesini gerçekleştirmesini sağlar. Aşağıdaki örnekte temel adımlar gösteriş verilmiştir:

* U-SQL Komut Dosyası için Python uzantılarını `REFERENCE ASSEMBLY` etkinleştirmek için deyimi kullanın
* Giriş `REDUCE` verilerini bir anahtarüzerinde bölmek için işlemi kullanma
* U-SQL için Python uzantıları, indiriciye atanan`Extension.Python.Reducer`her tepe noktası üzerinde Python kodunu çalıştıran yerleşik bir indirgeci () içerir
* U-SQL komut dosyası, pandadataframe'i `usqlml_main` girdi olarak kabul eden ve bir panda DataFrame'i çıktı olarak döndüren bir işlevi olan gömülü Python kodunu içerir.

--

    REFERENCE ASSEMBLY [ExtPython];

    DECLARE @myScript = @"
    def get_mentions(tweet):
        return ';'.join( ( w[1:] for w in tweet.split() if w[0]=='@' ) )

    def usqlml_main(df):
        del df['time']
        del df['author']
        df['mentions'] = df.tweet.apply(get_mentions)
        del df['tweet']
        return df
    ";

    @t  = 
        SELECT * FROM 
           (VALUES
               ("D1","T1","A1","@foo Hello World @bar"),
               ("D2","T2","A2","@baz Hello World @beer")
           ) AS 
               D( date, time, author, tweet );

    @m  =
        REDUCE @t ON date
        PRODUCE date string, mentions string
        USING new Extension.Python.Reducer(pyScript:@myScript);

    OUTPUT @m
        TO "/tweetmentions.csv"
        USING Outputters.Csv();

## <a name="how-python-integrates-with-u-sql"></a>Python U-SQL ile Nasıl Bütünleşir?

### <a name="datatypes"></a>Veri Tipleri

* U-SQL'den dize ve sayısal sütunlar Pandalar ve U-SQL arasında olduğu gibi dönüştürülür
* U-SQL Null'lar Pandas `NA` değerlerine dönüştürülür ve

### <a name="schemas"></a>Şemalar

* Pandalarda dizin vektörleri U-SQL'de desteklenmez. Python işlevindeki tüm giriş veri çerçeveleri her zaman 0'dan eksi 1 satır sayısına kadar 64 bitlik sayısal bir indekse sahiptir. 
* U-SQL veri kümelerinin yinelenen sütun adları olamaz
* U-SQL veri kümeleri dize olmayan sütun adlarını. 

### <a name="python-versions"></a>Python Sürümleri
Yalnızca Python 3.5.1 (Windows için derlenmiş) desteklenir. 

### <a name="standard-python-modules"></a>Standart Python modülleri
Tüm standart Python modülleri dahildir.

### <a name="additional-python-modules"></a>Ek Python modülleri
Standart Python kitaplıklarının yanı sıra, yaygın olarak kullanılan birkaç python kitaplıkları da dahildir:

    pandas
    numpy
    numexpr

### <a name="exception-messages"></a>Özel Durum Mesajları
Şu anda, Python kodunda bir özel durum genel tepe noktası hatası olarak gösterilmektedir. Gelecekte, U-SQL İş hata iletileri Python özel durum iletisini görüntüler.

### <a name="input-and-output-size-limitations"></a>Giriş ve Çıkış boyutu sınırlamaları
Her tepe noktası, ona atanmış sınırlı miktarda belleğe sahiptir. Şu anda, bu sınır bir AU için 6 GB'dır. Veri Çerçeveleri Python kodundaki bellekte bulunması gerektiğinden, giriş ve çıktının toplam boyutu 6 GB'ı geçemez.

## <a name="see-also"></a>Ayrıca bkz.
* [Microsoft Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md)
* [Azure Veri Gölü Analizi işleri için U-SQL pencere işlevlerini kullanma](data-lake-analytics-use-window-functions.md)
* [Visual Studio Code için Azure Data Lake Araçları’nı kullanma](data-lake-analytics-data-lake-tools-for-vscode.md)
