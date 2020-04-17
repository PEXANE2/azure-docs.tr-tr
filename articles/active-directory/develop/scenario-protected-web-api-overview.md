---
title: Korumalı web API - genel bakış
titleSuffix: Microsoft identity platform
description: Korumalı bir web API'sini (genel bakış) nasıl oluşturabildiğini öğrenin.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: cf66757d28a3883664aaacd85baad9cc0dea6956
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81537211"
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
