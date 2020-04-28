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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75945148"
---
## <a name="create-a-project-zip-file"></a>Proje ZIP dosyası oluşturma

>[!NOTE]
> Dosyaları bir ZIP dosyasına indirdiyseniz, önce dosyaları ayıklayın. Örneğin, GitHub 'dan bir ZIP dosyası indirdiyseniz, bu dosyayı olduğu gibi dağıtamazsınız. GitHub, App Service çalışmayan ek iç içe dizinler ekler. 
>

Yerel bir Terminal penceresinde, uygulama projenizin kök dizinine gidin. 

Bu dizin, Web uygulamanıza, _Dizin. html_, _index. php_ve _app. js_gibi giriş dosyasını içermelidir. Ayrıca, _Project. JSON_, _besteci. JSON_, _Package. JSON_, _Bower. JSON_ve _requirements. txt_gibi paket yönetim dosyalarını da içerebilir.

Dağıtım otomasyonunu sizin için çalıştırmak App Service istemiyorsanız, tüm derleme görevlerini çalıştırın ( `npm`örneğin `bower` `gulp` `composer`,,,, ve `pip`) ve uygulamayı çalıştırmak için ihtiyacınız olan tüm dosyalara sahip olduğunuzdan emin olun. [Paketinizi doğrudan çalıştırmak](../articles/app-service/deploy-run-package.md)istiyorsanız bu adım gereklidir.

Projenizdeki tüm öğeleri içeren bir ZIP arşivi oluşturun. Aşağıdaki komut terminalinizdeki varsayılan aracı kullanmaktadır:

```
# Bash
zip -r <file-name>.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath <file-name>.zip
``` 

