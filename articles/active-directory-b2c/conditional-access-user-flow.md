---
title: Azure AD B2C bir Kullanıcı akışına koşullu erişim ekleme
description: Azure AD B2C Kullanıcı akışlarınıza koşullu erişim eklemeyi öğrenin. İlke zorlamak ve riskli oturum açma işlemlerini düzeltmek için Kullanıcı akışlarınızda Multi-Factor Authentication (MFA) ayarlarını ve koşullu erişim ilkelerini yapılandırın.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: overview
ms.date: 09/01/2020
ms.author: mimart
author: msmimart
manager: celested
ms.collection: M365-identity-device-management
ms.openlocfilehash: 60bfac3b80e772e7b359b1e926d5fb84e447a8fb
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89271612"
---
# <a name="add-conditional-access-to-user-flows-in-azure-active-directory-b2c"></a>Azure Active Directory B2C Kullanıcı akışlarına koşullu erişim ekleme

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

Uygulamalarınıza yönelik riskli oturum açma işlemlerini yönetmek için, Azure Active Directory B2C Kullanıcı akışlarınıza koşullu erişim eklenebilir. Azure AD B2C ' deki kimlik koruması ve Koşullu erişimin tümleştirilmesi, riskli oturum açma davranışını belirleyen ve Kullanıcı veya yöneticiden daha fazla işlem gerektiren ilkeleri uygulayan ilkeler ayarlamanıza olanak sağlar. Bunlar Azure AD B2C Kullanıcı akışlarında koşullu erişimi etkinleştiren bileşenlerdir:

- **Kullanıcı akışı**. Kullanıcıya oturum açma ve kaydolma işlemi aracılığıyla rehberlik eden bir Kullanıcı akışı oluşturun. Kullanıcı akış ayarları ' nda, bir Kullanıcı Kullanıcı akışını izlediğinde koşullu erişim ilkelerinin etkinleştirilip etkinleştirilmeyeceğini belirtin.
- **Kullanıcıları Kullanıcı akışına yönlendiren uygulama**. Uygulamanızı, uygulamadaki Kullanıcı akış uç noktasını belirterek, kullanıcıları uygun kaydolma ve oturum açma Kullanıcı akışına yönlendirmek üzere yapılandırın.
- **Koşullu erişim ilkesi**. [Bir koşullu erişim Ilkesi oluşturun](conditional-access-identity-protection-setup.md) ve ilkenin uygulanmasını istediğiniz uygulamaları belirtin. Kullanıcı, uygulamanız için oturum açma veya kaydolma Kullanıcı akışını izlediğinde, koşullu erişim ilkesi riskli oturum açma işlemlerini belirlemek için kimlik koruması sinyallerini kullanır ve gerekirse uygun düzeltme eylemini gösterir.

Koşullu erişim, Kullanıcı akışlarının en son sürümlerinde desteklenir. Bunları oluştururken Yeni Kullanıcı akışlarına koşullu erişim ilkeleri ekleyebilir veya sürüm koşullu erişimi desteklediği sürece mevcut Kullanıcı akışlarına ekleyebilirsiniz. Mevcut bir Kullanıcı akışına koşullu erişim eklerken, gözden geçirmeniz gereken iki ayar vardır:

- **Multi-Factor Authentication (MFA)**: KULLANıCıLAR artık SMS veya Voice aracılığıyla tek seferlik bir kod veya Multi-Factor Authentication için e-posta aracılığıyla bir kerelik parola kullanabilir. MFA ayarları, koşullu erişim ayarlarından bağımsızdır. MFA 'yı **her zaman açık** olarak ayarlayabilirsiniz, bu sayede, koşullu erişim kurulumundan BAĞıMSıZ olarak MFA her zaman gereklidir. Ya da MFA 'yı **koşullu** olarak ayarlayabilirsiniz, böylelikle MFA yalnızca etkin bir koşullu erişim ilkesi gerektirdiğinde gereklidir.

