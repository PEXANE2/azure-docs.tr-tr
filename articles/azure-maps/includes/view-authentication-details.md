---
title: Azure haritalar 'ın kimlik doğrulama ayrıntılarını görüntüleme
description: Azure haritalar 'ın kimlik doğrulama ayrıntılarını görüntülemek için Azure portal kullanın.
author: philmea
ms.author: philmea
ms.date: 06/17/2020
ms.topic: include
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 9146844a6b83f78ad99ef7cd1aec4b028daf3ff6
ms.sourcegitcommit: 55b2bbbd47809b98c50709256885998af8b7d0c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84988678"
---
Azure haritalar hesabı kimlik doğrulama ayrıntılarını Azure portal görüntüleyebilirsiniz. Burada, hesabınızda, **Ayarlar** menüsünde **kimlik doğrulaması**' nı seçin.

![Kimlik doğrulaması ayrıntıları](../media/how-to-manage-authentication/how-to-view-auth.png)

Azure haritalar hesabı oluşturulduktan sonra, `x-ms-client-id` Azure Portal kimlik doğrulaması ayrıntıları sayfasında Azure Maps değeri mevcuttur. Bu değer, REST API istekleri için kullanılacak hesabı temsil eder. Bu değer, uygulama yapılandırmasında depolanmalıdır ve Azure Maps ile Azure AD kimlik doğrulaması kullanılırken http istekleri yapılmadan önce alınmalıdır.
