---
title: Kullanıcı VPN bağlantıları için sertifikaları oluşturma ve dışarı aktarma | Azure sanal WAN
description: Windows 10 veya Windows Server 2016 ' de otomatik olarak imzalanan bir kök sertifika oluşturun, ortak anahtarı dışarı aktarın ve PowerShell 'i kullanarak istemci sertifikaları oluşturun.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 03/18/2020
ms.author: cherylmc
ms.openlocfilehash: 0303bac88f34c895a4a680cd5bff0e9d1513d2e5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80059946"
---
# <a name="generate-and-export-certificates-for-user-vpn-connections"></a>Kullanıcı VPN bağlantıları için sertifikaları oluşturma ve dışarı aktarma

Kullanıcı VPN (Noktadan siteye) bağlantıları kimlik doğrulaması için sertifikaları kullanır. Bu makalede, otomatik olarak imzalanan bir kök sertifika oluşturma ve Windows 10 veya Windows Server 2016 ' de PowerShell kullanarak istemci sertifikaları oluşturma işlemlerinin nasıl yapılacağı gösterilir.

Bu makaledeki adımları Windows 10 veya Windows Server 2016 çalıştıran bir bilgisayarda gerçekleştirmeniz gerekir. Sertifika oluşturmak için kullandığınız PowerShell cmdlet 'leri işletim sisteminin bir parçasıdır ve Windows 'un diğer sürümlerinde çalışmaz. Windows 10 veya Windows Server 2016 bilgisayarı yalnızca sertifikaları oluşturmak için gereklidir. Sertifikalar oluşturulduktan sonra, bunları karşıya yükleyebilir veya desteklenen herhangi bir istemci işletim sistemine yükleyebilirsiniz.

[!INCLUDE [Export public key](../../includes/vpn-gateway-generate-export-certificates-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

[Kullanıcı VPN bağlantısı Için sanal WAN adımlarıyla](virtual-wan-about.md) devam edin
