---
title: Uygulama onayının işleyiş biçimi
description: Azure AD onay çerçevesinin nasıl çalıştığı hakkında daha fazla bilgi edinmek için bkz. Azure AD 'de uygulama geliştirirken bu uygulamayı nasıl kullanabileceğinizi öğrenin
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: ryanwi
ROBOTS: NOINDEX
ms.openlocfilehash: cd55375f2c5970ff0620e753923f369d40478cef
ms.sourcegitcommit: d1e56036f3ecb79bfbdb2d6a84e6932ee6a0830e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99052034"
---
# <a name="how-application-consent-works"></a>Uygulama onayının işleyiş biçimi

Bu makale, Azure AD onay çerçevesinin nasıl çalıştığı hakkında daha fazla bilgi edinmenize yardımcı olur. böylece, uygulamaları daha verimli bir şekilde geliştirebilirsiniz.

## <a name="recommended-documents"></a>Önerilen belgeler

- [Bir kaynak sahibinin, bir uygulamanın kaynaklara erişimini yönetme konusunda](./developer-glossary.md#consent)genel bir bakış sağlar.
- [Azure AD onay çerçevesinin izin nasıl uyguladığı konusunda](./quickstart-register-app.md)adım adım bir genel bakış alın.
- Daha ayrıntılı bilgi edinmek için, çok kiracılı bir uygulamanın "Kullanıcı" ve "Yönetici" onayını uygulamak için [izin çerçevesini nasıl kullandığını](./howto-convert-app-to-be-multi-tenant.md) öğrenin ve daha gelişmiş çok katmanlı uygulama düzenlerini destekler.
- Daha ayrıntılı bilgi edinmek için [yetkilendirme kodu verme akışı sırasında OAuth 2,0 protokol katmanında izin nasıl desteklendiğini öğrenin.](../azuread-dev/v1-protocols-oauth-code.md#request-an-authorization-code)

## <a name="next-steps"></a>Sonraki adımlar
[AzureAD Microsoft Q&A](https://docs.microsoft.com/answers/topics/azure-active-directory.html)