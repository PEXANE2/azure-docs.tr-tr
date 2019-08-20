---
title: Azure ağ kaynaklarını yeni aboneliğe veya kaynak grubuna taşıma | Microsoft Docs
description: Sanal ağları ve diğer ağ kaynaklarını yeni bir kaynak grubuna veya aboneliğe taşımak için Azure Resource Manager kullanın.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 08/19/2019
ms.author: tomfitz
ms.openlocfilehash: c41ab1c3309243fa3a96d907f620ffeff10376a1
ms.sourcegitcommit: 55e0c33b84f2579b7aad48a420a21141854bc9e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69626268"
---
# <a name="move-guidance-for-networking-resources"></a>Ağ kaynakları için kılavuzu taşıma

Bu makalede, belirli senaryolar için sanal ağların ve diğer ağ kaynaklarının nasıl taşınacağı açıklanır.

## <a name="dependent-resources"></a>Bağımlı kaynaklar

Bir sanal ağ taşırken, bağımlı kaynaklarını da taşımanız gerekir. VPN ağ geçitleri için IP adresleri, sanal ağ geçitleri ve tüm ilişkili bağlantı kaynakları taşımanız gerekir. Yerel ağ geçitleri farklı kaynak grubunda olabilir.

Bir sanal makineyi bir ağ arabirimi kartıyla taşımak için tüm bağımlı kaynakları taşımanız gerekir. Ağ arabirim kartının sanal ağını, sanal ağ için tüm diğer ağ arabirimi kartlarını ve VPN ağ geçitlerini taşıyın.

## <a name="state-of-dependent-resources"></a>Bağımlı kaynakların durumu

Kaynak veya hedef kaynak grubu bir sanal ağ içeriyorsa, sanal ağ için tüm bağımlı kaynakların durumları taşıma sırasında denetlenir. Bu kaynaklardan herhangi biri başarısız durumdaysa taşıma engellenir. Örneğin, sanal ağ kullanan bir sanal makine başarısız olduysa, taşıma engellenir. Sanal makine, taşınmakta olan kaynaklardan biri olmadığında ve taşıma için kaynak gruplarından birinde yer alsa bile taşıma engellenir. Bu sorundan kaçınmak için, kaynaklarınızı sanal ağı olmayan bir kaynak grubuna taşıyın.

## <a name="peered-virtual-network"></a>Eşlenen sanal ağ

Eşlenmiş sanal ağın taşımak için önce sanal ağ eşlemesi devre dışı bırakmanız gerekir. Devre dışı sonra sanal ağ taşıyabilirsiniz. Taşıma sonrasında, sanal ağ eşlemesi yeniden etkinleştirin.

## <a name="subnet-links"></a>Alt ağ bağlantıları

Bir alt ağ kaynak gezinti bağlantılarının bulunduğu sanal ağı içeriyorsa bir sanal ağ başka bir aboneliğe taşınamıyor. Örneğin, bir Azure önbelleği için Redis kaynak bir alt ağa dağıtılırsa, bu alt ağ kaynak Gezinti bağlantısı vardır.

## <a name="next-steps"></a>Sonraki adımlar

Kaynakları taşıma komutları için bkz. [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../resource-group-move-resources.md).
