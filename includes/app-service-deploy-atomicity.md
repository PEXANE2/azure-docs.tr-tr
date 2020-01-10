---
title: include dosyası
description: include dosyası
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 01/07/2020
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 3d9b0a7f08efcf9ea5892467b638e95d0404f35f
ms.sourcegitcommit: c32050b936e0ac9db136b05d4d696e92fefdf068
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75752127"
---
## <a name="what-happens-to-my-app-during-deployment"></a>Dağıtım sırasında uygulamama ne olur?

Resmi olarak desteklenen tüm dağıtım yöntemleri, uygulamanızın `/home/site/wwwroot` klasöründeki dosyalarda değişiklikler yapar. Bu dosyalar, uygulamanızı çalıştırmak için kullanılır. Bu nedenle, dağıtım kilitli dosyalar nedeniyle başarısız olabilir. Aynı zamanda tüm dosyalar aynı anda güncelleştirilemediğinden, uygulama dağıtım sırasında tahmin edilemeyecek şekilde davranabilir. Bu, müşteriye yönelik bir uygulama için istenmeyen bir uygulamadır. Bu sorunlardan kaçınmak için birkaç farklı yol vardır:

- Dağıtım sırasında uygulamanızı durdurun veya uygulamanız için çevrimdışı modu etkinleştirin. Daha fazla bilgi için bkz. [dağıtım sırasında kilitli dosyalarla uğraşın](https://github.com/projectkudu/kudu/wiki/Dealing-with-locked-files-during-deployment).
- [Otomatik takas](../articles/app-service/deploy-staging-slots.md#configure-auto-swap) özellikli bir [hazırlama yuvasına](../articles/app-service/deploy-staging-slots.md) dağıtın. 
- Sürekli dağıtım yerine [paketten Çalıştır 'ı](https://github.com/Azure/app-service-announcements/issues/84) kullanın.
