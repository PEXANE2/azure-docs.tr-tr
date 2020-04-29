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
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "67189020"
---
Bir işlev uygulaması tarafından şu anda kullanılan çalışma zamanı sürümünü görüntülemek ve güncelleştirmek için aşağıdaki yordamı kullanın.

1. [Azure Portal](https://portal.azure.com), işlev uygulamanıza gidin.

1. **Yapılandırılan Özellikler**altında **işlev uygulama ayarları**' nı seçin.

    ![İşlev uygulaması ayarlarını seçin](./media/functions-view-update-version-portal/add-update-app-setting.png)

1. **İşlev uygulaması ayarları** sekmesinde, **çalışma zamanı sürümünü**bulun. Belirli çalışma zamanı sürümüne ve istenen ana sürüme göz önünde edin. Aşağıdaki örnekte, sürümü olarak `~2`ayarlanır.

   ![İşlev uygulaması ayarlarını seçin](./media/functions-view-update-version-portal/function-app-view-version.png)

1. İşlev uygulamanızı sürüm 1. x çalışma zamanına sabitlemek için, **çalışma zamanı sürümü**altında **~ 1** seçeneğini belirleyin. Uygulamanızda işlevleriniz olduğunda bu anahtar devre dışıdır.

1. Çalışma zamanı sürümünü değiştirdiğinizde, **genel bakış** sekmesine dönün ve uygulamayı yeniden başlatmak Için **Yeniden Başlat** ' ı seçin.  Sürüm 1. x çalışma zamanı üzerinde çalışan işlev uygulaması yeniden başlatılır ve işlev oluştururken sürüm 1. x şablonları kullanılır.