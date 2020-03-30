---
title: Microsoft Tehdit Modelleme Aracı sürümü 02/11/2020 - Azure
description: Tehdit modelleme aracı için sürüm notlarını belgeleme
author: jegeib
ms.author: jegeib
ms.service: security
ms.topic: article
ms.date: 02/25/2020
ms.openlocfilehash: 354707aec90375b4bf25aea6e1baa743d85f20aa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77624845"
---
# <a name="threat-modeling-tool-update-release-73002061---02112020"></a>Tehdit Modelleme Aracı güncelleme sürümü 7.3.00206.1 - 02/11/2020

Microsoft Tehdit Modelleme Aracı'nın (TMT) 7.3.00206.1 sürümü 11 Şubat 2020'de yayımlanmış ve aşağıdaki değişiklikleri içermektedir:

- Hata düzeltmeleri

## <a name="notable-bug-fixes"></a>Önemli hata düzeltmeleri

### <a name="errors-related-to-priority-values-outside-of-the-expected-ranges"></a>Beklenen aralıkların dışındaki öncelik değerleriyle ilgili hatalar

Bazı müşteriler "Tehdit Modelleme Aracı 2016"da oluşturulan dosyaları açarken veya özel şablonlarda aşağıdaki hata iletisini aldığını bildirmişti:

    System.InvalidOperationException: Invalid Priority value. Accepted values are [0..4] and 'High', 'Medium', 'Low' at ThreatModeling.Model.Threat.get_Priority()
    
    System.ArgumentOutOfRangeException: Accepted values are 'High', 'Medium', and 'Low' Parameter name: value Actual value was 5.6. at ThreatModeling.Model.Threat.set_Priority(String value)

Bu sorun bu sürümde çözüldü.

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
