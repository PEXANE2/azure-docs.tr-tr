---
title: Sınırlamalar
description: SDK özellikleri için kod sınırlamaları
author: erscorms
ms.author: erscor
ms.date: 02/11/2020
ms.topic: reference
ms.openlocfilehash: 33f5314c80dc33dbec50dc21a71f4cb507979e12
ms.sourcegitcommit: 0dcafc8436a0fe3ba12cb82384d6b69c9a6b9536
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94427437"
---
# <a name="limitations"></a>Sınırlamalar

Birçok özellik boyut, sayı veya diğer sınırlamalara sahiptir.

## <a name="azure-frontend"></a>Azure ön ucu

Ön uç API 'SI için aşağıdaki sınırlamalar geçerlidir (C++ ve C#):
* İşlem başına toplam [Azurefrontend](/dotnet/api/microsoft.azure.remoterendering.azurefrontend) örneği: 16.
* [Azurefrontend](/dotnet/api/microsoft.azure.remoterendering.azurefrontend)başına toplam [AzureSession](/dotnet/api/microsoft.azure.remoterendering.azuresession) örneği: 16.

## <a name="objects"></a>Nesneler

* Tek bir türdeki izin verilen toplam nesne ([Entity](../concepts/entities.md), [CutPlaneComponent](../overview/features/cut-planes.md), vb.): 16.777.215.
* İzin verilen toplam etkin kesme düzlemleri: 8.

## <a name="geometry"></a>Geometri

* **Animasyon:** Animasyonlar, [oyun nesnelerinin](../concepts/entities.md)bireysel dönüştürmelerini hareketlendirmek ile sınırlıdır. Kaplama veya köşe animasyonları olan iskelet animasyonları desteklenmez. Kaynak varlık dosyasından animasyon parçaları korunmaz. Bunun yerine, nesne dönüştürme animasyonlarını istemci kodu tarafından yönlendirilmelidir.
* **Özel gölgelendiriciler:** Özel gölgelendiriciler yazma desteklenmiyor. Yalnızca yerleşik [renk malzemeleri](../overview/features/color-materials.md) veya [PBR malzemeleri](../overview/features/pbr-materials.md) kullanılabilir.
* Bir varlığın **en fazla benzersiz malzeme sayısı** : 65.535. Otomatik malzeme sayısı azaltma hakkında daha fazla bilgi için bkz. [malzeme çoğaltmayı](../how-tos/conversion/configure-model-conversion.md#material-de-duplication) kaldırma bölümü.
* **Tek bir dokunun en büyük boyutu** : 16.384 x 16.384. Daha büyük kaynak dokuları, dönüştürme işlemi tarafından boyut olarak azaltılır.

### <a name="overall-number-of-polygons"></a>Toplam poligonu sayısı

Tüm yüklü modeller için izin verilen sayıda poligon, [oturum yönetim REST API](../how-tos/session-rest-api.md#create-a-session)geçirilen sanal makinenin boyutuna bağlıdır:

| Sunucu boyutu | Maksimum çokgen sayısı |
|:--------|:------------------|
|Stand| 20.000.000 |
|Premium| sınır yok |

Bu kısıtlamayla ilgili ayrıntılı bilgi için [sunucu boyutu](../reference/vm-sizes.md) bölümüne bakın.

## <a name="platform-limitations"></a>Platform sınırlamaları

**Windows 10 Masaüstü**

* Win32/x64 desteklenen tek Win32 platformudur. Win32/x86 desteklenmez.

**HoloLens 2**

* [BD kamerasından işleme](/windows/mixed-reality/mixed-reality-capture-for-developers#render-from-the-pv-camera-opt-in) özelliği desteklenmiyor.