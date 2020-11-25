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
ms.openlocfilehash: f02fa49b62a2e3d617617a20518810209d3879b7
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96026360"
---
Aşağıdaki adımlar için aşağıdaki yapılandırma kullanılmıştır:

- Bilgisayar: Ubuntu Server 18,04
- Bağımlılıklar: strongSwan


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

[Azure CLı 'yı yüklemeye ilişkin ek yönergeler](/cli/azure/install-azure-cli-apt?view=azure-cli-latest)