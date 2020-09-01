---
title: Azure portal 'de GPU ile Azure Stack Edge cihazını etkinleştirme öğreticisi | Microsoft Docs
description: Azure Stack Edge GPU 'YU dağıtmaya yönelik öğretici, fiziksel cihazınızı etkinleştirmenizi sağlar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 08/31/2020
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to activate Azure Stack Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 88be4d9753e48f70dae5666e800a54209ed6ba3f
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89267947"
---
# <a name="tutorial-activate-azure-stack-edge-with-gpu"></a>Öğretici: GPU ile Azure Stack Edge 'i etkinleştirme

Bu öğretici, yerel Web Kullanıcı arabirimini kullanarak Azure Stack Edge cihazınızı birlikte bulunan bir GPU ile nasıl etkinleştirebileceğinizi açıklar.

Etkinleştirme işleminin tamamlanması 5 dakika sürebilir.

Bu öğreticide hakkında bilgi edindiniz:

> [!div class="checklist"]
> * Ön koşullar
> * Fiziksel cihazı etkinleştirin

## <a name="prerequisites"></a>Ön koşullar

Azure Stack Edge cihazınızı GPU ile yapılandırmadan ve ayarlamadan önce şunları yaptığınızdan emin olun:

* Fiziksel cihazınız için: 
    
    - Fiziksel cihazı [yükleme Azure Stack Edge](azure-stack-edge-gpu-deploy-install.md)bölümünde ayrıntılı olarak yüklediniz.
    - Ağı [, işlem ağını, Web proxy 'Sini yapılandırma](azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy.md) bölümünde ayrıntılı olarak ağ ve işlem ağ ayarlarını yapılandırdınız.
    - Cihaz adı veya DNS etki alanını **cihaz** sayfası aracılığıyla değiştirdiyseniz, kendi aygıtınızı karşıya yüklediniz veya cihazınızda cihaz sertifikaları oluşturmuş olursunuz. Bu adımı yapmadıysanız, cihaz etkinleştirme sırasında bir hata görürsünüz ve etkinleştirme engellenir. Daha fazla bilgi için [sertifikaları yapılandırma](azure-stack-edge-gpu-deploy-configure-certificates.md)bölümüne gidin.
    
* Azure Stack Edge cihazını yönetmek için oluşturduğunuz Azure Stack Edge hizmetinden etkinleştirme anahtarınız vardır. Daha fazla bilgi için, [Azure Stack Edge dağıtmaya hazırlanma](azure-stack-edge-gpu-deploy-prep.md)bölümüne gidin.


## <a name="activate-the-device"></a>Cihazı Etkinleştir

1. Cihazın yerel Web Kullanıcı arabiriminde, **Başlarken** sayfasına gidin.
2. **Etkinleştirme** kutucuğunda **Etkinleştir**' i seçin. 

    ![Yerel Web Kullanıcı arabirimi "bulut ayrıntıları" sayfası](./media/azure-stack-edge-gpu-deploy-activate/activate-1.png)
    
3. **Etkinleştir** bölmesinde [Azure Stack Edge Için etkinleştirme anahtarını al](azure-stack-edge-gpu-deploy-prep.md#get-the-activation-key)bölümünde aldığınız **etkinleştirme anahtarını** girin.

4. **Uygula**’yı seçin.

    ![Yerel Web Kullanıcı arabirimi "bulut ayrıntıları" sayfası](./media/azure-stack-edge-gpu-deploy-activate/activate-2.png)


5. İlk olarak cihaz etkinleştirilir. Daha sonra anahtar dosyasını indirmeniz istenir.
    
    ![Yerel Web Kullanıcı arabirimi "bulut ayrıntıları" sayfası](./media/azure-stack-edge-gpu-deploy-activate/activate-3.png)
    
    **Anahtar dosyasını indir** ' i seçin ve *keys.js* dosyayı cihazın dışından güvenli bir konuma kaydedin. **Bu anahtar dosya, cihazınızdaki işletim sistemi diski ve veri diskleri için kurtarma anahtarlarını içerir**. Dosyadaki *keys.js* içeriği aşağıda verilmiştir:

        
    ```json
    {
      "Id": "1ab3fe39-26e6-4984-bb22-2e02d3fb147e",
      "DataVolumeBitLockerExternalKeys": {
        "hcsinternal": "C086yg1DrPo0DuZB/a7hUh+kBWj804coJfBA9LDzZqw=",
        "hcsdata": "8ohX9bG3YSZl9DZmZLkYl//L9dXi1XiQrqza+iSd64Q="
      },
      "SystemVolumeBitLockerRecoveryKey": "105347-156739-594473-151107-005082-252604-471955-439395",
      "ServiceEncryptionKey": "oEwxNJeULzGRFt6DsLgcLw=="
    }
    ```
        
 
    Aşağıdaki tabloda çeşitli anahtarlar açıklanmaktadır:
    
    |Alan  |Açıklama  |
    |---------|---------|
    |`Id`    | Bu, cihazın KIMLIĞIDIR.        |
    |`DataVolumeBitLockerExternalKeys`|Bunlar, veri diskleri için bit Lockers anahtarlardır ve cihazınızdaki yerel verileri kurtarmak için kullanılır.|
    |`SystemVolumeBitLockerRecoveryKey`| Bu, sistem birimi için BitLocker anahtarıdır. Bu anahtar, cihazınızın sistem yapılandırma ve Sistem verilerinin kurtarılmasına yardımcı olur. |
    |`ServiceEncryptionKey`| Bu anahtar, Azure hizmeti üzerinden akan verileri korur. Bu anahtar, Azure hizmeti güvenliğinin tehlikeye girmesi, depolanan bilgilerin güvenliğinin aşılmasına neden olmaz. |

6. **Genel bakış** sayfasına gidin. Cihaz durumu, **etkinleştirilmiş**olarak gösterilmelidir.

    ![Yerel Web Kullanıcı arabirimi "bulut ayrıntıları" sayfası](./media/azure-stack-edge-gpu-deploy-activate/activate-4.png)
 
Cihaz etkinleştirme işlemi tamamlanmıştır. Artık cihazınıza paylaşımlar ekleyebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide hakkında bilgi edindiniz:

> [!div class="checklist"]
> * Ön koşullar
> * Fiziksel cihazı etkinleştirin

Azure Stack Edge cihazınızla veri aktarmayı öğrenmek için bkz.:

> [!div class="nextstepaction"]
> [Azure Stack Edge ile veri aktarımı](./azure-stack-edge-j-series-deploy-add-shares.md)
