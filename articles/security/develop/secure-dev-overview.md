---
title: Microsoft Azure'da güvenli geliştirme en iyi uygulamaları
description: Bulutta daha güvenli bir kod geliştirmenize ve daha güvenli bir uygulama dağıtmanıza yardımcı olacak en iyi uygulamalar.
author: TerryLanfear
manager: barbkess
ms.author: terrylan
ms.date: 06/11/2019
ms.topic: article
ms.service: security
ms.subservice: security-develop
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: c4314a0dcbbcb907ef4d6de0a2788cf04dfe1641
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68934860"
---
# <a name="secure-development-best-practices-on-azure"></a>Azure'da güvenli geliştirme en iyi uygulamaları
Bu makale serisi, bulut için uygulamalar geliştirirken göz önünde bulundurulması gereken güvenlik etkinlikleri ni ve denetimlerini sunar. Microsoft Güvenlik Geliştirme Yaşam Döngüsü 'nün (SDL) aşamaları ve yaşam döngüsünün her aşamasında göz önünde bulundurulması gereken güvenlik soruları ve kavramları ele alınır. Amaç, daha güvenli bir uygulama tasarlamak, geliştirmek ve dağıtmak için yaşam döngüsünün her aşamasında kullanabileceğiniz etkinlikleri ve Azure hizmetlerini tanımlamanıza yardımcı olmaktır.

Makalelerdeki öneriler Azure güvenliği yle ilgili deneyimlerimizden ve müşterilerimizin deneyimlerinden gelir. Bu makaleleri geliştirme projenizin belirli bir aşamasında göz önünde bulundurmanız gerekenler için bir referans olarak kullanabilirsiniz, ancak tüm makaleleri en az bir kez okumanızı öneririz. Tüm makaleleri okumak, projenizin önceki aşamalarında kaçırmış olabileceğiniz kavramları size sunar. Ürününüzü piyasaya sürmeden önce bu kavramları uygulamak, güvenli yazılım oluşturmanıza, güvenlik uyumluluğu gereksinimlerini gidermenize ve geliştirme maliyetlerini azaltmanıza yardımcı olabilir.

Bu makaleler, güvenli Azure uygulamaları oluşturan ve dağıtan her düzeydeki yazılım tasarımcıları, geliştiriciler ve sınayıcılar için bir kaynak olarak tasarlanmıştır.

## <a name="overview"></a>Genel Bakış

Güvenlik herhangi bir uygulamanın en önemli yönlerinden biridir ve doğru almak için basit bir şey değil. Neyse ki Azure, uygulamanızı bulutta güvenli hale almanıza yardımcı olabilecek birçok hizmet sunar. Bu makaleler, daha güvenli kod geliştirmenize ve bulutta daha güvenli bir uygulama dağıtmanıza yardımcı olmak için yazılım geliştirme yaşam döngünüzün her aşamasında uygulayabileceğiniz etkinlikleri ve Azure hizmetlerini ele aşar.

## <a name="security-development-lifecycle"></a>Güvenlik geliştirme yaşam döngüsü

