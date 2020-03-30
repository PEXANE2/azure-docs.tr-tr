---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/11/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 3ba5d74aa245fbcd9d43f2b4398387d7f59e202c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79299540"
---
### <a name="portal"></a>Portal

Diskleriniz için müşteri tarafından yönetilen anahtarları ayarlamak, bunu ilk kez yapıyorsanız belirli bir sırada kaynak oluşturmanızı gerektirir. İlk olarak, bir Azure Anahtar Kasası oluşturmanız ve kurmanız gerekir.

#### <a name="setting-up-your-azure-key-vault"></a>Azure Anahtar Kasanızı Ayarlama

1. [Azure portalında](https://portal.azure.com/) oturum açın ve Key Vault'u arayın
1. **Anahtar Kasalarını**arayın ve seçin.

    [![sse-key-vault-portal-search.png](media/virtual-machines-disk-encryption-portal/sse-key-vault-portal-search.png)](media/virtual-machines-disk-encryption-portal/sse-key-vault-portal-search-expanded.png#lightbox)

    > [!IMPORTANT]
    > Azure anahtar kasanız, disk şifreleme setiniz, VM'leriniz, disklerinizin ve anlık görüntülerinizin başarılı olması için tüm bunların aynı bölgede ve dağıtım aboneliğinde olması gerekir.

1. Yeni bir Anahtar Kasası oluşturmak için **+Ekle'yi** seçin.
1. Yeni bir kaynak grubu oluşturma
1. Anahtar kasa adı girin, bir bölge seçin ve bir fiyatlandırma katmanı seçin.
1. **Gözden Geçir + Oluştur'u**seçin, seçimlerinizi doğrulayın, ardından **Oluştur'u**seçin.

    ![Azure Key Vault oluşturma deneyiminin ekran görüntüsü. Oluşturduğunuz belirli değerleri gösterme](media/virtual-machines-disk-encryption-portal/sse-create-a-key-vault.png)

1. Anahtar kasanız dağıtımı tamamladıktan sonra, seçin.
1. **Ayarlar**altında **Tuşları** seçin.
1. **Oluştur/İçe Al'ı** Seçin

    ![Key Vault kaynak ayarları bölmesinin ekran görüntüsü. Ayarlar içinde oluşturma/alma düğmesini gösterir.](media/virtual-machines-disk-encryption-portal/sse-key-vault-generate-settings.png)

1. Hem **Anahtar Türü** kümesini **RSA'ya** hem de **RSA Anahtar Boyutu'na** **2080**olarak ayarlayın.
1. Kalan seçimleri istediğiniz gibi doldurun ve ardından **Oluştur'u**seçin.

    ![Oluşturma/alma düğmesi seçildikten sonra görünen bir anahtar bıçağı oluşturma ekran görüntüsü](media/virtual-machines-disk-encryption-portal/sse-create-a-key-generate.png)

#### <a name="setting-up-your-disk-encryption-set"></a>Disk şifreleme ayarınızı ayarlama

Disk şifreleme kümeleri oluşturmak ve yapılandırmak için https://aka.ms/diskencryptionsetsaşağıdaki bağlantıyı kullanmanız gerekir: . Microsoft Azure Kamu bölgelerindeyseniz, bunun yerine şu bağlantıyı [https://aka.ms/diskencryptionsetsff](https://aka.ms/diskencryptionsetsff)kullanmanız gerekir: . Disk şifreleme kümesi oluşturma, genel Azure portalında henüz kullanıma sunulmadı.

1. Bölgenize uygun disk şifreleme ayarlarını açın:

    Genel bölgeler:[https://aka.ms/diskencryptionsets](https://aka.ms/diskencryptionsets)

    Azure Devlet bölgeleri:[https://aka.ms/diskencryptionsetsff](https://aka.ms/diskencryptionsetsff)
    
1. **+Ekle'yi**seçin.

    ![Disk şifreleme portalı ana ekranının ekran görüntüsü. Ekle düğmesini vurgulama](media/virtual-machines-disk-encryption-portal/sse-create-disk-encryption-set.png)

1. Kaynak grubunuzu seçin, şifreleme kümenizi adlandırın ve anahtar kasanızla aynı bölgeyi seçin.
1. **Anahtar kasası ve tuşu**seçin.
1. Daha önce oluşturduğunuz anahtar kasasını ve anahtarı ve sürümü seçin.
1. **Seç**tuşuna basın.
1. **Gözden Geçir + Oluştur'u** seçin ve ardından **Oluşturun.**

    ![Disk şifreleme oluşturma bıçağının ekran görüntüsü. Aboneliği, kaynak grubunu, disk şifreleme kümesi adını, bölgesini ve anahtar kasasını + anahtar seçiciyi gösterir.](media/virtual-machines-disk-encryption-portal/sse-disk-enc-set-blade-key.png)

1. Oluşturmayı tamamladıktan sonra disk şifreleme ayarını açın ve açılan uyarıyı seçin.

    ![Uyarı açılır görüntüsünün ekran görüntüsü: 'Bir diski, görüntüyü veya anlık görüntüyü bir disk şifreleme kümesiyle ilişkilendirmek için, anahtar kasasına izin vermeniz gerekir'. Devam etmek için bu uyarıyı seçin](media/virtual-machines-disk-encryption-portal/sse-disk-enc-alert-fix.png)

İki bildirim açılır ve başarılı olmalıdır. Bunu yapmak, anahtar kasanızla birlikte disk şifreleme kümesini kullanmanıza olanak sağlar.

![Anahtar kasanız için başarılı izin ve rol atamasının ekran görüntüsü.](media/virtual-machines-disk-encryption-portal/disk-enc-notification-success.png)

#### <a name="deploy-a-vm"></a>VM'yi dağıtma

Artık anahtar kasanızı ve disk şifreleme setinizi oluşturup kurduğunuza göre, şifrelemeyi kullanarak bir VM dağıtabilirsiniz.
VM dağıtım işlemi standart dağıtım işlemine benzer, tek fark, VM'yi diğer kaynaklarla aynı bölgede dağıtmanız ve müşteri yönetilen bir anahtarı kullanmayı tercih etmenizdir.

1. Bölgenize uygun disk şifreleme ayarlarını açın:

    Genel bölgeler:[https://aka.ms/diskencryptionsets](https://aka.ms/diskencryptionsets)

    Azure Devlet bölgeleri:[https://aka.ms/diskencryptionsetsff](https://aka.ms/diskencryptionsetsff)

1. Sanal **Makineleri** arayın ve VM oluşturmak için **+ Ekle'yi** seçin.
1. **Temel** sekmesinde, disk şifreleme kümenizle ve Azure Anahtar Kasası ile aynı bölgeyi seçin.
1. **Temel** sekmesindeki diğer değerleri istediğiniz gibi doldurun.

    ![Bölge değeri vurgulanan VM oluşturma deneyiminin ekran görüntüsü.](media/virtual-machines-disk-encryption-portal/sse-create-a-vm-region.png)

1. **Diskler** sekmesinde, **müşteri tarafından yönetilen bir anahtarla istirahatte Şifreleme'yi**seçin.
1. **Disk şifreleme ayarını** açılır durumda seçin.
1. Kalan seçimleri istediğiniz gibi yapın.

    ![VM oluşturma deneyiminin ekran görüntüsü, diskler bıçak. Disk şifreleme seti açılır vurgulanır.](media/virtual-machines-disk-encryption-portal/sse-create-vm-select-cmk-encryption-set.png)

#### <a name="enable-on-an-existing-disk"></a>Varolan bir diskte etkinleştirme

Varolan disklerinizde disk şifrelemeyi yönetmek ve yapılandırmak için https://aka.ms/diskencryptionsetsaşağıdaki bağlantıyı kullanmanız gerekir: . Varolan disklerde müşteri tarafından yönetilen anahtarları etkinleştirmek henüz genel Azure portalında kullanılamıyor.

> [!CAUTION]
> VM'ye bağlı tüm disklerde disk şifrelemesini etkinleştirmek, VM'yi durdurmanızı gerektirir.

1. Bölgenize uygun disk şifreleme ayarlarını açın:

    Genel bölgeler:[https://aka.ms/diskencryptionsets](https://aka.ms/diskencryptionsets)

    Azure Devlet bölgeleri:[https://aka.ms/diskencryptionsetsff](https://aka.ms/diskencryptionsetsff)
    
1. Disk şifreleme kümelerinizden biriyle aynı bölgede bulunan bir VM'ye gidin.
1. VM'yi açın ve **Durdur'u**seçin.

    ![Örneğin VM için ana bindirme ekran görüntüsü. Stop düğmesi vurgulanmış](media/virtual-machines-disk-encryption-portal/sse-stop-VM-to-encrypt-disk.png)

1. VM durmayı bitirdikten sonra **Diskleri** seçin ve ardından şifrelemek istediğiniz diski seçin.

    ![Diskler bıçağı açık olan vm örneğinizin ekran görüntüsü. Os diski, seçmeniz için örnek bir disk olarak vurgulanır.](media/virtual-machines-disk-encryption-portal/sse-existing-disk-select.png)

1. Müşteri tarafından **yönetilen bir anahtarla birlikte** **Şifreleme'yi** seçin ve ardından açılan listede disk şifreleme setinizi seçin.
1. **Kaydet'i**seçin.

    ![Örnek işletim sistemi diskinizin ekran görüntüsü. Şifreleme bıçağı açıktır, müşteri tarafından yönetilen bir anahtarla birlikte şifreleme ve örnek Azure Key Vault seçilir. Bu seçimleri yaptıktan sonra kaydet düğmesi seçilir.](media/virtual-machines-disk-encryption-portal/sse-encrypt-existing-disk-customer-managed-key.png)

1. Şifrelemek istediğiniz VM'ye iliştirilen diğer diskler için bu işlemi yineleyin.
1. Diskleriniz müşteri tarafından yönetilen anahtarlara geçmeyi bitirdiğinde, şifrelemek istediğiniz başka ekli disk yoksa VM'nizi başlatabilirsiniz.