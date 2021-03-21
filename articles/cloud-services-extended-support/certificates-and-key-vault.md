---
title: Azure Cloud Services'te (genişletilmiş destek) sertifikaları depolama ve kullanma
description: Sertifikaları Azure Cloud Services depolama ve kullanma işlemi (genişletilmiş destek)
ms.topic: how-to
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: 7357703af41afc913ef63dff6ecae3d230c9eca0
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104583301"
---
# <a name="use-certificates-with-azure-cloud-services-extended-support"></a>Sertifikaları Azure Cloud Services kullanma (genişletilmiş destek)

Key Vault, Cloud Services ilişkili sertifikaları (genişletilmiş destek) depolamak için kullanılır. Anahtar kasaları [Azure Portal](../key-vault/general/quick-create-portal.md) ve [PowerShell](../key-vault/general/quick-create-powershell.md)aracılığıyla oluşturulabilir. Sertifikaları Key Vault ekleyin ve ardından hizmet yapılandırma dosyasında sertifika parmak izleriyle referans yapın. Ayrıca, Cloud Services (genişletilmiş destek) kaynağının Key Vault gizli dizi olarak depolanan sertifikayı alabilmesi için uygun izinler için Key Vault etkinleştirmeniz gerekir.  

## <a name="upload-a-certificate-to-key-vault"></a>Key Vault bir sertifika yükleyin 

1.  Azure portal oturum açın ve Key Vault gidin. Ayarlanmış bir Key Vault yoksa, bu pencerede bir tane oluşturmayı tercih edebilirsiniz.

2. **Erişim ilkelerini** seçin

    :::image type="content" source="media/certs-and-key-vault-1.png" alt-text="Görüntü, Anahtar Kasası dikey penceresinden erişim ilkelerini seçmeyi gösterir.":::

3. Erişim ilkelerinin aşağıdaki özellikleri içerdiğinden emin olun:
    - **Dağıtım için Azure sanal makinelerine erişimi etkinleştir**
    - **Şablon dağıtımı için Azure Resource Manager erişimi etkinleştir** 

    :::image type="content" source="media/certs-and-key-vault-2.png" alt-text="Görüntü Azure portal erişim ilkeleri penceresini gösterir.":::
 
4.  **Sertifika** Seç 

    :::image type="content" source="media/certs-and-key-vault-3.png" alt-text="Görüntüde, Azure portal Anahtar Kasası dikey penceresi penceresinde Sertifikalar seçeneğinin seçilmesi gösterilmektedir.":::

5. **Oluştur/Içeri aktar** 'ı seçin

    :::image type="content" source="media/certs-and-key-vault-4.png" alt-text="Görüntü oluştur/içeri aktar seçeneğini seçmeyi gösterir":::

4.  Sertifikayı karşıya yüklemeyi tamamlayacak gerekli bilgileri doldurun. Sertifikanın içinde olması gerekir **. PFX** biçimi.

    :::image type="content" source="media/certs-and-key-vault-5.png" alt-text="Görüntü Azure portal içeri aktarma penceresini gösterir.":::

5.  Sertifika ayrıntılarını hizmet yapılandırma (. cscfg) dosyasında rolünüze ekleyin. Azure portal sertifikanın parmak izinin, hizmet yapılandırma (. cscfg) dosyasındaki parmak iziyle eşleştiğinden emin olun. 
    
    ```json
    <Certificate name="<your cert name>" thumbprint="<thumbprint in key vault" thumbprintAlgorithm="sha1" /> 
    ```

## <a name="next-steps"></a>Sonraki adımlar 
- Cloud Services için [dağıtım önkoşullarını](deploy-prerequisite.md) gözden geçirin (genişletilmiş destek).
- Cloud Services için [sık sorulan soruları](faq.md) gözden geçirin (genişletilmiş destek).
- [Azure Portal](deploy-portal.md), [PowerShell](deploy-powershell.md), [şablon](deploy-template.md) veya [Visual Studio](deploy-visual-studio.md)kullanarak bir bulut hizmeti (genişletilmiş destek) dağıtın.
