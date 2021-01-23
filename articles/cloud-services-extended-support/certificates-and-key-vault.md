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
ms.openlocfilehash: 9e69b4e9279f9147c2ee13d42a42aec0c5a15d96
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2021
ms.locfileid: "98744729"
---
# <a name="use-certificates-with-azure-cloud-services-extended-support"></a>Sertifikaları Azure Cloud Services kullanma (genişletilmiş destek)

Key Vault, Cloud Services ilişkili sertifikaları (genişletilmiş destek) depolamak için kullanılır. Anahtar kasaları [Azure Portal](https://docs.microsoft.com/azure/key-vault/general/quick-create-portal) ve [PowerShell](https://docs.microsoft.com/azure/key-vault/general/quick-create-powershell)aracılığıyla oluşturulabilir. Sertifikaları Key Vault ekleyin ve ardından hizmet yapılandırma dosyasında sertifika parmak izleriyle referans yapın. Ayrıca, Cloud Services (genişletilmiş destek) kaynağının Key Vault gizli dizi olarak depolanan sertifikayı alabilmesi için uygun izinler için Key Vault etkinleştirmeniz gerekir.  

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

4.  Sertifikayı karşıya yüklemeyi tamamlayacak gerekli bilgileri doldurun. 

    :::image type="content" source="media/certs-and-key-vault-5.png" alt-text="Görüntü Azure portal içeri aktarma penceresini gösterir.":::

5.  Sertifika ayrıntılarını hizmet yapılandırma (. cscfg) dosyasında rolünüze ekleyin. Azure portal sertifikanın parmak izinin, hizmet yapılandırma (. cscfg) dosyasındaki parmak iziyle eşleştiğinden emin olun. 
    
    ```json
    <Certificate name="<your cert name>" thumbprint="<thumbprint in key vault" thumbprintAlgorithm="sha1" /> 
    ```

## <a name="next-steps"></a>Sonraki adımlar 
- Cloud Services için [dağıtım önkoşullarını](deploy-prerequisite.md) gözden geçirin (genişletilmiş destek).
- Cloud Services için [sık sorulan soruları](faq.md) gözden geçirin (genişletilmiş destek).
- [Azure Portal](deploy-portal.md), [PowerShell](deploy-powershell.md), [şablon](deploy-template.md) veya [Visual Studio](deploy-visual-studio.md)kullanarak bir bulut hizmeti (genişletilmiş destek) dağıtın.