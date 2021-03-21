---
title: Azure Application Gateway ınress denetleyicisi Held 'den AGIC eklentisi 'ne geçiş
description: Bu makalede, bir AKS eklentisi olarak dağıtılan Held 'ye dağıtılan AGIC 'ten nasıl geçiş yapılacağı hakkında yönergeler sağlanmaktadır
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 03/02/2021
ms.author: caya
ms.openlocfilehash: e83834fd5f8ca95826118c952f7884a494c7abbb
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102050843"
---
# <a name="migrate-from-agic-helm-to-agic-add-on"></a>AGIC Held 'den AGIC eklentiye geçirme 

Helk aracılığıyla dağıtılmış ancak bir AKS eklentisi olarak dağıtılan AGIC 'e geçiş yapmak istiyorsanız, aşağıdaki adımlar geçiş sürecinde size rehberlik eder. 

## <a name="prerequisites"></a>Önkoşullar 
Geçiş işlemine başlamadan önce, denetlenecek birkaç nokta vardır. 
  - [Şu anda agic eklentisi ile desteklenmeyen](ingress-controller-overview.md#difference-between-helm-deployment-and-aks-add-on)agic Held ile herhangi bir özellik kullanıyor musunuz?
  - AKS kümesi başına birden fazla AGıC Held dağıtımı kullanıyor musunuz? 
  - Bir Application Gateway hedeflemek için birden çok AGIC Held dağıtımı kullanıyor musunuz? 

Yukarıdaki sorulardan birine Evet yanıtını verdiyseniz, AGIC eklentisi henüz kullanım durumunu desteklemez, bu nedenle en iyi yöntem de AGIC Helm 'yi kullanmaya devam etmek için idealdir. Aksi takdirde, iş saatleri dışında aşağıdaki geçiş işlemine devam edin. 

## <a name="find-the-application-gateway-resource-id-that-agic-helm-is-currently-targeting"></a>AGıC Held 'nin şu anda hedeflediği Application Gateway kaynak KIMLIĞINI bulun 
AGIC helk dağıtımınızın hedeflediği Application Gateway gidin. Application Gateway kaynak KIMLIĞINI kopyalayın ve kaydedin. Kaynak KIMLIĞINE sonraki bir adımda ihtiyacınız olacak. Kaynak KIMLIĞI, Application Gateway veya Azure CLı aracılığıyla Portal 'da bulunabilir. Aşağıdaki örnekte, *Myresourcegroup* kaynak grubundaki *myApplicationGateway* adlı bir ağ geçidi için *APPGWıD* Application Gateway kaynak kimliği kaydedilir.

```azurecli-interactive
appgwId=$(az network application-gateway show -n myApplicationGateway -g myResourceGroup -o tsv --query "id") 
```

## <a name="delete-agic-helm-from-your-aks-cluster"></a>AKS kümenizdeki AGIC Held 'yi silme
Azure CLı aracılığıyla, kümeinizden AGIC Held dağıtımınızı silin. AGIC AKS eklentisini etkinleştirebilmeniz için önce AGIC Held dağıtımını silmeniz gerekir. AKS kümeniz içinde, AGIC helk dağıtımınızı silmenin süresi ile AGIC eklentisini etkinleştirdiğiniz zaman arasındaki tüm değişikliklerin Application Gateway yansıtılmadığını ve bu sayede, etkiyi en aza indirmek için bu geçiş işleminin iş saatleri dışında yapılması gerekir. Application Gateway, son yapılandırmanın AGIC tarafından uygulanması için devam edecek ve bu nedenle mevcut yönlendirme kurallarının etkilenmeyecektir. 

## <a name="enable-agic-add-on-using-your-existing-application-gateway"></a>Mevcut Application Gateway kullanarak AGIC eklentisini etkinleştirin 
Artık AKS kümenizdeki AGIC eklentisini Azure CLı veya portalı aracılığıyla mevcut Application Gateway hedeflemek için etkinleştirebilirsiniz. AKS kümenizdeki AGIC eklentisini etkinleştirmek için aşağıdaki Azure CLı komutunu çalıştırın. Örnek, *Myresourcegroup* adlı bir kaynak *grubunda,* myresourcegroup adlı bir kaynak grubundaki eklentiyi, yukarıda yer alan *APPGWıD* Application Gateway kaynak kimliği ' ni kullanarak, önceki adımda kaydettiğiniz bir kaynak grubunda sunar. 


```azurecli-interactive
az aks enable-addons -n myCluster -g myResourceGroup -a ingress-appgw --appgw-id $appgwId
```

Alternatif olarak, bu [bağlantıyı](https://portal.azure.com/?feature.aksagic=true) kullanarak portal 'daki aks kümenize gidebilir ve kümenizin Ağ sekmesinden agic eklentisini etkinleştirebilirsiniz. Eklentinin hangi Application Gateway hedeflemesini seçtiğinizde açılır menüden mevcut Application Gateway seçin. 

![Application Gateway giriş denetleyicisi portalı](./media/tutorial-ingress-controller-add-on-existing/portal-ingress-controller-add-on.png)

## <a name="next-steps"></a>Sonraki Adımlar
- [**Application Gateway giriş denetleyicisi sorunlarını giderme**](ingress-controller-troubleshoot.md): agic için sorun giderme kılavuzu 
- [**Application Gateway giriş denetleyicisi ek açıklamaları**](ingress-controller-annotations.md): agic üzerinde ek açıklamaların listesi 