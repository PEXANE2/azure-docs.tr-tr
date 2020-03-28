---
title: Öğretici - B2B için çok faktörlü kimlik doğrulama - Azure AD
description: Bu eğitimde, dış kullanıcılar ve iş ortağı kuruluşlarla işbirliği yapmak için Azure AD B2B'yi kullandığınızda çok faktörlü kimlik doğrulama (MFA) gereksinimini öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: tutorial
ms.date: 04/10/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: bddf1642b2013567fbc23278b3d8d32692601d55
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74420582"
---
# <a name="tutorial-enforce-multi-factor-authentication-for-b2b-guest-users"></a>Öğretici: B2B konuk kullanıcıları için çok faktörlü kimlik doğrulamasını zorlama

Şirket dışından B2B konuk kullanıcılarıyla işbirliği yaparken, uygulamalarınızı çok faktörlü kimlik doğrulaması (MFA) ilkeleriyle korumak iyi bir fikirdir. Daha sonra şirket dışından kullanıcıların kaynaklarınıza erişmek için bir kullanıcı adı ve paroladan fazlasına ihtiyacı olacaktır. Azure Etkin Dizin (Azure AD) 'de, bu hedefi erişim için MFA gerektiren koşullu erişim ilkesiyle gerçekleştirebilirsiniz. MFA ilkeleri, tıpkı kendi kuruluşunuzun üyeleri için etkinleştirildiği şekilde, kiracı, uygulama veya bireysel konuk kullanıcı düzeyinde uygulanabilir.

Örnek:

![Konuk kullanıcının bir şirketin uygulamalarında oturum açmalarını gösteren diyagram](media/tutorial-mfa/aad-b2b-mfa-example.png)

1.  A Şirketindeki bir yönetici veya çalışan, erişim için MFA gerekli kılınacak şekilde yapılandırılmış bir bulut veya şirket içi uygulamayı kullanması için bir konuk kullanıcıyı davet eder.
2.  Konuk kullanıcı, kendi iş, okul veya sosyal kimliği ile oturum açar. 
3.  Kullanıcıdan bir MFA testini tamamlaması istenir. 
4.  Kullanıcı, A Şirketi ile MFA’yı ayarlar ve kendi MFA seçeneğini belirler. Kullanıcının uygulamaya erişmesine izin verilir.

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> * MFA kurulumundan önce oturum açma deneyimini test edin.
> * Ortamınızdaki bir bulut uygulamasına erişmek için MFA gerektiren bir Koşullu Erişim ilkesi oluşturun. Bu öğreticide, işlemi göstermek için Microsoft Azure Management uygulamasını kullanacağız.
> * MFA oturum açma işleminin benzetimini yapmak için What If aracını kullanın.
> * Koşullu Erişim ilkenizi test edin.
> * Test kullanıcısını ve ilkeyi temizleyin.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide senaryoyu tamamlamak için şunlar gereklidir:

 - Koşullu Erişim ilkesi özelliklerini içeren **Azure AD Premium sürümüne erişim.** MFA'yı uygulamak için bir Azure AD Koşullu Erişim ilkesi oluşturmanız gerekir. Ortağın MFA özelliklerine sahip olup olmadığına bakılmaksızın, MFA ilkelerinin kuruluşunuzda her zaman uygulandığını unutmayın. Kuruluşunuz için MFA’yı ayarlarsanız, konuk kullanıcılarınız için yeterli Azure AD Premium lisanslarına sahip olduğunuzdan emin olmanız gerekir. 
 - Konuk kullanıcı olarak kiracı dizininize ekleyebileceğiniz ve oturum açmak için kullanabileceğiniz **geçerli bir harici e-posta hesabı**. Konuk hesabı nasıl oluşturulacağını bilmiyorsanız bkz. [Azure portalda B2B konuk kullanıcısı ekleme](add-users-administrator.md).

## <a name="create-a-test-guest-user-in-azure-ad"></a>Azure AD’de test konuk kullanıcısı oluşturma

