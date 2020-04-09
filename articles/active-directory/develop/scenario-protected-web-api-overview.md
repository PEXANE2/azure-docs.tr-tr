---
title: Korumalı Web API - genel bakış
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
ms.openlocfilehash: 9017fbde1a44bcdf39fa8730bed25141da19fd56
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80882412"
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
