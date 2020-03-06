---
title: Konuşma SDK 'Sı ile uygulama geliştirme-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Desteklenen platformlarda konuşma SDK 'sını kullanan bir uygulamayı dağıtmayı öğrenin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/30/2020
ms.author: dapine
ms.custom: seodec18
ms.openlocfilehash: 9507428e63b337b3d8419a833d03d081d494c522
ms.sourcegitcommit: 021ccbbd42dea64d45d4129d70fff5148a1759fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78330813"
---
# <a name="ship-an-application"></a>Uygulama teslim etme

Azure bilişsel hizmetler konuşma SDK 'sını dağıtırken [konuşma SDK lisansının](https://aka.ms/csspeech/license201809)yanı sıra [üçüncü taraf yazılım bildirimlerini](https://csspeechstorage.blob.core.windows.net/drop/1.0.0/ThirdPartyNotices.html) gözlemleyin. Ayrıca, [Microsoft gizlilik bildirimi](https://aka.ms/csspeech/privacy)' ni gözden geçirin.

Platforma bağlı olarak, uygulamanızı çalıştırmak için farklı bağımlılıklara mevcut.

## <a name="windows"></a>Windows

Bilişsel hizmetler konuşma SDK'sı, Windows 10 ve Windows Server 2016 üzerinde test edilir.

Bilişsel hizmetler konuşma SDK 'Sı, sistemde [Visual C++ Studio 2019 için Microsoft Visual Redistributable](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) gerektirir. `Microsoft Visual C++ Redistributable for Visual Studio 2019` en son sürümüne yönelik yükleyicileri buradan indirebilirsiniz:

- [Win](https://aka.ms/vs/16/release/vc_redist.x86.exe)
- [x64](https://aka.ms/vs/16/release/vc_redist.x64.exe)

Uygulamanız yönetilen kod kullanıyorsa, hedef makinede `.NET Framework 4.6.1` veya üzeri gereklidir.

Mikrofon girişi için Media Foundation kitaplıkları yüklü olması gerekir. Bu kitaplıklar, Windows 10 ve Windows Server 2016'ya bir parçasıdır. Mikrofon ses giriş cihazını kullanılmayan sürece bu kitaplıklar Speech SDK'sı kullanmak mümkündür.

Gerekli dosyaları Speech SDK'sı, uygulamanızın aynı dizinde dağıtılabilir. Bu şekilde uygulamanızı kitaplıkları doğrudan erişebilirsiniz. Uygulamanızı eşleşen doğru sürümünü (Win32/x64) seçtiğinizden emin olun.

| Adı | İşlev |
| :--- | :------- |
| `Microsoft.CognitiveServices.Speech.core.dll`   | Yerel ve yönetilen dağıtım için gerekli core SDK'sı |
| `Microsoft.CognitiveServices.Speech.csharp.dll` | Yönetilen dağıtım için gerekli                      |

> [!NOTE]
> Sürümden itibaren 1.3.0 (önceki sürümlerde sevk edilen) dosya `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` artık gerekli değildir. İşlevselliği artık çekirdek SDK 'sında tümleşiktir.

> [!NOTE]
> Windows Forms App (.NET Framework) C# projesi için, kitaplıkların projenizin dağıtım ayarlarına eklendiğinden emin olun. `Properties -> Publish Section`altında bunu kontrol edebilirsiniz. `Application Files` düğmesine tıklayın ve aşağı kaydırma listesinden karşılık gelen kitaplıkları bulun. Değerin `Included`olarak ayarlandığından emin olun. Proje yayımlandığında/dağıtıldığında, Visual Studio dosyayı içerecektir.

## <a name="linux"></a>Linux

Konuşma SDK 'Sı Şu anda Ubuntu 16,04, Ubuntu 18,04, de, 9, RHEL 8, CentOS 8 dağıtımlarını desteklemektedir.
Yerel bir uygulama için, `libMicrosoft.CognitiveServices.Speech.core.so`konuşma SDK Kitaplığı 'nı teslim etmeniz gerekir.
Uygulamanızı eşleşen sürümünü (x86, x64) seçtiğinizden emin olun. Linux sürümüne göre aşağıdaki bağımlılıkları içerecek şekilde gerekebilir:

- GNU C kitaplığının paylaşılan kitaplıkları (POSIX Iş parçacıkları programlama kitaplığı ile birlikte `libpthreads`)
- OpenSSL kitaplığı (`libssl.so.1.0.0` veya `libssl.so.1.0.2`)
- ALSA uygulamaları için paylaşılan kitaplık (`libasound.so.2`)

Ubuntu 'da, GNU C kitaplıklarının varsayılan olarak zaten yüklü olması gerekir. Son üç şu komutları kullanarak yüklenebilir:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.0 libasound2
```

DEYON 9 ' da şu paketleri yükler:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.2 libasound2
```

RHEL/CentOS 8 ' de:

```sh
sudo yum update
sudo yum install alsa-lib openssl
```

> [!NOTE]
> RHEL/CentOS 8 ' de, [Linux Için OpenSSL 'yi yapılandırma](~/articles/cognitive-services/speech-service/how-to-configure-openssl-linux.md)yönergelerini izleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
- [Bkz. konuşmayı tanımaC#](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnet)
