---
title: Korumalı Web API - genel bakış
titleSuffix: Microsoft identity platform
description: Korumalı bir web API'sini (genel bakış) nasıl oluşturabildiğini öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: abe4ecf77e7a65251830ff822b15f79291471202
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76773346"
---
# <a name="scenario-protected-web-api"></a>Senaryo: Korumalı web API

Bu senaryoda, bir web API'sini nasıl ortaya çıkarabileceğinizi öğrenirsiniz. Web API'sini nasıl koruyacağınızı da öğrenirsiniz, böylece yalnızca kimlik doğrulaması yapılan kullanıcılar bu api'ye erişebilir.

Web API'nizi kullanmak için, kimlik doğrulaması hem iş hem de okul hesapları olan kullanıcıları etkinleştirmeniz veya Microsoft'un kişisel hesaplarını etkinleştirmeniz gerekir.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="specifics"></a>Özellikleri

Web API'lerini korumak için bilmeniz gereken belirli bilgiler şunlardır:

- Uygulama kaydınız en az bir kapsamı ortaya çıkarmalıdır. Web API'nız tarafından kabul edilen belirteç sürümü oturum açma hedef kitlesine bağlıdır.
- Web API'sinin kod yapılandırması, web API'sı çağrıldığında kullanılan belirteci doğrulamalıdır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kaydı](scenario-protected-web-api-app-registration.md)
