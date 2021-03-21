---
title: Azure Active Directory B2C Identity Protection ile riski araştırın
description: Azure AD B2C kimlik koruması 'nda riskli kullanıcıları ve algılamaları İnceleme hakkında bilgi edinin
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: overview
ms.date: 03/03/2021
ms.custom: project-no-code
ms.author: mimart
author: msmimart
manager: celested
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: 8919285f31e04a51ce10afe3313b28cf86b64ee0
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102055718"
---
# <a name="investigate-risk-with-identity-protection-in-azure-ad-b2c"></a>Azure AD B2C kimlik koruması riskini araştırın

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

Kimlik koruması, Azure AD B2C kiracınız için sürekli risk algılama sağlar. Kuruluşların kimlik tabanlı riskleri bulmasına, araştırmasına ve düzeltmesine olanak tanır. Kimlik koruması, Azure AD B2C kiracılarda kimlik risklerini araştırmak için kullanılabilen risk raporları sunar. Bu makalede riskleri araştırıp azaltmanıza öğrenirsiniz.

## <a name="overview"></a>Genel Bakış

Azure AD B2C kimlik koruması iki rapor sağlar. *Riskli kullanıcılar* raporu, yöneticilerin hangi kullanıcıların risk altında olduğunu ve algılamalar hakkındaki ayrıntıları bulabilecekleri yerdir. *Risk algılamaları* raporu, her risk algılama hakkında, türü, aynı anda tetiklenen diğer riskler, oturum açma denemesi konumu ve daha fazlasını içeren bilgiler sağlar.

Her rapor, raporun en üstünde gösterilen süre için tüm algılamalardan oluşan bir liste ile başlatılır. Raporlar, raporun üst kısmında filtreler kullanılarak filtrelenebilir. Yöneticiler, verileri sürekli olarak dışarı aktarmak için verileri indirmeyi veya [MS Graph API ve POWERSHELL SDK Microsoft Graph](../active-directory/identity-protection/howto-identity-protection-graph-api.md) kullanmayı seçebilir.

## <a name="service-limitations-and-considerations"></a>Hizmet sınırlamaları ve konuları

Kimlik koruması 'nı kullanırken şunları göz önünde bulundurun:

- Kimlik koruması varsayılan olarak açık.
- Kimlik koruması hem yerel hem de sosyal kimlikler (örneğin, Google veya Facebook) için kullanılabilir. Sosyal kimlikler için Koşullu erişimin etkinleştirilmesi gerekir. Sosyal hesap kimlik bilgileri dış kimlik sağlayıcısı tarafından yönetildiğinden algılama sınırlıdır.
- Azure AD B2C kiracılarda, yalnızca [Azure AD kimlik koruması riskli algılamaların](../active-directory/identity-protection/overview-identity-protection.md) bir alt kümesi kullanılabilir. Aşağıdaki risk algılamaları Azure AD B2C tarafından desteklenir:  

|Risk algılama türü  |Description  |
|---------|---------|
| Olağandışı yolculuk     | Kullanıcının en son oturum açma işlemlerini temel alan bir sıradan konumdan oturum açın.        |
|Anonim IP adresi     | Anonim bir IP adresinden oturum açın (örneğin: Tor tarayıcısı, anonimleştirici VPN 'Ler).        |
|Kötü amaçlı yazılım bağlı IP adresi     | Kötü amaçlı yazılımdan bağlantılı IP adresinden oturum açın.         |
|Bilinmeyen oturum açma özellikleri     | Belirtilen kullanıcı için son gördüğdiğimiz özelliklerle oturum açın.        |
|Yönetici tarafından onaylanan Kullanıcı güvenliği aşılmış    | Bir yönetici, kullanıcının güvenliğinin aşıldığını belirtti.             |
|Parola spreyi     | Parola spreyi saldırısında oturum açın.      |
|Azure AD tehdit bilgileri     | Microsoft 'un dahili ve dış tehdit bilgileri kaynakları, bilinen bir saldırı modelini tanımladı.        |

## <a name="pricing-tier"></a>Fiyatlandırma katmanı

Bazı kimlik koruma özellikleri için Azure AD B2C Premium P2 gereklidir. Gerekirse, [Azure AD B2C fiyatlandırma katmanınızı Premium P2 olarak değiştirin](./billing.md). Aşağıdaki tabloda kimlik koruma özellikleri ve gerekli fiyatlandırma katmanı özetlenmektedir.  

|Özellik   |P1   |P2|
|----------|:-----------:|:------------:|
|Riskli kullanıcılar raporu     |&#x2713; |&#x2713; |
|Riskli kullanıcılar rapor ayrıntıları  | |&#x2713; |
|Riskli kullanıcılar rapor düzeltmesi    | &#x2713; |&#x2713; |
|Risk algılama raporu   |&#x2713;|&#x2713;|
|Risk algılamaları rapor ayrıntıları  ||&#x2713;|
|Rapor indirme |  &#x2713;| &#x2713;|
|MS Graph API erişimi |  &#x2713;| &#x2713;|

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]

