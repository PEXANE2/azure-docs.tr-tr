---
title: Sanal ağ iş sürekliliği | Microsoft Dokümanlar
description: Azure Sanal Ağları etkileyen bir Azure hizmetikesintisi durumunda ne yapmanız gerektiğini öğrenin.
services: virtual-network
documentationcenter: ''
author: NarayanAnnamalai
manager: jefco
editor: ''
ms.assetid: ad260ab9-d873-43b3-8896-f9a1db9858a5
ms.service: virtual-network
ms.workload: virtual-network
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2016
ms.author: narayan
ms.reviewer: aglick
ms.openlocfilehash: 3f91d24bff0bec540ff0e7964f21c2f47c03638c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67876182"
---
# <a name="virtual-network--business-continuity"></a>Sanal Ağ – İş Sürekliliği

## <a name="overview"></a>Genel Bakış
Sanal Ağ (VNet), ağınızın buluttaki mantıksal bir temsilidir. Kendi özel IP adresi alanınızı tanımlamanızı ve ağı alt ağlara bölmenizi sağlar. VNet'ler, Azure Sanal Makineleri ve Bulut Hizmetleri (web/çalışan rolleri) gibi bilgi işlem kaynaklarınızı barındırmak için bir güven sınırı görevi sunar. VNet, barındırılan kaynaklar arasında doğrudan özel IP iletişimine izin verir. Sanal ağı VPN Ağ Geçidi veya ExpressRoute aracılığıyla şirket içi ağa bağlayabilirsiniz.

Bir bölge kapsamında bir VNet oluşturulur. VNets'i iki farklı bölgede (Örneğin, ABD Doğu ve ABD Batısı) aynı adres alanına *sahip olarak oluşturabilirsiniz,* ancak aynı adres alanına sahip oldukları için bunları birbirine bağlayamadığınız için. 

## <a name="business-continuity"></a>İş Sürekliliği

Uygulamanızın kesintiye uğrayabileceği birkaç farklı yol olabilir. Bir bölge, birden fazla cihaz veya hizmetin arızalanması nedeniyle doğal afet veya kısmi bir felaket nedeniyle tamamen kesilebilir. Bu durumların her birinde VNet hizmeti üzerindeki etkisi farklıdır.

**S: Tüm bölge için bir kesinti meydana gelirse, ne yapmalıyım? Örneğin, bir bölge doğal afet nedeniyle tamamen kesilirse? Bölgede barındırılan sanal ağlara ne olur?**

C: Sanal ağa ve etkilenen bölgedeki kaynaklara hizmet kesintisi sırasında erişilemez kalır.

![Basit Sanal Ağ Diyagramı](./media/virtual-network-disaster-recovery-guidance/vnet.png)

**S: Aynı sanal ağı farklı bir bölgede yeniden oluşturmam için ne yapabilirim?**

C: Sanal ağlar oldukça hafif kaynaklardır. Farklı bir bölgede aynı adres alanına sahip bir VNet oluşturmak için Azure API'larını çağırabilirsiniz. Etkilenen bölgede bulunan aynı ortamı yeniden oluşturmak için, Bulut Hizmetleri web'ini ve çalışan rollerini ve sahip olduğunuz sanal makineleri yeniden dağıtmak için API çağrıları yaparsınız. Karma dağıtım gibi şirket içi bağlantınız varsa, yeni bir VPN Ağ Geçidi dağıtmanız ve şirket içi ağınıza bağlanmanız gerekir.

Sanal ağ oluşturmak için [bkz.](manage-virtual-network.md#create-a-virtual-network)

**S: Belirli bir bölgedeki VNet'in bir kopyası önceden başka bir bölgede yeniden oluşturulabilir mi?**

C: Evet, aynı özel IP adresi alanını ve kaynaklarını iki farklı bölgede önceden kullanarak iki VNet oluşturabilirsiniz. VNet'te internete bakan hizmetlerbarındırıyorsanız, trafiği etkin olan bölgeye coğrafi yönlendirme yapmak için Trafik Yöneticisi'ni ayarlamış olabilirsiniz. Ancak, yönlendirme sorunlarına neden olacağından, şirket içi ağınıza aynı adres alanına sahip iki VNet bağlayamazsınız. Bir bölgedeki bir felaket ve bir VNet kaybı sırasında, kullanılabilir bölgedeki diğer VNet'i şirket içi ağınıza eşleşen adres alanıyla bağlayabilirsiniz.

Sanal ağ oluşturmak için [bkz.](manage-virtual-network.md#create-a-virtual-network)

