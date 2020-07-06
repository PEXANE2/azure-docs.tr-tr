---
title: Karma kimlik tasarımı-veri koruma gereksinimleri Azure | Microsoft Docs
description: Hibrit kimlik çözümünüzü planlarken, işiniz için veri koruma gereksinimlerini ve bu gereksinimleri en iyi şekilde karşılamak için kullanılabilecek seçenekleri belirleyebilirsiniz.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: 40dc4baa-fe82-4ab6-a3e4-f36fa9dcd0df
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/29/2019
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 056d0caf2bfc6e99d5d659a2561bc41844feb79a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "64918786"
---
# <a name="plan-for-enhancing-data-security-through-a-strong-identity-solution"></a>Güçlü bir kimlik çözümü aracılığıyla veri güvenliğini geliştirmek için plan yapın
Verileri korumanın ilk adımı, verilere kimlerin erişebileceğini belirlemektir. Ayrıca, kimlik doğrulama ve yetkilendirme özellikleri sağlamak üzere sisteminizle tümleştirilebilen bir kimlik çözümünüz olması gerekir. Kimlik doğrulama ve yetkilendirme genellikle birbirleriyle ve rollerinin yanlış anlaşıldığı şekilde karıştırılır. Gerçekte, aşağıdaki şekilde gösterildiği gibi farklıdır:

![mobil cihaz yaşam döngüsü](./media/plan-hybrid-identity-design-considerations/mobile-devicemgt-lifecycle.png)

**Mobil cihaz yönetimi yaşam döngüsünün aşamaları**

Hibrit kimlik çözümünüzü planlarken, işiniz için veri koruma gereksinimlerini ve bu gereksinimlerin en iyi şekilde elde edilen seçeneklerini anlamanız gerekir.

> [!NOTE]
> Veri güvenliği planlamasını tamamladıktan sonra, Multi-Factor Authentication gereksinimleriyle ilgili seçimlerinizin bu bölümde yaptığınız kararlardan etkilenmemesini sağlamak için, [çok faktörlü kimlik doğrulama gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-multifactor-auth-requirements.md) konusunu gözden geçirin.
> 
> 

## <a name="determine-data-protection-requirements"></a>Veri koruma gereksinimlerini belirleme
Taşınabilirlik süresinin çoğunda, çoğu şirket ortak bir amaca sahiptir: kullanıcıların, üretkenliği artırmak için mobil cihazlarında, şirket içinde veya uzaktan bir yerden üretken olmalarını sağlama. Bu tür gereksinimlere sahip şirketler, şirket verilerinin güvenliğini sağlamak ve Kullanıcı gizliliğini korumak için, azaltılmalıdır olması gereken tehditler sayısını da karşılacaktır. Her şirket bu şekilde farklı gereksinimlere sahip olabilir; şirketin hangi sektöre göre hareket ettiğini fark eden farklı uyumluluk kuralları, farklı tasarım kararlarına yol açabilir. 

Ancak, sektörden bağımsız olarak keşfedilecek ve doğrulanacak bazı güvenlik yönleri vardır.

## <a name="data-protection-paths"></a>Veri koruma yolları
![veri koruma yolları](./media/plan-hybrid-identity-design-considerations/data-protection-paths.png)

**Veri koruma yolları**

Yukarıdaki diyagramda, veri erişmeden önce kimlik bileşeni doğrulanacak olan ilk bir değer olacaktır. Ancak, bu veriler erişildiği sırada farklı durumlarda olabilir. Bu diyagramdaki her bir sayı, verilerin belirli bir noktada bulunduğu yolu temsil eder. Bu numaralar aşağıda açıklanmıştır:

1. Cihaz düzeyinde veri koruma.
2. Aktarım sırasında veri koruması.
3. Şirket içi Rest sırasında veri koruması.
4. Bulutta bekleyen veri koruma.

