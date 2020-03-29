---
title: Karma kimlik tasarımı - veri koruma gereksinimleri Azure | Microsoft Dokümanlar
description: Karma kimlik çözümünüzü planlarken, işletmeniz için veri koruma gereksinimlerini ve bu gereksinimleri en iyi şekilde karşılamak için hangi seçeneklerin kullanılabildiği nizi belirleyin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "64918786"
---
# <a name="plan-for-enhancing-data-security-through-a-strong-identity-solution"></a>Güçlü bir kimlik çözümü yle veri güvenliğini artırma planı
Verileri korumanın ilk adımı, bu verilere kimlerin erişebileceğini belirlemektir. Ayrıca, kimlik doğrulama ve yetkilendirme özellikleri sağlamak için sisteminizle tümleştirebilen bir kimlik çözümüne sahip olmanız gerekir. Kimlik doğrulama ve yetkilendirme genellikle birbiriyle karıştırılır ve rolleri yanlış anlaşılır. Gerçekte, aşağıdaki şekilde gösterildiği gibi, farklı:

![mobil cihaz yaşam döngüsü](./media/plan-hybrid-identity-design-considerations/mobile-devicemgt-lifecycle.png)

**Mobil cihaz yönetimi yaşam döngüsünün aşamaları**

Karma kimlik çözümünüzü planlarken, işletmeniz için veri koruma gereksinimlerini ve bu gereksinimleri en iyi şekilde yerine getirmek için hangi seçeneklerin kullanılabildiği nizi anlamanız gerekir.

> [!NOTE]
> Veri güvenliği planlamasını tamamladıktan sonra, çok faktörlü kimlik doğrulama gereksinimleriyle ilgili seçimlerinizin bu bölümde ki kararlardan etkilenmediğinden emin olmak için [çok faktörlü kimlik doğrulama gereksinimlerini](plan-hybrid-identity-design-considerations-multifactor-auth-requirements.md) belirleyin'i gözden geçirin.
> 
> 

## <a name="determine-data-protection-requirements"></a>Veri koruma gereksinimlerini belirleme
Mobilite çağında, çoğu şirketin ortak bir amacı vardır: üretkenliği artırmak için kullanıcılarının mobil cihazlarında, şirket içinde yken veya herhangi bir yerden uzaktan üretken olmalarını sağlamak. Bu tür gereksinimlere sahip şirketler, şirketin verilerini güvende tutmak ve kullanıcının gizliliğini korumak için azaltılması gereken tehditlerin sayısı konusunda da endişe duyacaklardır. Her şirketin bu konuda farklı gereksinimleri olabilir; şirketin hangi sektöre göre hareket ettiğine göre değişen farklı uyumluluk kuralları farklı tasarım kararlarına yol açacaktır. 

Ancak, ne olursa olsun sanayi, araştırılması ve doğrulanması gereken bazı güvenlik yönleri vardır.

## <a name="data-protection-paths"></a>Veri koruma yolları
![veri koruma yolları](./media/plan-hybrid-identity-design-considerations/data-protection-paths.png)

**Veri koruma yolları**

Yukarıdaki diyagramda, verilere erişilmeden önce doğrulanan ilk kimlik bileşeni olacaktır. Ancak, bu veriler erişilen süre boyunca farklı durumlarda olabilir. Bu diyagramdaki her sayı, verilerin belirli bir zamanda bulunabileceği bir yolu temsil eder. Bu sayılar aşağıda açıklanmıştır:

1. Aygıt düzeyinde veri koruması.
2. Aktarım sırasında veri koruması.
3. Şirket içinde dinlenirken veri koruması.
4. Bulutta dinlenirken veri koruması.

Karma kimlik çözümünün, verilere erişim izni vermeden önce kullanıcıyı tanımlamak için hem şirket içi hem de bulut kimlik yönetimi kaynaklarından yararlanabilmesi gerekir. Karma kimlik çözümünüzü planlarken, aşağıdaki soruların kuruluşunuzun gereksinimlerine göre yanıtlandığından emin olun:

## <a name="data-protection-at-rest"></a>Istirahatte veri koruması
Verilerin nerede olduğuna bakılmaksızın (aygıt, bulut veya şirket içinde), kuruluşun bu konudaki ihtiyaçlarını anlamak için bir değerlendirme yapmak önemlidir. Bu alan için, aşağıdaki soruların sorulduğundan emin olun:

* Şirketinizin verileri istirahatte koruması gerekiyor mu?
  * Evet ise, karma kimlik çözümü mevcut şirket içi altyapınızla entegre edilebilir mi?
  * Evet ise, karma kimlik çözümü bulutta bulunan iş yüklerinizle tümleştirilebilir mi?
* Bulut kimlik yönetimi, kullanıcının kimlik bilgilerini ve bulutta depolanan diğer verileri koruyabilir mi?

## <a name="data-protection-in-transit"></a>Aktarımsırasında veri koruması
Aygıt ile veri merkezi arasında veya aygıt ile bulut arasında aktarım halindeki veriler korunmalıdır. Ancak, toplu taşıma, bulut hizmetinizin dışında bir bileşene sahip bir iletişim süreci anlamına gelmez; iki sanal ağ arasında olduğu gibi dahili olarak da hareket eder. Bu alan için, aşağıdaki soruların sorulduğundan emin olun:

* Şirketinizin aktarım sırasındaki verileri koruması gerekiyor mu?
  * Evet ise, hibrit kimlik çözümü SSL/TLS gibi güvenli denetimlerle entegre edilebilir mi?
* Bulut kimlik yönetimi trafiği dizin deposuna ve içinde (veri merkezleri içinde ve arasında) imzalı olarak tutar mı?

## <a name="compliance"></a>Uyumluluk
Yönetmelikler, yasalar ve mevzuata uygunluk gereklilikleri şirketinizin ait olduğu sektöre göre değişir. Yüksek düzenlemeye tabi sektörlerdeki şirketler, uyumluluk sorunlarıyla ilgili kimlik yönetimi sorunlarını ele almalıdır. Sarbanes-Oxley (SOX), Sağlık Sigortası Taşınabilirlik ve Sorumluluk Yasası (HIPAA), Gramm-Leach-Bliley Yasası (GLBA) ve Ödeme Kartı Endüstrisi Veri Güvenliği Standardı (PCI DSS) gibi düzenlemeler kimlik ve erişim konusunda katıdır. Şirketinizin benimseyeceği karma kimlik çözümü, bu düzenlemelerden bir veya daha fazlasının gereksinimlerini karşılayacak temel yeteneklere sahip olmalıdır. Bu alan için, aşağıdaki soruların sorulduğundan emin olun:

* Karma kimlik çözümü işletmeniz için düzenleyici gerekliliklere uygun mudur?
* Karma kimlik çözümü yerleşik mi 
* şirketinizin mevzuat gerekliliklerine uygun olmasını sağlayacak yeteneklere sahip misiniz? 

> [!NOTE]
> Her yanıtı not almayı unutmayın ve yanıtın arkasındaki mantığı anladığınızdan emin olun. [Veri Koruma Stratejisini Tanımla,](plan-hybrid-identity-design-considerations-data-protection-strategy.md) mevcut seçeneklerin ve her seçeneğin avantajlarının/dezavantajlarının üzerinden geçecektir.  Bu soruları yanıtlayarak, işletme ihtiyaçlarınıza en uygun seçeneği seçecektir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
 [İçerik yönetimi gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-contentmgt-requirements.md)

## <a name="see-also"></a>Ayrıca Bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)

