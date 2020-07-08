---
title: dosya dahil etme
description: dosya dahil etme
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 01/07/2020
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: feed8b21833d4244d027d64d5e6547b94e4fa66f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75945179"
---
## <a name="what-happens-to-my-app-during-deployment"></a>Dağıtım sırasında uygulamama ne olur?

Resmi olarak desteklenen tüm dağıtım yöntemleri, uygulamanızın klasöründeki dosyalarda değişiklikler yapar `/home/site/wwwroot` . Bu dosyalar, uygulamanızı çalıştırmak için kullanılır. Bu nedenle, dağıtım kilitli dosyalar nedeniyle başarısız olabilir. Aynı zamanda tüm dosyalar aynı anda güncelleştirilemediğinden, uygulama dağıtım sırasında tahmin edilemeyecek şekilde davranabilir. Bu, müşteriye yönelik bir uygulama için istenmeyen bir uygulamadır. Bu sorunlardan kaçınmak için birkaç farklı yol vardır:

- [Uygulamanızı paketten çıkmadan doğrudan ZIP paketinden çalıştırın](../articles/app-service/deploy-run-package.md) .
- Dağıtım sırasında uygulamanızı durdurun veya uygulamanız için çevrimdışı modu etkinleştirin. Daha fazla bilgi için bkz. [dağıtım sırasında kilitli dosyalarla uğraşın](https://github.com/projectkudu/kudu/wiki/Dealing-with-locked-files-during-deployment).
- [Otomatik takas](../articles/app-service/deploy-staging-slots.md#configure-auto-swap) özellikli bir [hazırlama yuvasına](../articles/app-service/deploy-staging-slots.md) dağıtın. 
