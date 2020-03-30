---
title: Bir VNet TAP oluşturma, değiştirme veya silme - Azure CLI
description: Azure CLI'yi kullanarak sanal ağ TAP'ı nasıl oluşturup değiştireceğinizi veya sileceğinizi öğrenin.
services: virtual-network
documentationcenter: na
author: karthikananth
manager: ganesr
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/18/2018
ms.author: kaanan
ms.openlocfilehash: 56288a65dc9e5b12a12393965b9670e394146181
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80234967"
---
# <a name="work-with-a-virtual-network-tap-using-the-azure-cli"></a>Azure CLI'yi kullanarak sanal ağ TAP ile çalışın

Azure sanal ağ TAP (Terminal Erişim Noktası), sanal makine ağ trafiğinizi sürekli olarak bir ağ paketi toplayıcısına veya analiz aracına aktarmanızı sağlar. Toplayıcı veya analiz aracı bir [ağ sanal cihaz](https://azure.microsoft.com/solutions/network-appliances/) ortağı tarafından sağlanır. Sanal ağ TAP ile çalışmak üzere doğrulanmış iş ortağı çözümlerinin listesi için [iş ortağı çözümlerine](virtual-network-tap-overview.md#virtual-network-tap-partner-solutions)bakın. 

## <a name="create-a-virtual-network-tap-resource"></a>Sanal ağ DOKUNUN kaynağı oluşturma

Sanal ağ TAP kaynağı oluşturmadan önce [ön koşulları](virtual-network-tap-overview.md#prerequisites) okuyun. [Azure Bulut Kabuğu'nda](https://shell.azure.com/bash)takip eden komutları veya bilgisayarınızdan Azure komut satırı arabirimini (CLI) çalıştırarak çalıştırabilirsiniz. Azure Bulut Kabuğu, Bilgisayarınıza Azure CLI yüklemeyi gerektirmeyen ücretsiz bir etkileşimli kabuktür. Azure'da uygun izinlere sahip bir hesapla oturum [açmanız](virtual-network-tap-overview.md#permissions)gerekir. Bu makale, Azure CLI sürümü 2.0.46 veya sonrası gerektirir. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). Sanal ağ TAP şu anda bir uzantısı olarak kullanılabilir. Çalıştırmanız gereken uzantıyı `az extension add -n virtual-network-tap`yüklemek için. Azure CLI'yi yerel olarak çalıştırıyorsanız, `az login` Azure ile bağlantı oluşturmak için de çalıştırmanız gerekir.

1. Aboneliğinizin kimliğini daha sonraki bir adımda kullanılan bir değişkene alın:

   ```azurecli-interactive
   subscriptionId=$(az account show \
   --query id \
   --out tsv)
   ```

2. Sanal ağ TAP kaynağı oluşturmak için kullanacağınız abonelik kimliğini ayarlayın.

   ```azurecli-interactive
   az account set --subscription $subscriptionId
   ```

3. Sanal ağ TAP kaynağı oluşturmak için kullanacağınız abonelik kimliğini yeniden kaydedin. BIR TAP kaynağı oluşturduğunuzda bir kayıt hatası alırsanız, aşağıdaki komutu çalıştırın:

   ```azurecli-interactive
   az provider register --namespace Microsoft.Network --subscription $subscriptionId
   ```

4. Sanal ağ TAP için hedef toplayıcı veya analitik aracı için ağ sanal cihaz üzerinde ağ arayüzü ise -

   - Ağ sanal cihazın ağ arabiriminin IP yapılandırmasını daha sonraki bir adımda kullanılan bir değişkene dönüştürün. Kimlik, TAP trafiğini biraraya getirecek bitiş noktasıdır. Aşağıdaki örnek, *myResourceGroup*adlı bir kaynak grubunda *myNetworkInterface*adlı bir ağ arabirimi için *ipconfig1* IP yapılandırmasının kimliğini alır:

      ```azurecli-interactive
       IpConfigId=$(az network nic ip-config show \
       --name ipconfig1 \
       --nic-name myNetworkInterface \
       --resource-group myResourceGroup \
       --query id \
       --out tsv)
      ```

   - Hedef ve isteğe bağlı bağlantı noktası özelliği olarak IP yapılandırmasının kimliğini kullanarak westcentralus azure bölgesinde sanal ağ TAP'ı oluşturun. Bağlantı noktası, TAP trafiğinin alınacağı ağ arabirimi IP yapılandırması üzerindeki hedef bağlantı noktasını belirtir:  

      ```azurecli-interactive
       az network vnet tap create \
       --resource-group myResourceGroup \
       --name myTap \
       --destination $IpConfigId \
       --port 4789 \
       --location westcentralus
      ```

5. Sanal ağ TAP'ın hedefi azure dahili yük dengeleyicisiyse:
  
   - Azure dahili yük dengeleyicisinin ön uç IP yapılandırmasını daha sonraki bir adımda kullanılan bir değişkene dönüştürün. Kimlik, TAP trafiğini biraraya getirecek bitiş noktasıdır. Aşağıdaki örnek, *myInternalLoadBalancer*adlı bir yük dengeleyicisi için *frontendipconfig1* ön uç IP yapılandırmasının kimliğini *myResourceGroup*adlı bir kaynak grubunda alır:

      ```azurecli-interactive
      FrontendIpConfigId=$(az network lb frontend-ip show \
      --name frontendipconfig1 \
      --lb-name myInternalLoadBalancer \
      --resource-group myResourceGroup \
      --query id \
      --out tsv)
      ```

   - Hedef ve isteğe bağlı bağlantı noktası özelliği olarak ön uç IP yapılandırmasının kimliğini kullanarak sanal ağ TAP'ı oluşturun. Bağlantı noktası, TAP trafiğinin alınacağı ön uç IP yapılandırması üzerindeki hedef bağlantı noktasını belirtir:  

      ```azurecli-interactive
      az network vnet tap create \
      --resource-group myResourceGroup \
      --name myTap \
      --destination $FrontendIpConfigId \
      --port 4789 \
     --location westcentralus
     ```

6. Sanal ağ TAP'In oluşturulmasını onaylayın:

   ```azurecli-interactive
   az network vnet tap show \
   --resource-group myResourceGroup
   --name myTap
   ```

## <a name="add-a-tap-configuration-to-a-network-interface"></a>Ağ arabirimine TAP yapılandırması ekleme

1. Varolan bir sanal ağ TAP kaynağının kimliğini alın. Aşağıdaki örnek, *myResourceGroup*adlı bir kaynak grubunda *myTap* adlı sanal ağ TAP'ı alır:

   ```azurecli-interactive
   tapId=$(az network vnet tap show \
   --name myTap \
   --resource-group myResourceGroup \
   --query id \
   --out tsv)
   ```

2. İzlenen sanal makinenin ağ arabiriminde bir TAP yapılandırması oluşturun. Aşağıdaki örnek, *myNetworkInterface*adlı bir ağ arabirimi için BIR TAP yapılandırması oluşturur:

   ```azurecli-interactive
   az network nic vtap-config create \
   --resource-group myResourceGroup \
   --nic myNetworkInterface \
   --vnet-tap $tapId \
   --name mytapconfig \
   --subscription subscriptionId
   ```

3. TAP yapılandırmasının oluşturulmasını onaylayın:

   ```azurecli-interactive
   az network nic vtap-config show \
   --resource-group myResourceGroup \
   --nic-name myNetworkInterface \
   --name mytapconfig \
   --subscription subscriptionId
   ```

## <a name="delete-the-tap-configuration-on-a-network-interface"></a>Ağ arabirimindeki TAP yapılandırmasını silme

   ```azurecli-interactive
   az network nic vtap-config delete \
   --resource-group myResourceGroup \
   --nic myNetworkInterface \
   --name myTapConfig \
   --subscription subscriptionId
   ```

## <a name="list-virtual-network-taps-in-a-subscription"></a>Abonelikteki sanal ağ TAP'lerini listele

   ```azurecli-interactive
   az network vnet tap list
   ```

## <a name="delete-a-virtual-network-tap-in-a-resource-group"></a>Kaynak grubundaki sanal ağ TAP'ı silme

   ```azurecli-interactive
   az network vnet tap delete \
   --resource-group myResourceGroup \
   --name myTap
   ```
