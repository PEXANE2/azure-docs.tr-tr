---
title: Azure HDInsight Hive tabloları - Team Data Science Process içinde örnek veriler
description: Aşağı örnek verileri analiz için daha kolay yönetilebilir bir boyuta azaltmak için Hive sorgularını kullanarak Azure HDInsight Hive tablolarında depolanan veriler.
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
ms.openlocfilehash: df85edc3de00e2b0342bc3102fe9e85564a9835b
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76720002"
---
# <a name="sample-data-in-azure-hdinsight-hive-tables"></a>Azure HDInsight Hive tablolarındaki örnek veriler
Bu makalede, aşağı örnek bunu analiz için daha kolay yönetilebilir bir boyuta indirmeyi Hive sorgularını kullanarak Azure HDInsight Hive tablolarında depolanan veriler açıklar. Üç adet popuya kullanılan örnekleme yöntemini içerir:

* Tekdüzen rastgele örnekleme
* Gruplara göre rastgele örnekleme
* Stratified örnekleme

**Verileriniz neden örnekleyebilirsiniz?**
Veri kümesini analiz etmek için planlama büyükse, genellikle aşağı örnek veriler için daha küçük ancak temsilcisi ve daha kolay yönetilebilir bir boyutunu azaltmak için iyi bir fikir olduğunu. Aşağı örnekleme, özellik Mühendisliği veri anlama ve keşfetme kolaylaştırır. Team Data Science Process içinde rolünü hızlı prototip oluşturma veri işleme işlevleri ve makine öğrenimi modellerini etkinleştirmektir.

Bu örnekleme görevi, [ekip veri bilimi işlemindeki (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/)bir adımdır.

## <a name="how-to-submit-hive-queries"></a>Hive sorguları göndermek nasıl
Hive sorguları, Hadoop küme baş düğümü üzerinde Hadoop komut satırı konsolundan gönderilebilir.  Hadoop kümesinin baş düğümünde oturum açın, Hadoop komut satırı konsolunu açın ve Hive sorgularını buradan gönderebilirsiniz. Hadoop komut satırı konsolunda Hive sorguları gönderme yönergeleri için bkz. [Hive sorguları gönderme](move-hive-tables.md#submit).

## <a name="uniform"></a>Tekdüzen rastgele örnekleme
Tekdüzen rastgele örnekleme, her satırda bir veri kümesi örnekleniyor eşit bir olasılığını olduğunu gösterir. Ek alan rand() veri kümesine iç sorgu "Seç" ve "seçin" dış sorgu bu koşul, rastgele alan ekleyerek uygulanabilir.

Örnek bir sorgu aşağıda verilmiştir:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, …, fieldN
    from
        (
        select
            field1, field2, …, fieldN, rand() as samplekey
        from <hive table name>
        )a
    where samplekey<='${hiveconf:sampleRate}'

Burada, `<sample rate, 0-1>` kullanıcıların örneklemek istediği kayıt oranını belirtir.

## <a name="group"></a>Gruplara göre rastgele örnekleme
Örnekleme kategorik veriler, dahil etmek veya hariç tüm örnekleri için bazı kategorik değişkenin değerini istediğinizde. Bu tür bir örnekleme "grubu tarafından örnekleme" adı verilir. Örneğin, NY, MA, CA, NJ ve PA gibi değerler içeren kategorik bir değişkeniniz "*durum*" ise, örneklenip örneklenmeseler her bir durum için kayıtların birlikte olmasını istersiniz.

Gruplandırma ölçütü bu örnekleri örnek bir sorgu aşağıdadır:

    SET sampleRate=<sample rate, 0-1>;
    select
        b.field1, b.field2, …, b.catfield, …, b.fieldN
    from
        (
        select
            field1, field2, …, catfield, …, fieldN
        from <table name>
        )b
    join
        (
        select
            catfield
        from
            (
            select
                catfield, rand() as samplekey
            from <table name>
            group by catfield
            )a
        where samplekey<='${hiveconf:sampleRate}'
        )c
    on b.catfield=c.catfield

## <a name="stratified"></a>Bağlı örnekleme
Elde edilen örnekler üst popülasyon oldukları gibi aynı oranı mevcut olan kategorik değerlere sahip olduğunda rastgele örnekleme Kategorik bir değişkene göre stratified. Yukarıdaki olarak aynı örneği kullanarak verilerinizi durumlara göre aşağıdaki gözlemlere sahip olduğunu varsayalım: NJ varsa 100 gözlemleri, NY olan 60 gözlemleri ve WA 300 gözlemler. 0,5 olmasını stratified örnekleme oranını belirtin, ardından alınan örnek yaklaşık 50, 30 ve 150 gözlemleri NJ, NY ve WA sırasıyla olmalıdır.

Örnek bir sorgu aşağıda verilmiştir:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, field3, ..., fieldN, state
    from
        (
        select
            field1, field2, field3, ..., fieldN, state,
            count(*) over (partition by state) as state_cnt,
              rank() over (partition by state order by rand()) as state_rank
          from <table name>
        ) a
    where state_rank <= state_cnt*'${hiveconf:sampleRate}'


Hive 'de kullanılabilen daha gelişmiş örnekleme yöntemleri hakkında daha fazla bilgi için bkz. [Languagemanual örnekleme](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).

