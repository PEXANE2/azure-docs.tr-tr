---
title: Özel olarak geliştirilmiş bir uygulamada oturum açma sorunları | Microsoft Docs
description: Azure AD ile geliştirmiş olduğunuz bir uygulamada oturum açmanıza neden olabilecek yaygın hatalar
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 07/11/2017
ms.author: kenwith
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: cdfc2a39c6bd3b68df7feb978d2548ad67631235
ms.sourcegitcommit: bc943dc048d9ab98caf4706b022eb5c6421ec459
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2020
ms.locfileid: "84759139"
---
# <a name="problems-signing-in-to-a-custom-developed-application"></a>Özel olarak geliştirilmiş bir uygulamada oturum açma sorunları

Bir uygulamada oturum açmanıza neden olabilecek birkaç hata vardır. İnsanların bu sorundan karşılaştığı en büyük neden yanlış yapılandırılmış uygulamalardır.

## <a name="errors-related-to--misconfigured-apps"></a>Yanlış yapılandırılmış uygulamalarla ilgili hatalar

* Portaldaki yapılandırmaların, uygulamanızda sahip olduğunuz ile eşleştiğini doğrulayın. Özellikle, Istemci/uygulama KIMLIĞINI, yanıt URL 'Lerini, Istemci gizli dizilerini/anahtarlarını ve uygulama KIMLIĞI URI 'sini karşılaştırın.

* Yalnızca yapılandırdığınız kaynakları istediğinizden emin olmak için, kodda erişim isteğinde bulunan kaynağı **gerekli kaynaklar** sekmesinde yapılandırılan izinlerle karşılaştırın.

* Benzer hatalar veya sorunlar için bkz. [Azure AD StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory) .

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD Geliştirici Kılavuzu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)<br>

[Uygulamaları Azure AD 'ye onay ve Tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)<br>

[Microsoft kimlik platformu uç noktasında izinler ve onay](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[Azure AD StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory>)
