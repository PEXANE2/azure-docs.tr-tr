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
ms.openlocfilehash: feed8b21833d4244d027d64d5e6547b94e4fa66f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75945179"
---
## <a name="what-happens-to-my-app-during-deployment"></a>Dağıtım sırasında uygulamama ne olur?

Resmi olarak desteklenen tüm dağıtım yöntemleri, uygulamanızın klasöründeki `/home/site/wwwroot` dosyalarda değişiklik yapar. Bu dosyalar uygulamanızı çalıştırmak için kullanılır. Bu nedenle, kilitli dosyalar nedeniyle dağıtım başarısız olabilir. Uygulama dağıtım sırasında da öngörülemez şekilde olabilir, çünkü tüm dosyalar aynı anda güncelleştirilmez. Bu, müşteriye yönelik bir uygulama için istenmeyen bir uygulamadır. Bu sorunları önlemek için birkaç farklı yolu vardır:

- [Uygulamanızı](../articles/app-service/deploy-run-package.md) açmadan doğrudan ZIP paketinden çalıştırın.
- Uygulamanızı durdurun veya dağıtım sırasında uygulamanız için çevrimdışı modu etkinleştirin. Daha fazla bilgi için dağıtım [sırasında kilitli dosyalarla başa çıkma](https://github.com/projectkudu/kudu/wiki/Dealing-with-locked-files-during-deployment)bilgisine bakın.
- [Otomatik takas](../articles/app-service/deploy-staging-slots.md#configure-auto-swap) etkinleştirilmiş bir [evreleme yuvasına](../articles/app-service/deploy-staging-slots.md) dağıtın. 
