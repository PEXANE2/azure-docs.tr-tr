---
title: PHP için Azure SDK'sını indirme
description: PHP için Azure SDK 'sını indirme ve yükleme hakkında bilgi edinin.
documentationcenter: php
services: app-service\web
author: allclark
manager: douge
editor: ''
ms.assetid: bac355ac-4c25-42f4-8273-c5112eafa8d4
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 06/01/2016
ms.author: yaya
ms.openlocfilehash: 9dd336ef973b603108aad62c90e4dab385d20317
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "67873889"
---
# <a name="download-the-azure-sdk-for-php"></a>PHP için Azure SDK'sını indirme

## <a name="overview"></a>Genel Bakış

PHP için Azure SDK 'Sı, Azure için PHP uygulamaları geliştirmenize, dağıtmanıza ve yönetmenize imkan tanıyan bileşenleri içerir. Özellikle, PHP için Azure SDK 'Sı şunları içerir:

* **Azure IÇIN php istemci kitaplıkları**. Bu sınıf kitaplıkları, veri yönetimi hizmetleri ve bulut hizmetleri gibi Azure özelliklerine erişim için bir arabirim sağlar.
* **Mac, Linux ve Windows Için Azure komut satırı arabirimi (Azure CLI)** . Bu, Azure Web siteleri ve Azure sanal makineleri gibi Azure hizmetlerini dağıtmaya ve yönetmeye yönelik bir dizi komut kümesidir. Azure CLı, Mac, Linux ve Windows dahil tüm platformlarda çalışır.
* **Azure PowerShell (yalnızca Windows)** . Bu, Cloud Services ve sanal makineler gibi Azure hizmetlerini dağıtmak ve yönetmek için bir PowerShell cmdlet kümesidir.
* **Azure öykünücüleri (yalnızca Windows)** . İşlem ve depolama öykünücüleri, bir uygulamayı yerel olarak test etme izni veren bulut hizmetleri ve veri yönetimi hizmetleri 'nin yerel öykünücülerdir. Azure öykünücüleri yalnızca Windows 'da çalışır.

Aşağıdaki bölümlerde yukarıda açıklanan bileşenlerin nasıl indirileceği ve kurulacağı açıklanmaktadır.

Bu konudaki yönergeler, [php][install-php] 'nin yüklü olduğunu varsayar.

> [!NOTE]
> Azure için PHP istemci kitaplıklarını kullanmak için PHP 5,5 veya üzeri bir sürüme sahip olmanız gerekir.
>
>

## <a name="php-client-libraries-for-azure"></a>Azure için PHP istemci kitaplıkları

Azure için PHP Istemci kitaplıkları, herhangi bir işletim sisteminden veri yönetimi hizmetleri ve bulut hizmetleri gibi Azure özelliklerine erişmek için bir arabirim sağlar. Bu kitaplıklar, besteci aracılığıyla yüklenebilir.

Azure için PHP Istemci kitaplıklarını kullanma hakkında daha fazla bilgi için bkz. [BLOB hizmetini kullanma][blob-service], [tablo hizmetini kullanma][table-service] ve [kuyruk hizmetini kullanma][queue-service].

### <a name="install-via-composer"></a>Besteci aracılığıyla Install

1. [Git’i yükleyin][install-git]. Windows 'da, git yürütülebilir dosyasını yol ortam değişkeniniz olarak da eklemeniz gerekir.

2. Projenizin kökünde **besteci. JSON** adlı bir dosya oluşturun ve bu dosyaya aşağıdaki kodu ekleyin:

        {
            "require": {
                "microsoft/windowsazure": "^0.4"
            }
        }

3. Proje kökünde **[besteci. phar][composer-phar]** 'yi indirin.

4. Bir komut istemi açın ve bunu proje kökünde yürütün

        php composer.phar install

## <a name="azure-powershell-and-azure-emulators"></a>Azure PowerShell ve Azure öykünücüleri

Azure PowerShell, Azure hizmetlerini dağıtmak ve yönetmek için bir PowerShell cmdlet 'leri kümesidir (örneğin, Cloud Services ve sanal makineler). Azure öykünücüleri, bir uygulamayı yerel olarak test etme izni veren bulut hizmetleri ve veri yönetimi hizmetleri öykünücülerdir. Bu bileşenler yalnızca desteklenen Windows bileşenleridir.

Azure PowerShell yüklemek için önerilen yöntem ve Azure öykünücüleri [Microsoft Web Platformu Yükleyicisi][download-wpi]kullanmaktır. PHP, SQL Server, PHP için SQL Server Microsoft sürücüleri ve WebMatrix gibi diğer geliştirme bileşenlerini de yüklemeyi seçebileceğinizi unutmayın.

Azure PowerShell kullanma hakkında daha fazla bilgi için bkz. [Azure PowerShell kullanma][powershell-tools].

## <a name="azure-cli"></a>Azure CLI

Azure CLı, Azure Web siteleri ve Azure sanal makineleri gibi Azure hizmetlerini dağıtmaya ve yönetmeye yönelik bir dizi komut kümesidir. Azure CLı yükleme hakkında daha fazla bilgi için bkz. [Azure CLI 'Yı yükleme](cli-install-nodejs.md).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [php Geliştirici Merkezi](https://azure.microsoft.com/develop/php/).

[install-php]: https://www.php.net/manual/en/install.php
[composer-github]: https://github.com/composer/composer
[composer-phar]: https://getcomposer.org/composer.phar
[nodejs-org]: https://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[download-wpi]: https://go.microsoft.com/fwlink/?LinkId=253447
[mac-installer]: https://go.microsoft.com/fwlink/?LinkId=252249
[blob-service]: https://go.microsoft.com/fwlink/?LinkId=252714
[table-service]: https://go.microsoft.com/fwlink/?LinkId=252715
[queue-service]: https://go.microsoft.com/fwlink/?LinkId=252716
[azure cli]: https://go.microsoft.com/fwlink/?LinkId=252717
[powershell-tools]: https://go.microsoft.com/fwlink/?LinkId=252718
[php-sdk-github]: https://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: https://git-scm.com/book/en/Getting-Started-Installing-Git
