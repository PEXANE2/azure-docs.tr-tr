---
title: Kullanıcı VPN bağlantıları için sertifikaları oluşturma ve dışarı aktarma | Azure sanal WAN
description: Windows 10 veya Windows Server 2016 ' de PowerShell 'i kullanarak otomatik olarak imzalanan bir kök sertifika oluşturun, ortak anahtarı dışarı aktarın ve kullanıcı VPN bağlantıları için istemci sertifikaları oluşturun.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 09/22/2020
ms.author: cherylmc
ms.openlocfilehash: 2205f170ee846d4db94db7f524a1c424cfbc8f7b
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "91328047"
---
# <a name="generate-and-export-certificates-for-user-vpn-connections"></a>Kullanıcı VPN bağlantıları için sertifikaları oluşturma ve dışarı aktarma

Kullanıcı VPN (Noktadan siteye) bağlantıları kimlik doğrulaması için sertifikaları kullanır. Bu makalede, otomatik olarak imzalanan bir kök sertifika oluşturma ve Windows 10 veya Windows Server 2016 ' de PowerShell kullanarak istemci sertifikaları oluşturma işlemlerinin nasıl yapılacağı gösterilir.

Bu makaledeki adımları Windows 10 veya Windows Server 2016 çalıştıran bir bilgisayarda gerçekleştirmeniz gerekir. Sertifika oluşturmak için kullandığınız PowerShell cmdlet 'leri işletim sisteminin bir parçasıdır ve Windows 'un diğer sürümlerinde çalışmaz. Windows 10 veya Windows Server 2016 bilgisayarı yalnızca sertifikaları oluşturmak için gereklidir. Sertifikalar oluşturulduktan sonra, bunları karşıya yükleyebilir veya desteklenen herhangi bir istemci işletim sistemine yükleyebilirsiniz.

[!INCLUDE [Export public key](../../includes/vpn-gateway-generate-export-certificates-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

[Kullanıcı VPN bağlantısı Için sanal WAN adımlarıyla](virtual-wan-about.md) devam edin
