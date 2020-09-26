---
title: Veri yerleşimi
description: Veri yerleşimi ve Azure Arc etkin sunucuları hakkında bilgiler.
ms.topic: reference
ms.date: 09/22/2020
ms.custom: references_regions
ms.openlocfilehash: 73acb07b8caa3c1368c936463f04969c29d37985
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91327774"
---
# <a name="azure-arc-enabled-servers-data-residency"></a>Azure Arc etkin sunucular: veri yerleşimi

Bu makalede, veri yerleşimi kavramı ve Azure Arc özellikli sunucular için nasıl uygulandığı açıklanmaktadır.

Azure yay özellikli sunucular **Birleşik Devletler, Avrupa, Birleşik Krallık veya Asya Pasifik** **[kullanılabilir](https://azure.microsoft.com/global-infrastructure/services/?products=azure-arc)** .

## <a name="data-residency"></a>Veri yerleşimi

Azure Arc etkin sunucular, [Azure VM uzantı](manage-vm-extensions.md) yapılandırma ayarlarını (diğer bir deyişle, özellik değerlerini), uzantının bağlı makinede etkinleştirmeyi denemeden önce belirtilmesini gerektirir. Örneğin, Log Analytics VM uzantısını etkinleştirdiğinizde, Log Analytics **çalışma alanı kimliği** ve **birincil anahtar**ister.

Bağlı makine ile ilgili meta veri bilgileri de toplanır. Özellikle:

* İşletim sistemi adı ve sürümü
* Bilgisayar adı
* Bilgisayar tam etki alanı adı (FQDN)
* Bağlı makine Aracısı sürümü

Yay özellikli sunucular, verilerinizin depolanacağı bölgeyi belirtmenize olanak tanır. Microsoft, veri dayanıklılığı için diğer bölgelere çoğaltılabilir, ancak Microsoft, Coğrafya dışında verileri çoğaltmaz veya taşımaz. Bu veriler, Azure Arc makinesi kaynağının yapılandırıldığı bölgede depolanır. Örneğin, makine Doğu ABD bölgesinde yaya kayıtlıysa, bu veriler ABD bölgesinde saklanır.

Bölgesel dayanıklılık ve uyumluluk desteğiniz hakkında daha fazla bilgi için bkz. [Azure Coğrafya](https://azure.microsoft.com/global-infrastructure/geographies/).

## <a name="next-steps"></a>Sonraki adımlar

[Azure dayanıklılığı](/azure/architecture/reliability/architect)için tasarlama hakkında daha fazla bilgi edinin.