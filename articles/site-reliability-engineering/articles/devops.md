---
title: 'SSS: SRE ve DevOps | Microsoft Docs'
titleSuffix: Azure
description: 'SSS: SRE ve DevOps arasındaki ilişkiyi anlama'
author: dnblankedelman
manager: ScottCa
ms.service: site-reliability-engineering
ms.topic: article
ms.date: 09/14/2020
ms.author: dnb
ms.openlocfilehash: 3d0698f2780a4ccc41eedbde70fddea1766c5f21
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90090338"
---
# <a name="frequently-asked-questions-whats-the-relationship-between-sre-and-devops"></a>Sık sorulan sorular: SRE ve DevOps arasındaki ilişki nedir?

Site güvenilirliği Mühendisliği ile DevOps arasındaki ilişkiyi kapsayan yaygın soruların bir kümesi var. Bunların birbirinden farkı nedir? Hem Kuruluşumuzdan "olabilir. Bu makale, bu ilişkiyi kavramak için daha yakın olan SRE ve DevOps toplulukları tarafından sunulan bazı yanıtları paylaşmaya çalışır.

## <a name="how-are-they-the-same"></a>Nasıl aynıdır?

SRE ve DevOps, dahil edilen güçlüklere yanıt olarak oluşturulan ve geliştirilen modern işlemler uygulamalardır:

- üretim ortamlarımızın ve geliştirme işlemlerimizin büyümekte olan karmaşıklığı
- işletmelerin bu ortamların sürekli çalışmasına olan bağımlılığının artması
- iş gücünü bu ortamların boyutuyla doğrusal olarak ölçeklendirmenin mümkün olmaması
- Hala işletimsel kararlılığı korurken daha hızlı hareket etmeniz gerekir

Her iki işlem de, izleme/Observability, otomasyon, belgeler ve işbirliğine dayalı yazılım geliştirme araçları gibi bu güçlükler için önemli olan konularla ilgili değer dikkatini sağlar.

Araçları ve iş alanlarında SRE ve DevOps arasındaki önemli bir çakışma vardır. _Site güvenilirlik çalışma kitabı_ , "DevOps ile aynı şekilde, ancak biraz farklı nedenlerle" düşündüğü için, "

## <a name="three-different-ways-to-compare-the-two-operations-practices"></a>İki işlem uygulamalarını karşılaştırmak için üç farklı yol

SRE ve DevOps arasındaki benzerlikler net. Bu, iki farklı veya ayrılmış bir şekilde oldukça ilgi çekici bir deneyimdir. Burada, bu soruya yönelik bazı numan getirmenin bir yolu olarak ilişkilerini düşünmek için üç yol sunuyoruz. Bu yanıtları kabul etmeyebilirsiniz, ancak her biri tartışmaya yönelik iyi bir başlangıç yeri sağlar.

### <a name="class-sre-implements-interface-devops"></a>"sınıf SRE arabirimini uygular DevOps"

_Site güvenilirlik çalışma kitabı_ ( [kaynak defteri listemize](../resources/books.md)bahsedildiğinde), ilk bölümünde SRE ve DevOps 'ı ele alır. Bu bölümde, alt başlığı olarak "Class SRE Implements interface DevOps" ifadesi kullanılmaktadır. Bu, bir DevOps felsebunun belirli bir uygulamasına göz önünde bulundurılanabileceğinden, önerilmesi (geliştiricilere hedeflenmiş bir tümcecik kullanarak) için tasarlanmıştır. Bölümün gösterdiği gibi, "DevOps, işlemleri ayrıntılı bir düzeyde çalıştırma konusunda görece sessizdir" Ancak SRE, uygulamalarında önemli ölçüde daha fazla zaman alır. Bu nedenle, iki ilgisi, DevOps 'un birçok olası uygulamalarından biri olarak kabul edilebilir.

### <a name="sre-is-to-reliability-as-devops-is-to-delivery"></a>DevOps teslim edilmesiyle SRE güvenilirlik

