---
title: Karma kimlik tasarımı - içerik yönetimi gereksinimleri Azure | Microsoft Dokümanlar
description: İşletmenizin içerik yönetimi gereksinimlerini nasıl belirleyene ilişkin bilgiler sağlar. Genellikle bir kullanıcıkendi aygıtı olduğunda, kullandıkları uygulamaya göre dönüşümlü olacak birden çok kimlik bilgileri de olabilir. Kişisel kimlik bilgileri kullanılarak hangi içeriğin oluşturulduğunu, şirket kimlik bilgileri kullanılarak oluşturulaniçeriğin farklılaşması önemlidir. Kimlik çözümünüz, son kullanıcıya sorunsuz bir deneyim sunarken, gizliliklerini sağlamak ve veri sızıntısına karşı korumayı artırmak için bulut hizmetleriyle etkileşimkurabilmeli.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: dd1ef776-db4d-4ab8-9761-2adaa5a4f004
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/29/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0d970fd133f8c43319e7f1fdb6b3a50c3c05f687
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "64918437"
---
# <a name="determine-content-management-requirements-for-your-hybrid-identity-solution"></a>Karma kimlik çözümünüz için içerik yönetimi gereksinimlerini belirleme
İşletmeniziçin içerik yönetimi gereksinimlerini anlamak, hangi karma kimlik çözümünüzün kullanılacağına ilişkin kararınızı doğrudan etkileyebilir. Birden fazla cihazın yaygınlaşması ve kullanıcıların kendi cihazlarını[(BYOD)](https://aka.ms/byodcg)getirebilme yeteneği sayesinde, şirket kendi verilerini korumalıdır, ancak aynı zamanda kullanıcının gizliliğini de sağlam tutmalıdır. Genellikle bir kullanıcıkendi aygıtı olduğunda, kullandıkları uygulamaya göre dönüşümlü olacak birden çok kimlik bilgileri de olabilir. Kişisel kimlik bilgileri kullanılarak hangi içeriğin oluşturulduğunu, şirket kimlik bilgileri kullanılarak oluşturulaniçeriğin farklılaşması önemlidir. Kimlik çözümünüz, son kullanıcıya sorunsuz bir deneyim sunarken, gizliliklerini sağlamak ve veri sızıntısına karşı korumayı artırmak için bulut hizmetleriyle etkileşimkurabilmeli. 

Aşağıdaki şekilde gösterildiği gibi içerik yönetimi sağlamak için kimlik çözümünüz farklı teknik kontrollerden yararlanacaktır:

![güvenlik kontrolleri](./media/plan-hybrid-identity-design-considerations/securitycontrols.png)

**Kimlik yönetim sisteminizden yararlanacak güvenlik denetimleri**

Genel olarak, içerik yönetimi gereksinimleri aşağıdaki alanlarda kimlik yönetim sisteminizden yararlanacaktır:

* Gizlilik: Kaynağı olan kullanıcıyı tanımlamak ve bütünlüğü korumak için uygun denetimleri uygulamak.
* Veri Sınıflandırması: kullanıcıyı veya grubu ve bir nesneye erişim düzeyini sınıflandırmasına göre tanımlayın. 
* Veri Kaçağı Koruması: Sızıntıyı önlemek için verileri korumaktan sorumlu güvenlik kontrollerinin kullanıcının kimliğini doğrulamak için kimlik sistemiyle etkileşime geçmesi gerekir. Bu aynı zamanda iz amacı nı denetlemek için de önemlidir.

> [!NOTE]
> En iyi uygulamalar ve veri sınıflandırması yönergeleri hakkında daha fazla bilgi için [buluta hazırlık için veri sınıflandırmasını](https://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) okuyun.
> 
> 

Karma kimlik çözümünüzü planlarken, aşağıdaki soruların kuruluşunuzun gereksinimlerine göre yanıtlanmasını sağlayın:

* Şirketinizin veri gizliliğini uygulamak için güvenlik denetimleri var mı?
  * Evet ise, güvenlik denetimleri benimseyeceğiniz karma kimlik çözümüyle entegre edebilecek mi?
* Şirketiniz veri sınıflandırması kullanıyor mu?
  * Evet ise, mevcut çözüm benimseyeceğiniz karma kimlik çözümü ile entegre mümkün mü?
* Şirketinizin şu anda veri sızıntısı için herhangi bir çözümü var mı? 
  * Evet ise, mevcut çözüm benimseyeceğiniz karma kimlik çözümü ile entegre mümkün mü?
* Şirketinizin kaynaklara erişimi denetlemesi gerekiyor mu?
  * Evet ise, ne tür kaynaklar?
  * Evet ise, ne düzeyde bilgi gereklidir?
  * Evet ise, denetim günlüğünerede olmalıdır? Şirket içinde mi, bulutta mı?
* Şirketinizin hassas veriler (SSN'ler, kredi kartı numaraları, vb.) içeren e-postaları şifrelemesi gerekiyor mu?
* Şirketinizin harici iş ortaklarıyla paylaşılan tüm belgeleri/içerikleri şifrelemesi gerekiyor mu?
* Şirketinizin belirli türde e-postalar üzerinde şirket politikaları uygulaması gerekiyor mu (tüm bunları yanıtlamayın, iletmeyin)?

> [!NOTE]
> Her yanıtı not almayı unutmayın ve yanıtın arkasındaki mantığı anladığınızdan emin olun. [Veri Koruma Stratejisini Tanımla,](plan-hybrid-identity-design-considerations-data-protection-strategy.md) mevcut seçeneklerin ve her seçeneğin avantajlarının/dezavantajlarının üzerinden geçecektir.  Bu soruları yanıtlayarak, işletme ihtiyaçlarınıza en uygun seçeneği seçecektir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
[Erişim denetimi gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-accesscontrol-requirements.md)

## <a name="see-also"></a>Ayrıca Bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)

