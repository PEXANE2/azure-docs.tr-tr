---
author: v-demjoh
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 05/15/2020
ms.author: v-demjoh
ms.openlocfilehash: c57187ae15e5f15a601edbc74921b3c879abe715
ms.sourcegitcommit: cf7caaf1e42f1420e1491e3616cc989d504f0902
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83800686"
---
## <a name="prerequisites"></a>Ön koşullar

Tek önkoşul, bir Azure konuşma aboneliğudur. Henüz bir abonelik yoksa, yeni abonelik oluşturma [kılavuzuna](../get-started.md#new-resource) bakın.

## <a name="download-and-install"></a>İndirme ve yükleme

#### <a name="windows-install"></a>[Windows yüklemesi](#tab/windowsinstall)

Windows 'a konuşma CLı 'Yı yüklemek için aşağıdaki adımları izleyin:

1. [.NET Framework 4,7](https://dotnet.microsoft.com/download/dotnet-framework/net471) veya [.NET Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0) ' ü yükleyebilir
2. Konuşma CLı [ZIP arşivini](https://aka.ms/speech/spx-zips.zip)indirip ayıklayın.
3. `spx-zips`İndirmenin ayıklanacağı kök dizine gidin ve ihtiyacınız olan alt dizini ayıklayın ( `spx-net471` .NET Framework 4,7 için veya `spx-netcore-win-x64` x64 CPU üzerinde .NET Core 3,0 için).

Komut isteminde dizini bu konum olarak değiştirin ve ardından yazarak `spx` konuşma CLI için yardımı görüntüleyin.

#### <a name="linux-install"></a>[Linux yüklemesi](#tab/linuxinstall)

Linux 'ta konuşma CLı 'Yı x64 CPU 'ya yüklemek için şu adımları izleyin:

1. [.NET Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0)'yi yükler.
2. Konuşma CLı [ZIP arşivini](https://aka.ms/speech/spx-zips.zip)indirip ayıklayın.
3. `spx-zips`İndirmenin ayıklanacağı kök dizine gidin ve `spx-netcore-30-linux-x64` Yeni bir `~/spx` dizine ayıklayın.
4. Bir terminalde şu komutları yazın:
   1. `cd ~/spx`
   2. `sudo chmod +r+x spx`
   3. `PATH=~/spx:$PATH`

`spx`Konuşma CLI yardımını görmek için yazın.

***

## <a name="create-subscription-config"></a>Abonelik yapılandırması oluştur

Konuşma CLı 'yı kullanmaya başlamak için önce konuşma aboneliği anahtarınızı ve bölge bilgilerinizi girmeniz gerekir. Bölge tanımlarınızı bulmak için [bölge desteği](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#speech-sdk) sayfasına bakın. Abonelik anahtarınızı ve bölge tanımlayıcıyı (örn. `eastus`, `westus` ), aşağıdaki komutları çalıştırın.

```shell
spx config @key --set YOUR-SUBSCRIPTION-KEY
spx config @region --set YOUR-REGION-ID
```

Abonelik kimlik doğrulaması artık gelecekteki SPX istekleri için depolandı. Bu depolanmış değerlerden birini kaldırmanız gerekirse, veya ' i çalıştırın `spx config @region --clear` `spx config @key --clear` .
