---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/15/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4c232e1ce183c6935d625b5bc9987a4981865ae4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188251"
---
Kaynak Yöneticisi dağıtım modeliyle çalışıyorsanız, yeni ağ geçidi STU'larına geçebilirsiniz. Eski bir ağ geçidi SKU'dan yeni bir SKU'ya geçiş yaptığınızda, varolan VPN ağ geçidini siler ve yeni bir VPN ağ geçidi oluşturursunuz.

Iş akışı:

1. Sanal ağ geçidine ilişkin tüm bağlantıları kesin.
2. Eski VPN ağ geçidini silin.
3. Yeni VPN ağ geçidini oluşturun.
4. Şirket içi VPN cihazlarınızdaki VPN ağ geçidi IP adresini (Siteden Siteye bağlantılar için) yenisi ile güncelleştirin.
5. Bu ağ geçidine bağlanacak tüm VNet-VNet yerel ağ geçitleri için ağ geçidi IP adresi değerini güncelleştirin.
6. Bu VPN ağ geçidi yoluyla sanal ağa bağlanan P2S istemcileri için yeni istemci VPN yapılandırma paketlerini indirin.
7. Sanal ağ geçidine ilişkin tüm bağlantıları yeniden oluşturun.

Dikkat edilmesi gerekenler:

* Yeni SNU'lara geçmek için VPN ağ geçidiniz Kaynak Yöneticisi dağıtım modelinde olmalıdır.
* Klasik bir VPN ağ geçidiniz varsa, bu ağ geçidi için eski eski SNU'ları kullanmaya devam etmelisiniz, ancak eski SNU'lar arasında yeniden boyutlandırma yapabilirsiniz. Yeni SNU'lara geçemezsiniz.
* Eski bir SKU'dan yeni bir SKU'ya geçiş yaptığınızda bağlantı kapalı kalma süreniz olur.
* Yeni bir ağ geçidi SKU'ya geçerken, VPN ağ geçidinizin genel IP adresi değişecektir. Bu, daha önce kullandığınız aynı genel IP adresi nesnesini belirtseniz bile gerçekleşir.