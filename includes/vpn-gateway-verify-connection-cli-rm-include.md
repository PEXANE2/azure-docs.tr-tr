---
title: dosya dahil etme
description: dosya dahil etme
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 0e009354e66ab13cdb9fbc3cf9e4b37e904bdfd1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67188201"
---
[az network vpn-connection show](/cli/azure/network/vpn-connection) komutunu kullanarak bağlantınızın başarılı olup olmadığını doğrulayabilirsiniz. Örnekte bulunan "-name", test etmek istediğiniz bağlantının adıdır. Bağlantı henüz kurulma aşamasındayken, bağlantı durumu "Bağlanıyor" olarak gösterilir. Bağlantı kurulduktan sonra durum "Bağlandı" olarak değişir.

```azurecli
az network vpn-connection show --name VNet1toSite2 --resource-group TestRG1
```
