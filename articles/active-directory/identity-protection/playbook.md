---
title: Azure Active Directory Kimlik Koruması PlayBook | Microsoft Docs
description: Azure AD Kimlik Koruması, bir saldırganın güvenliği aşılmış bir kimlik veya cihazdan yararlanma ve daha önce şüphelenildiği ya da tehlikede olduğu bilinen bir cihazın güvenliğini sağlama becerisini nasıl sağlayabileceğinizi öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7fcf24256634ef11b575348d9da7d6bbbab8b67c
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70127759"
---
# <a name="azure-active-directory-identity-protection-playbook"></a>Azure Active Directory Kimlik Koruması PlayBook

Bu PlayBook şunları yapmanıza yardımcı olur:

* Risk algılamalarını ve güvenlik açıklarını taklit ederek kimlik koruması ortamındaki verileri doldurun
* Risk tabanlı koşullu erişim ilkeleri ayarlayın ve bu ilkelerin etkisini test edin

## <a name="simulating-risk-detections"></a>Risk algılamaları benzetimi yapma

Bu bölümde, aşağıdaki risk algılama türlerinin benzetimini yapma adımları sunulmaktadır:

* Anonim IP adreslerinden oturum açma işlemleri (kolay)
* Bilmediğiniz konumlardan oturum açma işlemleri (orta)
* Atipik konumlara imkansız seyahat (zor)

Diğer risk algılamaları güvenli bir şekilde benzetilemez.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Anonim IP adreslerinden oturum açma işlemleri

