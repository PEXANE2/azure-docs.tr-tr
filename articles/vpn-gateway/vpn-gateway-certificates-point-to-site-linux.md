---
title: 'Noktadan siteye sertifikaları oluşturma ve dışarı aktarma: Linux: CLı'
description: Otomatik olarak imzalanan bir kök sertifika oluşturun, ortak anahtarı dışarı aktarın ve Linux (Strongswa) CLı kullanarak istemci sertifikaları oluşturun.
titleSuffix: Azure VPN Gateway
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 08/14/2019
ms.author: alzam
ms.openlocfilehash: 89f6014b548bd3dd66622d15149051e6b28e94b0
ms.sourcegitcommit: 55b2bbbd47809b98c50709256885998af8b7d0c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84984602"
---
# <a name="generate-and-export-certificates"></a>Sertifikaları oluşturma ve dışarı aktarma

Noktadan siteye bağlantılar kimlik doğrulaması için sertifikaları kullanır. Bu makalede, otomatik olarak imzalanan bir kök sertifika oluşturma ve Linux CLı ve Strongswa kullanarak istemci sertifikaları oluşturma işlemlerinin nasıl yapılacağı gösterilir. Farklı sertifika yönergeleri arıyorsanız, [PowerShell](vpn-gateway-certificates-point-to-site.md) veya [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) makalelerine bakın. CLı yerine GUI 'yi kullanarak Strongswa 'nın nasıl yükleneceği hakkında bilgi için [istemci yapılandırma](point-to-site-vpn-client-configuration-azure-cert.md#install) makalesindeki adımlara bakın.

## <a name="install-strongswan"></a>Strongswa 'yi yükler

[!INCLUDE [strongSwan Install](../../includes/vpn-gateway-strongswan-install-include.md)]

## <a name="generate-and-export-certificates"></a>Sertifikaları oluşturma ve dışarı aktarma

[!INCLUDE [strongSwan certificates](../../includes/vpn-gateway-strongswan-certificates-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

[VPN istemci yapılandırma dosyalarını oluşturmak ve yüklemek](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli)Için Noktadan siteye yapılandırmanızla devam edin.
