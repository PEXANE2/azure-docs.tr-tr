---
title: dosya dahil etme
description: dosya dahil etme
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/23/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: b8073240bdda38757a5e4feee66c9f54746966c4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80632067"
---
### <a name="portal"></a>Portal

Diskleriniz için müşteri tarafından yönetilen anahtarların kurulması, ilk kez yapıyorsanız, belirli bir sırada kaynak oluşturmanızı gerektirir. İlk olarak, bir Azure Key Vault oluşturmanız ve ayarlamanız gerekecektir.

#### <a name="setting-up-your-azure-key-vault"></a>Azure Key Vault ayarlama

1. [Azure portalında](https://portal.azure.com/) oturum açın.
1. **Anahtar kasaları**arayın ve seçin.

    [![sse-key-vault-portal-search.png](media/virtual-machines-disk-encryption-portal/sse-key-vault-portal-search.png)](media/virtual-machines-disk-encryption-portal/sse-key-vault-portal-search-expanded.png#lightbox)

    > [!IMPORTANT]
    > Azure anahtar kasanızın, disk şifreleme kümesi, VM, diskler ve anlık görüntülerinin tümünün başarılı olması için aynı bölgede ve aboneliğin olması gerekir.

1. Yeni bir Key Vault oluşturmak için **+ Ekle** ' yi seçin.
1. Yeni bir kaynak grubu oluşturun.
1. Bir Anahtar Kasası adı girin, bir bölge seçin ve bir fiyatlandırma katmanı seçin.
1. **Gözden geçir + oluştur**' u seçin, seçimlerinizi doğrulayın ve **Oluştur**' u seçin.

    ![Azure Key Vault oluşturma deneyiminin ekran görüntüsü. Oluşturduğunuz belirli değerleri gösterme](media/virtual-machines-disk-encryption-portal/sse-create-a-key-vault.png)

1. Anahtar kasanızın dağıtımı tamamlandıktan sonra, bunu seçin.
1. **Ayarlar**altında **anahtarlar** ' ı seçin.
1. **Oluştur/Içeri aktar**' ı seçin.

    ![Key Vault kaynak ayarları bölmesinin ekran görüntüsü. Ayarlar içinde oluştur/Al düğmesini gösterir.](media/virtual-machines-disk-encryption-portal/sse-key-vault-generate-settings.png)

1. Her iki **anahtar türünü** **RSA** ve **RSA anahtar boyutu** **2048**olarak ayarlayın.
1. Kalan seçimleri istediğiniz gibi doldurup **Oluştur**' u seçin.

    ![Oluştur/Al düğmesine bir kez görüntülenen anahtar oluştur dikey penceresinin ekran görüntüsü seçilir](media/virtual-machines-disk-encryption-portal/sse-create-a-key-generate.png)

#### <a name="setting-up-your-disk-encryption-set"></a>Disk şifreleme kümesini ayarlama

1. **Disk şifreleme kümelerini** arayın ve seçin.
1. **Disk şifreleme kümeleri** dikey penceresinde **+ Ekle**' yi seçin.

    ![Disk şifreleme portalı ana ekranının ekran görüntüsü. Ekle düğmesini vurgulama](media/virtual-machines-disk-encryption-portal/sse-create-disk-encryption-set.png)

1. Kaynak grubunuzu seçin, şifreleme kümesini adlandırın ve anahtar kasanız ile aynı bölgeyi seçin.
1. **Anahtar Kasası ve anahtar**seçin.
1. Daha önce oluşturduğunuz anahtar kasasını ve anahtarı ve sürümü seçin.
1. **Seç**' e basın.
1. **Gözden geçir + oluştur** ve sonra **Oluştur**' u seçin.

    ![Disk şifrelemesi oluşturma dikey penceresinin ekran görüntüsü. Abonelik, kaynak grubu, disk şifreleme kümesi adı, bölge ve Anahtar Kasası + anahtar Seçicisi gösteriliyor.](media/virtual-machines-disk-encryption-portal/sse-disk-enc-set-blade-key.png)

1. Oluşturma tamamlandıktan sonra disk şifrelemesi kümesini açın ve açılan uyarıyı seçin.

    ![Uyarı açılır penceresi: ' disk, resim veya anlık görüntüyü bir disk şifrelemesi kümesiyle ilişkilendirmek Için, Anahtar Kasası 'na izin vermeniz gerekir. Devam etmek için bu uyarıyı seçin](media/virtual-machines-disk-encryption-portal/sse-disk-enc-alert-fix.png)

İki bildirim açılır ve başarılı olmalıdır. Bunun yapılması, anahtar kasanızın bulunduğu disk şifreleme kümesini kullanmanıza olanak sağlar.

![Anahtar kasanızın başarılı izninin ve rol atamasının ekran görüntüsü.](media/virtual-machines-disk-encryption-portal/disk-enc-notification-success.png)

#### <a name="deploy-a-vm"></a>VM'yi dağıtma

Anahtar Kasanızı ve disk şifreleme kümesini oluşturup ayarladığınıza göre, şifrelemeyi kullanarak bir VM dağıtabilirsiniz.
VM dağıtım işlemi standart dağıtım işlemine benzer, tek fark, VM 'yi diğer kaynaklarınızla aynı bölgeye dağıtmanız ve müşteri tarafından yönetilen anahtar kullanmayı tercih etmeniz gerekir.

1. **Sanal makineleri** arayın ve **+ Ekle** ' yi seçerek bir VM oluşturun.
1. **Temel** sekmede, disk şifreleme kümesi ve Azure Key Vault aynı bölgeyi seçin.
1. **Temel** sekmedeki diğer değerleri istediğiniz şekilde girin.

    ![Bölge değeri vurgulanmış şekilde VM oluşturma deneyiminin ekran görüntüsü.](media/virtual-machines-disk-encryption-portal/sse-create-a-vm-region.png)

1. **Diskler** sekmesinde, **müşteri tarafından yönetilen bir anahtarla bekleyen şifreleme**' yi seçin.
1. Disk **şifreleme kümesi** açılır penceresinde disk şifreleme kümesini seçin.
1. Diğer seçimleri istediğiniz gibi yapın.

    ![VM oluşturma deneyiminin, diskler dikey penceresinin ekran görüntüsü. Disk şifreleme kümesi açılır ve vurgulanmış olarak.](media/virtual-machines-disk-encryption-portal/sse-create-vm-select-cmk-encryption-set.png)

#### <a name="enable-on-an-existing-disk"></a>Mevcut bir diskte etkinleştir

> [!CAUTION]
> Bir VM 'ye bağlı herhangi bir diskte disk şifrelemeyi etkinleştirmek, VM 'yi durdurmanız gerekir.
    
1. Disk şifreleme kümelerinizin biriyle aynı bölgede bulunan bir VM 'ye gidin.
1. VM 'yi açın ve **Durdur**' u seçin.

    ![Örnek sanal makinenizin ana kaplamasıyla ilgili ekran görüntüsü. Durdur düğmesi vurgulanmış olarak](media/virtual-machines-disk-encryption-portal/sse-stop-VM-to-encrypt-disk.png)

1. VM 'yi durdurmayı tamamladıktan sonra, **diskler** ' i seçin ve ardından şifrelemek istediğiniz diski seçin.

    ![Örnek sanal makinenizin, diskler dikey penceresi açık olan ekran görüntüsü. İşletim sistemi diski, seçmeniz için örnek bir disk olarak vurgulanır.](media/virtual-machines-disk-encryption-portal/sse-existing-disk-select.png)

1. **Şifreleme** ' yi seçin ve ardından **müşteri tarafından yönetilen bir anahtarla geri kalan şifrelemeyi** seçin ve ardından açılan listede disk şifreleme kümesini seçin.
1. **Kaydet**'i seçin.

    ![Örnek işletim sistemi diskinizin ekran görüntüsü. Şifreleme dikey penceresi açıktır, müşteri tarafından yönetilen bir anahtarla bekleyen şifreleme seçilidir ve ayrıca Azure Key Vault. Bu seçimleri yaptıktan sonra Kaydet düğmesi seçilidir.](media/virtual-machines-disk-encryption-portal/sse-encrypt-existing-disk-customer-managed-key.png)

1. Şifrelemek istediğiniz sanal makineye bağlı diğer diskler için bu işlemi tekrarlayın.
1. Disklerinizde, müşteri tarafından yönetilen anahtarlara geçiş tamamlandığında, şifrelemek istediğiniz başka bir bağlı disk yoksa VM 'nizi başlatabilirsiniz.
