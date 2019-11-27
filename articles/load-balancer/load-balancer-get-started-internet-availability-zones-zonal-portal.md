---
title: Load Balancer bir ön uç Azure portal oluşturun
titleSuffix: Azure Load Balancer
description: Azure Portal ile bölgesel ön ucu ile standart Load Balancer oluşturma hakkında bilgi edinin
services: load-balancer
documentationcenter: na
author: asudbring
manager: twooley
ms.service: load-balancer
ms.custom: seodec18
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/2018
ms.author: allensu
ms.openlocfilehash: 82a773b279780bc4eb784fa107d6b15bd0ff2672
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74225331"
---
#  <a name="create-a-standard-load-balancer-with-zonal-frontend-using-azure-portal"></a>Azure Portal kullanarak bölgesel ön ucu ile standart Load Balancer oluşturma

Bu makalede, bir özel ön uç IP yapılandırması ile genel [Standart Load Balancer](https://aka.ms/azureloadbalancerstandard) oluşturma adımları sağlanır. Kullanılabilirlik bölgelerinin Standart Load Balancer nasıl çalıştığını anlamak için, bkz. [Standart Load Balancer ve kullanılabilirlik alanları](load-balancer-standard-availability-zones.md). 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

> [!NOTE]
> Kullanılabilirlik bölgeleri, seçili Azure kaynakları ve bölgeler ve sanal makine boyutu aileleri için kullanılabilir. Kullanmaya başlama hakkında daha fazla bilgi edinmek ve kullanılabilirlik alanlarını ile hangi Azure kaynakları, bölgeleri ve VM boyut ailelerini deneyebileceğinizi öğrenmek için bkz. [kullanılabilirlik alanları genel bakış](https://docs.microsoft.com/azure/availability-zones/az-overview). Destek için [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) üzerinden bize ulaşabilir veya [bir Azure destek bileti açabilirsiniz](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-load-balancer-with-zonal-frontend-ip-address"></a>Bölgesel bir ön uç IP adresine sahip yük dengeleyici oluşturma

1. Bir tarayıcıdan Azure portal gidin: [https://portal.azure.com](https://portal.azure.com) ve Azure hesabınızla oturum açın.
2. Ekranın sol üst kısmında, **ağ** > Load Balancer > **kaynak oluştur** ' u seçin **.**
3. **Yük dengeleyici oluştur** sayfasında, **ad** türü **myloadbalancer**altında.
4. **Tür** bölümünde **Genel**’i seçin.
5. SKU altında **Standart**' ı seçin.
6. **Genel BIR IP adresi Seç**' e tıklayın, **Yeni oluştur**' a tıklayın ve **genel IP adresi oluştur** sayfasında, ad ' ın altında **mypublicipzonal**yazın, SKU için **Standart**, kullanılabilirlik bölgesi için **1**' i seçin.
    
>[!NOTE] 
> Genel IP Bu adımda oluşturduğunuz standart SKU'nun varsayılan olarak açıktır.

1. **Kaynak grubu**Için **Yeni oluştur**' a tıklayın ve ardından kaynak grubunun adı olarak **myresourcegroupzlb** yazın.
1. **Konum**için **Batı Avrupa**öğesini seçin ve ardından **Tamam**' a tıklayın. Yük dengeleyici dağıtımı başlar ve birkaç dakika içinde başarıyla tamamlanır.

    ![Azure portal ile bölge yedekli Standart Load Balancer oluşturma](./media/load-balancer-get-started-internet-availability-zones-zonal-portal/load-balancer-zonal-frontend.png)


## <a name="next-steps"></a>Sonraki adımlar
- [Standart Load Balancer ve kullanılabilirlik bölgeleri](load-balancer-standard-availability-zones.md)hakkında daha fazla bilgi edinin.