Karma kimlik çözümünün, verileri erişim vermeden önce kullanıcıyı tanımlamak için hem şirket içi hem de bulut kimlik yönetimi kaynaklarını kullanabildiğinden emin olmanız gerekir. Karma kimlik çözümünüzü planlarken, kuruluşunuzun gereksinimlerine göre aşağıdaki soruların yanıtlanmasından emin olun:

## <a name="data-protection-at-rest"></a>Bekleyen veri koruması
Verilerin, bekleyen (cihaz, bulut veya şirket içi) olduğu yere bakılmaksızın, kuruluşun ihtiyaçlarını bu şekilde anlamak için bir değerlendirme yapmak önemlidir. Bu alan için aşağıdaki sorulara sorulduğu emin olun:

* Şirketinizin bekleyen verileri koruması gerekiyor mu?
  * Yanıt Evet ise, karma kimlik çözümü mevcut şirket içi altyapınızla tümleştirilebiliyor mu?
  * Yanıt Evet ise, karma kimlik çözümü bulutta bulunan iş yükleriniz ile tümleştirilebiliyor mu?
* Bulut kimlik yönetimi, kullanıcının kimlik bilgilerini ve bulutta depolanan diğer verileri koruyabiliyor mu?

## <a name="data-protection-in-transit"></a>Aktarım sırasında veri koruması
Cihaz ile veri merkezi arasında ya da cihaz ile bulut arasında geçiş sırasında verilerin korunması gerekir. Ancak, aktarım sırasında, bulut hizmetiniz dışındaki bir bileşenle bir iletişim işlemi olması gerektiği anlamına gelir. iki sanal ağ arasında da olduğu gibi, dahili olarak da gider. Bu alan için aşağıdaki sorulara sorulduğu emin olun:

* Şirketinizin yoldaki verileri koruması gerekiyor mu?
  * Yanıt Evet ise, karma kimlik çözümü SSL/TLS gibi güvenli denetimlerle tümleştirilebiliyor mu?
* Bulut kimlik yönetimi, trafiği dizin deposu içinde ve içinde (veri merkezleri içinde ve arasında) açık tutar mı?

## <a name="compliance"></a>Uyumluluk
Yönetmelikler, yasalar ve mevzuat uyumluluk gereksinimleri, şirketinizin ait olduğu sektöre göre farklılık gösterecektir. Yüksek düzeyde düzenlenen sektörlerdeki şirketler, uyumluluk sorunlarıyla ilgili kimlik yönetimi sorunlarını ele almalıdır. Sarbanes-Oxley (SOX), sağlık sigortası taşınabilirlik ve Sorumluluk Yasası (HIPAA), Gramm-Leach-Bliley Yasası (GLBA) ve ödeme kartı sektör veri güvenliği standardı (PCI DSS), kimlik ve erişime yönelik katı. Şirketinizin benimseyeceği hibrit kimlik çözümü, bu düzenlemelerden bir veya daha fazla gereksinimi karşılayacak temel yeteneklere sahip olmalıdır. Bu alan için aşağıdaki sorulara sorulduğu emin olun:

* Karma kimlik çözümü, işletmeniz için mevzuat gereksinimleriyle uyumlu mı?
* Karma kimlik çözümü derlenmiştir 
* Şirketinizin uyumlu yasal gereksinimler olmasını sağlayacak yetenekler nelerdir? 

> [!NOTE]
> Her yanıtı not almayı unutmayın ve yanıtın arkasındaki mantığı anladığınızdan emin olun. [Veri koruma stratejisini tanımlama](plan-hybrid-identity-design-considerations-data-protection-strategy.md) , kullanılabilir seçenekler ve her seçeneğin olumlu/olumsuz yönleri üzerinden alınacaktır.  Bu sorulara yanıt vererek, iş gereksinimlerinize en uygun seçeneği seçersiniz.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
 [İçerik yönetimi gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-contentmgt-requirements.md)

## <a name="see-also"></a>Ayrıca Bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)

