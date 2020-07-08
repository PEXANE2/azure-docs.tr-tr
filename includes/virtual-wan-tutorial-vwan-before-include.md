---
title: dosya dahil etme
description: dosya dahil etme
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 09/12/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 945d701a2a7dffc259c601b4dab9fa1333ccc066
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74896597"
---
Yapılandırmanıza başlamadan önce aşağıdaki ölçütleri karşıladığınızdan emin olun:

* Bağlanmak istediğiniz sanal ağınız zaten varsa, şirket içi ağınızın alt ağlarının hiçbirinin onunla çakışmadığından emin olun. Sanal ağınız için bir ağ geçidi alt ağı gerekli değildir ve sanal ağ geçitleri olamaz. Bir sanal ağınız yoksa, bu makaledeki adımları kullanarak bir tane oluşturabilirsiniz.
* Hub bölgenizden bir IP adresi aralığı edinin. Hub bir sanal ağ ve Merkez bölgesi için belirttiğiniz adres aralığı, bağlandığınız mevcut bir sanal ağ ile çakışamaz. Ayrıca, şirket içine bağlandığınız adres aralıklarıyla de çakışamaz. Şirket içi ağ yapılandırmanızda bulunan IP adresi aralıklarını bilmiyorsanız, sizin için bu ayrıntıları sağlayabilecek biriyle koordine edin.
* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.