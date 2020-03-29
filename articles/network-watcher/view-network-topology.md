---
title: Azure sanal ağ topolojisi görüntüleyin | Microsoft Dokümanlar
description: Sanal ağdaki kaynakları ve kaynaklar arasındaki ilişkileri nasıl görüntüleyeceklerini öğrenin.
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2018
ms.author: damendo
ms.openlocfilehash: 675919db55932d3ccc04fd5397f6f673832b4900
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76840579"
---
# <a name="view-the-topology-of-an-azure-virtual-network"></a>Azure sanal ağının topolojisini görüntüleyin

Bu makalede, microsoft Azure sanal ağındaki kaynakları nasıl görüntülediğinizi ve kaynaklar arasındaki ilişkileri öğrenirsiniz. Örneğin, sanal ağ alt ağlar içerir. Alt ağlar Azure Sanal Makineleri (VM) gibi kaynaklar içerir. VM'lerin bir veya daha fazla ağ arabirimi vardır. Her alt ağda bir ağ güvenlik grubu ve bununla ilişkili bir rota tablosu olabilir. Azure Ağ İzleyicisi'nin topoloji özelliği, sanal ağdaki tüm kaynakları, sanal ağdaki kaynaklarla ilişkili kaynakları ve kaynaklar arasındaki ilişkileri görüntülemenize olanak tanır.

