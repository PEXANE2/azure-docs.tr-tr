---
title: Azure AD Kimlik Koruması'nda risk algılamalarının benzetimi
description: Kimlik Koruması'nda risk algılamaları nasıl simüle edilenöğrenin
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 10/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: e50e406e74c0b78f41830c4ea7c5b10830002ed3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72886941"
---
# <a name="simulating-risk-detections-in-identity-protection"></a>Kimlik Korumada risk tespitlerinin simüle edilmesi

Yöneticiler, aşağıdaki öğeleri gerçekleştirmek için ortamlarındaki riski simüle etmek isteyebilirler:

- Risk algılamaları ve güvenlik açıklarını simüle ederek kimlik koruma ortamındaki verileri doldurun.
- Risk tabanlı Koşullu Erişim ilkeleri ayarlayın ve bu ilkelerin etkisini test edin.

Bu makalede, aşağıdaki risk algılama türlerini simüle etmek için adımlar sağlar:

- Anonim IP adresi (kolay)
- Yabancı oturum açma özellikleri (orta)
- Atipik seyahat (zor)

Diğer risk algılamaları güvenli bir şekilde simüle edilemez.

Her risk tespiti hakkında daha fazla bilgi makalede bulunabilir, [Risk nedir](concept-identity-protection-risks.md).

## <a name="anonymous-ip-address"></a>Anonim IP adresi

Aşağıdaki yordamı tamamlamak için kullanmanız gerekmektedir:

