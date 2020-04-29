---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: 5cb345ef2d20f75066e90f9e6478be27f925b1b0
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80673349"
---
### <a name="retrieve-the-azure-storage-connection-string"></a>Azure depolama bağlantı dizesini alma

Daha önce, işlev uygulaması tarafından kullanılmak üzere bir Azure depolama hesabı oluşturdunuz. Bu hesabın bağlantı dizesi, Azure 'daki uygulama ayarlarında güvenli bir şekilde depolanır. Bu ayarı *Local. Settings. JSON* dosyasına indirerek bu bağlantıyı, işlevi yerel olarak çalıştırırken aynı hesaptaki bir depolama kuyruğuna yazma işlemi kullanabilirsiniz. 

1. Projenin kökünden, önceki hızlı başlangıçta işlev uygulamanızın adıyla değiştirerek `<app_name>` aşağıdaki komutu çalıştırın. Bu komut, dosyadaki varolan tüm değerlerin üzerine yazar.

    ```
    func azure functionapp fetch-app-settings <app_name>
    ```
    
1. *Local. Settings. JSON* ' u açın ve depolama hesabı `AzureWebJobsStorage`bağlantı dizesi olan adlı değeri bulun. Bu makalenin diğer bölümlerinde `AzureWebJobsStorage` adı ve bağlantı dizesini kullanırsınız.

> [!IMPORTANT]
> *Local. Settings. JSON* , Azure 'dan indirilen gizli dizileri içerdiğinden, bu dosyayı her zaman kaynak denetiminden dışlayın. Yerel işlevler projesiyle oluşturulan *. gitignore* dosyası, varsayılan olarak dosyayı dışlar.