Güvenli yazılım geliştirme için en iyi uygulamaları takip etmek, gereksinim analizinden bakıma kadar, proje metodolojisi[(şelale,](https://en.wikipedia.org/wiki/Waterfall_model) [çevik](https://en.wikipedia.org/wiki/Agile_software_development), veya [DevOps)](https://en.wikipedia.org/wiki/DevOps)ne olursa olsun, yazılımın geliştirme yaşam döngüsünün her aşamasına güvenliği entegre etmeyi gerektirir. Yüksek profilli veri ihlalleri ve operasyonel güvenlik kusurlarının sömürülmesi sonrasında, daha fazla geliştirici güvenlik geliştirme süreci boyunca ele alınması gerektiğini anlayış vardır.

Geliştirme yaşam döngünüzdeki bir sorunu ne kadar geç giderirseniz, bu düzeltme size o kadar pahalıya mal olur. Güvenlik sorunları bir istisna değildir. Yazılım geliştirmenin ilk aşamalarındaki güvenlik sorunlarını göz ardı ederseniz, izleyen her aşama önceki aşamanın güvenlik açıklarını devralabilir. Nihai ürününnüzde birden fazla güvenlik sorunu ve ihlal olasılığı olacaktır. Geliştirme yaşam döngüsünün her aşamasına güvenlik oluşturmak sorunları erken yakalamanıza ve geliştirme maliyetlerinizi azaltmanıza yardımcı olur.

Kullanım ömrünün her aşamasında güvenli yazılım geliştirme uygulamalarını yerine getirmek için kullanabileceğiniz etkinlikleri ve Azure hizmetlerini tanıtmak için Microsoft [Güvenlik Geliştirme Yaşam Döngüsü'nün (SDL)](https://msdn.microsoft.com/library/windows/desktop/84aed186-1d75-4366-8e61-8d258746bopq.aspx) aşamalarını takip ediyoruz.

SDL aşamaları şunlardır:

  - Eğitim
  - Gereksinimler
  - Tasarım
  - Uygulama
  - Doğrulama
  - Yayınla
  - Yanıt

![Güvenlik Geliştirme Yaşam Döngüsü](./media/secure-dev-overview/01-sdl-phase.png)

Bu makalelerde SDL aşamalarını tasarım, geliştirme ve dağıtma olarak gruplandırıyoruz.

## <a name="engage-your-organizations-security-team"></a>Kuruluşunuzun güvenlik ekibini meşgul edin

Kuruluşunuzun geliştirme yaşam döngüsü sırasında başından sonuna kadar güvenlik etkinliklerinde size yardımcı olan resmi bir uygulama güvenlik programı olabilir. Kuruluşunuzun güvenlik ve uyumluluk ekipleri varsa, uygulamanızı geliştirmeye başlamadan önce bu ekiplerle etkileşime girdiğinizden emin olun. SDL'nin her aşamasında kaçırdığınız görevler olup olmadığını sorun.

Birçok okuyucunun etkileşimde bulunabilecek bir güvenlik veya uyumluluk ekibi olmadığını biliyoruz. Bu makaleler, SDL'nin her aşamasında göz önünde bulundurmanız gereken güvenlik soru ve kararlarında size rehberlik edebilir.

## <a name="resources"></a>Kaynaklar

Güvenli uygulamalar geliştirme hakkında daha fazla bilgi edinmek ve Azure'daki uygulamalarınızın güvenliğini sağlamaya yardımcı olmak için aşağıdaki kaynakları kullanın:

[Microsoft Security Development Lifecycle (SDL)](https://msdn.microsoft.com/library/windows/desktop/84aed186-1d75-4366-8e61-8d258746bopq.aspx) – SDL, Microsoft'un geliştiricilerin daha güvenli yazılımlar oluşturmasına yardımcı olan bir yazılım geliştirme sürecidir. Geliştirme maliyetlerini azaltırken güvenlik uyumluluğu gereksinimlerini karşılamanıza yardımcı olur.

[Açık Web Uygulama Güvenliği Projesi (OWASP)](https://www.owasp.org/index.php/Main_Page) - OWASP, web uygulama güvenliği alanında serbestçe kullanılabilen makaleler, metodolojiler, belgeler, araçlar ve teknolojiler üreten çevrimiçi bir topluluktur.

[Push Left, Like a Boss](https://code.likeagirl.io/pushing-left-like-a-boss-part-1-80f1f007da95?WT.mc_id=docs-blog-tajanca) – Geliştiricilerin daha güvenli kod oluşturmak için tamamlamaları gereken farklı uygulama güvenlik etkinlikleri türlerini özetleyen bir dizi çevrimiçi makale.

[Microsoft kimlik platformu](../../active-directory/develop/index.yml) – Microsoft kimlik platformu, Azure AD kimlik hizmeti ve geliştirici platformunun bir evrimidir. Kimlik doğrulama hizmeti, açık kaynak kitaplıkları, uygulama kaydı ve yapılandırması, tam geliştirici belgeleri, kod örnekleri ve diğer geliştirici içeriğinden oluşan tam özellikli bir platformdur. Microsoft kimlik platformu, OAuth 2.0 ve OpenID Connect gibi endüstri standardı protokolleri destekler.

[Azure çözümleri için güvenlik en iyi uygulamaları](https://azure.microsoft.com/resources/security-best-practices-for-azure-solutions/) – Azure'u kullanarak bulut çözümleri tasarlarken, dağıtırken ve yönetirken kullanılacak en iyi güvenlik uygulamaları koleksiyonu. Bu makale, BT profesyonelleri için bir kaynak olması amaçlanmıştır. Bu, güvenli Azure çözümleri geliştiren ve dağıtan tasarımcıları, mimarları, geliştiricileri ve sınayıcıları içerebilir.

[Azure'da Güvenlik ve Uyumluluk Planları](https://servicetrust.microsoft.com/ViewPage/BlueprintOverview) – Azure Güvenlik ve Uyumluluk Planları, sıkı düzenlemelere ve standartlara uygun bulut destekli uygulamalar oluşturmanıza ve başlatmanıza yardımcı olabilecek kaynaklardır.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelerde, güvenli uygulamalar tasarlamanıza, geliştirmenize ve dağıtmanıza yardımcı olabilecek güvenlik denetimleri ve etkinlikleri öneririz.

- [Güvenli uygulamalar tasarla](secure-design.md)
- [Güvenli uygulamalar geliştirme](secure-develop.md)
- [Güvenli uygulamaları dağıtma](secure-deploy.md)
