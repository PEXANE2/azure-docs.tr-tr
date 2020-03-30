---
title: Azure Akış Analizi'nde JavaScript kullanıcı tanımlı toplamlar
description: Bu makalede, Azure Akış Analizi'nde JavaScript kullanıcı tanımlı agregalarla gelişmiş sorgu mekaniğinin nasıl gerçekleştirilecekiyi açıklanmaktadır.
author: rodrigoaatmicrosoft
ms.author: rodrigoa
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 10/28/2017
ms.openlocfilehash: c509d174787a58abeee33e039eb7bbbcbcb43f38
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79531743"
---
# <a name="azure-stream-analytics-javascript-user-defined-aggregates"></a>Azure Akış Analizi JavaScript kullanıcı tanımlı toplamları
 
Azure Akış Analizi, JavaScript'te yazılmış kullanıcı tanımlı toplamları (UDA) destekler, karmaşık durum sallanmasına olanak tanır. UDA içinde durum veri yapısı, durum birikimi, durum decumulation ve toplam sonuç hesaplama tam kontrole sahip. Makalede, iki farklı JavaScript UDA arabirimi, uda oluşturma adımları ve Stream Analytics sorgusunda pencere tabanlı işlemlerle UDA'nın nasıl kullanılacağı tanıtıştır.

## <a name="javascript-user-defined-aggregates"></a>JavaScript kullanıcı tanımlı agregalar

Kullanıcı tanımlı bir agrega, o penceredeki olayları toplamak ve tek bir sonuç değeri üretmek için bir zaman penceresi belirtiminin üstünde kullanılır. Stream Analytics'in bugün desteklediği iki tür UDA arabirimi vardır, BirikimYalnızca ve BirikirDebiri. UDA her iki tür yuvarlanma, atlamalı, sürgülü ve Oturum Penceresi tarafından kullanılabilir. AccumulateDeaccumulate UDA Atlamalı, Sürgülü ve Oturum Penceresi ile birlikte kullanıldığında Sadece UDA'dan daha iyi performans gösterir. Kullandığınız algoritmayı temel alan iki türden birini seçersiniz.

### <a name="accumulateonly-aggregates"></a>Biriken Sadece agregalar

AccumulateOnly toplamları sadece durumuna yeni olaylar birikebilir, algoritma değerlerin debirimi izin vermez. Durum değerinden bir olay bilgisi biriktirmek uygulamak mümkün değildir bu toplam türünü seçin. Aşağıdaki AccumulatOnly toplamları için JavaScript şablonu:

```JavaScript
// Sample UDA which state can only be accumulated.
function main() {
    this.init = function () {
        this.state = 0;
    }

    this.accumulate = function (value, timestamp) {
        this.state += value;
    }

    this.computeResult = function () {
        return this.state;
    }
}
```

### <a name="accumulatedeaccumulate-aggregates"></a>BirikirDebirik agregalar

Birikmiş Debirik agregalar, örneğin, olay değerleri listesinden bir anahtar değer çiftini kaldırmak veya toplamın bir durumundan bir değer çıkarmak gibi, önceki birikmiş değerin durumdan ayrıştırılmaya izin verir. Aşağıdaki AccumulateDeaccumulata agregaları için JavaScript şablonu:

```JavaScript
// Sample UDA which state can be accumulated and deaccumulated.
function main() {
    this.init = function () {
        this.state = 0;
    }

    this.accumulate = function (value, timestamp) {
        this.state += value;
    }

    this.deaccumulate = function (value, timestamp) {
        this.state -= value;
    }

    this.deaccumulateState = function (otherState){
        this.state -= otherState.state;
    }

    this.computeResult = function () {
        return this.state;
    }
}
```

## <a name="uda---javascript-function-declaration"></a>UDA - JavaScript işlev bildirimi

Her JavaScript UDA bir İşlev nesnesi bildirimi ile tanımlanır. UDA tanımındaki temel unsurlar aşağıda verilmiştir.

### <a name="function-alias"></a>İşlev takma adı

İşlev takma adı UDA tanımlayıcısIdır. Akış Analizi sorgusunda çağrıldığınızda, her zaman UDA diğer adını bir "uda" ile birlikte kullanın. ekleyin.

### <a name="function-type"></a>Fonksiyon türü

UDA için işlev türü **Javascript UDA**olmalıdır.

