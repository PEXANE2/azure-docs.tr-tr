---
title: 'Azure VPN Gateway: Noktadan siteye istemci sertifikası yüklemeyi'
description: P2S sertifikası kimlik doğrulaması için istemci sertifikası 'nı (Windows, Mac, Linux) yükler.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/02/2020
ms.author: cherylmc
ms.openlocfilehash: 8c4d37f67d89ad1b7973a85fb6c2a9f7e818a028
ms.sourcegitcommit: 9c262672c388440810464bb7f8bcc9a5c48fa326
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89421671"
---
# <a name="install-client-certificates-for-p2s-certificate-authentication-connections"></a>P2S sertifikası kimlik doğrulaması bağlantıları için istemci sertifikaları 'nı yükler

Noktadan siteye Azure sertifikası kimlik doğrulaması kullanarak bir sanal ağa bağlanan tüm istemciler istemci sertifikası gerektirir. Bu makale, P2S kullanarak bir VNet 'e bağlanılırken kimlik doğrulama için kullanılan bir istemci sertifikası yüklemenize yardımcı olur.

## <a name="acquire-a-client-certificate"></a><a name="generate"></a>İstemci sertifikası alma

Hangi istemci işletim sistemini bağlamak istediğinize bakılmaksızın, her zaman bir istemci sertifikanız olmalıdır. Bir kuruluş CA 'sı çözümü veya otomatik olarak imzalanan kök sertifika kullanılarak oluşturulan bir kök sertifikadan bir istemci sertifikası oluşturabilirsiniz. İstemci sertifikası oluşturma adımları için [PowerShell](vpn-gateway-certificates-point-to-site.md), [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md)veya [Linux](vpn-gateway-certificates-point-to-site-linux.md) yönergelerine bakın. 

## <a name="windows"></a><a name="installwin"></a>Windows

[!INCLUDE [Install on Windows](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="mac"></a><a name="installmac"></a>Mac

>[!NOTE]
>Mac VPN istemcileri yalnızca Kaynak Yöneticisi dağıtım modeli için desteklenir. Klasik dağıtım modeli için desteklenmez.
>
>

[!INCLUDE [Install on Mac](../../includes/vpn-gateway-certificates-install-mac-client-cert-include.md)]

## <a name="linux"></a><a name="installlinux"></a>Linux

Linux istemci sertifikası istemciye istemci yapılandırmasının bir parçası olarak yüklenir. Yönergeler için bkz. [istemci yapılandırması-Linux](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli) .

## <a name="next-steps"></a>Sonraki adımlar

[VPN istemci yapılandırma dosyalarını oluşturmak ve yüklemek](point-to-site-vpn-client-configuration-azure-cert.md)Için Noktadan siteye yapılandırma adımlarıyla devam edin.
