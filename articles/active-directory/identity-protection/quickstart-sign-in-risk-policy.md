---
title: Hızlı başlangıç-Azure Active Directory Kimlik Koruması bir oturum riski algılandığında erişimi engelleyin | Microsoft Docs
description: Bu hızlı başlangıçta, oturum riskleri temelinde oturum açma işlemlerini engellemek için bir Azure Active Directory (Azure AD) kimlik koruması oturum açma riski koşullu erişim ilkesini nasıl yapılandırabileceğinizi öğreneceksiniz.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: quickstart
ms.date: 09/13/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1bb1e29735a860f5dc3b6ce8996af9fcd4962871
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68335311"
---
# <a name="quickstart-block-access-when-a-session-risk-is-detected-with-azure-active-directory-identity-protection"></a>Hızlı Başlangıç: Azure Active Directory Kimlik Koruması ile oturum riski algılandığında erişimi engelleyin  

Ortamınızı korumalı tutmak için şüpheli kullanıcıların oturum açmasını engellemek isteyebilirsiniz. Azure Active Directory (Azure AD) kimlik koruması her oturum açmayı analiz eder ve bir oturum açma girişiminin Kullanıcı hesabının meşru sahibi tarafından gerçekleştirilme olasılığını hesaplar. Olasılık (düşük, orta, yüksek), oturum açma riski düzeyi adlı bir hesaplanan değer biçiminde belirtilir. Oturum açma riski koşulunu ayarlayarak, belirli oturum açma risk düzeylerine yanıt vermek için bir oturum açma riski koşullu erişim ilkesi yapılandırabilirsiniz. 

Bu hızlı başlangıçta, bir oturum açma riski koşullu erişim ilkesinin, bir orta ve yukarıdaki oturum açma risk düzeyi algılandığında oturum açmayı engelleyen bir oturum açma koşulu. 

![İlke oluştur](./media/quickstart-sign-in-risk-policy/1004.png)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar 

Bu öğreticide senaryoyu tamamlamak için şunlar gereklidir:

- **Azure AD Premium P2 Edition** -Azure AD kimlik koruması erişimi bir Azure AD Premium P2 özelliğidir. 
- **Kimlik koruması** -bu hızlı başlangıçtaki senaryo kimlik korumasının etkinleştirilmesini gerektirir. Kimlik korumasını nasıl etkinleştireceğinizi bilmiyorsanız, bkz. [Azure Active Directory kimlik koruması etkinleştirme](../identity-protection/enable.md).
- **Tor tarayıcısı** - [Tor tarayıcısı](https://www.torproject.org/projects/torbrowser.html.en) , gizliliğinizi çevrimiçi korumanıza yardımcı olacak şekilde tasarlanmıştır. Kimlik koruması, orta düzeyde risk düzeyine sahip **anonım IP adreslerinden oturum açma**işlemleri olarak bir Tor tarayıcısından oturum açma işlemi algılar. Daha fazla bilgi için bkz. [Azure Active Directory risk olayları](../reports-monitoring/concept-risk-events.md).  
- **Charon adlı bir test hesabı** -test hesabı oluşturmayı bilmiyorsanız, bkz. [Yeni Kullanıcı ekleme](../fundamentals/add-users-azure-active-directory.md#add-a-new-user).

## <a name="test-your-sign-in"></a>Oturum açma bilgilerinizi test etme 

Bu adımın amacı, test hesabınızın Tor tarayıcısını kullanarak kiracınıza erişebildiğinizden emin olmaktır.

**Oturum açma bilgilerinizi test etmek için:**

1. [Azure Portal](https://portal.azure.com) Için **Charon**olarak oturum açın.
2. Oturumunuzu kapatın. 

## <a name="create-your-conditional-access-policy"></a>Koşullu erişim ilkenizi oluşturma 

Bu hızlı başlangıçtaki senaryo, **anonım IP adresleri** risk olayından algılanan bir oturum açma işlemleri oluşturmak Için Tor tarayıcısından bir oturum açma kullanır. Bu risk olayının risk düzeyi Orta. Bu risk olayına yanıt vermek için oturum açma riski koşulunu orta olarak ayarlarsınız. 

Bu bölümde, gerekli oturum açma riski koşullu erişim ilkesinin nasıl oluşturulacağı gösterilmektedir. İlkenizde şunları ayarlayın:

|Ayar |Değer|
|---     | --- |
| Kullanıcılar  | Alain Charon  |
| Koşullar | Oturum açma riski, orta ve üstü |
| Denetimler | Erişimi engelle |

![İlke oluştur](./media/quickstart-sign-in-risk-policy/201.png)

**Koşullu erişim ilkenizi yapılandırmak için:**

1. [Azure Portal](https://portal.azure.com) genel yönetici olarak oturum açın.
2. [Azure AD kimlik koruması sayfasına](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/Overview)gidin.
3. **Azure AD kimlik koruması** sayfasında, **Yapılandır** bölümünde, **oturum açma risk ilkesi**' ne tıklayın.
4. İlke sayfasında, **atamalar** bölümünde **Kullanıcılar**' a tıklayın.
5. **Kullanıcılar** sayfasında, **Kullanıcıları Seç**' e tıklayın.
6. **Kullanıcıları seçin** sayfasında, **Charon**' ı seçin ve ardından **Seç**' e tıklayın.
7. **Kullanıcılar** sayfasında **bitti**' ye tıklayın. 
8. İlke sayfasında, **atamalar** bölümünde **koşullar**' a tıklayın.
9. **Koşullar** sayfasında, **oturum açma riski**' na tıklayın.
10. **Oturum açma riski** sayfasında **Orta ve üst**' i seçin ve ardından **Seç**' e tıklayın. 
11. **Koşullar** sayfasında **bitti**' ye tıklayın.
12. İlke sayfasında, **denetimler** bölümünde, **erişim**' e tıklayın.
13. **Erişim** sayfasında **erişime izin ver**' e tıklayın, **çok faktörlü kimlik doğrulaması gerektir**' i seçin ve ardından **Seç**' e tıklayın.
14. İlke sayfasında **Kaydet**' e tıklayın.  

## <a name="test-your-conditional-access-policy"></a>Koşullu erişim ilkenizi test etme

İlkenizi test etmek için, Tor tarayıcısı kullanılırken [Azure Portal](https://portal.azure.com) bir **alan** olarak oturum açmayı deneyin. Oturum açma denemeniz, koşullu erişim ilkeniz tarafından engellenmelidir.

![Çok öğeli kimlik doğrulama](./media/quickstart-sign-in-risk-policy/203.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, Tor tarayıcısı olan test kullanıcısını silin ve oturum açma riskini koşullu erişim ilkesini devre dışı bırakın:

- Bir Azure AD kullanıcısının nasıl silineceğini bilmiyorsanız bkz. [Kullanıcı ekleme veya silme](../fundamentals/add-users-azure-active-directory.md#delete-a-user).
- Tor tarayıcısını kaldırma yönergeleri için bkz. [kaldırma](https://tb-manual.torproject.org/uninstalling/).
