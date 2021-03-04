---
title: Azure Güvenlik Merkezi 'nde güvenlik ilkelerini, girişimleri ve önerileri anlama
description: Azure Güvenlik Merkezi 'nde güvenlik ilkeleri, girişimler ve öneriler hakkında bilgi edinin.
author: memildin
ms.author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 02/28/2021
ms.openlocfilehash: a5aae4013067ba37334e3e85a9e7ef882efd1dd2
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102107932"
---
# <a name="what-are-security-policies-initiatives-and-recommendations"></a>Güvenlik ilkeleri, girişimler ve öneriler nelerdir?

Güvenlik Merkezi, aboneliklerinize güvenlik girişimleri uygular. Bu girişimler bir veya daha fazla güvenlik ilkesi içerir. Bu ilkelerin her biri güvenlik duruşunuzu iyileştirmeye yönelik bir güvenlik önerisi ile sonuçlanır. Bu sayfada bu fikirlerin her biri ayrıntılı olarak açıklanmaktadır.


## <a name="what-is-a-security-policy"></a>Güvenlik ilkesi nedir?

Azure Ilkesinde oluşturulan bir Azure ilke tanımı, denetlenmesini istediğiniz belirli güvenlik koşulları hakkında bir kuraldır. Yerleşik tanımlar, hangi kaynak türlerinin dağıtılacağını veya tüm kaynaklarda etiketlerin kullanımını zorunlu tutmayı denetleme gibi şeyleri içerir. Ayrıca, kendi özel ilke tanımlarınızı oluşturabilirsiniz.

Bu ilke tanımlarını uygulamak için (yerleşik veya özel), bunları atamanız gerekir. Bu ilkelerden herhangi birini Azure portalı, PowerShell veya Azure CLI üzerinden atayabilirsiniz.


## <a name="what-is-a-security-initiative"></a>Güvenlik girişimi nedir?

Azure girişimi, Azure ilke tanımlarının veya kuralların, belirli bir amaç veya amaç doğrultusunda birlikte gruplanmış olan bir koleksiyonudur. Azure girişimleri, bir dizi ilkeyi mantıksal olarak tek bir öğe olarak gruplayarak ilkelerinizin yönetimini basitleştirir.

Bir güvenlik girişimi, iş yüklerinizin istenen yapılandırmasını tanımlar ve şirketinizin veya düzenleyicilerinin güvenlik gereksinimleriyle uyumlu olmanıza yardımcı olur.

Güvenlik ilkeleri gibi güvenlik merkezi girişimleri de Azure Ilkesinde oluşturulur. İlkelerinizi yönetmek, girişimleri derlemek ve birden çok aboneliğe veya tüm yönetim gruplarına girişimler atamak için [Azure ilkesini](../governance/policy/overview.md) kullanabilirsiniz.

