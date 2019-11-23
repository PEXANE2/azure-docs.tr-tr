---
title: Tutorial - Multi-factor authentication for B2B - Azure AD
description: In this tutorial, learn how to require multi-factor authentication (MFA) when you use Azure AD B2B to collaborate with external users and partner organizations.
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
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74420582"
---
# <a name="tutorial-enforce-multi-factor-authentication-for-b2b-guest-users"></a>Öğretici: B2B konuk kullanıcıları için çok faktörlü kimlik doğrulamasını zorlama

Şirket dışından B2B konuk kullanıcılarıyla işbirliği yaparken, uygulamalarınızı çok faktörlü kimlik doğrulaması (MFA) ilkeleriyle korumak iyi bir fikirdir. Daha sonra şirket dışından kullanıcıların kaynaklarınıza erişmek için bir kullanıcı adı ve paroladan fazlasına ihtiyacı olacaktır. In Azure Active Directory (Azure AD), you can accomplish this goal with a Conditional Access policy that requires MFA for access. MFA ilkeleri, tıpkı kendi kuruluşunuzun üyeleri için etkinleştirildiği şekilde, kiracı, uygulama veya bireysel konuk kullanıcı düzeyinde uygulanabilir.

Örnek:

![Diagram showing a guest user signing into a company's apps](media/tutorial-mfa/aad-b2b-mfa-example.png)

1.  A Şirketindeki bir yönetici veya çalışan, erişim için MFA gerekli kılınacak şekilde yapılandırılmış bir bulut veya şirket içi uygulamayı kullanması için bir konuk kullanıcıyı davet eder.
2.  Konuk kullanıcı, kendi iş, okul veya sosyal kimliği ile oturum açar. 
3.  Kullanıcıdan bir MFA testini tamamlaması istenir. 
4.  Kullanıcı, A Şirketi ile MFA’yı ayarlar ve kendi MFA seçeneğini belirler. Kullanıcının uygulamaya erişmesine izin verilir.

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> * MFA kurulumundan önce oturum açma deneyimini test edin.
> * Create a Conditional Access policy that requires MFA for access to a cloud app in your environment. Bu öğreticide, işlemi göstermek için Microsoft Azure Management uygulamasını kullanacağız.
> * MFA oturum açma işleminin benzetimini yapmak için What If aracını kullanın.
> * Test your Conditional Access policy.
> * Test kullanıcısını ve ilkeyi temizleyin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide senaryoyu tamamlamak için şunlar gereklidir:

 - **Access to Azure AD Premium edition**, which includes Conditional Access policy capabilities. To enforce MFA, you need to create an Azure AD Conditional Access policy. Ortağın MFA özelliklerine sahip olup olmadığına bakılmaksızın, MFA ilkelerinin kuruluşunuzda her zaman uygulandığını unutmayın. Kuruluşunuz için MFA’yı ayarlarsanız, konuk kullanıcılarınız için yeterli Azure AD Premium lisanslarına sahip olduğunuzdan emin olmanız gerekir. 
 - Konuk kullanıcı olarak kiracı dizininize ekleyebileceğiniz ve oturum açmak için kullanabileceğiniz **geçerli bir harici e-posta hesabı**. Konuk hesabı nasıl oluşturulacağını bilmiyorsanız bkz. [Azure portalda B2B konuk kullanıcısı ekleme](add-users-administrator.md).

## <a name="create-a-test-guest-user-in-azure-ad"></a>Azure AD’de test konuk kullanıcısı oluşturma

1. [Azure portalda](https://portal.azure.com/) Azure AD yöneticisi olarak oturum açın.
2. Sol bölmede **Azure Active Directory**’yi seçin.
3.  **Yönet** bölümünde **Kullanıcılar**’ı seçin.
4.  **Yeni konuk kullanıcı**’yı seçin.

    ![Screenshot showing where to select the New guest user option](media/tutorial-mfa/tutorial-mfa-user-3.png)

5.  **Kullanıcı adı** bölümüne, harici kullanıcının e-posta adresini girin. İsteğe bağlı olarak bir hoş geldiniz iletisi ekleyin. 

    ![Screenshot showing where to enter the guest invitation message](media/tutorial-mfa/tutorial-mfa-user-4.png)

6.  Konuk kullanıcıya otomatik olarak daveti göndermek için **Davet Et**’i seçin. **Kullanıcı başarıyla davet edildi** iletisi görüntülenir. 
7.  Daveti göndermenizin ardından kullanıcı hesabı otomatik olarak dizine konuk olarak eklenir.

## <a name="test-the-sign-in-experience-before-mfa-setup"></a>MFA kurulumundan önce oturum açma deneyimini test etme
1.  [Azure portalınızda](https://portal.azure.com/) oturum açmak için test kullanıcı adınızı ve parolanızı kullanın.
2.  Yalnızca oturum açma kimlik bilgilerinizi kullanarak Azure portala erişebileceğinizi unutmayın. Ek kimlik doğrulaması gerekmez.
3.  Oturumunuzu kapatın.

## <a name="create-a-conditional-access-policy-that-requires-mfa"></a>Create a Conditional Access policy that requires MFA
1.  Sign in to your [Azure portal](https://portal.azure.com/) as a security administrator or a Conditional Access administrator.
2.  Azure portalında **Azure Active Directory** seçeneğini belirleyin. 
3.  On the **Azure Active Directory** page, in the **Security** section, select **Conditional Access**.
4.  **Koşullu Erişim** sayfasında, üstteki araç çubuğunda **Yeni ilke**’yi seçin.
5.  **Yeni** sayfasında **Ad** metin kutusuna **B2B portal erişimi için MFA’yı gerekli kıl** yazın.
6.  **Atamalar** bölümünde **Kullanıcılar ve gruplar**’ı seçin.
7.  **Kullanıcılar ve gruplar** sayfasında **Kullanıcı ve grupları seçin** ve sonra **Tüm konuk kullanıcılar (önizleme)** seçeneğini belirleyin.

    ![Screenshot showing selecting all guest users](media/tutorial-mfa/tutorial-mfa-policy-6.png)
9.  **Done** (Bitti) öğesini seçin.
10. **Yeni** sayfasında **Atamalar** bölümünde **Bulut uygulamaları**’nı seçin.
11. **Bulut uygulamaları** sayfasında **Uygulama seç**’i ve sonra **Seç** öğesini seçin.

    ![Screenshot showing the Cloud apps page and the Select option](media/tutorial-mfa/tutorial-mfa-policy-10.png)

12. **Seç** sayfasında **Microsoft Azure Management**’ı ve sonra **Seç** öğesini seçin.

    ![Screenshot showing the Microsoft Azure Management app selected](media/tutorial-mfa/tutorial-mfa-policy-11.png)

13. **Bulut uygulamaları** sayfasında **Bitti**’yi seçin.
14. **Yeni** sayfasında **Erişim denetimleri** bölümünde **Erişim İzni Verme**’yi seçin.
15. **Erişim İzni Verme** sayfasında **Erişim izni ver**’i seçin, **Çok faktörlü kimlik doğrulamasını gerekli kıl** onay kutusunu ve sonra **Seç** öğesini seçin.

    ![Screenshot showing the Require multi-factor authentication option](media/tutorial-mfa/tutorial-mfa-policy-13.png)

16. **İlkeyi etkinleştir** bölümünde **Açık** seçeneğini belirleyin.

    ![Screenshot showing the Enable policy option set to On](media/tutorial-mfa/tutorial-mfa-policy-14.png)

17. **Oluştur**'u seçin.

## <a name="use-the-what-if-option-to-simulate-sign-in"></a>Oturum açmanın benzetimini yapmak için What If seçeneğini kullanma

1.  On the **Conditional Access - Policies** page, select **What If**. 

    ![Screenshot showing where to select the What if option](media/tutorial-mfa/tutorial-mfa-whatif-1.png)

2.  **Kullanıcı**’yı seçin, test konuk kullanıcınızı ve sonra **Seç** öğesini seçin.

    ![Screenshot showing a guest user selected](media/tutorial-mfa/tutorial-mfa-whatif-2.png)

3.  **Bulut uygulamaları**’nı seçin.
4.  **Bulut uygulamaları** sayfasında **Uygulama seç**’i seçin ve sonra **Seç**’e tıklayın. Uygulamalar listesinde **Microsoft Azure Management**’ı seçin ve sonra **Seç**’e tıklayın. 

    ![Screenshot showing the Microsoft Azure Management app selected](media/tutorial-mfa/tutorial-mfa-whatif-3.png)

5.  **Bulut uygulamaları** sayfasında **Bitti**’yi seçin.
6.  **What If** seçeneğini belirleyin ve yeni ilkenizin **Uygulanacak ilkeler** sekmesinde **Değerlendirme sonuçları** bölümünde görüntülendiğini doğrulayın.

    ![Screenshot showing where to select the What if option](media/tutorial-mfa/tutorial-mfa-whatif-4.png)

## <a name="test-your-conditional-access-policy"></a>Test your Conditional Access policy
1.  [Azure portalınızda](https://portal.azure.com/) oturum açmak için test kullanıcı adınızı ve parolanızı kullanın.
2.  Ek kimlik doğrulama yöntemlerine yönelik bir istek görmeniz gerekir. İlkenin geçerli olmasının biraz zaman alabileceğini unutmayın.

    ![Screenshot showing the More information required message](media/tutorial-mfa/mfa-required.png)
 
3.  Oturumunuzu kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme
When no longer needed, remove the test user and the test Conditional Access policy.
1.  [Azure portalda](https://portal.azure.com/) Azure AD yöneticisi olarak oturum açın.
2.  Sol bölmede **Azure Active Directory**’yi seçin.
3.  **Yönet** bölümünde **Kullanıcılar**’ı seçin.
4.  Test kullanıcısını seçin ve **Kullanıcıyı sil** seçeneğini belirleyin.
5.  Sol bölmede **Azure Active Directory**’yi seçin.
6.  **Güvenlik** bölümünde **Koşullu Erişim**’i seçin.
7.  **İlke Adı** listesinde, test ilkeniz için bağlam menüsünü (…) seçin ve **Sil** seçeneğini belirleyin. Onaylamak için **Evet**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar
In this tutorial, you’ve created a Conditional Access policy that requires guest users to use MFA when signing in to one of your cloud apps. İşbirliği yapmak üzere konuk kullanıcılar ekleme hakkında daha fazla bilgi için bkz. [Azure portalda Azure Active Directory B2B işbirliği kullanıcıları ekleme](add-users-administrator.md).
