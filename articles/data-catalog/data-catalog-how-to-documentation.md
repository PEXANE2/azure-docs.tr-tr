---
title: Azure Veri Kataloğu'nda veri kaynakları nasıl belgelene
description: Azure Veri Kataloğu'ndaki veri varlıklarının nasıl belgelenebildiğini vurgulayan nasıl bir makale.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: e9e9013d354585d04f205feb93a84d94c0f05905
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68950198"
---
# <a name="how-to-document-data-sources-in-azure-data-catalog"></a>Azure Veri Kataloğu'nda veri kaynakları nasıl belgelene

## <a name="introduction"></a>Giriş
**Microsoft Azure Veri Kataloğu,** kurumsal veri kaynakları için bir kayıt ve keşif sistemi olarak hizmet veren tam olarak yönetilen bir bulut hizmetidir. Başka bir deyişle, **Azure Veri Kataloğu** tamamen insanların veri kaynaklarını keşfetmesine, *anlamasına*ve kullanmasına ve kuruluşların mevcut verilerinden daha fazla değer elde etmesine yardımcı olmak için önemlidir.

Bir veri kaynağı **Azure Veri Kataloğu'na**kaydedildiğinde, meta verileri hizmet tarafından kopyalanır ve dizine eklenir, ancak hikaye burada bitmez. **Azure Veri Kataloğu** ayrıca, kullanıcıların veri kaynağının kullanımını ve yaygın senaryolarını açıklayabilecek kendi tüm belgelerini sağlamalarına olanak tanır.

[Veri kaynaklarına açıklama ekleme de,](data-catalog-how-to-annotate.md)veri kaynağını bilen uzmanların bu açıklamaya etiketler ve açıklamayla ek açıklama ekleyebilirlerini öğrenirsiniz. **Azure Veri Kataloğu** portalı, kullanıcıların veri varlıklarını ve kapsayıcılarını tam olarak belgeleyebilmeleri için zengin bir metin düzenleyicisi içerir. Düzenleyici, başlıklar, metin biçimlendirme, madde işaretli listeler, numaralanmış listeler ve tablolar gibi paragraf biçimlendirmesini içerir.

Etiketler ve açıklamalar basit ek açıklamalar için harikadır. Ancak, veri tüketicilerinin bir veri kaynağının kullanımını ve bir veri kaynağı için iş senaryolarını daha iyi anlamalarına yardımcı olmak için, bir uzman eksiksiz, ayrıntılı belgeler sağlayabilir. Bir veri kaynağını belgelemek kolaydır. Bir veri kıymeti veya kapsayıcı seçin ve **Belgeler'i**seçin.

![Veri Kataloğundaki Belgeler sekmesi](media/data-catalog-documentation/data-catalog-documentation.png)

## <a name="documenting-data-assets"></a>Veri varlıklarını belgeleme
**Azure Veri Kataloğu** belgelerinin avantajı, veri varlıklarınızın tam bir anlatısını oluşturmak için Veri Kataloğunuzu içerik deposu olarak kullanmanıza olanak tanır. Kapsayıcıları ve tabloları açıklayan ayrıntılı içeriği keşfedebilirsiniz. SharePoint veya dosya paylaşımı gibi başka bir içerik deposunda zaten içeriğiniz varsa, varolan bu içeriğe başvurmak için varlık belgeleri bağlantılarına ekleyebilirsiniz. Bu özellik, varolan belgelerinizi daha kullanılabilir hale getirir.

> [!NOTE]
> Belgeler arama dizinine dahil edilmez.
>

![Dokümantasyon sekmesi ve web bağlantısına köprü](media/data-catalog-documentation/data-catalog-documentation2.png)

Belge düzeyi, bir veri varlık kapsayıcısının özelliklerini ve değerini açıklamaktan, bir kapsayıcı içindeki tablo şemasının ayrıntılı bir açıklamasına kadar değişebilir. Sağlanan belgelerin düzeyi iş gereksinimlerinize göre yönlendirilmelidir. Ama genel olarak, burada veri varlıkları belgeleyen birkaç artıları ve eksileri şunlardır:

* Yalnızca bir kapsayıcıyı belgeleyin: Tüm içerik tek bir yerdedir, ancak kullanıcıların bilinçli bir karar verememesi için gerekli ayrıntılardan yoksun olabilir.
* Yalnızca tabloları belgele: İçerik o nesneye özgüdür, ancak kullanıcılarınızın belgeler için birden çok yeri vardır.
* Belge kapsayıcıları ve tabloları: En kapsamlı yaklaşım, ancak belgelerin daha fazla bakım tanıtmak olabilir.

## <a name="summary"></a>Özet
Veri kaynaklarını **Azure Veri Kataloğu** ile belgelemek, veri varlıklarınız hakkında ihtiyacınız olduğu kadar ayrıntılı bir anlatım oluşturabilir.  Bağlantıları kullanarak, varolan dokümanlarınızı ve veri varlıklarınızı bir araya getiren varolan bir içerik deposunda depolanan içeriğe bağlantı verebilirsiniz. Kullanıcılarınız uygun veri varlıklarını keşfettikten sonra, tam bir belge kümesine sahip olabilirler.
