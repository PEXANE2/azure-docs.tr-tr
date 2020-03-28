---
title: PowerShell - Azure Özel HSM | Microsoft Dokümanlar
description: PowerShell kullanarak özel bir HSM'nin mevcut bir sanal ağa nasıl dağıtılabildiğini gösteren öğretici
services: dedicated-hsm
documentationcenter: na
author: msmbaldwin
manager: rkarlin
editor: ''
ms.service: key-vault
ms.topic: tutorial
ms.custom: mvc, seodec18
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2019
ms.author: mbaldwin
ms.openlocfilehash: c1a847a315a264591c0d003ff691d9938c2bf0f5
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79474433"
---
# <a name="tutorial--deploying-hsms-into-an-existing-virtual-network-using-powershell"></a>Öğretici – PowerShell kullanarak HSM'leri mevcut bir sanal ağa dağıtma

Azure Özel HSM Hizmeti, tüm müşteri kullanımı için tam yönetim kontrolü ve tam yönetim sorumluluğuyla fiziksel bir aygıt sağlar. Fiziksel donanım sağlaması nedeniyle Microsoft, kapasitenin etkin bir şekilde yönetilmesini sağlamak için bu aygıtların nasıl tahsis edildiğini denetlemelidir. Sonuç olarak, bir Azure aboneliği içinde, Özel HSM hizmeti normalde kaynak sağlama için görünmez. Özel HSM hizmetine erişim gerektiren herhangi bir Azure müşterisi, Özel HSM hizmeti için kayıt istemek için öncelikle Microsoft hesap yöneticilerine başvurmalıdır. Ancak bu işlem başarıyla tamamlandığında sağlama mümkün olacaktır.
Bu öğretici, tipik bir sağlama işlemini göstermeyi amaçlamaktadır:

- Bir müşterinin zaten sanal bir ağı vardır
- Sanal bir makineleri var.
- HSM kaynaklarını bu varolan ortama eklemeleri gerekir.

Tipik, yüksek kullanılabilirlik, çok bölgeli dağıtım mimarisi aşağıdaki gibi görünebilir:

![çok bölgeli dağıtım](media/tutorial-deploy-hsm-powershell/high-availability.png)

Bu öğretici, bir çift HSM'ye ve gerekli ExpressRoute Ağ Geçidi'nin (yukarıdaki Subnet 1'e bakınız) varolan bir sanal ağa entegre edilmesine odaklanır (yukarıdaki VNET 1'e bakın).  Diğer tüm kaynaklar standart Azure kaynaklarıdır. Yukarıdaki VNET 3'te 4 alt netindeki HSM'ler için de aynı tümleştirme işlemi kullanılabilir.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Azure Özel HSM şu anda Azure portalında mevcut değildir, bu nedenle hizmetle tüm etkileşim komut satırı veya PowerShell kullanılarak olacaktır. Bu öğretici, Azure Bulut Shell'de PowerShell'i kullanır. PowerShell'de yeniyseniz, başlangıç yönergelerini buradan takip edin: [Azure PowerShell Başlayın.](https://docs.microsoft.com/powershell/azure/get-started-azureps)

Varsayımlar:

- Azure Özel HSM kayıt işleminden geçtiniz ve hizmetin kullanımı için onaylandınız. Değilse, ayrıntılar için Microsoft hesap temsilcinize başvurun. 
- Bu kaynaklar için bir Kaynak Grubu oluşturdunuz ve bu öğreticide dağıtılan yeni ler bu gruba katılacak.
- Yukarıdaki diyagrama göre gerekli sanal ağı, alt ağı ve sanal makineleri zaten oluşturdunuz ve şimdi bu dağıtıma 2 HSM entegre etmek istiyorsunuz.

Aşağıdaki tüm talimatlar Azure portalına çoktan yönlendirdiğinizi ve Bulut Kabuğu'nu\>\_açtığınızı varsayar (portalın sağ üst ağına doğru " " seçeneğini belirleyin).

## <a name="provisioning-a-dedicated-hsm"></a>Özel HSM Sağlama

HSM'lerin sağlanması ve ExpressRoute Ağ Geçidi üzerinden mevcut bir sanal ağa entegre edilmesi, hsm aygıtının daha fazla yapılandırma faaliyetleri için ulaşılabilirliğini ve temel kullanılabilirliğini sağlamak için ssh komut satırı aracı kullanılarak doğrulanacaktır. Aşağıdaki komutlar, HSM kaynaklarını ve ilişkili ağ kaynaklarını oluşturmak için bir Kaynak Yöneticisi şablonu kullanır.

### <a name="validating-feature-registration"></a>Özellik Kaydını Doğrulama

