---
title: Galeri dışı bir uygulama için federe tek oturum açma sorunu | Microsoft Dokümanlar
description: Azure AD Uygulama Galerisi'nde listelenmemiş özel SAML uygulamanızda federalerated tek oturum açma yaparken karşılaşabileceğiniz sık karşılaşılan sorunlardan bazılarını giderin
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76711882"
---
# <a name="problem-configuring-federated-single-sign-on-for-a-non-gallery-application"></a>Galeri dışı bir uygulama için federe tek oturum açma sorunu

Bir uygulamayı yapılandırırken bir sorunla karşılaşırsanız. [Azure Etkin Dizin uygulama galerisinde olmayan uygulamalarda tek oturum açma](configure-federated-single-sign-on-non-gallery-applications.md) yapılandırma makalesindeki tüm adımları izlediğinizi doğrulayın.

## <a name="cant-add-another-instance-of-the-application"></a>Uygulamanın başka bir örneğini ekleyebilir

Bir uygulamanın ikinci bir örneğini eklemek için şunları yapabilmeniz gerekir:

-   İkinci örnek için benzersiz bir tanımlayıcıyı yapılandırın. İlk örnek için kullanılan aynı tanımlayıcıyı yapılandıramazsınız.

-   İlk örnek için kullanılandan farklı bir sertifika yapılandırın.

Uygulama öncekilerden herhangi birini desteklemiyorsa, ikinci bir örneği yapılandıramazsınız.

## <a name="where-do-i-set-the-entityid-user-identifier-format"></a>EntityID (Kullanıcı Tanımlayıcısı) biçimini nerede ayarlıyorum

Azure AD'nin kullanıcı kimlik doğrulaması sonrasında yanıt ta uygulamaya gönderdiği EntityID (Kullanıcı Tanımlayıcısı) biçimini seçemezsiniz.

Azure AD, SAML AuthRequest'te seçilen değere veya uygulama tarafından istenen biçime göre NameID özniteliğinin (Kullanıcı Tanımlayıcısı) biçimini seçer. Daha fazla bilgi için NameIDPolicy bölümü altındaki [Tek Oturum-On SAML protokolünü](../develop/single-sign-on-saml-protocol.md#authnrequest) ziyaret edin,

## <a name="where-do-i-get-the-application-metadata-or-certificate-from-azure-ad"></a>Azure AD'den uygulama meta verilerini veya sertifikasını nereden alabilirim?

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

Uygulamanıza gönderilen SAML öznitelik taleplerini nasıl özelleştirebilirsiniz öğrenmek için daha fazla bilgi için [Azure Etkin Dizini'nde Talepler eşleme](../develop/active-directory-claims-mapping.md) sine bakın.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamaları Azure Active Directory ile yönetme](what-is-application-management.md)