1. [Azure portalda](https://portal.azure.com/) Azure AD yöneticisi olarak oturum açın.
2. Sol bölmede **Azure Active Directory**’yi seçin.
3.  **Yönet** bölümünde **Kullanıcılar**’ı seçin.
4.  **Yeni konuk kullanıcı**’yı seçin.

    ![Yeni konuk kullanıcı seçeneğinin seçilebileceği yeri gösteren ekran görüntüsü](media/tutorial-mfa/tutorial-mfa-user-3.png)

5.  **Kullanıcı adı** bölümüne, harici kullanıcının e-posta adresini girin. İsteğe bağlı olarak bir hoş geldiniz iletisi ekleyin. 

    ![Konuk davet iletisinin nereye girilirse girilen görüntüleri gösteren ekran görüntüsü](media/tutorial-mfa/tutorial-mfa-user-4.png)

6.  Konuk kullanıcıya otomatik olarak daveti göndermek için **Davet Et**’i seçin. **Kullanıcı başarıyla davet edildi** iletisi görüntülenir. 
7.  Daveti göndermenizin ardından kullanıcı hesabı otomatik olarak dizine konuk olarak eklenir.

## <a name="test-the-sign-in-experience-before-mfa-setup"></a>MFA kurulumundan önce oturum açma deneyimini test etme
1.  [Azure portalınızda](https://portal.azure.com/) oturum açmak için test kullanıcı adınızı ve parolanızı kullanın.
2.  Yalnızca oturum açma kimlik bilgilerinizi kullanarak Azure portala erişebileceğinizi unutmayın. Ek kimlik doğrulaması gerekmez.
3.  Oturumunuzu kapatın.

## <a name="create-a-conditional-access-policy-that-requires-mfa"></a>MFA gerektiren koşullu erişim ilkesi oluşturma
1.  [Azure portalınızda](https://portal.azure.com/) güvenlik yöneticisi veya Koşullu Erişim yöneticisi olarak oturum açın.
2.  Azure portalında **Azure Active Directory** seçeneğini belirleyin. 
3.  Azure **Etkin Dizin** sayfasında, **Güvenlik** bölümünde **Koşullu Erişim'i**seçin.
4.  **Koşullu Erişim** sayfasında, üstteki araç çubuğunda **Yeni ilke**’yi seçin.
5.  **Yeni** sayfasında **Ad** metin kutusuna **B2B portal erişimi için MFA’yı gerekli kıl** yazın.
6.  **Atamalar** bölümünde **Kullanıcılar ve gruplar**’ı seçin.
7.  **Kullanıcılar ve gruplar** sayfasında **Kullanıcı ve grupları seçin** ve sonra **Tüm konuk kullanıcılar (önizleme)** seçeneğini belirleyin.

    ![Tüm konuk kullanıcıların seçilmesini gösteren ekran görüntüsü](media/tutorial-mfa/tutorial-mfa-policy-6.png)
9.  **Done** (Bitti) öğesini seçin.
10. **Yeni** sayfasında **Atamalar** bölümünde **Bulut uygulamaları**’nı seçin.
11. **Bulut uygulamaları** sayfasında **Uygulama seç**’i ve sonra **Seç** öğesini seçin.

    ![Bulut uygulamaları sayfasını ve Seç seçeneğini gösteren ekran görüntüsü](media/tutorial-mfa/tutorial-mfa-policy-10.png)

12. **Seç** sayfasında **Microsoft Azure Management**’ı ve sonra **Seç** öğesini seçin.

    ![Microsoft Azure Yönetimi uygulamasını gösteren ekran görüntüsü](media/tutorial-mfa/tutorial-mfa-policy-11.png)

13. **Bulut uygulamaları** sayfasında **Bitti**’yi seçin.
14. **Yeni** sayfasında **Erişim denetimleri** bölümünde **Erişim İzni Verme**’yi seçin.
15. **Erişim İzni Verme** sayfasında **Erişim izni ver**’i seçin, **Çok faktörlü kimlik doğrulamasını gerekli kıl** onay kutusunu ve sonra **Seç** öğesini seçin.

    ![Çok faktörlü kimlik doğrulama yı gerektirme seçeneğini gösteren ekran görüntüsü](media/tutorial-mfa/tutorial-mfa-policy-13.png)

16. **İlkeyi etkinleştir** bölümünde **Açık** seçeneğini belirleyin.

    ![A'ya ayarlanmış etkinleştir meyat seçeneğini gösteren ekran görüntüsü](media/tutorial-mfa/tutorial-mfa-policy-14.png)

17. **Oluştur'u**seçin.

## <a name="use-the-what-if-option-to-simulate-sign-in"></a>Oturum açmanın benzetimini yapmak için What If seçeneğini kullanma

1.  **Koşullu Erişim - İlkeler** **sayfasında, Ne Varsa'yı**seçin. 

    !["Eğer" seçeneğini nerede seçebileceğinizi gösteren ekran görüntüsü](media/tutorial-mfa/tutorial-mfa-whatif-1.png)

2.  **Kullanıcı**’yı seçin, test konuk kullanıcınızı ve sonra **Seç** öğesini seçin.

    ![Seçilen konuk kullanıcıyı gösteren ekran görüntüsü](media/tutorial-mfa/tutorial-mfa-whatif-2.png)

3.  **Bulut uygulamaları**’nı seçin.
4.  **Bulut uygulamaları** sayfasında **Uygulama seç**’i seçin ve sonra **Seç**’e tıklayın. Uygulamalar listesinde **Microsoft Azure Management**’ı seçin ve sonra **Seç**’e tıklayın. 

    ![Microsoft Azure Yönetimi uygulamasını gösteren ekran görüntüsü](media/tutorial-mfa/tutorial-mfa-whatif-3.png)

5.  **Bulut uygulamaları** sayfasında **Bitti**’yi seçin.
6.  **What If** seçeneğini belirleyin ve yeni ilkenizin **Uygulanacak ilkeler** sekmesinde **Değerlendirme sonuçları** bölümünde görüntülendiğini doğrulayın.

    !["Eğer" seçeneğini nerede seçebileceğinizi gösteren ekran görüntüsü](media/tutorial-mfa/tutorial-mfa-whatif-4.png)

## <a name="test-your-conditional-access-policy"></a>Koşullu Erişim ilkenizi test edin
1.  [Azure portalınızda](https://portal.azure.com/) oturum açmak için test kullanıcı adınızı ve parolanızı kullanın.
2.  Ek kimlik doğrulama yöntemlerine yönelik bir istek görmeniz gerekir. İlkenin geçerli olmasının biraz zaman alabileceğini unutmayın.

    ![Daha fazla bilgi gerekli iletigösteren ekran görüntüsü](media/tutorial-mfa/mfa-required.png)
 
3.  Oturumunuzu kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekmediğinde, test kullanıcısını ve koşullu erişim ilkesini kaldırın.
1.  [Azure portalda](https://portal.azure.com/) Azure AD yöneticisi olarak oturum açın.
2.  Sol bölmede **Azure Active Directory**’yi seçin.
3.  **Yönet** bölümünde **Kullanıcılar**’ı seçin.
4.  Test kullanıcısını seçin ve **Kullanıcıyı sil** seçeneğini belirleyin.
5.  Sol bölmede **Azure Active Directory**’yi seçin.
6.  **Güvenlik** bölümünde **Koşullu Erişim**’i seçin.
7.  **İlke Adı** listesinde, test ilkeniz için bağlam menüsünü (…) seçin ve **Sil** seçeneğini belirleyin. Onaylamak için **Evet**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar
Bu eğitimde, konuk kullanıcıların bulut uygulamalarınızdan birinde oturum açtıklarında MFA'yı kullanmalarını gerektiren bir Koşullu Erişim ilkesi oluşturdunuz. İşbirliği yapmak üzere konuk kullanıcılar ekleme hakkında daha fazla bilgi için bkz. [Azure portalda Azure Active Directory B2B işbirliği kullanıcıları ekleme](add-users-administrator.md).
