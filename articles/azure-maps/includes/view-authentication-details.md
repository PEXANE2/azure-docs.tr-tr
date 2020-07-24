---
title: Azure haritalar 'ın kimlik doğrulama ayrıntılarını görüntüleme
description: Azure haritalar 'ın kimlik doğrulama ayrıntılarını görüntülemek için Azure portal kullanın.
author: anastasia-ms
ms.author: v-stharr
ms.date: 06/17/2020
ms.topic: include
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: a6ffcbf5a8c36958dd3ea74de4d826fe25a1139c
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87126473"
---
Azure haritalar hesabı kimlik doğrulama ayrıntılarını Azure portal görüntüleyebilirsiniz. Burada, hesabınızda, **Ayarlar** menüsünde **kimlik doğrulaması**' nı seçin.

![Kimlik doğrulaması ayrıntıları](../media/how-to-manage-authentication/how-to-view-auth.png)

Azure haritalar hesabı oluşturulduktan sonra, `x-ms-client-id` Azure Portal kimlik doğrulaması ayrıntıları sayfasında Azure Maps değeri mevcuttur. Bu değer, REST API istekleri için kullanılacak hesabı temsil eder. Bu değer, uygulama yapılandırmasında depolanmalıdır ve Azure Maps ile Azure AD kimlik doğrulaması kullanılırken http istekleri yapılmadan önce alınmalıdır.
