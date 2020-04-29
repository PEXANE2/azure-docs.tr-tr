---
title: Sınırlamalar
description: SDK özellikleri için kod sınırlamaları
author: erscorms
ms.author: erscor
ms.date: 02/11/2020
ms.topic: reference
ms.openlocfilehash: b6e501cdc1d5091a27d86406c6919587bedb261a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81417700"
---
# <a name="limitations"></a>Sınırlamalar

Birçok özellik boyut, sayı veya diğer sınırlamalara sahiptir.

## <a name="azure-frontend"></a>Azure ön ucu

* İşlem başına toplam AzureFrontend örneği: 16.
* AzureFrontend başına toplam AzureSession örneği: 16.

## <a name="objects"></a>Nesneler

* Tek bir türdeki izin verilen toplam nesne (Entity, CutPlaneComponent, vb.): 16.777.215.
* İzin verilen toplam etkin kesme düzlemleri: 8.

## <a name="materials"></a>Malzemeler

* Bir varlık için izin verilen toplam malzeme: 65.535.

## <a name="overall-number-of-polygons"></a>Toplam poligonu sayısı

Tüm yüklü modeller için izin verilen sayıda poligon, [oturum yönetim REST API](../how-tos/session-rest-api.md#create-a-session)geçirilen sanal makinenin boyutuna bağlıdır:

| VM boyutu | Maksimum çokgen sayısı |
|:--------|:------------------|
|Stand| 20.000.000 |
|Premium| sınır yok |


## <a name="platform-limitations"></a>Platform sınırlamaları

**Windows 10 Masaüstü**

* Unity 'den "PC tek başına" dağıtımı desteklenmez. Bunun yerine UWP kullanın.
* UWP/x86 yalnızca desteklenen UWP platformudur. UWP/x64 desteklenmez.

**HoloLens 2**

* [BD kamerasından işleme](https://docs.microsoft.com/windows/mixed-reality/mixed-reality-capture-for-developers#render-from-the-pv-camera-opt-in) özelliği desteklenmiyor.
