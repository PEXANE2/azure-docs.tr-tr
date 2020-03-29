---
title: Özel olarak geliştirilmiş bir uygulamada oturum açma sorunları | Microsoft Dokümanlar
description: Azure AD ile geliştirdiğiniz bir uygulamada oturum açamamanıza neden olabilecek sık karşılaşılan hatalar
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
ms.openlocfilehash: b8ad2499aea8bf4e41ca00d6c78d76e112f0493e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65825232"
---
# <a name="problems-signing-in-to-a-custom-developed-application"></a>Özel olarak geliştirilmiş bir uygulamada oturum açma sorunları

Bir uygulamada oturum açamamanıza neden olabilecek çeşitli hatalar vardır. İnsanların bu sorunla karşılaşmasının en büyük nedeni yanlış yapılandırılmış uygulamalardır.

## <a name="errors-related-to--misconfigured-apps"></a>Yanlış yapılandırılmış uygulamalarla ilgili hatalar

* Portaldaki her iki yapılandırmanın da uygulamanızdakilerle eşleşin. Özellikle, İstemci/Uygulama Kimliği, Yanıt URL'leri, İstemci Sırları/Anahtarları ve Uygulama Kimliği URI'yi karşılaştırın.

* Yalnızca yapılandırdığınız kaynakları istediğinizden emin olmak için, kod içinde erişmek istediğiniz kaynağı **Gerekli Kaynaklar** sekmesinde yapılandırılan izinlerle karşılaştırın.

* Benzer hatalar veya sorunlar için [Azure AD StackOverflow'a](https://stackoverflow.com/questions/tagged/azure-active-directory) bakın.

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD Geliştirici Kılavuzu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)<br>

[Uygulamaları Azure AD'ye İzin Verme ve Tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)<br>

[Azure AD v2.0 yakınsanan Uygulamalar için Onay ve İzin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[Azure AD StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory>)