Bir topolojiyi görüntülemek için [Azure portalını](#azure-portal), [Azure CLI](#azure-cli)veya [PowerShell'i](#powershell) kullanabilirsiniz.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="view-topology---azure-portal"></a><a name = "azure-portal"></a>Topolojiyi görüntüle - Azure portalı

1. Gerekli [izinlere](required-rbac-permissions.md)sahip bir hesapla [Azure portalına](https://portal.azure.com) giriş yapın.
2. Portalın üst, sol köşesinde, **Tüm hizmetleri**seçin.
3. Tüm **hizmetler** filtre kutusuna *Ağ İzleyicisi*girin. **Ağ İzleyicisi**, sonuçlarda görüntülendiğinde onu seçin.
4. **Topoloji**’yi seçin. Bir topoloji oluşturmak için topoloji oluşturmak istediğiniz sanal ağ var aynı bölgede bir ağ izleyici gerektirir. Topoloji oluşturmak istediğiniz sanal ağın bulunduğu bölgede etkin leştirilmiş bir ağ izleyiciniz yoksa, ağ izleyicileri tüm bölgelerde sizin için otomatik olarak oluşturulur. Ağ izleyicileri **NetworkWatcherRG**adlı bir kaynak grubunda oluşturulur.
5. Bir abonelik seçin, topolojiyi görüntülemek istediğiniz sanal ağın kaynak grubu ve ardından sanal ağı seçin. Aşağıdaki resimde, *MyResourceGroup*adlı kaynak grubunda *MyVnet*adlı sanal bir ağ için bir topoloji gösterilir:

    ![Topolojiyi görüntüle](./media/view-network-topology/view-topology.png)

    Önceki resimde de görebileceğiniz gibi, sanal ağ üç alt ağ içerir. Bir alt ağda bir VM dağıtılır. VM'nin bağlı bir ağ arabirimi ve buna bağlı genel bir IP adresi vardır. Diğer iki alt ağ, bunlarla ilişkili bir rota tablosuna sahiptir. Her rota tablosu iki rota içerir. Bir alt ağda bununla ilişkili bir ağ güvenlik grubu vardır. Topoloji bilgileri yalnızca şu kaynaklar için gösterilir:
    
    - *myVnet* sanal ağıyla aynı kaynak grubu ve bölge içinde. Örneğin, *MyResourceGroup*dışındaki bir kaynak grubunda bulunan bir ağ güvenlik grubu, ağ güvenlik grubu *MyVnet* sanal ağındaki bir alt ağla ilişkilendirilse bile gösterilmez.
    - *myVnet* sanal ağı içindeki kaynaklar içinde veya bu kaynaklarla ilişkili. Örneğin, *myVnet* sanal ağındaki bir alt ağ veya ağ arabirimiyle ilişkili olmayan bir ağ güvenlik grubu, ağ güvenlik grubu *MyResourceGroup* kaynak grubunda olsa bile gösterilmez.

   Resimde gösterilen topoloji, [Azure CLI](../virtual-network/scripts/virtual-network-cli-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json)veya [PowerShell](../virtual-network/scripts/virtual-network-powershell-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json)kullanarak dağıtabileceğiniz **bir ağ sanal cihaz komut dosyası örneği aracılığıyla Route trafiğini**dağıttıktan sonra oluşturulan sanal ağ içindir.

6. Görüntüyü svg formatında, editable dosya olarak indirmek için **İndir topolojisini** seçin.

Diyagramda gösterilen kaynaklar, sanal ağdaki ağ bileşenlerinin bir alt kümesidir. Örneğin, bir ağ güvenlik grubu gösterilirken, içindeki güvenlik kuralları diyagramda gösterilmez. Diyagramda farklılaşmamış olsa da, çizgiler iki ilişkiden birini temsil eder: *İçerme* veya *ilişkili.* Sanal ağdaki kaynakların tam listesini ve kaynaklar arasındaki ilişkinin türünü görmek için [PowerShell](#powershell) veya [Azure CLI](#azure-cli)ile topoloji oluşturun.

## <a name="view-topology---azure-cli"></a><a name = "azure-cli"></a>Topolojiyi görüntüle - Azure CLI

Aşağıdaki adımları komutları çalıştırabilirsiniz:
- Azure Bulut Kabuğu'nda, herhangi bir komutun sağ üst kısmında **n için dene'yi** seçerek. Azure Bulut Kabuğu, hesabınızla birlikte kullanmak üzere önceden yüklenmiş ve yapılandırılmış ortak Azure araçları olan ücretsiz bir etkileşimli kabuktür.
- CLI'yi bilgisayarınızdan çalıştırarak. CLI'yi bilgisayarınızdan çalıştırınca, bu makaledeki adımlar azure CLI sürüm 2.0.31 veya sonrası gerektirir. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). Azure CLI'yi yerel olarak çalıştırıyorsanız, `az login` Azure ile bağlantı oluşturmak için de çalıştırmanız gerekir.

Kullandığınız hesap gerekli [izinlere](required-rbac-permissions.md)sahip olmalıdır.

1. Bir topoloji oluşturmak istediğiniz sanal ağla aynı bölgede zaten bir ağ izleyiciniz varsa, adım 3'e geçin. Az grubu oluşturmak ile bir ağ izleyicisi içerecek bir kaynak grubu [oluşturun.](/cli/azure/group) Aşağıdaki *örnekeastus* bölgesinde kaynak grubu oluşturur:

    ```azurecli-interactive
    az group create --name NetworkWatcherRG --location eastus
    ```

2. [az ağ izleyicisi yapılandırmak](/cli/azure/network/watcher#az-network-watcher-configure)ile bir ağ izleyicioluşturun. Aşağıdaki örnek *eastus* bölgesinde bir ağ izleyicisi oluşturur:

    ```azurecli-interactive
    az network watcher configure \
      --resource-group NetworkWatcherRG \
      --location eastus \
      --enabled true
    ```

3. [az ağ izleyici show-topoloji](/cli/azure/network/watcher#az-network-watcher-show-topology)si ile topolojiyi görüntüleyin. Aşağıdaki örnek, *MyResourceGroup*adlı bir kaynak grubunun topolojisini görüntüler:

    ```azurecli-interactive
    az network watcher show-topology --resource-group MyResourceGroup
    ```

    Topoloji bilgileri yalnızca *MyResourceGroup* kaynak grubuyla aynı kaynak grubunda bulunan kaynaklar ve ağ izleyicisi ile aynı bölge için döndürülür. Örneğin, *MyResourceGroup*dışındaki bir kaynak grubunda bulunan bir ağ güvenlik grubu, ağ güvenlik grubu *MyVnet* sanal ağındaki bir alt ağla ilişkilendirilse bile gösterilmez.

   Döndürülen çıktıdaki ilişkiler ve [özellikler](#properties) hakkında daha fazla bilgi edinin. Bir topolojiyi görüntülemek için mevcut bir sanal ağınız yoksa, bir ağ sanal cihaz komut dosyası örneği [aracılığıyla Rota trafiğini](../virtual-network/scripts/virtual-network-cli-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) kullanarak bir ağ oluşturabilirsiniz. Topolojinin diyagramını görüntülemek ve editable bir dosyaya indirmek için [portalı](#azure-portal)kullanın.

## <a name="view-topology---powershell"></a><a name = "powershell"></a>Topolojiyi görüntüle - PowerShell

Aşağıdaki adımları komutları çalıştırabilirsiniz:
- Azure Bulut Kabuğu'nda, herhangi bir komutun sağ üst kısmında **n için dene'yi** seçerek. Azure Bulut Kabuğu, hesabınızla birlikte kullanmak üzere önceden yüklenmiş ve yapılandırılmış ortak Azure araçları olan ücretsiz bir etkileşimli kabuktür.
- Bilgisayarınızdan PowerShell çalıştırarak. PowerShell'i bilgisayarınızdan çalıştırınsa, bu `Az` makalede Azure PowerShell modülü gerekir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

Kullandığınız hesap gerekli [izinlere](required-rbac-permissions.md)sahip olmalıdır.

1. Bir topoloji oluşturmak istediğiniz sanal ağla aynı bölgede zaten bir ağ izleyiciniz varsa, adım 3'e geçin. [New-AzResourceGroup'a](/powershell/module/az.Resources/New-azResourceGroup)sahip bir ağ izleyicisi içerecek bir kaynak grubu oluşturun. Aşağıdaki *örnekeastus* bölgesinde kaynak grubu oluşturur:

    ```azurepowershell-interactive
    New-AzResourceGroup -Name NetworkWatcherRG -Location EastUS
    ```

2. [New-AzNetworkWatcher](/powershell/module/az.network/new-aznetworkwatcher)ile bir ağ izleyicisi oluşturun. Aşağıdaki örnek eastus bölgesinde bir ağ izleyicisi oluşturur:

    ```azurepowershell-interactive
    New-AzNetworkWatcher `
      -Name NetworkWatcher_eastus `
      -ResourceGroupName NetworkWatcherRG
    ```

3. [Get-AzNetworkWatcher](/powershell/module/az.network/get-aznetworkwatcher)ile bir Ağ İzleyicisi örneğini alın. Aşağıdaki örnek, Doğu ABD bölgesinde bir ağ izleyicisi alır:

    ```azurepowershell-interactive
    $nw = Get-AzResource `
      | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "EastUS" }
    $networkWatcher = Get-AzNetworkWatcher `
      -Name $nw.Name `
      -ResourceGroupName $nw.ResourceGroupName
    ```

4. [Get-AzNetworkWatcherTopology](/powershell/module/az.network/get-aznetworkwatchertopology)ile bir topoloji alın. Aşağıdaki örnek, *MyResourceGroup*adlı kaynak grubundaki bir sanal ağ için bir topoloji alır:

    ```azurepowershell-interactive
    Get-AzNetworkWatcherTopology `
      -NetworkWatcher $networkWatcher `
      -TargetResourceGroupName MyResourceGroup
    ```

   Topoloji bilgileri yalnızca *MyResourceGroup* kaynak grubuyla aynı kaynak grubunda bulunan kaynaklar ve ağ izleyicisi ile aynı bölge için döndürülür. Örneğin, *MyResourceGroup*dışındaki bir kaynak grubunda bulunan bir ağ güvenlik grubu, ağ güvenlik grubu *MyVnet* sanal ağındaki bir alt ağla ilişkilendirilse bile gösterilmez.

   Döndürülen çıktıdaki ilişkiler ve [özellikler](#properties) hakkında daha fazla bilgi edinin. Bir topolojiyi görüntülemek için mevcut bir sanal ağınız yoksa, bir ağ sanal cihaz komut dosyası örneği [aracılığıyla Rota trafiğini](../virtual-network/scripts/virtual-network-powershell-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) kullanarak bir ağ oluşturabilirsiniz. Topolojinin diyagramını görüntülemek ve editable bir dosyaya indirmek için [portalı](#azure-portal)kullanın.

## <a name="relationships"></a>İlişkiler

Bir topolojide döndürülen tüm kaynaklar, başka bir kaynakla aşağıdaki ilişki türlerinden birine sahiptir:

| İlişki türü | Örnek                                                                                                |
| ---               | ---                                                                                                    |
| Kapsama       | Sanal ağ bir alt ağ içerir. Bir alt ağ bir ağ arabirimi içerir.                            |
| İlişkili        | Ağ arabirimi vm ile ilişkilidir. Ortak BIR IP adresi bir ağ arabirimiyle ilişkilidir. |

## <a name="properties"></a>Özellikler

Bir topolojide döndürülen tüm kaynaklar aşağıdaki özelliklere sahiptir:

- **Adı**: Kaynağın adı
- **Id**: Kaynağın URI'si.
- **Konum**: Kaynağın bulunduğu Azure bölgesi.
- **Çağrışımlar**: Başvurulan nesneye çağrışımlistesi. Her ilişkilendirme aşağıdaki özelliklere sahiptir:
    - **AssociationType**: Alt nesne ile üst öğe arasındaki ilişkiye başvurur. Geçerli değerler *Içerir* veya *İlişkili.*
    - **Adı**: Başvurulan kaynağın adı.
    - **ResourceId**: - Ilişkilendirmede başvurulan kaynağın URI'si.

## <a name="next-steps"></a>Sonraki adımlar

- Ağ İzleyicisi'nin IP akışını doğrulama özelliğini kullanarak [bir Ağ trafiği filtresi sorununu VM'ye veya vm'den](diagnose-vm-network-traffic-filtering-problem.md) nasıl tanılayizleyeceğinizi öğrenin
- Network Watcher'ın bir sonraki atlama özelliğini kullanarak [bir VM'den ağ trafiği yönlendirme sorununu](diagnose-vm-network-routing-problem.md) nasıl tanılayizleyeceğizi öğrenin
