---
title: 'P2S için sertifika oluşturma ve verme: PowerShell'
titleSuffix: Azure VPN Gateway
description: Windows 10 veya Windows Server 2016'da PowerShell'i kullanarak kendi imzalı bir kök sertifikası oluşturun, ortak anahtarı dışa aktarın ve istemci sertifikaları oluşturun.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 10/10/2019
ms.author: cherylmc
ms.openlocfilehash: f28e76e9dcaf1331fd26a2321cd4deca1027e693
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79279344"
---
# <a name="generate-and-export-certificates-for-point-to-site-using-powershell"></a>PowerShell kullanarak Noktaya Sayfa için sertifika lar oluşturma ve dışa aktarma

Noktadan Siteye bağlantılar, kimlik doğrulaması için sertifikaları kullanır. Bu makalede, Windows 10 veya Windows Server 2016'da PowerShell kullanarak kendi imzalı bir kök sertifika oluşturma ve istemci sertifikaları oluşturma biçimi gösterilmektedir. Farklı sertifika yönergeleri arıyorsanız, [bkz.](vpn-gateway-certificates-point-to-site-linux.md) [Certificates - MakeCert](vpn-gateway-certificates-point-to-site-makecert.md)

Bu makaledeki adımları Windows 10 veya Windows Server 2016 çalıştıran bir bilgisayarda gerçekleştirmeniz gerekir. Sertifika oluşturmak için kullandığınız PowerShell cmdlet'ler işletim sisteminin bir parçasıdır ve Windows'un diğer sürümlerinde çalışmaz. Windows 10 veya Windows Server 2016 bilgisayarı yalnızca sertifikaları oluşturmak için gereklidir. Sertifikalar oluşturulduktan sonra bunları yükleyebilir veya desteklenen istemci işletim sistemine yükleyebilirsiniz.

Windows 10 veya Windows Server 2016 bilgisayarına erişiminiz yoksa, sertifika oluşturmak için [MakeCert'i](vpn-gateway-certificates-point-to-site-makecert.md) kullanabilirsiniz. Her iki yöntemi kullanarak oluşturduğunuz sertifikalar [desteklenen](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq) istemci işletim sistemine yüklenebilir.

[!INCLUDE [generate and export certificates](../../includes/vpn-gateway-generate-export-certificates-include.md)]

## <a name="install-an-exported-client-certificate"></a><a name="install"></a>Dışarı aktarılan bir istemci sertifikasını yükleme

P2S bağlantısı üzerinden VNet'e bağlanan her istemci, yerel olarak yüklenmesi için bir istemci sertifikası gerektirir.

İstemci sertifikası yüklemek [için, Siteye Puan](point-to-site-how-to-vpn-client-install-azure-cert.md)bağlantıları için istemci sertifikası yükleyin'e bakın.

## <a name="next-steps"></a>Sonraki adımlar

Noktadan Siteye yapılandırmanıza devam edin.

* **Kaynak Yöneticisi** dağıtım modeli adımları için [bkz.](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
* **Klasik** dağıtım modeli adımları için bkz: [Bir VNet'e (klasik) Bir Noktadan Siteye VPN bağlantısını yapılandırın.](vpn-gateway-howto-point-to-site-classic-azure-portal.md)