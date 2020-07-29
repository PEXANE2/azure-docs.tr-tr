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
ms.date: 03/16/2020
ms.author: memildin
ms.openlocfilehash: 57e13f3ed619fa88f29397fe44b783bd3650f636
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83759175"
---
# <a name="monitor-identity-and-access"></a>Kimlik ve erişimi izleme

> [!TIP]
> Azure Güvenlik Merkezi 'nin kimlik ve erişim önerileri, Mart 2020 ' den ücretsiz fiyatlandırma katmanında tüm aboneliklerde yer alır. Ücretsiz katmanda abonelikleriniz varsa, kendi kimlik ve erişim güvenliği için önceden değerlendirilmedikleri için güvenli puanı bundan etkilenir. 

Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde, kaynaklarınızı korumak ve korumak için gerekli denetimleri yapılandırma sürecinde size kılavuzluk eden öneriler oluşturur.

Güvenlik çevresi, bir ağ çevre öğesinden bir kimlik çevre 'e geliştirilmiştir. Güvenlik, ağınızı erteleme ve verilerinizi erteleme ve uygulamalarınızın ve kullanıcılarınızın güvenliğini yönetme hakkında daha az hale gelir. Günümüzde buluta taşınan veri ve uygulama miktarı arttıkça kimlik yeni savunma hattı haline geliyor.

Kimlik etkinliklerini izleyerek, bir olay gerçekleşmeden önce öngörülü eylemler gerçekleştirebilir veya bir saldırı girişimini durdurmak için reaktif eylemler gerçekleştirebilirsiniz. Örneğin, Güvenlik Merkezi kullanım dışı bırakılmış hesaplara işaret edebilir (artık gerekli olmayan ve Azure Active Directory tarafından oturum açması engellenen hesaplara) kaldırma için. 

Azure Güvenlik Merkezi 'nin **kimlik ve erişim** kaynak güvenliği bölümünde görebileceğiniz önerilerin örnekleri şunlardır:

- MFA, aboneliğinizde sahip izinleri olan hesaplarda etkinleştirilmelidir
- Aboneliğiniz için en fazla 3 sahip belirtilmelidir
- Okuma izinlerine sahip dış hesapların aboneliğinizden kaldırılması gerekir
- Kullanım dışı bırakılan hesaplar aboneliğinizden kaldırılmalıdır

Bu öneriler ve burada görebileceğiniz önerilerin tam listesi hakkında daha fazla bilgi için bkz. [kimlik ve erişim önerileri](recommendations-reference.md#recs-identity).

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

- AD 'nin Premium bir sürümüne sahipseniz, [koşullu erişim](../active-directory/conditional-access/concept-conditional-access-policy-common.md)kullanarak MFA 'yı etkinleştirin.

- AD ücretsiz sürüm kullanıcıları, [ad belgelerinde](https://docs.microsoft.com/azure/active-directory/fundamentals/concept-fundamentals-security-defaults) açıklandığı gibi Azure Active Directory **güvenlik varsayılanlarını** etkinleştirebilir, ancak MFA 'Yı etkinleştirmeye yönelik güvenlik merkezi önerisi görünmeye devam eder.


## <a name="next-steps"></a>Sonraki adımlar
Diğer Azure kaynak türlerine uygulanan öneriler hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Güvenlik Merkezi'nde makinelerinizi ve uygulamalarınızı koruma](security-center-virtual-machine-protection.md)
- [Azure Güvenlik Merkezi'nde ağınızı koruma](security-center-network-recommendations.md)
- [Azure Güvenlik Merkezi 'nde Azure SQL hizmetinizi ve verilerinizi koruma](security-center-sql-service-recommendations.md)