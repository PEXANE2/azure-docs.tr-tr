---
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 11/25/2018
ms.author: spelluru
ms.openlocfilehash: 5e3c4622131528fc2c40a1510aeea3092018d182
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77474236"
---
### <a name="install-via-composer"></a>Composer ile yükle
1. Projenizin kökünde **composer.json** adlı bir dosya oluşturun ve projeye aşağıdaki kodu ekleyin:
   
    ```json
    {
      "require": {
        "microsoft/windowsazure": "*"
      }
    }
    ```
2. Proje kökünde **[composer.phar'ı][composer-phar]** indirin.
3. Komut istemini açın ve proje kökünüzde aşağıdaki komutu çalıştırın
   
    ```
    php composer.phar install
    ```

Alternatif olarak, kaynak kodu klonlamak için GitHub'daki [Azure Depolama PHP İstemci Kitaplığı'na][php-sdk-github] gidin.

[php-sdk-github]: https://github.com/Azure/azure-storage-php
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[download-SDK-PHP]: https://github.com/Azure/azure-sdk-for-php
[composer-phar]: http://getcomposer.org/composer.phar
