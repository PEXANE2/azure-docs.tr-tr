---
title: 'Öğretici: kural altyapısını Yapılandırma-Azure ön kapısı'
description: Bu makalede, hem Azure portal hem de CLı 'de Rules Engine 'in nasıl yapılandırılacağı hakkında bir öğretici sunulmaktadır.
services: frontdoor
documentationcenter: ''
author: megan-beatty
editor: ''
ms.service: frontdoor
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 4/30/2020
ms.author: mebeatty
ms.custom: devx-track-azurecli
ms.openlocfilehash: 2ab9bc72aa30ddb16d0719827b6d7baeab86c057
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87491315"
---
# <a name="configure-your-rules-engine"></a>Kurallar altyapısını yapılandırma

Bu makalede, hem Azure portal hem de CLı 'de bir kural Altyapısı yapılandırması ve ilk kuralınız oluşturma adımları sağlanmaktadır. 

## <a name="configure-rules-engine-in-azure-portal"></a>Azure portal kural altyapısını yapılandırma
1. Kural Altyapısı yapılandırması oluşturmadan önce [bir ön kapı oluşturun](quickstart-create-front-door.md).

2. Ön kapı kaynağınız içinde **Ayarlar** ' a gidin ve **kural Altyapısı yapılandırması**' nı seçin. **Ekle**' ye tıklayın, yapılandırmanıza bir ad verin ve Ilk kural altyapısı yapılandırmanızı oluşturmaya başlayın.

    ![kural altyapısını bul](./media/front-door-rules-engine/rules-engine-tutorial-1.png)

3. İlk kuralınızı oluşturmak için **Kural Ekle** ' ye tıklayın. Ardından, **Koşul Ekle** veya **Eylem Ekle** ' ye tıklayarak kuralınızı tanımlayabilirsiniz.
    
    > [!NOTE]
    >- Kuraldan bir koşul veya eylem silmek için, belirli koşulun veya eylemin sağ tarafında çöp kutusu ' nu kullanın.
    > - Tüm gelen trafiğe uygulanan bir kural oluşturmak için herhangi bir koşul belirtmeyin.
    > - İlk eşleşme koşulu karşılandığında kuralları değerlendirmeyi durdurmak için, **kalan kuralı değerlendirmeyi durdur**' u işaretleyin. Bu işaretlenirse ve belirli bir kuralın eşleşme koşulları karşılanıyorsa, yapılandırmadaki kalan kurallar yürütülmez.  

    ![kural altyapısını bul](./media/front-door-rules-engine/rules-engine-tutorial-4.png) 

4. Yukarı taşı, aşağı taşı ve üste Taşı düğmelerini kullanarak yapılandırmanızın içindeki kuralların önceliğini belirleme. Öncelik artan sıradadır, yani ilk listelenen kural en önemli kuraldır.

5. Bir veya daha fazla kural oluşturduktan sonra **Kaydet**' e basın. Bu eylem, kural altyapısı yapılandırmanızı oluşturur.

6. Bir veya daha fazla yapılandırma oluşturduktan sonra, bir kural altyapısı yapılandırmasını bir yol kuralıyla ilişkilendirin. Tek bir yapılandırma birçok yol kuralına uygulanamadığından, bir yol kuralında yalnızca bir kural Altyapısı yapılandırması bulunabilir. İlişkilendirmeyi yapmak için **ön kapılı tasarımcı**  >  **yol kurallarınızın**bölümüne gidin. Kural altyapısı yapılandırmasını eklemek istediğiniz yol kuralını seçin, **yol ayrıntıları**  >  **kuralları altyapı yapılandırması**' na gidin ve ilişkilendirmek istediğiniz yapılandırmayı seçin.

    ![kural altyapısını bul](./media/front-door-rules-engine/rules-engine-tutorial-5.png)


## <a name="configure-rules-engine-in-azure-cli"></a>Azure CLı 'de kural altyapısını yapılandırma

1. Henüz yapmadıysanız [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)'yı yükleyemezsiniz. "Ön kapı" uzantısı ekleyin:-az Extension Add--Name ön kapısı. Ardından, oturum açın ve aboneliğinize geçiş yapın az Account set--Subscription <name_or_Id>.

2. Bir kural altyapısı oluşturarak başlayın-Bu örnekte, bir üst bilgi tabanlı eyleme ve bir eşleşme koşuluna sahip bir kural gösterilmektedir. 

```azurecli-interactive
az network front-door rules-engine rule create -f {front_door} -g {resource_group} --rules-engine-name {rules_engine} --name {rule1} --priority 1 --action-type RequestHeader --header-action Overwrite --header-name Rewrite --header-value True --match-variable RequestFilenameExtension --operator Contains --match-values jpg png --transforms Lowercase
```

3. Tüm kuralları listeleyin. 

```azurecli-interactive
az network front-door rules-engine rule list -f {front_door} -g {rg} --name {rules_engine}
```

4. Bir iletme yolu geçersiz kılma eylemi ekleyin. 

```azurecli-interactive
az network front-door rules-engine rule action add -f {front_door} -g {rg} --rules-engine-name {rules_engine} --name {rule1} --action-type ForwardRouteOverride --backend-pool {backend_pool_name} --caching Disabled
```

5. Bir kuraldaki tüm eylemleri listeleyin. 

```azurecli-interactive
az network front-door rules-engine rule action list -f {front_door} -g {rg} -r {rules_engine} --name {rule1}
```

6. Bir kural altyapısı yapılandırmasını bir yönlendirme kuralına bağlayın.  

```azurecli-interactive
az network front-door routing-rule update -g {rg} -f {front_door} -n {routing_rule_name} --rules-engine {rules_engine}
```

7. Kural altyapısının bağlantısını kaldır. 

```azurecli-interactive
az network front-door routing-rule update -g {rg} -f {front_door} -n {routing_rule_name} --remove rulesEngine # case sensitive word ‘rulesEngine’
```

Daha fazla bilgi için, [burada](https://docs.microsoft.com/cli/azure/ext/front-door/network/front-door/rules-engine?view=azure-cli-latest)AFD kuralları altyapı komutlarının tam listesini bulabilirsiniz.   

## <a name="next-steps"></a>Sonraki adımlar

- [AFD kuralları altyapısı](front-door-rules-engine.md)hakkında daha fazla bilgi edinin. 
- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.
- AFD kural altyapısı [CLI başvurusuna](https://docs.microsoft.com/cli/azure/ext/front-door/network/front-door/rules-engine?view=azure-cli-latest)daha fazla göz atın. 
- AFD kural altyapısı [PowerShell başvurusuna](https://docs.microsoft.com/powershell/module/az.frontdoor/?view=azps-3.8.0)daha fazla göz atın. 
