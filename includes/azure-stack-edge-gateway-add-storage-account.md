---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 01/04/2021
ms.author: alkohli
ms.openlocfilehash: 5f39f727deaf3a53db5e2928e5af23779c298318
ms.sourcegitcommit: d7d5f0da1dda786bda0260cf43bd4716e5bda08b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2021
ms.locfileid: "97911527"
---
1. [Azure Portal](https://portal.azure.com/), Azure Stack Edge kaynağını seçin ve **genel bakış**' a gidin. Cihazınızın çevrimiçi olması gerekir. **Bulut depolama ağ geçidi > depolama hesaplarına** gidin.

2. Cihaz komut çubuğunda **+ depolama hesabı ekle** ' yi seçin. 

   ![Depolama hesabı ekleme](media/azure-stack-edge-gateway-add-storage-account/add-storage-account-1.png)

3. **Edge depolama hesabı ekle** bölmesinde, aşağıdaki ayarları belirtin:

    a. Cihazınızdaki Edge depolama hesabı için benzersiz bir ad. Depolama hesabı adları, yalnızca küçük harf ve harf içerebilir. Özel karakterlere izin verilmez. Depolama hesabı adı, cihaz içinde (cihazlarda değil) benzersiz olmalıdır.

    b. Depolama hesabının tutulduğu veriler hakkında isteğe bağlı bir açıklama.  
    
    c. Varsayılan olarak, sınır depolama hesabı buluttaki bir Azure depolama hesabıyla eşlenir ve depolama hesabındaki veriler otomatik olarak buluta gönderilir. Edge depolama hesabınızın eşlendiği Azure Depolama hesabını belirtin.  

    d. Sonra yeni bir kapsayıcı oluşturun veya Azure Storage hesabındaki bir kapsayıcıdan seçin. Uç depolama hesabına yazılan cihazdaki tüm veriler, eşlenen Azure depolama hesabındaki seçili depolama kapsayıcısına otomatik olarak yüklenir.

    <!--![Add a storage account](media/azure-stack-edge-gateway-add-storage-account/add-storage-account-2.png)-->

    e. Tüm depolama hesabı seçenekleri belirtilmişse, kenar depolama hesabını oluşturmak için **Ekle** ' yi seçin. Sınır depolama hesabı başarıyla oluşturulduğunda size bildirilir. Yeni Edge depolama hesabı daha sonra Azure portal depolama hesapları listesinde görüntülenir. 

    
4. Bu yeni depolama hesabını seçer ve **erişim anahtarlarına** giderseniz, blob hizmeti uç noktasını ve karşılık gelen depolama hesabı adını bulabilirsiniz. Bu bilgileri erişim anahtarlarıyla birlikte, sınır depolama hesabına bağlanmanıza yardımcı olacak şekilde kopyalayın.

    ![Depolama hesabı ekleme 2](media/azure-stack-edge-gateway-add-storage-account/add-storage-account-4.png)

    [Azure Resource Manager kullanarak, cihaz yerel API 'Lerine bağlanarak](../articles/databox-online/azure-stack-edge-j-series-connect-resource-manager.md)erişim anahtarlarını alırsınız. 
