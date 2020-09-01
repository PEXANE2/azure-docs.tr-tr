---
title: Azure portal ' de GPU ile Azure Stack Edge cihazını bağlama, yapılandırma, etkinleştirme öğreticisi | Microsoft Docs
description: Azure Stack Edge GPU 'YU dağıtmaya yönelik öğretici, fiziksel cihazınızı bağlanmanızı, ayarlamanıza ve etkinleştirmenizi sağlar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 08/29/2020
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to connect and activate Azure Stack Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 41055fbd455d3f7b9da63ee8f7420f008ea75a00
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89254517"
---
# <a name="tutorial-configure-device-settings-for-azure-stack-edge-with-gpu"></a>Öğretici: GPU ile Azure Stack Edge için cihaz ayarlarını yapılandırma

Bu öğreticide, yerleşik bir GPU ile Azure Stack Edge cihazınız için cihazla ilgili ayarları nasıl yapılandıracağınız açıklanmaktadır. Yerel Web Kullanıcı arabirimi aracılığıyla cihaz adınızı, güncelleştirme sunucusunu ve zaman sunucusunu ayarlayabilirsiniz.

Cihaz ayarlarının tamamlanması yaklaşık 5-7 dakika sürebilir.

Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
>
> * Ön koşullar
> * Cihaz ayarlarını yapılandırma
> * Güncelleştirmeyi yapılandır 
> * Saati Yapılandır

## <a name="prerequisites"></a>Ön koşullar

GPU ile Azure Stack Edge cihazınızda cihazla ilgili ayarları yapılandırmadan önce şunları yaptığınızdan emin olun:

* Fiziksel cihazınız için:

    - Fiziksel cihazı [yükleme Azure Stack Edge](azure-stack-edge-gpu-deploy-install.md)bölümünde ayrıntılı olarak yüklediniz.
    - Eğitim bölümünde açıklandığı gibi cihazınızda ağ ve etkinleştirilmiş ve yapılandırılmış işlem ağını [yapılandırın: GPU ile Azure Stack Edge için ağı yapılandırma](azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy.md).


## <a name="configure-device-settings"></a>Cihaz ayarlarını yapılandırma

Cihazla ilgili ayarları yapılandırmak için bu adımları izleyin.
 
1. Cihaz **kurulumu** kutucuğunda **cihaz**için **Yapılandır**' ı seçin.

    ![Yerel Web Kullanıcı arabirimi "cihaz" sayfası](./media/azure-stack-edge-gpu-deploy-set-up-device-update-time/device-1.png)

    **Cihaz** sayfasında, aşağıdaki adımları uygulayın:

    1. Cihazınız için kolay bir ad girin. Kolay ad 1 ile 13 karakter arasında olmalıdır ve harf, rakam ve kısa çizgi içerebilir.

    2. Cihazınız için bir **DNS etki alanı** sağlayın. Bu etki alanı, cihazı bir dosya sunucusu olarak kurmak için kullanılır.

    3. Yapılandırılan cihaz ayarlarını doğrulamak ve uygulamak için **Uygula**' yı seçin.

        ![Yerel Web Kullanıcı arabirimi "cihaz" sayfası](./media/azure-stack-edge-gpu-deploy-set-up-device-update-time/device-2.png)

        Cihaz adını ve DNS etki alanını değiştirdiyseniz, cihazdaki otomatik olarak imzalanan sertifikalar çalışmaz. Sertifikaları yapılandırırken aşağıdaki seçeneklerden birini belirlemeniz gerekir.: 
        
        - Cihaz sertifikaları oluşturun ve indirin. 
        - İmzalama zinciri dahil olmak üzere cihaz için kendi sertifikalarınızı getirin.
    

        ![Yerel Web Kullanıcı arabirimi "cihaz" sayfası](./media/azure-stack-edge-gpu-deploy-set-up-device-update-time/device-3.png)

    4. Cihaz adı ve DNS etki alanı değiştirildiğinde, SMB ve NFS uç noktaları oluşturulur.  

    5. Ayarlar uygulandıktan sonra, **kullanmaya**başlamak için geri dönün.

## <a name="configure-update"></a>Güncelleştirmeyi yapılandır

