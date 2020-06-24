---
title: Hızlı başlangıç-Azure Kinect algılayıcı akışlarını bir dosyaya kaydetme
description: Bu hızlı başlangıçta, algılayıcı SDK 'dan bir dosyaya veri akışlarını nasıl kaydedeceğinizi öğreneceksiniz.
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: quickstart
keywords: Azure, Kinect, kayıt, kayıttan yürütme, okuyucu, Matroska, MKV, akışlar, derinlik, RGB, kamera, renk, İmu, ses, algılayıcı
ms.openlocfilehash: 3dab147b593bf012bd6cd9c95d0195e84a2cbcf1
ms.sourcegitcommit: fe6c9a35e75da8a0ec8cea979f9dec81ce308c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "85278000"
---
# <a name="quickstart-record-azure-kinect-sensor-streams-to-a-file"></a>Hızlı başlangıç: Azure Kinect algılayıcı akışlarını bir dosyaya kaydetme

Bu hızlı başlangıçta, algılayıcı SDK 'sindeki veri akışlarını bir dosyaya kaydetmek için [Azure Kinect kaydedici](azure-kinect-recorder.md) aracını nasıl kullanabileceğiniz hakkında bilgi sağlanır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç şunları varsayar:

- Ana BILGISAYARıNıZ için Azure Kinect DK bağlı ve düzgün şekilde güç.
- Donanımı [ayarlamayı](set-up-azure-kinect-dk.md) tamamladınız.

## <a name="create-recording"></a>Kayıt oluştur

1. Bir komut istemi açın ve yüklü Araçlar dizininde bulunan [Azure Kinect Kaydedicisi](azure-kinect-recorder.md)yolunu belirtin `k4arecorder.exe` . Örneğin: `C:\Program Files\Azure Kinect SDK\tools\k4arecorder.exe`.
2. 5 saniye kayıt.

    `k4arecorder.exe -l 5 %TEMP%\output.mkv`

3. Kaydedici, varsayılan olarak NFOV 'nin Bink olmayan derinlik modunu kullanır ve ıMU verileri dahil olmak üzere 30 fps 'de 1080p RGB 'ye çıktı verir. Kayıt seçenekleri ve ipuçları hakkında genel bir bakış için [Azure Kinect Kaydedicisi](azure-kinect-recorder.md)' ne bakın.

## <a name="play-back-recording"></a>Kayıttan yürütme

Bir kaydı kayıttan yürütmek için [Azure Kinect Görüntüleyicisi](azure-kinect-viewer.md) 'ni kullanabilirsiniz.

1. Man[`k4aviewer.exe`](azure-kinect-viewer.md)
2. **Açık kayıt** sekmesinin katlamayı kaldırır ve kaydınızı açın.

## <a name="next-steps"></a>Sonraki adımlar

Algılayıcı akışlarını bir dosyaya nasıl kaydedediğinize artık

> [!div class="nextstepaction"]
> [İlk Azure Kinect uygulamanızı oluşturun](build-first-app.md)
