---
title: Microsoft 365 Government'da Azure AD için yenilikler nelerdir? | Microsoft Belgeleri
description: Microsoft 365 Devlet bulutu örneğinde sizi etkileyebilecek Azure Etkin Dizin (Azure AD) ile ilgili bazı değişiklikler hakkında bilgi edinin.
services: active-directory
author: eross-msft
manager: daveba
ms.author: lizross
ms.reviewer: sumitp
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 05/07/2019
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 48a7e5e810e509a97b6010218532f6e7dff0b73f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77425553"
---
# <a name="whats-new-for-azure-active-directory-in-microsoft-365-government"></a>Microsoft 365 Government'da Azure Active Directory için yenilikler

Aşağıdaki hizmetleri kullanan müşteriler için geçerli olan Microsoft 365 Genel bulut örneğinde Azure Etkin Dizini'nde (Azure AD) bazı değişiklikler yaptık:

- Microsoft Azure Yönetimi

- Microsoft 365 Hükümet - GCC Yüksek

- Microsoft 365 Hükümet - DoD

Bu makale Microsoft 365 Government - GCC müşterileri için geçerli değildir.

## <a name="changes-to-the-initial-domain-name"></a>İlk etki alanı adındaki değişiklikler

Kuruluşunuzun bir Microsoft 365 Devlet çevrimiçi hizmetine ilk kaydolması sırasında, kuruluşunuzun etki `<your-domain-name>.onmicrosoft.com`alanı adını seçmeniz istendi. Zaten .com soneki olan bir etki alanı adınız varsa, hiçbir şey değişmez.

Ancak, yeni bir Microsoft 365 Devlet hizmetine kaydoluyorsanız, `.us` soneki kullanarak bir etki alanı adı seçmeniz istenir. Yani, öyle `<your-domain-name>.onmicrosoft.us`olacak.

>[!Note]
>Bu değişiklik, bulut hizmeti sağlayıcıları (CSP' ler) tarafından yönetilen müşteriler için geçerli değildir.

## <a name="changes-to-portal-access"></a>Portal erişiminde yapılan değişiklikler

[Bitiş Noktası eşleme tablosunda](#endpoint-mapping)gösterildiği gibi Microsoft Azure Hükümeti, Microsoft 365 Government – GCC High ve Microsoft 365 Government - DoD için portal uç noktalarını güncelledik.

Daha önce müşteriler dünya çapındaki Azure (portal.azure.com) ve Office 365 (portal.office.com) portallarını kullanarak oturum açabilir. Bu güncelleştirmeyle, müşterilerin artık belirli Microsoft Azure Hükümeti, Microsoft 365 Government - GCC High ve Microsoft 365 Government - DoD portallarını kullanarak oturum açmaları gerekmektedir.

## <a name="endpoint-mapping"></a>Bitiş noktası eşleme

Aşağıdaki tablo, tüm müşteriler için uç noktaları gösterir:

| Adı | Bitiş noktası ayrıntıları |
|------|------------------|
| Portallar |Microsoft Azure Yönetimi:https://portal.azure.us<p>Microsoft 365 Hükümet - GCC Yüksek:https://portal.office365.us<p>Microsoft 365 Hükümet - DoD:https://portal.apps.mil |
| Azure Active Directory Authority Bitiş Noktası | https://login.microsoftonline.us |
| Microsoft 365 Hükümet için Microsoft Graph API - GCC Yüksek | https://graph.microsoft.us |
| Microsoft 365 Hükümet için Microsoft Grafik API - DoD | https://dod-graph.microsoft.us |
| Azure Kamu hizmetleri uç noktaları | Ayrıntılar için Azure [Kamu geliştirici kılavuzuna](https://docs.microsoft.com/azure/azure-government/documentation-government-developer-guide) bakın |
| Microsoft 365 Hükümet - GCC Yüksek uç noktaları | Ayrıntılar için [Bkz. Office 365 ABD Hükümeti GCC Yüksek uç noktaları](https://docs.microsoft.com/office365/enterprise/office-365-u-s-government-gcc-high-endpoints) |
| Microsoft 365 Hükümet - DoD | Ayrıntılar için [Bkz. Office 365 ABD Hükümeti DoD bitiş noktaları](https://docs.microsoft.com/office365/enterprise/office-365-u-s-government-dod-endpoints) |

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için şu makalelere bakın:

- [Azure Government nedir?](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome)

- [Azure Devlet AAD Yetkilisi Bitiş Noktası Güncelleştirmesi](https://devblogs.microsoft.com/azuregov/azure-government-aad-authority-endpoint-update/)

- [Microsoft Graph, ABD Hükümeti bulutundaki uç noktaları](https://developer.microsoft.com/graph/blogs/new-microsoft-graph-endpoints-in-us-government-cloud/)

- [Office 365 ABD Hükümeti GCC Yüksek ve DoD](https://docs.microsoft.com/office365/servicedescriptions/office-365-platform-service-description/office-365-us-government/gcc-high-and-dod)