Yukarıda belirtildiği gibi, herhangi bir sağlama faaliyeti, Özel HSM hizmetinin aboneliğiniz için kayıtlı olduğunu gerektirir. Bunu doğrulamak için Azure portal bulutu kabuğunda aşağıdaki PowerShell komutunu çalıştırın. 

```powershell
Get-AzProviderFeature -ProviderNamespace Microsoft.HardwareSecurityModules -FeatureName AzureDedicatedHsm
```

Aşağıdaki komut, Özel HSM hizmeti için gereken ağ özelliklerini doğrular.

```powershell
Get-AzProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowBaremetalServers
```

Daha fazla ilerlemeden önce her iki komutda da "Kayıtlı" (aşağıda gösterildiği gibi) durumu döndürülmelidir.  Bu hizmete kaydolmanız gerekiyorsa, Microsoft hesap temsilcinize başvurun.

![abonelik durumu](media/tutorial-deploy-hsm-powershell/subscription-status.png)

### <a name="creating-hsm-resources"></a>HSM kaynakları oluşturma

Bir HSM aygıtı müşterilerin sanal ağına satılır. Bu, bir alt ağ gereksinimi anlamına gelir. Sanal ağ ve fiziksel aygıt arasındaki iletişimi etkinleştirmek için HSM için bir bağımlılık bir ExpressRoute Ağ Geçidi ve son olarak sanal bir makine Gemalto istemci yazılımı kullanarak HSM aygıtına erişmek için gereklidir. Bu kaynaklar, kullanım kolaylığı için ilgili parametre dosyasıyla birlikte bir şablon dosyasında toplanmıştır. Dosyalar, doğrudan Microsoft'a başvurarak HSMrequest@Microsoft.comkullanılabilir.

Dosyaları aldıktan sonra, kaynaklar için tercih ettiğiniz adları eklemek için parametre dosyasını doldurmanız gerekir. Bu, "değer" ile satırları düzenleme anlamına gelir: "".

- `namingInfix`HSM kaynaklarının adları için önek
- `ExistingVirtualNetworkName`HSM'ler için kullanılan sanal ağın adı
- `DedicatedHsmResourceName1`Veri merkezi damgasındaki HSM kaynağının adı 1
- `DedicatedHsmResourceName2`Datacenter damgasındaki HSM kaynağının adı 2
- `hsmSubnetRange`HSM'ler için Subnet IP Adresi aralığı
- `ERSubnetRange`VNET ağ geçidi için subnet IP Adres aralığı

Bu değişikliklere bir örnek aşağıdaki gibidir:

```json
{
"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "namingInfix": {
      "value": "MyHSM"
    },
    "ExistingVirtualNetworkName": {
      "value": "MyHSM-vnet"
    },
    "DedicatedHsmResourceName1": {
      "value": "HSM1"
    },
    "DedicatedHsmResourceName2": {
      "value": "HSM2"
    },
    "hsmSubnetRange": {
      "value": "10.0.2.0/24"
    },
    "ERSubnetRange": {
      "value": "10.0.255.0/26"
    },
  }
}
```

İlişkili Kaynak Yöneticisi şablon dosyası, bu bilgileri içeren 6 kaynak oluşturur:

- Belirtilen VNET'teki HSM'ler için bir alt ağ
- Sanal ağ ağ geçidi için bir alt ağ 
- VNET'i HSM aygıtlarına bağlayan sanal ağ ağ geçidi
- Ağ geçidi için genel bir IP adresi
- Damga 1'de bir HSM
- Damga 2'de bir HSM

Parametre değerleri ayarlandıktan sonra, dosyaların kullanılmak üzere Azure portal bulut bulut uyp dosyası paylaşımına yüklenmesi gerekir. Azure portalında , "\>\_" bulut kabuğu simgesinin sağ üst kısmına tıklayın ve bu da ekranın alt kısmını bir komut ortamı yapar. Bunun için seçenekler BASH ve PowerShell ve zaten ayarlanmış değilse BASH seçmelisiniz.

Komut kabuğuaraç çubuğunda bir yükleme/indirme seçeneği vardır ve şablon ve parametre dosyalarını dosya paylaşımınıza yüklemek için bunu seçmelisiniz:

![dosya paylaşımı](media/tutorial-deploy-hsm-powershell/file-share.png)

Dosyalar yüklendikten sonra kaynak oluşturmaya hazırsınız.
Yeni HSM kaynakları oluşturmadan önce, yerinde olduğundan emin olunması gereken bazı ön koşul kaynakları vardır. Bilgi işlem, HSM ve ağ geçidi için alt ağ aralıklarına sahip bir sanal ağınız olmalıdır. Aşağıdaki komutlar, böyle bir sanal ağ oluşturacak ne bir örnek olarak hizmet vermektedir.

