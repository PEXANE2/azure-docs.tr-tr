---
title: Özel olarak geliştirilmiş bir uygulamada oturum açma sorunları | Microsoft Docs
description: Azure AD ile geliştirmiş olduğunuz bir uygulamada oturum açmanıza neden olabilecek yaygın hatalar
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 01a424129abf88c18500c96bd1889fc6dcce2ec6
ms.sourcegitcommit: b396c674aa8f66597fa2dd6d6ed200dd7f409915
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2020
ms.locfileid: "82890698"
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

[Microsoft Identity platform uç noktasındaki izinler ve onay](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[Azure AD StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory>)
