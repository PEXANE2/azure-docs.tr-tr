---
title: Azure Güvenlik Karşılaştırması’na Giriş
description: Güvenlik kıyaslaması tanıtımı
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/16/2019
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 53473b6be9e91adb636c3c9528e97ec644616115
ms.sourcegitcommit: 94c750edd4d755d6ecee50ac977328098a277479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2020
ms.locfileid: "90058623"
---
# <a name="azure-security-benchmark-introduction"></a>Azure Güvenlik kıyaslaması tanıtımı

Yeni hizmetler ve Özellikler Azure 'da günlük olarak yayımlanır, geliştiriciler bu hizmetlerde oluşturulan yeni bulut uygulamalarını hızlıca yayımlamakta ve saldırganlar, yanlış yapılandırılmış kaynaklardan faydalanan yeni yollar aramaktadır. Bulut hızlı taşınır, geliştiriciler hızlı bir şekilde hareket eder ve saldırganlar her zaman taşınır. Nasıl devam edersiniz ve bulut dağıtımlarınızın güvende olduğundan emin misiniz? Bulut sistemlerine yönelik güvenlik uygulamaları, şirket içi sistemlerden farklı midir? Birçok bağımsız geliştirme takımı genelinde tutarlılığı nasıl izlersiniz?

Microsoft, *güvenlik kıyaslamaları* kullanmanın bulut dağıtımlarını hızlı bir şekilde korumanıza yardımcı olabilir. Bulut hizmeti sağlayıcınızdan alınan kıyaslama önerileri, ortamınızda belirli güvenlik yapılandırma ayarlarını seçmeye ve kuruluşunuzun riskini hızla azaltmanıza olanak sağlayan bir başlangıç noktası sağlar.

Azure Güvenlik kıyaslaması, Azure 'da kullandığınız hizmetlerin güvenliğini sağlamaya yardımcı olmak için kullanabileceğiniz yüksek etkili güvenlik önerileri koleksiyonu içerir:

- Güvenlik denetimleri: Bu öneriler, genellikle Azure kiracınızda ve Azure hizmetleriniz arasında geçerlidir. Her öneri, genellikle kıyaslamanın planlanmasına, onayına veya uygulanmasına dahil olan paydaşların bir listesini tanımlar. 
- Hizmet temelleri: Bu hizmetin güvenlik yapılandırmasına yönelik öneriler sağlamak için denetimleri tek tek Azure hizmetlerine uygular.

## <a name="implement-the-azure-security-benchmark"></a>Azure Güvenlik kıyaslaması 'nı uygulama
- Denetim çatısını planlamak için kurumsal denetimlerin ve hizmete özel temellerin [belgelerini](overview.md) Inceleyerek ve CIS (Controls v 7.1) ve NıST (SP800-53) çerçevesi gibi bir kılavuza nasıl eşlendiğini Inceleyerek Azure Güvenlik kıyaslama uygulamanızı **planlayın** .
- Azure Güvenlik Merkezi [mevzuat uyumluluk panosunu](../../security-center/security-center-compliance-dashboard.md)kullanarak Azure güvenliği kıyaslama durumu (ve diğer denetim kümeleri) ile uyumluluğunuzu **izleyin** .
- Azure şemaları ve Azure Ilkesi ile güvenli yapılandırmaların otomatikleştirilmesi ve Azure Güvenlik kıyaslamaları (ve kuruluşunuzdaki diğer gereksinimler) ile uyumluluğu zorlamak için **guardrayları oluşturun** .
 
Azure Güvenlik kıyaslamasının Microsoft 'un Azure Güvenlik önerilerinin tek birleştirilmiş bir görünümünü sağlamak için Azure Güvenlik kıyaslaması v2 'nin [Microsoft Security En Iyi uygulamaları](/security/compass/microsoft-security-compass-introduction) (eski adıyla Azure Güvenlik pusula) ile hizalandığını unutmayın.

## <a name="common-use-cases"></a>Genel Kullanım Örnekleri

Azure Güvenlik kıyaslaması, müşteriler veya hizmet iş ortakları için şu yaygın zorlukları karşılamak üzere sık kullanılır:
- Azure 'da yeni ve güvenli bir dağıtım sağlamak için en iyi güvenlik uygulamalarını arıyor.
- En iyi riskleri ve azaltmaları önceliklendirmek için mevcut Azure dağıtımlarının güvenlik duruşunu artırma.
- Azure hizmetlerini teknoloji ve işletme tarafından kullanılmak üzere, belirli güvenlik kılavuzlarını karşılamak üzere onaylama.
- Devlet veya yüksek düzeyde düzenlenen, finans ve sağlık (veya bu müşteriler için sistem derlemesi gereken hizmet satıcıları) gibi sektörlerden oluşan müşteriler için mevzuat gereksinimlerini karşılaın. Bu müşterilerin yapılandırmalarını, CIS, NıST veya PCI gibi bir sektör çerçevesinde belirtilen güvenlik yeteneklerini karşıladığından emin olunması gerekir. Azure Güvenlik kıyaslaması, bu sektör kıyaslamalarıyla önceden eşlenmiş denetimlerle etkili bir yaklaşım sağlar.

## <a name="terminology"></a>Terminoloji

"Denetim", "kıyaslama" ve "taban çizgisi" terimleri genellikle Azure Güvenlik kıyaslama belgelerinde kullanılır ve Azure 'un bu koşulları nasıl kullandığını anlamak önemlidir.


| Süre | Açıklama | Örnek |
|--|--|--|
| Denetim | Denetim, bir özellik veya etkinliğin giderilmesi ve bir teknoloji ya da uygulamaya özgü olmaması gereken üst düzey bir açıklamadır. | Veri koruma, güvenlik denetimlerinden biridir. Bu denetim, verilerin korunduğundan emin olmak için giderilmesi gereken belirli eylemleri içerir. |
| Kıyaslama | Bir kıyaslama, Azure gibi belirli bir teknoloji için güvenlik önerileri içerir. Öneriler, ait oldukları denetime göre kategorize edilir. | Azure Güvenlik kıyaslaması, Azure platformuna özgü güvenlik önerilerini içerir |
| Taban çizgisi | Bir taban çizgisi, tek bir Azure hizmetindeki kıyaslama uygulamasıdır. Her kuruluş, Azure Uygulama kapsamında kıyaslama önerisine ve ilgili yapılandırmaların gerekli olduğuna karar verir. | Contoso şirketi, Azure SQL güvenlik temeli 'nde önerilen yapılandırmayı izleyerek Azure SQL güvenlik özelliklerinin etkinleştirilmesini sağlar.

Azure Güvenlik kıyaslaması hakkında görüşlerinizi inceleyeceğiz! Aşağıdaki geri bildirim alanına yorum sağlamanızı öneririz. Girişinizi Azure Güvenlik kıyaslama ekibi ile daha özel olarak paylaşmayı tercih ediyorsanız, formu şu adreste doldurmaya hoş geldiniz: https://aka.ms/AzSecBenchmark