### <a name="output-type"></a>Çıkış türü

Akışı Analitiği'nin desteklediği belirli bir tür veya sorgunuzdaki türü işlemek istiyorsanız "Any".

### <a name="function-name"></a>İşlev adı

Bu İşlev nesnesinin adı. İşlev adı UDA diğer adı ile eşleşmelidir.

### <a name="method---init"></a>Yöntem - init()

Init() yöntemi toplamın durumunu başlarayatır. Pencere başladığında bu yöntem edenir.

### <a name="method--accumulate"></a>Yöntem – birikir()

Birikmiş() yöntemi, UDA durumunu önceki durumu ve geçerli olay değerlerini temel alınarak hesaplar. Bu yöntem, bir olay bir zaman penceresine girdiğinde (YUVARLANAN PENCERE, ATLAMAPENCERESI, SLIDINGWINDOW veya SESSIONWINDOW) olarak adlandırılır.

### <a name="method--deaccumulate"></a>Yöntem – deaccumulate()

Biriktirme() yöntemi durumu önceki durumu ve geçerli olay değerlerini temel alınarak yeniden hesaplar. Bu yöntem, bir olay Bir SLIDINGWINDOW veya SESSIONWINDOW ayrıldığında denir.

### <a name="method--deaccumulatestate"></a>Yöntem – deaccumulateState()

Biriktirme Durumu() yöntemi, durumu önceki duruma ve atlama durumuna göre yeniden hesaplar. Bu yöntem, bir dizi olay ATLAMAPENCERESI bıraktığında çağrılır.

### <a name="method--computeresult"></a>Yöntem – computeResult()

ComputeResult() yöntemi, geçerli duruma göre toplam sonucu döndürür. Bu yöntem, bir zaman penceresinin sonunda (YUVARLANMA PENCERESI, ATLAMALı PENCERE, SLIDINGWINDOW veya SESSIONWINDOW) olarak adlandırılır.

