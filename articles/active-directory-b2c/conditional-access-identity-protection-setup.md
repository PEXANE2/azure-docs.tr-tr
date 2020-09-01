---
title: Azure AD B2C 'da kimlik koruması ve koşullu erişim ayarlama
description: Azure AD B2C kiracının riskli oturum açma ve diğer risk olaylarını görüntülemesi ve risk algılamaları temelinde ilkeler oluşturması için kimlik koruması ve koşullu erişimi nasıl yapılandıracağınızı öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: overview
ms.date: 09/01/2020
ms.author: mimart
author: msmimart
manager: celested
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5554cfcde9aba1b0e5c9c8b60e2e6a7e9a8ba378
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89271597"
---
# <a name="set-up-identity-protection-and-conditional-access-in-azure-ad-b2c"></a>Azure AD B2C 'da kimlik koruması ve koşullu erişim ayarlama

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

Kimlik koruması, Azure AD B2C kiracınız için sürekli risk algılama sağlar. Azure AD B2C kiracı fiyatlandırma katmanınız Premium P2 ise, ayrıntılı kimlik koruması risk olaylarını Azure portal görebilirsiniz. Ayrıca, eylemleri tespit etmek ve kuruluş ilkelerini zorlamak için bu risk algılamalarını temel alan [koşullu erişim](../active-directory/conditional-access/overview.md) ilkelerini de kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Azure AD B2C kiracınızın [bir Azure AD aboneliğine bağlı](billing.md#link-an-azure-ad-b2c-tenant-to-a-subscription)olması gerekir.
- Azure AD B2C Premium P2, oturum açma ve Kullanıcı risk tabanlı koşullu erişim kullanımı için gereklidir. Gerekirse, [Azure AD B2C fiyatlandırma katmanınızı Premium P2 olarak değiştirin](https://aka.ms/exid-pricing-tier). 
- B2C kiracınızda kimlik koruması ve koşullu erişimi yönetmek için, genel yönetici rolüne veya güvenlik yöneticisi rolüne atanmış bir hesaba sahip olmanız gerekir.
- Kiracınızda bu özellikleri kullanmak için öncelikle Azure AD B2C Premium P2 fiyatlandırma katmanına geçmeniz gerekir.

## <a name="set-up-identity-protection"></a>Kimlik koruması ayarlama

Kimlik koruması varsayılan olarak açık. Azure AD B2C kiracınızda kimlik koruması risk olaylarını görüntüleyebilmek için, Azure AD B2C kiracınızı bir Azure AD aboneliğine bağlamanız ve Azure AD B2C Premium P2 fiyatlandırma katmanını seçmeniz yeterlidir. Azure portal ayrıntılı risk olayı raporları görüntüleyebilirsiniz.

### <a name="supported-identity-protection-risk-detections"></a>Desteklenen kimlik koruması risk algılamaları

Aşağıdaki risk algılamaları Şu anda Azure AD B2C için desteklenmektedir:  

|Risk algılama türü  |Açıklama  |
|---------|---------|
| Olağandışı yolculuk     | Kullanıcının en son oturum açma işlemlerini temel alarak sıradan bir konumdan oturum açın.        |
|Anonim IP adresi     | Anonim bir IP adresinden oturum açın (örneğin: Tor tarayıcısı, anonimleştirici VPN 'Leri)        |
|Bilinmeyen oturum açma özellikleri     | Belirtilen kullanıcı için son gördüğdiğimiz özelliklerle oturum açın.        |
|Kötü amaçlı yazılım bağlı IP adresi     | Kötü amaçlı yazılımdan bağlantılı IP adresinden oturum açın         |
|Azure AD tehdit bilgileri     | Microsoft 'un dahili ve dış tehdit bilgileri kaynakları, bilinen bir saldırı modelini tanımladı        |

## <a name="view-risk-events-for-your-azure-ad-b2c-tenant"></a>Azure AD B2C kiracınız için risk olaylarını görüntüleme

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. Portal araç çubuğunda **Dizin + abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.

1. Azure portal, araması yapın ve **Azure AD B2C**seçin.

1. **Güvenlik**altında **riskli kullanıcılar (Önizleme)** seçeneğini belirleyin.

   ![Riskli kullanıcılar](media/conditional-access-identity-protection-setup/risky-users.png)

1. **Güvenlik**altında **risk algılamaları (Önizleme)** seçeneğini belirleyin.

   ![Risk algılamaları](media/conditional-access-identity-protection-setup/risk-detections.png)

## <a name="add-a-conditional-access-policy"></a>Koşullu erişim ilkesi ekleme 

Kimlik koruması risk algılamalarını temel alan koşullu erişim ilkesi eklemek için, Azure AD B2C kiracınızda güvenlik varsayılanlarını devre dışı bırakıldığından emin olun ve sonra koşullu erişim ilkeleri oluşturun.

### <a name="to-disable-security-defaults"></a>Güvenlik varsayılanlarını devre dışı bırakmak için

1. [Azure portalında](https://portal.azure.com/) oturum açın.

2. Portal araç çubuğunda **Dizin + abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.

3. Azure portal, araması yapın ve **Azure Active Directory**seçin.

4. **Özellikler**' i seçin ve ardından **güvenlik varsayılanlarını Yönet**' i seçin.

   ![Güvenlik varsayılanlarını devre dışı bırak](media/conditional-access-identity-protection-setup/disable-security-defaults.png)

5. Güvenlik varsayılanlarını etkinleştir altında Hayır ' ı seçin. 

   ![Güvenlik varsayılanlarını etkinleştir ' i ayarla Hayır olarak ayarlayın](media/conditional-access-identity-protection-setup/enable-security-defaults-toggle.png)

### <a name="to-create-a-conditional-access-policy"></a>Koşullu erişim ilkesi oluşturmak için

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. Portal araç çubuğunda **Dizin + abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.

1. Azure portal, araması yapın ve **Azure AD B2C**seçin.

1. **Güvenlik**altında **koşullu erişim (Önizleme)** öğesini seçin. **Koşullu erişim ilkeleri** sayfası açılır. 

1. Yeni **ilke** ' yi seçin ve yeni bir ilke oluşturmak IÇIN Azure AD koşullu erişim belgelerini izleyin. Aşağıda bir örnek verilmiştir:

   - [Oturum açma riski tabanlı koşullu erişim: koşullu erişim ilkesiyle etkinleştirin](../active-directory/conditional-access/howto-conditional-access-policy-risk.md#enable-with-conditional-access-policy)

   > [!IMPORTANT]
   > İlkeyi uygulamak istediğiniz kullanıcıları seçerken, yalnızca **tüm kullanıcıları** seçmeyin veya kendi oturum açmasını engelleyebilirsiniz.

## <a name="test-the-conditional-access-policy"></a>Koşullu erişim Ilkesini test etme

1. Yukarıda belirtildiği gibi, aşağıdaki ayarlarla bir koşullu erişim ilkesi oluşturun:
   
   - **Kullanıcılar ve gruplar**için test kullanıcısını seçin. **Tüm kullanıcıları** seçmeyin veya kendi oturum açmasını engelleyebilirsiniz.
   - **Bulut uygulamaları veya eylemler**Için **Uygulama Seç**' i seçin ve ardından bağlı olan taraf uygulamanızı seçin.
   - Koşullar için **oturum açma riski** ve **yüksek**, **Orta**ve **düşük** riskli Düzeyler ' i seçin.
   - **Izin ver**Için **erişimi engelle**' yi seçin.

      ![Erişimi engelle 'yi seçin](media/conditional-access-identity-protection-setup/test-conditional-access-policy.png)

1. **Oluştur**' a tıklayarak test koşullu erişim ilkenizi etkinleştirin.

1. [Tor tarayıcısını](https://www.torproject.org/download/)kullanarak riskli oturum açma benzetimi yapın. 

1. Denenen oturum açma girişimi için jwt.ms kodu çözülen belirteçte, oturum açma 'nın engellendiğini görmeniz gerekir:

   ![Engellenen bir oturum açma sınamasını yapın](media/conditional-access-identity-protection-setup/test-blocked-sign-in.png)

## <a name="review-conditional-access-outcomes-in-the-audit-report"></a>Denetim raporundaki koşullu erişim sonuçlarını gözden geçirme

Koşullu erişim olayının sonucunu gözden geçirmek için:

1. [Azure portalında](https://portal.azure.com/) oturum açın.

2. Portal araç çubuğunda **Dizin + abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.

3. Azure portal, araması yapın ve **Azure AD B2C**seçin.

4. **Etkinlikler**' in altında, **Denetim günlükleri**' ni seçin.

5. **Kategoriyi** **B2C** olarak ayarlayarak ve **etkinlik kaynak türünü** **ıdentityprotection**olarak ayarlayarak denetim günlüğünü filtreleyin. Sonra **Uygula**'yı seçin.

6. Son 7 güne kadar denetim etkinliğini gözden geçirin. Aşağıdaki etkinlik türleri dahildir:

   - **Koşullu erişim Ilkelerini değerlendir**: Bu denetim günlüğü girdisi, bir kimlik doğrulaması sırasında koşullu erişim değerlendirmesinin gerçekleştirildiğini gösterir.
   - **Kullanıcıyı Düzelt**: Bu giriş, koşullu erişim ilkesinin verme veya gereksinimlerinin Son Kullanıcı tarafından karşılandığını ve bu etkinliğin, kullanıcının (hafifletmek) riskini azaltmak için risk altyapısına rapor edildiği anlamına gelir.

7. Listede bir **koşullu erişim ilkesi** günlüğü seçin: denetim günlüğü tanımlayıcılarını gösteren **Denetim** günlüğü, **ek ayrıntılar** bölümünde bu bilgilerle birlikte:

   - ConditionalAccessResult: koşullu ilke değerlendirmesi için gereken izin.
   - AppliedPolicies: koşulların karşılandığı ve ilkelerin açık olduğu tüm koşullu erişim ilkelerinin bir listesi.
   - ReportingPolicies: yalnızca rapor moduna ayarlanmış koşullu erişim ilkelerinin ve koşulların karşılandığından oluşan bir liste.

## <a name="next-steps"></a>Sonraki adımlar

[Kullanıcı akışına koşullu erişim ekleyin](conditional-access-user-flow.md).
