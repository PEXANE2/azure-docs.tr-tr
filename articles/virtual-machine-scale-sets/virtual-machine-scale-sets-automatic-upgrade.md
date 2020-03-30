---
title: Azure sanal makine ölçek kümeleri ile otomatik işletim sistemi görüntü yükseltmeleri
description: Bir ölçek kümesinde VM örneklerinde işletim sistemi görüntüsünü otomatik olarak nasıl yükselteceğimiz hakkında bilgi edinin
author: mayanknayar
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 03/18/2020
ms.author: manayar
ms.openlocfilehash: 6d550e8e960cb8e212702796467c91d1cd1ebb23
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80235172"
---
# <a name="azure-virtual-machine-scale-set-automatic-os-image-upgrades"></a>Azure sanal makine ölçeği otomatik işletim sistemi görüntü yükseltmeleri ayarlayın

Ölçek setinizde otomatik işletim sistemi görüntü yükseltmelerini etkinleştirmek, ölçek kümesindeki tüm örnekler için işletim sistemi diskini güvenli ve otomatik olarak yükselterek güncelleştirme yönetimini kolaylaştırmaya yardımcı olur.

Otomatik işletim sistemi yükseltmesi aşağıdaki özelliklere sahiptir:

- Yapılandırıldıktan sonra, görüntü yayımcılar tarafından yayınlanan en son işletim sistemi görüntüsü, kullanıcı müdahalesi olmadan ölçek kümesine otomatik olarak uygulanır.
- Yayımcı tarafından her yeni görüntü yayımlandığında, örneklerin toplu işlerini yuvarlanan bir şekilde yükseltir.
- Uygulama durumu sondaları ve [Uygulama Durumu uzantısı](virtual-machine-scale-sets-health-extension.md)ile entegre.
- Tüm VM boyutları ve hem Windows hem de Linux görüntüleri için çalışır.
- İstediğiniz zaman otomatik yükseltmeleri devre dışı bekleyebilirsiniz (Işletim Sistemi Yükseltmeleri el ile de başlatılabilir).
- Bir VM'nin işletim sistemi diski, en son görüntü sürümüyle oluşturulan yeni işletim sistemi diski ile değiştirilir. Yapılandırılan uzantılar ve özel veri komut dosyaları çalıştırılırken, kalıcı veri diskleri korunur.
- [Uzantı sıralaması](virtual-machine-scale-sets-extension-sequencing.md) desteklenir.
- Otomatik işletim sistemi görüntü yükseltmeherhangi bir boyutta bir ölçek kümesi üzerinde etkinleştirilebilir.

## <a name="how-does-automatic-os-image-upgrade-work"></a>Otomatik işletim sistemi görüntü yükseltmenasıl çalışır?

Bir yükseltme, bir VM'nin işletim sistemi diskini en son görüntü sürümü kullanılarak oluşturulan yeni bir diskle değiştirerek çalışır. Yapılandırılan tüm uzantılar ve özel veri komut dosyaları işletim sistemi diskinde çalıştırılırken, kalıcı veri diskleri korunur. Uygulama kapalı kalma süresini en aza indirmek için yükseltmeler toplu olarak gerçekleşir ve ölçek ayarlamasının en fazla %20'si herhangi bir zamanda yükseltiler. Ayrıca bir Azure Yük Bakiyesi uygulama durumu sondası veya [Uygulama Durumu uzantısı](virtual-machine-scale-sets-health-extension.md)entegre edebilirsiniz. Biz bir uygulama sinyalizasyon dahil önerilir ve yükseltme sürecinde her toplu iş için yükseltme başarısını doğrulamak.