## <a name="javascript-uda-supported-input-and-output-data-types"></a>JavaScript UDA desteklenen giriş ve çıktı veri türleri
JavaScript UDA veri türleri için, [Tümleç JavaScript UDF'lerinin](stream-analytics-javascript-user-defined-functions.md)bölüm **Akış Analizi ve JavaScript türü dönüştürmesine** bakın.

## <a name="adding-a-javascript-uda-from-the-azure-portal"></a>Azure portalından JavaScript UDA ekleme

Aşağıda Portal bir UDA oluşturma sürecinde yürümek. Burada kullandığımız örnek, zaman ağırlıklı ortalamaları hesaplamaktır.

Şimdi adımları izleyerek mevcut bir ASA iş altında bir JavaScript UDA oluşturalım.

1. Azure portalında oturum açın ve mevcut Akış Analizi işinizi bulun.
1. Daha sonra JOB TOPOLOGY altında fonksiyonlar **linkine**tıklayın.
1. Yeni bir işlev eklemek için **Ekle** simgesine tıklayın.
1. Yeni İşlev görünümünde, İşlev Türü olarak **JavaScript UDA'yı** seçin, ardından düzenleyicide varsayılan bir UDA şablonu görün.
1. UDA diğer adı olarak "TWA"yı doldurun ve işlev uygulamasını aşağıdaki gibi değiştirin:

    ```JavaScript
    // Sample UDA which calculate Time-Weighted Average of incoming values.
    function main() {
        this.init = function () {
            this.totalValue = 0.0;
            this.totalWeight = 0.0;
        }

        this.accumulate = function (value, timestamp) {
            this.totalValue += value.level * value.weight;
            this.totalWeight += value.weight;

        }

        // Uncomment below for AccumulateDeaccumulate implementation
        /*
        this.deaccumulate = function (value, timestamp) {
            this.totalValue -= value.level * value.weight;
            this.totalWeight -= value.weight;
        }

        this.deaccumulateState = function (otherState){
            this.state -= otherState.state;
            this.totalValue -= otherState.totalValue;
            this.totalWeight -= otherState.totalWeight;
        }
        */

        this.computeResult = function () {
            if(this.totalValue == 0) {
                result = 0;
            }
            else {
                result = this.totalValue/this.totalWeight;
            }
            return result;
        }
    }
    ```

1. "Kaydet" düğmesini tıklattığınızda, UDA'nız işlev listesinde belirir.

1. Yeni işlev "TWA" tıklayın, işlev tanımı nı kontrol edebilirsiniz.

## <a name="calling-javascript-uda-in-asa-query"></a>ASA sorgusunda JavaScript UDA'yı arama

Azure portalında ve işinizi açın, sorguyu edin ve TWA() işlevini "uda" görev önekiyle arayın. Örnek:

```SQL
WITH value AS
(
    SELECT
    NoiseLevelDB as level,
    DurationSecond as weight
FROM
    [YourInputAlias] TIMESTAMP BY EntryTime
)
SELECT
    System.Timestamp as ts,
    uda.TWA(value) as NoseDoseTWA
FROM value
GROUP BY TumblingWindow(minute, 5)
```

## <a name="testing-query-with-uda"></a>UDA ile test sorgusu

Aşağıdaki içeriğe sahip yerel bir JSON dosyası oluşturun, dosyayı Stream Analytics işine yükleyin ve sorgunun üzerinde test edin.

```JSON
[
  {"EntryTime": "2017-06-10T05:01:00-07:00", "NoiseLevelDB": 80, "DurationSecond": 22.0},
  {"EntryTime": "2017-06-10T05:02:00-07:00", "NoiseLevelDB": 81, "DurationSecond": 37.8},
  {"EntryTime": "2017-06-10T05:02:00-07:00", "NoiseLevelDB": 85, "DurationSecond": 26.3},
  {"EntryTime": "2017-06-10T05:03:00-07:00", "NoiseLevelDB": 95, "DurationSecond": 13.7},
  {"EntryTime": "2017-06-10T05:03:00-07:00", "NoiseLevelDB": 88, "DurationSecond": 10.3},
  {"EntryTime": "2017-06-10T05:05:00-07:00", "NoiseLevelDB": 103, "DurationSecond": 5.5},
  {"EntryTime": "2017-06-10T05:06:00-07:00", "NoiseLevelDB": 99, "DurationSecond": 23.0},
  {"EntryTime": "2017-06-10T05:07:00-07:00", "NoiseLevelDB": 108, "DurationSecond": 1.76},
  {"EntryTime": "2017-06-10T05:07:00-07:00", "NoiseLevelDB": 79, "DurationSecond": 17.9},
  {"EntryTime": "2017-06-10T05:08:00-07:00", "NoiseLevelDB": 83, "DurationSecond": 27.1},
  {"EntryTime": "2017-06-10T05:09:00-07:00", "NoiseLevelDB": 91, "DurationSecond": 17.1},
  {"EntryTime": "2017-06-10T05:09:00-07:00", "NoiseLevelDB": 115, "DurationSecond": 7.9},
  {"EntryTime": "2017-06-10T05:09:00-07:00", "NoiseLevelDB": 80, "DurationSecond": 28.3},
  {"EntryTime": "2017-06-10T05:10:00-07:00", "NoiseLevelDB": 55, "DurationSecond": 18.2},
  {"EntryTime": "2017-06-10T05:10:00-07:00", "NoiseLevelDB": 93, "DurationSecond": 25.8},
  {"EntryTime": "2017-06-10T05:11:00-07:00", "NoiseLevelDB": 83, "DurationSecond": 11.4},
  {"EntryTime": "2017-06-10T05:12:00-07:00", "NoiseLevelDB": 89, "DurationSecond": 7.9},
  {"EntryTime": "2017-06-10T05:15:00-07:00", "NoiseLevelDB": 112, "DurationSecond": 3.7},
  {"EntryTime": "2017-06-10T05:15:00-07:00", "NoiseLevelDB": 93, "DurationSecond": 9.7},
  {"EntryTime": "2017-06-10T05:18:00-07:00", "NoiseLevelDB": 96, "DurationSecond": 3.7},
  {"EntryTime": "2017-06-10T05:20:00-07:00", "NoiseLevelDB": 108, "DurationSecond": 0.99},
  {"EntryTime": "2017-06-10T05:20:00-07:00", "NoiseLevelDB": 113, "DurationSecond": 25.1},
  {"EntryTime": "2017-06-10T05:22:00-07:00", "NoiseLevelDB": 110, "DurationSecond": 5.3}
]
```

## <a name="get-help"></a>Yardım alın

Ek yardım için [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics) deneyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Akış Analizine Giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akışı Analytics sorgu dili başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akışı Analytics yönetimi REST API başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
