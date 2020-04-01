---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 01/30/2019
ms.author: tomfitz
ms.openlocfilehash: aac2f3ea2b52ac0319f96279deed13c1145749bd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74451513"
---
## <a name="deploy-template-from-cloud-shell"></a>Cloud Shell'den şablon dağıtma

Şablonunuzu dağıtmak için [Cloud Shell](../articles/cloud-shell/overview.md) kullanabilirsiniz. Harici bir şablon dağıtmak için, şablonun URI'sini herhangi bir dış dağıtımda olduğu gibi sağlayın. Yerel bir şablon dağıtmak için öncelikle şablonunuzu Bulut Shell'inizin depolama hesabına yüklemeniz gerekir. Bu bölümde, şablonun bulut kabuğu hesabınıza nasıl yüklenir ve yerel bir dosya olarak dağıtılır. Cloud Shell'i kullanmadıysanız, ayarlama hakkında bilgi almak için [Azure Bulut Shell'e Genel Bakış](../articles/cloud-shell/overview.md) bölümüne bakın.

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Cloud Shell kaynak grubunuzu seçin. Ad deseni `cloud-shell-storage-<region>` şeklindedir.

   ![Kaynak grubu seçin](./media/resource-manager-cloud-shell-deploy/select-cloud-shell-resource-group.png)

1. Cloud Shell için depolama hesabınızı seçin.

   ![Depolama hesabı seçme](./media/resource-manager-cloud-shell-deploy/select-storage.png)

1. **Bloblar**'ı seçin.

   ![Bloblar'ı seçin](./media/resource-manager-cloud-shell-deploy/select-blobs.png)

1. **+ Kapsayıcı**'yı seçin.

   ![Kapsayıcı ekleme](./media/resource-manager-cloud-shell-deploy/add-container.png)

1. Kapsayıcınıza bir ad ve erişim düzeyi verin. Bu makaledeki örnek şablon, hiçbir hassas bilgi içermez, bu nedenle anonim okuma erişimine izin verin. **Tamam'ı**seçin.

   ![Kapsayıcı değerleri sağlama](./media/resource-manager-cloud-shell-deploy/provide-container-values.png)

1. Oluşturduğunuz kapsayıcıyı seçin.

   ![Yeni kapsayıcı seçin](./media/resource-manager-cloud-shell-deploy/select-container.png)

1. **Yükle'yi**seçin.

   ![Blob yükle](./media/resource-manager-cloud-shell-deploy/upload-blob.png)

1. Şablonunuzu bulup karşıya yükleyin.

   ![Dosya yükleme](./media/resource-manager-cloud-shell-deploy/find-and-upload-template.png)

1. Yükledikten sonra şablonu seçin.

   ![Yeni şablon seçin](./media/resource-manager-cloud-shell-deploy/select-new-template.png)

1. URL'yi kopyalayın.

   ![URL'yi Kopyala](./media/resource-manager-cloud-shell-deploy/copy-url.png)

1. İstemi açın.

   ![Cloud Shell’i açma](./media/resource-manager-cloud-shell-deploy/start-cloud-shell.png)
