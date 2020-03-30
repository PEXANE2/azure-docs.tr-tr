---
title: Azure Güvenlik Karşılaştırması’na Giriş
description: Güvenlik Kıyaslama giriş
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/16/2019
ms.author: mbaldwin
ms.custom: security-baselines
ms.openlocfilehash: 8f65eb008d5df6ea3f2f85778d538023e6ce98eb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75945328"
---
# <a name="azure-security-benchmark-introduction"></a>Azure güvenlik kıyaslama girişi

Şirket içi bilgi işlem konusunda birkaç yıl, hatta onlarca yıllık deneyime sahip olabilirsiniz. Bu konuşlandırmaları nasıl güvence altına alabildiğinizi biliyorsunuz; ama bulut farklı. Bulut dağıtımlarınızın güvenli olup olmadığını nasıl anlarsınız? Şirket içi sistemler ve bulut dağıtımları için güvenlik uygulamaları arasındaki farklar nelerdir?

Bulutun güvenliğini sağlamaya yardımcı olmak için kullanılabilecek büyük bir beyaz kağıt koleksiyonu, en iyi uygulamalar, referans mimarileri, web rehberliği, açık kaynak araçları, ticari çözümler, istihbarat akışları ve daha fazlası vardır. Hangi seçeneği kullanmalısınız? Bulutta kabul edilebilir düzeyde güvenlik elde etmek için ne yapabilirsiniz? 

Bulut dağıtımlarınızı güvence altına almanın en iyi yollarından biri, bulut güvenliği kıyaslama önerilerine odaklanmaktır. Herhangi bir hizmeti güvence altına almak için kıyaslama önerileri, siber güvenlik riski ve nasıl yönetilmeye ilişkin temel bir anlayışla başlar. Daha sonra, ortamınızdaki belirli güvenlik yapılandırma ayarlarını seçmenize yardımcı olmak için bulut hizmeti sağlayıcınızdan kıyaslama güvenlik önerileri benimseyerek bu anlayışı kullanabilirsiniz. 

Azure Güvenlik Kıyaslaması, Azure'da kullandığınız hizmetlerin çoğunun güvenliğini sağlamaya yardımcı olmak için kullanabileceğiniz yüksek etkili güvenlik önerileri koleksiyonunu içerir. Çoğu Azure hizmeti için geçerli olduğu için bu önerileri "genel" veya "kuruluş" olarak düşünebilirsiniz. Azure Güvenlik Kıyaslama önerileri daha sonra her Azure hizmeti için özelleştirilmiştir ve bu özelleştirilmiş kılavuz hizmet önerileri makalelerinde bulunur. 

Azure Güvenlik Kıyaslama belgeleri güvenlik denetimlerini ve hizmet önerilerini belirtir.

- **Güvenlik Denetimleri**: Azure Güvenlik Kıyaslama önerileri güvenlik denetimlerine göre sınıflandırılır. Güvenlik denetimleri ağ güvenliği ve veri koruması gibi üst düzey satıcı-agnostik güvenlik gereksinimlerini temsil eder. Her güvenlik denetiminde, bu önerileri uygulamanıza yardımcı olan bir dizi güvenlik önerisi ve yönergesi vardır. 
- **Hizmet Önerileri**: Kullanılabilir olduğunda, Azure hizmetleri için kıyaslama önerileri, söz söz le özel olarak özel olarak hazırlanmış Azure Güvenlik Kıyaslama önerilerini içerir. 

"Denetim", "Kıyaslama" ve "Temel" terimleri Azure Güvenlik Kıyaslama belgelerinde sıklıkla kullanılır ve Azure'un bu terimleri nasıl kullandığını anlamak önemlidir. 

| Sözleşme Dönemi | Açıklama | Örnek |
|--|--|--|
| Denetim | **Denetim,** ele alınması gereken ve bir teknolojiye veya uygulamaya özgü olmayan bir özelliğin veya etkinliğin üst düzey bir açıklamasıdır. | Veri Koruma, güvenlik denetimlerinden biridir. Bu denetim, verilerin korunmasını sağlamaya yardımcı olmak için ele alınması gereken belirli eylemler içerir. |
| Kıyaslama | Bir **kıyaslama,** Azure gibi belirli bir teknoloji için güvenlik önerileri içerir. Öneriler, ait oldukları denetime göre sınıflandırılır. | Azure Güvenlik ölçüt, Azure platformuna özgü güvenlik önerilerini içerir  |
| Taban çizgisi | **Temel,** bir kuruluşun güvenlik gereksinimleridir. Güvenlik gereksinimleri kıyaslama önerilerine dayanır. Her kuruluş, hangi kıyaslama önerilerinin kendi taban çizgisine dahil edilene karar verir. | Contoso şirketi, Azure Güvenlik Kıyaslaması'nda belirli öneriler gerektirmeyi seçerek güvenlik taban çizgisini oluşturur. |

Azure Güvenlik Kıyaslama'sı hakkındaki görüşlerinizi bekliyoruz! Aşağıdaki geri bildirim alanında yorum larınızı sunmanızı öneririz. Girişinizi Azure Güvenlik Kıyaslama ekibiyle daha özel olarak paylaşmayı tercih ederseniz, formuhttps://aka.ms/AzSecBenchmark 
