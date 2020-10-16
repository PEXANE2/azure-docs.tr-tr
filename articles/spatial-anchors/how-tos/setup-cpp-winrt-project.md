---
title: C++/Wınrt HoloLens uygulaması için Azure uzamsal bağlayıcıları 'nı yükler
description: Azure uzamsal bağlayıcıları kullanmak için bir C++/Wınrt HoloLens projesi yapılandırma
author: craigktreasure
manager: vriveras
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: how-to
ms.service: azure-spatial-anchors
ms.openlocfilehash: 43d5c1ae03c359dcbef21f8e7ba3205bc6ab0004
ms.sourcegitcommit: 93329b2fcdb9b4091dbd632ee031801f74beb05b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92096122"
---
# <a name="configuring-azure-spatial-anchors-in-a-cwinrt-hololens-project"></a>C++/Wınrt HoloLens projesindeki Azure uzamsal bağlayıcılarını yapılandırma

## <a name="requirements"></a>Gereksinimler

* [Visual Studio 2019](https://www.visualstudio.com/downloads/) **Evrensel Windows platformu geliştirme** iş yükü ve **Windows 10 SDK (10.0.18362.0 veya daha yeni)** bileşeniyle yüklendi.

## <a name="configuring-a-project"></a>Proje yapılandırma

HoloLens ve C++/Wınrt için Azure uzamsal bağlantıları, [Microsoft. Azure. SpatialAnchors. WinRT](https://www.nuget.org/packages/Microsoft.Azure.SpatialAnchors.WinRT/) NuGet paketi kullanılarak dağıtılır.

Visual Studio 'nun NuGet Paket Yöneticisi 'Ni kullanarak projenize [Microsoft. Azure. SpatialAnchors. WinRT](https://www.nuget.org/packages/Microsoft.Azure.SpatialAnchors.WinRT/) NuGet paketini yüklemek için [buradaki](/nuget/consume-packages/install-use-packages-visual-studio) yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Nasıl yapılır: C++/Wınrt içinde bağlayıcıları oluşturma ve bulma](./create-locate-anchors-cpp-winrt.md)