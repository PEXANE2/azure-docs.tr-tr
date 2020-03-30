---
title: Microsoft Tehdit Modelleme Aracı sürümü 16/10/2019 - Azure
description: Tehdit modelleme aracı için sürüm notlarını belgeleme
author: jegeib
ms.author: jegeib
ms.service: security
ms.topic: article
ms.date: 10/16/2019
ms.openlocfilehash: 452b44653775a1bcb9456b62e1587b5ff2dff874
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75552058"
---
# <a name="threat-modeling-tool-update-release-71610151---10162019"></a>Threat Modeling Tool güncelleştirme sürümü 7.1.61015.1 - 16.10.2019

Microsoft Tehdit Modelleme Aracı'nın (TMT) 7.1.61015.1 sürümü 16 Ekim 2019'da yayımlanmış ve aşağıdaki değişiklikleri içermektedir:

- Erişilebilirlik geliştirmeleri
- Hata düzeltmeleri
- Azure Mantık Uygulamaları ve Azure Veri Gezgini için yeni şablonlar

## <a name="notable-bug-fixes"></a>Önemli hata düzeltmeleri

### <a name="improved-backward-compatibility-with-files-created-in-threat-modeling-tool-2016"></a>"Threat Modeling Tool 2016"da oluşturulan dosyalarla geriye dönük uyumluluğu artırdı

"Threat Modeling Tool 2016"da oluşturulan tehdit modeli dosyalarının açılması veya görüntülenmesiyle ilgili birçok hata düzeltildi.

## <a name="feature-enhancements"></a>Özellik geliştirmeleri

### <a name="new-stencils-for-azure-logic-apps-and-azure-data-explorer"></a>Azure Mantık Uygulamaları ve Azure Veri Gezgini için yeni şablonlar

Azure Mantık Uygulamaları ve Azure Veri Gezgini için yeni şablonlar, ilişkili tehdit ve azaltıcı etkenlerle birlikte Azure Şablonu'na eklendi.

![Azure Mantık Uygulamaları ve Azure Veri Gezgini Şablonları](./media/threat-modeling-tool-releases-71610151/tmt-logic-apps.png)

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="errors-related-to-priority-values-outside-of-the-expected-ranges"></a>Beklenen aralıkların dışındaki öncelik değerleriyle ilgili hatalar

Bazı müşteriler "Tehdit Modelleme Aracı 2016"da oluşturulan dosyaları açarken veya özel şablonlarda aşağıdaki hata iletisini aldığını bildirmiştir:

    System.InvalidOperationException: Invalid Priority value. Accepted values are [0..4] and 'High', 'Medium', 'Low' at ThreatModeling.Model.Threat.get_Priority()
    
    System.ArgumentOutOfRangeException: Accepted values are 'High', 'Medium', and 'Low' Parameter name: value Actual value was 5.6. at ThreatModeling.Model.Threat.set_Priority(String value)

Bu konu araştırılıyor

## <a name="system-requirements"></a>Sistem gereksinimleri

- Desteklenen İşletim Sistemleri
  - [Microsoft Windows 10 Yıldönümü Güncelleştirmesi](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) veya sonrası
- .NET Sürümü Gerekli
  - [.Net 4.7.1](https://go.microsoft.com/fwlink/?LinkId=863262) veya sonrası
- Ek Gereksinimler
  - Şablonların yanı sıra araçla ilgili güncelleştirmeleri de almak için Internet bağlantısı gereklidir.

## <a name="documentation-and-feedback"></a>Dokümantasyon ve geri bildirim

- Tehdit Modelleme Aracı için Dokümantasyon [docs.microsoft.com](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool)üzerinde yer alır ve [aracı kullanma hakkında](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-getting-started)bilgi içerir.

## <a name="next-steps"></a>Sonraki adımlar

Microsoft Tehdit Modelleme Aracı'nın en son sürümünü [indirin.](https://aka.ms/threatmodelingtool)
