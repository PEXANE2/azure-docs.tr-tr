---
title: Azure Güvenlik kıyaslaması tanıtımı
description: Güvenlik kıyaslaması tanıtımı
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/16/2019
ms.author: mbaldwin
ms.custom: security-baselines
ms.openlocfilehash: 2d0cceca841a2aff2538dcf8763d245ca8cfd13d
ms.sourcegitcommit: 5925df3bcc362c8463b76af3f57c254148ac63e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2019
ms.locfileid: "75564367"
---
# <a name="azure-security-benchmarks-introduction"></a>Azure Güvenlik kıyaslamaları giriş

Şirket içi bilgi işlem ile ilgili birkaç yıla veya hatta deneyiminiz olabilir. Bu dağıtımların nasıl güvence altına alınacağını bilirsiniz. Ancak bulut farklıdır. Bulut dağıtımlarınızın güvenli olup olmadığını nasıl anlarsınız? Şirket içi sistemler ve bulut dağıtımları için güvenlik uygulamaları arasındaki farklar nelerdir?

Büyük bir Teknik İnceleme, en iyi uygulamalar, başvuru mimarileri, Web Kılavuzu, açık kaynaklı araçlar, ticari çözümler, zeka akışları ve daha fazlasını içeren büyük bir koleksiyon, bulutun güvenliğini sağlamaya yardımcı olmak için kullanılabilir. Hangi seçeneği kullanmalısınız? Bulutta kabul edilebilir bir güvenlik düzeyini almak için ne yapabilirsiniz? 

Bulut dağıtımlarınızın güvenliğini sağlamanın en iyi yöntemlerinden biri, bulut güvenliği kıyaslama önerilerine odaklanmasıdır. Tüm hizmetleri güvenli hale getirmenin kıyaslama önerileri, siber güvenlik riskini temel alarak ve nasıl yönetileceği ile başlar. Daha sonra, ortamınızdaki belirli güvenlik yapılandırması ayarlarını seçmenize yardımcı olması için, bulut hizmeti sağlayıcınızdan kıyaslama güvenlik önerilerini benimseerek bu anlama 'yı kullanabilirsiniz. 

Azure Güvenlik kıyaslaması, Azure 'da kullandığınız hizmetlerin çoğunu güvenli hale getirmeye yardımcı olmak için kullanabileceğiniz, yüksek etkili güvenlik önerileri koleksiyonu içerir. Bu önerileri, çoğu Azure hizmeti için geçerli olan "genel" veya "kuruluş" olarak düşünebilirsiniz. Azure Güvenlik kıyaslaması önerileri her bir Azure hizmeti için özelleştirilir ve bu özelleştirilmiş rehberlik hizmet önerileri makalelerinde bulunur. 

Azure Güvenlik kıyaslama belgeleri güvenlik denetimlerini ve hizmet önerilerini belirler.

- **Güvenlik denetimleri**: Azure Güvenlik kıyaslama önerileri güvenlik denetimlerine göre kategorize edilir. Güvenlik denetimleri, ağ güvenliği ve veri koruması gibi yüksek düzeyde satıcıya özgü güvenlik gereksinimlerini temsil eder. Her güvenlik denetiminin, bu önerileri etkinleştirmenize yardımcı olan bir dizi güvenlik önerisi ve yönergeleri vardır. 
- **Hizmet önerileri**: kullanılabilir olduğunda, Azure hizmetlerine yönelik kıyaslama önerileri, hizmet Için tasarlanmış Azure Güvenlik kıyaslama önerilerini ve ayrıca belirli hizmet için benzersiz olan ek önerileri içerir. 

"Denetim", "kıyaslama" ve "taban çizgisi" terimleri genellikle Azure Güvenlik kıyaslama belgelerinde kullanılır ve Azure 'un bu koşulları nasıl kullandığını anlamak önemlidir. 

| Sözleşme Dönemi | Açıklama | Örnek |
|--|--|--|
| Denetim | **Denetim** , bir özellik veya etkinliğin giderilmesi gereken ve bir teknoloji ya da uygulamaya özgü olmayan üst düzey bir açıklamadır. | Veri koruma, güvenlik denetimlerinden biridir. Bu denetim, verilerin korunduğundan emin olmak için giderilmesi gereken belirli eylemleri içerir. |
| Kıyaslama | Bir **kıyaslama** , Azure gibi belirli bir teknoloji için güvenlik önerileri içerir. Öneriler, ait oldukları denetime göre kategorize edilir. | Azure Güvenlik kıyaslaması, Azure platformuna özgü güvenlik önerilerini içerir  |
| Taban Çizgisi | **Temel** bir kuruluş için güvenlik gereksinimleridir. Güvenlik gereksinimleri, kıyaslama önerilerini temel alır. Her kuruluş, hangi kıyaslama önerilerinin taban çizgisine dahil edileceğini belirler. | Contoso şirketi, Azure Güvenlik kıyaslaması içinde belirli öneriler kullanılmasını seçerek Güvenlik temelini oluşturur. |

Azure Güvenlik kıyaslaması hakkında görüşlerinizi inceleyeceğiz! Aşağıdaki geri bildirim alanına yorum sağlamanızı öneririz. Girişinizi Azure Güvenlik kıyaslama ekibi ile daha özel olarak paylaşmayı tercih ediyorsanız, https://aka.ms/AzSecBenchmark formu doldurmaya hoş geldiniz 