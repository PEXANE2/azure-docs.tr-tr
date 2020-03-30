---
title: include dosyası
description: include dosyası
services: storage
author: alkohli
ms.service: storage
ms.topic: include
ms.date: 09/15/2018
ms.author: alkohli
ms.custom: include file
ms.openlocfilehash: 4ba5c8b69776b39d8a6640744b0c24600f3a0d5b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188558"
---
#### <a name="to-create-a-new-service"></a>Yeni hizmet oluşturmak için

1.  Microsoft hesap kimlik bilgilerinizi kullanarak, bu URL'de <https://portal.azure.com/>Azure portalında oturum açın: . Aygıtı Devlet portalında dağıtıyorsanız, aşağıdaki leri oturum açın:<https://portal.azure.us/>

2.  Azure portalında &gt; + Kaynak **Depolama** &gt; **StorBasit Sanal Serisi** **oluştur'u** tıklatın.

    ![Yeni hizmet oluşturun](./media/storsimple-virtual-array-create-new-service/createnewservice2.png) 

3.  Açılan **StorSimple Device Manager** bıçağında aşağıdakileri yapın:

    1.  Hizmetiniz için benzersiz bir **Kaynak adı** sağlayın. Kaynak adı, hizmeti tanımlamak için kullanılabilecek kolay bir addır. Ad harf, rakam ve tirelerden oluşan 2-50 karakter arası uzunlukta olabilir. Ad bir harf veya sayıyla başlamalı ve bitmelidir.

    2.  Açılan listeden bir **Abonelik** seçin. Abonelik fatura hesabınıza bağlıdır. Bu alan bir aboneliğiniz olmadığı sürece yoktur.

    3.  **Kaynak grubu**için varolan bir grubu seçin veya yeni bir grup oluşturun. Daha fazla bilgi edinmek için bkz. [Azure kaynak grupları](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-infrastructure-resource-groups-guidelines/).

    4.  Hizmetiniz için bir **Konum** sağlayın. Hangi hizmetlerin hangi bölgede kullanılabilir olduğu hakkında daha fazla bilgi için [Azure Bölgeleri'ne](https://azure.microsoft.com/regions/#services) bakın. Genel olarak, cihazınızı dağıtmak istediğiniz coğrafi bölgeye en yakın **konum** seçin. Aşağıdakilerin de etkili olmasını isteyebilirsiniz:

        -   Azure'da StorSimple aygıtınızla dağıtmayı istediğiniz varolan iş yükleriniz varsa, bu veri merkezini kullanmanızı öneririz.

        -   StorSimple Aygıt Yöneticisi ve Azure depolama alanınız iki ayrı konumda olabilir. Böyle bir durumda, StorSimple Cihaz Yöneticisi ve Azure Storage hesabını ayrı ayrı oluşturmanız gerekir. Bir Azure depolama hesabı oluşturmak için Azure portalında Azure Depolama'ya gidin ve [depolama hesabı oluştur'da](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)açıklanan adımları izleyin. Bu hesabı oluşturduktan sonra, [Hizmet için yeni bir depolama hesabı yapılandırma](https://azure.microsoft.com/documentation/articles/storsimple-deployment-walkthrough/#configure-a-new-storage-account-for-the-service) konusundaki adımları uygulayarak bunu StorSimple Cihaz Yöneticisi hizmetine ekleyin.

        -   Sanal aygıtı Devlet Portalı'nda dağıtıyorsanız, StorSimple Device Manager hizmeti ABD Iowa ve ABD Virginia konumlarında kullanılabilir.

    5.  Hizmetle birlikte otomatik olarak bir depolama hesabı oluşturmak için **yeni bir Azure depolama hesabı oluştur'u** seçin. Bir **Depolama hesabı adı**belirtin. Verilerinizin farklı bir konumda olması gerekiyorsa bu kutunun işaretini kaldırın.

    6.  Panonuzda bu hizmetin hızlı bağlantısının olmasını istiyorsanız **Panoya sabitle** seçeneğini işaretleyin.

    7.  StorSimple Cihaz Yöneticisi’ni oluşturmak için **Oluştur**’a tıklayın.

        ![Yeni hizmet oluşturun](./media/storsimple-virtual-array-create-new-service/createnewservice4.png)  

**Hizmet** açılış sayfasına yönlendirilirsiniz. Hizmetin oluşturulması birkaç dakika sürer. Hizmet sorunsuz oluşturulduktan sonra, uygun şekilde size bildirilir ve hizmetin durumu **Etkin** olarak değişir.


