---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 01/30/2019
ms.author: tomfitz
ms.openlocfilehash: aac2f3ea2b52ac0319f96279deed13c1145749bd
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74451513"
---
## <a name="deploy-template-from-cloud-shell"></a>Cloud Shell'den şablon dağıtma

Şablonunuzu dağıtmak için [Cloud Shell](../articles/cloud-shell/overview.md) kullanabilirsiniz. Dış şablon dağıtmak için, şablon URI 'sini tam olarak herhangi bir dış dağıtım için yaptığınız gibi sağlayın. Yerel bir şablon dağıtmak için, önce şablonunuzu Cloud Shell depolama hesabına yüklemeniz gerekir. Bu bölümde, şablonun Cloud Shell hesabınıza yüklenmesi ve yerel bir dosya olarak dağıtılması açıklanmaktadır. Cloud Shell kullanmadıysanız, ayarlama hakkında bilgi için bkz. [Azure Cloud Shell Genel Bakış](../articles/cloud-shell/overview.md) .

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. Cloud Shell kaynak grubunuzu seçin. Ad deseni `cloud-shell-storage-<region>` şeklindedir.

   ![Kaynak grubu seçin](./media/resource-manager-cloud-shell-deploy/select-cloud-shell-resource-group.png)

1. Cloud Shell için depolama hesabınızı seçin.

   ![Depolama hesabı seçme](./media/resource-manager-cloud-shell-deploy/select-storage.png)

1. **Bloblar**'ı seçin.

   ![Bloblar'ı seçin](./media/resource-manager-cloud-shell-deploy/select-blobs.png)

1. **+ Kapsayıcı**'yı seçin.

   ![Kapsayıcı ekleme](./media/resource-manager-cloud-shell-deploy/add-container.png)

1. Kapsayıcınıza bir ad ve erişim düzeyi verin. Bu makaledeki örnek şablon gizli bilgiler içermez, bu nedenle anonim okuma erişimine izin verin. **Tamam**’ı seçin.

   ![Kapsayıcı değerlerini sağla](./media/resource-manager-cloud-shell-deploy/provide-container-values.png)

1. Oluşturduğunuz kapsayıcıyı seçin.

   ![Yeni kapsayıcı Seç](./media/resource-manager-cloud-shell-deploy/select-container.png)

1. **Karşıya Yükle**’yi seçin.

   ![Blobu karşıya yükle](./media/resource-manager-cloud-shell-deploy/upload-blob.png)

1. Şablonunuzu bulup karşıya yükleyin.

   ![Dosya yükleme](./media/resource-manager-cloud-shell-deploy/find-and-upload-template.png)

1. Karşıya yüklendikten sonra şablonu seçin.

   ![Yeni şablon seç](./media/resource-manager-cloud-shell-deploy/select-new-template.png)

1. URL'yi kopyalayın.

   ![URL 'YI Kopyala](./media/resource-manager-cloud-shell-deploy/copy-url.png)

1. İstemi açın.

   ![Cloud Shell’i açma](./media/resource-manager-cloud-shell-deploy/start-cloud-shell.png)
