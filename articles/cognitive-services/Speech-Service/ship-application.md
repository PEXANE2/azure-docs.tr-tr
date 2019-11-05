---
title: Konuşma SDK 'Sı ile uygulama geliştirme-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma SDK 'sını kullanarak uygulama oluşturmayı öğrenin.
services: cognitive-services
author: jhakulin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/23/2019
ms.author: jhakulin
ms.custom: seodec18
ms.openlocfilehash: 166ae00085f07ef24d746b60947a31e7680a0f00
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73491010"
---
# <a name="ship-an-application"></a>Uygulama teslim etme

Azure bilişsel hizmetler konuşma SDK 'sını dağıtırken [konuşma SDK lisansının](https://aka.ms/csspeech/license201809)yanı sıra [üçüncü taraf yazılım bildirimlerini](https://csspeechstorage.blob.core.windows.net/drop/1.0.0/ThirdPartyNotices.html) gözlemleyin. Ayrıca, [Microsoft gizlilik bildirimi](https://aka.ms/csspeech/privacy)' ni gözden geçirin.

Platforma bağlı olarak, uygulamanızı yürütmek için farklı bağımlılıklar vardır.

## <a name="windows"></a>Windows

Bilişsel hizmetler konuşma SDK 'Sı Windows 10 ve Windows Server 2016 ' de test edilmiştir.

Bilişsel hizmetler konuşma SDK 'Sı, sistemde [Visual C++ Studio 2019 için Microsoft Visual Redistributable](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) gerektirir. `Microsoft Visual C++ Redistributable for Visual Studio 2019` en son sürümüne yönelik yükleyicileri buradan indirebilirsiniz:

- [Win](https://aka.ms/vs/16/release/vc_redist.x86.exe)
- [x64](https://aka.ms/vs/16/release/vc_redist.x64.exe)

Uygulamanız yönetilen kod kullanıyorsa, hedef makinede `.NET Framework 4.6.1` veya üzeri gereklidir.

Mikrofon girişi için Medya Altyapısı kitaplıklarının yüklenmesi gerekir. Bu kitaplıklar Windows 10 ve Windows Server 2016 ' in bir parçasıdır. Ses giriş cihazı olarak bir mikrofon kullanıldığı sürece, bu kitaplıklar olmadan konuşma SDK 'sını kullanmak mümkündür.

Gerekli konuşma SDK dosyaları, uygulamanızla aynı dizine dağıtılabilir. Bu şekilde, uygulamanız kitaplıklara doğrudan erişim sağlayabilir. Uygulamanızla eşleşen doğru sürümü (Win32/x64) seçtiğinizden emin olun.

| Ad | İşlev
|:-----|:----|
| `Microsoft.CognitiveServices.Speech.core.dll` | Yerel ve yönetilen dağıtım için gerekli olan temel SDK
| `Microsoft.CognitiveServices.Speech.csharp.dll` | Yönetilen dağıtım için gerekli

>[!NOTE]
> Sürümden itibaren 1.3.0 (önceki sürümlerde sevk edilen) dosya `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` artık gerekli değildir. İşlevselliği artık çekirdek SDK 'sında tümleşiktir.

>[!NOTE]
> Windows Forms App (.NET Framework) C# projesi için, kitaplıkların projenizin dağıtım ayarlarına eklendiğinden emin olun. `Properties -> Publish Section`altında bunu kontrol edebilirsiniz. `Application Files` düğmesine tıklayın ve aşağı kaydırma listesinden karşılık gelen kitaplıkları bulun. Değerin `Included`olarak ayarlandığından emin olun. Proje yayımlandığında/dağıtıldığında, Visual Studio dosyayı içerecektir.

## <a name="linux"></a>Linux

Konuşma SDK 'Sı Şu anda Ubuntu 16,04, Ubuntu 18,04 ve demı 9 dağıtımlarını desteklemektedir.
Yerel bir uygulama için, `libMicrosoft.CognitiveServices.Speech.core.so`konuşma SDK Kitaplığı 'nı teslim etmeniz gerekir.
Uygulamanızla eşleşen sürümü (x86, x64) seçtiğinizden emin olun. Linux sürümüne bağlı olarak, aşağıdaki bağımlılıkları da eklemeniz gerekebilir:

* GNU C kitaplığının paylaşılan kitaplıkları (POSIX Iş parçacıkları programlama kitaplığı ile birlikte `libpthreads`)
* OpenSSL kitaplığı (`libssl.so.1.0.0` veya `libssl.so.1.0.2`)
* ALSA uygulamaları için paylaşılan kitaplık (`libasound.so.2`)

Ubuntu 'da, GNU C kitaplıklarının varsayılan olarak zaten yüklü olması gerekir. Son üç, şu komutlar kullanılarak yüklenebilir:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.0 libasound2
```

DEYON 9 ' da şu paketleri yükler:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.2 libasound2
```

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
* [Bkz. konuşmayı tanımaC#](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnet)
