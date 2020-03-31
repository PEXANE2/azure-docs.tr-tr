---
title: Azure DevTest Labs'da laboratuvar ilkelerini yönetme | Microsoft Dokümanlar
description: VM boyutları, kullanıcı başına maksimum VM ve kapatma otomasyonu gibi laboratuvar ilkelerini nasıl tanımlarız öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79270725"
---
# <a name="manage-all-policies-for-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'daki bir laboratuvar için tüm ilkeleri yönetme

Azure DevTest Labs, her laboratuvar için ilkeler (ayarlar) yöneterek laboratuvarlarınızdaki maliyeti kontrol etmenizi ve atıkları en aza indirmenizi sağlar. Bu makalede, her ilkenin nasıl ayarlanılabilen adım adım ayrıntılı olarak açıklanmaktadır.  

## <a name="set-allowed-virtual-machine-sizes"></a>İzin verilen sanal makine boyutlarını ayarlama
İzin verilen VM boyutlarını ayarlama ilkesi, laboratuvarda hangi VM boyutlarına izin verildiğini belirtmenize olanak sağlayarak laboratuvar atıklarını en aza indirmenize yardımcı olur. Bu ilke etkinleştirilirse, VM oluşturmak için yalnızca bu listeden VM boyutları kullanılabilir.

