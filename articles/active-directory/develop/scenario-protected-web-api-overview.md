---
title: Korumalı Web API 'SI-genel bakış | Mavisi
description: Korumalı bir Web API 'SI (genel bakış) oluşturmayı öğrenin.
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
ms.collection: M365-identity-device-management
ms.openlocfilehash: 02bd4b84cc7542714f6db45c12c4b5b13a7fb449
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68852592"
---
# <a name="scenario-protected-web-api"></a>Senaryo: Korumalı web API'si

Bu senaryoda, bir Web API 'sini nasıl kullanıma sunabileceğiniz ve API 'ye yalnızca kimliği doğrulanmış kullanıcıların erişebilmesi için nasıl koruyabileceğiniz gösterilmektedir. Web API 'nizi kullanmak için hem iş hem de okul hesaplarıyla kimliği doğrulanmış kullanıcıları veya kişisel Microsoft kişisel hesaplarını etkinleştirmek isteyeceksiniz.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="specifics"></a>Özelliklerini

Web API 'Lerini korumak için bilmeniz gereken bazı ayrıntılar aşağıda verilmiştir:

- Uygulama kaydınız en az bir kapsam kullanıma sunmalıdır. Web API 'niz tarafından kabul edilen belirteç sürümü, hedef kitlede bulunan oturum açma durumuna bağlıdır.
- Web API 'SI için kodun yapılandırması, Web API 'SI çağrılırken kullanılan belirteci doğrulamalıdır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kaydı](scenario-protected-web-api-app-registration.md)
