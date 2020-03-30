---
title: Azure Güvenlik Merkezi'nde tam zamanında sanal makine erişimi | Microsoft Dokümanlar
description: Bu belge, Azure Güvenlik Merkezi'ndeki tam zamanında VM erişiminin Azure sanal makinelerinize erişimi denetlemenize nasıl yardımcı olduğunu gösterir.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: memildin
ms.openlocfilehash: 4b2b388fb736997010a6cbbdf93b23b77c7ef3a3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77603976"
---
# <a name="secure-your-management-ports-with-just-in-time-access"></a>Tam zamanında erişimle yönetim bağlantı noktalarınızı güvenli hale

Güvenlik Merkezi'nin standart fiyatlandırma katmanındaysanız [(fiyatlandırmaya](/azure/security-center/security-center-pricing)bakın), tam zamanında (JIT) sanal makine (VM) erişimiyle Azure VM'lerinize gelen trafiği kilitleyebilirsiniz. Bu, gerektiğinde VM'lere bağlanmak için kolay erişim sağlarken saldırılara maruz kalmamayı azaltır.

> [!NOTE]
> Güvenlik Merkezi tam zamanında VM erişimi şu anda yalnızca Azure Kaynak Yöneticisi aracılığıyla dağıtılan VM'leri destekler. Klasik ve Kaynak Yöneticisi dağıtım modelleri hakkında daha fazla bilgi edinmek için [Azure Kaynak Yöneticisi ve klasik dağıtıma](../azure-resource-manager/management/deployment-models.md)bakın.

[!INCLUDE [security-center-jit-description](../../includes/security-center-jit-description.md)]

## <a name="configure-jit-on-a-vm"></a>VM'de JIT'yi yapılandırma

Bir VM üzerinde JIT ilkesini yapılandırmanın üç yolu vardır:

