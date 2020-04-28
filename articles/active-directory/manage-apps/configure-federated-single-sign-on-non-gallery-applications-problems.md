---
title: Galeri dışı bir uygulama için Federasyon çoklu oturum açmayı yapılandırma sorunu | Microsoft Docs
description: Azure AD uygulama galerisinde listelenmeyen özel SAML uygulamanızda federe çoklu oturum açmayı yapılandırırken karşılaşabileceğiniz yaygın sorunlardan bazılarını çözün
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
ms.openlocfilehash: e7894bfada4d363e89f526280e2925b4f4c6180a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76711882"
---
# <a name="problem-configuring-federated-single-sign-on-for-a-non-gallery-application"></a>Galeri dışı bir uygulama için Federasyon çoklu oturum açmayı yapılandırma sorunu

Bir uygulamayı yapılandırırken sorunla karşılaşırsanız. [Azure Active Directory Uygulama galerisinde olmayan uygulamalarda çoklu oturum açmayı yapılandırma](configure-federated-single-sign-on-non-gallery-applications.md) makalesindeki tüm adımları izlemediğinizi doğrulayın.

## <a name="cant-add-another-instance-of-the-application"></a>Uygulamanın başka bir örneği eklenemiyor

Uygulamanın ikinci bir örneğini eklemek için şunları yapmanız gerekir:

-   İkinci örnek için benzersiz bir tanımlayıcı yapılandırın. İlk örnek için kullanılan tanımlayıcıyı yapılandıramazsınız.

-   İlk örnek için kullanılandan farklı bir sertifika yapılandırın.

Uygulama, önceki bir sürümü desteklemiyorsa, ikinci bir örnek yapılandıramazsınız.

## <a name="where-do-i-set-the-entityid-user-identifier-format"></a>EntityId (Kullanıcı tanımlayıcısı) biçimini nereden ayarlayabilirim?

Azure AD 'nin Kullanıcı kimlik doğrulamasından sonra yanıtta uygulamaya gönderdiği EntityId (Kullanıcı tanımlayıcısı) biçimini seçemezsiniz.

Azure AD, NameID özniteliği (Kullanıcı tanımlayıcısı) için seçilen değere veya SAML AuthRequest içinde uygulama tarafından istenen biçime göre biçim seçer. Daha fazla bilgi için, Nameıdpolicy bölümünde [Çoklu oturum açma SAML Protokolü](../develop/single-sign-on-saml-protocol.md#authnrequest) makalesini ziyaret edin.

## <a name="where-do-i-get-the-application-metadata-or-certificate-from-azure-ad"></a>Azure AD 'den uygulama meta verilerini veya sertifikayı nereden alabilirim?

Azure AD 'den uygulama meta verilerini veya sertifikasını indirmek için şu adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** veya **ortak yönetici** olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' a tıklayın.

   * Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar** olarak ayarlayın.

6. Çoklu oturum açma yapılandırdığınız uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Çoklu oturum açma** seçeneğine tıklayın.

8. **SAML Imzalama sertifikası** bölümüne gidin ve ardından sütun değerini **İndir** ' e tıklayın. Uygulamanın çoklu oturum açmayı yapılandırma ihtiyacı olduğuna bağlı olarak, meta veri XML 'sini veya sertifikayı indirme seçeneğini görürsünüz.

Azure AD meta verileri almak için bir URL sağlamıyor. Meta veriler yalnızca bir XML dosyası olarak alınabilir.

## <a name="dont-know-how-to-customize-saml-claims-sent-to-an-application"></a>Bir uygulamaya gönderilen SAML taleplerini özelleştirmeyi bilmiyorum

Uygulamanıza gönderilen SAML öznitelik taleplerini özelleştirmeyi öğrenmek için, daha fazla bilgi için [Azure Active Directory 'Da talep eşleme](../develop/active-directory-claims-mapping.md) konusuna bakın.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamaları Azure Active Directory ile yönetme](what-is-application-management.md)
