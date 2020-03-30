---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 08/14/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: ddab6927044ce638e50ff3ad79aa3c35d046c820
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "69520891"
---
Aşağıdaki adımlar için aşağıdaki yapılandırma kullanılmıştır:

  | | |
  |---|---|
  |Bilgisayar| Ubuntu Server 18.04|
  |Bağımlılıklar| güçlü Kuğu |


Gerekli strongSwan yapılandırmasını yüklemek için aşağıdaki komutları kullanın:

```
sudo apt install strongswan
```

```
sudo apt install strongswan-pki
```

```
sudo apt install libstrongswan-extra-plugins
```

Azure komut satırı arabirimini yüklemek için aşağıdaki komutu kullanın:

```
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

[Azure CLI'nin nasıl yüklenirek ek yönergeleri](https://docs.microsoft.com/cli/azure/install-azure-cli-apt?view=azure-cli-latest)
