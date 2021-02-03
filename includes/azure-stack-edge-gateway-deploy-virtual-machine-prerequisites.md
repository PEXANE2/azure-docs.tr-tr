---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 01/15/2021
ms.author: alkohli
ms.openlocfilehash: 8f3031669723cd61715c12a42f99869ac0eaf3bc
ms.sourcegitcommit: 445ecb22233b75a829d0fcf1c9501ada2a4bdfa3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99500396"
---
Azure Stack Edge cihazınızda VM 'Leri dağıtabilmeniz için önce, istemcinizi Azure PowerShell üzerinden Azure Resource Manager aracılığıyla cihaza bağlanacak şekilde yapılandırmanız gerekir. Ayrıntılı adımlar için [Azure Stack Edge cihazınızda Azure Resource Manager bağlanma](../articles/databox-online/azure-stack-edge-j-series-connect-resource-manager.md)bölümüne gidin.

Aşağıdaki adımların, cihazınızdan cihaza erişmek için kullanılabilir olduğundan emin olun. (Azure Resource Manager bağlanırken bu yapılandırmayı tamamladınız. Artık yalnızca yapılandırmanın başarılı olduğunu doğrulıyoruz.) 

1. Azure Resource Manager iletişiminin çalıştığını doğrulayın. Şunları girin:     

    ```powershell
    Add-AzureRmEnvironment -Name <Environment Name> -ARMEndpoint "https://management.<appliance name>.<DNSDomain>"
    ```

1. Kimlik doğrulamak için yerel cihaz API 'Lerini çağırın. Şunları girin: 

    `login-AzureRMAccount -EnvironmentName <Environment Name>`

    Azure Resource Manager aracılığıyla bağlanmak için Kullanıcı adı- *Edgearmuser* ve parola sağlayın.

1. Kubernetes için **işlem** yapılandırdıysanız, bu adımı atlayabilirsiniz. İşlem için bir ağ arabirimini etkinleştirdiğinizden emin olmak için devam edin. Yerel Kullanıcı arabiriminizdeki **işlem** ayarları ' na gidin. Sanal anahtar oluşturmak için kullanmak istediğiniz ağ arabirimini seçin. Oluşturduğunuz VM 'Ler, bu bağlantı noktasına ve ilişkili ağa bağlı bir sanal anahtara eklenecektir. VM için kullanacağınız IP adresiyle eşleşen bir ağ seçtiğinizden emin olun.  

    ![İşlem ayarlarının nasıl etkinleştirileceğini gösteren ekran görüntüsü.](../articles/databox-online/media/azure-stack-edge-gpu-deploy-virtual-machine-templates/enable-compute-setting.png)

    Ağ arabiriminde işlem etkinleştirin. Azure Stack Edge, bu ağ arabirimine karşılık gelen bir sanal anahtar oluşturur ve yönetir. Şu anda Kubernetes için belirli IP 'Leri girmeyin. İşlem, işlemin etkinleştirilmesi birkaç dakika sürebilir.

    > [!NOTE]
    > GPU VM 'Leri oluşturuyorsanız, internet 'e bağlı bir ağ arabirimi seçin. Bu, cihazınıza bir GPU uzantısı yüklemenizi sağlar.