Yükseltme işlemi aşağıdaki gibi çalışır:
1. Yükseltme işlemine başlamadan önce, orkestratör tüm ölçek kümesindeki örneklerin en fazla %20'sinin sağlıksız (herhangi bir nedenle) olmasını sağlar.
2. Yükseltme orkestratörü, herhangi bir toplu işlem, bir sanal makinenin minimum toplu iş boyutuna tabi olarak toplam örnek sayısının en fazla %20'sine sahip olarak, yükseltme için VM örneklerinin toplu larını tanımlar.
3. Seçilen VM örneklerinin işletim sistemi diski, en son görüntüden oluşturulan yeni bir işletim sistemi diskiyle değiştirilir. Ölçek kümesi modelinde belirtilen tüm uzantılar ve yapılandırmalar yükseltilen örneğe uygulanır.
4. Yapılandırılmış uygulama durumu sondaları veya Uygulama Durumu uzantılı ölçek kümeleri için yükseltme, bir sonraki toplu işlemi yükseltmeye geçmeden önce örneğin sağlıklı olması için 5 dakikaya kadar bekler. Bir örnek yükseltmeden sonra 5 dakika içinde sistem durumunu kurtarmazsa, varsayılan olarak önceki işletim sistemi diski geri yüklenir.
5. Yükseltme orkestratör, yükseltme sonrası sağlıksız hale gelen örneklerin yüzdesini de izler. Yükseltme işlemi sırasında yükseltilen örneklerin %20'den fazlası sağlıksız hale gelirse yükseltme durdurulacaktır.
6. Yukarıdaki işlem, ölçek kümesindeki tüm örnekler yükseltilene kadar devam eder.

Ölçek, her toplu iş yükseltmeden önce genel ölçek kümesi sistem yükseltme orkestratör denetimleri ayarlayın. Bir toplu iş yükseltme sırasında, ölçek kümesi örneklerinin durumunu etkileyebilecek eşzamanlı veya planlanmamış başka bakım etkinlikleri de olabilir. Bu gibi durumlarda, ölçek kümesiörneklerinin %20'den fazlası sağlıksız hale gelirse, ölçek kümesi yükseltmesi geçerli toplu iş sonunda durur.

