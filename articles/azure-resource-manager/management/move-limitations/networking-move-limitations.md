---
title: Azure Ağ kaynaklarını yeni abonelik veya kaynak grubuna taşıma
description: Sanal ağları ve diğer ağ kaynaklarını yeni bir kaynak grubuna veya aboneye taşımak için Azure Kaynak Yöneticisi'ni kullanın.
ms.topic: conceptual
ms.date: 10/16/2019
ms.openlocfilehash: 0cd6887d3489f2ffede0f5e3d63533a33a6ccc04
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75485240"
---
# <a name="move-guidance-for-networking-resources"></a>Ağ kaynakları için kılavuzu taşıma

Bu makalede, belirli senaryolar için sanal ağların ve diğer ağ kaynaklarının nasıl taşınır.

## <a name="dependent-resources"></a>Bağımlı kaynaklar

Sanal bir ağı taşırken, bağımlı kaynaklarını da taşımanız gerekir. VPN Ağ Geçitleri için IP adreslerini, sanal ağ ağ ağ larını ve ilişkili tüm bağlantı kaynaklarını taşımanız gerekir. Yerel ağ ağ geçitleri farklı bir kaynak grubunda olabilir.

Ağ arabirim kartı olan sanal bir makineyi yeni bir aboneye taşımak için tüm bağımlı kaynakları taşımanız gerekir. Ağ arabirimi kartı için sanal ağı, sanal ağ için diğer tüm ağ arabirim kartlarını ve VPN ağ geçitlerini taşıyın.

Daha fazla bilgi [için, abonelikler arasında taşıma senaryosuna](../move-resource-group-and-subscription.md#scenario-for-move-across-subscriptions)bakın.

## <a name="peered-virtual-network"></a>Eşli sanal ağ

Eşlenen bir sanal ağı taşımak için öncelikle sanal ağ eşlemesi devre dışı bmelisiniz. Devre dışı bırakıldıktan sonra sanal ağı taşıyabilirsiniz. Taşımadan sonra, sanal ağ eşlemesini yeniden etkinleştirin.

## <a name="subnet-links"></a>Alt ağ bağlantıları

Sanal ağ kaynak gezinme bağlantılarına sahip bir alt ağ içeriyorsa, sanal ağı farklı bir aboneye taşıyamazsınız. Örneğin, Redis için bir Azure Önbelleği kaynağı bir alt ağa dağıtılırsa, bu alt ağda bir kaynak gezinti bağlantısı vardır.

## <a name="next-steps"></a>Sonraki adımlar

Komutların kaynakları taşıması [için](../move-resource-group-and-subscription.md)bkz.
