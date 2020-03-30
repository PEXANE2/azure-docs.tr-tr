---
title: Azure AD Galerisi uygulamaları için federe SSO yapılandırma sorunları
description: Azure AD Uygulama Galerisi'nde listelenen uygulamalar için SAML kullanarak federal tek oturum açma yapılandırırken karşılaşabileceğiniz sık karşılaşılan sorunlardan bazılarını giderin
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 87c2497a781b0d46b3b2f1e281a3d7b327b60952
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74274650"
---
# <a name="problem-configuring-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Azure AD Galerisi uygulaması için federe tek oturum açma sorunu

Bir uygulamayı yapılandırırken bir sorunla karşılaşırsanız. Uygulama için öğreticideki tüm adımları izlediğinizi doğrulayın. Uygulamanın yapılandırmasında, uygulamayı nasıl yapılandırabileceğinize ilişkin satır lı belgeleriniz vardır. Ayrıca, adım adım ayrıntılı bir kılavuz için [SaaS uygulamalarını Azure Active Directory ile nasıl tümleştirebileceğinize ilişkin öğreticiler listesine](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) de erişebilirsiniz.

## <a name="cant-add-another-instance-of-the-application"></a>Uygulamanın başka bir örneğini ekleyebilir

Bir uygulamanın ikinci bir örneğini eklemek için şunları yapabilmeniz gerekir:

-   İkinci örnek için benzersiz bir tanımlayıcıyı yapılandırın. İlk örnek için kullanılan aynı tanımlayıcıyı yapılandıramazsınız.

-   İlk örnek için kullanılandan farklı bir sertifika yapılandırın.

Uygulama yukarıdakilerden herhangi birini desteklemiyorsa. Ardından, ikinci bir örneği yapılandıramazsınız.

## <a name="cant-add-the-identifier-or-the-reply-url"></a>Tanımlayıcıveya Yanıt URL'si eklenemez

Tanımlayıcıyı veya YanıtURL'sini yapılandıramıyorsanız, Tanımlayıcı ve YanıtLA URL değerlerinin uygulama için önceden yapılandırılmış desenlerle eşleşip eşleşmediğini onaylayın.

Uygulama için önceden yapılandırılmış desenleri bilmek için:

1. Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator** veya **Co-admin** olarak oturum açın. Adım 7'ye git. Azure AD'deki uygulama yapılandırma bıçaklarında ysanız.

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5. **tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı tıklatın.

   * Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar Listesi'nin** en üstündeki **Filtre** denetimini kullanın ve Tüm Uygulamalar için **Göster** seçeneğini **ayarlayın.**

6. Tek oturum açma yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol navigasyon menüsünden **Tek oturum** açma'yı tıklatın.

8. **Mod** açılır düşüşünden **SAML tabanlı Oturum** Açma'yı seçin.

9. Etki Alanı ve **URL'ler bölümünün** altındaki **Tanımlayıcı** veya **Url'yi Yanıtla** metin kutusuna gidin.

10. Uygulama için desteklenen desenleri bilmenin üç yolu vardır:

    * Metin kutusunda, desteklenen desen(ler)'i yer tutucu olarak görürsünüz *Örnek:* <https://contoso.com>.

    * desen desteklenmiyorsa, metin kutusuna değer girmeye çalıştığınızda kırmızı bir ünlem işareti görürsünüz. Farenizi kırmızı ünlem işaretinin üzerinde gezdiriyseniz, desteklenen desenleri görürsünüz.

    * Uygulama için öğreticide, desteklenen desenler hakkında da bilgi alabilirsiniz. Azure **AD'yi Yapılandır** tek oturum açma bölümü altında. **Etki Alanı ve URL'ler** bölümü altındaki değerleri yapılandırmak için adıma gidin.

Değerler Azure AD'de önceden yapılandırılmış desenlerle eşleşmiyorsa. Şunları yapabilirsiniz:

-   Azure AD'de önceden yapılandırılmış desenle eşleşen değerleri almak için uygulama satıcısıyla birlikte çalışın

-   Veya, uygulama için desteklenen <aadapprequest@microsoft.com> desenlerin güncelleştirinibilmek için Azure AD ekibine başvurabilir veya eğitimde yorum bırakabilirsiniz

## <a name="where-do-i-set-the-entityid-user-identifier-format"></a>EntityID (Kullanıcı Tanımlayıcısı) biçimini nerede ayarlıyorum

Azure AD'nin kullanıcı kimlik doğrulaması sonrasında yanıt olarak uygulamaya gönderdiği EntityID (Kullanıcı Tanımlayıcısı) biçimini seçemezsiniz.

Azure AD, SAML AuthRequest'te seçilen değere veya uygulama tarafından istenen biçime göre NameID özniteliğinin (Kullanıcı Tanımlayıcısı) biçimini seçin. Daha fazla bilgi için NameIDPolicy bölümü altındaki [Tek Oturum-On SAML protokolünü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) ziyaret edin,

## <a name="cant-find-the-azure-ad-metadata-to-complete-the-configuration-with-the-application"></a>Uygulamayla yapılandırmayı tamamlamak için Azure AD meta verilerini bulamıyorum

Uygulama meta verilerini veya sertifikasını Azure AD'den indirmek için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator** veya **Co-admin** olarak oturum açın.

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5. **tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı tıklatın.

   * Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar Listesi'nin** en üstündeki **Filtre** denetimini kullanın ve Tüm Uygulamalar için **Göster** seçeneğini **ayarlayın.**

6. Tek oturum açma yı yapılandırdığınız uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol navigasyon menüsünden **Tek oturum** açma'yı tıklatın.

8. **SAML İmza Sertifikası** bölümüne gidin ve ardından sütun değerini **karşıdan yükle'yi** tıklatın. Uygulamanın tek oturum açma yı yapılandırmayı gerektirdiğine bağlı olarak, Metadata XML'i veya Sertifikayı indirme seçeneğini görürsünüz.

Azure AD, meta verileri almak için bir URL sağlamaz. Meta veriler yalnızca XML dosyası olarak alınabilir.

## <a name="dont-know-how-to-customize-saml-claims-sent-to-an-application"></a>Bir uygulamaya gönderilen SAML taleplerinin nasıl özelleştirilebildiğini bilmiyorum

Uygulamanıza gönderilen SAML öznitelik taleplerini nasıl özelleştirebilirsiniz öğrenmek için daha fazla bilgi için [Azure Etkin Dizini'nde Talepler eşleme](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) sine bakın.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamaları Azure Active Directory ile yönetme](what-is-application-management.md)
