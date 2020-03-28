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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79461223"
---
## <a name="use-cli-shell"></a>CLI Shell kullanın

CLI komutlarını yürütmek için [Azure Bulut Su şusu](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest) kullanılması önerilir. **Cloud Shell,** bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz, etkileşimli bir kabuktur. Yaygın kullanılan Azure araçları hesabınızla kullanmanız için Cloud Shell'de önceden yüklenir ve yapılandırılır. Bu hizmet, çalışma şeklinize en uygun kabuk deneyimini seçme esnekliği sunar. Linux kullanıcıları Bash deneyimini, Windows kullanıcıları da PowerShell deneyimini seçebilir.

CLI'yi yerel olarak da yükleyebilirsiniz. Platformunuziçin talimatlar için [Azure CLI'yi yükleyin'](https://docs.microsoft.com/cli/azure/install-azure-cli) e bakın.

### <a name="sign-in"></a>Oturum aç

CLI'nin yerel bir yüklemesini kullanmak için Azure'da oturum açma gerekir. Bu adım Azure Bulut BulutU Için gerekli değildir. `az login` Komutla oturum açın.

CLI varsayılan tarayıcınızı açabiliyorsa, tarayıcıyı açar ve oturum açma sayfasını yükler. Aksi takdirde, tarayıcınızda gezindikten https://aka.ms/devicelogin sonra bir yetkilendirme kodu girmek için bir tarayıcı sayfası açmanız ve komut satırındaki yönergeleri izlemeniz gerekir.

### <a name="specify-location-of-files"></a>Dosyaların konumunu belirtin

Birçok Medya Hizmetleri CLI komutları, dosya adı içeren bir parametreyi geçirmenize olanak sağlar. **Cloud Shell**kullanıyorsanız, dosyanızı clouddrive'ınıza yükleyebilirsiniz (Bash veya PowerShell'i kullanarak). 

![Dosyaları karşıya yükleme]

İster yerel bir CLI veya **Cloud Shell**kullanıyor olun, dosya yolunu kullanmakta olduğunuz işletim sistemi veya Bulut Kabuğu'na (Bash veya PowerShell) göre belirtmeniz gerekir. Aşağıda bazı örnekler verilmiştir:

Dosyaya göreli yol (tüm işletim sistemi)

* `@"mytestfile.json"`
* `@"../mytestfile.json"`

Linux/Mac ve Windows OS'de mutlak dosya yolu

* `@ "/usr/home/mytestfile.json"`
*    `@"c:\tmp\user\mytestfile.json"`

Komut `{file}` dosyaya giden bir yol soruyorsa kullanın. Örneğin, `az ams transform create -a amsaccount -g resourceGroup -n custom --preset .\customPreset.json`. <br/> Komut `@{file}` belirtilen dosyayı yükleyecekse kullanın. Örneğin, `az ams account-filter create -a amsaccount -g resourceGroup -n filterName --tracks @tracks.json`.

[Dosyaları karşıya yükleme]: ./media/media-services-cli/upload-download-files.png
