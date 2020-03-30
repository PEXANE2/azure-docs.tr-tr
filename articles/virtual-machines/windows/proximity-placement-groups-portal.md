---
title: Portalı kullanarak bir yakınlık yerleşim grubu oluşturma
description: Azure portalını kullanarak yakınlık yerleştirme grubu oluşturmayı öğrenin.
services: virtual-machines
author: cynthn
manager: gwallace
ms.service: virtual-machines
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 10/30/2019
ms.author: cynthn
ms.openlocfilehash: 8512d9b701242dc686d49bbe56e8a2059f14ee3d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73180257"
---
# <a name="create-a-proximity-placement-group-using-the-portal"></a>Portalı kullanarak bir yakınlık yerleşim grubu oluşturma

VM'leri mümkün olduğunca yakın hale getirmek ve mümkün olan en düşük gecikmeyi elde etmek için, bunları bir [yakınlık yerleşim grubu](co-location.md#proximity-placement-groups)içinde dağıtmanız gerekir.

Yakınlık yerleşim grubu, Azure hesaplama kaynaklarının fiziksel olarak birbirine yakın olduğundan emin olmak için kullanılan mantıksal bir gruplandırmadır. Yakınlık yerleşimgrupları, düşük gecikme nin bir gereklilik olduğu iş yükleri için yararlıdır.


## <a name="create-the-proximity-placement-group"></a>Yakınlık yerleşim grubunu oluşturma

1. Aramada **yakınlık yerleşim grubu** yazın.
1. Arama sonuçlarındaki **Hizmetler** altında **Yakınlık yerleşim grupları**seçin.
1. Yakınlık **yerleşim grupları** sayfasında **Ekle'yi**seçin.
1. Temel **Bilgiler** sekmesinde, **Proje ayrıntıları**altında doğru aboneliğin seçildiğinden emin olun.
1. **Kaynak grubunda** yeni bir grup oluşturmak için **yeni oluştur'u** seçin veya açılır kaynaktan varolan bir kaynak grubu seçin.
1. **Bölge'de** yakınlık yerleşim grubunun oluşturulmasını istediğiniz konumu seçin.
1. **Yakınlık yerleşiminde grup adı** bir ad yazın ve ardından Gözden Geçir + **oluştur'u**seçin.
1. Doğrulama geçtikten sonra, yakınlık yerleşim için yapım grubu oluşturmak için **Yarat'** ı sürü

    ![Yakınlık yerleşim grubu oluşturma ekran görüntüsü](./media/ppg/ppg.png)


## <a name="create-a-vm"></a>VM oluşturma

1. Portalda bir VM oluştururken **Gelişmiş** sekmesine gidin. 
1. Yakınlık **yerleşim grubu** seçiminde doğru yerleşim grubunu seçin. 

    ![Portalda yeni bir VM oluştururken yakınlık yerleşim grubu bölümünün ekran görüntüsü](./media/ppg/vm-ppg.png)

1. Gerekli diğer tüm seçimleri yapmayı bitirdiğinizde, **Gözden Geçir + oluştur'u**seçin.
1. Doğrulamageçtikten sonra, VM'yi yerleşim grubuna dağıtmak için **Oluştur'u** seçin.




## <a name="next-steps"></a>Sonraki adımlar

Yakınlık yerleşim grupları oluşturmak için [Azure PowerShell'i](proximity-placement-groups.md) de kullanabilirsiniz.

