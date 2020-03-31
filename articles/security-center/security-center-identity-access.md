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
ms.openlocfilehash: 152168bbadd5815659bc5f70c91bd2a28f5e049d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79481968"
---
# <a name="monitor-identity-and-access"></a>Kimlik ve erişimi izleme

> [!TIP]
> Azure Güvenlik Merkezi'nin kimlik ve erişim önerileri Mart 2020'den itibaren ücretsiz fiyatlandırma katmanındaki tüm aboneliklere dahildir. Ücretsiz katmanda aboneliklerin varsa, daha önce kimlikleri ve erişim güvenliği için değerlendirilmedikleri için Güvenli Puanları etkilenir. 

Güvenlik Merkezi olası güvenlik açıklarını tanımladığında, kaynaklarınızı sertleştirmek ve korumak için gerekli denetimleri yapılandırma sürecinde size rehberlik eden öneriler oluşturur.

Güvenlik çemberi bir ağ çevresinden kimlik çevresine dönüştü. Güvenlik, ağınızı savunmaktan çok, verilerinizi savunmanın yanı sıra uygulamalarınızın ve kullanıcılarınızın güvenliğini yönetme konusunda daha fazla bilgi edinirken olur. Günümüzde buluta taşınan veri ve uygulama miktarı arttıkça kimlik yeni savunma hattı haline geliyor.

Kimlik etkinliklerini izleyerek, bir olay gerçekleşmeden önce proaktif eylemlerde bulunabilir veya bir saldırı girişimini durdurmak için tepkisel eylemlerde bulunabilirsiniz. Örneğin, Güvenlik Merkezi amortismana alınan hesapları (artık ihtiyaç duyulmayan ve Azure Etkin Dizini tarafından oturum açması engellenen hesaplar) kaldırılması için işaretleyebilir. 

Azure Güvenlik Merkezi'nin **Kimlik ve erişim** kaynağı güvenliği bölümünde görebileceğiniz önerilere örnek olarak şunlar verilebilir:

- MFA, aboneliğinizde sahip izni olan hesaplarda etkinleştirilmelidir
- Aboneliğiniz için en fazla 3 sahip belirlenmelidir
- Okuma izinleri olan harici hesaplar aboneliğinizden kaldırılmalıdır
- Amortismana alınan hesaplar aboneliğinizden kaldırılmalıdır

Bu öneriler hakkında daha fazla bilgi ve burada görebileceğiniz önerilerin tam listesi için [Kimlik ve Erişim önerilerine](recommendations-reference.md#recs-identity)bakın.

> [!NOTE]
> Aboneliğinizde 600'den fazla hesap varsa, Güvenlik Merkezi aboneliğinize karşı Kimlik önerilerini çalıştıramaz. Çalıştırılamayan öneriler aşağıdaki "kullanılamayan değerlendirmeler" altında listelenmiştir.
Güvenlik Merkezi, Bir Bulut Çözüm Sağlayıcısı (CSP) iş ortağının yönetici temsilcilerine karşı Kimlik önerilerini çalıştıramaz.
>


Tüm kimlik ve erişim önerileri **Öneriler** sayfasındaki iki güvenlik denetimi nde mevcuttur:

- Erişimi ve izinleri yönetme 
- MFA’yı etkinleştirme

![Kimlik ve erişimle ilgili önerilerle birlikte iki güvenlik denetimi](media/security-center-identity-access/two-security-controls-for-identity-and-access.png)


## <a name="enable-multi-factor-authentication-mfa"></a>Çok faktörlü kimlik doğrulamayı etkinleştirme (MFA)

MFA'yı etkinleştirmek için [Azure Etkin Dizin (AD) kiracı izinleri](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)gerekir. 

- AD'nin premium sürümü varsa, [koşullu erişimi](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)kullanarak MFA'yı etkinleştirin.

- AD ücretsiz sürümü kullanıcıları, [AD belgelerinde](https://docs.microsoft.com/azure/active-directory/fundamentals/concept-fundamentals-security-defaults) açıklandığı gibi Azure Active Directory'de **güvenlik varsayılanlarını** etkinleştirebilir, ancak MFA'yı etkinleştirmek için Güvenlik Merkezi önerisi görünmeye devam eder.


## <a name="next-steps"></a>Sonraki adımlar
Diğer Azure kaynak türleri için geçerli olan öneriler hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Güvenlik Merkezi'nde makinelerinizi ve uygulamalarınızı koruma](security-center-virtual-machine-protection.md)
- [Azure Güvenlik Merkezi'nde ağınızı koruma](security-center-network-recommendations.md)
- [Azure SQL hizmetinizi ve verilerinizi Azure Güvenlik Merkezi'nde koruma](security-center-sql-service-recommendations.md)