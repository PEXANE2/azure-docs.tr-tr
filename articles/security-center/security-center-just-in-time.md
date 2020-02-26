---
title: Azure Güvenlik Merkezi 'nde tam zamanında sanal makine erişimi | Microsoft Docs
description: Bu belge, Azure Güvenlik Merkezi 'ndeki tam zamanında VM erişiminin Azure sanal makinelerinize erişimi denetlemenize nasıl yardımcı olduğunu gösterir.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: memildin
ms.openlocfilehash: 4b2b388fb736997010a6cbbdf93b23b77c7ef3a3
ms.sourcegitcommit: 0cc25b792ad6ec7a056ac3470f377edad804997a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77603976"
---
# <a name="secure-your-management-ports-with-just-in-time-access"></a>Tam zamanında erişimli yönetim bağlantı noktalarınızı güvenli hale getirin

Güvenlik Merkezi 'nin standart fiyatlandırma katmanındaysanız (bkz. [fiyatlandırma](/azure/security-center/security-center-pricing)), tam ZAMANıNDA (JIT) sanal makıne (VM) erişimi Ile Azure VM 'lerinize gelen trafiği kilitleyebilir. Bu, gerektiğinde VM 'lere bağlanmak için kolay erişim sağlarken saldırılardan etkilenme olasılığını azaltır.

> [!NOTE]
> Güvenlik Merkezi tam zamanında VM erişimi şu anda yalnızca Azure Resource Manager aracılığıyla dağıtılan VM 'Leri desteklemektedir. Klasik ve Kaynak Yöneticisi dağıtım modelleri hakkında daha fazla bilgi için bkz. [Azure Resource Manager ve klasik dağıtım karşılaştırması](../azure-resource-manager/management/deployment-models.md).

[!INCLUDE [security-center-jit-description](../../includes/security-center-jit-description.md)]

## <a name="configure-jit-on-a-vm"></a>VM üzerinde JıT yapılandırma

Bir sanal makinede JıT ilkesi yapılandırmanın üç yolu vardır:

