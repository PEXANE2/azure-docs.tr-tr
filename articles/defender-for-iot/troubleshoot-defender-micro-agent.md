---
title: Defender IoT mikro aracı sorunlarını giderme (Önizleme)
titleSuffix: Azure Defender for IoT
description: Beklenmeyen veya açıklanamayan hataları nasıl işleyeceğinizi öğrenin.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 1/24/2021
ms.topic: reference
ms.service: azure
ms.openlocfilehash: 07198a5d0ef5d0a6c9eed97523c61826e451b7f5
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102124185"
---
# <a name="defender-iot-micro-agent-troubleshooting-preview"></a>Defender IoT mikro aracı sorunlarını giderme (Önizleme)

Beklenmedik veya açıklanamayan hatalardan oluşan olayda, sorunlarınızı çözmeye çalışmak için aşağıdaki sorun giderme yöntemlerini kullanın. Gerektiğinde yardım için IoT ürün ekibine yönelik Azure Defender 'a da ulaşabilirsiniz.   

## <a name="service-status"></a>Hizmet durumu 

Hizmetin durumunu görüntülemek için: 

1. Aşağıdaki komutu çalıştırın

    ```azurecli
    systemctl status defender-iot-micro-agent.service 
    ```

1. Hizmetin kararlı olduğundan ve işlemdeki çalışma süresinin uygun olduğundan emin olun `active` .

    :::image type="content" source="media/troubleshooting/active-running.png" alt-text="Etkin olduğunu ve çalışma süresinin uygun olup olmadığını denetleyerek hizmetinizin kararlı olduğundan emin olun.":::

Hizmet olarak listeleniyorsa `inactive` , hizmeti başlatmak için aşağıdaki komutu kullanın:

```azurecli
systemctl start defender-iot-micro-agent.service 
```

İşlemin çalışma süresi çok kısaysa hizmetin kilitlenmesini anlarsınız. Bu sorunu çözmek için günlükleri gözden geçirmeniz gerekir.

## <a name="review-logs"></a>Günlükleri gözden geçirme 

Defender IoT mikro Aracı hizmetinin kök ayrıcalıklarla çalıştığını doğrulamak için aşağıdaki komutu kullanın.

```azurecli
ps -aux | grep " defender-iot-micro-agent"
```

:::image type="content" source="media/troubleshooting/root-privileges.png" alt-text="IoT mikro aracı hizmeti için Defender 'ın kök ayrıcalıklarla çalıştığını doğrulayın.":::

Günlükleri görüntülemek için aşağıdaki komutu kullanın:  

```azurecli
sudo journalctl -u defender-iot-micro-agent | tail -n 200 
```

Hizmeti yeniden başlatmak için aşağıdaki komutu kullanın: 

```azurecli
sudo systemctl restart defender-iot-micro-agent  
```

## <a name="next-steps"></a>Sonraki adımlar

[Özellik desteği ve emekliliğinizi](edge-security-module-deprecation.md)inceleyin.
