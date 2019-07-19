---
title: Linux Azure VM 'lerini birlikte bulun | Microsoft Docs
description: Azure VM kaynakları ile gecikme süresini nasıl iyileştirebileceğinizi öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/28/2019
ms.author: cynthn
ms.openlocfilehash: d6d8986117bd2899ea0de0aa6490954aef6c3a56
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67850346"
---
# <a name="co-locate-resources-for-improved-latency"></a>İyileştirilmiş gecikme süresine yönelik kaynakları birlikte bulun

Uygulamanızı Azure 'da dağıttığınızda, örnekleri bölgeler veya kullanılabilirlik alanları arasında yaymak, uygulamanızın genel performansını etkileyebilecek ağ gecikmesi oluşturur. 

## <a name="preview-proximity-placement-groups"></a>Önizleme: Yakınlık yerleştirme grupları

[!INCLUDE [virtual-machines-common-ppg-overview](../../../includes/virtual-machines-common-ppg-overview.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure CLı kullanarak bir [yakınlık yerleşimi grubuna](proximity-placement-groups.md) VM dağıtın.

