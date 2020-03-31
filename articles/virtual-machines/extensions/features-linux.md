---
title: Linux için Azure VM uzantıları ve özellikleri
description: Azure sanal makineleri için hangi uzantıların kullanılabileceğini ve sağladıklarına veya geliştirdiklerine göre gruplandırılan uzantıları öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: axayjo
manager: gwallace
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: 52f5d0ec-8f75-49e7-9e15-88d46b420e63
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/30/2018
ms.author: akjosh
ms.openlocfilehash: 67df46742be52b03bd91af19654fbfac5df29646
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79250523"
---
# <a name="virtual-machine-extensions-and-features-for-linux"></a>Linux için sanal makine uzantıları ve özellikleri

Azure sanal makine (VM) uzantıları, dağıtım sonrası yapılandırma ve Azure VM'lerinde otomasyon görevleri sunan küçük uygulamalardır. Örneğin bir sanal makinede yazılım yüklemesi gerekiyorsa, virüsten koruma gerekiyorsa veya içinde bir betik çalıştırılacaksa VM uzantısı kullanılabilir. Azure VM uzantıları Azure CLI, PowerShell, Azure Resource Manager şablonları ve Azure portal ile çalıştırılabilir. Uzantılar yeni bir VM dağıtımıyla paketlenebilir veya var olan herhangi bir sistemde çalıştırılabilir.

Bu makalede, VM uzantılarına genel bakış, Azure VM uzantılarını kullanmak için ön koşullar ve VM uzantılarının nasıl algılanıp, yöneteceğimize ve kaldırılacaklarına ilişkin kılavuzlar yer almaktadır. Bu makalede, her biri benzersiz bir yapılandırmaya sahip birçok VM uzantısı bulunduğundan genelleştirilmiş bilgiler sağlanmaktadır. Uzantıya özgü ayrıntılar, her belgede tek tek uzantıya özgü bulunabilir.

## <a name="use-cases-and-samples"></a>Servis taleplerini ve örneklerini kullanma

Her biri belirli bir kullanım örneğine sahip birkaç farklı Azure VM uzantısı mevcuttur. Bazı örnekler:

