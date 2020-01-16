---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/13/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 662b2792a2e09603425b1988138326799334f323
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75973424"
---
### <a name="portal"></a>Portal

Diskleriniz için müşteri tarafından yönetilen anahtarların kurulması, ilk kez yapıyorsanız, belirli bir sırada kaynak oluşturmanızı gerektirir. İlk olarak, bir Azure Key Vault oluşturmanız ve ayarlamanız gerekecektir.

#### <a name="setting-up-your-azure-key-vault"></a>Azure Key Vault ayarlama

1. [Azure Portal](https://portal.azure.com/) oturum açın ve Key Vault arayın
1. **Anahtar kasaları**arayın ve seçin.

    [![SSE-Key-Vault-Portal-Search. png](media/virtual-machines-disk-encryption-portal/sse-key-vault-portal-search.png)](media/virtual-machines-disk-encryption-portal/sse-key-vault-portal-search-expanded.png#lightbox)

    > [!IMPORTANT]
    > Azure anahtar kasanızın, disk şifreleme kümesi, VM, diskler ve anlık görüntülerinin tümünün başarılı olması için aynı bölgede ve aboneliğin olması gerekir.

1. Yeni bir Key Vault oluşturmak için **+ Ekle** ' yi seçin.
1. Yeni bir kaynak grubu oluşturma
1. Bir Anahtar Kasası adı girin, bir bölge seçin ve bir fiyatlandırma katmanı seçin.
1. **Gözden geçir + oluştur**' u seçin, seçimlerinizi doğrulayın ve **Oluştur**' u seçin.

    ![SSE-Create-a-Key-Vault. png](media/virtual-machines-disk-encryption-portal/sse-create-a-key-vault.png)

1. Anahtar kasanızın dağıtımı tamamlandıktan sonra, bunu seçin.
1. **Ayarlar**altında **anahtarlar** ' ı seçin.
1. **Oluştur/Içeri aktar** 'ı seçin

    ![SSE-Key-Vault-Generate-Settings. png](media/virtual-machines-disk-encryption-portal/sse-key-vault-generate-settings.png)

1. Her iki **anahtar türünü** **RSA** ve **RSA anahtar boyutu** **2080**olarak ayarlayın.
1. Kalan seçimleri istediğiniz gibi doldurup **Oluştur**' u seçin.

    ![SSE-Create-a-Key-Generate. png](media/virtual-machines-disk-encryption-portal/sse-create-a-key-generate.png)

#### <a name="setting-up-your-disk-encryption-set"></a>Disk şifreleme kümesini ayarlama

Disk şifreleme kümeleri oluşturup yapılandırmak için şu bağlantıyı kullanmanız gerekir: https://aka.ms/diskencryptionsets. Disk şifreleme kümesi oluşturma işlemi henüz genel Azure portal kullanılamıyor.

1. [Disk şifreleme kümeleri bağlantısını](https://aka.ms/diskencryptionsets)açın.
1. **+ Ekle**' yi seçin.

    ![SSE-Create-disk-Encryption-set. png](media/virtual-machines-disk-encryption-portal/sse-create-disk-encryption-set.png)

1. Kaynak grubunuzu seçin, şifreleme kümesini adlandırın ve anahtar kasanız ile aynı bölgeyi seçin.
1. **Anahtar Kasası ve anahtar**seçin.
1. Daha önce oluşturduğunuz anahtar kasasını ve anahtarı ve sürümü seçin.
1. **Seç**' e basın.
1. **Gözden geçir + oluştur** ve sonra **Oluştur**' u seçin.

    ![SSE-disk-ENC-set-Blade-Key. png](media/virtual-machines-disk-encryption-portal/sse-disk-enc-set-blade-key.png)

1. Oluşturma tamamlandıktan sonra disk şifrelemesi kümesini açın ve açılan uyarıyı seçin.

    ![SSE-disk-ENC-Alert-Fix. png](media/virtual-machines-disk-encryption-portal/sse-disk-enc-alert-fix.png)

İki bildirim açılır ve başarılı olmalıdır. Bunun yapılması, anahtar kasanızın bulunduğu disk şifreleme kümesini kullanmanıza olanak sağlar.

![disk-ENC-Notification-Success. png](media/virtual-machines-disk-encryption-portal/disk-enc-notification-success.png)

#### <a name="deploy-a-vm"></a>VM'yi dağıtma

Anahtar Kasanızı ve disk şifreleme kümesini oluşturup ayarladığınıza göre, şifrelemeyi kullanarak bir VM dağıtabilirsiniz.
VM dağıtım işlemi standart dağıtım işlemine benzer, tek fark, VM 'yi diğer kaynaklarınızla aynı bölgeye dağıtmanız ve müşteri tarafından yönetilen anahtar kullanmayı tercih etmeniz gerekir.

1. **Sanal makineleri** arayın ve **+ Ekle** ' yi seçerek bir VM oluşturun.
1. **Temel** sekmede, disk şifreleme kümesi ve Azure Key Vault aynı bölgeyi seçin.
1. **Temel** sekmedeki diğer değerleri istediğiniz şekilde girin.

    ![SSE-Create-a-VM-Region. png](media/virtual-machines-disk-encryption-portal/sse-create-a-vm-region.png)

1. **Diskler** sekmesinde, **müşteri tarafından yönetilen bir anahtarla bekleyen şifreleme**' yi seçin.
1. Disk **şifreleme kümesi** açılır penceresinde disk şifreleme kümesini seçin.
1. Diğer seçimleri istediğiniz gibi yapın.

    ![SSE-Create-VM-Select-CMK-Encryption-set. png](media/virtual-machines-disk-encryption-portal/sse-create-vm-select-cmk-encryption-set.png)

#### <a name="enable-on-an-existing-disk"></a>Mevcut bir diskte etkinleştir

Mevcut disklerinizde disk şifrelemesini yönetmek ve yapılandırmak için şu bağlantıyı kullanmanız gerekir: https://aka.ms/diskencryptionsets. Mevcut disklerde müşteri tarafından yönetilen anahtarların etkinleştirilmesi, genel Azure portal henüz kullanılamamaktadır.

> [!CAUTION]
> Bir VM 'ye bağlı herhangi bir diskte disk şifrelemeyi etkinleştirmek, VM 'yi durdurmanız gerekir.

1. Disk şifreleme kümelerinizin biriyle aynı bölgedeki bir VM 'ye gidin.
1. VM 'yi açın ve **Durdur**' u seçin.

    ![sse-stop-VM-to-encrypt-disk. png](media/virtual-machines-disk-encryption-portal/sse-stop-VM-to-encrypt-disk.png)

1. VM 'yi durdurmayı tamamladıktan sonra, **diskler** ' i seçin ve ardından şifrelemek istediğiniz diski seçin.

    ![SSE-Existing-disk-Select. png](media/virtual-machines-disk-encryption-portal/sse-existing-disk-select.png)

1. **Şifreleme** ' yi seçin ve ardından **müşteri tarafından yönetilen bir anahtarla geri kalan şifrelemeyi** seçin ve ardından açılan listede disk şifreleme kümesini seçin.
1. **Kaydet**’i seçin.

    ![SSE-Encrypt-Existing-disk-Customer-Managed-Key. png](media/virtual-machines-disk-encryption-portal/sse-encrypt-existing-disk-customer-managed-key.png)

1. Şifrelemek istediğiniz sanal makineye bağlı diğer diskler için bu işlemi tekrarlayın.
1. Disklerinizde, müşteri tarafından yönetilen anahtarlara geçiş tamamlandığında, şifrelemek istediğiniz başka bir bağlı disk yoksa VM 'nizi başlatabilirsiniz.