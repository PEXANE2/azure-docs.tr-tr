---
title: dosya dahil etme
description: dosya dahil etme
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 08/14/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: ddab6927044ce638e50ff3ad79aa3c35d046c820
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "69520891"
---
Aşağıdaki adımlar için aşağıdaki yapılandırma kullanılmıştır:

  | | |
  |---|---|
  |Bilgisayar| Ubuntu Server 18.04|
  |Bağımlılıklar| Strongswan istemcisi ile |


Gerekli Strongswa yapılandırmasını yüklemek için aşağıdaki komutları kullanın:

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

[Azure CLı 'yı yüklemeye ilişkin ek yönergeler](https://docs.microsoft.com/cli/azure/install-azure-cli-apt?view=azure-cli-latest)
