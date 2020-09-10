---
title: Bölgeler arasında Azure kaynaklarını taşımak için bir araç seçin
description: Azure kaynaklarını bölgeler arasında taşımaya yönelik seçenekleri ve araçları gözden geçirin
author: rayne-wiselman
manager: evansma
ms.service: resource-move
ms.topic: overview
ms.date: 09/09/2020
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 8af5cd4f961a2138f09bb3e56fccf7aa8f726dd9
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89653618"
---
# <a name="choose-a-tool-for-moving-azure-resources"></a>Azure kaynaklarını taşımak için bir araç seçin

Azure içindeki kaynakları aşağıdaki gibi taşıyabilirsiniz:

- **Kaynakları bölgeler arasında taşıma**: kaynağı kaynak taşıyıcısı hub 'ından veya bir kaynak grubu içinden taşıyın. 
- **Kaynakları kaynak grupları/abonelikler arasında taşıma**: kaynak grubu içinden taşıyın. 
- **Azure bulutları arasında kaynakları taşıma**: kaynakları ortak ve kamu bulutları arasında taşımak için Azure Site Recovery hizmetini kullanın.
- **Kaynakları aynı bölgedeki kullanılabilirlik alanları arasında taşıma**: kaynakları aynı Azure bölgesindeki kullanılabilirlik alanları arasında taşımak için Azure Site Recovery hizmetini kullanın.


## <a name="compare-move-tools"></a>Taşıma araçlarını karşılaştırma

**Araç** | **Kullanılması gereken durumlar**
--- | --- 
**Kaynak grubu içinde taşı** | Kaynakları farklı bir kaynak grubuna/aboneliğe veya bölgeler arasında taşıyın.<br/><br/> Bölgeler arasında geçiş yaparsanız, kaynak grubunda, taşımak istediğiniz kaynakları seçin ve ardından, bağımlılıkları doğrulamak ve kaynakları hedef bölgeye taşımak için kaynak taşıyıcısı hub 'ına geçiş yapın.
**Kaynak taşıyıcısı hub 'ından taşıma** | Kaynakları bölgeler arasında taşıyın. <br/><br/> Hedef bölge dahilinde bir hedef bölgeye veya belirli bir kullanılabilirlik bölgesine veya kullanılabilirlik kümesine taşıyabilirsiniz.
**VM 'Leri Site Recovery taşıyın** | Azure VM 'lerini kamu ve özel bulutlar arasında taşımak için kullanın.<br/><br/> VM 'Leri aynı bölgedeki kullanılabilirlik alanları arasında taşımak istiyorsanız kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Kaynak taşıyıcısı bileşenleri ve taşıma işlemi hakkında [daha fazla bilgi edinin](about-move-process.md) .
