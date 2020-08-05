---
title: Microsoft Identity platform SAML protokolünü nasıl kullanır?
description: Bu makalede, Azure Active Directory ' deki çoklu oturum açma ve çoklu oturum kapatma SAML profillerine genel bakış sunulmaktadır.
services: active-directory
author: kenwith
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 10/05/2018
ms.author: kenwith
ms.custom: aaddev
ms.reviewer: paulgarn
ms.openlocfilehash: 06f80f94be25e42c9e8f0270e6cb15aca086ae18
ms.sourcegitcommit: 1b2d1755b2bf85f97b27e8fbec2ffc2fcd345120
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87552807"
---
# <a name="how-microsoft-identity-platform-uses-the-saml-protocol"></a>Microsoft Identity platform SAML protokolünü nasıl kullanır?

Microsoft Identity platform, uygulamaların kullanıcılarına çoklu oturum açma deneyimi sağlamasına olanak tanımak için SAML 2,0 protokolünü kullanır. Azure AD 'nin [Çoklu oturum açma](single-sign-on-saml-protocol.md) ve [Çoklu oturum açma](single-sign-out-saml-protocol.md) SAML profilleri, SAML onayları, protokolleri ve bağlamaları kimlik sağlayıcısı hizmetinde nasıl kullanıldığını açıklar.

SAML protokolü, kimlik sağlayıcısını (Microsoft kimlik platformu) ve hizmet sağlayıcısını (uygulama) kendileri hakkındaki bilgileri değiş tokuş etmek için gereklidir.

Bir uygulama Azure AD 'ye kaydedildiğinde, uygulama geliştiricisi Federasyon ile ilgili bilgileri Azure AD 'ye kaydeder. Bu bilgiler, uygulamanın **yeniden YÖNLENDIRME URI** 'Sini ve **meta veri URI** 'sini içerir.

Microsoft Identity platform, imzalama anahtarını ve oturum kapatma URI 'sini almak için bulut hizmetinin **meta VERI URI** 'sini kullanır. Müşteri, uygulamayı **Azure AD > uygulama kaydında** açabilir ve sonra **Ayarlar-> Özellikler**bölümünde oturum kapatma URL 'sini güncelleştirebilir. Bu şekilde, Microsoft Identity platform yanıtı doğru URL 'ye gönderebilir. 

Azure Active Directory, kiracıya özgü ve ortak (kiracıdan bağımsız) çoklu oturum açma ve çoklu oturum açma uç noktalarını kullanıma sunar. Bu URL 'Ler adreslenebilir konumları temsil eder; bu, yalnızca tanımlayıcılardır; bu nedenle meta verileri okumak için uç noktaya gidebilirsiniz.

* Kiracıya özgü uç nokta konumunda bulunur `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml` . *\<TenantDomainName>* Yer tutucu, bir Azure AD kiracısının bir kayıtlı etki alanı adını veya Tenantıd 'sini temsil eder. Örneğin, contoso.com kiracının Federasyon meta verileri şu konumda:https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

* Kiracıdan bağımsız uç nokta konumunda bulunur `https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml` . Bu uç nokta adresinde, kiracı etki alanı adı veya KIMLIĞI yerine **yaygın** görüntülenir.

Azure AD 'nin yayımladığı Federasyon meta verileri belgeleri hakkında daha fazla bilgi için bkz. [Federasyon meta verileri](../azuread-dev/azure-ad-federation-metadata.md).
