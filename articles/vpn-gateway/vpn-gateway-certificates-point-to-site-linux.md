---
title: 'Noktadan siteye sertifikaları oluşturma ve dışarı aktarma: Linux: CLı'
description: Otomatik olarak imzalanan bir kök sertifika oluşturun, ortak anahtarı dışarı aktarın ve Linux (Strongswa) CLı kullanarak istemci sertifikaları oluşturun.
titleSuffix: Azure VPN Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/02/2020
ms.author: alzam
ms.openlocfilehash: 0be5bb042649b0fe425077b5b3feb3cea728218c
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89394015"
---
# <a name="generate-and-export-certificates"></a>Sertifikaları oluşturma ve dışarı aktarma

Noktadan siteye bağlantılar kimlik doğrulaması için sertifikaları kullanır. Bu makalede, otomatik olarak imzalanan bir kök sertifika oluşturma ve Linux CLı ve Strongswa kullanarak istemci sertifikaları oluşturma işlemlerinin nasıl yapılacağı gösterilir. Farklı sertifika yönergeleri arıyorsanız, [PowerShell](vpn-gateway-certificates-point-to-site.md) veya [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) makalelerine bakın. CLı yerine GUI 'yi kullanarak Strongswa 'nın nasıl yükleneceği hakkında bilgi için [istemci yapılandırma](point-to-site-vpn-client-configuration-azure-cert.md#install) makalesindeki adımlara bakın.

## <a name="install-strongswan"></a>Strongswa 'yi yükler

[!INCLUDE [strongSwan Install](../../includes/vpn-gateway-strongswan-install-include.md)]

## <a name="generate-and-export-certificates"></a>Sertifikaları oluşturma ve dışarı aktarma

[!INCLUDE [strongSwan certificates](../../includes/vpn-gateway-strongswan-certificates-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

[VPN istemci yapılandırma dosyalarını oluşturmak ve yüklemek](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli)Için Noktadan siteye yapılandırmanızla devam edin.