```powershell
$compute = New-AzVirtualNetworkSubnetConfig `
  -Name compute `
  -AddressPrefix 10.2.0.0/24
```

```powershell
$delegation = New-AzDelegation `
  -Name "myDelegation" `
  -ServiceName "Microsoft.HardwareSecurityModules/dedicatedHSMs"

```

```powershell
$hsmsubnet = New-AzVirtualNetworkSubnetConfig ` 
  -Name hsmsubnet ` 
  -AddressPrefix 10.2.1.0/24 ` 
  -Delegation $delegation 

```

```powershell

$gwsubnet= New-AzVirtualNetworkSubnetConfig `
  -Name GatewaySubnet `
  -AddressPrefix 10.2.255.0/26

```

```powershell

New-AzVirtualNetwork `
  -Name myHSM-vnet `
  -ResourceGroupName myRG `
  -Location westus `
  -AddressPrefix 10.2.0.0/16 `
  -Subnet $compute, $hsmsubnet, $gwsubnet

```

>[!NOTE]
>Sanal ağ için dikkat edilmesi gereken en önemli yapılandırma, HSM aygıtının alt ağının "Microsoft.HardwareSecurityModules/dedicatedHSMs" olarak ayarlanmış delegasyonları olması gerektiğidir.  HSM sağlama bu olmadan çalışmaz.

Tüm ön koşullar yerine geçtikten sonra, değerleri benzersiz adlarNızla (en azından kaynak grubu adı) güncelleştirdiğinizi sağlamak için Kaynak Yöneticisi şablonunu kullanmak için aşağıdaki komutu çalıştırın:

```powershell

New-AzResourceGroupDeployment -ResourceGroupName myRG `
     -TemplateFile .\Deploy-2HSM-toVNET-Template.json `
     -TemplateParameterFile .\Deploy-2HSM-toVNET-Params.json `
     -Name HSMdeploy -Verbose

```

Bu komutun tamamlanması yaklaşık 20 dakika sürer. Kullanılan "-ayrıntılı" seçeneği, durumun sürekli olarak görüntülenmesini sağlar.

![sağlama durumu](media/tutorial-deploy-hsm-powershell/progress-status.png)

"ProvisioningState" ile gösterildiği nde başarılı bir şekilde tamamlandığında, mevcut sanal makinenizde oturum açabilir ve HSM aygıtının kullanılabilirliğini sağlamak için SSH'yi kullanabilirsiniz.

## <a name="verifying-the-deployment"></a>Dağıtımı Doğrulama

Aygıtların sağlandığını doğrulamak ve aygıt özniteliklerini görmek için aşağıdaki komut kümesini çalıştırın. Kaynak grubunun uygun şekilde ayarlandığından ve kaynak adının parametre dosyasında tam olarak olduğu gibi olduğundan emin olun.

```powershell

$subid = (Get-AzContext).Subscription.Id
$resourceGroupName = "myRG"
$resourceName = "HSM1"  
Get-AzResource -Resourceid /subscriptions/$subId/resourceGroups/$resourceGroupName/providers/Microsoft.HardwareSecurityModules/dedicatedHSMs/$resourceName

```

![hüküm durumu](media/tutorial-deploy-hsm-powershell/progress-status2.png)

Artık [Azure kaynak gezginini](https://resources.azure.com/)kullanarak kaynakları da görebilirsiniz.   Explorer'a girdiğinde, soldaki "abonelikleri" genişletin, Özel HSM için özel aboneliğinizi genişletin, "kaynak gruplarını" genişletin, kullandığınız kaynak grubunu genişletin ve son olarak "kaynaklar" öğesini seçin.

## <a name="testing-the-deployment"></a>DağıtımTest

Dağıtımı sınamak, HSM'ye erişebilen sanal bir makineye bağlanma ve ardından doğrudan HSM aygıtına bağlanma durumudur. Bu eylemler HSM'ye ulaşılabilir olduğunu doğrular.
Ssh aracı sanal makineye bağlanmak için kullanılır. Komut aşağıdakine benzer, ancak parametrede belirttiğiniz yönetici adı ve dns adı ile.

`ssh adminuser@hsmlinuxvm.westus.cloudapp.azure.com`

Kullanılacak parola parametre dosyasından bir paroladır.
Linux VM'de oturum açtıktan sonra, hsm_vnic \<kaynak öneki için portalda bulunan özel IP adresini kullanarak HSM'ye giriş yapabilirsiniz>.

```powershell