Bu risk algılama hakkında daha fazla bilgi için bkz. [anonım IP adreslerinden oturum açma](../reports-monitoring/concept-risk-events.md#sign-ins-from-anonymous-ip-addresses)işlemleri. 

Aşağıdaki yordamı tamamlamak için şunları kullanmanız gerekir:

- Anonim IP adreslerinin benzetimini yapmak için [Tor tarayıcısı](https://www.torproject.org/projects/torbrowser.html.en) . Kuruluşunuz Tor tarayıcısı kullanılarak sınırlandırdıysanız bir sanal makine kullanmanız gerekebilir.
- Henüz Multi-Factor Authentication için kaydedilmemiş bir test hesabı.

**Anonım IP 'den bir oturum açma benzetimi yapmak için aşağıdaki adımları uygulayın**:

1. [Tor tarayıcısını](https://www.torproject.org/projects/torbrowser.html.en)kullanarak öğesine [https://myapps.microsoft.com](https://myapps.microsoft.com)gidin.   
2. **Anonım IP adreslerinden oturum açma** işlemleri raporunda görünmesini istediğiniz hesabın kimlik bilgilerini girin.

Oturum açma, kimlik koruması panosunda 10-15 dakika içinde görüntülenir. 

### <a name="sign-ins-from-unfamiliar-locations"></a>Alışılmadık konumlardan oturum açma işlemleri

Bu risk algılama hakkında daha fazla bilgi için bkz. [Bilinmeyen konumlardan oturum açma](../reports-monitoring/concept-risk-events.md#sign-in-from-unfamiliar-locations)işlemleri. 

Tanıdık konumların benzetimini yapmak için, test hesabınızın öncesinde oturum açmadığı bir konumdan ve cihazdan oturum açmanız gerekir.

Aşağıdaki yordam yeni oluşturulan bir kullanır:

- Yeni konumun benzetimini yapmak için VPN bağlantısı.
- Sanal makine, yeni bir cihazın benzetimini yapar.

Aşağıdaki yordamı tamamlamak için şunları içeren bir kullanıcı hesabı kullanmanız gerekir:

- En az 30 günlük bir oturum açma geçmişi.
- Multi-Factor Authentication etkinleştirildi.

**Bilmediğiniz bir konumdan bir oturum açma benzetimi yapmak için aşağıdaki adımları uygulayın**:

1. Test hesabınızla oturum açarken MFA sınamasını geçirerek MFA sınamasını başarısız yapın.
2. Yeni VPN 'nizi kullanarak şuraya gidin [https://myapps.microsoft.com](https://myapps.microsoft.com) ve test hesabınızın kimlik bilgilerini girin.

Oturum açma, kimlik koruması panosunda 10-15 dakika içinde görüntülenir.

### <a name="impossible-travel-to-atypical-location"></a>Olağan konuma imkansız seyahat

Bu risk algılama hakkında daha fazla bilgi için bkz. [normal konuma yönelik imkansız seyahat](../reports-monitoring/concept-risk-events.md#impossible-travel-to-atypical-locations). 

Algoritma, yabancı olmayan seyahat koşulunun benzetimini yapmak zordur çünkü algoritma, bilinen cihazlardan veya dizindeki diğer kullanıcılar tarafından kullanılan VPN 'lerden oturum açma işlemleri gibi hatalı pozitif sonuçlar almak için makine öğrenimini kullanır. Ayrıca, algoritma, risk algılamalarını oluşturmaya başlamadan önce 14 gün ve Kullanıcı için 10 oturum açma geçmişi gerektirir. Karmaşık makine öğrenimi modelleri ve yukarıdaki kurallar nedeniyle, aşağıdaki adımların bir risk algılamasına neden olmayacağı bir şansınız vardır. Bu risk algılamayı yayımlamak için bu adımları birden çok Azure AD hesabı için çoğaltmak isteyebilirsiniz.

**Sıradan bir konuma imkansız bir seyahat benzetimi yapmak için aşağıdaki adımları uygulayın**:

1. Standart tarayıcınızı kullanarak öğesine [https://myapps.microsoft.com](https://myapps.microsoft.com)gidin.  
2. İçin imkansız bir seyahat riski algılaması üretmek istediğiniz hesabın kimlik bilgilerini girin.
3. Kullanıcı aracınızı değiştirin. Internet Explorer 'daki Kullanıcı aracısını Geliştirici Araçları ' dan değiştirebilir veya Kullanıcı Aracısı değiştirici eklentisini kullanarak Firefox veya Chrome 'daki Kullanıcı aracınızı değiştirebilirsiniz.
4. IP adresinizi değiştirin. Bir VPN, Tor eklentisi kullanarak IP adresinizi değiştirebilir veya farklı bir veri merkezinde Azure 'da yeni bir makine getirebilirsiniz.
5. Önceki oturum açma işleminden daha önce ve birkaç dakika içinde aynı kimlik bilgilerini [kullanarakoturumaçın. https://myapps.microsoft.com](https://myapps.microsoft.com)

Oturum açma, kimlik koruması panosunda 2-4 saat içinde görüntülenir.

## <a name="simulating-vulnerabilities"></a>Güvenlik açıklarının benzetimini yapma
Güvenlik açıkları, bir Azure AD ortamında kötü bir aktör tarafından yararlanılabilen zayıflardır. Şu anda 3 tür güvenlik açığı, Azure AD 'nin diğer özelliklerinden yararlanan Azure AD Kimlik Koruması ortaya çıkmış. Bu güvenlik açıkları, bu özellikler kurulduktan sonra kimlik koruması panosunda otomatik olarak görüntülenir.

* Azure AD [Multi-Factor Authentication](../authentication/multi-factor-authentication.md)
* Azure AD [Cloud Discovery](https://docs.microsoft.com/cloud-app-security/).
* Azure AD [Privileged Identity Management](../privileged-identity-management/pim-configure.md). 

## <a name="testing-security-policies"></a>Güvenlik ilkelerini test etme

Bu bölüm, Kullanıcı riskini ve oturum açma riski güvenlik ilkesini test etmek için gereken adımları sağlar.

### <a name="user-risk-security-policy"></a>Kullanıcı riski güvenlik ilkesi

Daha fazla bilgi için bkz. [Kullanıcı risk ilkesini yapılandırma](howto-user-risk-policy.md).

![Kullanıcı riski](./media/playbook/02.png "PlayBook")

**Bir Kullanıcı risk güvenlik ilkesini test etmek için aşağıdaki adımları gerçekleştirin**:

1. [https://portal.azure.com](https://portal.azure.com) Kiracınız için genel yönetici kimlik bilgileriyle oturum açın.
2. **Kimlik koruması**' na gidin. 
3. **Azure AD kimlik koruması** sayfasında, **Kullanıcı risk ilkesi**' ne tıklayın.
4. **Atamalar** bölümünde, istenen kullanıcılar (ve gruplar) ve Kullanıcı risk düzeyini seçin.

    ![Kullanıcı riski](./media/playbook/03.png "PlayBook")

5. Denetimler bölümünde, istenen erişim denetimini (ör. parola değişikliği gerektir) seçin.
5. **Ilkeyi zorunlu kıl**' ıseçin.
6. Bir sınama hesabının kullanıcı riskini, örneğin, risk algılarından birinin birkaç kez benzetimini yaparak yükseltin.
7. Birkaç dakika bekleyin ve ardından Kullanıcı için Kullanıcı düzeyinin Orta olduğunu doğrulayın. Aksi takdirde, Kullanıcı için daha fazla risk algılamalarını taklit edin.
8. **Ilkeyi zorla**' yı seçin.
9. Artık yükseltilmiş risk düzeyine sahip bir kullanıcı kullanarak oturum açarak Kullanıcı risk tabanlı koşullu erişimi test edebilirsiniz.

### <a name="sign-in-risk-security-policy"></a>Oturum açma riski güvenlik ilkesi

Daha fazla bilgi için bkz. [Oturum açma risk ilkesini yapılandırma](howto-sign-in-risk-policy.md).

![Oturum açma riski](./media/playbook/01.png "PlayBook")

**Bir oturum açma risk ilkesini test etmek için aşağıdaki adımları gerçekleştirin:**

1. [https://portal.azure.com](https://portal.azure.com) Kiracınız için genel yönetici kimlik bilgileriyle oturum açın.
2. **Azure AD kimlik koruması**gidin.
3. Ana **Azure AD kimlik koruması** sayfasında **oturum açma risk ilkesi**' ne tıklayın. 
4. **Atamalar** bölümünde, istenen kullanıcılar (ve gruplar) ve oturum açma risk düzeyini seçin.

    ![Oturum açma riski](./media/playbook/04.png "PlayBook")

5. **Denetimler** bölümünde, istenen erişim denetimini seçin (örneğin, **çok faktörlü kimlik doğrulaması gerektir**). 
6. **Ilkeyi zorla**' yı seçin.
7. **Kaydet**’e tıklayın.
8. Artık riskli bir oturum kullanarak oturum açma riski tabanlı koşullu erişimi test edebilirsiniz (örneğin, Tor tarayıcısını kullanarak). 

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Active Directory Kimlik Koruması](../active-directory-identityprotection.md)
