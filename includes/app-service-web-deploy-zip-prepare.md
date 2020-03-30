---
title: include dosyası
description: include dosyası
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 01/14/2020
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 6b5aa4f409b8c2f5a9125ab01e8587f4ac9c4ee5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75945148"
---
## <a name="create-a-project-zip-file"></a>Proje ZIP dosyası oluşturma

>[!NOTE]
> Dosyaları zip dosyasına indirdiyseniz, önce dosyaları ayıklayın. Örneğin, GitHub'dan bir ZIP dosyası indirdiyseniz, bu dosyayı olduğu gibi dağıtamazsınız. GitHub, App Service ile çalışmayan ek iç içe dizinler ekler. 
>

Yerel bir terminal penceresinde, uygulama projenizin kök dizinine gidin. 

Bu dizin, _index.html,_ _index.php_ve _app.js_gibi web uygulamanızın giriş dosyasını içermelidir. Ayrıca _project.json,_ _composer.json,_ _package.json,_ _bower.json,_ ve _requirements.txt_gibi paket yönetim dosyalarını da içerebilir.

Uygulama Hizmeti'nin sizin için dağıtım otomasyonu çalıştırmasını istemiyorsanız, `npm` `bower`tüm `gulp` `composer`yapı `pip`görevlerini çalıştırın (örneğin, , , , ve ) ve uygulamayı çalıştırmak için gereken tüm dosyalara sahip olduğundan emin olun. [Paketinizi doğrudan çalıştırmak](../articles/app-service/deploy-run-package.md)istiyorsanız bu adım gereklidir.

Projenizdeki tüm öğeleri içeren bir ZIP arşivi oluşturun. Aşağıdaki komut terminalinizdeki varsayılan aracı kullanmaktadır:

```
# Bash
zip -r <file-name>.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath <file-name>.zip
``` 