- **Koşullu erişim**: Bu ayar her zaman **Açık**olmalıdır. Genellikle, bu ayarı sorun giderme veya geçiş sırasında veya eski uygulamalar **için kapatabilirsiniz.**

Azure AD B2C ' de [kimlik koruması ve koşullu erişim](conditional-access-identity-protection-overview.md) hakkında daha fazla bilgi edinin veya [nasıl ayarlanacağını](conditional-access-identity-protection-setup.md)öğrenin.

## <a name="add-conditional-access-to-a-new-user-flow"></a>Yeni bir Kullanıcı akışına koşullu erişim ekleme

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Portal araç çubuğunda **Dizin + abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure portal, araması yapın ve **Azure AD B2C**seçin.
1. **İlkeler**altında **Kullanıcı akışları**' nı seçin ve ardından **Yeni Kullanıcı akışı**' nı seçin.
1. **Kullanıcı akışı oluştur** sayfasında, Kullanıcı akış türünü seçin.
1. **Sürüm seçin**altında **Önerilen**' i seçin ve ardından **Oluştur**' u seçin. (Kullanıcı akışı sürümleri hakkında[daha fazla bilgi edinin](user-flow-versions.md) .)

    ![Azure portal ' de vurgulanan özelliklerle Kullanıcı akışı oluşturma sayfası](./media/tutorial-create-user-flows/select-version.png)

1. Kullanıcı akışı için bir **ad** girin. Örneğin, *signupsignin1*.
1. **Kimlik sağlayıcıları** bölümünde, bu kullanıcı akışı için izin vermek istediğiniz kimlik sağlayıcılarını seçin.
2. **Çok faktörlü kimlik doğrulaması** bölümünde, istenen **MFA yöntemini**seçin ve ardından **MFA zorlaması** altında **koşullu (önerilen)** seçeneğini belirleyin.
 
   ![Çok faktörlü kimlik doğrulamasını yapılandırma](media/conditional-access-user-flow/configure-mfa.png)

1. **Koşullu erişim** bölümünde **koşullu erişim ilkelerini zorla** onay kutusunu seçin.

   ![Koşullu erişim ayarlarını yapılandırma](media/conditional-access-user-flow/configure-conditional-access.png)

1. **Kullanıcı öznitelikleri ve talepler** bölümünde, kayıt sırasında kullanıcıdan toplamak ve göndermek istediğiniz talepleri ve öznitelikleri seçin. Örneğin, **daha fazla göster**' i seçin ve ardından **ülke/bölge** ve **görünen ad**için öznitelikler ve talepler ' i seçin. **Tamam**’ı seçin.

    ![Üç talep seçili olan öznitelikler ve talepler seçim sayfası](./media/conditional-access-user-flow/configure-user-attributes-claims.png)

1. Kullanıcı akışını eklemek için **Oluştur** ' a tıklayın. *B2C_1* bir ön eki otomatik olarak ada eklenir.

## <a name="add-conditional-access-to-an-existing-user-flow"></a>Mevcut bir Kullanıcı akışına koşullu erişim ekleme

> [!NOTE]
> Mevcut Kullanıcı akışı, koşullu erişimi destekleyen bir sürüm olmalıdır. Bu Kullanıcı akışı sürümlerinin **kullanılması önerilir**.

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. Portal araç çubuğunda **Dizin + abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.

1. Azure portal, araması yapın ve **Azure AD B2C**seçin.

1. **İlkeler**altında **Kullanıcı akışları**' nı seçin. Ardından Kullanıcı akışını seçin.

1. **Özellikler** **' i** seçin ve **koşullu erişim**etiketli ayarı arayarak Kullanıcı akışının koşullu erişimi desteklediğinden emin olun.
 
   ![Özelliklerde MFA ve koşullu erişimi yapılandırma](media/conditional-access-user-flow/add-conditional-access.png)

1. **Çok faktörlü kimlik doğrulaması** bölümünde, istenen **MFA yöntemini**seçin ve ardından **MFA zorlaması** altında **koşullu (önerilen)** seçeneğini belirleyin.
 
1. **Koşullu erişim** bölümünde **koşullu erişim ilkelerini zorla** onay kutusunu seçin.

1. **Kaydet**’i seçin.

## <a name="test-the-user-flow"></a>Kullanıcı akışını test etme

Kullanıcı akışındaki koşullu erişimi test etmek için, [bir koşullu erişim ilkesi oluşturun](conditional-access-identity-protection-setup.md) ve yukarıda açıklandığı gibi Kullanıcı akışınızda koşullu erişimi etkinleştirin. 

### <a name="prerequisites"></a>Ön koşullar

- Riskli oturum açma ilkeleri oluşturmak için Azure AD B2C Premium 2 gereklidir. Premium P1 kiracılar konum, uygulama veya grup tabanlı ilkeler oluşturabilir.
- Sınama amacıyla, [register the test web application](tutorial-register-applications.md) `https://jwt.ms` bir belirtecin kodu çözülmüş Içeriğini görüntüleyen, Microsoft 'a ait bir Web uygulaması olan test Web uygulamasını kaydedebilirsiniz (belirtecin içeriği hiçbir şekilde tarayıcıdan ayrılmayın). 
- Riskli bir oturum açma benzetimi yapmak için TOR tarayıcısını indirin ve Kullanıcı akış uç noktasında oturum açmayı deneyin.
- Aşağıdaki ayarları kullanarak, [bir koşullu erişim ilkesi oluşturun](conditional-access-identity-protection-setup.md):
   
   - **Kullanıcılar ve gruplar**için, test kullanıcısını seçin ( **tüm kullanıcıları** seçmeyin veya kendi oturum açmasını engelleyebilirsiniz).
   - **Bulut uygulamaları veya eylemler**Için **Uygulama Seç**' i seçin ve ardından bağlı olan taraf uygulamanızı seçin.
   - Koşullar için **oturum açma riski** ve **yüksek**, **Orta**ve **düşük** riskli Düzeyler ' i seçin.
   - **Izin ver**Için **erişimi engelle**' yi seçin.

      ![Risk algılamaları](media/conditional-access-identity-protection-setup/test-conditional-access-policy.png)

### <a name="run-the-user-flow"></a>Kullanıcı akışını çalıştırma

1. Genel Bakış sayfasını açmak için oluşturduğunuz kullanıcı akışını seçin ve ardından **Kullanıcı akışını Çalıştır**' ı seçin. **Uygulama**altında *WebApp1*öğesini seçin. **Yanıt URL 'si** gösterilmesi gerekir `https://jwt.ms` .

   ![Kullanıcı akışını Çalıştır düğmesi vurgulanmış şekilde portalda Kullanıcı akış sayfasını Çalıştır](./media/tutorial-create-user-flows/signup-signin-run-now.PNG)

1. **Kullanıcı akış uç noktası çalıştırma**altındaki URL 'yi kopyalayın.

1. Riskli oturum açma benzetimi yapmak için, [Tor tarayıcısını](https://www.torproject.org/download/) açın ve kayıtlı uygulamada oturum açmak için önizleme adımında kopyaladığınız URL 'yi kullanın.

1. Oturum açma sayfasında istenen bilgileri girin ve oturum açmayı deneyin. Belirteç öğesine döner `https://jwt.ms` ve size gösterilmesi gerekir. Jwt.ms kodu çözülen belirteçte, oturum açma 'nın engellendiğini görmeniz gerekir:

   ![Engellenen bir oturum açma sınamasını yapın](media/conditional-access-identity-protection-setup/test-blocked-sign-in.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD B2C Kullanıcı akışında Kullanıcı arabirimini özelleştirme](customize-ui-overview.md)
