---
title: Azure AD Application Proxy için PowerShell örnekleri
description: Dizininizdeki Uygulama Proxy uygulamaları ve bağlayıcıları hakkında bilgi almak, kullanıcıları ve grupları uygulamalara atamak ve sertifika bilgilerini almak için Azure AD Application Proxy için bu PowerShell örneklerini kullanın.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 12/05/2019
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 476bdfdd2c0b81d86900cee18024cc261ee80a07
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79481271"
---
# <a name="azure-ad-powershell-examples-for-azure-ad-application-proxy"></a>Azure AD Uygulama Proxy için Azure AD PowerShell örnekleri

Aşağıdaki tablo, Azure AD Application Proxy için PowerShell komut dosyası örneklerine bağlantılar içerir. Bu örnekler, aksi belirtilmedikçe, [Grafik modülü için AzureAD V2 PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0) veya [Graph modülü önizleme sürümü gerektirir.](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview)


Bu örneklerde kullanılan cmdlets hakkında daha fazla bilgi için, [Uygulama Proxy Uygulama Yönetimi](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#application_proxy_application_management) ve Uygulama Proxy Bağlayıcı [Yönetimi](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#application_proxy_connector_management)bakın.

| | |
|---|---|
|**Uygulama Proxy uygulamaları**||
| [Tüm Uygulama Proxy uygulamaları için temel bilgileri listeleyin](scripts/powershell-get-all-app-proxy-apps-basic.md) | Dizininizdeki tüm Uygulama Proxy uygulamaları hakkında temel bilgileri (AppId, DisplayName, ObjId) listeler. |
| [Tüm Uygulama Proxy uygulamaları için genişletilmiş bilgileri listele](scripts/powershell-get-all-app-proxy-apps-extended.md) | Dizininizdeki tüm Uygulama Proxy uygulamaları hakkında genişletilmiş bilgileri (AppId, DisplayName, ExternalUrl, InternalUrl, ExternalAuthenticationType) listeler.  |
| [Bağlayıcı grubuna göre tüm Uygulama Proxy uygulamalarını listele](scripts/powershell-get-all-app-proxy-apps-by-connector-group.md) | Dizininizdeki tüm Uygulama Proxy uygulamaları ve uygulamaların atandığı bağlayıcı grupları hakkındaki bilgileri listeler. |
| [Bir belirteç ömür boyu ilkesi ile tüm Uygulama Proxy uygulamaları alın](scripts/powershell-get-all-app-proxy-apps-with-policy.md) | Bir belirteç ömür boyu ilkesi ve ayrıntıları ile dizininizdeki tüm Uygulama Proxy uygulamaları listeler. Bu [örnek, Grafik modülü önizleme sürümü için AzureAD V2 PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview)gerektirir. |
|**Bağlayıcı grupları**||
| [Tüm bağlayıcı gruplarını ve bağlayıcıları dizine alma](scripts/powershell-get-all-connectors.md) | Dizininizdeki tüm bağlayıcı gruplarını ve bağlayıcıları listeler. |
| [Bağlayıcı grubuna atanan tüm uygulamaları başka bir bağlayıcı grubuna taşıma](scripts/powershell-move-all-apps-to-connector-group.md) | Şu anda bir bağlayıcı grubuna atanan tüm uygulamaları farklı bir bağlayıcı grubuna taşır. |
|**Kullanıcılar ve atanan grup**||
| [Uygulama Proxy uygulamasına atanan kullanıcıları ve grupları görüntüleme](scripts/powershell-display-users-group-of-app.md) | Belirli bir Uygulama Proxy uygulamasına atanan kullanıcıları ve grupları listeler. |
| [Bir uygulamaya kullanıcı atama](scripts/powershell-assign-user-to-app.md) | Bir uygulamaya belirli bir kullanıcı atar. |
| [Bir uygulamaya grup atama](scripts/powershell-assign-group-to-app.md) | Bir uygulamaya belirli bir grup atar. |
|**Harici URL yapılandırması**||
| [Varsayılan etki alanlarını kullanarak tüm Uygulama Proxy uygulamalarını edinin (.msappproxy.net)](scripts/powershell-get-all-default-domain-apps.md)  | Varsayılan etki alanlarını (.msappproxy.net) kullanarak tüm Uygulama Proxy uygulamalarını listeler. |
| [Joker karakter yayımlamayı kullanarak tüm Application Proxy uygulamalarını edinin](scripts/powershell-get-all-wildcard-apps.md) | Joker karakter yayımlamayı kullanarak tüm Uygulama Proxy uygulamalarını listeler. |
|**Özel Etki Alanı yapılandırması**||
| [Özel etki alanlarını ve sertifika bilgilerini kullanarak tüm Uygulama Proxy uygulamalarını edinin](scripts/powershell-get-all-custom-domains-and-certs.md) | Özel etki alanlarını kullanan tüm Uygulama Proxy uygulamalarını ve özel etki alanlarıyla ilişkili sertifika bilgilerini listeler. |
| [Sertifika yüklenmeden tüm Azure AD Proxy uygulama uygulamalarını yayından edinin](scripts/powershell-get-all-custom-domain-no-cert.md) | Özel etki alanları kullanan ancak geçerli bir TLS/SSL sertifikası yüklenen olmayan tüm Uygulama Proxy uygulamalarını listeler. |
| [Tüm Azure AD Proxy uygulama uygulamalarını aynı sertifikayla yayımlanın](scripts/powershell-get-custom-domain-identical-cert.md) | Aynı sertifikayla yayınlanan tüm Azure AD Proxy uygulama uygulamalarını listeler. |
| [Tüm Azure AD Proxy uygulama uygulamalarını aynı sertifikayla yayınlatın ve değiştirin](scripts/powershell-get-custom-domain-replace-cert.md) | Aynı sertifikayla yayınlanan Azure AD Proxy uygulamaları için, sertifikayı toplu olarak değiştirmenize olanak tanır. |
