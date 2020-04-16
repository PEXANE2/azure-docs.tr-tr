---
title: Sınırlamalar
description: SDK özellikleri için kod sınırlamaları
author: erscorms
ms.author: erscor
ms.date: 02/11/2020
ms.topic: reference
ms.openlocfilehash: b6e501cdc1d5091a27d86406c6919587bedb261a
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81417700"
---
# <a name="limitations"></a>Sınırlamalar

Bazı özelliklerin boyutu, sayısı veya diğer sınırlamaları vardır.

## <a name="azure-frontend"></a>Azure Ön Uç

* İşlem başına toplam AzureFrontend örnekleri: 16.
* AzureFrontend başına toplam AzureSession örnekleri: 16.

## <a name="objects"></a>Nesneler

* Tek bir türde izin verilebilen toplam nesneler (Varlık, CutPlaneComponent, vb.): 16.777.215.
* İzin verilebilen toplam aktif kesme düzlemi: 8.

## <a name="materials"></a>Malzemeler

* Bir varlıkta izin verilebilen toplam malzeme sayısı: 65.535.

## <a name="overall-number-of-polygons"></a>Toplam çokgen sayısı

Tüm yüklenen modeller için izin verilebilen çokgen [sayısı, oturum yönetimi REST API'ye](../how-tos/session-rest-api.md#create-a-session)geçildiği gibi VM'nin boyutuna bağlıdır:

| VM boyutu | Maksimum çokgen sayısı |
|:--------|:------------------|
|Standart| 20 milyon |
|Premium| limitsiz |


## <a name="platform-limitations"></a>Platform sınırlamaları

**Windows 10 masaüstü**

* Unity'den "PC Bağımsız" dağıtımı desteklenmez. Bunun yerine UWP kullanın.
* UWP/x86 desteklenen tek UWP platformudur. UWP/x64 desteklenmez.

**Hololens 2**

* [PV kamera özelliğinden alınan görüntü](https://docs.microsoft.com/windows/mixed-reality/mixed-reality-capture-for-developers#render-from-the-pv-camera-opt-in) desteklenmez.
