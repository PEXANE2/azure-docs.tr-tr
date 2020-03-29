---
title: Azure HDInsight Hive tablolarında örnek veriler - Takım Veri Bilimi Süreci
description: Verileri analiz için daha yönetilebilir bir boyuta düşürmek için Hive sorgularını kullanarak Azure HDInsight Hive tablolarında depolanan alt örnek verileri.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76720002"
---
# <a name="sample-data-in-azure-hdinsight-hive-tables"></a>Azure HDInsight Hive tablolarındaki örnek veriler
Bu makalede, azure HDInsight Hive tablolarında depolanan verileri, çözümleme için daha yönetilebilir bir boyuta düşürmek için Hive sorgularını kullanarak nasıl alttan örnekleyikarşılanınca açıklanmaktadır. Halk arasında kullanılan üç örnekleme yöntemini kapsar:

* Düzgün rastgele örnekleme
* Gruplara göre rastgele örnekleme
* Tabakalı örnekleme

**Neden verilerinizi örneklediniz?**
Çözümlemayı planladığınız veri kümesi büyükse, verileri daha küçük ancak temsili ve daha yönetilebilir boyuta düşürmek için verileri alttan örneklemek genellikle iyi bir fikirdir. Aşağı örnekleme veri anlaşılmasını, keşfi ni ve özellik mühendisliğini kolaylaştırır. Ekip Veri Bilimi Süreci'ndeki rolü, veri işleme işlevlerinin ve makine öğrenimi modellerinin hızlı prototiplemesini sağlamaktır.

Bu örnekleme [görevi, Ekip Veri Bilimi Süreci'nin (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/)bir adımıdır.

## <a name="how-to-submit-hive-queries"></a>Hive sorguları nasıl gönderilir?
Kovan sorguları Hadoop kümesinin baş düğümündeki Hadoop Command-Line konsolundan gönderilebilir.  Hadoop kümesinin baş düğümüne giriş yapın, Hadoop Command-Line konsolunu açın ve Hive sorgularını oradan gönderin. Hadoop Command-Line konsolunda Hive sorguları gönderme yönergeleri için [bkz.](move-hive-tables.md#submit)

## <a name="uniform-random-sampling"></a><a name="uniform"></a>Düzgün rastgele örnekleme
Düzgün rasgele örnekleme, veri kümesindeki her satırın örnekleme olasılığı eşit olduğu anlamına gelir. İç "select" sorgusundaki veri kümesine ve bu rasgele alandaki dış "select" sorgusuna fazladan bir alan rand() eklenerek uygulanabilir.

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

Burada, `<sample rate, 0-1>` kullanıcıların örneklemek istediği kayıtların oranını belirtir.

## <a name="random-sampling-by-groups"></a><a name="group"></a>Gruplara göre rastgele örnekleme
Kategorik verileri örnekleme yaparken, kategorik değişkenin bazı değeri için tüm örnekleri dahil etmek veya hariç tutmak isteyebilirsiniz. Bu tür örnekleme "gruba göre örnekleme" olarak adlandırılır. Örneğin, NY, MA, CA, NJ ve PA gibi değerlere sahip kategorik bir değişken "*Durum*" varsa, örneklenmiş olsun veya olmasın, her durumdan kayıtların birlikte olmasını istersiniz.

Burada grup örnekleri bir örnek sorgu:

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

## <a name="stratified-sampling"></a><a name="stratified"></a>Tabakalı örnekleme
Rasgele örnekleme, elde edilen örneklerin ana popülasyondakiyle aynı oranda mevcut olan kategorik değerlere sahip olması durumunda kategorik bir değişkene göre sınıflandırılır. Yukarıdaki örneği kullanarak, verilerinizin durumlara göre aşağıdaki gözlemlere sahip olduğunu varsayalım: NJ'nin 100 gözlemi, NY'nin 60 gözlemi ve WA'nın 300 gözlemi vardır. Tabakalı örnekleme oranını 0,5 olarak belirtirseniz, elde edilen örneklemde sırasıyla NJ, NY ve WA'nın yaklaşık 50, 30 ve 150 gözlemi olmalıdır.

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


Hive'da bulunan daha gelişmiş örnekleme yöntemleri hakkında daha fazla bilgi için [LanguageManual Sampling'a](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling)bakın.