Hem SRE hem de DevOps için birden çok tanım olduğundan, bu karşılaştırma biraz zaman alabilir, ancak hala faydalı olabilir. "Her bir işlemin, temel nedenini yansıtan bir veya iki kelimeyle karşılaşmak zorunda kaldıysanız, ne olacak?" sorusu ile başlar.

[Site güvenilirlik mühendisliği hub 'ında](../index.yml)SRE 'nin bu tanımını kullandığımızda:

> Site Güvenilirliği Mühendisliği kuruluşun sistemlerinde, hizmetlerinde ve ürünlerinde uygun güvenilirlik düzeyine sürdürülebilir bir şekilde ulaşmasına yardımcı olmaya adanmış bir mühendislik disiplinidir.

daha sonra, SRE için Word 'ün "güvenilirlik" olduğunu söylemek kolay bir işlemdir. Adın ortasında doğru olması, bu talep için bazı harika bir kanıt de sunmaktadır.

[Azure DevOps kaynak merkezi](https://docs.microsoft.com/azure/devops/learn/)'Nden bu DevOps tanımını kullandığımızda:

> DevOps, son kullanıcılarımıza sürekli değer sunulmasını sağlamak için tasarlanmış kişilerin, süreçlerin ve ürünlerin birleşimidir.

DevOps için benzer bir işlem "teslim" olabilir.

Bu nedenle "SRE, DevOps 'ın tesliminde olduğu kadar güvenilirlik sağlamak".

### <a name="direction-of-attention"></a>İlgilenme yönü

Bu yanıt, Thomas Limoncelli tarafından verilen bir katkıdan, [kaynak defteri listemize](../resources/books.md)bahsedilen _arama SRE_ defterine kadar tırnaklı veya biraz paraphrased. DevOps mühendislerinin büyük ölçüde üretim işlemleri sorumluluklarına sahip yazılım geliştirme yaşam döngüsü ardışık düzenine odaklanıldığı ve SREs, zaman içinde SDLC ardışık düzen sorumluluklarıyla üretim işlemlerine odaklanmaktadır.

Ancak daha da önemlisi, ayrıca bir tarafta yazılım geliştirme işlemiyle başlayan bir diyagram çizer ve üretim işlemleri diğeri üzerinde çalışır. İkisi, bir geliştiriciden kodu almak için oluşturulan her zamanki işlem hattı tarafından bağlanır, istenen sayıda test ve aşama aracılığıyla bunu yapın ve ardından bu kodu üretime taşıyın.

DevOps mühendislerinin geliştirme ortamında başlayıp üretim aşamasındaki adımları otomatikleştirdiği limoncelli notları. Tamamlandıktan sonra, performans sorunlarını iyileştirmek için geri döner.

SREs, diğer yandan üretim işlemlerine odaklanın ve nihai sonucu geliştirme (temelde ters yönde çalışan) bir yol olarak işlem hattına çok daha derin ulaşın.

Bu fark, SRE ve DevOps odağının yönlerine göre birbirinden ayırt edilmesine yardımcı olabilir.

## <a name="coexistence-in-the-same-organization"></a>Aynı kuruluşta birlikte bulunma

Ele almak istediğimiz son soru "aynı kuruluşta hem SRE hem de DevOps olabilir mi?"

Bu sorunun yanıtı bir "Yes!" emphatic.

Önceki yanıtların, iki işlem ile nasıl çakışma olduğunu ve çakışmayan ne zaman, bu uygulamaların odaklanmadan nasıl avantajlarına nasıl bir fikir suntığınızı umuyoruz. DevOps uygulamalarına sahip kuruluşlar, SRE pozisyonları veya takımları oluşturmak için yürütmeye gerek kalmadan küçük ölçekte (örneğin, SLIs ve SLOs 'ler), SRE uygulamalarından birini deneyebilir. Bu, oldukça yaygın bir SRE benimseme modelidir.

## <a name="next-steps"></a>Sonraki Adımlar

Site güvenilirliği Mühendisliği veya DevOps hakkında daha fazla bilgi edinmek mi istiyorsunuz? [Site güvenilirlik mühendisliği hub 'ını](../index.yml) ve [Azure DevOps kaynak merkezi](https://docs.microsoft.com/azure/devops/learn/)' ne göz atın.
