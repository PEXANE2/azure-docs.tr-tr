---
title: Microsoft Tehdit Modelleme Aracı sürümü 09.09.2019
titleSuffix: Azure
description: Tehdit modelleme aracı için sürüm notlarını belgeleme
author: jegeib
ms.author: jegeib
ms.service: security
ms.subservice: security-develop
ms.topic: article
ms.date: 04/03/2019
ms.openlocfilehash: 59d385ba7de5bf7bceae4dc8ddadbca813046094
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78269721"
---
# <a name="threat-modeling-tool-update-release-71604081---492019"></a>Tehdit Modelleme Aracı güncelleme sürümü 7.1.60408.1 - 4/9/2019

Microsoft Tehdit Modelleme Aracı'nın (TMT) 7.1.60408.1 sürümü 9 Nisan 2019'da yayımlanmış ve aşağıdaki değişiklikleri içermektedir:

- Azure Anahtar Kasası ve Azure Trafik Yöneticisi için Yeni Şablonlar
- TMT sürüm numarası artık ana ekranda gösteriliyor
- Destek bağlantıları güncellendi
- Hata düzeltmeleri

## <a name="feature-changes"></a>Özellik değişiklikleri

### <a name="new-stencils-for-azure-key-vault-and-azure-traffic-manager"></a>Azure Anahtar Kasası ve Azure Trafik Yöneticisi için Yeni Şablonlar

![Azure Anahtar Kasa Şablonu](./media/threat-modeling-tool-releases-71604081/tmt_keyvault_trafficmanager.PNG)

Azure şablon setine Azure Anahtar Kasası ve Azure Trafik Yöneticisi için yeni şablonlar ve tehditler eklendi. Azure şablon kümesini temel alan modelleri açarken, kullanıcılardan modelle ilişkili şablonu güncelleştirmeleri istenir. Azure şablon kümesini temel alan bir modeli güncelleştirme, "Dosya" menüsündeki "Şablon Uygula" komutu kullanılarak ve en son Azure Bulut Hizmetleri.tb7 dosyasını yeniden uygulayarak el ile başlatılabilir.

### <a name="tmt-version-number-is-now-shown-on-the-home-screen"></a>TMT sürüm numarası artık ana ekranda gösteriliyor

Tehdit Modelleme Aracı'nın istemci sürümü artık erişim kolaylığı için uygulamanın ana ekranında gösterilmiştir.

![Azure Anahtar Kasa Şablonu](./media/threat-modeling-tool-releases-71604081/tmt_version.PNG)

### <a name="support-links-have-been-updated"></a>Destek bağlantıları güncellendi

Araç içindeki tüm destek bağlantıları, kullanıcıları [tmtextsupport@microsoft.com](mailto:tmtextsupport@microsoft.com) bir MSDN forumu yerine yönlendirmek için güncelleştirildi.

## <a name="system-requirements"></a>Sistem gereksinimleri

- Desteklenen İşletim Sistemleri
  - [Microsoft Windows 10 Yıldönümü Güncelleştirmesi](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) veya sonrası
- .NET Sürümü Gerekli
  - [.Net 4.7.1](https://go.microsoft.com/fwlink/?LinkId=863262) veya sonrası
- Ek Gereksinimler
  - Şablonların yanı sıra araçla ilgili güncelleştirmeleri de almak için Internet bağlantısı gereklidir.

## <a name="documentation-and-feedback"></a>Dokümantasyon ve geri bildirim

- Tehdit Modelleme Aracı için Dokümantasyon [docs.microsoft.com](threat-modeling-tool.md)üzerinde yer alır ve [aracı kullanma hakkında](threat-modeling-tool-getting-started.md)bilgi içerir.

## <a name="next-steps"></a>Sonraki adımlar

Microsoft Tehdit Modelleme Aracı'nın en son sürümünü [indirin.](https://aka.ms/threatmodelingtool)
