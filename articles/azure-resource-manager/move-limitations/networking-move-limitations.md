---
title: Azure ağ kaynaklarını yeni aboneliğe veya kaynak grubuna taşıma | Microsoft Docs
description: Sanal ağları ve diğer ağ kaynaklarını yeni bir kaynak grubuna veya aboneliğe taşımak için Azure Resource Manager kullanın.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 10/16/2019
ms.author: tomfitz
ms.openlocfilehash: 14a7cb326a3017b1bdbcad21c8483eaaacd54674
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72437810"
---
# <a name="move-guidance-for-networking-resources"></a>Ağ kaynakları için kılavuzu taşıma

Bu makalede, belirli senaryolar için sanal ağların ve diğer ağ kaynaklarının nasıl taşınacağı açıklanır.

## <a name="dependent-resources"></a>Bağımlı kaynaklar

Bir sanal ağı taşırken bağımlı kaynaklarını da taşımanız gerekir. VPN ağ geçitleri için IP adreslerini, sanal ağ geçitlerini ve tüm ilişkili bağlantı kaynaklarını taşımanız gerekir. Yerel ağ geçitleri farklı bir kaynak grubunda olabilir.

Bir sanal makineyi bir ağ arabirimi kartıyla yeni bir aboneliğe taşımak için tüm bağımlı kaynakları taşımanız gerekir. Ağ arabirim kartının sanal ağını, sanal ağ için tüm diğer ağ arabirimi kartlarını ve VPN ağ geçitlerini taşıyın.

Daha fazla bilgi için bkz. [abonelikler arasında taşıma senaryosu](../resource-group-move-resources.md#scenario-for-move-across-subscriptions).

## <a name="peered-virtual-network"></a>Eşlenen sanal ağ

Eşlenen bir sanal ağı taşımak için öncelikle sanal ağ eşlemesini devre dışı bırakmanız gerekir. Devre dışı bırakıldıktan sonra sanal ağı taşıyabilirsiniz. Taşıdıktan sonra sanal ağ eşlemesini yeniden etkinleştirin.

## <a name="subnet-links"></a>Alt ağ bağlantıları

Sanal ağ, kaynak gezintisi bağlantıları içeren bir alt ağ içeriyorsa, sanal ağı farklı bir aboneliğe taşıyamazsınız. Örneğin, Redsıs kaynağı için bir Azure önbelleği bir alt ağa dağıtılmışsa, bu alt ağda bir kaynak gezinti bağlantısı bulunur.

## <a name="next-steps"></a>Sonraki adımlar

Kaynakları taşıma komutları için bkz. [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../resource-group-move-resources.md).
