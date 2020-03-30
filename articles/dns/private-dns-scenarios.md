---
title: Özel Bölgeler için Senaryolar - Azure DNS
description: Bu makalede, Azure DNS Özel Bölgeleri'ni kullanmak için sık karşılaşılan senaryolar hakkında bilgi edinin.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: article
ms.date: 10/05/2019
ms.author: rohink
ms.openlocfilehash: ab850adb2e9a25778d5f44ba711eb0762fe562c8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76939333"
---
# <a name="azure-dns-private-zones-scenarios"></a>Azure DNS Özel bölgeler senaryoları

Azure DNS Özel Bölgeler, sanal ağ içinde ve sanal ağlar arasında ad çözümlemesi sağlar. Bu makalede, bu özelliği kullanarak gerçekleştirilebilecek bazı yaygın senaryolara bakıyoruz.

## <a name="scenario-name-resolution-scoped-to-a-single-virtual-network"></a>Senaryo: Tek bir sanal ağa yönelik Ad Çözümlemesi
Bu senaryoda, Azure'da sanal makineler (VM' ler) dahil olmak üzere bir dizi Azure kaynağı bulunan bir sanal ağınız vardır. Belirli bir etki alanı adı (DNS bölgesi) aracılığıyla sanal ağ içinden kaynakları çözmek istiyorsunuz ve ad çözünürlüğünün özel olması ve internetten erişilememesi gerekir. Ayrıca, VNET içindeki VM'ler için Azure'un bunları otomatik olarak DNS bölgesine kaydetmesi gerekir. 

Bu senaryo aşağıda gösterilmiştir. "A" adlı Sanal Ağ iki VM (VNETA-VM1 ve VNETA-VM2) içerir. Bunların her birinde özel IP'ler ilişkilidir. contoso.com adında bir Özel Bölge oluşturduktan ve bu sanal ağı Kayıt sanal ağı olarak bağladıktan sonra, Azure DNS bölgede görüntülenmişolarak otomatik olarak iki A kaydı oluşturur. Şimdi, VNETA-VM2.contoso.com çözmek için VNETA-VM1'den Gelen DNS sorguları, VNETA-VM2'nin Özel IP'sini içeren bir DNS yanıtı alır. Ayrıca, VNETA-VM2'den verilen VNETA-VM1'in (10.0.0.1) Özel IP'si için bir Ters DNS sorgusu (PTR), beklendiği gibi VNETA-VM1 adını içeren bir DNS yanıtı alır. 

![Tek Sanal ağ çözünürlüğü](./media/private-dns-scenarios/single-vnet-resolution.png)

## <a name="scenario-name-resolution-across-virtual-networks"></a>Senaryo: Sanal ağlar arasında Ad Çözümü

Bu senaryo, bir Özel Bölge'yi birden çok sanal ağla ilişkilendirmeniz gereken daha yaygın bir durumdur. Bu senaryo, birden çok diğer Spoke sanal ağlarıbağlı merkezi bir Hub sanal ağ olduğu Hub ve-Spoke modeli gibi mimarileri sığdırabilir. Merkezi Hub sanal ağı, kayıt sanal ağı olarak özel bir bölgeye bağlanabilir ve Spoke sanal ağları Çözünürlük sanal ağları olarak bağlanabilir. 

Aşağıdaki diyagram, bu senaryonun yalnızca iki sanal anın -A ve B- bulunduğu basit bir sürümünü gösterir. A, Kayıt sanal ağı olarak belirlenir ve B çözünürlük sanal ağı olarak belirlenir. Amaç, her iki sanal anın da ortak bir contoso.com paylaşmasıdır. Bölge oluşturulduğunda ve Çözünürlük ve Kayıt sanal ağları bölgeye bağlandığında, Azure sanal ağ A'dan VM'ler (VNETA-VM1 ve VNETA-VM2) için DNS kayıtlarını otomatik olarak kaydeder. Ayrıca, B Çözünürlük sanal ağındaki VM'ler için bölgeye DNS kayıtlarını el ile ekleyebilirsiniz. Bu kurulumla, dns sorgularını ileri ve geri almak için aşağıdaki davranışı gözlemleyeceksiniz:
* VNETA-VM1.contoso.com için B Çözünürlük sanal ağında VNETB-VM1'den bir DNS sorgusu, VNETA-VM1'in Özel IP'sini içeren bir DNS yanıtı alır.
* 10.1.0.1 çözünürlük sanal ağ B VNETB-VM2 bir Ters DNS (PTR) sorgusu, FQDN VNETB-VM1.contoso.com içeren bir DNS yanıtı alırsınız.  
* 10.0.0.1 için VNETB-VM3 çözünürlük sanal ağ B bir Ters DNS (PTR) sorgusu, NXDOMAIN alacaksınız. Bunun nedeni, Ters DNS sorgularının yalnızca aynı sanal ağa kapsamı nın bulunmasıdır. 


![Birden çok Sanal ağ çözünürlüğü](./media/private-dns-scenarios/multi-vnet-resolution.png)

## <a name="scenario-split-horizon-functionality"></a>Senaryo: Split-Horizon işlevselliği

Bu senaryoda, istemcinin aynı DNS bölgesi için nerede oturduğuna (Azure'un içinde veya internette dışında) bağlı olarak farklı DNS çözümleme davranışını gerçekleştirmek istediğiniz bir kullanım örneğiniz vardır. Örneğin, uygulamanızın farklı işlevselliği veya davranışı olan özel ve herkese açık bir sürümü olabilir, ancak her iki sürüm için de aynı etki alanı adını kullanmak istiyorsunuz. Bu senaryo, aynı ada sahip bir Özel Bölge nin yanı sıra ortak bir DNS bölgesi oluşturarak Azure DNS ile gerçekleştirilebilir.

Aşağıdaki diyagramda bu senaryo gösterilmiştir. Hem Özel IP'ler hem de Genel IP'ler ayrılmış iki VM'si (VNETA-VM1 ve VNETA-VM2) olan bir sanal ağınız vardır. contoso.com adlı bir Genel DNS bölgesi oluşturur sunuz ve bu VM'ler için Ortak IP'leri bölge içinde DNS kayıtları olarak kaydettirin. Ayrıca, Kayıt sanal ağı olarak A'yı belirten contoso.com olarak da adlandırılan özel bir DNS bölgesi oluşturursunuz. Azure, Özel IP'lerini işaret ederek VM'leri otomatik olarak Özel Bölge'ye A kaydı olarak kaydeder.

Şimdi, bir internet istemcisi VNETA-VM1.contoso.com aramak için bir DNS sorgusu verdiğinde, Azure ortak bölgeden Genel IP kaydını döndürecektir. Aynı DNS sorgusu aynı sanal ağ A'daki başka bir VM'den (örneğin: VNETA-VM2) verilirse, Azure Özel IP kaydını özel bölgeden döndürecektir. 

![Brian çözünürlüğünü böl](./media/private-dns-scenarios/split-brain-resolution.png)

## <a name="next-steps"></a>Sonraki adımlar
Özel DNS bölgeleri hakkında daha fazla bilgi için bkz. [Özel etki alanları için Azure DNS'i kullanma](private-dns-overview.md).

Azure [DNS'de özel bir DNS bölgesi oluşturmayı](./private-dns-getstarted-powershell.md) öğrenin.

Ziyaret ederek DNS bölgeleri ve kayıtları hakkında bilgi edinin: [DNS bölgeleri ve kayıtlara genel bakış.](dns-zones-records.md)

Azure'un diğer önemli [ağ özelliklerinden](../networking/networking-overview.md) bazıları hakkında bilgi edinin.

