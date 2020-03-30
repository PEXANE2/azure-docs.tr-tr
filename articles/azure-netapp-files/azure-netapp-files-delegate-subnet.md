---
title: Azure NetApp Dosyalarına bir alt ağ atama | Microsoft Dokümanlar
description: Bir alt ağı Azure NetApp Dosyalarına nasıl atanın açıklar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: b-juche
ms.openlocfilehash: b83f530549ffa43789963fd0c95b4982f5289356
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80054459"
---
# <a name="delegate-a-subnet-to-azure-netapp-files"></a>Azure NetApp Files için bir alt ağı temsilci olarak belirleme 

Azure NetApp Files'a bir alt ağ aktarmanız gerekir.   Bir birim oluşturduğunuzda, devredilen alt ağı belirtmeniz gerekir.

## <a name="considerations"></a>Dikkat edilmesi gerekenler
* Yeni bir alt ağ oluşturma sihirbazı, 251 kullanılabilir IP adresi sağlayan /24 ağ maskesine varsayılan olarak verilir. 16 kullanılabilir IP adresi sağlayan bir /28 ağ maskesi kullanmak hizmet için yeterlidir.
* Her Azure Sanal Ağında (VNet), Azure NetApp Dosyalarına yalnızca bir alt ağ devredilebilir.   
   Azure, bir VNet'te birden çok devralınan alt ağ oluşturmanıza olanak tanır.  Ancak, birden fazla temsilci alt ağ kullanırsanız, yeni bir birim oluşturma girişimleri başarısız olur.
* Devredilen alt ağda bir ağ güvenlik grubu veya hizmet bitiş noktası atayamazsınız. Bunu yapmak subnet delegasyonunun başarısız olmasını neden olur.
* Genel olarak bakan bir sanal ağdan bir birisesine erişim şu anda desteklenmez.
* Azure NetApp Files'a devredilen bir alt ağa adres öneki (hedef) ile VM alt ağlarında [kullanıcı tanımlı özel rotalar](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#custom-routes) oluşturmak desteklenmez. Bunu yapmak VM bağlantısını etkileyecektir.

## <a name="steps"></a>Adımlar 
1.  Azure portalından **Sanal ağlar** blade'ine gidin ve Azure NetApp Dosyaları için kullanmak istediğiniz sanal ağı seçin.    

1. Sanal ağ bıçağından **Alt Ağlar'ı** seçin ve **+Subnet** düğmesini tıklatın. 

1. Alt Net Ekle sayfasında aşağıdaki gerekli alanları tamamlayarak Azure NetApp Dosyaları için kullanılacak yeni bir alt ağ oluşturun:
    * **Adı**: Alt net adını belirtin.
    * **Adres aralığı**: IP adres aralığını belirtin.
    * **Subnet delegasyonu**: **Microsoft.NetApp/volumes'u**seçin. 

      ![Alt ağ temsilcisi](../media/azure-netapp-files/azure-netapp-files-subnet-delegation.png)
    
[Azure NetApp Dosyaları için bir birim oluşturduğunuzda](azure-netapp-files-create-volumes.md)bir alt ağ oluşturabilir ve temsilci lik de atayabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar  
* [Azure NetApp Files için birim oluşturma](azure-netapp-files-create-volumes.md)
* [Azure hizmetleri için sanal ağ tümleştirmesi hakkında bilgi edinin](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)


