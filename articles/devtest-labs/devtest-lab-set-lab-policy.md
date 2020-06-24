---
title: Laboratuvar ilkelerini Azure DevTest Labs yönetme | Microsoft Docs
description: VM boyutları, Kullanıcı başına en fazla VM ve kapalı otomasyon gibi Laboratuvar ilkeleri tanımlama hakkında bilgi edinin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.assetid: 7756aa64-49ca-45a0-9f90-0fd101c7be85
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: aa0ffbd69e73ddbef72e0eabf79f2736079c3d23
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84896805"
---
# <a name="manage-all-policies-for-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs laboratuvar için tüm ilkeleri yönetme

Azure DevTest Labs, her bir laboratuvarın ilkelerini (ayarlarını) yöneterek laboratuvarınızda maliyeti denetlemenizi ve en aza indirecek şekilde yararlanmanızı sağlar. Bu makalede, her bir ilkeyi nasıl ayarlayabileceğiniz adım adım ayrıntılı bilgiler açıklanmaktadır.  

## <a name="set-allowed-virtual-machine-sizes"></a>İzin verilen sanal makine boyutlarını ayarla
İzin verilen VM boyutlarını ayarlamaya yönelik ilke, laboratuvarda hangi VM boyutlarının izin verileceğini belirtmenizi sağlayarak laboratuvar israfını en aza indirmenize yardımcı olur. Bu ilke etkinleştirildiyse, VM 'Ler oluşturmak için yalnızca bu listedeki VM boyutları kullanılabilir.

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040)bir laboratuvar seçin ve ardından **yapılandırma ve ilkeler**' i seçin.

    ![Laboratuvarın yapılandırmasına ve ilkelerine erişin](./media/devtest-lab-set-lab-policy/policies-menu.png)

1. Laboratuvarın **yapılandırma ve ilkeler** bölmesinde **izin verilen sanal makine boyutları**' nı seçin.
   
    ![İzin verilen sanal makine boyutları](./media/devtest-lab-set-lab-policy/allowed-vm-sizes.png)

1. Bu ilkeyi **etkinleştirmek ve devre dışı bırakmak Için** **Açık** seçeneğini belirleyin.

1. Bu ilkeyi etkinleştirirseniz laboratuvarınızda oluşturulabilecek bir veya daha fazla VM boyutunu seçin.

1. **Kaydet**’i seçin.

## <a name="set-virtual-machines-per-user"></a>Kullanıcı başına sanal makine ayarlama
**Kullanıcı başına sanal makine** ilkesi, tek bir kullanıcı tarafından oluşturulabilen VM sayısını belirtmenizi sağlar. Kullanıcı sınırı karşılandığında bir sanal makine oluşturmaya veya talep almaya çalışırsa, VM 'nin oluşturuamayacağını/istenmediğini belirten bir hata iletisi görüntülenir. 

