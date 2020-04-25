---
title: Azure Marketi için bir Kullanıcı VM görüntüsü oluşturma
description: Bir Kullanıcı VM görüntüsü oluşturmak için gereken adımları ve başvuruları listeler.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/29/2018
ms.author: dsindona
ms.openlocfilehash: 9d82d50769925480d461c122096c3919d7e8940d
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82146562"
---
# <a name="create-a-user-vm-image"></a>Kullanıcı VM görüntüsü oluşturma

> [!IMPORTANT]
> 13 Nisan 2020 ' den itibaren, Azure sanal makine tekliflerinizin Iş Ortağı Merkezi 'nin hareketli yönetimine başlayacağız. Geçişten sonra, Iş Ortağı Merkezi 'nde tekliflerinizi oluşturup yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Azure sanal makine teknik varlıklarınızı oluşturma](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-vm-create-offer) bölümündeki yönergeleri izleyin.

Bu makalede, genelleştirilmiş bir VHD 'den yönetilmeyen bir görüntü oluşturmak için gereken iki genel adım açıklanmaktadır.  Her adımda size rehberlik etmek için başvurular sağlanır: görüntüyü yakalama ve görüntüyü Genelleştirme.


## <a name="capture-the-vm-image"></a>VM görüntüsünü yakala

Erişim yaklaşımınıza karşılık gelen VM 'yi yakalamak için aşağıdaki makaledeki yönergeleri kullanın:

-  PowerShell: [bir Azure VM 'den YÖNETILMEYEN VM görüntüsü oluşturma](../../../virtual-machines/windows/capture-image-resource.md)
-  Azure CLı: [bir sanal makinenin veya VHD 'nin görüntüsünü oluşturma](../../../virtual-machines/linux/capture-image.md)
-  API: [sanal makineler-yakala](https://docs.microsoft.com/rest/api/compute/virtualmachines/capture)


## <a name="generalize-the-vm-image"></a>VM görüntüsünü genelleştirin

Kullanıcı görüntüsünü önceden genelleştirilmiş bir VHD 'den oluşturmuş olduğunuzdan, genelleştirilmiş de genelleştirilmelidir.  Yine, erişim mekanizmanıza karşılık gelen aşağıdaki makaleyi seçin.  (Diski yakalandığında zaten genelleştirilmiş bir durum olabilir.)

-  PowerShell: [VM 'Yi Genelleştirme](https://docs.microsoft.com/azure/virtual-machines/windows/sa-copy-generalized#generalize-the-vm)
-  Azure CLı: [2. Adım: VM görüntüsü oluşturma](https://docs.microsoft.com/azure/virtual-machines/linux/capture-image#step-2-create-vm-image)
-  API: [sanal makineler-Genelleştir](https://docs.microsoft.com/rest/api/compute/virtualmachines/generalize)


## <a name="next-steps"></a>Sonraki adımlar

Daha sonra [bir sertifika oluşturup](cpp-create-key-vault-cert.md) yeni bir Azure Key Vault depolayacaksınız.  Bu sertifika, sanal makineye güvenli bir WinRM bağlantısı kurmak için gereklidir.