1. **Cihaz kurulumu** kutucuğunda **güncelleştirme**için **Yapılandır**' ı seçin. Bundan böyle, cihazınızı için güncelleştirmelerin indirileceği konumu yapılandırabilirsiniz.  

    ![Yerel Web Kullanıcı arabirimi "güncelleştirme sunucusu" sayfası](./media/azure-stack-edge-gpu-deploy-set-up-device-update-time/update-1.png)

    - Güncelleştirmeleri doğrudan **Microsoft Update sunucusundan**alabilirsiniz.

        ![Yerel Web Kullanıcı arabirimi "güncelleştirme sunucusu" sayfası](./media/azure-stack-edge-gpu-deploy-set-up-device-update-time/update-2.png)

        Güncelleştirmeleri **Windows Server Update Services** 'dan (WSUS) dağıtmayı da tercih edebilirsiniz. WSUS sunucusunun yolunu belirtin.
        
        ![Yerel Web Kullanıcı arabirimi "güncelleştirme sunucusu" sayfası](./media/azure-stack-edge-gpu-deploy-set-up-device-update-time/update-3.png)

        > [!NOTE] 
        > Ayrı bir Windows Update sunucusu yapılandırıldıysa ve *https* üzerinden bağlanmayı seçerseniz ( *http*yerine), güncelleştirme sunucusuna bağlanmak için gereken imzalama zinciri sertifikaları gereklidir. Sertifika oluşturma ve karşıya yükleme hakkında daha fazla bilgi için, [sertifikaları Yönet](azure-stack-edge-j-series-manage-certificates.md)' e gidin. 

2. **Uygula**’yı seçin.
3. Güncelleştirme sunucusu yapılandırıldıktan sonra, **kullanmaya**başlamak için geri dönün.
    

## <a name="configure-time"></a>Saati Yapılandır

Cihazınızda saat ayarlarını yapılandırmak için bu adımları izleyin. 

1. **Cihaz kurulumu** kutucuğunda **saat**' i seçin. Cihazınız için saat dilimini ve birincil ve ikincil NTP sunucularını seçebilirsiniz.  

    > [!IMPORTANT]
    > Zaman ayarları isteğe bağlı olsa da, cihazınız için yerel ağ üzerinde birincil bir NTP ve ikincil bir NTP sunucusu yapılandırmanızı önemle öneririz. Yerel sunucu kullanılabilir değilse, genel NTP sunucuları yapılandırılabilir.
    
    Cihazınızın bulut hizmeti sağlayıcılarınız ile kimlik doğrulaması yapabilmesi için zaman eşitlemesini gerektiğinden NTP sunucuları gereklidir.

    ![Yerel Web Kullanıcı arabirimi "saat" sayfası](./media/azure-stack-edge-gpu-deploy-set-up-device-update-time/time-1.png)

2. **Zaman** sayfasında, aşağıdakileri yapın:
    
    1. **Saat dilimi** açılan listesinde, cihazın dağıtıldığı coğrafi konuma karşılık gelen saat dilimini seçin.
        Cihazınızın varsayılan saat dilimi PST ' dir. Cihazınız zamanlanan tüm işlemler için bu saat dilimini kullanır.

    2. **BIRINCIL NTP sunucusu** kutusunda, cihazınızın birincil sunucusunu girin veya Time.Windows.com varsayılan değerini kabul edin.  
        Ağınızda NTP trafiğinin veri merkezinizden internet 'e geçmesine izin verdiğinden emin olun.

    3. İsteğe bağlı olarak, **IKINCIL NTP sunucusu** kutusuna cihazınız için bir ikincil sunucu girin.

    4. Yapılandırılan saat ayarlarını doğrulamak ve uygulamak için **Uygula**' yı seçin.

        ![Yerel Web Kullanıcı arabirimi "saat" sayfası](./media/azure-stack-edge-gpu-deploy-set-up-device-update-time/time-2.png)

3. Ayarlar uygulandıktan sonra, **kullanmaya**başlamak için geri dönün.



## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
>
> * Ön koşullar
> * Cihaz ayarlarını yapılandırma
> * Güncelleştirmeyi yapılandır 
> * Saati Yapılandır

Azure Stack Edge cihazınız için sertifikaların nasıl yapılandırılacağını öğrenmek için bkz.:

> [!div class="nextstepaction"]
> [Sertifikaları yapılandırma](./azure-stack-edge-gpu-deploy-configure-certificates.md)
