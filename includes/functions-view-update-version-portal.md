---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/26/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: d06bda1826964b019edb156375885c7f389ca6ec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67189020"
---
Bir işlev uygulaması tarafından şu anda kullanılan çalışma zamanı sürümünü görüntülemek ve güncelleştirmek için aşağıdaki yordamı kullanın.

1. Azure [portalında](https://portal.azure.com)işlev uygulamanıza göz atın.

1. **Yapılandırılmış Özellikler** **altında, İşlev uygulama ayarlarını**seçin.

    ![İşlev uygulama ayarlarını seçin](./media/functions-view-update-version-portal/add-update-app-setting.png)

1. **İşlev uygulaması ayarları** sekmesinde, **Runtime sürümünü**bulun. Belirli çalışma zamanı sürümüne ve istenen ana sürüme dikkat edin. Aşağıdaki örnekte, sürüm `~2`.

   ![İşlev uygulama ayarlarını seçin](./media/functions-view-update-version-portal/function-app-view-version.png)

1. Fonksiyon uygulamanızı sürüm 1.x çalışma süresine sabitlemek için **Runtime sürümü**altında **~1'i** seçin. Uygulamanızda işlevleriniz olduğunda bu anahtar devre dışı bırakılır.

1. Çalışma zamanı sürümünü değiştirdiğinizde, Genel **Bakış** sekmesine geri dön ve uygulamayı yeniden başlatmak için **Yeniden Başlat'ı** seçin.  İşlev uygulaması sürüm 1.x çalışma zamanında yeniden çalışmaya başlar ve işlevler oluşturduğunuzda sürüm 1.x şablonları kullanılır.