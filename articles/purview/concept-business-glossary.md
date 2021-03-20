---
title: Azure purview 'ta iş sözlüğü özelliklerini anlama (Önizleme)
description: Bu makalede, Azure purview 'da iş sözlüğü ne olduğu açıklanır.
author: nayenama
ms.author: nayenama
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: conceptual
ms.date: 11/13/2020
ms.openlocfilehash: 8b391438d8d6605e7ef493a6552af634db840ad5
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96555522"
---
# <a name="understand-business-glossary-features-in-azure-purview"></a>Azure purview 'da iş sözlüğü özelliklerini anlama

Bu makalede, Azure purview 'daki iş sözlüğü özelliğine bir genel bakış sunulmaktadır. 

## <a name="business-glossary"></a>İş sözlüğü

Sözlük, iş kullanıcıları için sözlük sağlar.  Bu, birbirleriyle ilgili olabilecek iş koşullarından oluşur ve farklı bağlamlarda anlaşılabilmesi için kategorilere ayrılmalarını sağlar. Bu terimler daha sonra bir veritabanı, tablolar, sütunlar vb. gibi varlıklarla eşleştirilebilir. Bu, veri depolarıyla ilişkili Technical jarg'in soyut olmasına yardımcı olur ve iş kullanıcısının bu verileri daha tanıdık olan Sözlük içinde bulmasına ve verilerle çalışmasına izin verir.


İş sözlüğü bir terim koleksiyonudur. Her terim kuruluştaki bir nesneyi temsil eder ve aynı nesneyi temsil eden birden çok terim vardır. Müşteri, Satınalmacı veya alıcı olarak da adlandırılır. Bu birden çok terim birbirleriyle bir ilişkiye sahiptir. Bu terimler arasındaki ilişki aşağıdakilerden biri olabilir:

- eş anlamlı-aynı tanıma sahip farklı terimler
- benzer tanımda ilgili-farklı ad

Aynı terim birden çok iş nesnesini de kapsıyor. Her bir terimin, kuruluş içinde iyi tanımlanmış ve açıkça anlaşıldığından emin olmanız önemlidir.

## <a name="custom-attributes"></a>Özel öznitelikler

Azure purview, herhangi bir iş sözlüğü terimi için sekiz kullanıma hazır öznitelikleri destekler:
- Name
- Tanım
- Veri busürümleri
- Veri uzmanları
- Kısaltması
- Eş Anlamlı Sözcükler
- İlgili terimler
- Kaynaklar

Bu öznitelikler düzenlenemez veya silinemez. Ancak, bu öznitelikler bir kuruluştaki bir terimi tamamen tanımlamak için yeterli değildir. Bu sorunu çözmek için, purview, sözlüğizin için özel öznitelikler tanımlayabileceğiniz bir özellik sağlar.

## <a name="term-templates"></a>Terim şablonları

Terim şablonları, Katalog içinde mantıksal olarak birlikte gruplanmış sözlük özel öznitelikleri sağlar. Özelliği, ilgili tüm özel öznitelikleri bir şablonda birlikte gruplamanıza ve sözlük terimi oluştururken şablonu uygulamanıza olanak tanır. Örneğin, maliyet merkezi, kar merkezi, muhasebe kodu gibi tüm finans ile ilgili özel öznitelikler, bir terim şablonu finans şablonunda gruplandırılabilir ve finans eklentisi finansal Sözlük terimleri oluşturmak için kullanılabilir.

Tüm standart öznitelikler, sistem varsayılan şablonunda gruplandırılır. Oluşturduğunuz herhangi bir terim şablonu, şablon oluşturma sürecinin bir parçası olarak oluşturulan ek özel özniteliklerle birlikte bu öznitelikleri içerir.

## <a name="glossary-vs-classification-vs-sensitivity-labels"></a>Sözlük vs sınıflandırma vs duyarlılık etiketleri

Sözlük terimleri, sınıflandırmalar ve Etiketler bir veri varlığının ek açıklamalarıyla, her birinin Katalog bağlamında farklı bir anlamı vardır. 

### <a name="glossary"></a>Sözlük

Yukarıda belirtildiği gibi, Iş sözlüğü terimi bir kuruluşun iş sözlüğünü tanımlar ve şirketinizdeki çeşitli Departmanlar arasında boşluk köprülemeye yardımcı olur.

### <a name="classifications"></a>Sınıflandırmalar

Sınıflandırmalar, varlıklara atanabileceği ek açıklamalardır. Sınıflandırmaların esnekliği, bunları gibi birden çok senaryo için kullanmanıza olanak sağlar:

- veri varlıklarıyla depolanan verilerin yapısını anlama
- erişim denetim ilkelerini tanımlama

Takip eden, bugün 100 ' den fazla sistem sınıflandırıcıya sahiptir ve Katalog içinde kendi sınıflandırıcılarınızı tanımlayabilirsiniz. Tarama sürecinin bir parçası olarak, bu sınıflandırmaları otomatik olarak algılar ve bunları veri varlıklarına ve şemalarına uygularsınız. Ancak, bunları dilediğiniz zaman geçersiz kılabilirsiniz. İnsan geçersiz kılmaları hiçbir şekilde otomatik taramalarla değiştirilmez.

### <a name="sensitivity-labels"></a>Duyarlılık etiketleri

Duyarlılık etiketleri, hindering üretkenlik ve işbirliği olmadan kuruluşunuzun verilerini sınıflandırmanıza ve korumanıza olanak sağlayan bir ek açıklama türüdür. Duyarlılık etiketleri, kuruluş verilerinizde sınıflandırma türlerinin kategorilerini belirlemek için kullanılır ve her kategoriye uygulamak istediğiniz ilkeleri gruplayın. Takip görünümü, tüm içerik ve veri Emlak için mevcut güvenlik ilkelerinizi ve korumanızı uzatmanıza olanak sağlayan Microsoft 365 ile aynı hassas bilgi türlerini kullanır. Aynı Etiketler, purview 'daki Microsoft Office ürünler ve veri varlıkları arasında paylaşılabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Terim şablonlarını yönetme](how-to-manage-term-templates.md)
- [Azure purview 'da veri kataloğuna gözatın](how-to-browse-catalog.md)
