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
ms.openlocfilehash: 15270a998aff174c04acf2969d984eb022852635
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88117371"
---
# <a name="how-application-consent-works"></a>Uygulama onayının işleyiş biçimi

Bu makale, Azure AD onay çerçevesinin nasıl çalıştığı hakkında daha fazla bilgi edinmenize yardımcı olur. böylece, uygulamaları daha verimli bir şekilde geliştirebilirsiniz.

## <a name="recommended-documents"></a>Önerilen belgeler

- [Bir kaynak sahibinin, bir uygulamanın kaynaklara erişimini yönetme konusunda](./developer-glossary.md#consent)genel bir bakış sağlar.
- [Azure AD onay çerçevesinin izin nasıl uyguladığı konusunda](./quickstart-register-app.md)adım adım bir genel bakış alın.
- Daha ayrıntılı bilgi edinmek için, çok kiracılı bir uygulamanın "Kullanıcı" ve "Yönetici" onayını uygulamak için [izin çerçevesini nasıl kullandığını](./howto-convert-app-to-be-multi-tenant.md) öğrenin ve daha gelişmiş çok katmanlı uygulama düzenlerini destekler.
- Daha ayrıntılı bilgi edinmek için [yetkilendirme kodu verme akışı sırasında OAuth 2,0 protokol katmanında izin nasıl desteklendiğini öğrenin.](../azuread-dev/v1-protocols-oauth-code.md#request-an-authorization-code)

## <a name="next-steps"></a>Sonraki adımlar
[AzureAD StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)