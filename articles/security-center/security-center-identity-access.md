---
title: Azure Güvenlik Merkezi'nde kimliği ve erişimi izleme | Microsoft Docs
description: Kullanıcılarınızın erişim etkinliğini ve kimlikle ilgili sorunları izleme amacıyla Azure Güvenlik Merkezi'ndeki tanımlama ve erişim özelliklerini nasıl kullanacağınızı öğrenin.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 9f04e730-4cfa-4078-8eec-905a443133da
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/06/2020
ms.author: memildin
ms.openlocfilehash: 183b81134b2fe72a539cc6460a05d828342aafbb
ms.sourcegitcommit: 20429bc76342f9d365b1ad9fb8acc390a671d61e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79086501"
---
# <a name="monitor-identity-and-access"></a>Kimlik ve erişimi izleme

> [!TIP]
> Azure Güvenlik Merkezi 'nin kimlik ve erişim önerileri, Mart 2020 ' den ücretsiz fiyatlandırma katmanında tüm aboneliklerde yer alır. Ücretsiz katmanda abonelikleriniz varsa, kendi kimlik ve erişim güvenliği için önceden değerlendirilmedikleri için güvenli puanı bundan etkilenir. 

Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde, kaynaklarınızı korumak ve korumak için gerekli denetimleri yapılandırma sürecinde size kılavuzluk eden öneriler oluşturur.

Güvenlik çevresi, bir ağ çevre öğesinden bir kimlik çevre 'e geliştirilmiştir. Güvenlik, ağınızı erteleme ve verilerinizi erteleme ve uygulamalarınızın ve kullanıcılarınızın güvenliğini yönetme hakkında daha az hale gelir. Günümüzde buluta taşınan veri ve uygulama miktarı arttıkça kimlik yeni savunma hattı haline geliyor.

Kimlik etkinliklerini izleyerek bir olay gerçekleşmeden önce öngörülü eylemlerde veya bir saldırı girişimini durdurmak için reaktif eylemlerde bulunabilirsiniz. Azure Güvenlik Merkezi 'nin **kimlik ve erişim** kaynak güvenliği bölümünde görebileceğiniz önerilerin örnekleri şunlardır:

- MFA, aboneliğinizde sahip izinleri olan hesaplarda etkinleştirilmelidir
- Aboneliğiniz için en fazla 3 sahip belirtilmelidir
- Kullanım dışı bırakılan hesaplar aboneliğinizden kaldırılmalıdır
- Okuma izinlerine sahip dış hesapların aboneliğinizden kaldırılması gerekir

Burada görebileceğiniz önerilerin tam listesi için bkz. [kimlik ve erişim önerileri](recommendations-reference.md#recs-identity).

> [!NOTE]
> Aboneliğinizin 600 'den fazla hesabı varsa, Güvenlik Merkezi, aboneliğinize karşı kimlik önerilerini çalıştıramadı. Çalıştırılmayan öneriler aşağıda "kullanılamayan değerlendirmeler" altında listelenmiştir.
Güvenlik Merkezi, bulut çözümü sağlayıcısı (CSP) ortağının yönetim aracılarından dolayı kimlik önerilerini çalıştıramıyor.
>


Tüm kimlik ve erişim önerileri, **öneriler** sayfasında iki güvenlik denetimi içinde mevcuttur:

- Erişimi ve izinleri yönetme 
- MFA’yı etkinleştirme

![Kimlik ve erişim ile ilgili önerilere sahip iki güvenlik denetimi](media/security-center-identity-access/two-security-controls-for-identity-and-access.png)


## <a name="enable-multi-factor-authentication-mfa"></a>Multi-Factor Authentication 'ı (MFA) etkinleştir

MFA 'nın etkinleştirilmesi [Azure Active Directory (ad) kiracı izinleri](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)gerektirir. 

- AD 'nin Premium bir sürümüne sahipseniz, [koşullu erişimi](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)kullanarak MFA 'yı etkinleştirin.

- AD ücretsiz sürüm kullanıcıları, [ad belgelerinde](https://docs.microsoft.com/azure/active-directory/fundamentals/concept-fundamentals-security-defaults) açıklandığı gibi Azure Active Directory **güvenlik varsayılanlarını** etkinleştirebilir, ancak MFA 'Yı etkinleştirmeye yönelik güvenlik merkezi önerisi görünmeye devam eder.


## <a name="next-steps"></a>Sonraki adımlar
Diğer Azure kaynak türlerine uygulanan öneriler hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Güvenlik Merkezi'nde makinelerinizi ve uygulamalarınızı koruma](security-center-virtual-machine-protection.md)
- [Azure Güvenlik Merkezi'nde ağınızı koruma](security-center-network-recommendations.md)
- [Azure Güvenlik Merkezi 'nde Azure SQL hizmetinizi ve verilerinizi koruma](security-center-sql-service-recommendations.md)