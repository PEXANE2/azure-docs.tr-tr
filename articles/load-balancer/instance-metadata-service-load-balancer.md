---
title: Azure Instance Metadata Service kullanarak yük dengeleyici bilgilerini alma
titleSuffix: Azure Load Balancer
description: Yük dengeleyici bilgilerini almak için Azure Instance Metadata Service kullanma hakkında bilgi edinmeye başlayın.
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: conceptual
ms.date: 02/12/2021
ms.author: allensu
ms.openlocfilehash: 0d7d08eb5e77e744fb6ce0abefc550bc79de4c8c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "100519091"
---
# <a name="retrieve-load-balancer-information-by-using-the-azure-instance-metadata-service"></a>Azure Instance Metadata Service kullanarak yük dengeleyici bilgilerini alma

IDS (Azure Instance Metadata Service), çalışmakta olan sanal makine örnekleri hakkında bilgi sağlar. Hizmet, iyi bilinen, yönlendirilemeyen bir IP adresinde (169.254.169.254) kullanılabilen bir REST API. 

Sanal makineyi veya sanal makine kümesi örneklerini bir Azure Standart Load Balancer arkasına yerleştirdiğinizde, yük dengeleyici ve örneklerle ilgili meta verileri almak için ıMDS 'yi kullanın.

Meta veriler, sanal makineler veya sanal makine ölçek kümeleri için aşağıdaki bilgileri içerir:

* Standart SKU genel IP 'si.
* Ağ arabiriminin her bir özel IP 'si için yük dengeleyicinin gelen kural yapılandırması.
* Ağ arabiriminin her özel IP 'si için yük dengeleyicinin giden kuralı yapılandırması.

## <a name="access-the-load-balancer-metadata-using-the-imds"></a>Yük dengeleyici meta verilerine ıMDS kullanarak erişin

Yük dengeleyici meta verilerine erişme hakkında daha fazla bilgi için bkz. [Azure Instance Metadata Service kullanarak yük dengeleyici bilgilerine erişin](howto-load-balancer-imds.md).

## <a name="troubleshoot-common-error-codes"></a>Sık karşılaşılan hata kodlarında sorun giderme

Ortak hata kodları ve bunların risk azaltma yöntemleri hakkında daha fazla bilgi için bkz. [ıMDS kullanırken yaygın hata kodlarında sorun giderme](troubleshoot-load-balancer-imds.md). 

## <a name="support"></a>Destek

Birden çok denemeden sonra meta veri yanıtı alamadıysanız Azure portal bir destek sorunu oluşturun.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Instance Metadata Service](../virtual-machines/windows/instance-metadata-service.md) hakkında daha fazla bilgi

[Standart yük dengeleyici dağıtma](quickstart-load-balancer-standard-public-portal.md)

