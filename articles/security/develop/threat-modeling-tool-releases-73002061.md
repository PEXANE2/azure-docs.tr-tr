---
title: Microsoft Threat Modeling Tool Release 02/11/2020-Azure
description: Tehdit modelleme aracı için sürüm notlarını belgeleme
author: jegeib
ms.author: jegeib
ms.service: security
ms.topic: article
ms.date: 02/25/2020
ms.openlocfilehash: 354707aec90375b4bf25aea6e1baa743d85f20aa
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77624845"
---
# <a name="threat-modeling-tool-update-release-73002061---02112020"></a>Threat Modeling Tool güncelleştirme sürümü 7.3.00206.1-02/11/2020

Microsoft Threat Modeling Tool (TMT) sürüm 7.3.00206.1, Şubat 11 2020 ' de yayımlanmıştır ve aşağıdaki değişiklikleri içerir:

- Hata düzeltmeleri

## <a name="notable-bug-fixes"></a>Notable hata düzeltmeleri

### <a name="errors-related-to-priority-values-outside-of-the-expected-ranges"></a>Beklenen aralıkların dışındaki öncelik değerleriyle ilgili hatalar

Bazı müşteriler, "Threat Modeling Tool 2016" veya özel şablonlarda oluşturulan dosyaları açarken aşağıdaki hata iletisini almayı raporladı:

    System.InvalidOperationException: Invalid Priority value. Accepted values are [0..4] and 'High', 'Medium', 'Low' at ThreatModeling.Model.Threat.get_Priority()
    
    System.ArgumentOutOfRangeException: Accepted values are 'High', 'Medium', and 'Low' Parameter name: value Actual value was 5.6. at ThreatModeling.Model.Threat.set_Priority(String value)

Bu sorun bu sürümde çözüldü.

## <a name="system-requirements"></a>Sistem Gereksinimleri

- Desteklenen İşletim Sistemleri
  - [Microsoft Windows 10 yıldönümü güncelleştirmesi](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) veya üzeri
- .NET sürümü gerekli
  - [.NET 4.7.1](https://go.microsoft.com/fwlink/?LinkId=863262) veya üzeri
- Ek Gereksinimler
  - Araç ve şablonların güncelleştirmelerini almak için bir Internet bağlantısı gerekir.

## <a name="documentation-and-feedback"></a>Belgeler ve geri bildirim

- Threat Modeling Tool belgeleri [docs.Microsoft.com](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool)konumunda bulunur ve [Aracı kullanma hakkında](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-getting-started)bilgiler içerir.

## <a name="next-steps"></a>Sonraki adımlar

[Microsoft Threat Modeling Tool](https://aka.ms/threatmodelingtool)en son sürümünü indirin.
