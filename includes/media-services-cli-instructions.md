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
ms.openlocfilehash: 1327ac49920af353d6adbbe592ab4189417e60e8
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "95996924"
---
## <a name="use-cli-shell"></a>CLı kabuğu kullanma

CLı komutlarını yürütmek için [Azure Cloud Shell](../articles/cloud-shell/overview.md?view=azure-cli-latest) kullanılması önerilir. **Cloud Shell** , bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz ve etkileşimli bir kabuktur. Yaygın kullanılan Azure araçları hesabınızla kullanmanız için Cloud Shell'de önceden yüklenir ve yapılandırılır. Bu hizmet, çalışma şeklinize en uygun kabuk deneyimini seçme esnekliği sunar. Linux kullanıcıları Bash deneyimini, Windows kullanıcıları da PowerShell deneyimini seçebilir.

CLı 'yi yerel olarak da yükleyebilirsiniz. Platformunuza ilişkin yönergeler için bkz. [Azure CLI 'Yı yüklemeyin](/cli/azure/install-azure-cli) .

### <a name="sign-in"></a>Oturum açın

CLı 'nın yerel yüklemesinin kullanılması için Azure 'da oturum açmak gerekir. Azure Cloud Shell için bu adım gerekli değildir. Komutuyla oturum açın `az login` .

CLI varsayılan tarayıcınızı açabiliyorsa, tarayıcıyı açar ve oturum açma sayfasını yükler. Aksi takdirde, tarayıcınızda gezindikten sonra bir yetkilendirme kodu girmek için bir tarayıcı sayfası açmanız ve komut satırındaki yönergeleri izlemeniz gerekir https://aka.ms/devicelogin .

### <a name="specify-location-of-files"></a>Dosyaların konumunu belirtin

Birçok Media Services CLı komutu bir dosya adı ile bir parametre geçirmenize izin verir. **Cloud Shell** kullanıyorsanız, dosyanızı CloudDrive 'A (bash veya PowerShell kullanarak) yükleyebilirsiniz. 

![Dosyaları karşıya yükleme]

Yerel bir CLı veya **Cloud Shell** kullanıyor olmanıza bakılmaksızın, kullanmakta olduğunuz işletim sistemine veya Cloud Shell (bash veya PowerShell) göre dosya yolunu belirtmeniz gerekir. Aşağıda bazı örnekler verilmiştir:

Dosyanın göreli yolu (tüm işletim sistemi)

* `@"mytestfile.json"`
* `@"../mytestfile.json"`

Linux/Mac ve Windows işletim sistemi için mutlak dosya yolu

* `@ "/usr/home/mytestfile.json"`
*    `@"c:\tmp\user\mytestfile.json"`

`{file}`Komutun dosyanın yolunu soruyor olması halinde kullanın. Örneğin, `az ams transform create -a amsaccount -g resourceGroup -n custom --preset .\customPreset.json`. <br/> `@{file}`Komut belirtilen dosyayı yükleyecekse kullanın. Örneğin, `az ams account-filter create -a amsaccount -g resourceGroup -n filterName --tracks @tracks.json`.

[Dosyaları karşıya yükleme]: ./media/media-services-cli/upload-download-files.png