- [Azure Güvenlik Merkezi'nde JIT erişimini yapılandırma](#jit-asc)
- [Azure VM sayfasında JIT erişimini yapılandırma](#jit-vm)
- [VM üzerinde jit ilkesini programlı olarak yapılandırma](#jit-program)

## <a name="configure-jit-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde JIT'yi yapılandırın

Güvenlik Merkezi'nden bir JIT ilkesini yapılandırabilir ve JIT ilkesini kullanarak VM'ye erişim isteğinde bulunabilirsiniz

### <a name="configure-jit-access-on-a-vm-in-security-center"></a>Güvenlik Merkezi'ndeki bir VM'de JIT erişimini yapılandırma<a name="jit-asc"></a>

1. **Güvenlik Merkezi** panosunu açın.

1. Sol bölmede, **Tam zamanında VM erişimi**seçin.

    ![Tam zamanında VM erişim döşemesi](./media/security-center-just-in-time/just-in-time.png)

    **Tam zamanında VM erişim** penceresi açılır ve VM'lerinizin durumu yla ilgili bilgileri gösterir:

    - **Tam** zamanında VM erişimini destekleyecek şekilde yapılandırılan - VM'ler. Sunulan veriler geçen hafta içindir ve her VM için onaylanan istek sayısını, son erişim tarihini ve saatini ve son kullanıcıyı içerir.
    - **Önerilen** - Tam zamanında VM erişimini destekleyebilen ancak yapılandırılmayan VM'ler. Bu VM'ler için tam zamanında VM erişim denetimini etkinleştirmenizi öneririz.
    - **Öneri olmayan** - Bir VM’nin önerilmemesinin olası nedenleri şunlardır:
      - Eksik NSG - Tam zamanında çözüm yerine bir NSG gerektirir.
      - Klasik VM - Güvenlik Merkezi tam zamanında VM erişimi şu anda yalnızca Azure Kaynak Yöneticisi aracılığıyla dağıtılan VM'leri destekler. Klasik bir dağıtım, tam zamanında çözüm tarafından desteklenmez. 
      - Diğer - Aboneliğin veya kaynak grubunun güvenlik ilkesinde tam zamanında çözüm kapatılırsa veya VM'de genel bir IP eksikse ve yerinde Bir NSG yoksa, VM bu kategoridedir.

1. **Önerilen** sekmeyi seçin.

1. **VIRTUAL MACHINE**altında, etkinleştirmek istediğiniz VM'leri tıklatın. Bu, VM'nin yanına bir onay işareti koyar.

      ![Tam zamanında erişimi etkinleştirme](./media/security-center-just-in-time/enable-just-in-time.png)

1. **VM'lerde JIT'yi etkinleştir'i**tıklatın. Azure Güvenlik Merkezi tarafından önerilen varsayılan bağlantı noktalarını gösteren bir bölme açılır:
    - 22 - SSH
    - 3389 - RDP
    - 5985 - Winrm 
    - 5986 - Winrm
1. İsteğe bağlı olarak, listeye özel bağlantı noktaları ekleyebilirsiniz:

      1. **Ekle**’ye tıklayın. **Ekle bağlantı noktası yapılandırma** penceresi açılır.
      1. Yapılandırmayı seçtiğiniz her bağlantı noktası için, hem varsayılan hem de özel, aşağıdaki ayarları özelleştirebilirsiniz:
            - **Protokol türü**- İstek onaylandığında bu bağlantı noktasında izin verilen protokol.
            - **İzin verilen kaynak IP adresleri**- İstek onaylandığında bu bağlantı noktasında izin verilen IP aralıkları.
            - **Maksimum istek süresi**- Belirli bir bağlantı noktasının açılabildiği maksimum zaman penceresi.

     1. **Tamam**'a tıklayın.

1. **Kaydet**'e tıklayın.

> [!NOTE]
>JIT VM Access bir VM için etkinleştirildiğinde, Azure Güvenlik Merkezi ilişkili ağ güvenlik gruplarında seçili bağlantı noktaları ve onunla birlikte Azure Güvenlik Duvarı için "tüm gelen trafiği reddetme" kuralları oluşturur. Seçili bağlantı noktaları için başka kurallar oluşturulmuşsa, varolan kurallar yeni "gelen tüm trafiği reddet" kurallarına göre öncelik alır. Seçili bağlantı noktalarında varolan kurallar yoksa, yeni "gelen tüm trafiği reddet" kuralları Ağ Güvenlik Grupları ve Azure Güvenlik Duvarı'nda en yüksek önceliği alır.


## <a name="request-jit-access-via-security-center"></a>Güvenlik Merkezi üzerinden JIT erişimi talep edin

Güvenlik Merkezi aracılığıyla Bir VM'ye erişim talebinde bulunmak için:

1. **Tam zamanında VM erişimi**altında, **Yapılandırılmış** sekmesini seçin.

1. **Sanal Makine**altında, erişmek istediğiniz VM'leri tıklatın. Bu, VM'nin yanına bir onay işareti koyar.

    - **Bağlantı Ayrıntıları** sütunundaki simge, JIT'nin NSG veya FW'de etkin olup olmadığını gösterir. Her ikide de etkinse, yalnızca Güvenlik Duvarı simgesi görüntülenir.

    - **Bağlantı Ayrıntıları** sütunu VM'yi ve açık bağlantı noktalarını bağlamak için gereken bilgileri sağlar.

      ![Tam zamanında erişim isteme](./media/security-center-just-in-time/request-just-in-time-access.png)

1. **Erişim İste'yi**tıklatın. **İstek erişim** penceresi açılır.

      ![JIT ayrıntıları](./media/security-center-just-in-time/just-in-time-details.png)

1. Her VM için **İstek erişimi**altında, açmak istediğiniz bağlantı noktalarını ve bağlantı noktasının açıldığı kaynak IP adreslerini ve bağlantı noktasının açılacağı zaman penceresini yapılandırın. Yalnızca tam zamanında ilkede yapılandırılan bağlantı noktalarına erişim isteğinde bulunmak mümkündür. Her bağlantı noktası, tam zamanında ilkeden türetilen en fazla izin verilen süreye sahiptir.

1. **Bağlantı noktalarını aç'ı**tıklatın.

> [!NOTE]
> Erişim isteyen bir kullanıcı bir proxy'nin arkasındaysa, **IP seçeneği çalışmayabilir.** Kuruluşun tam IP adresi aralığını tanımlamanız gerekebilir.



## <a name="edit-a-jit-access-policy-via-security-center"></a>Güvenlik Merkezi aracılığıyla JIT erişim ilkesini edin

VM'nin mevcut tam zamanında ilkesini, o VM için korumak için yeni bir bağlantı noktası ekleyerek ve yapılandırarak veya zaten korunan bir bağlantı noktasıyla ilgili diğer ayarları değiştirerek değiştirebilirsiniz.

VM'nin varolan tam zamanında ilkesini sağlamak için:

1. **Yapılandırılmış** sekmesinde, **VM'lerin**altında, bu VM için satır içindeki üç noktayı tıklatarak bağlantı noktası ekleyeceğiniz bir VM seçin. 

1. **Edit'i**seçin.

1. **JIT VM erişim yapılandırması**altında, zaten korunmuş bir bağlantı noktasının varolan ayarlarını ayarlayabilir veya yeni bir özel bağlantı noktası ekleyebilirsiniz. 
  ![jit vm erişim](./media/security-center-just-in-time/edit-policy.png)



## <a name="audit-jit-access-activity-in-security-center"></a>Güvenlik Merkezi'nde Denetim JIT erişim etkinliği

Günlük aramanızı kullanarak VM etkinlikleri hakkında bilgi edinebilirsiniz. Günlükleri görüntülemek için:

1. **Tam zamanında VM erişimi**altında, **Yapılandırılmış** sekmesini seçin.
2. **VM'ler**altında, bu VM için satırdaki üç noktayı tıklatarak hakkında bilgi görüntülemek için bir VM seçin ve menüden **Etkinlik Günlüğü'nü** seçin. **Etkinlik günlüğü** açılır.

   ![Etkinlik günlüğünü seçin](./media/security-center-just-in-time/select-activity-log.png)

   **Etkinlik günlüğü,** saat, tarih ve abonelikle birlikte o VM için önceki işlemlerin filtrelenmiş bir görünümünü sağlar.

**Tüm öğeleri CSV olarak indirmek için buraya tıklayın'ı**seçerek günlük bilgilerini indirebilirsiniz.

Bir arama ve günlük oluşturmak için filtreleri değiştirin ve **Uygula'yı** tıklatın.



## <a name="configure-jit-access-from-an-azure-vms-page"></a>Azure VM'nin sayfasından JIT erişimini yapılandırma<a name="jit-vm"></a>

Size kolaylık sağlamak için, Doğrudan Güvenlik Merkezi'ndeki VM sayfası nın içinden JIT'yi kullanarak bir VM'ye bağlanabilirsiniz.

### <a name="configure-jit-access-on-a-vm-via-the-azure-vm-page"></a>Azure VM sayfası üzerinden VM'de JIT erişimini yapılandırın

VM'lerinizde tam zamanında erişimi kullanıma sunmayı kolaylaştırmak için, doğrudan VM'den yalnızca tam zamanında erişime izin verecek bir VM ayarlayabilirsiniz.

1. Azure [portalından](https://ms.portal.azure.com) **Sanal makineleri**arayın ve seçin. 
2. Tam zamanında erişimle sınırlamak istediğiniz sanal makineyi seçin.
3. Menüde **Yapılandırma'yı**seçin.
4. **Tam zamanında erişim**altında, **tam zamanında etkinleştir'i**seçin. 

Bu, aşağıdaki ayarları kullanarak VM için tam zamanında erişim sağlar:

- Windows sunucuları:
    - RDP bağlantı noktası 3389
    - İzin verilen maksimum erişim üç saat
    - İzin verilen kaynak IP adresleri Herhangi bir
- Linux sunucuları:
    - SSH bağlantı noktası 22
    - İzin verilen maksimum erişim üç saat
    - İzin verilen kaynak IP adresleri Herhangi bir
     
Bir VM zaten tam zamanında etkinleştirildiyse, yapılandırma sayfasına gittiğinizde tam zamanında etkinleştirilmeye ve ayarları görüntülemek ve değiştirmek için Azure Güvenlik Merkezi'ndeki ilkeyi açmak için bağlantıyı kullanabilirsiniz.

![vm içinde jit config](./media/security-center-just-in-time/jit-vm-config.png)

### <a name="request-jit-access-to-a-vm-via-an-azure-vms-page"></a>Azure VM'nin sayfası üzerinden VM'ye JIT erişimi isteğinde

Azure portalında, bir VM'ye bağlanmaya çalıştığınızda, Azure, bu VM'de tam zamanında bir erişim ilkesinin yapılandırıp yapılmamadığını denetler.

- VM'de yapılandırılan bir JIT ilkeniz varsa, VM için ayarlanan JIT ilkesine uygun olarak hibe **erişimi için İsteğ'i** tıklatabilirsiniz. 

  >![jit isteği](./media/security-center-just-in-time/jit-request.png)

  Erişim aşağıdaki varsayılan parametrelerle istenir:

  - **kaynak IP**: 'Any' (*) (değiştirilemez)
  - **zaman aralığı**: Üç saat (değiştirilemez) <!--Isn't this set in the policy-->
  - **bağlantı noktası numarası** Windows için RDP portu 3389 / Linux için port 22 (değiştirilebilir)

    > [!NOTE]
    > Güvenlik Merkezi, Azure Güvenlik Duvarı tarafından korunan bir VM için istek onaylandıktan sonra, kullanıcıya VM'ye bağlanmak için kullanmak üzere uygun bağlantı ayrıntılarını (DNAT tablosundaki bağlantı noktası eşlemesi) sağlar.

- Bir VM üzerinde JIT yapılandırılmamışsa, üzerinde bir JIT ilkesi yapılandırmanız istenir.

  ![jit istemi](./media/security-center-just-in-time/jit-prompt.png)

## <a name="configure-a-jit-policy-on-a-vm-programmatically"></a>VM üzerinde jit ilkesini programlı olarak yapılandırma<a name="jit-program"></a>

REST API'leri ve PowerShell üzerinden tam zamanında ayarlayabilir ve kullanabilirsiniz.

### <a name="jit-vm-access-via-rest-apis"></a>REST API'leri ile JIT VM erişimi

Tam zamanında VM erişim özelliği Azure Güvenlik Merkezi API'si aracılığıyla kullanılabilir. Bu API aracılığıyla yapılandırılmış VM'ler hakkında bilgi alabilir, yenilerini ekleyebilir, VM'ye erişim isteğinde bulunabilir ve daha fazlasını yapabilirsiniz. Tam zamanında REST API hakkında daha fazla bilgi edinmek için [Jit Network Access İlkeleri'ne](https://docs.microsoft.com/rest/api/securitycenter/jitnetworkaccesspolicies)bakın.

### <a name="jit-vm-access-via-powershell"></a>PowerShell üzerinden JIT VM erişimi

PowerShell üzerinden tam zamanında VM erişim çözümlerini kullanmak için, resmi Azure Güvenlik Merkezi `Set-AzJitNetworkAccessPolicy`PowerShell cmdlets'i ve özellikle .

Aşağıdaki örnek, belirli bir VM'de tam zamanında VM erişim ilkesini belirler ve aşağıdakileri ayarlar:

1.  22 ve 3389 bağlantı noktalarını kapatın.

2.  Onaylanan istek başına açIlebilmeleri için her biri için en fazla 3 saatlik bir zaman penceresi ayarlayın.
3.  Kaynak IP adreslerini denetlemek için erişim isteyen kullanıcıya izin verir ve onaylanan tam zamanında erişim isteği üzerine kullanıcının başarılı bir oturum oluşturmasına olanak tanır.

Bunu başarmak için PowerShell'de aşağıdakileri çalıştırın:

1.  Bir VM için tam zamanında VM erişim ilkesini tutan bir değişken atayın:

        $JitPolicy = (@{
         id="/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Compute/virtualMachines/VMNAME"
        ports=(@{
             number=22;
             protocol="*";
             allowedSourceAddressPrefix=@("*");
             maxRequestAccessDuration="PT3H"},
             @{
             number=3389;
             protocol="*";
             allowedSourceAddressPrefix=@("*");
             maxRequestAccessDuration="PT3H"})})

2.  VM'yi tam zamanında VM erişim ilkesini bir diziye takın:
    
        $JitPolicyArr=@($JitPolicy)

3.  Seçili VM'de tam zamanında VM erişim ilkesini yapılandırın:
    
        Set-AzJitNetworkAccessPolicy -Kind "Basic" -Location "LOCATION" -Name "default" -ResourceGroupName "RESOURCEGROUP" -VirtualMachine $JitPolicyArr 

### <a name="request-access-to-a-vm-via-powershell"></a>PowerShell üzerinden VM'ye erişim isteğinde bulunun

Aşağıdaki örnekte, belirli bir VM bağlantı noktası 22'nin belirli bir IP adresi ve belirli bir süre için açılmasının istendiği belirli bir VM'ye tam zamanında VM erişim isteği görebilirsiniz:

PowerShell'de aşağıdakileri çalıştırın:
1.  VM istek erişim özelliklerini yapılandırma

        $JitPolicyVm1 = (@{
          id="/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Compute/virtualMachines/VMNAME"
        ports=(@{
           number=22;
           endTimeUtc="2018-09-17T17:00:00.3658798Z";
           allowedSourceAddressPrefix=@("IPV4ADDRESS")})})
2.  VM erişim isteği parametrelerini bir diziye ekleyin:

        $JitPolicyArr=@($JitPolicyVm1)
3.  İstek erişimini gönderin (adım 1'de aldığınız kaynak kimliğini kullanın)

        Start-AzJitNetworkAccessPolicy -ResourceId "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Security/locations/LOCATION/jitNetworkAccessPolicies/default" -VirtualMachine $JitPolicyArr

Daha fazla bilgi için [PowerShell cmdlet belgelerine](https://docs.microsoft.com/powershell/scripting/developer/cmdlet/cmdlet-overview)bakın.


## <a name="automatic-cleanup-of-redundant-jit-rules"></a>Gereksiz JIT kurallarının otomatik olarak temizlenmesi 

Bir JIT ilkesini güncelleştirdiğinizde, tüm kural kümenizin geçerliliğini denetlemek için bir temizleme aracı otomatik olarak çalışır. Araç, politikanızdaki kurallarla NSG'deki kurallar arasındaki uyuşmazlıkları arar. Temizleme aracı bir uyumsuzluk bulursa, nedenini belirler ve güvenli olduğunda artık gerekmeyen yerleşik kuralları kaldırır. Temizleyici, oluşturduğunuz kuralları asla silmez.

Temizleyicinin yerleşik bir kuralı kaldırabileceği senaryolara örnek:

- Özdeş tanımlara sahip iki kural varsa ve biri diğerinden daha yüksek önceliğe sahipse (yani, alt öncelik kuralı asla kullanılmaz)
- Kural açıklaması, kuraldaki hedef IP ile eşleşmeyen bir VM'nin adını içeriyorsa 


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Güvenlik Merkezi'ndeki tam zamanında VM erişiminin Azure sanal makinelerinize erişimi denetlemenize nasıl yardımcı olduğunu öğrendiniz.

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

- [Güvenlik ilkeleri belirleme](tutorial-security-policy.md) — Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini nasıl yapılandırıştıracaklarını öğrenin.
- [Güvenlik önerilerini yönetme](security-center-recommendations.md) — Önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
- [Güvenlik durumu izleme](security-center-monitoring.md) — Azure kaynaklarınızın sistem durumunu nasıl izleyeceğinizi öğrenin.