- [Azure Güvenlik Merkezi 'nde JıT erişimini yapılandırma](#jit-asc)
- [Azure VM sayfasında JıT erişimini yapılandırma](#jit-vm)
- [VM 'de bir JıT ilkesini program aracılığıyla yapılandırma](#jit-program)

## <a name="configure-jit-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde JıT yapılandırma

Güvenlik Merkezi 'nden bir JıT ilkesi yapılandırabilir ve bir sanal makineye bir JıT ilkesi kullanarak erişim isteğinde olabilirsiniz

### Güvenlik Merkezi 'nde bir sanal makinede JıT erişimini yapılandırma<a name="jit-asc"></a>

1. **Güvenlik Merkezi** panosunu açın.

1. Sol bölmede, **tam ZAMANıNDA VM erişimi**' ni seçin.

    ![Tam zamanında VM erişimi kutucuğu](./media/security-center-just-in-time/just-in-time.png)

    **Tam ZAMANıNDA VM erişimi** penceresi açılır ve VM 'lerinizin durumu hakkında bilgi gösterir:

    - **Yapılandırıldı** -tam zamanında VM erişimini destekleyecek şekilde yapılandırılmış VM 'ler. Sunulan veriler geçen hafta içindir ve her VM için onaylanan istek sayısı, son erişim tarihi ve saati ve son kullanıcı için içerir.
    - **Tam ZAMANıNDA** VM erişimini destekleyebilen, ancak için yapılandırılmamış VM 'ler. Bu VM 'Ler için tam zamanında VM erişimi denetimini etkinleştirmenizi öneririz.
    - **Öneri olmayan** - Bir VM’nin önerilmemesinin olası nedenleri şunlardır:
      - Eksik NSG-tam zamanında çözüm, bir NSG 'nin yerinde olmasını gerektirir.
      - Klasik VM-Güvenlik Merkezi tam zamanında VM erişimi şu anda yalnızca Azure Resource Manager aracılığıyla dağıtılan VM 'Leri desteklemektedir. Klasik bir dağıtım, tam zamanında çözüm tarafından desteklenmez. 
      - Diğer-bir sanal makine, aboneliğin veya kaynak grubunun güvenlik ilkesinde tam zamanında çözüm kapatılmışsa veya VM 'nin genel IP 'si eksikse ve bir NSG yoksa, bu kategoride olur.

1. **Önerilen** sekmeyi seçin.

1. **Sanal makine**altında, etkinleştirmek Istediğiniz VM 'lere tıklayın. Bu, bir VM 'nin yanına onay işareti koyar.

      ![Tam zamanında erişimi etkinleştir](./media/security-center-just-in-time/enable-just-in-time.png)

1. **VM 'LERDE JIT 'ı etkinleştir**' e tıklayın. Azure Güvenlik Merkezi tarafından önerilen varsayılan bağlantı noktalarını görüntüleyen bir bölme açılır:
    - 22-SSH
    - 3389-RDP
    - 5985-WinRM 
    - 5986-WinRM
1. İsteğe bağlı olarak, listeye özel bağlantı noktaları ekleyebilirsiniz:

      1. **Ekle**'ye tıklayın. **Bağlantı noktası yapılandırması Ekle** penceresi açılır.
      1. Yapılandırmak üzere seçtiğiniz her bağlantı noktası için, hem varsayılan hem de özel, aşağıdaki ayarları özelleştirebilirsiniz:
            - **Protokol türü**-bir istek onaylandığında bu bağlantı noktasında izin verilen protokol.
            - **Izin verilen kaynak IP adresleri**-bir istek onaylandığında bu bağlantı noktasında ızın verilen IP aralıkları.
            - **En fazla istek süresi**-belirli bir bağlantı noktasının açılabileceği en uzun zaman penceresi.

     1. **OK (Tamam)** düğmesine tıklayın.

1. **Kaydet**’e tıklayın.

> [!NOTE]
>Azure Güvenlik Merkezi, bir VM için JıT VM erişimi etkinleştirildiğinde, ilişkili ağ güvenlik gruplarında ve Azure Güvenlik Duvarı 'nda bulunan seçili bağlantı noktaları için "tüm gelen trafiği reddet" kurallarını oluşturur. Seçilen bağlantı noktaları için başka kurallar oluşturulduysa, mevcut kurallar yeni "tüm gelen trafiği reddetme" kuralları üzerinden önceliklidir. Seçilen bağlantı noktalarında mevcut kurallar yoksa, yeni "tüm gelen trafiği reddetme" kuralları, ağ güvenlik gruplarında ve Azure Güvenlik duvarında en üst önceliğe sahip olacaktır.


## <a name="request-jit-access-via-security-center"></a>Güvenlik Merkezi aracılığıyla JıT erişimi iste

Bir sanal makineye Güvenlik Merkezi aracılığıyla erişim istemek için:

1. **Tam ZAMANıNDA VM erişimi**altında, **yapılandırılan** sekmesini seçin.

1. **Sanal makine**altında, erişim Istemek Istediğiniz VM 'lere tıklayın. Bu, VM 'nin yanına bir onay işareti koyar.

    - **Bağlantı ayrıntıları** sütunundaki SIMGE, NSG veya FW üzerinde JIT özelliğinin etkin olup olmadığını gösterir. Her ikisinde de etkinse yalnızca güvenlik duvarı simgesi görüntülenir.

    - **Bağlantı ayrıntıları** sütunu, VM 'yi bağlamak için gereken bilgileri ve açık bağlantı noktalarını sağlar.

      ![Tam zamanında erişim isteme](./media/security-center-just-in-time/request-just-in-time-access.png)

1. **Erişim iste**' ye tıklayın. **Erişim iste** penceresi açılır.

      ![JıT ayrıntıları](./media/security-center-just-in-time/just-in-time-details.png)

1. **Erişim iste**, her VM için, açmak istediğiniz bağlantı noktalarını ve bağlantı noktasının açık olduğu kaynak IP adreslerini ve bağlantı noktasının açılacağı zaman penceresini yapılandırın. Yalnızca tam zamanında ilkesinde yapılandırılmış bağlantı noktalarına erişim isteğinde bulunur. Her bağlantı noktası, tam zamanında ilkeden elde edilen izin verilen en fazla süreyi içerir.

1. **Açık bağlantı noktaları**' na tıklayın.

> [!NOTE]
> Erişim isteyen bir Kullanıcı proxy 'nin arkasındaysa, **alanım** seçeneği çalışmayabilir. Kuruluşun tam IP adres aralığını tanımlamanız gerekebilir.



## <a name="edit-a-jit-access-policy-via-security-center"></a>Güvenlik Merkezi ile bir JıT erişim ilkesini düzenleme

Bu VM 'ye karşı koruma için yeni bir bağlantı noktası ekleyip yapılandırarak veya zaten korumalı bir bağlantı noktasıyla ilgili diğer ayarları değiştirerek bir sanal makinenin mevcut tam zamanında ilkesini değiştirebilirsiniz.

Bir VM 'nin mevcut bir tam zamanında ilkesini düzenlemek için:

1. **Yapılandırılan** sekmede, VM **'ler**altında, söz konusu VM 'nin satır içindeki üç noktaya tıklayarak BIR bağlantı noktasının ekleneceği VM 'yi seçin. 

1. **Düzenle**’yi seçin.

1. **JıT VM erişimi yapılandırması**altında, zaten korumalı bir bağlantı noktasının var olan ayarlarını düzenleyebilir ya da yeni bir özel bağlantı noktası ekleyebilirsiniz. 
  ![JIT VM erişimi](./media/security-center-just-in-time/edit-policy.png)



## <a name="audit-jit-access-activity-in-security-center"></a>Güvenlik Merkezi 'nde JıT erişim etkinliğini denetleme

Günlük aramasını kullanarak VM etkinlikleri hakkında öngörüler elde edebilirsiniz. Günlükleri görüntülemek için:

1. **Tam ZAMANıNDA VM erişimi**altında, **yapılandırılan** sekmesini seçin.
2. VM **'ler**altında, söz konusu VM 'nin satır içindeki üç noktaya tıklayarak ve menüden **etkinlik günlüğü** ' nü seçerek hakkında BILGI görüntülemek için bir VM seçin. **Etkinlik günlüğü** açılır.

   ![Etkinlik günlüğü seçin](./media/security-center-just-in-time/select-activity-log.png)

   **Etkinlik günlüğü** , o VM için zaman, tarih ve abonelikle birlikte önceki işlemlerin filtrelenmiş bir görünümünü sağlar.

**Tüm ÖĞELERI CSV olarak indirmek için buraya tıklayın ' ı**seçerek günlük bilgilerini indirebilirsiniz.

Filtreleri değiştirin ve bir arama ve günlük oluşturmak için **Uygula** ' ya tıklayın.



## Azure VM 'nin sayfasından JıT erişimini yapılandırma<a name="jit-vm"></a>

Kolaylık olması için, güvenlik merkezi 'nde VM 'nin içinden JıT kullanarak doğrudan bir VM 'ye bağlanabilirsiniz.

### <a name="configure-jit-access-on-a-vm-via-the-azure-vm-page"></a>Azure VM sayfası aracılığıyla bir VM 'de JıT erişimini yapılandırma

Sanal makinelerinizden tam zamanında erişimi daha kolay hale getirmek için bir VM 'yi doğrudan VM içinden yalnızca tam zamanında erişime izin verecek şekilde ayarlayabilirsiniz.

1. [Azure Portal](https://ms.portal.azure.com), **sanal makineleri**arayıp seçin. 
2. Tam zamanında erişim ile sınırlamak istediğiniz sanal makineyi seçin.
3. Menüde **yapılandırma**' yı seçin.
4. **Tam zamanında erişim**altında, **tam zamanında etkinleştir**' i seçin. 

Bu, aşağıdaki ayarları kullanarak VM için tam zamanında erişim imkanı sunar:

- Windows Server 'lar:
    - RDP bağlantı noktası 3389
    - İzin verilen en fazla üç saat
    - İzin verilen kaynak IP adresleri any olarak ayarlandı
- Linux sunucuları:
    - SSH bağlantı noktası 22
    - İzin verilen en fazla üç saat
    - İzin verilen kaynak IP adresleri any olarak ayarlandı
     
Bir VM 'nin zaten etkin bir şekilde etkinleştirilmesi durumunda, yapılandırma sayfasına gittiğinizde, tam zamanında etkin olduğunu görebilir ve bu bağlantıyı, ayarları görüntülemek ve değiştirmek için Azure Güvenlik Merkezi 'ndeki ilkeyi açmak için kullanabilirsiniz.

![VM 'de JIT yapılandırması](./media/security-center-just-in-time/jit-vm-config.png)

### <a name="request-jit-access-to-a-vm-via-an-azure-vms-page"></a>Azure VM 'nin sayfası aracılığıyla bir VM 'ye JıT erişimi isteme

Azure portal, bir VM 'ye bağlanmaya çalıştığınızda, Azure bu VM 'de tam zamanında erişim ilkesi yapılandırılıp yapılandırılmadığını denetler.

- VM üzerinde yapılandırılmış bir JıT ilkeniz varsa, sanal makine için JıT ilke kümesine uygun olarak erişim izni vermek üzere **erişim iste** ' ye tıklayabilirsiniz. 

  >![JIT isteği](./media/security-center-just-in-time/jit-request.png)

  Erişim aşağıdaki varsayılan parametrelerle istenir:

  - **kaynak IP**: ' any ' (*) (değiştirilemez)
  - **zaman aralığı**: üç saat (değiştirilemez) <!--Isn't this set in the policy-->
  - **bağlantı noktası numarası** Linux için Windows/Port 22 için RDP bağlantı noktası 3389 (değiştirilebilir)

    > [!NOTE]
    > Azure Güvenlik Duvarı tarafından korunan bir VM için bir istek onaylandıktan sonra, güvenlik merkezi kullanıcıya, sanal makineye bağlanmak için kullanmak üzere uygun bağlantı ayrıntılarını (DNAT tablosundan bağlantı noktası eşlemesi) sağlar.

- Bir VM üzerinde JıT yapılandırılmamışsa, bunun üzerinde bir JıT İlkesi yapılandırmanız istenir.

  ![JIT istemi](./media/security-center-just-in-time/jit-prompt.png)

## VM 'de bir JıT ilkesini program aracılığıyla yapılandırma<a name="jit-program"></a>

REST API 'Leri aracılığıyla ve PowerShell aracılığıyla tam zamanında kurulum ve kullanım yapabilirsiniz.

### <a name="jit-vm-access-via-rest-apis"></a>REST API 'Leri aracılığıyla JıT VM erişimi

Tam zamanında VM erişimi özelliği Azure Güvenlik Merkezi API 'SI aracılığıyla kullanılabilir. Yapılandırılan VM 'Ler hakkında bilgi alabilir, yenilerini ekleyebilir, bir VM 'ye erişim isteğinde bulunabilir ve daha fazlasını bu API aracılığıyla yapabilirsiniz. Tam zamanında REST API hakkında daha fazla bilgi edinmek için bkz. [JIT ağ erişim ilkeleri](https://docs.microsoft.com/rest/api/securitycenter/jitnetworkaccesspolicies).

### <a name="jit-vm-access-via-powershell"></a>PowerShell aracılığıyla JıT VM erişimi

PowerShell aracılığıyla tam zamanında VM erişimi çözümünü kullanmak için resmi Azure Güvenlik Merkezi PowerShell cmdlet 'lerini kullanın ve özellikle `Set-AzJitNetworkAccessPolicy`.

Aşağıdaki örnek, belirli bir sanal makinede tam zamanında VM erişim ilkesi ayarlıyor ve aşağıdakileri ayarlıyor:

1.  22 ve 3389 bağlantı noktalarını kapatın.

2.  Her biri için en fazla 3 saat, her bir onaylanan istek için açılabilecekleri bir zaman penceresi ayarlayın.
3.  Kaynak IP adreslerini denetlemek için erişim isteyen kullanıcının, bir tam zamanında erişim isteği üzerinde başarılı bir oturum kurmasını sağlar.

Bunu gerçekleştirmek için PowerShell 'de aşağıdakileri çalıştırın:

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

2.  VM 'ye tam zamanında VM erişim ilkesini bir diziye ekleyin:
    
        $JitPolicyArr=@($JitPolicy)

3.  Seçilen VM 'de tam zamanında VM erişim ilkesini yapılandırın:
    
        Set-AzJitNetworkAccessPolicy -Kind "Basic" -Location "LOCATION" -Name "default" -ResourceGroupName "RESOURCEGROUP" -VirtualMachine $JitPolicyArr 

### <a name="request-access-to-a-vm-via-powershell"></a>PowerShell aracılığıyla bir VM 'ye erişim isteme

Aşağıdaki örnekte, belirli bir IP adresi ve belirli bir süre için, bağlantı noktası 22 ' nin açılabileceği belirli bir sanal makineye tam zamanında VM erişimi isteği görebilirsiniz:

PowerShell 'de aşağıdakileri çalıştırın:
1.  VM isteği erişim özelliklerini yapılandırma

        $JitPolicyVm1 = (@{
          id="/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Compute/virtualMachines/VMNAME"
        ports=(@{
           number=22;
           endTimeUtc="2018-09-17T17:00:00.3658798Z";
           allowedSourceAddressPrefix=@("IPV4ADDRESS")})})
2.  VM erişim isteği parametrelerini bir diziye ekleyin:

        $JitPolicyArr=@($JitPolicyVm1)
3.  İstek erişimini gönderin (1. adımda aldığınız kaynak KIMLIĞINI kullanın)

        Start-AzJitNetworkAccessPolicy -ResourceId "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Security/locations/LOCATION/jitNetworkAccessPolicies/default" -VirtualMachine $JitPolicyArr

Daha fazla bilgi için bkz. [PowerShell cmdlet belgeleri](https://docs.microsoft.com/powershell/scripting/developer/cmdlet/cmdlet-overview).


## <a name="automatic-cleanup-of-redundant-jit-rules"></a>Yedekli JıT kuralları otomatik temizleme 

Bir JıT ilkesini her güncelleştirdiğinizde, tüm RuleSet 'nizin geçerliliğini denetlemek için otomatik olarak bir temizleme aracı çalışır. Araç, ilkenizin kuralları ile NSG 'deki kurallar arasında uyuşmazlıkları arar. Temizleme Aracı bir uyumsuzluk bulursa, nedeni belirler ve bunu yapmak güvenli olduğunda, gerek olmayan yerleşik kuralları kaldırır. Temizleyici, oluşturduğunuz kuralları hiçbir şekilde silmez.

Temizleyici bir yerleşik kuralı kaldırabilen senaryolar örnekleri:

- Aynı tanımlara sahip iki kural varsa ve biri diğerine kıyasla daha yüksek bir önceliğe sahip olduğunda (yani, düşük öncelikli kural hiçbir zaman kullanılmaz)
- Bir kural açıklaması, kuraldaki hedef IP ile eşleşmeyen bir VM 'nin adını içerdiğinde 


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, güvenlik merkezi 'nde tam zamanında VM erişiminin Azure sanal makinelerinize erişimi denetlemenize nasıl yardımcı olduğunu öğrendiniz.

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

- [Güvenlik Ilkelerini ayarlama](tutorial-security-policy.md) -Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
- [Güvenlik önerilerini yönetme](security-center-recommendations.md) — önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
- [Güvenlik sistem durumu izleme](security-center-monitoring.md) — Azure kaynaklarınızın sistem durumunu izlemeyi öğrenin.