Azure Güvenlik Merkezi 'ndeki her aboneliğe otomatik olarak atanan varsayılan girişim, Azure Güvenlik kıyaslamaktır. Bu kıyaslama, yaygın uyumluluk çerçevelerine göre güvenlik ve uyum en iyi uygulamaları için Microsoft tarafından yazılan, Azure 'a özgü bir dizi kılavuzlardır. Bu, yaygın olarak kullanılan kıyaslama, [Internet güvenliği (CIS) Için merkezden](https://www.cisecurity.org/benchmark/azure/) ve [ulusal standartlar ve teknolojı Enstitüsü (NIST)](https://www.nist.gov/) ile bulut merkezli güvenliğe odaklanarak yapılar oluşturur. [Azure Güvenlik Karşılaştırması](../security/benchmarks/introduction.md) hakkında daha fazla bilgi edinin.

Güvenlik Merkezi, güvenlik girişimleri ve ilkeleriyle çalışmak için aşağıdaki seçenekleri sunar:

- **Yerleşik varsayılan girişimi görüntüleyin ve düzenleyin** -Güvenlik Merkezi 'ni etkinleştirdiğinizde, ' Azure Güvenlik kıyaslaması ' adlı girişim, tüm güvenlik merkezi kayıtlı aboneliklerine otomatik olarak atanır. Bu girişimi özelleştirmek için, içindeki ilkeleri tek tek etkinleştirebilir veya devre dışı bırakabilirsiniz. Kullanıma hazır seçenekleri anlamak için [yerleşik güvenlik ilkeleri](./policy-reference.md) listesine bakın.

- **Kendi özel girişimlerinizi ekleyin** -aboneliğinize uygulanan güvenlik girişimlerini özelleştirmek istiyorsanız, bunu Güvenlik Merkezi içinde yapabilirsiniz. Daha sonra, makineleriniz oluşturduğunuz ilkeleri izleyememesi durumunda öneriler alacaksınız. Özel ilkeler oluşturma ve atama hakkında yönergeler için bkz. [özel güvenlik girişimlerini ve Ilkelerini kullanma](custom-security-policies.md).

- **Yönetmelik uyumluluk standartlarını girişim olarak ekleyin** -Güvenlik Merkezi 'nin mevzuat uyumluluk panosu, ortamınızdaki tüm değerlendirmelerin durumunu belirli bir standart veya Yönetmeliği (Azure CIS, nıst SP 800-53 R4, SWIFT CSP cscf-V2020) bağlamında gösterir. Daha fazla bilgi için bkz. [mevzuata uyumluluğunu geliştirme](security-center-compliance-dashboard.md).

## <a name="what-is-a-security-recommendation"></a>Güvenlik önerisi nedir?

Azure Güvenlik Merkezi, olası güvenlik açıklarını belirlemek için Azure kaynaklarınızın güvenlik durumunu düzenli olarak analiz eder. Daha sonra bu güvenlik açıklarını nasıl düzeltebileceğiniz konusunda öneriler sağlar. Bir öneri, kaynaklarınızın ilgili ilkelere göre değerlendirmesine ve tanımlı gereksinimlerinizi karşılanmamış kaynakları tanımlamaya neden olur.

Azure Güvenlik Merkezi, seçtiğiniz girişimlerinize göre güvenlik önerilerini sağlar. 

Önerileriniz, kaynaklarınızı güvenli ve güvenilir hale getirmek için yapmanız gereken eylemlerdir. Her öneri aşağıdaki bilgileri sağlar:

- Sorunun kısa bir açıklaması
- Öneriyi uygulamak için gerçekleştirilecek düzeltme adımları
- Etkilenen kaynaklar

Uygulamada, aşağıdaki gibi bir işe yarar:

1. Azure Güvenlik kıyaslaması bir ***girişim***
1. Bu, tüm Azure depolama hesaplarının ağ erişimini kısıtlamalarını gerektiren bir ***ilke*** içerir, böylece saldırı yüzeyi azalır. Bu ilke, "depolama hesapları sanal ağ kurallarını kullanarak ağ erişimini kısıtlamalı" olarak adlandırılır ve devre dışı bırakılabilir veya Azure Ilkesinde etkinleştirilebilir
1. Azure Güvenlik Merkezi, korunan aboneliklerinizin hiçbirinde bir Azure depolama hesabı bulursa, sanal ağ kurallarıyla korunup korunmadığını görmek için bu hesapları değerlendirir. Bu değillerse, bu durumun düzeltilmesi ve bu kaynakların güvenliğini sağlamak için bir ***öneri*** görüntüler. 

Bu nedenle, bir girişim (1) uygun olduğunda (3) öneriler üreten ilkeler (2) içerir. 

## <a name="viewing-the-relationship-between-a-recommendation-and-a-policy"></a>Bir öneri ve ilke arasındaki ilişkiyi görüntüleme

Yukarıda belirtildiği gibi, güvenlik merkezi 'nin yerleşik önerileri Azure Güvenlik kıyaslaması ' nı temel alır. Neredeyse her öneri, kıyaslama gereksiniminden türetilen temel bir ilkeye sahiptir.

Bir önerinin ayrıntılarını gözden geçirirken genellikle temel alınan ilkeyi görmeniz yararlı olur. Bir ilke tarafından desteklenen her öneri için, öneri ayrıntıları sayfasındaki **ilke tanımını görüntüle** bağlantısını kullanarak doğrudan Ilgili Ilkenin Azure ilke girişine gidebilirsiniz:

:::image type="content" source="media/release-notes/view-policy-definition.png" alt-text="Öneriyi destekleyen belirli bir ilke için Azure Ilke sayfasına bağlantı":::

İlke tanımını görüntülemek ve değerlendirme mantığını gözden geçirmek için bu bağlantıyı kullanın. 

[Güvenlik önerileri başvuru kılavuzumuzdaki](recommendations-reference.md)önerilerin listesini gözden geçiriyorsunuz, ayrıca ilke tanımı sayfalarına bağlantılar da görürsünüz:

:::image type="content" source="media/release-notes/view-policy-definition-from-documentation.png" alt-text="Belirli bir ilke için Azure Ilke sayfasına doğrudan Azure Güvenlik Merkezi öneriler başvuru sayfasından erişme":::


## <a name="next-steps"></a>Sonraki adımlar

Bu sayfa, yüksek düzeyde, ilkeler, girişimler ve öneriler arasındaki temel kavramların ve ilişkilerin açıklanacaktır. İlgili bilgiler için bkz.:

- [Özel girişimler oluşturma](custom-security-policies.md)
- [Önerileri devre dışı bırakmak için güvenlik ilkelerini devre dışı bırakma](tutorial-security-policy.md#disable-security-policies-and-disable-recommendations)
- [Azure Ilkesinde bir güvenlik ilkesini nasıl düzenleyeceğinizi öğrenin](../governance/policy/tutorials/create-and-manage.md)