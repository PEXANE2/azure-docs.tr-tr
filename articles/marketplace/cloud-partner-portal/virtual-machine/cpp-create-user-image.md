---
title: Azure Marketi için bir kullanıcı VM görüntüsü oluşturma
description: Kullanıcı VM görüntüsü oluşturmak için gereken adımları ve başvuruları listeler.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/29/2018
ms.author: dsindona
ms.openlocfilehash: 49db8c6717cd26886c3b49f8c99fdd2b08e8713d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80278016"
---
# <a name="create-a-user-vm-image"></a>Kullanıcı VM görüntüsü oluşturma

Bu makalede, genelleştirilmiş bir VHD'den yönetilmeyen bir görüntü oluşturmak için gereken iki genel adım açıklanmaktadır.  Referanslar her adımda size rehberlik etmek için sağlanır: görüntüyü yakalayın ve görüntüyü genelleştirin.


## <a name="capture-the-vm-image"></a>VM görüntüsünü yakalama

Erişim yaklaşımınıza karşılık gelen VM'yi yakalamayla ilgili aşağıdaki makalede talimatları kullanın:

-  PowerShell: [Azure VM'den yönetilmeyen bir VM görüntüsü nasıl oluşturulur?](../../../virtual-machines/windows/capture-image-resource.md)
-  Azure CLI: [Sanal makine veya VHD görüntüsü nasıl oluşturulur?](../../../virtual-machines/linux/capture-image.md)
-  API: [Sanal Makineler - Yakalama](https://docs.microsoft.com/rest/api/compute/virtualmachines/capture)


## <a name="generalize-the-vm-image"></a>VM görüntüsünü genelleştirin

Daha önce genelleştirilmiş bir VHD kullanıcı görüntüsü oluşturduğunuziçin, aynı zamanda genelleştirilmiş olmalıdır.  Yine, erişim mekanizmasınıza karşılık gelen aşağıdaki makaleyi seçin.  (Diskinizi yakaladığınızda zaten genellemiş olabilirsiniz.)

-  PowerShell: [VM'yi genelleştirin](https://docs.microsoft.com/azure/virtual-machines/windows/sa-copy-generalized#generalize-the-vm)
-  Azure CLI: [Adım 2: VM görüntüsü oluşturma](https://docs.microsoft.com/azure/virtual-machines/linux/capture-image#step-2-create-vm-image)
-  API: [Sanal Makineler - Genelleme](https://docs.microsoft.com/rest/api/compute/virtualmachines/generalize)


## <a name="next-steps"></a>Sonraki adımlar

Ardından [bir sertifika oluşturup](cpp-create-key-vault-cert.md) yeni bir Azure Anahtar Kasası'nda saklarsınız.  Bu sertifika, VM'ye güvenli bir WinRM bağlantısı kurmak için gereklidir.
