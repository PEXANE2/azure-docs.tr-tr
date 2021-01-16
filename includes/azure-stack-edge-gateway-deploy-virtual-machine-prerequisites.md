---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 01/15/2021
ms.author: alkohli
ms.openlocfilehash: 56fc24966fa60c3a5e91f92b57332ae2f6a525ff
ms.sourcegitcommit: 25d1d5eb0329c14367621924e1da19af0a99acf1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98256241"
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