## <a name="investigate-risky-users"></a>Riskli kullanıcılar araştırma

Riskli kullanıcılar raporu tarafından sağlanacak bilgiler ile yöneticiler şunları bulabilir:

- Risk altında olan kullanıcılar risk düzeltildi mi, ne de risk kapatıldı mi?
- Algılamalar hakkındaki ayrıntılar
- Tüm riskli oturum açma işlemlerinin geçmişi
- Risk geçmişi
 
Yöneticiler daha sonra bu olaylar üzerinde işlem yapmak için seçim yapabilir. Yöneticiler şunları yapabilir:

- Kullanıcı parolasını sıfırlayın
- Kullanıcının güvenliğini onaylama
- Kullanıcı riskini kapat
- Kullanıcının oturum açmasını engelle
- Azure ATP 'yi kullanarak daha fazla araştırma yapın

### <a name="navigating-the-risky-users-report"></a>Riskli kullanıcılar raporuna gitme

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. Portal araç çubuğunda **Dizin + abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.

1. **Azure hizmetleri** altında **Azure AD B2C**' yi seçin. İsterseniz arama kutusunu kullanarak **Azure AD B2C** bulun ve seçin.

1. **Güvenlik** altında **riskli kullanıcılar (Önizleme)** seçeneğini belirleyin.

   ![Riskli kullanıcılar](media/identity-protection-investigate-risk/risky-users.png)

    Tek tek girişlerin seçilmesi, algılamaların altına bir ayrıntılar penceresi genişletir. Ayrıntılar görünümü, yöneticilerin her bir algılama üzerinde eylemleri araştırmasını ve gerçekleştirmesini sağlar.

    ![Riskli kullanıcılar eylemleri](media/identity-protection-investigate-risk/risky-users-report-actions.png)


## <a name="risk-detections-report"></a>Risk algılama raporu

Risk algılamaları raporu, son 90 güne kadar (üç ay) filtrelenebilir verileri içerir.

Risk algılamaları raporu tarafından sağlanabilecek bilgiler ile yöneticiler şunları bulabilir:

- Türü de dahil olmak üzere her risk algılaması hakkında bilgi.
- Aynı anda tetiklenen diğer riskler.
- Oturum açma denemesi konumu.

Yöneticiler daha sonra toplanan bilgilere göre eylemler gerçekleştirmek için kullanıcının risk veya oturum açma raporuna geri dönerek seçim yapabilir.

### <a name="navigating-the-risk-detections-report"></a>Risk algılamaları raporuna gidiliyor

1. Azure portal, araması yapın ve **Azure AD B2C** seçin.
1. **Güvenlik** altında **risk algılamaları (Önizleme)** seçeneğini belirleyin.

   ![Risk algılamaları](media/identity-protection-investigate-risk/risk-detections.png)


## <a name="next-steps"></a>Sonraki adımlar

- [Kullanıcı akışına koşullu erişim ekleyin](conditional-access-user-flow.md).