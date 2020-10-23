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
ms.date: 10/08/2020
ms.author: memildin
ms.openlocfilehash: 575c139a3b417eb9429695d3ea6be26bf5625de5
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92371078"
---
# <a name="monitor-identity-and-access"></a>Kimlik ve erişimi izleme

Güvenlik çevresi, bir ağ çevre öğesinden bir kimlik çevre 'e geliştirilmiştir. Bu geliştirmeyle, güvenlik, ağınızı erteleme ve uygulamalarınızın, verilerinizin ve kullanıcılarınızın güvenliğini yönetme hakkında daha azdır.

Kimlik ile ilgili etkinlikleri ve yapılandırma ayarlarını izleyerek, bir olay gerçekleşmeden önce öngörülü eylemler gerçekleştirebilir ya da denenen saldırıları durdurmak için reaktif eylemler gerçekleştirebilirsiniz.

## <a name="what-identity-and-access-safeguards-does-security-center-provide"></a>Güvenlik Merkezi hangi kimlik ve erişim korumaları sağlar? 

Azure Güvenlik Merkezi, kuruluşunuzun kimlik ve güvenlik gereksinimlerini karşıladığınızı sağlamaya yönelik iki adanmış güvenlik denetimine sahiptir: 

 - **Erişimi ve Izinleri yönetme** - [en az ayrıcalık erişim modelini](/windows-server/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models) benimsemenizi ve kullanıcılarınıza yalnızca onların işlerini yapması için gereken erişimi vermenizi öneririz. Bu denetim Ayrıca kaynaklarınıza erişimi denetlemek için [rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md) uygulamaya yönelik öneriler içerir.
 
 - **MFA 'Yı etkinleştirme** - [MFA](https://www.microsoft.com/security/business/identity/mfa) etkinken hesaplarınız daha güvenlidir ve kullanıcılar, tek oturum açma ile neredeyse her uygulama için kimlik doğrulaması yapabilir.

### <a name="example-recommendations-for-identity-and-access"></a>Kimlik ve erişim için örnek öneriler

Güvenlik Merkezi 'nin **öneriler** sayfasında bu iki denetim içinde görebileceğiniz önerilerin örnekleri:

- MFA, aboneliğinizde sahip izinleri olan hesaplarda etkinleştirilmelidir
- Aboneliğiniz için en fazla 3 sahip belirtilmelidir
- Okuma izinlerine sahip dış hesapların aboneliğinizden kaldırılması gerekir
- Kullanım dışı bırakılan hesaplar aboneliğinizden çıkarılmalıdır (kullanım dışı hesaplar artık gerekli olmayan ve Azure Active Directory tarafından oturum açması engellenen hesaplardır)

> [!TIP]
> Bu öneriler ve bu denetimlerde görebileceğiniz diğerleri hakkında daha fazla bilgi için bkz. [kimlik ve erişim önerileri](recommendations-reference.md#recs-identity).

### <a name="limitations"></a>Sınırlamalar

Güvenlik Merkezi 'nin kimlik ve erişim korumalarının bazı sınırlamaları vardır:

- Kimlik önerileri 600 taneden fazla hesabı olan abonelikler için kullanılamaz. Bu gibi durumlarda, bu öneriler "kullanılamayan değerlendirmeler" altında listelenecektir.
- Bulut çözümü sağlayıcısı (CSP) ortağının yönetim aracılarında kimlik önerileri kullanılamaz.
- Kimlik önerileri, ayrıcalıklı kimlik yönetimi (PıM) sistemi ile yönetilen hesapları tanımlamaz. PıM aracı kullanıyorsanız, **erişimi yönet ve izinler** denetiminde yanlış sonuçlar görebilirsiniz.

## <a name="multi-factor-authentication-mfa-and-azure-active-directory"></a>Multi-Factor Authentication (MFA) ve Azure Active Directory 

MFA 'nın etkinleştirilmesi [Azure Active Directory (ad) kiracı izinleri](../active-directory/roles/permissions-reference.md)gerektirir.

- AD 'nin Premium bir sürümüne sahipseniz, [koşullu erişim](../active-directory/conditional-access/concept-conditional-access-policy-common.md)kullanarak MFA 'yı etkinleştirin.
- AD ücretsiz sürüm kullanıyorsanız, [Azure Active Directory belgelerinde](../active-directory/fundamentals/concept-fundamentals-security-defaults.md)açıklandığı şekilde **güvenlik varsayılanlarını** etkinleştirin.

## <a name="identify-accounts-without-multi-factor-authentication-mfa-enabled"></a>Multi-Factor Authentication (MFA) etkin olmayan hesapları tanımla

Hangi hesapların MFA 'nın etkin olduğunu görmek için aşağıdaki Azure Kaynak Grafiği sorgusunu kullanın. Sorgu, tüm sağlıksız kaynakları döndürür-önerinin tüm uygun olmayan kaynakları-hesaplar, aboneliğiniz üzerinde sahip izinleri olan hesaplarda MFA etkinleştirilmelidir. 

1. **Azure Kaynak Grafiği Gezginini**açın.

    :::image type="content" source="./media/security-center-identity-access/opening-resource-graph-explorer.png" alt-text="Azure Kaynak Grafiği Gezgini * * öneri sayfası başlatılıyor" :::

1. Aşağıdaki sorguyu girin ve **Sorguyu Çalıştır**' ı seçin.

    ```kusto
    securityresources
     | where type == "microsoft.security/assessments"
     | where properties.displayName == "MFA should be enabled on accounts with owner permissions on your subscription"
     | where properties.status.code == "Unhealthy"
    ```

1. `additionalData`Özelliği, MFA 'ya sahip olmayan hesapların Hesap nesne kimliklerinin listesini ortaya koyar. 

    > [!NOTE]
    > Hesaplar hesap sahiplerinin gizliliğini korumak için hesap adları yerine nesne kimlikleri olarak gösterilir.

> [!TIP]
> Alternatif olarak, güvenlik merkezi 'nin REST API yöntemi [değerlendirmesi-al](/rest/api/securitycenter/assessments/get)kullanabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar
Diğer Azure Kaynak türleri için uygulanan öneriler hakkında daha fazla bilgi edinmek için aşağıdaki makaleye bakın:

- [Azure Güvenlik Merkezi'nde ağınızı koruma](security-center-network-recommendations.md)