- Anonim IP adreslerini simüle etmek için [Tor Browser.](https://www.torproject.org/projects/torbrowser.html.en) Kuruluşunuz Tor tarayıcısını kullanmayı kısıtlarsa sanal bir makine kullanmanız gerekebilir.
- Azure Çok Faktörlü Kimlik Doğrulaması için henüz kaydedilmemiş bir test hesabı.

**Anonim bir IP'den oturum açma simüle etmek için aşağıdaki adımları gerçekleştirin:**

1. [Tor Browser'ı](https://www.torproject.org/projects/torbrowser.html.en)kullanarak [https://myapps.microsoft.com](https://myapps.microsoft.com), ' e gidin.   
2. Anonim IP adresleri raporundan **Oturum Açma'larda** görünmesini istediğiniz hesabın kimlik bilgilerini girin.

Oturum açma 10-15 dakika içinde Kimlik Koruması panosunda belirir. 

## <a name="unfamiliar-sign-in-properties"></a>Bilmediğiniz oturum açma özellikleri

Bilmediğiniz konumları simüle etmek için, test hesabınızın daha önce oturum açmadığı bir konumdan ve cihazdan oturum açmanız gerekir.

Aşağıdaki yordam, yeni oluşturulan bir kullanır:

- VPN bağlantısı, yeni konumu simüle etmek için.
- Sanal makine, yeni bir cihaz simüle etmek için.

Aşağıdaki yordamı tamamlamak için aşağıdakilere sahip bir kullanıcı hesabı kullanmanız gerekmektedir:

- En az 30 günlük bir oturum açma tarihi.
- Azure Çok Faktörlü Kimlik Doğrulama etkinleştirildi.

**Bilmediğiniz bir konumdan oturum açma yı simüle etmek için aşağıdaki adımları gerçekleştirin:**

1. Test hesabınızla oturum açtığınızda, MFA meydan okumasını geçerek çok faktörlü kimlik doğrulama (MFA) mücadelesinden başarısız olun.
2. Yeni VPN'inizi kullanarak, test hesabınızın kimlik bilgilerine gidin [https://myapps.microsoft.com](https://myapps.microsoft.com) ve girin.

Oturum açma 10-15 dakika içinde Kimlik Koruması panosunda belirir.

## <a name="atypical-travel"></a>Atipik seyahat

Algoritma, tanıdık cihazlardan atipik seyahat veya dizindeki diğer kullanıcılar tarafından kullanılan VPN'lerden oturum açma gibi yanlış pozitifleri saymak için makine öğrenimini kullandığından, atipik seyahat durumunu simüle etmek zordur. Ayrıca, algoritma risk algılamaları oluşturmaya başlamadan önce kullanıcının 14 günlük oturum açma geçmişini ve 10 oturum açmayı gerektirir. Karmaşık makine öğrenme modelleri ve yukarıdaki kurallar nedeniyle, aşağıdaki adımların risk tespitine yol açmama olasılığı vardır. Bu algılamayı simüle etmek için birden çok Azure REKLAM hesabı için bu adımları çoğaltmak isteyebilirsiniz.

**Atipik bir seyahat riski algılamasını simüle etmek için aşağıdaki adımları gerçekleştirin:**

1. Standart tarayıcınızı [https://myapps.microsoft.com](https://myapps.microsoft.com)kullanarak.  
2. Atipik bir seyahat riski algılama oluşturmak istediğiniz hesabın kimlik bilgilerini girin.
3. Kullanıcı aracınızı değiştirin. Microsoft Edge'deki kullanıcı aracını Geliştirici Araçları'ndan (F12) değiştirebilirsiniz.
4. IP adresinizi değiştirin. VPN, Tor eklentisi kullanarak veya Azure'da farklı bir veri merkezinde yeni bir sanal makine oluşturarak IP adresinizi değiştirebilirsiniz.
5. Önceki oturum [https://myapps.microsoft.com](https://myapps.microsoft.com) açmadan sonra ve birkaç dakika içinde önceki yle aynı kimlik bilgilerini kullanmak için oturum açın.

Oturum açma 2-4 saat içinde Kimlik Koruması panosunda belirir.

## <a name="testing-risk-policies"></a>Risk politikalarını test etme

Bu bölümde, kullanıcıyı ve oturum açma risk ilkelerini sınamak için adımlar sağlar, [nasıl oluşturulur: Risk ilkelerini yapılandırmave etkinleştirme.](howto-identity-protection-configure-risk-policies.md)

### <a name="user-risk-policy"></a>Kullanıcı risk ilkesi

Kullanıcı risk güvenlik ilkesini sınamak için aşağıdaki adımları gerçekleştirin:

1. [Azure portalına](https://portal.azure.com) gidin.
1. **Azure Etkin Dizin** > **Güvenliğine** > **Genel Bakış'a**göz atın.
1. **Kullanıcı risk ilkesini yapılandır'ı**seçin.
   1. **Atamalar** Altında
      1. **Kullanıcılar** - Ürününüzü sınırlandırıyorsa **Tüm kullanıcıları** seçin veya kişileri ve **grupları seçin.**
         1. İsteğe bağlı olarak, kullanıcıları ilkeden hariç tutmayı seçebilirsiniz.
      1. **Koşullar** - **Kullanıcı riski** Microsoft'un önerisi yüksek bu seçeneği ayarlamaktır. **High**
   1. **Denetimler** Altında
      1. **Access** - Microsoft'un önerisi **erişime izin** vermek ve **parola değişikliği ni gerektirmektir.**
   1. **İlkeYi** - **Uygulama**
   1. **Kaydet** - Bu eylem sizi **Genel Bakış** sayfasına döndürecektir.
1. Örneğin, bir test hesabının kullanıcı riskini birkaç kez risk tespitlerinden birini simüle ederek yükseltin.
1. Birkaç dakika bekleyin ve ardından kullanıcınız için bu riskin yükseldiğini doğrulayın. Değilse, kullanıcı için daha fazla risk algılaması simüle edin.
1. Risk politikanıza geri dönün ve Politika değişikliğini **uygulamak** ve **kaydetmek** için **İlkeyi** Uygulayın'ı ayarlayın.
1. Artık yüksek risk düzeyine sahip bir kullanıcı kullanarak oturum imzalayarak kullanıcı risk tabanlı Koşullu Erişimi sınayabilirsiniz.

### <a name="sign-in-risk-security-policy"></a>Oturum açma riski güvenlik ilkesi

Risk ilkesinde bir oturum u test etmek için aşağıdaki adımları gerçekleştirin:

1. [Azure portalına](https://portal.azure.com) gidin.
1. **Azure Etkin Dizin** > **Güvenliğine** > **Genel Bakış'a**göz atın.
1. **Oturum açma risk ilkesini yapılandır'ı**seçin.
   1. **Atamalar** Altında
      1. **Kullanıcılar** - Ürününüzü sınırlandırıyorsa **Tüm kullanıcıları** seçin veya kişileri ve **grupları seçin.**
         1. İsteğe bağlı olarak, kullanıcıları ilkeden hariç tutmayı seçebilirsiniz.
      1. **Koşullar** - **Oturum Açma riski** Microsoft'un önerisi bu seçeneği Orta ve **üzeri**olarak ayarlamaktır.
   1. **Denetimler** Altında
      1. **Access** - Microsoft'un önerisi **erişime izin** vermek ve **çok faktörlü kimlik doğrulamayı**gerektirmektir.
   1. **Politikayı** - **Uygula**
   1. **Kaydet** - Bu eylem sizi **Genel Bakış** sayfasına döndürecektir.
1. Artık riskli bir oturum kullanarak oturum açarak Oturum Açma Risk Tabanlı Koşullu Erişimi sınayabilirsiniz (örneğin, Tor tarayıcısını kullanarak). 

## <a name="next-steps"></a>Sonraki adımlar

- [Risk nedir?](concept-identity-protection-risks.md)

- [Nasıl Yapilir: Risk ilkelerini yapılandırma ve etkinleştirme](howto-identity-protection-configure-risk-policies.md)

- [Azure Active Directory Kimlik Koruması](overview-identity-protection.md)
