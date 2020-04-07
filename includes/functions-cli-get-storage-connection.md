---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: 5cb345ef2d20f75066e90f9e6478be27f925b1b0
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673349"
---
### <a name="retrieve-the-azure-storage-connection-string"></a>Azure Depolama bağlantı dizesini alın

Daha önce, işlev uygulaması tarafından kullanılmak üzere bir Azure Depolama hesabı oluşturabilirsiniz. Bu hesabın bağlantı dizesi Azure'daki uygulama ayarlarında güvenli bir şekilde depolanır. Ayarı *local.settings.json* dosyasına indirerek, işlevi yerel olarak çalıştırırken bu bağlantıyı aynı hesaptaki Depolama kuyruğuna yazma'yı kullanabilirsiniz. 

1. Projenin kökünden, önceki hızlı başlangıçtaki işlev `<app_name>` uygulamanızın adını değiştirerek aşağıdaki komutu çalıştırın. Bu komut, dosyadaki varolan değerlerin üzerine yazılır.

    ```
    func azure functionapp fetch-app-settings <app_name>
    ```
    
1. *local.settings.json'u* açın ve `AzureWebJobsStorage`Depolama hesabı bağlantı dizesi olan adlı değeri bulun. Bu makalenin `AzureWebJobsStorage` diğer bölümlerinde adı ve bağlantı dizesini kullanırsınız.

> [!IMPORTANT]
> *local.settings.json* Azure'dan indirilen sırları içerdiğinden, bu dosyayı her zaman kaynak denetiminden hariç tutar. Yerel işlevler projesiyle oluşturulan *.gitignore* dosyası varsayılan olarak dosyayı dışlar.