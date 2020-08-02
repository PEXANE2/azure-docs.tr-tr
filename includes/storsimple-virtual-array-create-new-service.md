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
ms.openlocfilehash: b7bdeeedaac65f67a3224e824c19e8cad794682b
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87507169"
---
#### <a name="to-create-a-new-service"></a>Yeni hizmet oluşturmak için

1.  Microsoft hesabı kimlik bilgilerinizi kullanarak bu URL 'de Azure portal oturum açın: <https://portal.azure.com/> . Cihazı kamu portalında dağıtıyorsanız şurada oturum açın:<https://portal.azure.us/>

2.  Azure Portal **+ kaynak** &gt; **depolaması** &gt; **StorSimple Sanal serisi**' ne tıklayın.

    ![Yeni hizmet oluştur](./media/storsimple-virtual-array-create-new-service/createnewservice2.png) 

3.  Açılan **StorSimple aygıt yöneticisi** dikey penceresinde aşağıdakileri yapın:

    1.  Hizmetiniz için benzersiz bir **Kaynak adı** sağlayın. Kaynak adı, hizmeti tanımlamak için kullanılabilecek kolay bir addır. Ad harf, rakam ve tirelerden oluşan 2-50 karakter arası uzunlukta olabilir. Ad bir harf veya sayıyla başlamalı ve bitmelidir.

    2.  Açılan listeden bir **Abonelik** seçin. Abonelik fatura hesabınıza bağlıdır. Bu alan bir aboneliğiniz olmadığı sürece yoktur.

    3.  **Kaynak grubu**için var olanı seçin veya yeni bir grup oluşturun. Daha fazla bilgi edinmek için bkz. [Azure kaynak grupları](/azure/azure-resource-manager/management/manage-resource-groups-portal).

    4.  Hizmetiniz için bir **Konum** sağlayın. Hangi bölgede hangi hizmetlerin kullanılabildiği hakkında daha fazla bilgi için bkz. [Azure bölgeleri](https://azure.microsoft.com/regions/#services) . Genel olarak, cihazınızı dağıtmak istediğiniz coğrafi bölgeye en yakın **konumu** seçin. Aşağıdakilerin de etkili olmasını isteyebilirsiniz:

        -   Azure 'da StorSimple aygıtınızla dağıtmayı planladığınız mevcut iş yükleriniz varsa, bu veri merkezini kullanmanızı öneririz.

        -   StorSimple Aygıt Yöneticisi ve Azure Storage iki ayrı konumda olabilir. Böyle bir durumda, StorSimple Cihaz Yöneticisi ve Azure Storage hesabını ayrı ayrı oluşturmanız gerekir. Bir Azure depolama hesabı oluşturmak için Azure portal Azure Storage ' a gidin ve [depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)bölümünde açıklanan adımları izleyin. Bu hesabı oluşturduktan sonra, [Hizmet için yeni bir depolama hesabı yapılandırma](https://azure.microsoft.com/documentation/articles/storsimple-deployment-walkthrough/#configure-a-new-storage-account-for-the-service) konusundaki adımları uygulayarak bunu StorSimple Cihaz Yöneticisi hizmetine ekleyin.

        -   Sanal cihazı kamu portalında dağıtıyorsanız, StorSimple Aygıt Yöneticisi hizmeti ABD Iowa ve ABD Virginia konumlarında kullanılabilir.

    5.  Hizmeti kullanarak otomatik olarak bir depolama hesabı oluşturmak için **Yeni bir Azure depolama hesabı oluştur** ' u seçin. Bir **depolama hesabı adı**belirtin. Verilerinizin farklı bir konumda olması gerekiyorsa bu kutunun işaretini kaldırın.

    6.  Panonuzda bu hizmetin hızlı bağlantısının olmasını istiyorsanız **Panoya sabitle** seçeneğini işaretleyin.

    7.  StorSimple Cihaz Yöneticisi’ni oluşturmak için **Oluştur**’a tıklayın.

        ![Yeni hizmet oluştur](./media/storsimple-virtual-array-create-new-service/createnewservice4.png)  

**Hizmet** giriş sayfasına yönlendirilirsiniz. Hizmetin oluşturulması birkaç dakika sürer. Hizmet sorunsuz oluşturulduktan sonra, uygun şekilde size bildirilir ve hizmetin durumu **Etkin** olarak değişir.


