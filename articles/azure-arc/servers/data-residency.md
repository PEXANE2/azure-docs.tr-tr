---
title: Veri yerleşimi
description: Veri yerleşimi ve Azure Arc etkin sunucularla ilgili bilgiler (Önizleme).
ms.topic: reference
ms.date: 08/25/2020
ms.custom: references_regions
ms.openlocfilehash: 028398c27dde9760192d366b9e8a91dca8e429cf
ms.sourcegitcommit: b33c9ad17598d7e4d66fe11d511daa78b4b8b330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88861223"
---
# <a name="azure-arc-enabled-servers-preview-data-residency"></a>Azure Arc etkin sunucular (Önizleme): veri yerleşimi

Bu makalede, veri yerleşimi kavramı ve Azure Arc özellikli sunucular (Önizleme) için nasıl uygulandığı açıklanmaktadır.

Azure yay özellikli sunucular (Önizleme) **Birleşik Devletler, Avrupa veya Asya Pasifik** **[Önizleme sürümünde kullanılabilir](https://azure.microsoft.com/global-infrastructure/services/?products=azure-arc)** .

## <a name="data-residency"></a>Veri yerleşimi

Azure Arc etkin sunucular (Önizleme) [Azure VM uzantı](manage-vm-extensions.md) yapılandırma ayarlarını (diğer bir deyişle, özellik değerlerini) depolama, bağlı makinede etkinleştirmeyi denemeden önce Bu uzantının belirtilmesini gerektirir. Örneğin, Log Analytics VM uzantısını etkinleştirdiğinizde, Log Analytics **çalışma alanı kimliği** ve **birincil anahtar**ister.

Bağlı makine ile ilgili meta veri bilgileri de toplanır. Özellikle:

* İşletim sistemi adı ve sürümü
* Bilgisayar adı
* Bilgisayar tam etki alanı adı (FQDN)
* Bağlı makine Aracısı sürümü

Yay etkin sunucular (Önizleme), verilerinizin depolanacağı bölgeyi belirtmenize olanak tanır. Microsoft, veri dayanıklılığı için diğer bölgelere çoğaltılabilir, ancak Microsoft, Coğrafya dışında verileri çoğaltmaz veya taşımaz. Bu veriler, Azure Arc makinesi kaynağının yapılandırıldığı bölgede depolanır. Örneğin, makine Doğu ABD bölgesinde yaya kayıtlıysa, bu veriler ABD bölgesinde saklanır.

Bölgesel dayanıklılık ve uyumluluk desteğiniz hakkında daha fazla bilgi için bkz. [Azure Coğrafya](https://azure.microsoft.com/global-infrastructure/geographies/).

## <a name="next-steps"></a>Sonraki adımlar

[Azure dayanıklılığı](/architecture/reliability/architect)için tasarlama hakkında daha fazla bilgi edinin.