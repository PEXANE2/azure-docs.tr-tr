---
title: Microsoft Threat Modeling Tool Release 07/29/2020-Azure
description: Tehdit modelleme aracı sürümü 7.3.00729.1 için sürüm notlarını belgeleme.
author: jegeib
ms.author: jegeib
ms.service: security
ms.topic: article
ms.date: 07/29/2020
ms.openlocfilehash: 3e6fcd52ad9cb6c127c14bac2f33223fb921519e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "94516382"
---
# <a name="threat-modeling-tool-update-release-73007291---07292020"></a>Threat Modeling Tool güncelleştirme sürümü 7.3.00729.1-07/29/2020

Microsoft Threat Modeling Tool (TMT) sürümü 7.3.00729.1, Temmuz 29 2020 ' de yayımlanmıştır ve aşağıdaki değişiklikleri içerir:

- Hata düzeltmeleri
 
## <a name="known-issues"></a>Bilinen sorunlar

### <a name="errors-related-to-tmt7application-file-deserialization"></a>TMT7 ile ilgili hatalar. uygulama dosyası serisini kaldırma

#### <a name="issue"></a>Sorun

Bazı müşteriler Threat Modeling Tool karşıdan yüklerken aşağıdaki hata iletisini almayı raporladı:

```
The threat model file '$PATH\TMT7.application' could not be deserialized. File is not an actual threat model or the threat model may be corrupted.
```

Bu hata, bazı tarayıcıların ClickOnce yüklemesini yerel olarak desteklemediği için oluşur. Bu durumlarda, ClickOnce uygulama dosyası kullanıcının sabit sürücüsüne indirilir.

#### <a name="workaround"></a>Geçici çözüm

Bu hata, TMT7. Application dosyasına çift tıklanarak Threat Modeling Tool başlatıldığında görünmeye devam edecektir. Bununla birlikte, araç normal olarak işlev görür. TMT7. Application dosyasına çift tıklayarak Threat Modeling Tool başlatmak yerine, kullanıcılar, yükleme sırasında Windows menüsünde oluşturulan kısayolları Threat Modeling Tool başlatmak üzere kullanmalıdır.

## <a name="system-requirements"></a>Sistem Gereksinimleri

- Desteklenen İşletim Sistemleri
  - [Microsoft Windows 10 yıldönümü güncelleştirmesi](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) veya üzeri
- .NET sürümü gerekli
  - [.NET 4.7.1](https://go.microsoft.com/fwlink/?LinkId=863262) veya üzeri
- Ek Gereksinimler
  - Araç ve şablonların güncelleştirmelerini almak için bir Internet bağlantısı gerekir.

## <a name="documentation-and-feedback"></a>Belgeler ve geri bildirim

- Threat Modeling Tool belgeleri [docs.Microsoft.com](./threat-modeling-tool.md)konumunda bulunur ve [Aracı kullanma hakkında](./threat-modeling-tool-getting-started.md)bilgiler içerir.

## <a name="next-steps"></a>Sonraki adımlar

[Microsoft Threat Modeling Tool](https://aka.ms/threatmodelingtool)en son sürümünü indirin.