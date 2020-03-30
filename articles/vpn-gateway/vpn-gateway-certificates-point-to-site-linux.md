---
title: 'Point-to-Site için sertifika oluşturma ve dışa aktarma: Linux: CLI'
description: Kendi imzalı bir kök sertifikası oluşturun, ortak anahtarı dışa aktarın ve Linux (strongSwan) CLI'yi kullanarak istemci sertifikaları oluşturun.
titleSuffix: Azure VPN Gateway
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: article
ms.date: 08/14/2019
ms.author: alzam
ms.openlocfilehash: a0f996ff2805da4dd5af400642eef2506c228d33
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75779765"
---
# <a name="generate-and-export-certificates"></a>Sertifika oluşturma ve dışa aktarma

Noktadan Siteye bağlantılar, kimlik doğrulaması için sertifikaları kullanır. Bu makalede, linux CLI ve strongSwan kullanarak kendi imzalı bir kök sertifika oluşturmak ve istemci sertifikaları oluşturmak nasıl gösterir. Farklı sertifika yönergeleri arıyorsanız [Powershell](vpn-gateway-certificates-point-to-site.md) veya [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) makalelerini görün. CLI yerine GUI'yi kullanarak strongSwan'ı nasıl yükleyeceğiz hakkında bilgi için [İstemci yapılandırma](point-to-site-vpn-client-configuration-azure-cert.md#install) makalesindeki adımlara bakın.

## <a name="install-strongswan"></a>strongSwan'ı yükleyin

[!INCLUDE [strongSwan Install](../../includes/vpn-gateway-strongswan-install-include.md)]

## <a name="generate-and-export-certificates"></a>Sertifika oluşturma ve dışa aktarma

[!INCLUDE [strongSwan certificates](../../includes/vpn-gateway-strongswan-certificates-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

VPN istemci yapılandırma dosyaları oluşturmak [ve yüklemek](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli)için Yer Noktadayapılandırmanıza devam edin.
