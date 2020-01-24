---
title: Azure AD 'nin SAML protokolünü nasıl kullandığı. | Microsoft Docs
description: Bu makalede, Azure Active Directory ' deki çoklu oturum açma ve çoklu oturum kapatma SAML profillerine genel bakış sunulmaktadır.
services: active-directory
author: rwike77
manager: CelesteDG
ms.assetid: 88125cfc-45c1-448b-9903-a629d8f31b01
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 10/05/2018
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: hirsin
ms.openlocfilehash: 372eac63b2ab9ea7dea540a088d61a5144886686
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76698603"
---
# <a name="how-azure-ad-uses-the-saml-protocol"></a>Azure AD, SAML protokolünü nasıl kullanır?

Azure Active Directory (Azure AD), uygulamaların kullanıcılarına çoklu oturum açma deneyimi sağlamasına olanak tanımak için SAML 2,0 protokolünü kullanır. Azure AD 'nin [Çoklu oturum açma](single-sign-on-saml-protocol.md) ve [Çoklu oturum açma](single-sign-out-saml-protocol.md) SAML profilleri, SAML onayları, protokolleri ve bağlamaları kimlik sağlayıcısı hizmetinde nasıl kullanıldığını açıklar.

SAML protokolü, kimlik sağlayıcısını (Azure AD) ve hizmet sağlayıcısını (uygulama) kendileri hakkındaki bilgileri değiş tokuş etmek için gereklidir.

Bir uygulama Azure AD 'ye kaydedildiğinde, uygulama geliştiricisi Federasyon ile ilgili bilgileri Azure AD 'ye kaydeder. Bu bilgiler, uygulamanın **yeniden YÖNLENDIRME URI** 'Sini ve **meta veri URI** 'sini içerir.

Azure AD, imzalama anahtarını ve oturum kapatma URI 'sini almak için bulut hizmetinin **meta VERI URI** 'sini kullanır. Müşteri, uygulamayı **Azure AD > uygulama kaydında** açabilir ve sonra **Ayarlar-> Özellikler**bölümünde oturum kapatma URL 'sini güncelleştirebilir. Bu şekilde, Azure AD yanıtı doğru URL 'ye gönderebilir. 

Azure Active Directory, kiracıya özgü ve ortak (kiracıdan bağımsız) çoklu oturum açma ve çoklu oturum açma uç noktalarını kullanıma sunar. Bu URL 'Ler adreslenebilir konumları temsil eder; bu, yalnızca tanımlayıcılardır; bu nedenle meta verileri okumak için uç noktaya gidebilirsiniz.

* Kiracıya özgü uç nokta `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`konumunda bulunur. *\<TenantDomainName >* yer tutucusu, BIR Azure AD kiracısının bir kayıtlı etki alanı adını veya tenantıd 'sini temsil eder. Örneğin, contoso.com kiracının Federasyon meta verileri: https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

* Kiracının bağımsız uç noktası `https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml`konumunda bulunur. Bu uç nokta adresinde, kiracı etki alanı adı veya KIMLIĞI yerine **yaygın** görüntülenir.

Azure AD 'nin yayımladığı Federasyon meta verileri belgeleri hakkında daha fazla bilgi için bkz. [Federasyon meta verileri](azure-ad-federation-metadata.md).
