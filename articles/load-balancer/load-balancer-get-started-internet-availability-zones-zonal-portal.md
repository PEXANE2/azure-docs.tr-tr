---
title: Create a Load Balancer with a zonal frontend - Azure portal
titleSuffix: Azure Load Balancer
description: Learn how to create a Standard Load Balancer with zonal frontend with the Azure portal
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
#  <a name="create-a-standard-load-balancer-with-zonal-frontend-using-azure-portal"></a>Create a Standard Load Balancer with zonal frontend using Azure portal

This article steps through creating a public [Standard Load Balancer](https://aka.ms/azureloadbalancerstandard) with a zonal frontend IP configuration. To understand how availability zones work with Standard Load Balancer, see [Standard Load Balancer and Availability zones](load-balancer-standard-availability-zones.md). 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

> [!NOTE]
> Support for Availability Zones is available for select Azure resources and regions, and VM size families. For more information on how to get started, and which Azure resources, regions, and VM size families you can try availability zones with, see [Overview of Availability Zones](https://docs.microsoft.com/azure/availability-zones/az-overview). Destek için [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) üzerinden bize ulaşabilir veya [bir Azure destek bileti açabilirsiniz](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-load-balancer-with-zonal-frontend-ip-address"></a>Create a load balancer with zonal frontend IP address

1. From a browser navigate to the Azure portal: [https://portal.azure.com](https://portal.azure.com) and login with your Azure account.
2. On the top left-hand side of the screen, select **Create a resource** > **Networking** > **Load Balancer.**
3. In the **Create load balancer** page, under **Name** type **myLoadBalancer**.
4. **Tür** bölümünde **Genel**’i seçin.
5. Under SKU, select **Standard**.
6. Click **Choose a Public IP address**, click **Create new**, and in **Create public IP address** page, under name, type **myPublicIPZonal**, for SKU, select **Standard**, for Availability zone, select **1**.
    
>[!NOTE] 
> The public IP created in this step is of Standard SKU by default.

1. For **Resource group**, click **Create new**, and then type **myResourceGroupZLB** as the name of the resource group.
1. For **Location**, select **West Europe**, and then click **OK**. Yük dengeleyici dağıtımı başlar ve birkaç dakika içinde başarıyla tamamlanır.

    ![create zone-redundant Standard Load Balancer with the Azure portal](./media/load-balancer-get-started-internet-availability-zones-zonal-portal/load-balancer-zonal-frontend.png)


## <a name="next-steps"></a>Sonraki adımlar
- Learn more about [Standard Load Balancer and Availability zones](load-balancer-standard-availability-zones.md).



