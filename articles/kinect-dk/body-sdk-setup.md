---
title: Hızlı başlangıç-Azure Kinect gövdesi izlemeyi ayarlama
description: Bu hızlı başlangıçta, Azure Kinect için gövde izleme SDK 'sını ayarlayacaksınız.
author: qm13
ms.author: quentinm
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: quickstart
keywords: Kinect, Azure, algılayıcı, erişim, derinlik, SDK, gövde, izleme, birleşme, kurulum, CUDA, NVIDIA
ms.openlocfilehash: 2cf4c1097730f88fc4bd66c28e1bdddd7fea8640
ms.sourcegitcommit: fe6c9a35e75da8a0ec8cea979f9dec81ce308c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "85278003"
---
# <a name="quickstart-set-up-azure-kinect-body-tracking"></a>Hızlı başlangıç: Azure Kinect gövdesi izlemeyi ayarlama

Bu hızlı başlangıç, Azure Kinect DK 'de çalışan gövde izlemeyi alma sürecinde size kılavuzluk eder.

## <a name="system-requirements"></a>Sistem gereksinimleri

Gövde Izleme SDK 'Sı, ana bılgısayarda yüklü bir NVıDıA GPU gerektirir. Önerilen gövde izleme ana bilgisayar gereksinimi, [sistem gereksinimleri](system-requirements.md) sayfasında açıklanmaktadır.

## <a name="install-software"></a>Yazılımı yükleme

### <a name="install-the-latest-nvidia-driver"></a>[En son NVıDıA sürücüsünü yükler](https://www.nvidia.com/Download/index.aspx?lang=en-us)

Grafik kartınız için en son NVıDıA sürücüsünü indirin ve yükleyin. Eski sürücüler CUDA ikilileri, gövde izleme SDK 'Sı ile yeniden dağıtılabilir.

### <a name="visual-c-redistributable-for-visual-studio-2015"></a>[Visual C++ Redistributable for Visual Studio 2015](https://www.microsoft.com/en-us/download/details.aspx?id=48145)

Visual Studio için Visual C++ Yeniden Dağıtılabilir 2015 indirin ve yükleyin. 

## <a name="set-up-hardware"></a>Donanım ayarlama

### <a name="set-up-azure-kinect-dk"></a>[Azure Kinect DK 'yi ayarlama](set-up-azure-kinect-dk.md)

Azure Kinect DK 'nin doğru şekilde ayarlandığından emin olmak için [Azure Kinect görüntüleyicisini](azure-kinect-viewer.md) başlatın.

## <a name="download-the-body-tracking-sdk"></a>Gövde Izleme SDK 'sını indirin
 
1. [Gövde izleme SDK 'Sını indirmek](body-sdk-download.md) için bağlantıyı seçin
2. Bilgisayarınıza gövde Izleme SDK 'sını yükler.

## <a name="verify-body-tracking"></a>Gövde izlemeyi doğrula

Gövde Izleme SDK 'sının doğru şekilde ayarlandığından emin olmak için **Azure Kinect gövde Izleme görüntüleyicisini** başlatın. Görüntüleyici, SDK MSI Yükleyicisi ile birlikte yüklenir. Bunu başlangıç menüsünde veya adresinde bulabilirsiniz `<SDK Installation Path>\tools\k4abt_simple_3d_viewer.exe` .

Yeterince güçlü bir GPU yoksa ve yine de sonucu test etmek istiyorsanız, komut satırında aşağıdaki komutla **Azure Kinect gövde Izleme görüntüleyicisini** başlatabilirsiniz:`<SDK Installation Path>\tools\k4abt_simple_3d_viewer.exe CPU`

Her şey doğru şekilde ayarlandıysa, 3B nokta bulutu ve izlenen gövdeler içeren bir pencere görünmelidir.


![Gövde Izleme 3B görüntüleyici](./media/quickstarts/samples-simple3dviewer.png)

## <a name="examples"></a>Örnekler

Gövde izleme SDK 'sının nasıl kullanılacağına ilişkin örnekleri [burada](https://github.com/microsoft/Azure-Kinect-Samples/tree/master/body-tracking-samples)bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
>[İlk gövde izleme uygulamanızı oluşturma](build-first-body-app.md)

