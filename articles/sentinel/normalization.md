---
title: Azure Sentinel 'de veri normalleştirme | Microsoft Docs
description: Bu makalede, Azure Sentinel 'in verileri birçok farklı kaynaktan normalleştirir ve normalleştirme şemasının ayrıntıları açıklanmaktadır.
services: sentinel
cloud: na
documentationcenter: na
author: yelevin
manager: rkarlin
ms.assetid: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/08/2020
ms.author: yelevin
ms.openlocfilehash: 5d847ac7ed805ad88bc24ed63896edc6f7596f9b
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "101729785"
---
# <a name="normalization-in-azure-sentinel"></a>Azure Sentinel 'te normalleştirme

Bu makalede, Azure Sentinel 'de veri şeması normalleştirme ve özellikle bir bağlantının dahil olduğu ağ bağlantıları ve oturumlar şeması açıklanmaktadır.

## <a name="what-is-normalization"></a>Normalleştirme nedir

Çeşitli veri türleri ve tablolarla birlikte çalışarak sorunlar sunulmaktadır. Birçok farklı veri türü ve şeması hakkında bilgi sahibi olmanız, her biri için benzersiz bir analiz kuralları kümesi, çalışma kitabı ve sorgu oluşturma sorguları (örneğin, güvenlik duvarı cihazlarıyla ilgili) paylaştıkları gibi, bunların her biri için yazma ve kullanma zorunlulukından yararlanabilirsiniz. Araştırma ve arama için gereken farklı veri türleri arasındaki bağıntı da zordur. Azure Sentinel, çeşitli kaynaklardan gelen verileri Tekdüzen, normalleştirilmiş görünümlerde işlemek için sorunsuz bir deneyim sunar.

Azure Sentinel **ortak bilgi modeli** üç unsurdan oluşur:

- **Normalleştirilmiş şemalar** , ile birlikte çalışmak ve birleştirilmiş özellikleri (örneğin, ağ tablosu) oluşturmak için kolay olan, öngörülebilir olay türleri (tablolar) kümesini kapsar. Şema ayrıca normalleştirilmiş bir sütun kuralı ve değer ve biçim standartlaştırma tanımları (IP adresleri gibi verilerin standart tutarlı gösterimi) içerir.

- **Çözümleyiciler** , farklı türlerdeki mevcut verileri normalleştirilmiş şemaya eşler. Modele göre, veriler sorgu süresi (işlevleri kullanılarak) veya alım süresi içinde normalleştirilmiş şemayla ayrıştırılabilir. Şu anda yalnızca sorgu zamanı ayrıştırma desteklenir.

- **Her normalleştirilmiş şemanın içeriği** analiz kuralları, etkileşimli çalışma kitapları, arama sorguları ve ek içerik içerir.

### <a name="current-release"></a>Geçerli yayın

Bu sürümde, [Normalleştirilmemiş ağ bağlantıları ve oturumlar şeması](./normalization-schema.md) (v 1.0.0) genel önizleme için kullanılabilir. Şema, ağ bağlantılarını veya güvenlik duvarları, hat verileri, NSG, Netflow, proxy sistemleri ve web güvenliği ağ geçitleri tarafından günlüğe kaydedilen oturumları açıklar.  Seçili sorgu zamanı Çözümleyicileri ve analiz kuralları şemayla birlikte kullanılabilir ve bunu kullanabilir.

Şema Şu anda yalnızca sorgu zamanı Çözümleyicileri (bkz. çözümleyiciler bölümü) ile kullanılabilir.

