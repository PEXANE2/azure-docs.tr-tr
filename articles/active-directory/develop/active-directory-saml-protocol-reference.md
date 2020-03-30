---
title: Azure AD, SAML protokolünü nasıl kullanır? Microsoft Dokümanlar
description: Bu makalede, Azure Etkin Dizini'ndeki Tek Oturum Açma ve Tek Oturum Açma SAML profillerine genel bir bakış sağlanır.
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
ms.openlocfilehash: dc7771f29fb5d00aedfe5162a98f5f0c14544a7b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77161180"
---
# <a name="how-azure-ad-uses-the-saml-protocol"></a>Azure AD, SAML protokolünü nasıl kullanır?

Azure Active Directory (Azure AD), uygulamaların kullanıcılarına tek bir oturum açma deneyimi sağlamasını sağlamak için SAML 2.0 protokolünü kullanır. Azure AD'nin [Tek Oturum Açma](single-sign-on-saml-protocol.md) ve Tek Oturum [Açma](single-sign-out-saml-protocol.md) SAML profilleri, SAML iddialarının, protokollerinin ve bağlamalarının kimlik sağlayıcı hizmetinde nasıl kullanıldığını açıklar.

SAML Protokolü, kimlik sağlayıcısının (Azure AD) ve hizmet sağlayıcısının (uygulama) kendileri hakkında bilgi alışverişinde bulunmalarını gerektirir.

Bir uygulama Azure AD'ye kaydedildiğinde, uygulama geliştiricisi federasyonla ilgili bilgileri Azure AD'ye kaydeder. Bu bilgiler, uygulamanın URI ve **Metadata URI** **Yönlendirme** içerir.

Azure AD, imzalama anahtarını ve oturum açma URI'sini almak için bulut hizmetinin **Metadata URI'sini** kullanır. Müşteri uygulamayı Azure **AD -> Uygulama Kaydı'nda** açabilir ve ardından **Ayarlar -> Özellikleri'nde**Logout URL'sini güncelleyebilir. Bu şekilde Azure AD yanıtı doğru URL'ye gönderebilir. 

Azure Etkin Dizin, kiracıya özel ve ortak (kiracıdan bağımsız) tek oturum açma ve tek oturum açma bitiş noktalarını ortaya çıkarır. Bu URL'ler adreslenebilir konumları temsil eder -- bunlar sadece tanımlayıcı değildir -- böylece meta verileri okumak için bitiş noktasına gidebilirsiniz.

* Kiracıya özgü bitiş `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`noktası. TenantDomainName>yer tutucusu, kayıtlı bir etki alanı adını veya Azure AD kiracısının TenantID GUID'ini temsil eder. * \<* Örneğin, contoso.com kiracının federasyon meta verileri şu şekildedir:https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

* Kiracıdan bağımsız bitiş `https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml`noktası. Bu bitiş noktası adresinde, kiracı etki alanı adı veya kimliği yerine **ortak** görünür.

Azure AD'nin yayımettiği federasyon meta veri belgeleri hakkında bilgi için [Federasyon Meta verileri](../azuread-dev/azure-ad-federation-metadata.md)bölümüne bakın.
