---
title: Azure Veri Kataloğu'nda iş sözlüğü ayarlama
description: Kayıtlı veri varlıklarını etiketlemek için ortak bir işletme sözcük dağarcığı tanımlamak ve kullanmak için Azure Veri Kataloğu'ndaki iş sözlüğünün vurgulandığı nasıl dır?
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: 1065abecb1f0ef57eb13b1ec3f194f07ae01eaee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68976803"
---
# <a name="set-up-the-business-glossary-for-governed-tagging"></a>Yönetilen etiketleme için iş sözlüğü ayarlama

## <a name="introduction"></a>Giriş

Azure Veri Kataloğu, analiz yapmak ve karar vermek için ihtiyacınız olan veri kaynaklarını kolayca keşfedebilmeniz ve anlayabilirsiniz. Bu özellikler, mevcut veri kaynaklarının en geniş yelpazesini bulabileceğinizve anladığınızı zedebilirsiniz.

Varlıkların verilerinin daha iyi anlaşılmasını teşvik eden bir Veri Kataloğu özelliği etiketlemedir. Etiketlemeyi kullanarak, anahtar kelimeleri bir varlık veya sütunla ilişkilendirebilirsiniz, bu da arama veya tarama yoluyla varlığı keşfetmeyi kolaylaştırır. Etiketleme, varlığın içeriğini ve amacını daha kolay anlamanıza da yardımcı olur.

Ancak, etiketleme bazen kendi sorunlarına neden olabilir. Etiketlemenin başlatabileceği sorunlara bazı örnekler şunlardır:

* Bazı varlıklarda kısaltmakullanımı ve diğerlerinde genişletilmiş metin. Bu tutarsızlık, amaç varlıkları aynı etiketle etiketlemek olsa bile varlıkların keşfini engeller.
* İçerime bağlı olarak anlam daki olası değişimler. Örneğin, müşteri veri kümesindeki *Gelir* adlı bir etiket, müşteriye göre gelir anlamına gelebilir, ancak üç aylık satış veri kümesindeki aynı etiket şirket için üç aylık gelir anlamına gelebilir.  

Bu ve benzeri zorlukları n gidermek için Veri Kataloğu bir iş sözlüğü içerir.

Veri Kataloğu iş sözlüğü kullanarak, bir kuruluş ortak bir iş sözlüğü oluşturmak için önemli iş terimlerini ve tanımlarını belgeleyebilir. Bu yönetim, kuruluş genelinde veri kullanımında tutarlılık sağlar. Bir terim iş sözlüğünde tanımlandıktan sonra, katalogdaki bir veri kıymetine atanabilir. Bu yaklaşım, *yönetilen etiketleme,* etiketleme ile aynı yaklaşımdır.

## <a name="glossary-availability-and-privileges"></a>Sözlük kullanılabilirliği ve ayrıcalıkları

İş sözlüğü yalnızca Azure Veri Kataloğu'nun Standart Sürümü'nde kullanılabilir. Veri Kataloğunun Ücretsiz Sürümü bir sözlük içermez ve yönetilen etiketleme için özellikler sağlamaz.

Veri Kataloğu portalının navigasyon menüsündeki **Sözlük** seçeneğinden işletme sözlüğüne erişebilirsiniz.  

![Veri Kataloğu - İş sözlüğüne erişin](./media/data-catalog-how-to-business-glossary/01-portal-menu.png)

Veri Kataloğu yöneticileri ve sözlük yöneticileri rolü üyeleri, iş sözlüğünde sözlük terimleri oluşturabilir, düzenleyebilir ve silebilir. Tüm Veri Kataloğu kullanıcıları terim tanımlarını görüntüleyebilir ve varlıkları sözlük terimleriyle etiketleyebilir.

![Veri Kataloğu - Yeni bir sözlük terimi ekle](./media/data-catalog-how-to-business-glossary/02-new-term.png)

## <a name="creating-glossary-terms"></a>Sözlük terimleri oluşturma