1. Azure [portalında,](https://go.microsoft.com/fwlink/p/?LinkID=525040)bir laboratuvar seçin ve ardından **Yapılandırma ve ilkeler'i**seçin.

    ![Laboratuvarın yapılandırmave ilkelerine erişin](./media/devtest-lab-set-lab-policy/policies-menu.png)

1. Laboratuvarın Yapılandırma **ve ilkeler** bölmelerinde, **İzin Verilen sanal makine boyutlarını**seçin.
   
    ![İzin verilen sanal makine boyutları](./media/devtest-lab-set-lab-policy/allowed-vm-sizes.png)

1. Bu ilkeyi etkinleştirmek **için** **On'u** ve devre dışı bırak'ı seçin.

1. Bu ilkeyi etkinleştiriyseniz, laboratuarınızda oluşturulabilecek bir veya daha fazla VM boyutu seçin.

1. **Kaydet'i**seçin.

## <a name="set-virtual-machines-per-user"></a>Kullanıcı başına sanal makineleri ayarlama
**Kullanıcı başına Sanal makineler** için ilke, tek bir kullanıcı tarafından oluşturulabilecek VM sayısını belirtmenize olanak tanır. Kullanıcı sınırı karşılandığında bir Kullanıcı VM oluşturmaya veya talep etmeye çalışırsa, bir hata iletisi VM'nin oluşturulamayacağını/talep edilemeyeceğini gösterir. 

1. Laboratuvarın Yapılandırma **ve ilkeler** bölmelerinde, **kullanıcı başına Sanal makineleri**seçin.
   
    ![Kullanıcı başına sanal makineler](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. Kullanıcı başına VM sayısını sınırlamak için **Evet'i** seçin. Kullanıcı başına VM sayısını sınırlamak istemiyorsanız, **Hayır'ı**seçin. **Evet'i**seçerseniz, bir kullanıcı tarafından oluşturulabilecek veya talep edilebilen VM sayısını gösteren sayısal bir değer girin. 

1. SSD (katı hal diski) kullanabilen VM sayısını sınırlamak için **Evet'i** seçin. SSD'yi kullanabilen VM sayısını sınırlamak istemiyorsanız, **Hayır'ı**seçin. **Evet'i**seçerseniz, SSD kullanılarak oluşturulabilecek VM sayısını gösteren bir değer girin. 

1. **Kaydet'i**seçin.

## <a name="set-virtual-machines-per-lab"></a>Laboratuvar başına sanal makineleri ayarlama
**Laboratuvar başına Sanal makineler** için ilke, geçerli laboratuvar için oluşturulabilecek VM sayısını belirtmenize olanak tanır. Bir kullanıcı laboratuvar sınırı karşılandığında bir VM oluşturmaya çalışırsa, bir hata iletisi VM oluşturulamayacağını gösterir. 

1. Laboratuvarın Yapılandırma **ve ilkeler** bölmelerinde, **laboratuvar başına Sanal makineleri**seçin.
   
    ![Laboratuvar başına sanal makineler](./media/devtest-lab-set-lab-policy/max-vms-per-lab.png)

1. Laboratuvar başına VM sayısını sınırlamak için **Evet'i** seçin. Laboratuvar başına VM sayısını sınırlamak istemiyorsanız, **No'yu**seçin. **Evet'i**seçerseniz, bir kullanıcı tarafından oluşturulabilecek veya talep edilebilen VM sayısını gösteren sayısal bir değer girin. 

1. SSD (katı hal diski) kullanabilen VM sayısını sınırlamak için **Evet'i** seçin. SSD'yi kullanabilen VM sayısını sınırlamak istemiyorsanız, **Hayır'ı**seçin. **Evet'i**seçerseniz, SSD kullanılarak oluşturulabilecek VM sayısını gösteren bir değer girin. 

1. **Kaydet'i**seçin.

## <a name="set-auto-shutdown"></a>Otomatik kapatmayı ayarlama
Otomatik kapatma ilkesi, bu laboratuvarın VM'lerinin kapattığı zamanı belirtmenize izin vererek laboratuvar atıklarını en aza indirmenize yardımcı olur.

1. Laboratuvarın Yapılandırma **ve ilkeler** bölmelerinde **Otomatik kapatma'yı**seçin.
   
    ![Otomatik kapatma](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. Bu ilkeyi etkinleştirmek **için** **On'u** ve devre dışı bırak'ı seçin.

1. Bu ilkeyi etkinleştiriseniz, geçerli laboratuardaki tüm VM'leri kapatmak için saati (ve saat dilimini) belirtin.

1. Belirtilen otomatik kapatma saatinden 15 dakika önce bildirim gönderme seçeneği için **Evet** veya **Hayır** belirtin. **Evet'i**seçerseniz, bildirimin nerede gönderilmesini veya gönderilmesini istediğinizi belirten bir webhook URL bitiş noktası veya e-posta adresi girin. Kullanıcı bildirim alır ve kapatmayı erteleme seçeneği verilir.

   Web hooks hakkında daha fazla bilgi için [bkz.](../azure-functions/functions-create-a-web-hook-or-api-function.md) 

1. **Kaydet'i**seçin.

Varsayılan olarak, bir kez etkinleştirildiğinde, bu ilke geçerli laboratuardaki tüm VM'ler için geçerlidir. Bu ayarı belirli bir VM'den kaldırmak için VM'nin yönetim bölmesini açın ve **Otomatik kapatma** ayarını değiştirin.

## <a name="set-auto-shutdown-policy"></a>Otomatik kapatma ilkesini ayarlama
Laboratuvar sahibi olarak, laboratuarınızdaki tüm VM'ler için bir kapatma zamanlamasını yapılandırabilirsiniz. Bunu yaparak, kullanılmayan (boşta) çalışan makinelerden maliyet kaydedebilirsiniz. Tüm laboratuvar VM'lerinizde bir kapatma ilkesi uygulayabilir ve aynı zamanda laboratuvar kullanıcılarınızı tek tek makineleri için bir zamanlama ayarlamaktan kurtarabilirsiniz. Bu özellik, hiçbir denetim sunmadan tam denetime, laboratuvar kullanıcılarınıza çalışmaktan başlayarak ilkeyi laboratuvar zamanlamanızda ayarlamanızı sağlar. Bir laboratuvar sahibi olarak, aşağıdaki adımları atarak bu ilkeyi yapılandırabilirsiniz:

1. Laboratuvarınızın ana sayfasında Yapılandırma **ve ilkeler'i**seçin.
2. Sol menünün **Zamanlamalar** bölümünde **Otomatik kapatma ilkesini** seçin.
3. Seçeneklerden birini seçin. Aşağıdaki bölümler size bu seçenekler hakkında daha fazla ayrıntı verir: Ayarlanan ilke yalnızca laboratuvarda oluşturulan yeni VM'ler için geçerlidir, zaten var olan VM'ler için geçerli değildir. 

    ![Otomatik kapatma ilkesi seçenekleri](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-options.png)

### <a name="user-sets-a-schedule-and-can-opt-out"></a>Kullanıcı bir zamanlama ayarlar ve devre dışı kalabilir
Laboratuvarınızı bu ilkeye ayarlarsanız, laboratuar kullanıcıları geçersiz kılınabilir veya laboratuvar zamanlamasını devre dışı bırakabilirsiniz. Bu seçenek, laboratuvar kullanıcılarına VM'lerinin otomatik kapatma zamanlaması üzerinde tam denetim sağlar. Laboratuvar kullanıcıları VM otomatik kapatma zamanlama sayfalarında değişiklik görmezler.

![Otomatik kapatma ilkesi seçeneği - 1](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-1.png)

### <a name="user-sets-a-schedule-and-cannot-opt-out"></a>Kullanıcı bir zamanlama ayarlar ve devre dışı kaldıramaz
Laboratuvarınızı bu ilkeye ayarlarsanız, laboratuvar kullanıcıları laboratuvar zamanlamasını geçersiz kılabilir. Ancak, otomatik kapatma ilkesini devre dışı kaldırabilirsiniz. Bu seçenek, laboratuarınızdaki her makinenin otomatik kapatma zamanlaması altında olmasını sağlar. Laboratuvar kullanıcıları, VM'lerinin otomatik kapatma zamanlamasını güncelleyebilir ve kapatma bildirimlerini ayarlayabilir.

![Otomatik kapatma ilkesi seçeneği - 2](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-2.png)

### <a name="user-has-no-control-over-the-schedule-set-by-lab-admin"></a>Kullanıcı nın laboratuvar yöneticisi tarafından ayarlanan zamanlama üzerinde hiçbir denetimi yoktur
Laboratuvarınızı bu ilkeye ayarlarsanız, laboratuar kullanıcıları geçersiz kalamaz veya laboratuvar zamanlamasını devre dışı bırakamaz. Bu seçenek, laboratuvar yöneticisine laboratuardaki her makine için zamanlamada tam denetim sunar. Laboratuvar kullanıcıları yalnızca VM'leri için otomatik kapatma bildirimleri ayarlayabilir.

![Otomatik kapatma ilkesi seçeneği - 3](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-3.png)

## <a name="set-autostart"></a>Otomatik başlatmayı ayarlama
Otomatik başlatma ilkesi, geçerli laboratuardaki VM'lerin ne zaman başlatılması gerektiğini belirtmenize olanak tanır.  

1. Laboratuvarın Yapılandırma **ve ilkeler** bölmelerinde **Otomatik Başlat'ı**seçin.
   
    ![Otomatik başlatma](./media/devtest-lab-set-lab-policy/auto-start.png)

2. Bu ilkeyi etkinleştirmek **için** **On'u** ve devre dışı bırak'ı seçin.

3. Bu ilkeyi etkinleştiriseniz, zamanlanan başlangıç saatini, saat dilimini ve saatin uygulandığı haftanın günlerini belirtin. 

4. **Kaydet'i**seçin.

Etkinleştirildikten sonra, bu ilke geçerli laboratuardaki hiçbir VM'ye otomatik olarak uygulanmaz. Bu ayarı belirli bir VM'ye uygulamak için VM'nin yönetim bölmesini açın ve **Otomatik Başlatma** ayarını değiştirin.

## <a name="set-expiration-date"></a>Son kullanma tarihini ayarlama
[VM'yi oluşturduğunuzda](devtest-lab-add-vm.md)bir son kullanma tarihi ayarlayabilirsiniz. **Gelişmiş ayarlarda,** VM'nin otomatik olarak silindiği tarihi belirtmek için takvim simgesini seçin. Varsayılan olarak, VM'nin süresi asla dolmaz.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
Laboratuarınız için çeşitli VM ilke ayarlarını tanımladıktan ve uyguladıktan sonra, ardından deneyecek bazı şeyler şunlardır:

* [Paylaşılan IP adreslerini anlama](devtest-lab-shared-ip.md) - Laboratuvar VM'lerinize bağlanmak için gereken genel IP adreslerinin sayısını en aza indirmek için DevTest Labs'da paylaşılan IP adreslerinin nasıl kullanıldığını açıklar.
* [Maliyet yönetimini yapılandırma](devtest-lab-configure-cost-management.md) - **Aylık Tahmini Maliyet Eğilimi** grafiğinin nasıl kullanılacağını gösterir  
  geçerli ayın tahmini maliyet-to-date ve öngörülen ay sonu maliyetini görüntülemek için.
* [Özel görüntü oluşturma](devtest-lab-create-template.md) - VM oluşturduğunuzda, özel bir görüntü veya Market görüntüsü olabilecek bir taban belirtirsiniz. Bu makalede, bir VHD dosyasından özel bir görüntü oluşturmak için nasıl gösteriş.
* [Market görüntülerini yapılandırma](devtest-lab-configure-marketplace-images.md) - Azure DevTest Labs, Azure Marketi görüntülerine dayalı VM oluşturmayı destekler. Bu makalede, bir laboratuvarda VM oluştururken hangi Azure Marketi görüntülerinin kullanılabilir, varsa nasıl kullanılacağı belirtilecektir.
* [Laboratuvarda VM oluşturma](devtest-lab-add-vm.md) - Temel görüntüden (özel veya Pazar Yeri) VM'nin nasıl oluşturulabildiğini ve VM'nizdeki yapılarla nasıl çalışacağınızı gösterir.

