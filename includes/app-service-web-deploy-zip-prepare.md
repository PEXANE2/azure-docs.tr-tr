---
title: dosya dahil etme
description: dosya dahil etme
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 01/14/2020
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 035399924216434de85865102db8838ea3fa15a3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85570161"
---
## <a name="create-a-project-zip-file"></a>Proje ZIP dosyası oluşturma

>[!NOTE]
> Dosyaları bir ZIP dosyasına indirdiyseniz, önce dosyaları ayıklayın. Örneğin, GitHub 'dan bir ZIP dosyası indirdiyseniz, bu dosyayı olduğu gibi dağıtamazsınız. GitHub, App Service çalışmayan ek iç içe dizinler ekler. 
>

Yerel bir Terminal penceresinde, uygulama projenizin kök dizinine gidin. 

Bu dizin, _index.html_, _index. php_ve _app.js_gibi Web uygulamanıza giriş dosyasını içermelidir. Ayrıca, _project.json_, _composer.jsüzerinde_ _package.js_, üzerinde _bower.js_ve _requirements.txt_gibi paket yönetim dosyalarını da içerebilir.

Dağıtım otomasyonunu sizin için çalıştırmak App Service istemiyorsanız, tüm derleme görevlerini çalıştırın (örneğin,,,, `npm` `bower` `gulp` `composer` ve `pip` ) ve uygulamayı çalıştırmak için ihtiyacınız olan tüm dosyalara sahip olduğunuzdan emin olun. [Paketinizi doğrudan çalıştırmak](../articles/app-service/deploy-run-package.md)istiyorsanız bu adım gereklidir.

Projenizdeki tüm öğeleri içeren bir ZIP arşivi oluşturun. `dotnet`Projeler için, bu klasör komutun çıkış klasörüdür `dotnet publish` . Aşağıdaki komut terminalinizdeki varsayılan aracı kullanmaktadır:

```
# Bash
zip -r <file-name>.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath <file-name>.zip
``` 

