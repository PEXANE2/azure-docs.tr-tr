---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 12/21/2020
ms.author: alkohli
ms.openlocfilehash: f2443765ecc9116193cefbc729ced25fa5657e59
ms.sourcegitcommit: 799f0f187f96b45ae561923d002abad40e1eebd6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/24/2020
ms.locfileid: "97763446"
---
Azure Stack Edge cihazınızda VM 'Leri dağıtabilmeniz için önce, istemcinizi Azure PowerShell üzerinden Azure Resource Manager aracılığıyla cihaza bağlanacak şekilde yapılandırmanız gerekir. Ayrıntılı adımlar için [Azure Stack Edge cihazınızda Azure Resource Manager bağlanma](../articles/databox-online/azure-stack-edge-j-series-connect-resource-manager.md)bölümüne gidin.


İstemcinizden cihaza erişmek için aşağıdaki adımların kullanılabildiğinden emin olun (Bu yapılandırmayı Azure Resource Manager bağlanırken gerçekleştirdiyseniz, yalnızca yapılandırmanın başarılı olduğunu doğrulamanız gerekir.): 

1. Azure Resource Manager iletişiminin çalıştığını doğrulayın. Şunu yazın:     

    ```powershell
    Add-AzureRmEnvironment -Name <Environment Name> -ARMEndpoint "https://management.<appliance name>.<DNSDomain>"
    ```

1. Kimlik doğrulamak için yerel cihaz API 'Lerini çağırın. Şunu yazın: 

    `login-AzureRMAccount -EnvironmentName <Environment Name>`

    Azure Resource Manager aracılığıyla bağlanmak için Kullanıcı adı- *Edgearmuser* ve parola sağlayın.

1. Kubernetes için **işlem** yapılandırdıysanız, bu adımı atlayabilirsiniz. İşlem için bir ağ arabirimini etkinleştirdiğinizden emin olmak için devam edin. Yerel Kullanıcı arabiriminde, **işlem** ayarları ' na gidin. Sanal anahtar oluşturmak için kullanacağınız ağ arabirimini seçin. Oluşturduğunuz VM 'Ler, bu bağlantı noktasına ve ilişkili ağa bağlı bir sanal anahtara eklenecektir. VM için kullanacağınız IP adresiyle eşleşen bir ağ seçtiğinizden emin olun.  

    ![İşlem ayarlarını etkinleştir 1](../articles/databox-online/media/azure-stack-edge-gpu-deploy-virtual-machine-templates/enable-compute-setting.png)

    Ağ arabiriminde işlem etkinleştirin. Azure Stack Edge, bu ağ arabirimine karşılık gelen bir sanal anahtar oluşturur ve yönetir. Şu anda Kubernetes için belirli IP 'Leri girmeyin. İşlem, işlemin etkinleştirilmesi birkaç dakika sürebilir.

    > [!NOTE]
    > GPU VM 'Leri oluşturuyorsanız, Internet 'e bağlı bir ağ arabirimi seçin. Bu, cihazınıza GPU uzantısı yüklemenizi sağlar.


1. Azure portal VM rolünü etkinleştirin. Bu adım, cihazınızın yerel API 'Leri aracılığıyla VM oluşturmak için kullanılan, cihazınız için benzersiz bir abonelik oluşturur. 

    1. VM rolünü etkinleştirmek için, Azure portal, Azure Stack Edge cihazınız için Azure Stack Edge kaynağına gidin. **Uç işlem > sanal makinelere** gidin.

        ![VM görüntüsü Ekle 1](../articles/databox-online/media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-image-1.png)

    1. **Genel bakış** sayfasına gitmek Için **sanal makineleri** seçin. Sanal makine bulutu yönetimini **etkinleştirin** .
        ![VM görüntüsü Ekle 2](../articles/databox-online/media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-image-2.png)