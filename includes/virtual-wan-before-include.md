---
title: include dosyası
description: include dosyası
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 11/06/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4940bce453729d13ba76071fa59bcf95c7672a24
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2020
ms.locfileid: "94359512"
---
* Azure aboneliğiniz var. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

* Bağlanmak istediğiniz bir sanal ağınız var. Şirket içi ağlarınızın alt ağlarının hiçbirinin, bağlanmak istediğiniz sanal ağlarla çakışmadığından emin olun. Azure portal bir sanal ağ oluşturmak için [hızlı başlangıç](../articles/virtual-network/quick-create-portal.md) makalesine bakın.

* Sanal ağınızda mevcut bir sanal ağ geçidi olmaması gerekir. Sanal ağınızda ağ geçitleri zaten varsa (VPN veya ExpressRoute), devam etmeden önce tüm ağ geçitlerini kaldırmanız gerekir. Bu yapılandırma, sanal ağların yalnızca sanal WAN hub ağ geçidine bağlanmasını gerektirir.

* Sanal hub, sanal WAN tarafından oluşturulan ve kullanılan sanal bir ağ. Bu, sanal WAN ağınızın bir bölgedeki temel sayısıdır. Sanal hub bölgeniz için bir IP adresi aralığı alın. Hub için belirttiğiniz adres aralığı, bağlandığınız mevcut sanal ağlardan hiçbiriyle çakışamaz. Ayrıca, bağlandığınız şirket içi adres aralıklarıyla de çakışamaz. Şirket içi ağ yapılandırmanızda bulunan IP adresi aralıklarını tanımıyorsanız, sizin için bu ayrıntıları sağlayabilecek biriyle koordine edebilirsiniz.
