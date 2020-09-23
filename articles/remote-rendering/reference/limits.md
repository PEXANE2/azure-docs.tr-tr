---
title: Sınırlamalar
description: SDK özellikleri için kod sınırlamaları
author: erscorms
ms.author: erscor
ms.date: 02/11/2020
ms.topic: reference
ms.openlocfilehash: e6b12c2bac4a9732f868f6a6ac3491ef993f54c3
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90976559"
---
# <a name="limitations"></a>Sınırlamalar

Birçok özellik boyut, sayı veya diğer sınırlamalara sahiptir.

## <a name="azure-frontend"></a>Azure ön ucu

* İşlem başına toplam AzureFrontend örneği: 16.
* AzureFrontend başına toplam AzureSession örneği: 16.

## <a name="objects"></a>Nesneler

* Tek bir türdeki izin verilen toplam nesne (Entity, CutPlaneComponent, vb.): 16.777.215.
* İzin verilen toplam etkin kesme düzlemleri: 8.

## <a name="geometry"></a>Geometri

* Bir varlık için izin verilen toplam malzeme: 65.535. Daha fazla bilgi için bkz. [materyalde çoğaltma](../how-tos/conversion/configure-model-conversion.md#material-de-duplication) .
* Tek bir dokunun en büyük boyutu: 16.384 x 16.384. Daha büyük kaynak dokuları, dönüştürme işlemi tarafından aşağı ölçeklenacaktır.

## <a name="overall-number-of-polygons"></a>Toplam poligonu sayısı

Tüm yüklü modeller için izin verilen sayıda poligon, [oturum yönetim REST API](../how-tos/session-rest-api.md#create-a-session)geçirilen sanal makinenin boyutuna bağlıdır:

| Sunucu boyutu | Maksimum çokgen sayısı |
|:--------|:------------------|
|Stand| 20.000.000 |
|Premium| sınır yok |

Bu sınırlama hakkında daha ayrıntılı bilgi için bkz. [sunucu boyutu](../reference/vm-sizes.md) bölümü.

## <a name="platform-limitations"></a>Platform sınırlamaları

**Windows 10 Masaüstü**

* Win32/x64 desteklenen tek Win32 platformudur. Win32/x86 desteklenmez.

**HoloLens 2**

* [BD kamerasından işleme](https://docs.microsoft.com/windows/mixed-reality/mixed-reality-capture-for-developers#render-from-the-pv-camera-opt-in) özelliği desteklenmiyor.
