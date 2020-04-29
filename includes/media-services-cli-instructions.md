---
title: include dosyası
description: include dosyası
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 01/28/2019
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: 5ebbac39c8850737ea6f9ef333e45d305a520655
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79461223"
---
## <a name="use-cli-shell"></a>CLı kabuğu kullanma

CLı komutlarını yürütmek için [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest) kullanılması önerilir. **Cloud Shell** , bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz ve etkileşimli bir kabuktur. Yaygın kullanılan Azure araçları hesabınızla kullanmanız için Cloud Shell'de önceden yüklenir ve yapılandırılır. Bu hizmet, çalışma şeklinize en uygun kabuk deneyimini seçme esnekliği sunar. Linux kullanıcıları Bash deneyimini, Windows kullanıcıları da PowerShell deneyimini seçebilir.

CLı 'yi yerel olarak da yükleyebilirsiniz. Platformunuza ilişkin yönergeler için bkz. [Azure CLI 'Yı yüklemeyin](https://docs.microsoft.com/cli/azure/install-azure-cli) .

### <a name="sign-in"></a>Oturum açma

CLı 'nın yerel yüklemesinin kullanılması için Azure 'da oturum açmak gerekir. Azure Cloud Shell için bu adım gerekli değildir. `az login` Komutuyla oturum açın.

CLI varsayılan tarayıcınızı açabiliyorsa, tarayıcıyı açar ve oturum açma sayfasını yükler. Aksi takdirde, tarayıcınızda gezindikten https://aka.ms/devicelogin sonra bir yetkilendirme kodu girmek için bir tarayıcı sayfası açmanız ve komut satırındaki yönergeleri izlemeniz gerekir.

### <a name="specify-location-of-files"></a>Dosyaların konumunu belirtin

Birçok Media Services CLı komutu bir dosya adı ile bir parametre geçirmenize izin verir. **Cloud Shell**kullanıyorsanız, dosyanızı CloudDrive 'A (bash veya PowerShell kullanarak) yükleyebilirsiniz. 

![Dosyaları karşıya yükleme]

Yerel bir CLı veya **Cloud Shell**kullanıyor olmanıza bakılmaksızın, kullanmakta olduğunuz işletim sistemine veya Cloud Shell (bash veya PowerShell) göre dosya yolunu belirtmeniz gerekir. Aşağıda bazı örnekler verilmiştir:

Dosyanın göreli yolu (tüm işletim sistemi)

* `@"mytestfile.json"`
* `@"../mytestfile.json"`

Linux/Mac ve Windows işletim sistemi için mutlak dosya yolu

* `@ "/usr/home/mytestfile.json"`
*    `@"c:\tmp\user\mytestfile.json"`

Komutun `{file}` dosyanın yolunu soruyor olması halinde kullanın. Örneğin, `az ams transform create -a amsaccount -g resourceGroup -n custom --preset .\customPreset.json`. <br/> Komut `@{file}` belirtilen dosyayı yükleyecekse kullanın. Örneğin, `az ams account-filter create -a amsaccount -g resourceGroup -n filterName --tracks @tracks.json`.

[Dosyaları karşıya yükleme]: ./media/media-services-cli/upload-download-files.png