Verileri ek gösterimlerine ayrıştırarak, mevcut ve gelecekteki normalleştirilmiş tablolarla tutarlı olacak sütunlar oluşturmak için [Ossem varlıkları adlandırma modelini](https://ossemproject.com/cdm/entities/intro.html#) kullanabilirsiniz.

## <a name="normalized-schema-and-parsing"></a>Normalleştirilmiş şema ve ayrıştırma

### <a name="how-our-normalized-schemas-are-defined"></a>Normalleştirilmiş şemalarımızın tanımlanması

Azure Sentinel, [açık kaynak güvenliği olayları meta verileri (OSSEM)](https://ossemproject.com/intro.html) ortak bilgi modeliyle hizalanıyor ve bu da normalleştirilmiş varlıklarda normalleştirilenlere izin verir. OSSEM, birincil olarak farklı veri kaynaklarından ve işletim sistemlerinden gelen güvenlik olay günlüklerinin belgelerine ve standartına odaklanan topluluk temelli bir projem. Ayrıca, proje, Güvenlik analistlerinin farklı veri kaynakları genelinde verileri sorgulamasına ve çözümlemesine olanak tanımak için veri normalleştirme yordamları sırasında veri mühendisleri için kullanılabilen ortak bir bilgi modeli (CıM) sağlar.

[Ossem CIM](https://ossemproject.com/cdm/intro.html) bir varlık kümesi tanımlar (örneğin: dosya, ana BILGISAYAR, IP, işlem) ve bu tür bir varlık için bir öznitelik kümesi tanımlar. Ayrıca CıM, sorunsuz ve öngörülebilir bağıntı sağlayan bu varlıklardan ilgili öznitelikleri kullanan bir tablo kümesi tanımlar (örneğin, [ağ oturumu](https://ossemproject.com/cdm/tables/network_session.html) tablosu). Varlıkların iç içe olabileceğini unutmayın (örneğin, kaynak varlık bir ad özniteliği olacak bir dosya varlığı içerebilir).

OSSEM varlık yapısı hakkında daha fazla bilgi edinmek için [RESMI ossem başvurusunu](https://ossemproject.com/cdm/guidelines/entity_structure.html)ziyaret edin.

### <a name="how-the-normalized-schemas-are-implemented-in-azure-sentinel"></a>Azure Sentinel 'de normalleştirilmiş şemalar nasıl uygulanır

Azure Sentinel 'de OSSEM CıM 'in uygulanmasında, bu temsilin yerleşik bir tablo olup olmadığı veya mevcut verileri bu gösterimde eşleştiren sorgu zamanı Çözümleyicileri ya da işlevleri kullanılarak oluşturulmuş olması halinde, OSSEM gösterimini Log Analytics tablolu bir gösterimine yansıtıyoruz. Tablo gösteriminde, OSSEM varlık adlarını ve öznitelik adlarını birleştirirken ve bunları topluca tek bir sütun adıyla eşleştiriyoruz. Örneğin, MD5 özniteliği içeren bir karma varlık içeren bir dosya varlığı içeren bir kaynak varlık, şu Log Analytics sütunu olarak uygulanır: SrcFileHashMd5. (OSSEM varsayılan olarak *snake_case* kullanır, Azure Sentinel ve Log Analytics *PascalCase* kullanır. OSSEM 'de böyle bir sütun src_file_hash_md5.)

Azure Sentinel uygulamasında ek özel alanlar, Log Analytics Platform gereksinimleri ve Azure Sentinel müşterilerine özgü durumlar nedeniyle mevcut olabilir.

### <a name="schema-reference"></a>Şema başvurusu

Daha fazla bilgi için bkz. [şema başvuru belgesi](./normalization-schema.md)ve resmi [ossem proje belgeleri](https://ossemproject.com/cdm/intro.html).

Şema başvurusu Ayrıca değer ve biçim standartlaştırma içerir. Özgün veya ayrıştırılmış kaynak alanları standart bir biçimde olmayabilir veya şema standart değerleri listesini kullanamaz ve bu nedenle tamamen normalleştirilmeleri için şema standart gösterimine dönüştürülmesi gerekir.

## <a name="parsers"></a>Icılar

- [Ayrıştırma nedir](#what-is-parsing)
- [Sorgu süresi Çözümleyicileri kullanma](#using-query-time-parsers)

### <a name="what-is-parsing"></a>Ayrıştırma nedir

Bir temel tanımlanmış normalleştirilmiş tablolar kümesi ile, verilerinizi bu tablolarla dönüştürmeniz (ayrıştırmanıza/eşlemenizi) gerekir. Diğer bir deyişle, ham biçimindeki belirli verileri, normalleştirilmiş şemada iyi bilinen sütunlara ayıklayacaksınız. Azure Sentinel 'de ayrıştırma **sorgu zaman** -Çözümleyicileri, varolan tablolardaki (CommonSecurityLog, özel günlük tabloları, Syslog gibi) verileri normalleştirilmiş tablolar şemasına dönüştüren Log Analytics Kullanıcı Işlevleri (kusto sorgu DILI-KQL kullanarak) olarak oluşturulur.

Azure Sentinel 'de henüz desteklenmeyen diğer ayrıştırma türü, veri kaynaklarından alınan verileri doğrudan normalleştirilmiş  tablo (lar) halinde toplamaya izin veriliyor. Alma süresi ayrıştırma, veri modeli işlevleri kullanma gerekmeden doğrudan sorgulandığı için gelişmiş performans sağlar.

### <a name="using-query-time-parsers"></a>Sorgu süresi Çözümleyicileri kullanma

- [Ayrıştırıcı yükleme](#installing-a-parser)
- [Çözümleyicileri kullanma](#using-the-parsers)
- [Çözümleyicileri özelleştirme](#customizing-parsers)

#### <a name="installing-a-parser"></a>Ayrıştırıcı yükleme

Kullanılabilir sorgu süresi ayrıştırıcıları, Azure Sentinel [resmi GitHub deposunda](https://github.com/Azure/Azure-Sentinel/tree/master/Parsers/Normalized%20Schema%20-%20Networking%20(v1.0.0))bulunur. Her ayrıştırıcı, müşterilerin gelecekteki güncelleştirmeleri kolayca kullanmalarına ve izlemesine olanak tanımak için sürümlüdür. Bir Ayrıştırıcı yüklemek için:

1. Yukarıdaki GitHub bağlantısı içindeki ilgili her bir KQL dosyasındaki ilgili ayrıştırıcı içeriğini panonuza kopyalayın

1. Azure Sentinel portalında Günlükler sayfasını açın ve KQL dosyasının içeriğini Günlükler ekranına yapıştırın ve **Kaydet**' e tıklayın.

    :::image type="content" source="./media/normalization/install-new-parser.png" alt-text="Yeni bir Ayrıştırıcı yükler":::

1. Kaydet iletişim kutusunda alanları aşağıdaki gibi doldurun:
    1. **Ad**: **işlev diğer adı** alanında kullanılan aynı değerin kullanılması önerilir (Yukarıdaki örnekte, *CheckPoint_Network_NormalizedParser*)
    
    1. **Farklı kaydet**: **işlev** Seç

    1. **Işlev diğer adı**: aşağıdaki adlandırma kuralı- *PRODUCT_Network_NormalizedParser* olarak adlandırılmalıdır (Yukarıdaki örnekte, *CheckPoint_Network_NormalizedParser*).

    1. **Kategori**: varolan bir kategoriyi seçebilir veya yeni kategori ( *Normalizednetworksessionspariler* gibi) oluşturabilirsiniz
    
        :::image type="content" source="./media/normalization/save-new-parser.png" alt-text="Ayrıştırıcısı Kaydet":::

Çözümleyicileri doğru bir şekilde kullanabilmek için, boş ağ şeması ayrıştırıcısının (tüm ağ oturumları şeması alanlarının boş bir tablosal görünümü oluşturan) ve ağ meta ayrıştırıcısının (ağ şemasında çeşitli kaynaklardan gelen verilerin tek bir görünümünü oluşturmak için tüm etkin Çözümleyicileri birlikte bulunan) de yüklenmesi gerekir. Bu iki ayrıştırıcıları yüklemek, belirtilen adımlara benzer bir şekilde yapılır.

Sorgu işlevini kaydettikten sonra, sorgu Gezginini kapatıp yeni işlevin yansıtılması için yeniden açmanız gerekebilir.

#### <a name="using-the-parsers"></a>Çözümleyicileri kullanma

Etkinleştirildikten sonra, şu anda etkinleştirilmiş olan tüm çözümleyiciler üzerinde Birleşik bir görünümü sorgulamak için meta ayrıştırıcısı kullanabilirsiniz. Bunu yapmak için Azure Sentinel Günlükler sayfasına gidin ve meta ayrıştırıcısı sorgulayın:

:::image type="content" source="./media/normalization/query-parser.png" alt-text="Ayrıştırıcısı sorgulama":::
 
Ayrıca, ' sorgu Gezgini ' ' ne tıklayarak Günlükler sayfasında sorgu Gezgini ' ni kullanarak meta ayrıştırıcıya da tek tek ayrıştırıcıya erişebilirsiniz:

:::image type="content" source="./media/normalization/query-explorer.png" alt-text="Sorgu Gezgini":::

Sağdaki bölmede, "kayıtlı sorgular" bölümünü genişletin ve ' Normalizednetworkayrıştırıcıları ' klasörünü (veya Çözümleyicileri oluştururken seçtiğiniz kategori adını) bulun:

:::image type="content" source="./media/normalization/find-parser.png" alt-text="Ayrıştırıcılarınızı bulun":::

Her bir ayrıştırıcıya tıklayıp kullandığı temel işlevi görebilir ve bunu çalıştırabilir (veya yukarıda açıklandığı şekilde, diğer adıyla doğrudan erişebilirsiniz). Bazı ayrıştırıcıların, özgün alanları kolay bir şekilde normalleştirilmek üzere normalleştirilmiş alanlara korumasını sağlayabilirsiniz. Bu, ayrıştırıcının sorgusunda kolayca düzenlenebilir.

> [!TIP]
> Arama ve algılama sorguları dahil olmak üzere herhangi bir sorguda Azure Sentinel tabloları yerine kaydedilmiş işlevlerinizi kullanabilirsiniz. Daha fazla bilgi için bkz.
>
> - [Azure Sentinel 'de veri normalleştirme](normalization.md#parsers)
> - [Azure Izleyici günlüklerinde metin ayrıştırma](../azure-monitor/logs/parse-text.md)
>
#### <a name="customizing-parsers"></a>Çözümleyicileri özelleştirme

Yukarıdaki adımları tekrarlayabilirsiniz (sorgu Gezgini 'nde ayrıştırıcısı bulma), ilgili ayrıştırıcıya tıklayıp işlev uygulamasını görebilirsiniz.
Örneğin, bağımsız ayrıştırıcıları eklemek/kaldırmak için meta ayrıştırıcısı düzenlemeye karar verebilirsiniz.

:::image type="content" source="./media/normalization/customize-parser.png" alt-text="Ayrıştırıcılarınızı özelleştirme":::
 
İşlev değiştirildikten sonra, "Kaydet" e tıklayın ve aynı adı, diğer adı ve kategoriyi kullanın. Bir geçersiz kılma iletişim kutusu açılır. "Tamam" düğmesine basın:

:::image type="content" source="./media/normalization/are-you-sure.png" alt-text="Emin misiniz":::

#### <a name="additional-information"></a>Ek bilgiler

JSON, XML ve CSV, özellikle sorgu zamanında ayrıştırmaya uygundur. Azure Sentinel, JSON, XML ve CSV için yerleşik ayrıştırma işlevlerine ve ayrıca bir JSON ayrıştırma aracına sahiptir.  Daha fazla bilgi için bkz. [Azure Sentinel 'de JSON alanlarını kullanma](https://techcommunity.microsoft.com/t5/azure-sentinel/tip-easily-use-json-fields-in-sentinel/ba-p/768747) (blog). 

Log Analytics 'de [kayıtlı sorgular](../azure-monitor/logs/example-queries.md) (sorgu zaman Çözümleyicileri uygulama) hakkında daha fazla bilgi edinin.


## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Azure Sentinel 'in normalleştirme şeması hakkında bilgi edindiniz. Başvuru şemasının kendisi için bkz. [Azure Sentinel veri normalleştirme şeması başvurusu](./normalization-schema.md).

* [Azure Sentinel blogu](https://aka.ms/azuresentinelblog). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.