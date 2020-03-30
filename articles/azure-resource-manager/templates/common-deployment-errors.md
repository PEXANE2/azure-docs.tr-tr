---
title: Sık karşılaşılan dağıtım hatalarını giderme
description: Azure Kaynak Yöneticisi'ni kullanarak kaynakları Azure'a dağıtırken sık karşılaşılan hataların nasıl çözüleceğini açıklar.
tags: top-support-issue
ms.topic: troubleshooting
ms.date: 10/04/2019
ms.openlocfilehash: bc1568c53cdb5518f694d77a2f28f3cf77296ee2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79460390"
---
# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Azure Resource Manager'la yaygın Azure dağıtım hatalarını giderme

Bu makalede, bazı yaygın Azure dağıtım hataları açıklanır ve hataları gidermek için bilgi sağlar. Dağıtım hatanızın hata kodunu bulamıyorsanız, [bkz.](#find-error-code)

Bir hata kodu hakkında bilgi arıyorsanız ve bu bilgiler bu makalede sağlanmadıysa, bize bildirin. Bu sayfanın alt kısmında, geribildirim bırakabilirsiniz. Geri bildirim GitHub Sorunları ile izlenir.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="error-codes"></a>Hata kodları

| Hata kodu | Risk azaltma | Daha fazla bilgi |
| ---------- | ---------- | ---------------- |
| AccountNameInvalid | Depolama hesapları için adlandırma kısıtlamalarını izleyin. | [Depolama hesabı adını çözme](error-storage-account-name.md) |
| AccountPropertyCannotbeset | Kullanılabilir depolama hesabı özelliklerini kontrol edin. | [depolamaHesapları](/azure/templates/microsoft.storage/storageaccounts) |
| Tahsisat Başarısız Oldu | Küme veya bölge kullanılabilir kaynaklara sahip değildir veya istenen VM boyutunu destekleyemez. İsteği daha sonra yeniden deneyin veya farklı bir VM boyutu isteyin. | [Linux için sağlama ve ayırma sorunları](../../virtual-machines/linux/troubleshoot-deployment-new-vm.md), Windows ve Sorun [Giderme hataları](../../virtual-machines/troubleshooting/allocation-failure.md) için sağlama ve ayırma [sorunları](../../virtual-machines/windows/troubleshoot-deployment-new-vm.md)|
| AnotherOperationInProgress | Eşzamanlı işlemin tamamlanmasını bekleyin. | |
| Yetkilendirme Başarısız Oldu | Hesabınız veya hizmet sorumlusunuz dağıtımı tamamlamak için yeterli erişime sahip değil. Hesabınızın ait olduğu rolü ve dağıtım kapsamına erişimini denetleyin.<br><br>Gerekli bir kaynak sağlayıcısı kayıtlı olmadığında bu hatayı alabilirsiniz. | [Azure Rol Tabanlı Erişim Denetimi](../../role-based-access-control/role-assignments-portal.md)<br><br>[Kaydı çözme](error-register-resource-provider.md) |
| BadRequest | Kaynak Yöneticisi'nin beklediğiyle eşleşmeyan dağıtım değerleri gönderdiniz. Sorun giderme yle ilgili yardım için iç durum iletisini denetleyin. | [Şablon başvurusu](/azure/templates/) ve [Desteklenen konumlar](resource-location.md) |
| Çakışma | Kaynağın geçerli durumunda izin verilmeyen bir işlem isteğinde bulunun. Örneğin, disk boyutlandırmaya yalnızca VM oluşturulurken veya VM tahsis edildiğinde izin verilir. | |
| DeploymentActiveAndUneditable | Bu kaynak grubuna eşzamanlı dağıtımın tamamlanmasını bekleyin. | |
| DağıtımBaşarısız CleanUp | Tam modda dağıtıladiğinizde, şablonda olmayan tüm kaynaklar silinir. Şablonda olmayan tüm kaynakları silmek için yeterli izine sahip olmadığınız zaman bu hatayı alırsınız. Hatayı önlemek için dağıtım modunu artımlı olarak değiştirin. | [Azure Kaynak Yöneticisi dağıtım modları](deployment-modes.md) |
| DeploymentNameGeçersiz Karakterler | Dağıtım adı yalnızca harf, basamak, '-', '.' veya '_' içerebilir. | |
| DeploymentNameLengthLimitExceeded | Dağıtım adları 64 karakterle sınırlıdır.  | |
| DağıtımBaşarısız | DeploymentFailed hatası, hatayı çözmek için gereken ayrıntıları sağlamayan genel bir hatadır. Daha fazla bilgi sağlayan bir hata kodu için hata ayrıntılarına bakın. | [Hata kodunu bulma](#find-error-code) |
| DeploymentQuotaExceeded | Kaynak grubu başına 800 dağıtım sınırına ulaşırsanız, artık gerekmeden gelen dağıtımları geçmişten silin. | [Dağıtım sayısı 800'ü aştığında hatayı giderme](deployment-quota-exceeded.md) |
| DnsRecordInUse | DNS kayıt adı benzersiz olmalıdır. Farklı bir ad girin. | |
| ImageNotFound | VM görüntü ayarlarını kontrol edin. |  |
| InUseSubnetCannotSilinmez | Bir kaynağı güncelleştirmeye çalışırken bu hatayı alabilirsiniz ve istek, kaynağı silerek ve oluşturularak işlenir. Tüm değişmemiş değerleri belirttiğinden emin olun. | [Güncelleştirme kaynağı](/azure/architecture/building-blocks/extending-templates/update-resource) |
| Geçersiz AuthenticationTokenTenant | Uygun kiracı için erişim jetonu alın. Jetonu yalnızca hesabınızın ait olduğu kiracıdan alabilirsiniz. | |
| GeçersizİçerikBağlantısı | Büyük olasılıkla kullanılamayan iç içe bir şablona bağlanmayı denediniz. İç içe şablon için sağladığınız URI'yi iki kez kontrol edin. Şablon bir depolama hesabında varsa, URI'ye erişilebilir olduğundan emin olun. Bir SAS jetonu geçirmeniz gerekebilir. Şu anda, Azure [Depolama güvenlik duvarının](../../storage/common/storage-network-security.md)arkasındaki bir depolama hesabında bulunan bir şablona bağlanamazsınız. Şablonunuzu GitHub gibi başka bir depoya taşımayı düşünün. | [Bağlı şablonlar](linked-templates.md) |
| GeçersizDeploymentLocation | Abonelik düzeyinde dağıtım yaparken, daha önce kullanılmış bir dağıtım adı için farklı bir konum sağladınız. | [Abonelik düzeyi dağıtımları](deploy-to-subscription.md) |
| GeçersizParametre | Kaynak için sağladığınız değerlerden biri beklenen değerle eşleşmiyor. Bu hata birçok farklı koşuldan kaynaklanabilir. Örneğin, bir parola yetersiz olabilir veya bir blob adı yanlış olabilir. Hata iletisi, hangi değerin düzeltilmesi gerektiğini belirtmelidir. | |
| Geçersizİstekİçerik | Dağıtım değerleri, tanınmayan veya gerekli değerler eksik olan değerleri içerir. Kaynak türünüz için değerleri onaylayın. | [Şablon başvurusu](/azure/templates/) |
| GeçersizİstekFormat | Dağıtımı çalıştırırken hata ayıklama günlamasını etkinleştirin ve isteğin içeriğini doğrulayın. | [Hata ayıklama günlüğü](#enable-debug-logging) |
| GeçersizKaynakNamespace | **Tür** özelliğinde belirttiğiniz kaynak ad alanını denetleyin. | [Şablon başvurusu](/azure/templates/) |
| GeçersizKaynakBaşvurusu | Kaynak henüz yok veya yanlış başvurulmuyor. Bağımlılık eklemeniz gerekip gerekmediğini denetleyin. **Başvuru** işlevini kullanımınızın senaryonuz için gerekli parametreleri içerdiğini doğrulayın. | [Bağımlılıkları çözümle](error-not-found.md) |
| GeçersizKaynak Türü | **Tür** özelliğinde belirttiğiniz kaynak türünü denetleyin. | [Şablon başvurusu](/azure/templates/) |
| GeçersizAbonelikRegistrationState | Aboneliğinizi kaynak sağlayıcısına kaydedin. | [Kaydı çözme](error-register-resource-provider.md) |
| InvalidTemplate | Şablon sözdizimini hatalariçin denetleyin. | [Geçersiz şablonu çözümle](error-invalid-template.md) |
| GeçersizTemplateCircularDependency | Gereksiz bağımlılıkları kaldırın. | [Dairesel bağımlılıkları çözümle](error-invalid-template.md#circular-dependency) |
| LinkedAuthorizationFailed | Hesabınızın, dağıtmakta olduğunuz kaynak grubuyla aynı kiracıya ait olup olmadığını kontrol edin. | |
| LinkedInvalidPropertyId | Bir kaynağın kaynak kimliği doğru çözülmüyor. Abonelik kimliği, kaynak grubu adı, kaynak türü, üst kaynak adı (gerekirse) ve kaynak adı da dahil olmak üzere kaynak kimliği için gerekli tüm değerleri sağladığınızı denetleyin. | |
| Gerekli Konum | Kaynak için bir konum sağlayın. | [Konum ayarlama](resource-location.md) |
| Yanlış EşleştirmeKaynak Segmentleri | İç içe bulunan kaynağın ad ve tür olarak doğru sayıda parçaya sahip olduğundan emin olun. | [Kaynak segmentlerini çözümle](error-invalid-template.md#incorrect-segment-lengths)
| Eksik KayıtForLocation | Kaynak sağlayıcı kayıt durumunu ve desteklenen konumları denetleyin. | [Kaydı çözme](error-register-resource-provider.md) |
| Eksik Abonelik Kaydı | Aboneliğinizi kaynak sağlayıcısına kaydedin. | [Kaydı çözme](error-register-resource-provider.md) |
| NoRegisteredProviderFound | Kaynak sağlayıcı kayıt durumunu denetleyin. | [Kaydı çözme](error-register-resource-provider.md) |
| NotFound | Bir üst kaynağa paralel olarak bağımlı bir kaynak dağıtmaya çalışıyor olabilirsiniz. Bağımlılık eklemeniz gerekip gerekip gerekmeden kontrol edin. | [Bağımlılıkları çözümle](error-not-found.md) |
| İşleme İzin Verilmedi | Dağıtım, abonelik, kaynak grubu veya bölge kotasını aşan bir işlem dener. Mümkünse, kotalar içinde kalmak için dağıtımınızı gözden geçirin. Aksi takdirde, kotalarınızda değişiklik isteğinde bulunmayı düşünün. | [Kotaları çözme](error-resource-quota.md) |
| ParentResourceNotFound | Alt kaynak oluşturmadan önce bir üst kaynağın bulunduğundan emin olun. | [Üst kaynağı çözümle](error-parent-resource.md) |
| ŞifreTooLong | Çok fazla karakteriçeren bir parola seçmiş veya parola değerinizi parametre olarak geçirmeden önce güvenli bir dizeye dönüştürebilirsiniz. Şablon da güvenli bir **dize** parametresi içeriyorsa, değeri güvenli bir dize dönüştürmeniz gerekmez. Parola değerini metin olarak girin. |  |
| PrivateIPAddressInreservedRange | Belirtilen IP adresi, Azure tarafından gereken bir adres aralığı içerir. Ayrılmış aralıktan kaçınmak için IP adresini değiştirin. | [IP adresleri](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) |
| PrivateIPAddressNotInSubnet | Belirtilen IP adresi alt net aralığının dışındadır. IP adresini alt ağ aralığına düşecek şekilde değiştirin. | [IP adresleri](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) |
| Özellik Değişikliğine İzin Verilmedi | Dağıtılan bir kaynakta bazı özellikler değiştirilemez. Bir kaynağı güncellerken, değişikliklerinizi izin verilen özelliklerle sınırlayın. | [Güncelleştirme kaynağı](/azure/architecture/building-blocks/extending-templates/update-resource) |
| RequestDisallowedByPolicy | Aboneliğiniz, dağıtım sırasında gerçekleştirmeye çalıştığınız bir eylemi engelleyen bir kaynak ilkesi içerir. Eylemi engelleyen ilkeyi bulun. Mümkünse, ilkeden gelen sınırlamaları karşılamak için dağıtımınızı değiştirin. | [İlkeleri çözümle](error-policy-requestdisallowedbypolicy.md) |
| ReservedResourceName | Ayrılmış bir ad içermeyen bir kaynak adı sağlayın. | [Ayrılmış kaynak adları](error-reserved-resource-name.md) |
| Kaynak GrubuSilinen | Silme işleminin tamamlanmasını bekleyin. | |
| Kaynak GroupNotFound | Dağıtım için hedef kaynak grubunun adını denetleyin. Hedef kaynak grubunun aboneliğinizde zaten bulunması gerekir. Abonelik bağlamınızı kontrol edin. | [Azure CLI](/cli/azure/account?#az-account-set) [PowerShell](/powershell/module/Az.Accounts/Set-AzContext) |
| Kaynak Bulunamadı | Dağıtımınız çözülmeyen bir kaynağa başvurur. **Başvuru** işlevini kullanımınızın senaryonuz için gerekli parametreleri içerdiğini doğrulayın. | [Başvuruları çözümle](error-not-found.md) |
| ResourceQuotaExceeded | Dağıtım, abonelik, kaynak grubu veya bölge kotasını aşan kaynaklar oluşturmaya çalışıyor. Mümkünse, kotalar içinde kalmak için altyapınızı gözden geçirin. Aksi takdirde, kotalarınızda değişiklik isteğinde bulunmayı düşünün. | [Kotaları çözme](error-resource-quota.md) |
| SkuNotAvailable | Seçtiğiniz konum için kullanılabilen SKU'yu (VM boyutu gibi) seçin. | [SKU'ya çözüm](error-sku-not-available.md) |
| Depolama Hesabı Zaten Var | Depolama hesabına benzersiz bir ad verin. | [Depolama hesabı adını çözme](error-storage-account-name.md)  |
| Depolama Hesabı Zaten Alındı | Depolama hesabına benzersiz bir ad verin. | [Depolama hesabı adını çözme](error-storage-account-name.md) |
| DepoHesabı Bulunamadı | Kullanmaya çalıştığınız depolama hesabının aboneliğini, kaynak grubunu ve adını denetleyin. | |
| SubnetsNotInSameVnet | Sanal bir makinenin yalnızca bir sanal ağı olabilir. Birkaç NIC dağıtırken, aynı sanal ağa ait olduğundan emin olun. | [Birden Fazla NIC](../../virtual-machines/windows/multiple-nics.md) |
| AbonelikKayıtlı Değil | Ağ kaynaklarını dağıtırken, Microsoft.Network kaynak sağlayıcısı aboneye otomatik olarak kaydedilir. Bazen otomatik kayıt zamanında tamamlanmaz. Bu aralıklı hatayı önlemek için dağıtımdan önce Microsoft.Network kaynak sağlayıcısını kaydedin. | [Kaydı çözme](error-register-resource-provider.md) |
| ŞablonKaynakDaire Bağımlılık | Gereksiz bağımlılıkları kaldırın. | [Dairesel bağımlılıkları çözümle](error-invalid-template.md#circular-dependency) |
| TooManyTargetResourceGroups | Tek bir dağıtım için kaynak gruplarının sayısını azaltın. | [Çapraz kaynak grubu dağıtımı](cross-resource-group-deployment.md) |

## <a name="find-error-code"></a>Hata kodunu bulma

İki tür hata alabilirsiniz:

* doğrulama hataları
* dağıtım hataları

Doğrulama hataları dağıtım öncesinde saptanabilen senaryolardan kaynaklanır. Bunlar şablonunuzdaki söz dizimi hataları veya abonelik kotalarınızı aşabilecek kaynak dağıtımı denemeleri olabilir. Dağıtım hataları, dağıtım işlemi sırasında oluşan koşullardan kaynaklanır. Bu paralel olarak dağıtılan bir kaynağa erişme denemesi olabilir.

Her iki tür hata da dağıtım sorunlarını gidermek için kullanabileceğiniz bir hata kodu döndürür. Her iki tür hata da [etkinlik günlüğünde](../management/view-activity-logs.md) görüntülenir. Öte yandan doğrulama hataları dağıtım geçmişinizde görüntülenmez çünkü dağıtım hiç başlatılmamıştır.

### <a name="validation-errors"></a>Doğrulama hataları

Portal üzerinden dağıtım yaparken değerlerinizi gönderdikten sonra doğrulama hatasını görürsünüz.

![portal doğrulama hatasini göster](./media/common-deployment-errors/validation-error.png)

Ayrıntıları görmek için hatayı seçin. Aşağıdaki resimde, geçersiz **şablon dağıtımı** hatası ve ilke engellenen dağıtımı gösteren bir ileti görürsünüz.

![doğrulama ayrıntılarını göster](./media/common-deployment-errors/validation-details.png)

### <a name="deployment-errors"></a>Dağıtım hataları

İşlem doğrulamayı geçer ama dağıtım sırasında başarısız olursa dağıtım hatası alırsınız.

PowerShell'le dağıtım hata kodlarını ve iletileri görmek için şunu kullanın:

```azurepowershell-interactive
(Get-AzResourceGroupDeploymentOperation -DeploymentName exampledeployment -ResourceGroupName examplegroup).Properties.statusMessage
```

Azure CLI ile dağıtım hata kodlarını ve iletileri görmek için şunu kullanın:

```azurecli-interactive
az deployment group operation list --name exampledeployment -g examplegroup --query "[*].properties.statusMessage"
```

Portalda bildirimi seçin.

![bildirim hatası](./media/common-deployment-errors/notification.png)

Dağıtım hakkında daha fazla ayrıntı görürsünüz. Hata hakkında daha fazla bilgi bulmak için seçeneği belirtin.

![Dağıtım başarısız oldu](./media/common-deployment-errors/deployment-failed.png)

Hata iletisini ve hata kodlarını görürsünüz. İki hata kodu olduğuna dikkat edin. İlk hata kodu (**DeploymentFailed**), hataya çözmek için ihtiyacınız olan ayrıntıları sağlamayan genel bir hatadır. İkinci hata kodu (**StorageAccountNotFound**) ihtiyacınız olan ayrıntıları sağlar.

![hata ayrıntıları](./media/common-deployment-errors/error-details.png)

## <a name="enable-debug-logging"></a>Hata ayıklama günlüğe kaydetmeyi etkinleştirme

Bazen neyin yanlış gittiğini öğrenmek için istek ve yanıt hakkında daha fazla bilgiye ihtiyacınız olabilir. Dağıtım sırasında, dağıtım sırasında ek bilgilerin günlüğe kaydedilmesini isteyebilirsiniz.

### <a name="powershell"></a>PowerShell

PowerShell'de **DeploymentDebugLogLevel** parametresini Tümü, Yanıt İçeriği veya RequestContent olarak ayarlayın.

```powershell
New-AzResourceGroupDeployment `
  -Name exampledeployment `
  -ResourceGroupName examplegroup `
  -TemplateFile c:\Azure\Templates\storage.json `
  -DeploymentDebugLogLevel All
```

İstek içeriğini aşağıdaki cmdlet ile inceleyin:

```powershell
(Get-AzResourceGroupDeploymentOperation `
-DeploymentName exampledeployment `
-ResourceGroupName examplegroup).Properties.request `
| ConvertTo-Json
```

Veya aşağıdakilerle yanıt içeriği:

```powershell
(Get-AzResourceGroupDeploymentOperation `
-DeploymentName exampledeployment `
-ResourceGroupName examplegroup).Properties.response `
| ConvertTo-Json
```

Bu bilgiler, şablondaki bir değerin yanlış ayarlanıp ayarlanmadığını belirlemenize yardımcı olabilir.

### <a name="azure-cli"></a>Azure CLI

Şu anda Azure CLI hata ayıklama günlemesi açmayı desteklemez, ancak hata ayıklama günlemesi'ni alabilirsiniz.

Dağıtım işlemlerini aşağıdaki komutla inceleyin:

```azurecli
az deployment group operation list \
  --resource-group examplegroup \
  --name exampledeployment
```

İstek içeriğini aşağıdaki komutla inceleyin:

```azurecli
az deployment group operation list \
  --name exampledeployment \
  -g examplegroup \
  --query [].properties.request
```

Yanıt içeriğini aşağıdaki komutla inceleyin:

```azurecli
az deployment group operation list \
  --name exampledeployment \
  -g examplegroup \
  --query [].properties.response
```

### <a name="nested-template"></a>İç içe şablon

İç içe bir şablon için hata ayıklama bilgilerini günlüğe kaydetmek için **hata ayıklama** öğesini kullanın.

```json
{
  "type": "Microsoft.Resources/deployments",
  "apiVersion": "2016-09-01",
  "name": "nestedTemplate",
  "properties": {
    "mode": "Incremental",
    "templateLink": {
      "uri": "{template-uri}",
      "contentVersion": "1.0.0.0"
    },
    "debugSetting": {
       "detailLevel": "requestContent, responseContent"
    }
  }
}
```

## <a name="create-a-troubleshooting-template"></a>Sorun giderme şablonu oluşturma

Bazı durumlarda, şablonunuzu gidermenin en kolay yolu şablonunuzun parçalarını sınamaktır. Hataya neden olduğuna inandığınız bölüme odaklanmanızı sağlayan basitleştirilmiş bir şablon oluşturabilirsiniz. Örneğin, bir kaynağa başvururken bir hata aldığınızı varsayalım. Tüm şablonla uğraşmak yerine, soruna neden olabilecek bölümü döndüren bir şablon oluşturun. Doğru parametreleri geçirip geçirmediğinizi, şablon işlevlerini doğru şekilde kullanıp kullanmadığınızı ve beklediğiniz kaynağı alıp almadığınızı belirlemenize yardımcı olabilir.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
  "storageName": {
    "type": "string"
  },
  "storageResourceGroup": {
    "type": "string"
  }
  },
  "variables": {},
  "resources": [],
  "outputs": {
  "exampleOutput": {
    "value": "[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageName')), '2016-05-01')]",
    "type" : "object"
  }
  }
}
```

Veya, yanlış ayarlanmış bağımlılıklarla ilişkili olduğunu inandığınız dağıtım hataları aldığınızı varsayalım. Şablonunuzu basitleştirilmiş şablonlara ayırarak test edin. İlk olarak, yalnızca tek bir kaynak (SQL Server gibi) dağıtan bir şablon oluşturun. Bu kaynağıdoğru tanımladığınıza emin olduğunuzda, kaynağına bağlı bir kaynak ekleyin (SQL Veritabanı gibi). Bu iki kaynağı doğru şekilde tanımladığınız zaman, diğer bağımlı kaynakları (denetim ilkeleri gibi) ekleyin. Her test dağıtımı arasında, bağımlılıkları yeterince sınadığınızdan emin olmak için kaynak grubunu silin.

## <a name="next-steps"></a>Sonraki adımlar

* Sorun giderme öğreticisine geçmek için [Bkz. Öğretici: Kaynak Yöneticisi şablondağıtımları sorun giderme](template-tutorial-troubleshoot.md)
* Denetim eylemleri hakkında bilgi edinmek için [Kaynak Yöneticisi ile Denetim işlemlerine](../management/view-activity-logs.md)bakın.
* Dağıtım sırasında hataları belirlemek için eylemler hakkında bilgi edinmek için [bkz.](deployment-history.md)
