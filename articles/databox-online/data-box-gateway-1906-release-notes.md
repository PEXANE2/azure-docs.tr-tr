---
title: Azure Veri Kutusu Ağ Geçidi & Azure Veri Kutusu Kenarı 1906 sürüm notları| Microsoft Dokümanlar
description: 1906 sürümünde çalışan Azure Veri Kutusu Ağ Geçidi ve Azure Veri Kutusu Kenarı için kritik açık sorunları ve çözümleri açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 09/18/2019
ms.author: alkohli
ms.openlocfilehash: 69c905176a5beb1c7d442cd5f1a69161cd520c5a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71099492"
---
# <a name="azure-data-box-edge-and-azure-data-box-gateway-1906-release-notes"></a>Azure Veri Kutusu Kenarı ve Azure Veri Kutusu Ağ Geçidi 1906 sürüm notları

Aşağıdaki sürüm notları, Azure Veri Kutusu Kenarı ve Azure Veri Kutusu Ağ Geçidi için 1906 sürümü için kritik açık sorunları ve çözülmüş sorunları tanımlar.

Sürüm notları sürekli olarak güncelleştirilir ve geçici çözüm gerektiren kritik sorunlar keşfedildikçe eklenir. Veri Kutusu Kenarı/Veri Kutusu Ağ Geçidi'ni dağıtmadan önce, sürüm notlarında yer alan bilgileri dikkatle inceleyin.

Bu sürüm yazılım sürümlerine karşılık gelir:

- **Veri Kutusu Ağ Geçidi 1906 (1.6.978.743)**
- **Veri Kutusu Kenar 1906 (1.6.978.743)**

> [!NOTE]
> Güncelleştirme 1906 yalnızca yazılımın genel kullanılabilirliği (GA) veya 1905 sürümünü çalıştıran Veri Kutusu Kenarı aygıtlarına uygulanabilir.

## <a name="whats-new"></a>Yenilikler

- **Kurtarma anahtarı yönetimi iş akışında hata düzeltmesi** - Önceki sürümde, kurtarma anahtarının uygulanmadığı bir hata vardı. Bu hata bu sürümde düzeltilir. Kurtarma anahtarı, aygıtın önyükleme yapmaması durumunda aygıttaki verileri kurtarmanızı sağladığından, bu güncelleştirmeyi uygulamanızı şiddetle öneririz. Daha fazla bilgi için, [Veri Kutusu Kenarı veya Veri Kutusu Ağ Geçidi'ni dağıtırken kurtarma anahtarını](data-box-edge-deploy-connect-setup-activate.md#set-up-and-activate-the-physical-device)nasıl kaydedebilirsiniz.
- **Alan Programlanabilir Kapı Dizisi (FPGA) günlük iyileştirmeleri** - 1905 sürümü, günlük ve Uyarı geliştirmeleri FPGA ile ilgili yapıldı. FPGA ile Edge bilgi işlem özelliğini kullanıyorsanız, bu Işlem Veri Kutusu Kenarı için gerekli bir güncelleştirme olmaya devam etmektedir. Daha fazla bilgi için, [Veri Kutusu Kenarı'ndaki Edge bilgi işlemle verileri nasıl dönüştürdüğünüze](data-box-edge-deploy-configure-compute-advanced.md)bakın.

## <a name="known-issues-in-ga-release"></a>GA sürümünde bilinen sorunlar

Bu sürüm için yeni bir sorun yayınlanmaz. Tüm sürüm sorunları önceki sürümlerden taşınmıştır kaydetti. Bilinen sorunların listesini görmek [için, GA sürümünde bilinen sorunlara](data-box-gateway-release-notes.md#known-issues-in-ga-release)gidin.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Box Gateway'i dağıtmaya hazırlanma](data-box-gateway-deploy-prep.md)
- [Azure Data Box Edge’i dağıtmaya hazırlanma](data-box-edge-deploy-prep.md)