1. Laboratuvarın **yapılandırma ve ilkeler** bölmesinde **Kullanıcı başına sanal makine**' yi seçin.
   
    ![Kullanıcı başına sanal makine sayısı](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. Kullanıcı başına VM sayısını sınırlandırmak için **Evet** ' i seçin. Kullanıcı başına VM sayısını sınırlandırmak istemiyorsanız, **Hayır**' ı seçin. **Evet**' i seçerseniz, bir kullanıcı tarafından oluşturulabilen veya talep edilen sanal makine sayısını gösteren sayısal bir değer girin. 

1. SSD 'yi (katı hal diski) kullanılabilecek VM 'lerin sayısını sınırlandırmak için **Evet** ' i seçin. SSD 'yi kullanacak sanal makinelerin sayısını sınırlandırmak istemiyorsanız **Hayır**' ı seçin. **Evet**' i SEÇERSENIZ, SSD kullanılarak oluşturulabilen VM sayısını belirten bir değer girin. 

1. **Kaydet**’i seçin.

## <a name="set-virtual-machines-per-lab"></a>Laboratuvar başına sanal makine ayarlama
**Laboratuvar başına sanal makine** ilkesi, geçerli laboratuvar Için oluşturulabilecek VM sayısını belirtmenizi sağlar. Laboratuvar sınırı karşılandığında bir Kullanıcı VM oluşturmaya çalışırsa, VM 'nin oluşturuamayacağını belirten bir hata iletisi görüntülenir. 

1. Laboratuvarın **yapılandırma ve ilkeler** bölmesinde, **Laboratuvar başına sanal makine**' yi seçin.
   
    ![Laboratuvar başına sanal makine sayısı](./media/devtest-lab-set-lab-policy/max-vms-per-lab.png)

1. Laboratuvar başına sanal makine sayısını sınırlandırmak için **Evet** ' i seçin. Laboratuvar başına sanal makine sayısını sınırlamak istemiyorsanız **Hayır**' ı seçin. **Evet**' i seçerseniz, bir kullanıcı tarafından oluşturulabilen veya talep edilen sanal makine sayısını gösteren sayısal bir değer girin. 

1. SSD 'yi (katı hal diski) kullanılabilecek VM 'lerin sayısını sınırlandırmak için **Evet** ' i seçin. SSD 'yi kullanacak sanal makinelerin sayısını sınırlandırmak istemiyorsanız **Hayır**' ı seçin. **Evet**' i SEÇERSENIZ, SSD kullanılarak oluşturulabilen VM sayısını belirten bir değer girin. 

1. **Kaydet**’i seçin.

## <a name="set-auto-shutdown"></a>Otomatik kapanışı ayarla
Otomatik kapatma ilkesi, bu laboratuvarın VM 'lerinin kapandığı saati belirtmenize izin vererek laboratuvar harcanmasının en aza indirilmesine yardımcı olur.

1. Laboratuvarın **yapılandırma ve ilkeler** bölmesinde **otomatik olarak kapalı**' yı seçin.
   
    ![Otomatik olarak kapatılıyor](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. Bu ilkeyi **etkinleştirmek ve devre dışı bırakmak Için** **Açık** seçeneğini belirleyin.

1. Bu ilkeyi etkinleştirirseniz, geçerli laboratuvardaki tüm VM 'Leri kapatmak için saati (ve saat dilimini) belirtin.

1. Belirtilen otomatik kapatılma zamanından 15 dakika önce bildirim gönderme seçeneği için Evet veya **Hayır** **değerini** belirtin. **Evet**' i seçerseniz, bir Web kancası URL uç noktası veya bildirimin nakledilmesini veya gönderilmesini istediğinizi belirten bir e-posta adresi girin. Kullanıcı bildirimi alır ve kapanışı erteleme seçeneği verilir.

   Web kancaları hakkında daha fazla bilgi için bkz. [Web kancası veya API Azure Işlevi oluşturma](../azure-functions/functions-create-a-web-hook-or-api-function.md). 

1. **Kaydet**’i seçin.

Varsayılan olarak, bu ilke, etkinleştirildikten sonra geçerli laboratuvardaki tüm VM 'Ler için geçerlidir. Bu ayarı belirli bir VM 'den kaldırmak için VM 'nin Yönetim bölmesini açın ve **otomatik kapatılma** ayarını değiştirin.

## <a name="set-auto-shutdown-policy"></a>Otomatik kapatılma ilkesini ayarla
Laboratuvar sahibi olarak, laboratuvarınızda tüm VM 'Ler için bir kapalı zamanlama yapılandırabilirsiniz. Bunu yaparak, kullanılmayan makinelerden (boşta) maliyetlerin tasarruf edebilirsiniz. Tüm laboratuar sanal makinelerinizde bir kapalı ilke uygulayabilir, ancak laboratuvar kullanıcılarınıza bireysel makineler için bir zamanlama ayarlama çabalarını kaydedebilirsiniz. Bu özellik, laboratuvar zamanlamanızda ilke ayarlamanıza olanak sağlayarak laboratuvar kullanıcılarınıza denetim olmadan tam denetim sunmadan başlayarak ilkeyi ayarlamanıza olanak sağlar. Laboratuvar sahibi olarak, aşağıdaki adımları uygulayarak bu ilkeyi yapılandırabilirsiniz:

1. Laboratuvarınızın giriş sayfasında **yapılandırma ve ilkeler**' i seçin.
2. Sol menünün **zamanlamalar** bölümünde **otomatik kapatılma ilkesi** ' ni seçin.
3. Seçeneklerden birini belirleyin. Aşağıdaki bölümlerde bu seçenekler hakkında daha fazla ayrıntı verilmektedir: Set Policy yalnızca laboratuvarda oluşturulan yeni VM 'lere uygulanır ve mevcut VM 'lere uygulanmaz. 

    ![Otomatik olarak kapatılmış ilke seçenekleri](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-options.png)

### <a name="user-sets-a-schedule-and-can-opt-out"></a>Kullanıcı bir zamanlama ayarlar ve devre dışı bırakabilirsiniz
Laboratuvarınızı bu ilkeye ayarlarsanız, laboratuvar kullanıcıları laboratuvar zamanlamasını geçersiz kılabilir veya devre dışı bırakabilirsiniz. Bu seçenek, laboratuvar kullanıcılarına VM 'lerinin otomatik kapatılma zamanlaması üzerinde tam denetim verir. Laboratuvar kullanıcıları, sanal makine otomatik kapatılma zamanlaması sayfasında değişiklik görmez.

![Otomatik olarak kapatılmış ilke seçeneği-1](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-1.png)

### <a name="user-sets-a-schedule-and-cannot-opt-out"></a>Kullanıcı bir zamanlama ayarlıyor ve devre dışı bırakılamaz
Laboratuvarınızı bu ilkeye ayarlarsanız, laboratuvar kullanıcıları laboratuvar zamanlamasını geçersiz kılabilir. Ancak otomatik kapatma ilkesini devre dışı bırakıamazlar. Bu seçenek, laboratuvarınızda bulunan her makinenin otomatik kapatılma zamanlaması altında olmasını sağlar. Laboratuvar kullanıcıları, sanal makinelerinin otomatik kapatma zamanlamasını güncelleştirebilir ve kapalı bildirimleri ayarlayabilir.

![Otomatik olarak kapatılmış ilke seçeneği-2](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-2.png)

### <a name="user-has-no-control-over-the-schedule-set-by-lab-admin"></a>Kullanıcının Laboratuvar Yöneticisi tarafından ayarlanan zamanlama üzerinde denetimi yok
Laboratuvarınızı bu ilkeye ayarlarsanız, laboratuvar kullanıcıları laboratuvar zamanlamasını geçersiz kılabilir veya devre dışı kılamaz. Bu seçenek, laboratuvardaki her makine için zamanlamaya göre tüm denetimi laboratuvar yöneticisine sunar. Laboratuvar kullanıcıları, sanal makineleri için yalnızca otomatik olarak kapanmaya yönelik bildirimler ayarlayabilir.

![Otomatik olarak kapatılıyor ilke seçeneği-3](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-3.png)

## <a name="set-autostart"></a>Autostart 'ı ayarla
Otomatik başlatma ilkesi, geçerli laboratuvardaki VM 'Lerin ne zaman başlatılacağını belirtmenize izin verir.  

1. Laboratuvarın **yapılandırma ve ilkeler** bölmesinde otomatik **Başlat**' ı seçin.
   
    ![Otomatik başlatma](./media/devtest-lab-set-lab-policy/auto-start.png)

2. Bu ilkeyi **etkinleştirmek ve devre dışı bırakmak Için** **Açık** seçeneğini belirleyin.

3. Bu ilkeyi etkinleştirirseniz, zamanlanan başlangıç saati, saat dilimi ve saatin geçerli olduğu haftanın günleri belirtin. 

4. **Kaydet**’i seçin.

Bu ilke etkinleştirildikten sonra, geçerli laboratuvardaki tüm sanal makinelere otomatik olarak uygulanmaz. Bu ayarı belirli bir VM 'ye uygulamak için, VM 'nin Yönetim bölmesini açın ve **autostart** ayarını değiştirin.

## <a name="set-expiration-date"></a>Sona erme tarihi ayarla
[VM oluştururken](devtest-lab-add-vm.md)bir sona erme tarihi ayarlayabilirsiniz. **Gelişmiş ayarlar**' da, VM 'nin otomatik olarak silineceği bir tarihi belirtmek için takvim simgesini seçin. Varsayılan olarak, VM hiçbir zaman sona ermez.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
Laboratuvarınız için çeşitli VM ilkesi ayarlarını tanımladıktan ve uyguladıktan sonra, daha sonra deneyebileceğiniz bazı şeyler aşağıda verilmiştir:

* [PAYLAŞıLAN IP adreslerini anlama](devtest-lab-shared-ip.md) -laboratuvar sanal makinelerinize bağlanmak için gereken genel IP adreslerinin sayısını en aza indirmek için, paylaşılan IP adreslerinin DevTest Labs 'de nasıl kullanıldığını açıklar.
* [Maliyet yönetimini yapılandırma](devtest-lab-configure-cost-management.md) - **aylık tahmini maliyet eğilimi** grafiğinin nasıl kullanılacağını açıklar  
  geçerli ayın tahmini maliyetini ve tahmini aylık ücreti görüntülemek için.
* [Özel görüntü oluşturma](devtest-lab-create-template.md) -bir VM oluşturduğunuzda, özel bir görüntü veya Market görüntüsü olabilen bir temel belirlersiniz. Bu makalede, bir VHD dosyasından nasıl özel bir görüntü oluşturacağınız gösterilmektedir.
* [Market görüntülerini yapılandırma](devtest-lab-configure-marketplace-images.md) -Azure DevTest Labs Azure Market görüntülerine göre VM oluşturmayı destekler. Bu makalede, bir laboratuvarda VM oluştururken Azure Market görüntülerinin kullanılabileceği, hangilerinin kullanılacağı gösterilmektedir.
* [Laboratuvarda sanal makine oluşturma](devtest-lab-add-vm.md) -bir temel görüntüden (özel veya Market) sanal makine OLUŞTURMAYı ve sanal makinenizde yapıtlarla çalışmayı gösterir.

