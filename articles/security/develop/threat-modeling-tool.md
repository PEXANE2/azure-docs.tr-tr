---
title: Microsoft Tehdit Modelleme Aracına genel bakış - Azure
description: Tehdit Modelleme işlemi de dahil olmak üzere, araçla işe başlamayla ilgili bilgiler içeren Microsoft Tehdit Modelleme Aracına genel bakış.
author: jegeib
ms.author: jegeib
ms.service: security
ms.subservice: security-develop
ms.topic: article
ms.date: 02/16/2017
ms.openlocfilehash: 0d800102b6f6ff77944a2b625d3bcecef69c1ee9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75548758"
---
# <a name="microsoft-threat-modeling-tool"></a>Microsoft Tehdit Modelleme Aracı

Tehdit Modelleme Aracı, Microsoft Güvenlik Geliştirme Yaşam Döngüsü'nün (SDL) temel bir öğesidir. Yazılım mimarlarının, çözümlenebilmek için nispeten kolay ve uygun maliyetli olduklarında olası güvenlik sorunlarını erkenden tanımlamasına ve azaltmasına olanak tanır. Sonuç olarak, büyük ölçüde geliştirme toplam maliyetini azaltır. Ayrıca, tehdit modelleri oluşturma ve analiz etme konusunda net rehberlik sağlayarak tehdit modellemeyi tüm geliştiriciler için daha kolay hale getirerek aracı güvenlik dışı uzmanları göz önünde bulundurarak tasarladık. 

Araç, herkesin aşağıdakileri yapabilmesini sağlar:

* Sistemlerinin güvenlik tasarımı hakkında iletişim kurun
* Kanıtlanmış bir metodoloji kullanarak olası güvenlik sorunları için bu tasarımları analiz edin
* Güvenlik sorunları için azaltıcı etkenler önerme ve yönetme

Burada bazı takım yetenekleri ve yenilikler, sadece birkaç isim şunlardır:

* **Otomasyon:** Bir model çiziminde rehberlik ve geri bildirim
* **Eleman başına STRIDE:** Tehditlerin ve azaltımların güdümlü analizi
* **Raporlama:** Doğrulama aşamasında güvenlik faaliyetleri ve testler
* **Benzersiz Metodoloji:** Kullanıcıların tehditleri daha iyi görselleştirmesini ve anlamasını sağlar
* **Geliştiriciler için tasarlanmış ve Yazılım merkezli:** birçok yaklaşım varlıklar veya saldırganlar üzerinde ortalanır. Biz yazılım merkezli. Tüm yazılım geliştiricilerin ve mimarların aşina olduğu etkinlikler üzerine inşa ediyoruz -- örneğin yazılım mimarileri için resim çizmek gibi.
* **Tasarım Analizine Odaklanmış:** "Tehdit modelleme" terimi gereksinimlere veya tasarım çözümleme tekniğine atıfta bulunabilir. Bazen, bu iki karmaşık bir karışımı anlamına gelir. Tehdit modellemeye Microsoft SDL yaklaşımı odaklı bir tasarım analizi tekniğidir

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki tablo, Tehdit Modelleme Aracı ile başlamak için önemli bağlantılar içerir:

| Adım  | Açıklama                                                                                   |
| ----- | --------------------------------------------------------------------------------------------- |
| **1** | [Tehdit Modelleme Aracını İndirin](https://aka.ms/threatmodelingtool)                                |
| **2** | [Başlangıç kılavuzumuzu okuyun](threat-modeling-tool-getting-started.md)    |
| **3** | [Özellikleri tanıma](threat-modeling-tool-feature-overview.md)   |
| **4** | [Oluşturulan tehdit kategorileri hakkında bilgi edinin](threat-modeling-tool-threats.md)   |
| **5** | [Oluşturulan tehditlere yönelik azaltıcı etkenleri bulma](threat-modeling-tool-mitigations.md) |

## <a name="resources"></a>Kaynaklar

Burada hala tehdit modelleme bugün ilgili birkaç eski makaleler şunlardır:

* [Tehdit Modellemenin Önemi Hakkında Makale](https://docs.microsoft.com/archive/msdn-magazine/2009/january/security-briefs-getting-started-with-the-sdl-threat-modeling-tool)
* [Eğitim Trustworthy Computing tarafından yayınlandı](https://www.microsoft.com/download/details.aspx?id=16420)

Birkaç Tehdit Modelleme Aracı uzmanlarının neler yaptığına göz atın:

* [Tehditler Yöneticisi](https://simoneonsecurity.com/threatsmanagersetup-v1-5-10/)
* [Simone Curzi Güvenlik Blog](https://simoneonsecurity.com/)
