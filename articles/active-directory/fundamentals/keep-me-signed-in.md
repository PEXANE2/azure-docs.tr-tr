---
title: "' Oturum açmış kalsın ' öğesini yapılandırın Azure Active Directory hesapları istemi"
description: Oturumumu Açık tut (KMSı) hakkında bilgi edinin komut istemi, Azure Active Directory portalında yapılandırma ve oturum açma sorunlarını giderme.
services: active-directory
author: CelesteDG
manager: daveba
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.subservice: fundamentals
ms.date: 06/05/2020
ms.author: celested
ms.reviewer: asteen, jlu, hirsin
ms.collection: M365-identity-device-management
ms.openlocfilehash: bed6bc43dfc15abf2bdf9f38a5de2240d348d6fb
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89320265"
---
# <a name="configure-the-stay-signed-in-prompt-for-azure-ad-accounts"></a>' Oturum açmış kalsın ' öğesini yapılandırın Azure AD hesapları istemi

Oturumumu Açık tut (KMSı) **, oturum açmış bir oturum açıp açmamızı ister** . Bir Kullanıcı Bu istem için **Evet** yanıtını verirse, Oturumumu Açık tut hizmeti bunlara kalıcı [yenileme belirteci](../develop/developer-glossary.md#refresh-token)verir. Federasyon kiracılar için, kullanıcı federe kimlik hizmeti ile kimlik doğrulamasından geçtikten sonra istem görünür.

Aşağıdaki diyagramda, yönetilen bir kiracı ve Federal kiracı için Kullanıcı oturum açma akışı ve Oturumumu Açık bırak isteği gösterilmektedir. Machine Learning sistemi, paylaşılan bir cihazdan yüksek riskli oturum açma veya oturum açma algıladığında, bu akış Akıllı mantık **'yi** içerir.

:::image type="content" source="./media/keep-me-signed-in/kmsi-workflow.png" alt-text="Yönetilen ve Federal kiracı için Kullanıcı oturum açma akışını gösteren diyagram":::

> [!NOTE]
> Oturumumu Açık tut seçeneğinin yapılandırılması, Azure Active Directory (Azure AD) Premium 1, Premium 2 veya temel sürümlerini kullanmanızı veya bir Microsoft 365 lisansına sahip olmanızı gerektirir. Lisanslama ve sürümler hakkında daha fazla bilgi için bkz. [Azure AD Premium kaydolma](active-directory-get-started-premium.md).<br><br>Azure AD Premium ve temel sürümler, Çin 'deki müşteriler için Azure AD 'nin dünya çapındaki örneğini kullanarak kullanılabilir. Azure AD Premium ve Temel sürümleri, şu anda Çin’de 21Vianet tarafından işletilen Azure hizmeti kapsamında desteklenmemektedir. Daha fazla bilgi için [Azure AD Forumu](https://feedback.azure.com/forums/169401-azure-active-directory/)ile bizimle iletişim kurun.

## <a name="configure-kmsi"></a>KMSI 'yi yapılandırma

1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com/) oturum açın.
1. **Azure Active Directory**' yi seçin, **Şirket markasını**seçin ve ardından **Yapılandır**' ı seçin.
1. **Gelişmiş ayarlar** bölümünde, **oturum açmış olarak kalan ayarı göster seçeneğini** bulun.

   Bu ayar, kullanıcılarınızın açık olarak oturumu açana kadar Azure AD 'de oturum açmış olup olmayacağını seçmenize olanak sağlar.
   * **Hayır**' ı seçerseniz, **oturum açık kalsın mı?** seçeneği, Kullanıcı başarıyla oturum açtıktan sonra ve kullanıcının her kapatılıp yeniden açıldığı zaman oturum açması gerekir.
   * **Evet**' i seçerseniz, **oturum açmış durumda kal?** seçeneği kullanıcıya gösterilir.

    :::image type="content" source="./media/keep-me-signed-in/kmsi-company-branding-advanced-settings-kmsi-1.png" alt-text="Ekran görüntüsü, oturum açmış olarak kalan ayarı göster seçeneğini gösterir":::

## <a name="troubleshoot-sign-in-issues"></a>Oturum açma sorunlarını giderme

Bir Kullanıcı, aşağıdaki diyagramda gösterildiği gibi oturum **açmış kalmak** için işlem yapmaz, ancak oturum açma girişimini terk ederseniz, kesmeyi gösteren bir oturum açma günlüğü girişi görürsünüz.

:::image type="content" source="./media/keep-me-signed-in/kmsi-stay-signed-in-prompt.png" alt-text="Oturum açık kalsın ' i gösterir mi? isteme":::

Oturum açma hatası hakkındaki ayrıntılar aşağıda verilmiştir ve örnekte vurgulanır.

* **Oturum açma hata kodu**: 50140
* **Hata nedeni**: Kullanıcı oturum açarken "Oturumumu Açık tut" kesmesi nedeniyle bu hata oluştu.

:::image type="content" source="./media/keep-me-signed-in/kmsi-sign-ins-log-entry.png" alt-text="Oturumumu Açık tut kesintiye sahip örnek oturum açma günlüğü girişi":::

Gelişmiş marka ayarlarında **göster seçeneğini oturum açmış olarak kalacak şekilde** ayarlayarak, kullanıcıların kesme işlemini görmesini durdurabilirsiniz. **No** Bu, Azure AD dizininizde tüm kullanıcılar için KMSı isteğini devre dışı bırakır.

Ayrıca, kullanıcıların KMSı sorgusunu gördüğünü engellemek için koşullu erişim 'teki kalıcı tarayıcı oturumu denetimlerini de kullanabilirsiniz. Bu seçenek, dizinde kalan kullanıcılar için oturum açma davranışını etkilemeden, belirli bir Kullanıcı grubu (Genel Yöneticiler gibi) için KMSı isteğini devre dışı bırakmanızı sağlar. Daha fazla bilgi için bkz. [Kullanıcı oturum açma sıklığı](../conditional-access/howto-conditional-access-session-lifetime.md). 

KMSı isteminin yalnızca kullanıcıya yararlanabileceği durumlarda gösterildiğinden emin olmak için, aşağıdaki senaryolarda KMSı istemi özellikle gösterilmez:

* Kullanıcı sorunsuz SSO ve tümleşik Windows kimlik doğrulaması (ıWA) aracılığıyla oturum açtı
* Kullanıcı Active Directory Federasyon Hizmetleri (AD FS) ve ıWA aracılığıyla oturum açtı
* Kullanıcı Kiracıdaki bir konudır
* Kullanıcının risk puanı yüksek
* Kullanıcı veya yönetici onay akışı sırasında oturum açma işlemi gerçekleşir
* Kalıcı tarayıcı oturumu denetimi, koşullu erişim ilkesinde yapılandırılır

## <a name="next-steps"></a>Sonraki adımlar

Oturum açma oturum zaman aşımını etkileyen diğer ayarlar hakkında bilgi edinin:

* Microsoft 365 – [Boşta oturum zaman aşımı](/sharepoint/sign-out-inactive-users)
* Azure AD koşullu erişim- [Kullanıcı oturum açma sıklığı](../conditional-access/howto-conditional-access-session-lifetime.md)
* Azure portal – [dizin düzeyindeki eylemsizlik zaman aşımı](../../azure-portal/set-preferences.md#change-the-directory-timeout-setting-admin)