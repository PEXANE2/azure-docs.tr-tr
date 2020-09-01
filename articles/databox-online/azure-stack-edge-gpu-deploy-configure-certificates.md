---
title: Azure portal 'de GPU ile Azure Stack Edge cihazının sertifikalarını yapılandırma öğreticisi | Microsoft Docs
description: Azure Stack Edge dağıtımı öğreticisi, fiziksel cihazınızda sertifika yapılandırmanızı sağlar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 08/29/2020
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to configure certificates for Azure Stack Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 1f8e0464eb7f513149dd1cfa2ec5dcdc0f193417
ms.sourcegitcommit: 3fb5e772f8f4068cc6d91d9cde253065a7f265d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89181231"
---
# <a name="tutorial-configure-certificates-for-your-azure-stack-edge-with-gpu"></a>Öğretici: GPU ile Azure Stack Edge için sertifikaları yapılandırma

Bu öğreticide, yerel Web Kullanıcı arabirimini kullanarak Azure Stack Edge cihazınız için sertifikaları, yerleşik bir GPU ile nasıl yapılandırabileceğiniz açıklanmaktadır.

Bu adım için geçen süre, seçtiğiniz belirli seçeneğe ve sertifika akışının ortamınızda nasıl Kuruladığına bağlı olarak değişebilir.

Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
>
> * Ön koşullar
> * Fiziksel cihaz için sertifikaları yapılandırma

## <a name="prerequisites"></a>Ön koşullar

Azure Stack Edge cihazınızı GPU ile yapılandırmadan ve ayarlamadan önce şunları yaptığınızdan emin olun:

* Fiziksel cihazı [yükleme Azure Stack Edge](azure-stack-edge-gpu-deploy-install.md)bölümünde ayrıntılı olarak yüklediniz.
* Kendi sertifikalarınızı getirmeyi planlıyorsanız:
    - İmza Zinciri sertifikası da dahil olmak üzere, sertifikalarınızın uygun biçimde hazırlanmalıdır.
<!--    - If your device is deployed in Azure Government or Azure Government Secret or Azure Government top secret cloud and not deployed in Azure public cloud, a signing chain certificate is required before you can activate your device. 
    For details on certificate, go to [Manage certificates](azure-stack-edge-j-series-manage-certificates.md).-->


## <a name="configure-certificates-for-device"></a>Cihaz için sertifikaları yapılandırma


1. **Güvenlik** kutucuğunda, sertifikalar için **Yapılandır** ' ı seçin. 

    ![Yerel Web Kullanıcı arabirimi "Sertifikalar" sayfası](./media/azure-stack-edge-gpu-deploy-configure-certificates/generate-certificate-1.png)

