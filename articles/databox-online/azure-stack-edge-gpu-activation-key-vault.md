---
title: Azure Key Vault Azure Stack Edge kaynağı ve cihaz etkinleştirmeyle tümleştirme
description: Azure Stack Edge Pro cihazının etkinleştirilmesi sırasında Azure Key Vault gizli yönetimin nasıl ilişkilendirildiğini açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: conceptual
ms.date: 10/10/2020
ms.author: alkohli
ms.openlocfilehash: 8957d8982a3bfe1da2811dc10d0c3e77a72fc288
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92367610"
---
# <a name="azure-key-vault-integration-with-azure-stack-edge"></a>Azure Stack Edge ile tümleştirme Azure Key Vault 

Azure Key Vault, gizli yönetim için Azure Stack Edge kaynağıyla tümleşiktir. Bu makalede cihaz etkinleştirme sırasında Azure Stack Edge kaynağı için bir Azure Key Vault nasıl oluşturulduğu ve daha sonra gizli yönetim için kullanıldığı hakkında ayrıntılar sağlanmaktadır. 


## <a name="about-key-vault-and-azure-stack-edge"></a>Anahtar Kasası ve Azure Stack Edge hakkında

Azure Key Vault bulut hizmeti belirteçleri, parolaları, sertifikaları, API anahtarlarını ve diğer gizli dizileri güvenli bir şekilde depolamak ve bu erişimi denetlemek için kullanılır. Key Vault Ayrıca, verilerinizi şifrelemek için kullanılan şifreleme anahtarlarını oluşturmayı ve denetlemeyi kolaylaştırır. İzin verilen işlemler ve karşılık gelen ücretler hakkında daha fazla bilgi için bkz. [Azure Key Vault fiyatlandırması](https://azure.microsoft.com/pricing/details/key-vault/).

Azure Stack Edge hizmeti için kullanılan sırların biri kanal bütünlüğü anahtarıdır (CIK). Bu anahtar, sırlarınızı şifrelemenizi sağlar. Anahtar Kasası tümleştirmesiyle, CıK, anahtar kasasında güvenli bir şekilde depolanır. Daha fazla bilgi için bkz. [gizli dizileri ve anahtarları güvenli bir şekilde depolayın](../key-vault/general/overview.md#securely-store-secrets-and-keys).


## <a name="key-vault-creation"></a>Anahtar Kasası oluşturma

Etkinleştirme anahtarı oluşturma işlemi sırasında Azure Stack Edge kaynağı için bir Anahtar Kasası oluşturulur. 

- Azure Stack Edge kaynağı oluşturduğunuzda, *Microsoft. Keykasası* kaynak sağlayıcısını kaydetmeniz gerekir. Aboneliğe sahip veya katılımcı erişiminiz varsa kaynak sağlayıcı otomatik olarak kaydedilir. Anahtar Kasası, Azure Stack Edge kaynağıyla aynı abonelikte ve kaynak grubunda oluşturulur. 

- Bir Azure Stack Edge kaynağı oluşturduğunuzda, kaynağın kullanım ömrü boyunca devam eden ve bulutta kaynak sağlayıcıyla iletişim kuran bir Yönetilen Hizmet Kimliği (MSI) de oluşturulur. 

    MSI etkinleştirildiğinde Azure Azure Stack Edge kaynağı için güvenilir bir kimlik oluşturur.

- Azure Stack Edge kaynağını oluşturduktan ve Azure portal bir etkinleştirme anahtarı oluşturduğunuzda bir Anahtar Kasası oluşturulur. Bu Anahtar Kasası, gizli yönetim için kullanılır ve Azure Stack Edge kaynağı mevcut olduğu sürece devam ettirir. 

    ![Etkinleştirme anahtarı oluşturma sırasında Key Vault oluşturuldu](media/azure-stack-edge-gpu-deploy-prep/azure-stack-edge-resource-3.png)

- Varsayılan anahtar adını kabul edebilir veya Anahtar Kasası için özel bir ad belirtebilirsiniz. Anahtar Kasası adı 3 ile 24 karakter uzunluğunda olmalıdır. Zaten kullanımda olan bir Anahtar Kasası kullanamazsınız. <!--The MSI is then used to authenticate to key vault to retrieve secrets.--> 

    ![Azure Stack Edge kaynağı oluşturma sırasında MSI oluşturuldu](media/azure-stack-edge-gpu-deploy-prep/create-resource-8.png)

- Azure Anahtar Kasası 'na gitmek için Azure Stack Edge kaynağınızın **özelliklerine** gidin ve Anahtar Kasası adını seçin. 

- Yanlışlıkla silinmeye engel olmak için anahtar kasasında bir kaynak kilidi etkindir. Yanlışlıkla silme işlemi varsa anahtar kasasının 90 gün içinde geri yüklenmesine izin veren anahtar kasasında geçici silme de etkinleştirilir. Daha fazla bilgi için bkz. [Azure Key Vault geçici genel bakış](../key-vault/general/soft-delete-overview.md)

    Anahtar Kasası yanlışlıkla silinirse ve 90 gün süreyle Temizleme koruma süresi dolmuşsa, [anahtar kasanızı kurtarmak](../key-vault/general/soft-delete-powershell.md#recovering-a-key-vault)için bu adımları izleyin. 

- Azure Key Vault Azure Stack Edge kaynağıyla tümleştirilemeden önce mevcut bir Azure Stack Edge kaynağına sahipseniz, bundan etkilenmezsiniz. Mevcut Azure Stack Edge kaynağını kullanmaya devam edebilirsiniz. 

- Azure Stack Edge kaynağınız silindiğinde, Azure Key Vault kaynakla de silinir. Onaylamanız istenir. Bu anahtar kasasını silmeyi düşünmüyorsanız, izin sağlamayı tercih edebilirsiniz. Anahtar kasasından etkilenmeden yalnızca Azure Stack Edge kaynağı silinir. 

- Bu Anahtar Kasası diğer anahtarları depolamak için kullanılmışsa, bu işlemi yine 90 gün içinde geri yükleyebilirsiniz. Bu temizleme koruma süresi boyunca, Anahtar Kasası adı yeni bir Anahtar Kasası oluşturmak için kullanılamaz.

Anahtar Kasası ve cihaz etkinleştirmeyle ilgili herhangi bir sorunla karşılaşırsanız, bkz. [Cihaz etkinleştirme sorunlarını giderme](azure-stack-edge-gpu-troubleshoot-activation.md).

<!--## Key vault secret management

When you generate an activation key, the following events occur:

1. You request an activation key in the Azure portal. The request is then sent to Key Vault resource provider. 
1. A standard tier key vault with access policy is created and is locked by default. This key vault uses the default name or the custom name that you specified.
1. The key vault authenticates with MSI the request to generate activation key. The MSI is also added to the key vault access policy and a channel integrity key is generated and placed in the key vault.
1. The activation key is returned to the Azure portal. You can then copy this key and use it in the local UI to activate your device.-->



## <a name="next-steps"></a>Sonraki adımlar

- [Etkinleştirme anahtarı oluşturma](azure-stack-edge-gpu-deploy-prep.md#get-the-activation-key)hakkında daha fazla bilgi edinin.

