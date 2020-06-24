---
title: Azure AD Uygulama Ara Sunucusu için PowerShell örnekleri
description: Dizininizde uygulama proxy 'Si uygulamaları ve bağlayıcılar hakkında bilgi almak, uygulamalara Kullanıcı ve grupları atamak ve sertifika bilgilerini almak için Azure AD Uygulama Ara Sunucusu için bu PowerShell örneklerini kullanın.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: sample
ms.date: 12/05/2019
ms.author: kenwith
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6e624be8103097a8356ae4167cb8f21cce819649
ms.sourcegitcommit: bc943dc048d9ab98caf4706b022eb5c6421ec459
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2020
ms.locfileid: "84760193"
---
# <a name="azure-ad-powershell-examples-for-azure-ad-application-proxy"></a>Azure AD Uygulama Ara Sunucusu için Azure AD PowerShell örnekleri

Aşağıdaki tablo Azure AD Uygulama Ara Sunucusu için PowerShell betiği örneklerine bağlantılar içerir. Bu örnekler, aksi belirtilmedikçe, Graph modülü [Için Azuread v2 PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0) veya [Graf modülü önizleme sürümü Için azuread v2 PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview)gerektirir.


Bu örneklerde kullanılan cmdlet 'ler hakkında daha fazla bilgi için bkz. [uygulama proxy 'Si uygulama yönetimi](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#application_proxy_application_management) ve [uygulama proxy Bağlayıcısı yönetimi](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#application_proxy_connector_management).

| | |
|---|---|
|**Uygulama proxy 'Si uygulamaları**||
| [Tüm uygulama proxy uygulamaları için temel bilgileri listeleyin](scripts/powershell-get-all-app-proxy-apps-basic.md) | Dizininizdeki tüm uygulama proxy uygulamaları hakkında temel bilgileri (AppID, DisplayName, ObjId) listeler. |
| [Tüm uygulama proxy uygulamaları için genişletilmiş bilgileri listeleyin](scripts/powershell-get-all-app-proxy-apps-extended.md) | Dizininizdeki tüm uygulama proxy uygulamaları hakkında genişletilmiş bilgileri (AppID, DisplayName, ExternalUrl, InternalUrl, ExternalAuthenticationType) listeler.  |
| [Tüm uygulama proxy uygulamalarını bağlayıcı grubuna göre Listele](scripts/powershell-get-all-app-proxy-apps-by-connector-group.md) | Dizininizdeki tüm uygulama proxy uygulamalarıyla ilgili bilgileri ve uygulamaların hangi bağlayıcı gruplarına atandığını listeler. |
| [Belirteç ömür ilkesi ile tüm uygulama proxy uygulamalarını edinme](scripts/powershell-get-all-app-proxy-apps-with-policy.md) | Bir belirteç ömür ilkesi ve ayrıntıları ile dizininizdeki tüm uygulama proxy uygulamalarını listeler. Bu örnek, [Graf modülü önizleme sürümü Için Azuread v2 PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview)'i gerektirir. |
|**Bağlayıcı grupları**||
| [Dizindeki tüm bağlayıcı gruplarını ve bağlayıcıları al](scripts/powershell-get-all-connectors.md) | Dizininizdeki tüm bağlayıcı gruplarını ve bağlayıcıları listeler. |
| [Bir bağlayıcı grubuna atanan tüm uygulamaları başka bir bağlayıcı grubuna Taşı](scripts/powershell-move-all-apps-to-connector-group.md) | Bir bağlayıcı grubuna şu anda atanmış olan tüm uygulamaları farklı bir bağlayıcı grubuna taşırlar. |
|**Atanan kullanıcılar ve Grup**||
| [Uygulama proxy 'Si uygulamasına atanan kullanıcıları ve grupları görüntüleme](scripts/powershell-display-users-group-of-app.md) | Belirli bir uygulama proxy 'Si uygulamasına atanan kullanıcıları ve grupları listeler. |
| [Bir uygulamaya Kullanıcı atama](scripts/powershell-assign-user-to-app.md) | Uygulamaya belirli bir Kullanıcı atar. |
| [Bir uygulamaya Grup atama](scripts/powershell-assign-group-to-app.md) | Uygulamaya belirli bir grup atar. |
|**Dış URL yapılandırması**||
| [Varsayılan etki alanlarını (. msappproxy.net) kullanarak tüm uygulama proxy uygulamalarını al](scripts/powershell-get-all-default-domain-apps.md)  | Varsayılan etki alanlarını (. msappproxy.net) kullanan tüm uygulama proxy uygulamalarını listeler. |
| [Joker bir yayımlama kullanarak tüm uygulama proxy uygulamalarını al](scripts/powershell-get-all-wildcard-apps.md) | Joker bir yayımlama kullanan tüm uygulama proxy uygulamalarını listeler. |
|**Özel etki alanı yapılandırması**||
| [Özel etki alanları ve sertifika bilgilerini kullanarak tüm uygulama proxy uygulamalarını alın](scripts/powershell-get-all-custom-domains-and-certs.md) | Özel etki alanları kullanan tüm uygulama proxy uygulamalarını ve özel etki alanlarıyla ilişkili sertifika bilgilerini listeler. |
| [Hiçbir sertifika karşıya yüklemeden yayınlanan tüm Azure AD proxy uygulama uygulamalarını alın](scripts/powershell-get-all-custom-domain-no-cert.md) | Özel etki alanları kullanan ancak geçerli bir TLS/SSL sertifikasına sahip olmayan tüm uygulama proxy uygulamalarını listeler. |
| [Aynı sertifikayla yayınlanan tüm Azure AD proxy uygulama uygulamalarını alın](scripts/powershell-get-custom-domain-identical-cert.md) | Aynı sertifikayla yayınlanan tüm Azure AD proxy uygulama uygulamalarını listeler. |
| [Aynı sertifikayla yayınlanan tüm Azure AD proxy uygulama uygulamalarını alın ve değiştirin](scripts/powershell-get-custom-domain-replace-cert.md) | Aynı sertifikayla yayınlanan Azure AD proxy uygulama uygulamaları için, sertifikayı toplu olarak değiştirmenize izin verir. |
