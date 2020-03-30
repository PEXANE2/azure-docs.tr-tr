---
title: B2B işbirliği için SaaS uygulamalarını yapılandırın - Azure AD
description: Azure Active Directory B2B işbirliği için kod ve PowerShell örnekleri
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7c73a14c2a8cd063672bd0998368ca660f52cd5a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74272945"
---
# <a name="configure-saas-apps-for-b2b-collaboration"></a>B2B işbirliği için SaaS uygulamalarını yapılandırma

Azure Etkin Dizin (Azure AD) B2B işbirliği, Azure AD ile tümleşen uygulamaların çoğunda çalışır. Bu bölümde, Azure AD B2B ile kullanılmak üzere bazı popüler SaaS uygulamalarını yapılandırma yönergelerini gözden geçiriyoruz.

Uygulamaya özel talimatlara bakmadan önce, başparmak bazı kurallar şunlardır:

* Uygulamaların çoğunda, kullanıcı kurulumunun el ile gerçekleşmesi gerekir. Diğer bir deyişle, kullanıcıların uygulamada da el ile oluşturulması gerekir.

* Dropbox gibi otomatik kurulumu destekleyen uygulamalar için uygulamalardan ayrı davetiyeler oluşturulur. Kullanıcılar her daveti kabul etmek gerekir.

* Kullanıcı özniteliklerinde, konuk kullanıcılarda ezilmiş kullanıcı profil diski (UPD) ile ilgili sorunları azaltmak için, **kullanıcı tanımlayıcısını** her zaman **user.mail**olarak ayarlayın.


## <a name="dropbox-business"></a>Dropbox İş

Kullanıcıların kuruluş hesaplarını kullanarak oturum açmalarını sağlamak için, Azure AD'yi Güvenlik İddiası Biçimlendirme Dili (SAML) kimlik sağlayıcısı olarak kullanacak şekilde Dropbox Business'ı el ile yapılandırmanız gerekir. Dropbox Business bunu yapacak şekilde yapılandırılmamışsa, kullanıcıların Azure AD kullanarak oturum açmasına izin veremez veya başka bir şekilde oturum açamaz.

1. Dropbox Business uygulamasını Azure AD'ye eklemek için sol bölmedeki **Kurumsal uygulamaları** seçin ve sonra **Ekle'yi**tıklatın.

   ![Kurumsal uygulamalar sayfasındaki "Ekle" düğmesi](media/configure-saas-apps/add-dropbox.png)

2. Uygulama **ekle** penceresinde, arama kutusuna **dropbox** girin ve ardından sonuç listesinde **İşletmeler için Dropbox'ı** seçin.

   ![Uygulama ekle sayfasında "dropbox" araması](media/configure-saas-apps/add-app-dialog.png)

3. Tek **oturum açma** sayfasında, sol bölmede **Tek oturum** açma'yı seçin ve ardından User **Identifier** kutusuna **user.mail** girin. (Varsayılan olarak UPN olarak ayarlanır.)

   ![Uygulama için tek oturum açma yapılandırma](media/configure-saas-apps/configure-app-sso.png)

4. Dropbox yapılandırması için kullanılacak sertifikayı indirmek için **DropBox'ı Yapılandır'ı**ve ardından listede **SAML Tek Oturum Açma Hizmet URL'sini** seçin.

   ![Dropbox yapılandırması için sertifikayı indirme](media/configure-saas-apps/download-certificate.png)

5. Tek oturum açma sayfasındaki oturum açma URL'si ile Dropbox'ta oturum **açın.**

   ![Dropbox oturum açma sayfasını gösteren ekran görüntüsü](media/configure-saas-apps/sign-in-to-dropbox.png)

6. Menüde Yönetici **Konsolu'nu**seçin.

   ![Dropbox menüsündeki "Yönetici Konsolu" bağlantısı](media/configure-saas-apps/dropbox-menu.png)

7. Kimlik **Doğrulama** iletişim **kutusunda, Daha Fazla,** sertifikayı yükleyin ve ardından **URL'de Oturum Aç** kutusuna SAML tek oturum açma URL'sini girin.

   ![Daraltılmış Kimlik Doğrulama iletişim kutusundaki "Daha fazla" bağlantı](media/configure-saas-apps/dropbox-auth-01.png)

   ![Genişletilmiş Kimlik Doğrulama iletişim kutusundaki "GIRIŞ URL'sini oturum aç"](media/configure-saas-apps/paste-single-sign-on-URL.png)

8. Azure portalında otomatik kullanıcı kurulumunu yapılandırmak için sol bölmede **Sağlama'yı** seçin, **Sağlama Modu** kutusunda **Otomatik'i** seçin ve ardından **Yetkilendirme'yi**seçin.

   ![Azure portalında otomatik kullanıcı sağlama yapılandırma](media/configure-saas-apps/set-up-automatic-provisioning.png)

Dropbox uygulamasında konuk veya üye kullanıcılar kurulduktan sonra Dropbox'tan ayrı bir davet alırlar. Dropbox'ta tek oturum açma seçeneğini kullanmak için davetlilerin daveti bir bağlantıyı tıklatarak kabul etmesi gerekir.

## <a name="box"></a>Box
SAML protokolünü temel alan federasyonu kullanarak kullanıcıların Azure AD hesaplarıyla Kutu konuk kullanıcıların kimliğini doğrulamalarını sağlayabilirsiniz. Bu yordamda, meta verileri Box.com yüklersiniz.

1. Kurumsal uygulamalardan Kutu uygulamasını ekleyin.

2. Aşağıdaki sırada tek oturum açma yapılandırma:

   ![Tek oturum açma yapılandırma ayarlarını gösteren ekran görüntüsü](media/configure-saas-apps/configure-box-sso.png)

   a. **URL'de Oturum Aç** kutusunda, oturum açma URL'sinin Azure portalındaki Kutu için uygun şekilde ayarlandığından emin olun. Bu URL, Box.com kiracınızın URL'sidir. Adlandırma kuralını *https://.box.com*takip etmelidir.  
   **Tanımlayıcı** bu uygulama için geçerli değildir, ancak yine de zorunlu bir alan olarak görünür.

   b. Kullanıcı **tanımlayıcı** kutusuna **user.mail** girin (konuk hesapları için SSO için).

   c. **SAML İmzaSertifikası**altında, **yeni sertifika oluştur'u**tıklatın.

   d. Box.com kiracınızı Azure AD'yi kimlik sağlayıcısı olarak kullanacak şekilde yapılandırmaya başlamak için meta veri dosyasını indirin ve ardından yerel sürücünüze kaydedin.

   e. Meta veri dosyasını, sizin için tek oturum açma yı yapılandıran Kutu destek ekibine iletin.

3. Azure AD otomatik kullanıcı kurulumu için, sol **bölmede, Sağlama'yı**seçin ve ardından **Yetkilendirme'yi**seçin.

   ![Azure AD'yi Kutuya bağlanmak için yetkilendirme](media/configure-saas-apps/auth-azure-ad-to-connect-to-box.png)

Dropbox davetlileri gibi Box davetlilerinin de davetlerini Kutu uygulamasından kullanmaları gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği ile ilgili aşağıdaki makalelere bakın:

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [Dinamik gruplar ve B2B işbirliği](use-dynamic-groups.md)
- [B2B işbirliği kullanıcı iddiaları eşleme](claims-mapping.md)
- [Office 365 dış paylaşım](o365-external-user.md)

