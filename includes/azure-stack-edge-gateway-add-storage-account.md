---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 08/30/2020
ms.author: alkohli
ms.openlocfilehash: 30ca4d330d9b16214396ac81e5ab5722ca0e7569
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89254312"
---
1. [Azure Portal](https://portal.azure.com/), Azure Stack Edge kaynağını seçin ve **genel bakış**' a gidin. Cihazınızın çevrimiçi olması gerekir.

2. Cihaz komut çubuğunda **+ depolama hesabı ekle** ' yi seçin. 

   ![Depolama hesabı ekleme](media/azure-stack-edge-gateway-add-storage-account/add-storage-account-1.png)

3. **Edge depolama hesabı ekle** bölmesinde, aşağıdaki ayarları belirtin:

    a. Cihazınızdaki Edge depolama hesabı için benzersiz bir ad. Depolama hesabı adları, yalnızca küçük harf ve harf içerebilir. Özel karakterlere izin verilmez. Depolama hesabı adı, cihaz içinde (cihazlarda değil) benzersiz olmalıdır.

    b. Depolama hesabının tutulduğu veriler hakkında isteğe bağlı bir açıklama.  
    
    c. Varsayılan olarak, sınır depolama hesabı buluttaki bir Azure depolama hesabıyla eşlenir ve depolama hesabındaki veriler otomatik olarak buluta gönderilir. Edge depolama hesabınızın eşlendiği Azure Depolama hesabını belirtin.  

    d. Sonra yeni bir kapsayıcı oluşturun veya Azure Storage hesabındaki bir kapsayıcıdan seçin. Uç depolama hesabına yazılan cihazdaki tüm veriler, eşlenen Azure depolama hesabındaki seçili depolama kapsayıcısına otomatik olarak yüklenir.

    <!--![Add a storage account](media/azure-stack-edge-gateway-add-storage-account/add-storage-account-2.png)-->

    e. Tüm depolama hesabı seçenekleri belirtilmişse, kenar depolama hesabını oluşturmak için **Ekle** ' yi seçin. Sınır depolama hesabı başarıyla oluşturulduğunda size bildirilir. Yeni Edge depolama hesabı daha sonra Azure portal depolama hesapları listesinde görüntülenir. 

    
4. Bu yeni depolama hesabını seçer ve **erişim anahtarlarına**giderseniz, blob hizmeti uç noktasını ve karşılık gelen depolama hesabı adını bulabilirsiniz. Bu bilgileri erişim anahtarlarıyla birlikte, sınır depolama hesabına bağlanmanıza yardımcı olacak şekilde kopyalayın.

    ![Depolama hesabı ekleme](media/azure-stack-edge-gateway-add-storage-account/add-storage-account-4.png)

    [Azure Resource Manager kullanarak, cihaz yerel API 'Lerine bağlanarak](../articles/databox-online/azure-stack-edge-j-series-connect-resource-manager.md)erişim anahtarlarını alırsınız. 
