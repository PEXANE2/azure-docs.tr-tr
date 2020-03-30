---
title: 'Azure VPN Ağ Geçidi: Bir Noktaya Nokta istemci sertifikası yükleme'
description: Windows, Mac, Linux - P2S sertifika kimlik doğrulaması için istemci sertifika yükleyin.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 01/10/2020
ms.author: cherylmc
ms.openlocfilehash: 787b8a34ed4b232b9e6cc033e67b1a8162c85f6c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75902842"
---
# <a name="install-client-certificates-for-p2s-certificate-authentication-connections"></a>P2S sertifika kimlik doğrulama bağlantıları için istemci sertifikaları yükleme

Site Tabanlı Azure sertifika kimlik doğrulaması kullanarak sanal ağa bağlanan tüm istemcilerin bir istemci sertifikası na ihtiyacı vardır. Bu makale, P2S kullanarak bir VNet'e bağlanırken kimlik doğrulaması için kullanılan bir istemci sertifikası yüklemenize yardımcı olur.

## <a name="acquire-a-client-certificate"></a><a name="generate"></a>İstemci sertifikası edinme

Bağlanmak istediğiniz istemci işletim sistemi ne olursa olsun, her zaman bir istemci sertifikasına sahip olmalısınız. Kurumsal CA çözümü kullanılarak oluşturulan bir kök sertifikadan veya kendi imzalı kök sertifikasından istemci sertifikası oluşturabilirsiniz. Bir istemci sertifikası oluşturmak için adımlar için [PowerShell,](vpn-gateway-certificates-point-to-site.md) [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md)veya [Linux](vpn-gateway-certificates-point-to-site-linux.md) yönergelerine bakın. 

## <a name="windows"></a><a name="installwin"></a>Windows

[!INCLUDE [Install on Windows](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="mac"></a><a name="installmac"></a>Mac

>[!NOTE]
>Mac VPN istemcileri yalnızca Kaynak Yöneticisi dağıtım modeli için desteklenir. Bunlar klasik dağıtım modeli için desteklenmez.
>
>

[!INCLUDE [Install on Mac](../../includes/vpn-gateway-certificates-install-mac-client-cert-include.md)]

## <a name="linux"></a><a name="installlinux"></a>Linux

Linux istemci sertifikası istemci yapılandırmasının bir parçası olarak istemciye yüklenir. Bkz. [Müşteri yapılandırması -](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli) Talimatlar için Linux.

## <a name="next-steps"></a>Sonraki adımlar

[VPN istemci yapılandırma dosyaları oluşturmak ve yüklemek](point-to-site-vpn-client-configuration-azure-cert.md)için Site'ye Nokta yapılandırma adımlarıyla devam edin.
