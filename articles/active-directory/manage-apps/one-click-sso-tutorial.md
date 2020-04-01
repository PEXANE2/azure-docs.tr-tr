---
title: Azure Marketi uygulamanızın tek tıklamayla, tek oturum açma (SSO) yapılandırması | Microsoft Dokümanlar
description: Azure Marketi'nden uygulamanız için SSO'nun tek tıklamayla yapılandırması için adımlar.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: e0416991-4b5d-4b18-89bb-91b6070ed3ba
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/11/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: a83d27af4fd783b95c53ef3a9169cb72bfc29d34
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67872436"
---
# <a name="one-click-app-configuration-of-single-sign-on"></a>Tek oturum açmanın tek tıklamayla uygulama yapılandırması

 Bu eğitimde, Azure Marketi'nden SAML destekli Azure Active Directory (Azure AD) uygulamaları için tek tıklamalı, tek oturum açma (SSO) yapılandırmasını nasıl gerçekleştireceklerini öğreneceksiniz.

## <a name="introduction-to-one-click-sso"></a>Tek tıklamayla SSO'ya giriş

Tek tıklatma SSO özelliği, SAML protokolünü destekleyen Azure Marketi uygulamaları için tek oturum açma yapmak üzere tasarlanmıştır. Azure AD SSO yapılandırma sayfasında, bu seçenek uygulama tarafındaki Azure AD meta verilerini otomatik olarak yapılandırmanıza olanak tanır. Bu şekilde, sso'u en az manuel çabayla hızlı bir şekilde ayarlayabilirsiniz.

## <a name="advantages-of-one-click-sso"></a>Tek tıklamayla SSO'nun avantajları

- Uygulama tarafında manuel kurulum gerektiren Azure Marketi uygulamalarının hızlı SSO yapılandırması.
- Daha verimli ve doğru SSO yapılandırması.
- Kurulum için ortak iletişimi veya desteği gerekmez. Uygulama SAML yapılandırması için Kullanıcı Arabirimi sağlar.

## <a name="prerequisites"></a>Ön koşullar

- SSO ile yapılandırmak için uygulamanın etkin bir abonelik. Ayrıca yönetici kimlik bilgilerine de ihtiyacınız var.
- Microsoft'un **Uygulamalarım Güvenli Oturum Açma uzantısı** tarayıcıda yüklü. Daha fazla bilgi için [Access'e bakın ve Uygulamalarım portalındaki uygulamaları kullanın.](https://docs.microsoft.com/azure/active-directory/user-help/my-apps-portal-end-user-access)

## <a name="one-click-sso-configuration-steps"></a>Tek tıklamayla SSO yapılandırma adımları

1. Uygulamayı Azure Marketi'nden ekleyin.

2. **Tek oturum açma'yı**seçin.

3. **Tek oturum açmayı etkinleştir'i**seçin.

4. **Temel SAML Yapılandırması** bölümündezorunlu yapılandırma değerlerini doldurma.

    > [!NOTE]
    > Uygulama, yapılandırmanız gereken özel talepleri varsa, tek tıklamayla SSO gerçekleştirmeden önce bunları işleyin.

5. Azure Marketi uygulamanız için tek tıklamayla SSO özelliği varsa, aşağıdaki ekranı görürsünüz. Uzantıyı yükle seçeneğini seçerek **Uygulamalarım Güvenli Oturum Açma tarayıcı** **uzantısını**yüklemeniz gerekebilir.

   ![Uygulamalarımı Güvenli Oturum Açma tarayıcı uzantısını yükleme](./media/one-click-sso-tutorial/install-myappssecure-extension.png)

6. Uzantıyı tarayıcıya ekledikten **sonra, \<\>Kurulum Uygulama Adı'nı**seçin. Uygulama yöneticisi portalına yönlendirildikten sonra yönetici olarak oturum açın.

   ![Kurulum uygulama adı](./media/one-click-sso-tutorial/setup-sso.png)

7. Tarayıcı uzantısı uygulamada SSO'yu otomatik olarak yapılandırır. **Evet'i**seçerek onayla.

   ![Otomatik doldurulan verileri kaydetme](./media/one-click-sso-tutorial/save-autopopulate.png)

   > [!NOTE]
   > Uygulamanız için SSO yapılandırması, adımları gerçekleştirmek için istemleri izleyerek ek adımlar gerektiriyorsa.

8. Yapılandırma tamamlandıktan sonra değişiklikleri kaydetmek için **Tamam'ı** seçin.

   ![Otomatik doldurulan verileri kaydetme](./media/one-click-sso-tutorial/save-data.png)

9. SSO ayarlarının başarıyla yapılandırıldığına dair bir onay penceresi görüntülenir.

   ![SSO yapılandırılmış](./media/one-click-sso-tutorial/sso-configured.png)

10. Yapılandırma başarılı olduktan sonra, uygulamadan çıkış yapar ve Azure portalına döndürülür.

11. Tek oturum açma yı test etmek için **Test'i** seçebilirsiniz.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamalarının Azure Active Directory ile nasıl entegre edilebildiğini anlatan öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)
* [Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısı nedir?](https://docs.microsoft.com/azure/active-directory/user-help/my-apps-portal-end-user-access)
 
