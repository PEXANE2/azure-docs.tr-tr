---
title: Microsoft Tehdit Modelleme Aracı sürümü 29/1/2019
titleSuffix: Azure
description: Tehdit modelleme aracı için sürüm notlarını belgeleme
author: jegeib
ms.author: jegeib
ms.service: security
ms.subservice: security-develop
ms.topic: article
ms.date: 01/25/2019
ms.openlocfilehash: 7d0be8d7243331264c10a407e3d78370ea798928
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78269767"
---
# <a name="threat-modeling-tool-update-release-71601261---1292019"></a>Threat Modeling Tool güncelleştirme sürümü 7.1.60126.1 - 29.1.2019

Microsoft Tehdit Modelleme Aracı'nın 7.1.60126.1 sürümü 29 Ocak 2019'da yayımlanmış ve aşağıdaki değişiklikleri içermektedir:

- .NET'in minimum gerekli sürümü [.NET 4.7.1'e](https://go.microsoft.com/fwlink/?LinkId=863262)yükseltilmiştir.
- Windows'un gereken minimum sürümü.NET bağımlılığı nedeniyle [Windows 10 Anniversary Update'e](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) yükseltildi.
- Aracın Seçenekler menüsüne bir model doğrulama geçiş özelliği eklendi.
- Tehdit Özellikleri'ndeki çeşitli bağlantılar güncelleştirildi.
- Küçük UX aracın ana ekranında değişir.
- Tehdit Modelleme Aracı artık ana bilgisayar işletim sisteminin TLS ayarlarını devralır ve TLS 1,2 veya daha büyük gerektiren ortamlarda desteklenir.

## <a name="feature-changes"></a>Özellik değişiklikleri

### <a name="model-validation-option"></a>Model doğrulama seçeneği

Müşteri geri bildirimine bağlı olarak, model doğrulaması etkinleştirmek veya devre dışı bırakma için araca bir seçenek eklendi. Daha önce, şablonunuzun iki nesne arasında tek yönlü tek yönlü veri akışı kullanılışı varsa, İletiler çerçevesinde şu belirten bir hata iletisi almış olabilirsiniz: ObjectsName en az bir 'Any' gerektirir. Model doğrulamayı devre dışı bırakmak, bu uyarıların görünümde gösterilmesini engeller.

Model doğrulamayı açma ve kapama seçeneği Dosya >Ayarlar->Seçenekleri menüsünde bulunabilir. Bu ayar için varsayılan değer Devre Dışı bırakılır.

![Model Doğrulama Seçeneği](./media/threat-modeling-tool-releases-71601261/tmt_model_validation_option.png)

## <a name="system-requirements"></a>Sistem gereksinimleri

- Desteklenen İşletim Sistemleri
  - [Microsoft Windows 10 Yıldönümü Güncelleştirmesi](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) veya sonrası
- .NET Sürümü Gerekli
  - [.NET 4.7.1](https://go.microsoft.com/fwlink/?LinkId=863262) veya sonrası
- Ek Gereksinimler
  - Şablonların yanı sıra araçla ilgili güncelleştirmeleri de almak için Internet bağlantısı gereklidir.

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="unsupported-systems"></a>Desteklenmeyen sistemler

#### <a name="issue"></a>Sorun

.NET 4.7.1 veya daha sonra (Windows 10 Enterprise LTSB (sürüm 1507) gibi yükleyemeyen Windows 10 sistemlerinin kullanıcıları, yükseltmeden sonra aracı açamayacaktır. Windows'un bu eski sürümleri artık Tehdit Modelleme Aracı için desteklenen platformlar değildir ve en son güncelleştirmeyi yüklememelidir.

#### <a name="workaround"></a>Geçici çözüm

En son güncelleştirmeyi yüklemiş olan Windows 10 Enterprise LTSB (sürüm 1507) kullanıcıları, Apps & Özellikler'deki kaldırma iletişim kutusunda tehdit modelleme aracının önceki sürümüne geri dönebilir.

## <a name="documentation-and-feedback"></a>Dokümantasyon ve geri bildirim

- Tehdit Modelleme Aracı için Dokümantasyon [docs.microsoft.com](threat-modeling-tool.md)üzerinde yer alır ve [aracı kullanma hakkında](threat-modeling-tool-getting-started.md)bilgi içerir.

## <a name="next-steps"></a>Sonraki adımlar

Microsoft Tehdit Modelleme Aracı'nın en son sürümünü [indirin.](https://aka.ms/threatmodelingtool)
