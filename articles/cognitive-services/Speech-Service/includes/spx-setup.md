---
author: v-demjoh
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 05/15/2020
ms.author: v-demjoh
ms.openlocfilehash: 6011bf90d5a97dcc027f8a9a0916c28226c5c354
ms.sourcegitcommit: 16c7fd8fe944ece07b6cf42a9c0e82b057900662
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96584503"
---
## <a name="download-and-install"></a>İndirme ve yükleme

#### <a name="windows-install"></a>[Windows yüklemesi](#tab/windowsinstall)

Windows 'a konuşma CLı 'Yı yüklemek için aşağıdaki adımları izleyin:

1. Windows 'ta, platformunuz için [Visual Studio 2019 Için yeniden dağıtılabilir Microsoft Visual C++](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) gerekir. Bunu ilk kez yüklemek için yeniden başlatma gerekebilir.
2. Konuşma CLı [ZIP arşivini](https://aka.ms/speech/spx-zips.zip)indirip ayıklayın.
3. Ayıkladığınız dizine gidin `spx-zips` . Bu klasör çeşitli platformlarda konuşma CLı için program dosyalarını içerir. 
4. Platformunuzun dosyalarını ( `spx-net471` `spx-netcore-win-x64` BIR x64 CPU üzerinde .NET Framework 4,7 veya .net Core 3,0 için) ayıklayın. Bu dizinden çalıştıracağınızı aklınızda bulundurun `spx` .

### <a name="run-the-speech-cli"></a>Konuşma CLı 'sını çalıştırma

1. Komut istemi veya PowerShell ' i açın, ardından konuşma CLı 'yı ayıkladığınız dizine gidin.  
2. `spx`Konuşma CLI için yardım komutlarını görmek için yazın.

> [!NOTE]
> PowerShell, bir komut ararken yerel dizini denetlemez. PowerShell 'de Dizin ' i konum olarak değiştirin `spx` ve girerek aracı çağırın `.\spx` .
> Bu dizini yolunuza eklerseniz, PowerShell ve Windows komut istemi, `spx` ön eki dahil etmeden herhangi bir dizinden bulacaktır `.\` .

### <a name="font-limitations"></a>Yazı tipi sınırlamaları

Windows 'da, konuşma CLı yalnızca yerel bilgisayardaki komut isteminde kullanılabilen yazı tiplerini gösterebilir.
[Windows Terminal](https://www.microsoft.com/en-us/p/windows-terminal/9n0dx20hk701) , konuşma CLI tarafından etkileşimli olarak üretilen tüm yazı tiplerini destekler.

Bir dosyaya çıkış yaparsanız, Not defteri veya Microsoft Edge gibi bir Web tarayıcısı gibi bir metin Düzenleyicisi de tüm yazı tiplerini gösterebilir.

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

#### <a name="docker-install-windows-linux-macos"></a>[Docker yüklemesi (Windows, Linux, macOS)](#tab/dockerinstall)

Konuşma CLı 'Yı bir Docker kapsayıcısına yüklemek için şu adımları izleyin:

1. <a href="https://www.docker.com/get-started" target="_blank">Docker Desktop <span class="docon docon-navigate-external x-hidden-focus"></span> 'ı yükler</a> zaten yüklü değilse, platformunuz için.
2. Yeni bir komut isteminde veya terminalde aşağıdaki komutu yazın:
   ```shell   
   docker pull msftspeech/spx
   ```
3. Bu komutu yazın. Konuşma CLı için yardım bilgileri görmeniz gerekir:
   ```shell 
   docker run -it --rm msftspeech/spx help
   ```

### <a name="mount-a-directory-in-the-container"></a>Kapsayıcıda bir dizin bağlama

Konuşma CLı aracı yapılandırma ayarlarını dosyalar olarak kaydeder ve herhangi bir komut gerçekleştirirken (yardım komutları hariç) bu dosyaları yükler.
Bir Docker kapsayıcısı içinde konuşma CLı kullanırken, bir yerel dizini kapsayıcıdan bağlamanız gerekir, böylece araç yapılandırma ayarlarını saklayabilir veya bulabilir ve ayrıca araç, konuşma ses dosyaları gibi komut için gereken herhangi bir dosyayı okuyabilir veya yazabilir.

Windows 'ta, konuşma CLı 'nın kapsayıcı içinden kullanabileceği yerel bir dizin oluşturmak için bu komutu yazın:

`mkdir c:\spx-data`

Veya Linux veya macOS 'ta bir dizin oluşturmak ve mutlak yolunu görmek için bu komutu bir terminale yazın:

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

```shell
docker run -it -v c:\spx-data:/data --rm msftspeech/spx
```

Linux veya macOS 'ta, komutlarınız aşağıdaki örneğe benzer şekilde görünür. `ABSOLUTE_PATH`Bağlı dizininizin mutlak yoluyla değiştirin. Bu yol, `pwd` önceki bölümde komutu tarafından döndürüldü. 

Anahtarınızı ve bölgenizi ayarlamadan önce bu komutu çalıştırırsanız, anahtarınızı ve bölgenizi ayarlamanızı söyleyen bir hata alacaksınız:
```shell   
sudo docker run -it -v ABSOLUTE_PATH:/data --rm msftspeech/spx
```

`spx`Bir kapsayıcıda yüklü olan komutu kullanmak için, her zaman yukarıda gösterilen tam komutu ve ardından isteğinizin parametrelerini girin.
Örneğin, Windows 'da bu komut anahtarınızı ayarlar:

```shell
docker run -it -v c:\spx-data:/data --rm msftspeech/spx config @key --set SUBSCRIPTION-KEY
```

> [!WARNING]
> Bir Docker kapsayıcısı içinde konuşma CLı çalıştırdığınızda bilgisayarınızın mikrofonunu kullanamazsınız. Ancak, yerel olarak bağlı dizininizde ses dosyalarını okuyabilir ve kaydedebilirsiniz. 

<!-- Need to troubleshoot issues with docker pull image

### Optional: Create a command line shortcut

If you're running the the Speech CLI from a Docker container on Linux or macOS you can create a shortcut. 

Follow these instructions to create a shortcut:
1. Open `.bash_profile` with your favorite text editor. For example:
   ```shell
   nano ~/.bash_profile
   ```
2. Next, add this function to your `.bash_profile`. Make sure you update this function with the correct path to your mounted directory:
   ```shell   
   spx(){
       sudo docker run -it -v ABSOLUTE_PATH:/data --rm msftspeech/spx
   }
   ```
3. Source your profile:
   ```shell
   source ~/.bash_profile
   ```
4. Now instead of running `sudo docker run -it -v ABSOLUTE_PATH:/data --rm msftspeech/spx`, you can just type `spx` followed by arguments. For example: 
   ```shell
   // Get some help
   spx help recognize

   // Recognize speech from an audio file 
   spx recognize --file /mounted/directory/file.wav
   ```

> [!WARNING]
> If you change the mounted directory that Docker is referencing, you need to update the function in `.bash_profile`.
--->
***

## <a name="create-subscription-config"></a>Abonelik yapılandırması oluştur

Konuşma CLı 'yı kullanmaya başlamak için konuşma abonelik anahtarınızı ve bölge tanımlayıcıyı girmeniz gerekir. [Konuşma hizmetini ücretsiz deneyin](../overview.md#try-the-speech-service-for-free)bölümündeki adımları izleyerek bu kimlik bilgilerini alın.
Abonelik anahtarınızı ve bölge tanımlayıcıyı (örn. `eastus`, `westus` ), aşağıdaki komutları çalıştırın.

```shell
spx config @key --set SUBSCRIPTION-KEY
spx config @region --set REGION
```

Abonelik kimlik doğrulaması artık gelecekteki SPX istekleri için depolandı. Bu depolanmış değerlerden birini kaldırmanız gerekirse, veya ' i çalıştırın `spx config @region --clear` `spx config @key --clear` .
