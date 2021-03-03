---
title: Hızlı başlangıç-Azure portal VM 'Ler için Azure oto yönetimini etkinleştirin
description: Azure portal yeni veya var olan bir VM 'de sanal makineler için kolayca hızlı bir şekilde etkinleştirmeyi öğrenin.
author: ju-shim
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: quickstart
ms.date: 02/17/2021
ms.author: jushiman
ms.openlocfilehash: 6e0e582ed37230ba3f379f193a229cfec06f066c
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101648042"
---
# <a name="quickstart-enable-azure-automanage-for-virtual-machines-in-the-azure-portal"></a>Hızlı başlangıç: Azure portal sanal makineler için Azure oto yönetimini etkinleştirin

Yeni veya mevcut bir sanal makinede oto yönetimini etkinleştirmek için Azure portal kullanarak sanal makineler için Azure 'u Oto için bir kez daha kullanmaya başlayın.


## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [bir hesap oluşturun](https://azure.microsoft.com/pricing/purchase-options/pay-as-you-go/) .

> [!NOTE]
> Ücretsiz deneme hesaplarının bu öğreticide kullanılan sanal makinelere erişimi yoktur. Lütfen Kullandıkça Öde aboneliğine yükseltin.

> [!IMPORTANT]
> Mevcut bir oto Yönet hesabı kullanarak, oto yönetimini etkinleştirmek için VM 'lerinizi içeren kaynak grubunda **katkıda** bulunan rolüne sahip olmanız gerekir. Yeni bir bir oto Yönet hesabıyla bir oto yönetimi etkinleştirirseniz, aşağıdaki izinlere sahip olmanız gerekir: **sahip** rolü veya **katılımcısı** , aboneliğinizdeki **Kullanıcı erişimi yönetici** rolleriyle birlikte.


## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](https://aka.ms/AutomanagePortal-Ignite21) oturum açın.

## <a name="enable-automanage-for-a-single-vm"></a>Tek bir VM için oto yönetimini etkinleştir

1. Etkinleştirmek istediğiniz sanal makineye gidin.

2. **İşlemler** altındaki içindekiler tablosunda bulunan **oto Yönet (Önizleme)** girişine tıklayın.

3. **Kullanmaya** başlayın ' ı seçin.

    :::image type="content" source="media\quick-create-virtual-machine-portal\VMManage-GetStarted.png" alt-text="Tek VM 'yi kullanmaya başlayın.":::

4. Oto Yönet ayarlarınızı (ortam, Tercihler, oto Yönet hesabı) seçin ve **Etkinleştir**' i tıklayın.

    :::image type="content" source="media\quick-create-virtual-machine-portal\VMManage-Enable.png" alt-text="Tek bir VM 'de etkinleştirin.":::

## <a name="enable-automanage-for-multiple-vms"></a>Birden çok VM için oto yönetimini etkinleştir

1. Arama çubuğunda, **Automanage-Azure Machine en iyi uygulamaları**' nı arayıp seçin.

2. **Mevcut VM 'de etkinleştir '** i seçin.

    :::image type="content" source="media\quick-create-virtual-machine-portal\zero-vm-list-view.png" alt-text="Mevcut VM 'de etkinleştirin.":::

3. **Makineleri seçin** dikey penceresinde:
    1. **Abonelik** ve **kaynak grubunuza** göre VM 'leri listesini filtreleyin.
    1. Eklemek istediğiniz her bir sanal makinenin onay kutusunu işaretleyin.
    1. **Seç** düğmesine tıklayın.

    :::image type="content" source="media\quick-create-virtual-machine-portal\existing-vm-select-machine.png" alt-text="Kullanılabilir VM 'Ler listesinden mevcut VM 'yi seçin.":::

4. **Yapılandırma profili** altında, Araştır ' a tıklayın **ve profilleri ve tercihleri değiştirin**.

    :::image type="content" source="media\quick-create-virtual-machine-portal\existing-vm-quick-create.png" alt-text="Profiller ve Tercihler 'e gözatıp değiştirin.":::