Veri Kataloğu yöneticileri ve sözlük yöneticileri **Yeni Dönem** düğmesini tıklatarak sözlük terimleri oluşturabilir. Her sözlük terimi aşağıdaki alanları içerir:

* Terim için bir iş tanımı
* Varlık veya sütun için amaçlanan kullanım veya iş kurallarını yakalayan bir açıklama
* Dönem hakkında en çok şey bilen paydaşların listesi
* Terimin düzenlendiği hiyerarşiyi tanımlayan ana terim

## <a name="glossary-term-hierarchies"></a>Sözlük dönem hiyerarşileri

Veri Kataloğu iş sözlüğü kullanarak, bir kuruluş iş sözcük dağarcığını terimler hiyerarşisi olarak tanımlayabilir ve iş taksonomisini daha iyi temsil eden terimlerin sınıflandırılmasını oluşturabilir.

Bir terim belirli bir hiyerarşi düzeyinde benzersiz olmalıdır. Yinelenen adlar izin verilmez. Bir hiyerarşideki düzey sayısının sınırı yoktur, ancak hiyerarşi genellikle üç veya daha az düzey olduğunda daha kolay anlaşılır.

İş sözlüğünde hiyerarşilerin kullanımı isteğe bağlıdır. Sözlük terimleri için üst terim alanını boş bırakmak, sözlükte düz (hiyerarşik olmayan) terimler listesi oluşturur.  

## <a name="tagging-assets-with-glossary-terms"></a>Varlıkları sözlük terimleriyle etiketleme

Katalogda sözlük terimleri tanımlandıktan sonra, varlıkları etiketleme deneyimi, bir kullanıcı etiketi ni yazdıkça sözlüğü aramak için optimize edilmiştir. Veri Kataloğu portalı, aralarından seçim yapabileceğiniz eşleşen sözlük terimlerinin bir listesini görüntüler. Kullanıcı listeden bir sözlük terimi seçerse, terim kıymete etiket olarak eklenir (sözlük etiketi olarak da adlandırılır). Kullanıcı, sözlükte olmayan (kullanıcı etiketi olarak da adlandırılır) bir terim yazarak yeni bir etiket oluşturmayı da seçebilir.

![Veri varlığı bir kullanıcı etiketi ve iki sözlük etiketiyle etiketlendi](./media/data-catalog-how-to-business-glossary/03-tagged-asset.png)

> [!NOTE]
> Kullanıcı etiketleri, Veri Kataloğu'nun Ücretsiz Sürümü'nde desteklenen tek etiket türüdür.

### <a name="hover-behavior-on-tags"></a>Etiketlerde gezinme davranışı

Veri Kataloğu portalında, iki etiket türü görsel olarak farklıdır ve farklı gezinme davranışları sunar. Bir kullanıcı etiketinin üzerinde gezinirken, etiket metnini ve etiketi ekleyen kullanıcı veya kullanıcıları görebilirsiniz. Bir sözlük etiketinin üzerinde gezinirken, sözlük teriminin tanımını ve terimin tam tanımını görüntülemek için iş sözlüğünün açılmasını sağlayacak bir bağlantı da görürsünüz.

### <a name="search-filters-for-tags"></a>Etiketler için filtreleri ara

Sözlük etiketleri ve kullanıcı etiketleri her ikisi de aranabilir ve bunları bir aramada filtre olarak uygulayabilirsiniz.

## <a name="summary"></a>Özet

Azure Veri Kataloğu'ndaki iş sözlüğü ve sağladığı geçerli etiketlemeyi kullanarak veri varlıklarını tutarlı bir şekilde tanımlayabilir, yönetebilir ve keşfedebilirsiniz. İş sözlüğü, işletme sözlüğünün kuruluş üyeleri tarafından öğrenilen öğrenmeyi teşvik edebilir. Sözlük ayrıca varlık bulma ve anlama basitleştirir anlamlı meta veri yakalama destekler.

## <a name="next-steps"></a>Sonraki adımlar

* [İş sözlüğü işlemleri için REST API belgeleri](/rest/api/datacatalog/data-catalog-glossary)
