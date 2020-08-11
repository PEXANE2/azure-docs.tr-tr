---
author: v-demjoh
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 05/15/2020
ms.author: v-demjoh
ms.openlocfilehash: c92d6569e3c92d3bad3575599283c7796bd78225
ms.sourcegitcommit: d8b8768d62672e9c287a04f2578383d0eb857950
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88068606"
---
## <a name="prerequisites"></a>Önkoşullar

Tek önkoşul, bir Azure konuşma aboneliğudur. Henüz bir abonelik yoksa, yeni abonelik oluşturma [kılavuzuna](../get-started.md#new-resource) bakın.

## <a name="download-and-install"></a>İndirme ve yükleme

#### <a name="windows-install"></a>[Windows yüklemesi](#tab/windowsinstall)

Windows 'a konuşma CLı 'Yı yüklemek için aşağıdaki adımları izleyin:

1. [.NET Framework 4,7](https://dotnet.microsoft.com/download/dotnet-framework/net471) veya [.NET Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0) ' ü yükleyebilir
2. Konuşma CLı [ZIP arşivini](https://aka.ms/speech/spx-zips.zip)indirip ayıklayın.
3. `spx-zips`İndirmenin ayıklanacağı kök dizine gidin ve ihtiyacınız olan alt dizini ayıklayın ( `spx-net471` .NET Framework 4,7 için veya `spx-netcore-win-x64` x64 CPU üzerinde .NET Core 3,0 için).

Komut isteminde dizini bu konum olarak değiştirin ve ardından yazarak `spx` konuşma CLI için yardımı görüntüleyin.

> [!NOTE]
> Windows 'da, konuşma CLı yalnızca yerel bilgisayardaki komut isteminde kullanılabilen yazı tiplerini gösterebilir.
> [Windows Terminal](https://www.microsoft.com/en-us/p/windows-terminal/9n0dx20hk701) , konuşma CLI tarafından etkileşimli olarak üretilen tüm yazı tiplerini destekler.
> Bir dosyaya çıkış yaparsanız, Not defteri veya Microsoft Edge gibi bir Web tarayıcısı gibi bir metin Düzenleyicisi de tüm yazı tiplerini gösterebilir.

> [!NOTE]
> PowerShell, bir komut ararken yerel dizini denetlemez. PowerShell 'de Dizin ' i konum olarak değiştirin `spx` ve girerek aracı çağırın `.\spx` .
> Bu dizini yolunuza eklerseniz, PowerShell ve Windows komut istemi, `spx` ön eki dahil etmeden herhangi bir dizinden bulacaktır `.\` .

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

#### <a name="docker-install"></a>[Docker yüklemesi](#tab/dockerinstall)

Konuşma CLı 'Yı bir Docker kapsayıcısına yüklemek için şu adımları izleyin:

1. [Platformunuz Için Docker Desktop](https://www.docker.com/get-started)'ı yükleyip çalıştırın.
1. Yeni bir komut isteminde veya terminalde aşağıdaki komutu yazın:`docker pull msftspeech/spx`
1. Bu komutu yazın. Konuşma CLı için yardım bilgileri görmeniz gerekir:`docker run -it --rm msftspeech/spx help`

### <a name="mount-a-directory-in-the-container"></a>Kapsayıcıda bir dizin bağlama

Konuşma CLı aracı yapılandırma ayarlarını dosyalar olarak kaydeder ve herhangi bir komut gerçekleştirirken (yardım komutları hariç) bu dosyaları yükler.
Bir Docker kapsayıcısı içinde konuşma CLı kullanırken, bir yerel dizini kapsayıcıdan bağlamanız gerekir, böylece araç yapılandırma ayarlarını saklayabilir veya bulabilir ve ayrıca araç, konuşma ses dosyaları gibi komut için gereken herhangi bir dosyayı okuyabilir veya yazabilir.

Windows 'ta, konuşma CLı 'nın kapsayıcı içinden kullanabileceği yerel bir dizin oluşturmak için bu komutu yazın:

`mkdir c:\spx-data`

Veya Linux veya Mac üzerinde, bir dizin oluşturmak ve mutlak yolunu görmek için bu komutu bir terminale yazın:

```bash
mkdir ~/spx-data
cd ~/spx-data
pwd
```

Konuşma CLı 'Yı çağırdığınızda mutlak yol kullanacaksınız.

### <a name="run-speech-cli-in-the-container"></a>Kapsayıcıda konuşma CLı 'Yı çalıştırma

Bu belgelerde `spx` Docker olmayan yüklemelerde kullanılan konuşma CLI komutu gösterilmektedir.
`spx`Bir Docker kapsayıcısında komutunu çağırırken, konuşma CLI 'nın yapılandırma değerlerini depolayabileceği ve bulabileceği ve dosyaları okuyabildiği ve yazacağı dosya sistemine kapsayıcıda bir dizin bağlamanız gerekir.
Windows 'ta komutlarınız şöyle olacaktır:

`docker run -it -v c:\spx-data:/data --rm msftspeech/spx`

Linux veya Mac üzerinde, komutlarınız şuna benzer şekilde başlayacaktır:

`sudo docker run -it -v /ABSOLUTE_PATH:/data --rm msftspeech/spx`

> [!NOTE]
> `/ABSOLUTE_PATH`Yukarıdaki bölümde komut tarafından gösterilen mutlak yol ile değiştirin `pwd` .

`spx`Bir kapsayıcıda yüklü olan komutu kullanmak için, her zaman yukarıda gösterilen tam komutu ve ardından isteğinizin parametrelerini girin.
Örneğin, Windows 'da bu komut anahtarınızı ayarlar:

`docker run -it -v c:\spx-data:/data --rm msftspeech/spx config @key --set SUBSCRIPTION-KEY`

> [!NOTE]
> Bir Docker kapsayıcısı içinde konuşma CLı çalıştırdığınızda bilgisayarınızın mikrofonunu veya hoparlörünü kullanamazsınız.
> Bu cihazları kullanmak için, Docker kapsayıcısının dışında kayıt/kayıttan yürütme için konuşma CLı 'ye ve ses dosyalarını geçirin.
> Konuşma CLı Aracı, yukarıdaki adımlarda ayarladığınız yerel dizine erişebilir.

***

## <a name="create-subscription-config"></a>Abonelik yapılandırması oluştur

Konuşma CLı 'yı kullanmaya başlamak için önce konuşma aboneliği anahtarınızı ve bölge bilgilerinizi girmeniz gerekir. Bölge tanımlarınızı bulmak için [bölge desteği](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#speech-sdk) sayfasına bakın. Abonelik anahtarınızı ve bölge tanımlayıcıyı (örn. `eastus`, `westus` ), aşağıdaki komutları çalıştırın.

```shell
spx config @key --set SUBSCRIPTION-KEY
spx config @region --set REGION
```

Abonelik kimlik doğrulaması artık gelecekteki SPX istekleri için depolandı. Bu depolanmış değerlerden birini kaldırmanız gerekirse, veya ' i çalıştırın `spx config @region --clear` `spx config @key --clear` .