## <a name="supported-os-images"></a>Desteklenen işletim sistemi görüntüleri
Şu anda yalnızca belirli işletim sistemi platformu görüntüleri desteklenir. Özel görüntü desteği [Paylaşılan Resim Galerisi](shared-image-galleries.md)aracılığıyla özel görüntüler için [önizleme](virtual-machine-scale-sets-automatic-upgrade.md#automatic-os-image-upgrade-for-custom-images-preview) mevcuttur.

Aşağıdaki platform SDO'lar şu anda desteklenir (ve daha fazlası periyodik olarak eklenir):

| Yayımcı               | İşletim Sistemi Teklifi      |  Sku               |
|-------------------------|---------------|--------------------|
| Canonical               | UbuntuServer  | 16.04-LTS          |
| Canonical               | UbuntuServer  | 18.04-LTS          |
| Haydut Dalga (OpenLogic)  | CentOS        | 7,5                |
| CoreOS                  | CoreOS        | Dengeli             |
| Microsoft Corporation   | WindowsServer | 2012-R2-Datacenter |
| Microsoft Corporation   | WindowsServer | 2016-Datacenter    |
| Microsoft Corporation   | WindowsServer | 2016-Datacenter-Smalldisk |
| Microsoft Corporation   | WindowsServer | 2016-Datacenter-With-Containers |
| Microsoft Corporation   | WindowsServer | 2019-Datacenter |
| Microsoft Corporation   | WindowsServer | 2019-Datacenter-Smalldisk |
| Microsoft Corporation   | WindowsServer | 2019-Datacenter-With-Containers |
| Microsoft Corporation   | WindowsServer | Datacenter-Core-1903-with-Containers-smalldisk |


## <a name="requirements-for-configuring-automatic-os-image-upgrade"></a>Otomatik işletim sistemi görüntü yükseltme yapılandırma gereksinimleri

- Görüntünün *sürüm* özelliği *en son*olarak ayarlanmalıdır.
- Hizmet Dışı Kumaş ölçek kümeleri için uygulama durumu problarını veya [Uygulama Durumu uzantısını](virtual-machine-scale-sets-health-extension.md) kullanın.
- 2018-10-01 veya daha yüksek Compute API sürümünü kullanın.
- Ölçek kümesi modelinde belirtilen dış kaynakların kullanılabilir ve güncelleştirilmesini sağlayın. Örnekler arasında VM uzantılı özelliklerdeki yükü önyükleme için SAS URI, depolama hesabındaki taşıma yükü, modeldeki sırlara başvuru ve daha fazlası verilebilir.
- Windows sanal makineleri kullanarak ölçek kümeleri için, Compute API sürümü 2019-03-01 ile başlayan, özellik *virtualMachineProfile.osProfile.windowsConfiguration.enableAutomaticUpdates* özelliği ölçek kümesi modeli tanımında *yanlış* ayarlanmalıdır. Yukarıdaki özellik, "Windows Update"in işletim sistemi yamaları işletim sistemi yamaları işletim sistemi diskini değiştirmeden uyguladığı VM içi yükseltmelere olanak tanır. Ölçek setinizde otomatik işletim sistemi görüntü yükseltmeleri etkinolduğundan, "Windows Update" ile ek bir güncelleştirme gerekmez.

### <a name="service-fabric-requirements"></a>Servis Kumaş ı gereksinimleri

Service Fabric kullanıyorsanız, aşağıdaki koşulların yerine getirildiğinden emin olun:
-   Hizmet Kumaş [dayanıklılık seviyesi](../service-fabric/service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) Gümüş veya Altın değil, Bronz.
-   Ölçek kümesi model tanımındaki Service Fabric uzantısı TypeHandlerVersion 1.1 veya üzeri olmalıdır.
-   Dayanıklılık düzeyi, ölçek kümesi model tanımındaki Service Fabric kümesi nde ve Servis Kumaşı uzantısında aynı olmalıdır.

Bir uyumsuzluk yükseltme hatalarına neden olacağından, Hizmet Kumaşı kümesi ve Servis Kumaşı uzantısında dayanıklılık ayarlarının eşleşmediğinden emin olun. Dayanıklılık düzeyleri [bu sayfada](../service-fabric/service-fabric-cluster-capacity.md#changing-durability-levels)özetlenen yönergelere göre değiştirilebilir.


## <a name="automatic-os-image-upgrade-for-custom-images-preview"></a>Özel görüntüler için otomatik işletim sistemi görüntü yükseltmesi (önizleme)

> [!IMPORTANT]
> Özel görüntüler için otomatik işletim sistemi görüntü yükseltmesi şu anda Genel Önizleme'dedir. Aşağıda açıklanan genel önizleme işlevini kullanmak için bir kabul prosedürü gereklidir.
> Bu önizleme sürümü hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Otomatik Işletim Sistemi görüntü yükseltmesi, [Paylaşılan Resim Galerisi](shared-image-galleries.md)aracılığıyla dağıtılan özel görüntülerin önizlemesinde kullanılabilir. Diğer özel görüntüler otomatik işletim sistemi görüntü yükseltmeleri için desteklenmez.

Önizleme işlevini etkinleştirmek, aşağıda açıklandığı gibi abonelik başına *OtomatikOSUpgradeWithGalleryImage* özelliği için bir kerelik bir tercih gerektirir.

### <a name="rest-api"></a>REST API
Aşağıdaki örnekte, aboneliğiniz için önizlemenin nasıl etkinleştirilen açıklanmıştır:

```
POST on `/subscriptions/{subscriptionId}/providers/Microsoft.Features/providers/Microsoft.Compute/features/AutomaticOSUpgradeWithGalleryImage/register?api-version=2015-12-01`
```

Özellik kaydı 15 dakika kadar sürebilir. Kayıt durumunu kontrol etmek için:

```
GET on `/subscriptions/{subscriptionId}/providers/Microsoft.Features/providers/Microsoft.Compute/features/AutomaticOSUpgradeWithGalleryImage?api-version=2015-12-01`
```

Özellik aboneliğiniz için kaydedildikten sonra, değişikliği Bilgi İşlem kaynak sağlayıcısına yayarak kabul işlemini tamamlayın.

```
POST on `/subscriptions/{subscriptionId}/providers/Microsoft.Compute/register?api-version=2019-10-01`
```

### <a name="azure-powershell"></a>Azure PowerShell
Aboneliğinizin önizlemesini etkinleştirmek için [Register-AzProviderFeature](/powershell/module/az.resources/register-azproviderfeature) cmdlet'i kullanın.

```azurepowershell-interactive
Register-AzProviderFeature -FeatureName AutomaticOSUpgradeWithGalleryImage -ProviderNamespace Microsoft.Compute
```

Özellik kaydı 15 dakika kadar sürebilir. Kayıt durumunu kontrol etmek için:

```azurepowershell-interactive
Get-AzProviderFeature -FeatureName AutomaticOSUpgradeWithGalleryImage -ProviderNamespace Microsoft.Compute
```

Özellik aboneliğiniz için kaydedildikten sonra, değişikliği Bilgi İşlem kaynak sağlayıcısına yayarak kabul işlemini tamamlayın.

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Aboneliğinizin önizlemesini etkinleştirmek için [az özellik kaydını](/cli/azure/feature#az-feature-register) kullanın.

```azurecli-interactive
az feature register --namespace Microsoft.Compute --name AutomaticOSUpgradeWithGalleryImage
```

Özellik kaydı 15 dakika kadar sürebilir. Kayıt durumunu kontrol etmek için:

```azurecli-interactive
az feature show --namespace Microsoft.Compute --name AutomaticOSUpgradeWithGalleryImage
```

Özellik aboneliğiniz için kaydedildikten sonra, değişikliği Bilgi İşlem kaynak sağlayıcısına yayarak kabul işlemini tamamlayın.

```azurecli-interactive
az provider register --namespace Microsoft.Compute
```

### <a name="additional-requirements-for-custom-images"></a>Özel görüntüler için ek gereksinimler
- Yukarıda açıklanan kabul işleminin abonelik başına yalnızca bir kez tamamlanması gerekir. Kabul tamamlandıktan sonra, otomatik işletim sistemi yükseltmeleri bu abonelikte ayarlanan herhangi bir ölçek için etkinleştirilebilir.
- Paylaşılan Resim Galerisi herhangi bir abonelikte olabilir ve ayrı olarak kabul edilmesi gerekmez. Yalnızca ölçek kümesi aboneliği özelliğini devre dışı bırakmayı gerektirir.
- Otomatik işletim sistemi görüntü yükseltmesi için yapılandırma işlemi, bu sayfanın [yapılandırma bölümünde](virtual-machine-scale-sets-automatic-upgrade.md#configure-automatic-os-image-upgrade) ayrıntılı olarak açıklanan tüm ölçek kümeleri için aynıdır.
- Otomatik işletim sistemi görüntü yükseltmeleri için yapılandırılan ölçek kümeleri, görüntünün yeni bir sürümü yayımlandığında ve bu ölçek kümesinin bölgesine [çoğaltıldığında](shared-image-galleries.md#replication) Paylaşılan Görüntü Galerisi görüntüsünün en son sürümüne yükseltilir. Yeni görüntü ölçeğin dağıtıldığı bölgeye çoğaltılmazsa, ölçek kümesi örnekleri en son sürüme yükseltilmez. Bölgesel görüntü çoğaltma ölçek kümeleriniz için yeni görüntünün kullanıma sunulmasını denetlemenize olanak tanır.
- Yeni resim sürümü, galeri resmi için en son sürümden dışlanmamalıdır. Galeri resminin en son sürümünden dışlanan resim sürümleri otomatik işletim sistemi görüntü yükseltmesi yoluyla ayarlanan ölçekte kullanıma sunulmaz.

> [!NOTE]
>Ölçek kümesi otomatik işletim sistemi yükseltmeleri için yapılandırıldıktan sonra ilk görüntü lansmanını almak için bir ölçek kümesinin 2 saat kadar sürebilir. Bu, ölçek kümesi başına bir kerelik gecikmedir. Sonraki görüntü rollouts bu gecikme olmadan ölçek kümesine uygulanır.


## <a name="configure-automatic-os-image-upgrade"></a>Otomatik işletim sistemi görüntü yükseltmeyapılandırma
Otomatik işletim sistemi görüntü yükseltmesini yapılandırmak için *automaticOSUpgradePolicy.enableAutomaticOSUpgrade* özelliğinin ölçek kümesi modeli tanımında *doğru* ayarlandığından emin olun.

### <a name="rest-api"></a>REST API
Aşağıdaki örnekte, bir ölçek kümesi modelinde otomatik işletim sistemi yükseltmelerinin nasıl ayarlanır:

```
PUT or PATCH on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet?api-version=2018-10-01`
```

```json
{
  "properties": {
    "upgradePolicy": {
      "automaticOSUpgradePolicy": {
        "enableAutomaticOSUpgrade":  true
      }
    }
  }
}
```

### <a name="azure-powershell"></a>Azure PowerShell
Ölçek setiniz için otomatik işletim sistemi görüntü yükseltmelerini yapılandırmak için [Update-AzVmss](/powershell/module/az.compute/update-azvmss) cmdlet'i kullanın. Aşağıdaki örnek, *myResourceGroup*adlı kaynak grubunda *myScaleSet* adlı ölçek kümesi için otomatik yükseltmeleri yapılandırır:

```azurepowershell-interactive
Update-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -AutomaticOSUpgrade $true
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Ölçek kümeniz için otomatik işletim sistemi görüntü yükseltmelerini yapılandırmak için [az vmss güncelleştirmesini](/cli/azure/vmss#az-vmss-update) kullanın. Azure CLI 2.0.47 ve üzeri kullanın. Aşağıdaki örnek, *myResourceGroup*adlı kaynak grubunda *myScaleSet* adlı ölçek kümesi için otomatik yükseltmeleri yapılandırır:

```azurecli-interactive
az vmss update --name myScaleSet --resource-group myResourceGroup --set UpgradePolicy.AutomaticOSUpgradePolicy.EnableAutomaticOSUpgrade=true
```

## <a name="using-application-health-probes"></a>Uygulama Durumu Sondalarını Kullanma

İşletim Sistemi Yükseltmesi sırasında, ölçek kümesindeki VM örnekleri her seferinde bir toplu iş olarak yükseltilir. Yükseltme yalnızca yükseltilen VM örneklerinde müşteri uygulaması sağlıklıysa devam etmelidir. Uygulamanın ölçek kümesi OS Yükseltme motoruna sistem durumu sinyalleri sağlamasını öneririz. Varsayılan olarak, OS Yükseltmeleri sırasında platform, bir VM örneğinin yükseltmeden sonra sağlıklı olup olmadığını belirlemek için VM güç durumunu ve genişletme sağlama durumunu dikkate alır. Bir VM örneğinin işletim sistemi yükseltmesi sırasında, VM örneğindeki işletim sistemi diski en son görüntü sürümüne dayalı yeni bir diskle değiştirilir. İşletim Sistemi Yükseltmesi tamamlandıktan sonra, yapılandırılan uzantılar bu VM'lerde çalıştırılır. Uygulama, yalnızca örnekteki tüm uzantılar başarıyla sağlandığında sağlıklı kabul edilir.

Bir ölçek kümesi isteğe bağlı olarak Uygulama Durumu Sondaları ile uygulamanın devam eden durumu hakkında doğru bilgi ile platform sağlamak için yapılandırılabilir. Uygulama Durumu Probları, sağlık sinyali olarak kullanılan Özel Yük Dengeleyici Problarıdır. VM örneği ölçeğinde çalışan uygulama, sağlıklı olup olmadığını belirten harici HTTP veya TCP isteklerine yanıt verebilir. Özel Yük Dengeleyici Problarının nasıl çalıştığı hakkında daha fazla bilgi için [yük dengeleyici problarını anlamak](../load-balancer/load-balancer-custom-probe-overview.md)için bkz. Uygulama Durumu Probları Servis Kumaş ölçek setleri için desteklenmez. Hizmet Dışı Kumaş ölçek kümeleri yük dengeleyici uygulama sistem sondaları veya [Uygulama Durumu uzantısı](virtual-machine-scale-sets-health-extension.md)gerektirir.

Ölçek kümesi birden çok yerleşim grubu kullanacak şekilde yapılandırılırsa, [Standart Yük Dengeleyicisi](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview) kullanan probların kullanılması gerekir.

### <a name="configuring-a-custom-load-balancer-probe-as-application-health-probe-on-a-scale-set"></a>Özel Yük Dengeleyici Probu'nun Uygulama Durumu Sondası olarak bir ölçekte yapılandırılma
En iyi uygulama olarak, ölçek kümesi durumu için açıkça bir yük dengeleyici sondası oluşturun. Varolan bir HTTP sondası veya TCP sondası için aynı bitiş noktası kullanılabilir, ancak bir sağlık sondası geleneksel yük dengeleyici sondasından farklı davranışlar gerektirebilir. Örneğin, örneğin üzerindeki yük çok yüksekse, geleneksel bir yük dengeleyici sondası sağlıksız dönebilir, ancak bu otomatik bir işletim sistemi yükseltmesi sırasında örneğin durumunu belirlemek için uygun olmaz. Sondayı iki dakikadan kısa bir süre yüksek bir sondalama oranına sahip olacak şekilde yapılandırın.

Yük dengeleyici prob ölçek kümesinin *ağProfili'nde* başvurulabilir ve aşağıdaki gibi dahili veya genel olarak bakan bir yük dengeleyicisi ile ilişkilendirilebilir:

```json
"networkProfile": {
  "healthProbe" : {
    "id": "[concat(variables('lbId'), '/probes/', variables('sshProbeName'))]"
  },
  "networkInterfaceConfigurations":
  ...
}
```

> [!NOTE]
> Service Fabric ile Otomatik İşletim Sistemi Yükseltmeleri kullanılırken, Hizmet Kumaşı'nda çalışan hizmetlerin yüksek kullanılabilirliğini korumak için yeni işletim sistemi görüntüsü, Etki Alanını Güncelleştirme tarafından Güncelleştir'den çıkar. Hizmet Kumaşında Otomatik İşletim Sistemi Yükseltmeleri'ni kullanmak için kümenizin Gümüş Dayanıklılık Katmanı'nı veya daha yüksek ini kullanacak şekilde yapılandırılması gerekir. Service Fabric kümelerinin dayanıklılık özellikleri hakkında daha fazla bilgi için lütfen [bu dokümantasyona](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster)bakın.

### <a name="keep-credentials-up-to-date"></a>Kimlik bilgilerini güncel tutma
Ölçek kümeniz, depolama hesabı için SAS belirteci kullanmak üzere yapılandırılan VM uzantısı gibi dış kaynaklara erişmek için herhangi bir kimlik bilgileri kullanıyorsa, kimlik bilgilerinin güncelleştirilmesini sağlayın. Sertifikalar ve belirteçler de dahil olmak üzere herhangi bir kimlik belgesinin süresi dolmuşsa, yükseltme başarısız olur ve ilk toplu VM'ler başarısız durumda bırakılır.

Kaynak kimlik doğrulama hatası varsa VM'leri kurtarmak ve otomatik işletim sistemi yükseltmesini yeniden etkinleştirmek için önerilen adımlar şunlardır:

* Uzantı(lar)'ınıza geçirilen belirteci (veya başka herhangi bir kimlik bilgilerini) yeniden oluşturun.
* Harici varlıklarla konuşmak için VM'nin içinden kullanılan herhangi bir kimlik bilgisinin güncel olduğundan emin olun.
* Ölçek kümesi modelindeki uzantıyı yeni belirteçlerle güncelleştirin.
* Başarısız olanlar da dahil olmak üzere tüm VM örneklerini güncelleştiren güncelleştirilmiş ölçek kümesini dağıtın.

## <a name="using-application-health-extension"></a>Uygulama Durumu uzantısını kullanma
Uygulama Durumu uzantısı sanal bir makine ölçeği kümesi örneği içinde dağıtılır ve ölçek kümesi örneği içinden VM durumu hakkında raporlar. Uzantıyı bir uygulama bitiş noktası üzerinde sonda lamak için yapılandırabilir ve uygulamanın durumunu bu örnekte güncelleştirebilirsiniz. Bu örnek durumu, bir örneğin yükseltme işlemleri için uygun olup olmadığını belirlemek için Azure tarafından denetlenir.

Uzantı bir VM içinden sağlık raporladığı için, uzantı, Uygulama Durumu Sondaları (özel Azure Yük Dengeleyici [sondaları](../load-balancer/load-balancer-custom-probe-overview.md)kullanan) gibi harici sondaların kullanılamayacağı durumlarda kullanılabilir.

Uygulama Durumu [uzantısını, bu makaledeki](virtual-machine-scale-sets-health-extension.md#deploy-the-application-health-extension)örneklerde ayrıntılı olarak açıklandığı şekilde ölçek kümelerinize dağıtmanın birden çok yolu vardır.

## <a name="get-the-history-of-automatic-os-image-upgrades"></a>Otomatik işletim sistemi görüntü yükseltmelerinin geçmişini alın
Azure PowerShell, Azure CLI 2.0 veya REST API'leri ile ölçeğinizde gerçekleştirilen en son işletim sistemi yükseltmesinin geçmişini kontrol edebilirsiniz. Son iki ay içinde son beş işletim sistemi yükseltme denemesi için geçmiş alabilirsiniz.

### <a name="rest-api"></a>REST API
Aşağıdaki örnek, *myResourceGroup*adlı kaynak grubunda *myScaleSet* adlı ölçek kümesinin durumunu kontrol etmek için [REST API](/rest/api/compute/virtualmachinescalesets/getosupgradehistory) kullanır:

```
GET on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/osUpgradeHistory?api-version=2018-10-01`
```

GET çağrısı aşağıdaki örnek çıktıya benzer özellikleri döndürür:

```json
{
    "value": [
        {
            "properties": {
        "runningStatus": {
          "code": "RollingForward",
          "startTime": "2018-07-24T17:46:06.1248429+00:00",
          "completedTime": "2018-04-21T12:29:25.0511245+00:00"
        },
        "progress": {
          "successfulInstanceCount": 16,
          "failedInstanceCount": 0,
          "inProgressInstanceCount": 4,
          "pendingInstanceCount": 0
        },
        "startedBy": "Platform",
        "targetImageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2016-Datacenter",
          "version": "2016.127.20180613"
        },
        "rollbackInfo": {
          "successfullyRolledbackInstanceCount": 0,
          "failedRolledbackInstanceCount": 0
        }
      },
      "type": "Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades",
      "location": "westeurope"
    }
  ]
}
```

### <a name="azure-powershell"></a>Azure PowerShell
Ölçek kümeniz için işletim sistemi yükseltme geçmişini kontrol etmek için [Get-AzVmss](/powershell/module/az.compute/get-azvmss) cmdlet'i kullanın. Aşağıdaki örnek, *myResourceGroup*adlı kaynak grubunda *myScaleSet* adlı bir ölçek kümesinin işletim sistemi yükseltme durumunu nasıl gözden geçirdiğinizhakkında ayrıntılı bilgi verir:

```azurepowershell-interactive
Get-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -OSUpgradeHistory
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Ölçek kümeniz için işletim sistemi yükseltme geçmişini denetlemek için [az vmss get-os-upgrade geçmişi](/cli/azure/vmss#az-vmss-get-os-upgrade-history) kullanın. Azure CLI 2.0.47 ve üzeri kullanın. Aşağıdaki örnek, *myResourceGroup*adlı kaynak grubunda *myScaleSet* adlı bir ölçek kümesinin işletim sistemi yükseltme durumunu nasıl gözden geçirdiğinizhakkında ayrıntılı bilgi verir:

```azurecli-interactive
az vmss get-os-upgrade-history --resource-group myResourceGroup --name myScaleSet
```

## <a name="how-to-get-the-latest-version-of-a-platform-os-image"></a>Nasıl bir platform işletim sistemi görüntünün en son sürümünü almak için?

Aşağıdaki örnekleri kullanarak otomatik işletim sistemi yükseltme destekli SNU'lar için kullanılabilir resim sürümlerini alabilirsiniz:

### <a name="rest-api"></a>REST API
```
GET on `/subscriptions/subscription_id/providers/Microsoft.Compute/locations/{location}/publishers/{publisherName}/artifacttypes/vmimage/offers/{offer}/skus/{skus}/versions?api-version=2018-10-01`
```

### <a name="azure-powershell"></a>Azure PowerShell
```azurepowershell-interactive
Get-AzVmImage -Location "westus" -PublisherName "Canonical" -Offer "UbuntuServer" -Skus "16.04-LTS"
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
```azurecli-interactive
az vm image list --location "westus" --publisher "Canonical" --offer "UbuntuServer" --sku "16.04-LTS" --all
```

## <a name="manually-trigger-os-image-upgrades"></a>İşletim sistemi görüntü yükseltmelerini el ile tetikler
Ölçek setinizde otomatik işletim sistemi görüntü yükseltmesi etkinken, ölçek setinizde görüntü güncelleştirmelerini el ile tetiklemeniz gerekmez. İşletim sistemi yükseltme orkestratörü, kullanılabilir en son resim sürümünü herhangi bir el ile müdahale olmaksızın ölçek seti örneklerinize otomatik olarak uygular.

Orkestratörün en son görüntüyü kullanmasını beklemek istemediğiniz belirli durumlarda, aşağıdaki örnekleri kullanarak bir işletim sistemi görüntü yükseltmesini el ile tetikleyebilirsiniz.

> [!NOTE]
> Os görüntü yükseltmelerinin manuel tetikleyicisi otomatik geri alma özellikleri sağlamaz. Bir örnek yükseltme işleminden sonra sistem durumunu kurtaramazsa, önceki işletim sistemi diski geri yüklenebilir.

### <a name="rest-api"></a>REST API
Tüm sanal makine ölçeği set örneklerini en son kullanılabilir görüntü işletim sistemi sürümüne taşımak için bir yuvarlanma yükseltmesi başlatmak için [Başlat OS Yükseltme](/rest/api/compute/virtualmachinescalesetrollingupgrades/startosupgrade) API çağrısını kullanın. Kullanılabilir en son işletim sistemi sürümünü çalıştıran örnekler etkilenmez. Aşağıdaki örnek, *myResourceGroup*adlı kaynak grubunda *myScaleSet* adlı bir ölçek kümesinde yuvarlanan işletim sistemi yükseltmesini nasıl başlatabileceğinizi ayrıntıları:

```
POST on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/osRollingUpgrade?api-version=2018-10-01`
```

### <a name="azure-powershell"></a>Azure PowerShell
Ölçek kümeniz için işletim sistemi yükseltme geçmişini kontrol etmek için [Start-AzVmssRollingOSUpgrade](/powershell/module/az.compute/Start-AzVmssRollingOSUpgrade) cmdlet'i kullanın. Aşağıdaki örnek, *myResourceGroup*adlı kaynak grubunda *myScaleSet* adlı bir ölçek kümesinde yuvarlanan işletim sistemi yükseltmesini nasıl başlatabileceğinizi ayrıntıları:

```azurepowershell-interactive
Start-AzVmssRollingOSUpgrade -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Ölçek kümeniz için işletim sistemi yükseltme geçmişini denetlemek için [az vmss yuvarlanma yükseltme başlangıcını](/cli/azure/vmss/rolling-upgrade#az-vmss-rolling-upgrade-start) kullanın. Azure CLI 2.0.47 ve üzeri kullanın. Aşağıdaki örnek, *myResourceGroup*adlı kaynak grubunda *myScaleSet* adlı bir ölçek kümesinde yuvarlanan işletim sistemi yükseltmesini nasıl başlatabileceğinizi ayrıntıları:

```azurecli-interactive
az vmss rolling-upgrade start --resource-group "myResourceGroup" --name "myScaleSet" --subscription "subscriptionId"
```

## <a name="deploy-with-a-template"></a>Şablonla dağıtma

[Ubuntu 16.04-LTS](https://github.com/Azure/vm-scale-sets/blob/master/preview/upgrade/autoupdate.json)gibi desteklenen görüntüler için otomatik işletim sistemi yükseltmeleri içeren bir ölçek kümesi dağıtmak için şablonlar kullanabilirsiniz.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fvm-scale-sets%2Fmaster%2Fpreview%2Fupgrade%2Fautoupdate.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

## <a name="next-steps"></a>Sonraki adımlar
Ölçek kümeleriyle otomatik işletim sistemi yükseltmelerinin nasıl kullanılacağına ilişkin daha fazla örnek için [GitHub repo'yu](https://github.com/Azure/vm-scale-sets/tree/master/preview/upgrade)inceleyin.
