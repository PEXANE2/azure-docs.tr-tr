---
title: VM 'Ler için Azure oto yönetiminde özel bir tercih oluşturma
description: VM 'Ler için Azure oto yönetimi 'nde yapılandırma profilini ayarlamayı ve kendi tercihlerinizi ayarlamayı öğrenin.
author: ju-shim
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: how-to
ms.date: 09/04/2020
ms.author: jushiman
ms.openlocfilehash: 377677c9e5e81487059241db68baff639a3de033
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91715042"
---
# <a name="create-a-custom-preference-in-azure-automanage-for-vms"></a>VM 'Ler için Azure oto yönetiminde özel bir tercih oluşturma

Sanal makine için Azure oto yönetimi en iyi yöntemleri, gerekirse ayarlanabilecek varsayılan yapılandırma profillerine sahiptir. Bu makalede, yeni veya mevcut bir VM 'de oto yönetimini etkinleştirdiğinizde kendi yapılandırma profili tercihlerinizi nasıl ayarlayabileceğiniz anlatılmıştır.

Şu anda [Azure Backup](..\backup\backup-azure-arm-vms-prepare.md#create-a-custom-policy) ve [Microsoft Antimalware](../security/fundamentals/antimalware.md#default-and-custom-antimalware-configuration)'te özelleştirmeleri destekliyoruz.


> [!NOTE]
> Bir yapılandırma profilini veya bir tercihi, sizin için, oto yönetimi etkinken değiştiremezsiniz. Bu VM için, bu sanal makine için yeniden yönetme 'yi devre dışı bırakmanız ve ardından, istenen yapılandırma profili ve tercihleriyle tekrar yönetmeyi yeniden etkinleştirmeniz gerekir.


## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [bir hesap oluşturun](https://azure.microsoft.com/pricing/purchase-options/pay-as-you-go/) .

> [!NOTE]
> Ücretsiz deneme hesaplarının bu öğreticide kullanılan sanal makinelere erişimi yoktur. Lütfen Kullandıkça Öde aboneliğine yükseltin.

> [!IMPORTANT]
> Aşağıdaki Azure RBAC izni, **Kullanıcı erişimi yönetici** rolleriyle birlikte, oto yönetimi: **Owner** rolü veya **katkıda bulunan** ' i etkinleştirmek için gereklidir.


## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](https://portal.azure.com/) oturum açın.


## <a name="enable-automanage-for-vms-on-an-existing-vm"></a>Mevcut bir VM 'de VM 'Ler için oto yönetimini etkinleştir

1. Arama çubuğunda, **Automanage: Azure sanal makine en iyi uygulamaları**' nı arayıp seçin.

2. **Mevcut VM 'de etkinleştir '** i seçin.

3. **Makineleri seçin** dikey penceresinde:
    1. **Abonelik** ve **kaynak grubunuza**göre VM 'leri listesini filtreleyin.
    1. Eklemek istediğiniz her bir sanal makinenin onay kutusunu işaretleyin.
    1. **Seç** düğmesine tıklayın.

    :::image type="content" source="media\virtual-machine-custom-preferences\existing-vm-select-machine.png" alt-text="Kullanılabilir VM 'Ler listesinden mevcut VM 'yi seçin.":::

4. **Yapılandırma profili**altında, Araştır ' a tıklayın **ve profilleri ve tercihleri değiştirin**.

    :::image type="content" source="media\virtual-machine-custom-preferences\existing-vm-quick-create.png" alt-text="Kullanılabilir VM 'Ler listesinden mevcut VM 'yi seçin.":::

5. **Yapılandırma profilini seç + Tercihler** dikey penceresinde sol taraftaki bir profil seçin: test için *geliştirme/test* *, üretime yönelik üretim.*

    :::image type="content" source="media\virtual-machine-custom-preferences\browse-production-profile.png" alt-text="Kullanılabilir VM 'Ler listesinden mevcut VM 'yi seçin.":::

6. Seçilen profilde, **Yapılandırma tercihleri** altında belirli hizmetler için ayarlayabileceğiniz bir açılan menü bulunur.
    1. **Yeni Tercihler oluştur**' a tıklayın.
    1. **Yapılandırma tercihi oluştur** dikey penceresinde temel bilgiler sekmesini doldurun:
        1. Abonelik
        1. Kaynak grubu
        1. Tercih adı
        1. Bölge

    :::image type="content" source="media\virtual-machine-custom-preferences\create-preference.png" alt-text="Kullanılabilir VM 'Ler listesinden mevcut VM 'yi seçin.":::

7. Tercihler sekmesine gidin ve istediğiniz yapılandırma tercihlerini ayarlayın.
        
    > [!NOTE]
    > Profil yapılandırmalarının değiştirilmesi sırasında yalnızca en iyi uygulama üst ve alt limitlerimize sığabilecek ayarlamalarına izin verilir.

8. Yapılandırma profilinizi gözden geçirin.
9. **Oluştur** düğmesine tıklayın.

10. **Etkinleştir** düğmesine tıklayın.


## <a name="disable-automanage-for-vms"></a>VM 'Ler için oto yönetimini devre dışı bırak

Oto yönetimini devre dışı bırakarak sanal makineler için Azure oto yönetimi 'ni hızlıca durdurun.

:::image type="content" source="media\virtual-machine-custom-preferences\disable-step-1.png" alt-text="Kullanılabilir VM 'Ler listesinden mevcut VM 'yi seçin.":::

1. Otomatik olarak yönetilen tüm sanal **makinelerinizi listeleyen otomatik Yönet – Azure sanal makine en iyi uygulamaları** sayfasına gidin.
1. Devre dışı bırakmak istediğiniz sanal makinenin yanındaki onay kutusunu seçin.
1. **Devre dışı bırak automanagent** düğmesine tıklayın.
1. Kabul etmiş önce **devre dışı bırakmak**için ortaya çıkan açılan pencerede iletiyi dikkatle okuyun.


## <a name="clean-up-resources"></a>Kaynakları temizleme

Sanal makineler için Azure oto yönetimi 'ni denemek üzere yeni bir kaynak grubu oluşturduysanız ve artık gerekmiyorsa, kaynak grubunu silebilirsiniz. Grubun silinmesi, VM 'yi ve kaynak grubundaki tüm kaynakları da siler.

Azure oto yönetimi, içindeki kaynakları depolamak için varsayılan kaynak gruplarını oluşturur. Tüm kaynakları temizlemek için "DefaultResourceGroupRegionName" ve "AzureBackupRGRegionName" adlandırma kuralına sahip kaynak gruplarını kontrol edin.

1. **Kaynak grubunu**seçin.
1. Kaynak grubunun sayfasında **Sil**' i seçin.
1. İstendiğinde, kaynak grubunun adını onaylayın ve **Sil**' i seçin.


## <a name="next-steps"></a>Sonraki adımlar 

SSS bölümünde Cevaplanan en sık sorulan soruları öğrenin. 

> [!div class="nextstepaction"]
> [Sık Sorulan Sorular](faq.md)