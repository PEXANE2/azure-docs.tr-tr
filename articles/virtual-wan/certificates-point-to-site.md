---
title: Kullanıcı VPN bağlantıları için sertifika oluşturma ve dışa aktarma | Azure Sanal WAN
description: Windows 10 veya Windows Server 2016'da PowerShell'i kullanarak kendi imzalı bir kök sertifikası oluşturun, ortak anahtarı dışa aktarın ve istemci sertifikaları oluşturun.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 03/18/2020
ms.author: cherylmc
ms.openlocfilehash: 0303bac88f34c895a4a680cd5bff0e9d1513d2e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80059946"
---
# <a name="generate-and-export-certificates-for-user-vpn-connections"></a>Kullanıcı VPN bağlantıları için sertifika oluşturma ve dışa aktarma

Kullanıcı VPN (noktadan siteye) bağlantıları kimlik doğrulaması için sertifikaları kullanır. Bu makalede, Windows 10 veya Windows Server 2016'da PowerShell kullanarak kendi imzalı bir kök sertifika oluşturma ve istemci sertifikaları oluşturma biçimi gösterilmektedir.

Bu makaledeki adımları Windows 10 veya Windows Server 2016 çalıştıran bir bilgisayarda gerçekleştirmeniz gerekir. Sertifika oluşturmak için kullandığınız PowerShell cmdlet'ler işletim sisteminin bir parçasıdır ve Windows'un diğer sürümlerinde çalışmaz. Windows 10 veya Windows Server 2016 bilgisayarı yalnızca sertifikaları oluşturmak için gereklidir. Sertifikalar oluşturulduktan sonra bunları yükleyebilir veya desteklenen istemci işletim sistemine yükleyebilirsiniz.

[!INCLUDE [Export public key](../../includes/vpn-gateway-generate-export-certificates-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

[Kullanıcı VPN bağlantısı için Sanal WAN adımlarıyla](virtual-wan-about.md) devam edin
