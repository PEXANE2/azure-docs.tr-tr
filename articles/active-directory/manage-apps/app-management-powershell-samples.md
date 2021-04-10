---
title: Azure Active Directory Uygulama yönetimi için PowerShell örnekleri
description: Bu PowerShell örnekleri, Azure Active Directory kiracınızda yönettiğiniz uygulamalar için kullanılır. Bu örnek betikleri, gizli diziler ve sertifikalar hakkında süre sonu bilgilerini bulmak için kullanabilirsiniz.
services: active-directory
author: kenwith
manager: daveba
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: sample
ms.date: 02/18/2021
ms.author: kenwith
ms.reviewer: mifarca
ms.openlocfilehash: f5f7ec8245a43440a400b9ca6b55bf1093eb62cc
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "102636197"
---
# <a name="azure-active-directory-powershell-examples-for-application-management"></a>Uygulama yönetimi için PowerShell örnekleri Azure Active Directory

Aşağıdaki tabloda, Azure AD uygulama yönetimi için PowerShell betiği örneklerinin bağlantıları yer almaktadır. Bu örnekler şunları gerektirir:
- [Grafik modülü veya, Için Azuread v2 PowerShell](/powershell/azure/active-directory/install-adv2)
- Aksi belirtilmediği takdirde, [grafik modülü önizleme sürümü Için Azuread v2 PowerShell](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview&preserve-view=true).

Bu örneklerde kullanılan cmdlet 'ler hakkında daha fazla bilgi için bkz. [uygulamalar](/powershell/module/azuread/#applications).

| Bağlantı | Description |
|---|---|
|**Uygulama yönetimi betikleri**||
| [Gizli dizileri ve sertifikaları dışa aktarma (uygulama kayıtları)](scripts/powershell-export-all-app-registrations-secrets-and-certs.md) | Azure Active Directory kiracısındaki uygulama kayıtları için gizli dizileri ve sertifikaları dışarı aktarın. |
| [Gizli dizileri ve sertifikaları dışa aktarma (kurumsal uygulamalar)](scripts/powershell-export-all-enterprise-apps-secrets-and-certs.md) | Azure Active Directory kiracısındaki kurumsal uygulamalar için gizli dizileri ve sertifikaları dışarı aktarın. |
| [Süresi dolan gizli dizileri ve sertifikaları dışa aktarma](scripts/powershell-export-apps-with-expriring-secrets.md) | Azure Active Directory kiracısındaki süresi dolan gizli dizileri ve sertifikaları ve onların sahiplerini kullanarak uygulama kayıtlarını dışarı aktarın. |
| [Parolaların ve sertifikaların süresi gereken tarihten sonra dışarı aktarma](scripts/powershell-export-apps-with-secrets-beyond-required.md) | Uygulama kayıtlarını gizli dizileri ve sertifikalarla dışa aktarma Azure Active Directory kiracısında gerekli tarihin ötesinde süresi dolar. Bu, etkileşimli olmayan Client_Credentials OAuth akışını kullanır. |