- Linux için DSC uzantısı ile PowerShell İstenilen Durum yapılandırmalarını vm'ye uygulayın. Daha fazla bilgi için Bkz. [Azure İstenilen Durum yapılandırma uzantısı.](https://github.com/Azure/azure-linux-extensions/tree/master/DSC)
- Bir VM'nin izlemesini Microsoft İzleme Aracısı VM uzantısı ile yapılandırın. Daha fazla bilgi için linux [VM'yi nasıl izleyebilirsiniz.](../linux/tutorial-monitoring.md)
- Azure altyapınızın izleme denetimini Chef veya Datadog uzantısı ile yapılandırın. Daha fazla bilgi için [Şef dokümanlarına](https://docs.chef.io/azure_portal.html) veya [Datadog bloguna](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/)bakın.

İşleme özel uzantılara ek olarak, hem Windows hem de Linux sanal makineleri için özel komut dosyası uzantısı kullanılabilir. Linux için Özel Komut Dosyası uzantısı, herhangi bir Bash komut dosyasının VM'de çalıştırılmasına izin verir. Özel komut dosyaları, yerel Azure araçlarının sağlayabileceğinin ötesinde yapılandırma gerektiren Azure dağıtımları tasarlamak için yararlıdır. Daha fazla bilgi için [Linux VM Özel Komut Dosyası uzantısına](custom-script-linux.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

VM'deki uzantıyı işlemek için Azure Linux Aracısı'nın yüklü olması gerekir. Bazı bireysel uzantıların kaynaklara erişim veya bağımlılıklar gibi ön koşulları vardır.

### <a name="azure-vm-agent"></a>Azure VM aracısı

Azure VM aracısı, Bir Azure VM ile Azure kumaş denetleyicisi arasındaki etkileşimleri yönetir. VM aracısı, VM uzantılarının çalıştırılması da dahil olmak üzere Azure VM'lerini dağıtmanın ve yönetmenin birçok işlevsel yönüne sorumludur. Azure VM aracısı Azure Marketi görüntülerine önceden yüklenir ve desteklenen işletim sistemlerine el ile yüklenebilir. Linux için Azure VM Agent Linux aracısı olarak bilinir.

Desteklenen işletim sistemleri ve yükleme yönergeleri hakkında daha fazla bilgi için [Azure sanal makine aracısı'na](agent-linux.md)bakın.

#### <a name="supported-agent-versions"></a>Desteklenen aracı sürümleri

Mümkün olan en iyi deneyimi sağlamak için, aracının minimum sürümleri vardır. Daha fazla bilgi için [bu makaleye](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support)bakın.

#### <a name="supported-oses"></a>Desteklenen OS'ler

Linux aracısı birden fazla OS üzerinde çalışır, ancak uzantıları çerçeve os için bir sınırı vardır bu uzantıları. Daha fazla bilgi için [bu makaleye](https://support.microsoft.com/en-us/help/4078134/azure-extension-supported-operating-systems
)bakın.

Bazı uzantılar tüm Işletim Sistemi'lerde desteklenmez ve *'Desteklenmeyen Işletim Sistemi' hata kodu 51'i*yayabilir. Desteklenebilirlik için tek tek uzantı belgelerini kontrol edin.

#### <a name="network-access"></a>Ağ erişimi

Uzantı paketleri Azure Depolama uzantısı deposundan indirilir ve uzantı durumu yüklemeleri Azure Depolama'ya nakledilir. Aracıların [desteklenen](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) sürümünü kullanıyorsanız, aracı iletişimi için iletişimi Azure kumaş denetleyicisine yönlendirmek için aracıyı kullanabileceğiniz gibi, VM bölgesindeki Azure Depolama'ya erişime izin vermeniz gerekmez. Aracının desteklenmeyen bir sürümündeyseniz, VM'den o bölgedeki Azure depolama alanına giden erişime izin verebilirsiniz.

> [!IMPORTANT]
> Konuk güvenlik duvarını kullanarak *168.63.129.16'ya* erişimi engellediyseniz, uzantılar yukarıdakilerden bağımsız olarak başarısız olur.

Aracılar yalnızca uzantı paketlerini ve raporlama durumunu indirmek için kullanılabilir. Örneğin, bir uzantı yüklemesi GitHub'dan (Özel Komut Dosyası) komut dosyası indirmesi gerekiyorsa veya Azure Depolama 'ya (Azure Yedeklemesi) erişmesi gerekiyorsa, ek güvenlik duvarı/Ağ Güvenlik Grubu bağlantı noktalarının açılması gerekir. Kendi başlarına uygulamalar olduğundan, farklı uzantıların farklı gereksinimleri vardır. Azure Depolama alanına erişim gerektiren uzantılar için, [Depolama](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags)için Azure NSG Hizmet Etiketleri'ni kullanarak erişime izin verebilirsiniz.

Aracı trafik isteklerini yönlendirmek için Linux Agent proxy sunucu desteğine sahiptir. Ancak, bu proxy sunucu desteği uzantıları geçerli değildir. Her bir uzantıyı bir proxy ile çalışacak şekilde yapılandırmanız gerekir.

## <a name="discover-vm-extensions"></a>VM uzantılarını keşfedin

Azure VM'leri ile kullanabileceğiniz birçok farklı VM uzantısı vardır. Tam listeyi görmek için [az vm uzantılı resim listesini](/cli/azure/vm/extension/image#az-vm-extension-image-list)kullanın. Aşağıdaki örnek, *westus* konumundaki tüm kullanılabilir uzantıları listeler:

```azurecli
az vm extension image list --location westus --output table
```

## <a name="run-vm-extensions"></a>VM uzantılarını çalıştırma

Azure VM uzantıları varolan VM'lerde çalışır, bu da zaten dağıtılmış bir VM'de yapılandırma değişiklikleri yapmanız veya bağlantı kurtarmanız gerektiğinde yararlıdır. VM uzantıları, Azure Kaynak Yöneticisi şablon dağıtımlarıyla da birlikte kullanılabilir. Azure VM'leri, Kaynak Yöneticisi şablonlarına sahip uzantılar kullanarak dağıtım sonrası müdahale olmadan dağıtılabilir ve yapılandırılabilir.

Aşağıdaki yöntemler, bir uzantıyı varolan bir VM'ye karşı çalıştırmak için kullanılabilir.

### <a name="azure-cli"></a>Azure CLI

Azure VM [uzantıları, az vm uzantı kümesi](/cli/azure/vm/extension#az-vm-extension-set) komutuyla varolan bir VM'ye karşı çalıştırılabilir. Aşağıdaki örnek, *myResourceGroup*adlı bir kaynak grubunda *myVM* adlı bir VM'ye karşı Özel Komut Dosyası uzantısını çalıştırın. Çalıştırmak için örnek kaynak grubu adını, VM adını\/ve komut dosyasını (https: /raw.githubusercontent.com/me/project/hello.sh) kendi bilgilerinizle değiştirin. 

```azurecli
az vm extension set `
  --resource-group myResourceGroup `
  --vm-name myVM `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://raw.githubusercontent.com/me/project/hello.sh"],"commandToExecute": "./hello.sh"}'
```

Uzantı doğru çalıştığında, çıktı aşağıdaki örneğe benzer:

```bash
info:    Executing command vm extension set
+ Looking up the VM "myVM"
+ Installing extension "CustomScript", VM: "mvVM"
info:    vm extension set command OK
```

### <a name="azure-portal"></a>Azure portalında

VM uzantıları, Azure portalı üzerinden varolan bir VM'ye uygulanabilir. Portaldaki VM'yi seçin, **Uzantıları**seçin, ardından **Ekle'yi**seçin. Kullanılabilir uzantılar listesinden istediğiniz uzantıyı seçin ve sihirbazdaki yönergeleri izleyin.

Aşağıdaki resim, Azure portalından Linux Özel Komut Dosyası uzantısının yüklenmesini gösterir:

![Özel komut dosyası uzantısını yükleme](./media/features-linux/installscriptextensionlinux.png)

### <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları

VM uzantıları bir Azure Kaynak Yöneticisi şablonuna eklenebilir ve şablonun dağıtımıyla yürütülebilir. Bir uzantıyı şablonla dağıttığınızda, tam olarak yapılandırılmış Azure dağıtımları oluşturabilirsiniz. Örneğin, aşağıdaki JSON, yük dengeli VM'ler kümesini ve Azure SQL veritabanını dağıtan ve her VM'ye bir .NET Core uygulaması yükleyen bir Kaynak Yöneticisi şablonundan alınır. VM uzantısı yazılım yükleme ilgilenir.

Daha fazla bilgi için kaynak [yöneticisi şablonuna](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux)bakın.

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
    }
}
```

Kaynak Yöneticisi şablonları oluşturma hakkında daha fazla bilgi için [bkz.](../windows/template-description.md#extensions)

## <a name="secure-vm-extension-data"></a>Güvenli VM uzantısı verileri

Bir VM uzantısı çalıştırdığınızda, kimlik bilgileri, depolama hesabı adları ve depolama hesabı erişim anahtarları gibi hassas bilgileri eklemek gerekebilir. Birçok VM uzantısı, verileri şifreleyen ve yalnızca hedef VM'nin içindeki şifresini çözen korumalı bir yapılandırma içerir. Her uzantı belirli bir korumalı yapılandırma şeması vardır ve her biri uzantıya özgü belgelerde ayrıntılı olarak açıklanır.

Aşağıdaki örnek, Linux için Özel Komut Dosyası uzantısı nın bir örneğini gösterir. Yürütülecek komut bir kimlik bilgileri kümesi içerir. Bu örnekte, yürütülecek komut şifrelenmez:

```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ],
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

Özelliği **nizli yapılandırmaya taşımak,** aşağıdaki örnekte gösterildiği gibi yürütme dizesini korur: **protected**

```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

### <a name="how-do-agents-and-extensions-get-updated"></a>Aracılar ve uzantılar nasıl güncellenir?

Aracılar ve Uzantılar aynı güncelleştirme mekanizmasını paylaşır. Bazı güncelleştirmeler ek güvenlik duvarı kuralları gerektirmez.

Bir güncelleştirme kullanılabilir olduğunda, yalnızca uzantılarda bir değişiklik olduğunda vm'ye yüklenir ve aşağıdakiler gibi diğer VM Modeli değişiklikleri yapılır:

- Veri diskleri
- Uzantılar
- Önyükleme tanılama konteyneri
- Konuk İşletim Sistemi sırları
- VM boyutu
- Ağ profili

Yayıncılar güncelleştirmeleri farklı zamanlarda bölgelere kullanılabilir hale getirmektedir, bu nedenle farklı sürümlerde farklı bölgelerdeki VM'lere sahip olabilirsiniz.

#### <a name="agent-updates"></a>Aracı güncelleştirmeleri

Linux VM Aracısı, ayrılamayan bir pakette *Sağlama Aracıkodu* ve *Uzantı Taşıma kodu* içerir. Bulut init'i kullanarak Azure'da hüküm vermek istediğinizde *Sağlama Aracısını* devre dışı kullanabilirsiniz. Bunu yapmak için [bulut-init'i kullanma](../linux/using-cloud-init.md)hakkında bkz.

Aracıların desteklenen sürümleri otomatik güncelleştirmeleri kullanabilir. Güncelleştirilebilen tek kod, sağlama kodu değil, *Uzantı İşleme kodudur.* *Geçici Madde Kodu* bir kez çalıştırılan.

*Uzantı İşleme kodu,* Azure dokusuyla iletişim kurmak ve yüklemeler, durum bildirme, tek tek uzantıları güncelleştirme ve bunları kaldırma gibi VM uzantıları işlemlerini işlemekten sorumludur. Güncelleştirmeler güvenlik düzeltmeleri, hata düzeltmeleri ve *Uzantı İşleme kodundaki*geliştirmeleri içerir.

Aracı yüklendiğinde, bir üst daemon oluşturulur. Bu üst sonra uzantıları işlemek için kullanılan bir alt işlem yumurtlar. Aracı için bir güncelleştirme varsa, karşıdan yüklenir, üst öğe alt işlemi durdurur, yükseltir ve yeniden başlatır. Güncelleştirmeyle ilgili bir sorun varsa, üst işlem önceki alt sürüme geri döner.

Üst işlem otomatik olarak güncelleştirilemez. Üst yalnızca bir dağıtım paketi güncelleştirmesi tarafından güncellenebilir.

Hangi sürümü çalıştırdığınızı denetlemek `waagent` için aşağıdaki leri denetleyin:

```bash
waagent --version
```

Çıktı aşağıdaki örneğe benzer:

```bash
WALinuxAgent-2.2.17 running on ubuntu 16.04
Python: 3.5.2
Goal state agent: 2.2.18
```

Önceki örnek çıktıda, üst veya 'paket dağıtılan sürüm' *WALinuxAgent-2.2.17*

'Hedef durum aracısı' otomatik güncelleştirme sürümüdür.

Bu son derece her zaman aracı, [AutoUpdate.Enabled= y](https://docs.microsoft.com/azure/virtual-machines/linux/update-agent)için otomatik güncelleme olması tavsiye edilir. Bunun etkin olmaması, aracıyı el ile güncelleştirmeye devam etmeniz ve hata ve güvenlik düzeltmeleri almamanız gerektiği anlamına gelir.

#### <a name="extension-updates"></a>Uzantı güncelleştirmeleri

Bir uzantı güncelleştirmesi olduğunda, Linux Agent uzantıyı karşıdan yüklenir ve yükseltir. Otomatik uzantı güncelleştirmeleri *Minor* veya *Hotfix'tir.* Uzantıyı siz lerle birlikte verirken *Küçük* güncelleştirmeleri devre dışı kılabilir veya devre dışı kullanabilirsiniz. Aşağıdaki örnek, otomatik YükseltmeMinorVersion ile Kaynak Yöneticisi şablonundaki küçük sürümleri niçin otomatik olarak yükselteceklerini *gösterir": doğru,'*:

```json
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
        ]
    },
```

En son küçük sürüm hata düzeltmelerini almak için, uzantı dağıtımlarınızda her zaman otomatik güncelleştirmeyi seçmeniz önerilir. Güvenlik veya anahtar hata düzeltmeleri taşıyan Hotfix güncelleştirmeleri devre dışı kalınamaz.

### <a name="how-to-identify-extension-updates"></a>Uzantı güncelleştirmeleri nasıl tanımlanır?

#### <a name="identifying-if-the-extension-is-set-with-autoupgrademinorversion-on-a-vm"></a>Bir VM'de otomatik YükseltmeMinorVersion ile uzanTıolup olmadığını belirleme

Uzantın 'autoUpgradeMinorVersion' ile sağlanmış olup olmadığını VM modelinden görebilirsiniz. Kontrol etmek için [az vm show'u](/cli/azure/vm#az-vm-show) kullanın ve kaynak grubunu ve VM adını aşağıdaki gibi sağlayın:

```azurecli
az vm show --resource-group myResourceGroup --name myVM
```

Aşağıdaki örnek *çıktı, autoUpgradeMinorVersion'un* *doğru*olarak ayarladığını gösterir:

```json
  "resources": [
    {
      "autoUpgradeMinorVersion": true,
      "forceUpdateTag": null,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM/extensions/CustomScriptExtension",
```

#### <a name="identifying-when-an-autoupgrademinorversion-occurred"></a>OtomatikYükseltmeMinorVersion'un ne zaman oluştuğunu tanımlama

Uzantıiçin bir güncelleştirmenin ne zaman oluştuğunu görmek için, */var/log/waagent.log.log*adresindevm'deki aracı günlüklerini gözden geçirin.

Aşağıdaki örnekte, VM *Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9025* yüklü vardı. Bir düzeltme *Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027*için kullanılabilir:

```bash
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Expected handler state: enabled
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Decide which version to use
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Use version: 2.3.9027
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Current handler state is: NotInstalled
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Download extension package
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Unpack extension package
INFO Event: name=Microsoft.OSTCExtensions.LinuxDiagnostic, op=Download, message=Download succeeded
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Initialize extension directory
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Update settings file: 0.settings
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9025] Disable extension.
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9025] Launch command:diagnostic.py -disable
...
INFO Event: name=Microsoft.OSTCExtensions.LinuxDiagnostic, op=Disable, message=Launch command succeeded: diagnostic.py -disable
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Update extension.
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Launch command:diagnostic.py -update
2017/08/14 20:21:57 LinuxAzureDiagnostic started to handle.
```

## <a name="agent-permissions"></a>Aracı izinleri

Görevlerini gerçekleştirmek için aracının *kök*olarak çalışması gerekir.

## <a name="troubleshoot-vm-extensions"></a>Sorun Giderme VM uzantıları

Her VM uzantısı, uzantıya özgü sorun giderme adımları olabilir. Örneğin, Özel Komut Dosyası uzantısını kullandığınızda, komut dosyası yürütme ayrıntıları, uzantıntın çalıştırıldığı VM'de yerel olarak bulunabilir. Uzantıya özgü sorun giderme adımları uzantıya özgü belgelerde ayrıntılı olarak açıklanır.

Aşağıdaki sorun giderme adımları tüm VM uzantıları için geçerlidir.

1. Linux Agent Log'u kontrol etmek için uzantınız */var/log/waagent.log'da* sağlanan aktiviteye bakın

2. */var/log/azure/\<extensionName>'da* daha fazla bilgi için gerçek uzantı günlüklerini kontrol edin

3. Hata kodları, bilinen sorunlar vb. için uzantıya özgü belgeler sorun giderme bölümlerini denetleyin.

3. Sistem kayıtlarına bak. Özel paket yöneticisi erişimi gerektiren başka bir uygulamanın uzun süre devam eden yüklemesi gibi uzantıyı etkileyebilecek diğer işlemleri denetleyin.

### <a name="common-reasons-for-extension-failures"></a>Uzantı hatalarının yaygın nedenleri

1. Uzantıları çalıştırmak için 20 dakika var (özel durumlar CustomScript uzantıları, Şef ve 90 dakika var DSC vardır). Dağıtımınız bu zamanı aşarsa, zaman aşımı olarak işaretlenir. Bunun nedeni, düşük kaynak VM'leri, diğer VM yapılandırmaları/uzantı sağlamaya çalışırken yüksek miktarda kaynak tüketen görevleri başlatmadan kaynaklanabilir.

2. Asgari ön koşullar yerine getirilmedi. Bazı uzantıların HPC görüntüleri gibi VM SKU'lara bağımlı olduğu vardır. Uzantılar, Azure Depolama alanıyla veya kamu hizmetlerine iletişim kurmak gibi belirli ağ erişimi gereksinimleri gerektirebilir. Diğer örnekler paket depolarına erişim, disk alanının tükenmesi veya güvenlik kısıtlamaları olabilir.

3. Özel paket yöneticisi erişimi. Bazı durumlarda, her ikisinin de paket yöneticisine özel erişime ihtiyaç duyduğu uzun soluklu bir VM yapılandırması ve uzantı yüklemesi çakışıyor olabilir.

### <a name="view-extension-status"></a>Uzantı durumunu görüntüleme

VM uzantısı bir VM'ye karşı çalıştırıldıktan sonra, uzantı durumunu aşağıdaki gibi döndürmek için [az vm get-instance-view'ı](/cli/azure/vm#az-vm-get-instance-view) kullanın:

```azurecli
az vm get-instance-view \
    --resource-group rgName \
    --name myVM \
    --query "instanceView.extensions"
```

Çıktı aşağıdaki örnek çıktıya benzer:

```bash
  {
    "name": "customScript",
    "statuses": [
      {
        "code": "ProvisioningState/failed/0",
        "displayStatus": "Provisioning failed",
        "level": "Error",
        "message": "Enable failed: failed to execute command: command terminated with exit status=127\n[stdout]\n\n[stderr]\n/bin/sh: 1: ech: not found\n",
        "time": null
      }
    ],
    "substatuses": null,
    "type": "Microsoft.Azure.Extensions.customScript",
    "typeHandlerVersion": "2.0.6"
  }
```

Uzantı yürütme durumu Azure portalında da bulunabilir. Uzantının durumunu görüntülemek için VM'yi seçin, **Uzantıları**seçin ve ardından istediğiniz uzantıyı seçin.

### <a name="rerun-a-vm-extension"></a>VM uzantısını yeniden çalıştırın

VM uzantısının yeniden çalıştırılması gereken durumlar olabilir. Bir uzantıyı kaldırarak ve ardından istediğiniz yürütme yöntemiyle uzantıyı yeniden çalıştırarak yeniden çalıştırabilirsiniz. Bir uzantıyı kaldırmak için [az vm uzantısı silme'yi](/cli/azure/vm/extension#az-vm-extension-delete) aşağıdaki gibi kullanın:

```azurecli
az vm extension delete \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --name customScript
```

Azure portalındaki bir uzantıyı aşağıdaki gibi kaldırabilirsiniz:

1. Bir VM seçin.
2. **Uzantıları**seçin.
3. İstenilen uzantıyı seçin.
4. **Kaldır'ı**seçin.

## <a name="common-vm-extension-reference"></a>Ortak VM uzantısı başvurusu

| Uzantı adı | Açıklama | Daha fazla bilgi |
| --- | --- | --- |
| Linux için Özel Script uzantısı |Komut dosyalarını Azure sanal makinesine karşı çalıştırma |[Linux için Özel Script uzantısı](custom-script-linux.md) |
| VM Erişimi uzantısı |Azure sanal makinesine yeniden erişin |[VM Erişimi uzantısı](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |
| Azure Tanılama uzantısı |Azure Tanılama'yı Yönetme |[Azure Tanılama uzantısı](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| Azure VM Access uzantısı |Kullanıcıları ve kimlik bilgilerini yönetme |[Linux için VM Access uzantısı](https://azure.microsoft.com/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |

## <a name="next-steps"></a>Sonraki adımlar

VM uzantıları hakkında daha fazla bilgi için [Azure sanal makine uzantılarına ve özelliklere genel bakış](overview.md)alabakın.
