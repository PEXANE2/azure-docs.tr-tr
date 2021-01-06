---
title: Azure purview hesabına bağlanma 
description: Bir Azure purview hesabını bir Synapse çalışma alanına bağlayın.
services: synapse-analytics
author: ArnoMicrosoft
ms.service: synapse-analytics
ms.topic: quickstart
ms.date: 12/16/2020
ms.author: acomet
ms.reviewer: jrasnick
ms.openlocfilehash: f85a76a7ce01c11255f54c6b956088f9328dfc9f
ms.sourcegitcommit: 67b44a02af0c8d615b35ec5e57a29d21419d7668
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2021
ms.locfileid: "97918741"
---
# <a name="quickstartconnect-an-azure-purview-account-to-a-synapse-workspace"></a>Hızlı başlangıç: bir Azure purview hesabını bir Synapse çalışma alanına bağlama 

> [!IMPORTANT]
> Azure SYNAPSE Analytics ve Azure purview arasındaki tümleştirme Şu anda önizleme aşamasındadır. Azure purview 'i SYNAPSE ' de denemek istiyorsanız lütfen Microsoft satış temsilcinizle bağlantı sağlayın.

Bu hızlı başlangıçta bir Azure purview hesabını bir Synapse çalışma alanına kaydedecaksınız. Bu bağlantı, Azure purview varlıklarını keşfetmenize ve SYNAPSE özellikleri aracılığıyla bunlarla etkileşime geçmesini sağlar. 

SYNAPSE içinde aşağıdaki görevleri gerçekleştirebilirsiniz: 
- Anahtar sözcüklere göre varlıkları bulmak için üstteki arama kutusunu kullanın 
- Meta veriler, kökenini, ek açıklamalar temelinde verileri anlayın 
- Bağlı hizmetler veya tümleştirme veri kümeleriyle bu verileri çalışma alanınıza bağlayın 
- Bu veri kümelerini SYNAPSE Apache Spark, SYNAPSE SQL ve veri akışı ile çözümleyin 

## <a name="prerequisites"></a>Ön koşullar 
- [Azure purview hesabı](../../purview/create-catalog-portal.md) 
- [SYNAPSE çalışma alanı](../quickstart-create-workspace.md) 

## <a name="signin-toa-synapse-workspace"></a>Bir Synapse çalışma alanında oturum açın 

https://web.azuresynapse.netÇalışma alanınıza gidin ve oturum açın. 

## <a name="permissions-for-connecting-an-azure-purview-account"></a>Azure purview hesabını bağlama izinleri 

- Bir Azure purview hesabını bir Synapse çalışma alanına bağlamak için Azure portal ıAM 'den SYNAPSE çalışma alanında **katkıda** bulunan bir rol gerekir ve bu Azure purview hesabına erişmeniz gerekir.

## <a name="connect-an-azure-purview-account"></a>Azure purview hesabına bağlanma  

- SYNAPSE çalışma alanında   ->  **Azure purview**'ı Yönet ' e gidin. **Bir purview hesabına Bağlan**' ı seçin. 
- **Azure aboneliği** arasından seçim yapabilir veya **el ile girebilirsiniz**. **Azure aboneliğinden** erişiminiz olan hesabı seçebilirsiniz. 
- Bağlandıktan sonra, **Azure purview hesabı** sekmesinden purview hesabının adını görmeniz gerekir. 
- Veri aramak için SYNAPSE çalışma alanının en üst merkezinde bulunan arama çubuğunu kullanabilirsiniz. 

## <a name="nextsteps"></a>Sonraki adımlar 

[Azure SYNAPSE varlıklarını Azure purview 'da kaydetme ve tarama](../../purview/register-scan-azure-synapse-analytics.md)

[Azure purview kullanarak SYNAPSE 'de verileri bulma, bağlama ve keşfetme](how-to-discover-connect-analyze-azure-purview.md)   