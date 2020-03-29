---
title: Speech SDK ile uygulama geliştirin - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Desteklenen platformlarda Konuşma SDK kullanan bir uygulamayı nasıl dağıttığınızı öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78330813"
---
# <a name="ship-an-application"></a>Uygulama teslim etme

Azure Bilişsel Hizmetler Konuşma SDK'sını dağıtırken [Konuşma SDK lisansını](https://aka.ms/csspeech/license201809)ve [üçüncü taraf yazılım bildirimlerini](https://csspeechstorage.blob.core.windows.net/drop/1.0.0/ThirdPartyNotices.html) izleyin. Ayrıca, [Microsoft Gizlilik Bildirimi'ni](https://aka.ms/csspeech/privacy)gözden geçirin.

Platforma bağlı olarak, uygulamanızı yürütmek için farklı bağımlılıklar vardır.

## <a name="windows"></a>Windows

Bilişsel Hizmetler Konuşma SDK Windows 10 ve Windows Server 2016 üzerinde test edilmiştir.

Bilişsel Hizmetler Konuşma SDK sistemde [Visual Studio 2019 için Microsoft Visual C++ Yeniden Dağıtılabilir](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) gerektirir. `Microsoft Visual C++ Redistributable for Visual Studio 2019` Burada en son sürümü için yükleyiciler indirebilirsiniz:

- [Win32](https://aka.ms/vs/16/release/vc_redist.x86.exe)
- [x64](https://aka.ms/vs/16/release/vc_redist.x64.exe)

Uygulamanız yönetilen kodu kullanıyorsa, hedef makinede `.NET Framework 4.6.1` veya daha sonra gereklidir.

Mikrofon girişi için Medya Vakfı kitaplıkları yüklenmelidir. Bu kitaplıklar Windows 10 ve Windows Server 2016'nın bir parçasıdır. Ses giriş aygıtı olarak mikrofon kullanılmaması sürece, bu kitaplıklar olmadan Speech SDK'yı kullanmak mümkündür.

Gerekli Konuşma SDK dosyaları uygulamanızla aynı dizinde dağıtılabilir. Bu şekilde uygulamanız kitaplıklara doğrudan erişebilir. Uygulamanızla eşleşen doğru sürümü (Win32/x64) seçtiğinizden emin olun.

| Adı | İşlev |
| :--- | :------- |
| `Microsoft.CognitiveServices.Speech.core.dll`   | Çekirdek SDK, yerel ve yönetilen dağıtım için gerekli |
| `Microsoft.CognitiveServices.Speech.csharp.dll` | Yönetilen dağıtım için gerekli                      |

> [!NOTE]
> Sürüm 1.3.0 ile başlayarak dosya `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` (önceki sürümlerde sevk) artık gerekli değildir. İşlevsellik artık çekirdek SDK entegre edilmiştir.

> [!NOTE]
> Windows Forms App (.NET Framework) C# projesi için kitaplıkların projenizin dağıtım ayarlarına dahil olduğundan emin olun. Bunu altında `Properties -> Publish Section`kontrol edebilirsiniz. `Application Files` Düğmeye tıklayın ve aşağı kaydırma listesinden ilgili kitaplıkları bulun. Değerin `Included`' e ayarlandıkundan emin olun Visual Studio, proje yayımlandığında/dağıtıldığında dosyayı içerir.

## <a name="linux"></a>Linux

Konuşma SDK şu anda Ubuntu 16.04, Ubuntu 18.04, Debian 9, RHEL 8, CentOS 8 dağılımları destekler.
Yerel bir uygulama için, Konuşma SDK kitaplığını göndermeniz gerekir. `libMicrosoft.CognitiveServices.Speech.core.so`
Uygulamanızla eşleşen sürümü (x86, x64) seçtiğinizden emin olun. Linux sürümüne bağlı olarak, aşağıdaki bağımlılıkları da eklemeniz gerekebilir:

- GNU C kütüphanesinin ortak kitaplıkları (POSIX Threads `libpthreads`Programming kitaplığı dahil)
- OpenSSL kitaplığı`libssl.so.1.0.0` `libssl.so.1.0.2`( veya )
- ALSA uygulamaları için ortak`libasound.so.2`kitaplık ( )

Ubuntu'da, GNU C kitaplıkları varsayılan olarak zaten yüklenmiş olmalıdır. Son üç bu komutları kullanarak yüklenebilir:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.0 libasound2
```

Debian 9'da bu paketleri yükleyin:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.2 libasound2
```

RHEL/CentOS 8'de:

```sh
sudo yum update
sudo yum install alsa-lib openssl
```

> [!NOTE]
> RHEL/CentOS 8'de [OpenSSL'in Linux için nasıl yapılandırılacağı](~/articles/cognitive-services/speech-service/how-to-configure-openssl-linux.md)yla ilgili talimatları uygulayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
- [C'deki konuşmayı nasıl tanıyacağınızı görme #](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnet)
