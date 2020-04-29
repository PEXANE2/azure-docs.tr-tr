---
title: Korumalı Web API 'SI-genel bakış
titleSuffix: Microsoft identity platform
description: Korumalı bir Web API 'SI (genel bakış) oluşturmayı öğrenin.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81537211"
---
# <a name="scenario-protected-web-api"></a>Senaryo: korumalı Web API 'SI

Bu senaryoda, bir Web API 'sini kullanıma sunma hakkında bilgi edineceksiniz. Ayrıca, Web API 'sini yalnızca kimliği doğrulanmış kullanıcıların erişebileceği şekilde nasıl koruyacağınızı öğreneceksiniz.

Web API 'nizi kullanmak için hem iş hem de okul hesaplarıyla kimliği doğrulanmış kullanıcıları etkinleştirmeniz ya da Microsoft kişisel hesaplarını etkinleştirmeniz gerekir.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="specifics"></a>Özelliklerini

Web API 'Lerini korumak için bilmeniz gereken belirli bilgiler aşağıda verilmiştir:

- Uygulama kaydınız en az bir kapsam kullanıma sunmalıdır. Web API 'niz tarafından kabul edilen belirteç sürümü, oturum açma izleyicisine bağlıdır.
- Web API 'SININ kod yapılandırması, Web API 'SI çağrıldığında kullanılan belirteci doğrulamalıdır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kaydı](scenario-protected-web-api-app-registration.md)