(Get-AzResource -ResourceGroupName myRG -Name HSMdeploy -ExpandProperties).Properties.networkProfile.networkInterfaces.privateIpAddress

```
IP adresiniz varsa, aşağıdaki komutu çalıştırın:

`ssh tenantadmin@<ip address of HSM>`

Başarılı olursa, bir parola istenir. Varsayılan parola PASSWORD'tir. HSM sizden parolanızı değiştirmenizi ister, böylece güçlü bir parola belirleyin ve kuruluşunuzun parolayı depolamak ve kaybı önlemek için hangi mekanizmayı tercih edeceğini kullanın.  

>[!IMPORTANT]
>Bu parolayı kaybederseniz, HSM'nin sıfırlanması gerekir ve bu da anahtarlarınızı kaybetmek anlamına gelir.

SSH kullanarak HSM aygıtına bağlandığınızda, HSM'nin çalışır durumda olduğundan emin olmak için aşağıdaki komutu çalıştırın.

`hsm show`

Çıktı aşağıda gösterilen resim gibi görünmelidir:

![hüküm durumu](media/tutorial-deploy-hsm-powershell/output.png)

Bu noktada, yüksek kullanılabilir, iki HSM dağıtımı ve doğrulanmış erişim ve operasyonel durum için tüm kaynakları ayırdınız. Daha fazla yapılandırma veya test HSM aygıtının kendisi ile daha fazla çalışma içerir. Bunun için, HSM'yi başlatmave bölümler oluşturmak için Gemalto Luna Network HSM 7 Yönetim Kılavuzu bölüm 7'deki yönergeleri izlemeniz gerekir. Gemalto Müşteri Destek Portalı'na kaydolduktan ve müşteri kimliğine sahip olduktan sonra tüm dokümantasyon ve yazılımlar doğrudan Gemalto'dan indirilebilir. Gerekli tüm bileşenleri almak için İstemci Yazılımı sürüm 7.2'yi indirin.

## <a name="delete-or-clean-up-resources"></a>Kaynakları silme veya temizleme

Sadece HSM aygıtıile bitirdiyseniz, kaynak olarak silinebilir ve boş havuza döndürülebilir. Bunu yaparken en belirgin endişe cihazda herhangi bir hassas müşteri verileridir. Bir cihazı "sıfırlamanın" en iyi yolu, HSM yönetici parolasını yanlış 3 kez almaktır (not: bu cihaz yöneticisi değil, gerçek HSM yöneticisidir). Anahtar malzemeyi korumak için bir güvenlik önlemi olarak, aygıt sıfırlanmış duruma gelene kadar Azure kaynağı olarak silinemez.

> [!NOTE]
> Herhangi bir Gemalto cihaz yapılandırması ile ilgili bir sorununuz varsa [Gemalto müşteri desteğine](https://safenet.gemalto.com/technical-support/)başvurmalısınız.

Azure'daki HSM kaynağını kaldırmak istiyorsanız, "$" değişkenlerini benzersiz parametrelerinizle değiştirmek için aşağıdaki komutu kullanabilirsiniz:

```powershell

$subid = (Get-AzContext).Subscription.Id
$resourceGroupName = "myRG" 
$resourceName = "HSMdeploy"  
Remove-AzResource -Resourceid /subscriptions/$subId/resourceGroups/$resourceGroupName/providers/Microsoft.HardwareSecurityModules/dedicatedHSMs/$resourceName 

```

## <a name="next-steps"></a>Sonraki adımlar

Öğreticideki adımları tamamladıktan sonra, Özel HSM kaynakları sağlanır ve sanal aağınızda kullanılabilir. Artık bu dağıtıma, tercih ettiğiniz dağıtım mimarisinin gerektirdiği şekilde daha fazla kaynakla iltifat edebilecek bir konumdasınız. Dağıtımınızı planlamaya yardımcı olmak hakkında daha fazla bilgi için Kavramlar belgelerine bakın. Raf düzeyinde kullanılabilirliği ele alan birincil bölgede iki HSM'li bir tasarım ve bölgesel kullanılabilirliği ele alan ikincil bir bölgede iki HSM önerilir. Bu öğreticide kullanılan şablon dosyası, iki HSM dağıtımı için kolayca temel olarak kullanılabilir, ancak parametrelerinin gereksinimlerinizi karşılamak için değiştirilmesi gerekir.

* [Yüksek Kullanılabilirlik](high-availability.md)
* [Fiziksel Güvenlik](physical-security.md)
* [Ağ Oluşturma](networking.md)
* [İzleme](monitoring.md)
* [Desteklenebilirlik](supportability.md)