2. Cihaz **kurulumu** kutucuğunda cihaz adını veya DNS etki alanını değiştirdiğinize bağlı olarak, sertifikalarınız için aşağıdaki seçeneklerden birini belirleyebilirsiniz.

    - Önceki adımda cihaz adını veya DNS etki alanını değiştirmediyseniz ve kendi sertifikalarınızı getirmek istemiyorsanız, bu adımı atlayabilir ve sonraki adıma geçebilirsiniz. Cihaz, ile başlamak için otomatik olarak imzalanan sertifikalar oluşturdu. 

        ![Yerel Web Kullanıcı arabirimi "Sertifikalar" sayfası](./media/azure-stack-edge-gpu-deploy-configure-certificates/generate-certificate-2.png)

    - Cihaz adını veya DNS etki alanını değiştirdiyseniz, cihazınızı başarıyla etkinleştirmek için aşağıdaki seçeneklerden birini belirleyebilirsiniz: 
    
        - **Tüm cihaz sertifikalarını oluştur**. Cihaz sertifikaları yalnızca test için kullanılmalıdır ve üretim iş yükleri ile kullanılmaz. * * daha fazla bilgi Için [Azure Stack kenarunuzda cihaz sertifikaları oluşturma](#generate-device-certificates)bölümüne gidin.

        - **Kendi sertifikalarınızı getirin**. Kendi imzalı uç nokta sertifikalarınızı ve buna karşılık gelen imzalama zincirlerini getirebilirsiniz. Önce imzalama zincirini ekleyin ve ardından uç nokta sertifikalarını karşıya yükleyin. **Üretim iş yükleri için her zaman kendi sertifikalarınızı getirmenizi öneririz.** Daha fazla bilgi için [Azure Stack Edge cihazınızda kendi sertifikalarınızı getirme](#bring-your-own-certificates)bölümüne gidin.
    
        - Kendi sertifikalarınızı getirip bazı cihaz sertifikaları oluşturabilirsiniz. **Sertifika oluştur** seçeneği yalnızca cihaz sertifikalarını yeniden oluşturur.

    - Cihaz adı veya DNS etki alanını değiştirdiyseniz ve sertifika oluşturmadıysanız veya kendi sertifikalarınızı getirmediyseniz etkinleştirme engellenir.


### <a name="generate-device-certificates"></a>Cihaz sertifikaları oluşturma

Cihaz sertifikaları oluşturmak için aşağıdaki adımları izleyin.

Azure Stack Edge cihaz sertifikalarını yeniden oluşturmak ve indirmek için bu adımları kullanın:

1. Cihazınızın yerel kullanıcı arabiriminde **yapılandırma > sertifikaları**' na gidin. **Sertifika oluştur**' u seçin.

    ![Sertifika Oluştur ve indir 1](./media/azure-stack-edge-gpu-deploy-configure-certificates/generate-certificate-3.png)

2. **Cihaz sertifikaları oluştur**' da **Oluştur**' u seçin.

    ![Sertifika Oluştur ve indir 2](./media/azure-stack-edge-gpu-deploy-configure-certificates/generate-certificate-4.png)

    Cihaz sertifikaları artık oluşturulup uygulanır. 
    
    > [!IMPORTANT]
    > Sertifika oluşturma işlemi devam ederken, kendi sertifikalarınızı getirmeyin ve **+ sertifika ekle** seçeneği aracılığıyla bunları eklemeyi deneyin.

    İşlem başarıyla tamamlandığında size bildirilir. Olası önbellek sorunlarından kaçınmak için tarayıcınızı yeniden başlatın. 
    
    ![Sertifika Oluştur ve indir 4](./media/azure-stack-edge-gpu-deploy-configure-certificates/generate-certificate-5.png)

3. **Sertifikalar** sayfasında, **indirme** sütununun artık doldurulmuş olduğunu ve yeniden oluşturulan sertifikaların indirileceği bağlantıların kullanılabildiğini görürsünüz. 

    ![Sertifika Oluştur ve indir 5](./media/azure-stack-edge-gpu-deploy-configure-certificates/generate-certificate-6.png)

4. Bir sertifika için indirme bağlantısını seçin ve istendiğinde sertifikayı kaydedin. 

    ![Sertifika Oluştur ve indir 6](./media/azure-stack-edge-gpu-deploy-configure-certificates/generate-certificate-7.png)

5. İndirmek istediğiniz tüm sertifikalar için bu işlemi tekrarlayın. 
    
    ![Sertifika oluşturma ve indirme 7](./media/azure-stack-edge-gpu-deploy-configure-certificates/generate-certificate-8.png)

    Cihaz tarafından oluşturulan sertifikalar aşağıdaki ad biçimiyle DER sertifikaları olarak kaydedilir: 

    `<Device name>_<Endpoint name>.cer`. Bu sertifikalar, cihazda yüklü karşılık gelen sertifikaların ortak anahtarını içerir. 

Bu sertifikaları Ao cihazında uç noktalara erişmek için kullandığınız istemci sistemine yüklemeniz gerekir. Bu sertifikalar istemciyle cihaz arasında güven kurar.

Bu sertifikaları cihaza erişmek için kullandığınız istemciye içeri ve dışarı aktarmak için [Azure Stack Edge cihazınıza erişen istemcilerde sertifikaları Içeri aktarma](azure-stack-edge-j-series-manage-certificates.md#import-certificates-on-the-client-accessing-the-device)' daki adımları izleyin. 

Azure Depolama Gezgini kullanıyorsanız, istemcinizi pek biçiminde yüklemeniz gerekir ve cihaz tarafından üretilen sertifikaları ped biçimine dönüştürmeniz gerekir. 

> [!IMPORTANT]
> - İndirme bağlantısı yalnızca cihaz tarafından oluşturulan sertifikalar için kullanılabilir ve kendi sertifikalarınızı getirdiğinizde değildir.
> - Diğer sertifika gereksinimleri karşılandığında, cihaz tarafından üretilen sertifikaların bir karışımına sahip olmak ve kendi sertifikalarınızı getirmek için karar verebilirsiniz. Daha fazla bilgi için [sertifika gereksinimleri](azure-stack-edge-j-series-certificate-requirements.md)' ne gidin.
    

### <a name="bring-your-own-certificates"></a>Kendi sertifikalarınızı getir

İmzalama zinciri dahil kendi sertifikalarınızı eklemek için bu adımları izleyin.

1. Sertifikayı karşıya yüklemek için **sertifika** sayfasında **+ sertifika ekle**' yi seçin.

    ![Yerel Web Kullanıcı arabirimi "Sertifikalar" sayfası](./media/azure-stack-edge-gpu-deploy-configure-certificates/add-certificate-1.png)

2. Önce imzalama zincirini karşıya yükleyin ve **& Ekle**' yi seçin.

    ![Yerel Web Kullanıcı arabirimi "Sertifikalar" sayfası](./media/azure-stack-edge-gpu-deploy-configure-certificates/add-certificate-2.png)

3. Artık diğer sertifikaları karşıya yükleyebilirsiniz. Örneğin, Azure Resource Manager ve BLOB depolama uç noktası sertifikalarını karşıya yükleyebilirsiniz.

    ![Yerel Web Kullanıcı arabirimi "Sertifikalar" sayfası](./media/azure-stack-edge-gpu-deploy-configure-certificates/add-certificate-3.png)

    Yerel Web Kullanıcı arabirimi sertifikasını da karşıya yükleyebilirsiniz. Bu sertifikayı karşıya yükledikten sonra tarayıcınızı başlatmanız ve Önbelleği temizlemeniz gerekir. Daha sonra cihazın yerel Web Kullanıcı arabirimine bağlanmanız gerekir.  

    ![Yerel Web Kullanıcı arabirimi "Sertifikalar" sayfası](./media/azure-stack-edge-gpu-deploy-configure-certificates/add-certificate-5.png)

    Düğüm sertifikasını da karşıya yükleyebilirsiniz.

    ![Yerel Web Kullanıcı arabirimi "Sertifikalar" sayfası](./media/azure-stack-edge-gpu-deploy-configure-certificates/add-certificate-4.png)

    Sertifika sayfasının yeni eklenen sertifikaları yansıtacak şekilde güncelleştirilmesi gerekir.

    ![Yerel Web Kullanıcı arabirimi "Sertifikalar" sayfası](./media/azure-stack-edge-gpu-deploy-configure-certificates/add-certificate-6.png)  

    > [!NOTE]
    > Azure genel bulutu haricinde, imzalama zinciri sertifikalarının tüm bulut yapılandırmalarında (Azure Kamu veya Azure Stack) etkinleştirilmesinden önce getirilmesi gerekir.


Cihazınız artık etkinleştirilmeye hazırdır.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
>
> * Ön koşullar
> * Fiziksel cihaz için sertifikaları yapılandırma

Azure Stack Edge cihazınızı nasıl etkinleştireceğinizi öğrenmek için bkz.:

> [!div class="nextstepaction"]
> [Azure Stack Edge cihazını etkinleştir](./azure-stack-edge-gpu-deploy-activate.md)
