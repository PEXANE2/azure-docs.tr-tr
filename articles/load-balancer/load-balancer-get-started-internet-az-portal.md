---
title: Bölgesel olarak yedekli ön uç Azure portal Load Balancer oluşturma
titleSuffix: Azure Load Balancer
description: Azure portalı ile bölgesel olarak yedekli genel IP adresi ön uç ile genel bir Standard Load Balancer oluşturma konusunda bilgi edinin
services: load-balancer
documentationcenter: na
author: asudbring
manager: kumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2018
ms.author: allensu
ms.openlocfilehash: bd85e062b2b45abc32269b94ce678ebf63fb3fc1
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75894446"
---
#  <a name="create-a-standard-load-balancer-with-zone-redundant-frontend-using-azure-portal"></a>Azure portalını kullanarak bölgesel olarak yedekli ön uç ile standart yük dengeleyici oluşturma

Bu makalede, genel bir IP standart adresi kullanılarak bölge yedekli ön uçta ortak [Standart Load Balancer](https://aka.ms/azureloadbalancerstandard) oluşturma adımları sağlanır. Bölgesel olarak yedekli varsayılan olarak standart yük dengeleyici üzerindeki bir tek bir ön uç IP adresi.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

> [!NOTE]
>  Kullanılabilirlik bölgeleri, seçili Azure kaynakları ve bölgeler ve sanal makine boyutu aileleri için kullanılabilir. Kullanmaya başlamak nasıl daha fazla bilgi ve hangi Azure kaynakları, bölgeleri ve kullanılabilirlik alanları ile deneyebilirsiniz sanal makine boyutu aileleri için bkz. [kullanılabilirlik alanlarına genel bakış](https://docs.microsoft.com/azure/availability-zones/az-overview). Destek için [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) üzerinden bize ulaşabilir veya [bir Azure destek bileti açabilirsiniz](../azure-portal/supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-zone-redundant-load-balancer"></a>Bölge yedekli yük dengeleyici oluşturma

1. Bir tarayıcıdan Azure portalına gidin: [ https://portal.azure.com ](https://portal.azure.com) ve Azure hesabınızla oturum açın.
2. Ekranın sol üst tarafında seçin **kaynak Oluştur** > **ağ** > **yük dengeleyici.**
3. İçinde **yük dengeleyici Oluştur** sayfasındaki **adı** türü **myLoadBalancer**.
4. **Tür** bölümünde **Genel**’i seçin.
5. SKU altında seçin **standart**.
6. Tıklayın **genel IP adresi**, tıklayın **Yeni Oluştur**hem de **genel IP adresi oluşturma** sayfasında, adı, türü altında **myPublicIPStandard**.
    >[!NOTE] 
    > Genel IP Bu adımda oluşturduğunuz standart SKU'su ve bölgesel olarak yedekli varsayılan olarak. 
8. Altında **konumu**seçin **Doğu ABD 2**ve ardından **Tamam**. Yük dengeleyici dağıtımı başlar ve birkaç dakika içinde başarıyla tamamlanır.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [Standard Load Balancer ve kullanılabilirlik bölgeleri](load-balancer-standard-availability-zones.md).



