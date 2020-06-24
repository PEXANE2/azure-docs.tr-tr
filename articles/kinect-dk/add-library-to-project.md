---
title: Visual Studio projenize Azure Kinect kitaplığı ekleme
description: Azure Kinect NuGet paketini Visual Studio projenize eklemeyi öğrenin.
author: wes-b
ms.author: wesbarc
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Kinect, Azure, algılayıcı, SDK, Visual Studio 2017, Visual Studio 2019, NuGet
ms.openlocfilehash: b0395118481cbaecd5ad0b6a3a6b3e89cc29dfaf
ms.sourcegitcommit: fe6c9a35e75da8a0ec8cea979f9dec81ce308c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "85277676"
---
# <a name="add-azure-kinect-library-to-your-visual-studio-project"></a>Visual Studio projenize Azure Kinect kitaplığı ekleme

Bu makalede, Visual Studio projenize Azure Kinect NuGet paketi ekleme işleminde size yol gösterilir.

## <a name="install-azure-kinect-nuget-package"></a>Azure Kinect NuGet paketini yükler

Azure Kinect NuGet paketini yüklemek için:

1. Visual Studio 'da bir NuGet paketini yüklemeye yönelik ayrıntılı yönergeleri hızlı başlangıçta bulabilirsiniz [: Visual Studio 'da bir paket yükleme ve kullanma](https://docs.microsoft.com/nuget/quickstart/install-and-use-a-package-in-visual-studio).
2. Paketi eklemek için, başvurular ' a sağ tıklayıp, Çözüm Gezgini NuGet Paketlerini Yönet ' i seçerek Paket Yöneticisi Kullanıcı arabirimini kullanabilirsiniz.
3. Paket kaynağı olarak [NuGet.org](https://www.nuget.org) öğesini seçin, gözatıp sekmesini seçin ve arama yapın `Microsoft.Azure.Kinect.Sensor` .
4. Listeden bu paketi seçin ve yüklemeyi yapın.

## <a name="use-azure-kinect-nuget-package"></a>Azure Kinect NuGet paketini kullanma

Paket eklendikten sonra, kaynak koduna aşağıdakiler gibi bir başlık dosyası ekleyin:

- `#include <k4a/k4a.h>`
- `#include <k4arecord/record.h>`
- `#include <k4arecord/playback.h>`

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
>[Şimdi ilk uygulamanızı oluşturmaya hazırsınız](build-first-app.md)