5. **Yapılandırma profilini seç + Tercihler** dikey penceresinde:
    1. Sol taraftaki bir profil seçin: test için *geliştirme/test* *, üretim için üretim.*
    1. **Seç** düğmesine tıklayın.

    :::image type="content" source="media\quick-create-virtual-machine-portal\browse-production-profile.png" alt-text="Üretim yapılandırma profiline gözatın.":::

6. **Etkinleştir** düğmesine tıklayın.


## <a name="enable-automanage-for-a-new-vm"></a>Yeni bir VM için oto yönetimini etkinleştir

Yeni bir sanal makine oluşturmak ve oto yönetimi etkinleştirmek için [burada](https://aka.ms/AutomanagePortal-Ignite21) Azure Portal oturum açın.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.

2. Azure Marketi kaynakları listesinin üzerindeki arama kutusunda, kullanmak istediğiniz görüntüyü arayıp seçin ve ardından **Oluştur**' u seçin.

> [!NOTE]
> Desteklenen [Linux Distro 'lara](automanage-linux.md#supported-linux-distributions-and-versions) ve [Windows Server sürümlerini](automanage-windows-server.md#supported-windows-server-versions)oto Yönet ' i inceleyin.

3. VM ayrıntılarınız ile **Ilgili temel bilgiler** sekmesini doldurun.

> [!NOTE]
> [Desteklenen bölgeleri](automanage-virtual-machines#supported-regions)oto Yönet ' i kontrol edin.

4. **Yönetim** sekmesine gidin ve **oto Yönet ortamınızı** seçin.

    :::image type="content" source="media\quick-create-virtual-machine-portal\VMCreate-Management-Tab.png" alt-text="Yönetim sekmesinde, oto Yönet 'i etkinleştirin.":::

5. Kalan varsayılan ayarları bırakın, ardından sayfanın alt kısmındaki **Gözden geçir + oluştur** düğmesini seçin.

6. Doğrulamanın geçtiğini belirten iletiyi gördüğünüzde **Oluştur**' u seçin.

## <a name="disable-automanage-for-vms"></a>VM 'Ler için oto yönetimini devre dışı bırak

Oto yönetimini devre dışı bırakarak sanal makineler için Azure oto yönetimi 'ni hızlıca durdurun.

:::image type="content" source="media\automanage-virtual-machines\disable-step-1.png" alt-text="Bir sanal makinede oto yönetimi devre dışı bırakılıyor.":::

1. Otomatik olarak yönetilen tüm sanal **makinelerinizi listeleyen otomatik Yönet – Azure sanal makine en iyi uygulamaları** sayfasına gidin.
1. Devre dışı bırakmak istediğiniz sanal makinenin yanındaki onay kutusunu seçin.
1. **Devre dışı bırak automanagent** düğmesine tıklayın.
1. Kabul etmiş önce **devre dışı bırakmak** için ortaya çıkan açılan pencerede iletiyi dikkatle okuyun.


## <a name="clean-up-resources"></a>Kaynakları temizleme

Sanal makineler için Azure oto yönetimi 'ni denemek üzere yeni bir kaynak grubu oluşturduysanız ve artık gerekmiyorsa, kaynak grubunu silebilirsiniz. Grubun silinmesi, VM 'yi ve kaynak grubundaki tüm kaynakları da siler.

Azure oto yönetimi, içindeki kaynakları depolamak için varsayılan kaynak gruplarını oluşturur. Tüm kaynakları temizlemek için "DefaultResourceGroupRegionName" ve "AzureBackupRGRegionName" adlandırma kuralına sahip kaynak gruplarını kontrol edin.

1. **Kaynak grubunu** seçin.
1. Kaynak grubunun sayfasında **Sil**' i seçin.
1. İstendiğinde, kaynak grubunun adını onaylayın ve **Sil**' i seçin.


## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta VM 'Ler için Azure oto yönetimi 'ni etkinleştirdiniz.

Sanal makinenizde oto yönetimi etkinleştirirken özelleştirilmiş tercihleri nasıl oluşturup uygulayabileceğinizi öğrenin.

> [!div class="nextstepaction"]
> [VM 'Ler için Azure oto yönetimi-özel yapılandırma profili](virtual-machines-custom-preferences.md)
