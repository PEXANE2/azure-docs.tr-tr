---
title: Kaynakları yeni bir abonelik veya kaynak grubuna taşıma
description: Kaynakları yeni bir kaynak grubuna veya aboneye taşımak için Azure Kaynak Yöneticisi'ni kullanın.
ms.topic: conceptual
ms.date: 03/02/2020
ms.openlocfilehash: ffb5f8be81d3628084d127db404ab994d4d5b938
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80631497"
---
# <a name="move-resources-to-a-new-resource-group-or-subscription"></a>Kaynakları yeni kaynak grubuna veya aboneliğe taşıma

Bu makalede, Azure kaynaklarını aynı abonelik altında başka bir Azure aboneliğine veya başka bir kaynak grubuna nasıl taşıyabileceğinizgösterilmektedir. Azure portalı, Azure PowerShell, Azure CLI veya REST API ile kaynakları taşıyabilirsiniz.

Hareket işlemi sırasında hem kaynak grup hem de hedef grup kilitlenir. Yazma ve silme işlemleri, taşıma tamamlanana kadar kaynak gruplarında engellenir. Bu kilit, kaynak gruplarına kaynak ekleyemezsiniz, güncelleştiremezsiniz veya silemezsiniz anlamına gelir. Bu kaynakların dondurulduğu anlamına gelmez. Örneğin, bir SQL Server'ı ve veritabanını yeni bir kaynak grubuna taşırsanız, veritabanını kullanan bir uygulama da kesinti olmaz. Yine de veritabanına okuyup yazabilir. Kilit en fazla dört saat sürebilir, ancak çoğu hareket çok daha kısa sürede tamamlanabilir.

Bir kaynağı taşıdığınızda yalnızca onu yeni bir kaynak grubuna veya aboneliğe taşımış olursunuz. Bu işlem kaynağın konumunu değiştirmez.

## <a name="checklist-before-moving-resources"></a>Kaynakları taşımadan önce kullanılacak kontrol listesi

Bir kaynağı taşımadan önce gerçekleştirmeniz gereken bazı önemli adımlar vardır. Bu koşulları doğrulayarak hataları önleyebilirsiniz.

1. Taşımak istediğiniz kaynaklar taşıma işlemini desteklemelidir. Kaynakların taşınmasını destekleyen lerin listesi [için](move-support-resources.md)bkz.

1. Bazı hizmetlerin kaynakları taşınırken belirli sınırlamaları veya gereksinimleri vardır. Aşağıdaki hizmetlerden herhangi birini taşıyorsanız, taşımadan önce bu kılavuzu kontrol edin.

   * [Uygulama Hizmetleri yönlendirmeyi hareket ettirin](./move-limitations/app-service-move-limitations.md)
   * [Azure DevOps Hizmetleri yönlendirmeyi taşır](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json)
   * [Klasik dağıtım modeli taşıma kılavuzu](./move-limitations/classic-model-move-limitations.md) - Klasik İşlem, Klasik Depolama, Klasik Sanal Ağlar ve Bulut Hizmetleri
   * [Ağ taşıma kılavuzu](./move-limitations/networking-move-limitations.md)
   * [Kurtarma Hizmetleri yönlendirmeyi taşır](../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json)
   * [Sanal Makineler yönlendirmeyi hareket ettirin](./move-limitations/virtual-machines-move-limitations.md)

1. Kaynak ve hedef abonelikleri etkin olmalıdır. Devre dışı bırakılmış bir hesabı etkinleştirmekte sorun yaşıyorsanız, [bir Azure destek isteği oluşturun.](../../azure-portal/supportability/how-to-create-azure-support-request.md) Sorun türü için **Abonelik Yönetimi'ni** seçin.

1. Kaynak ve hedef abonelikleri aynı [Azure Etkin Dizin kiracı](../../active-directory/develop/quickstart-create-new-tenant.md)içinde olmalıdır. Her iki aboneliğin de aynı kiracı kimliğine sahip olup olmadığını kontrol etmek için Azure PowerShell veya Azure CLI kullanın.

   Azure PowerShell için şunları kullanın:

   ```azurepowershell-interactive
   (Get-AzSubscription -SubscriptionName <your-source-subscription>).TenantId
   (Get-AzSubscription -SubscriptionName <your-destination-subscription>).TenantId
   ```

   Azure CLI için şunu kullanın:

   ```azurecli-interactive
   az account show --subscription <your-source-subscription> --query tenantId
   az account show --subscription <your-destination-subscription> --query tenantId
   ```

   Kaynak ve hedef aboneliklerin kiracı işlemi aynı değişiklikleri aynı değişiklik teşekkür etmiyorsa, kiracı işleri uzlaştırmak için şeşer yöntemleri kullan

   * [Azure aboneliğinin sahipliğini başka bir hesaba devretme](../../billing/billing-subscription-transfer.md)
   * [Azure Active Directory'ye bir Azure aboneliğini ekleme veya ilişkilendirme](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)

1. Hedef abonelik, taşınan kaynağın kaynak sağlayıcısına kayıtlı olmalıdır. Değilse, **aboneliğin kaynak türü için kayıtlı olmadığını**belirten bir hata alırsınız. Bir kaynağı yeni bir aboneye taşıyırken bu hatayı görebilirsiniz, ancak bu abonelik bu kaynak türüyle hiç kullanılmamıştır.

   PowerShell için, kayıt durumunu almak için aşağıdaki komutları kullanın:

   ```azurepowershell-interactive
   Set-AzContext -Subscription <destination-subscription-name-or-id>
   Get-AzResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
   ```

   Bir kaynak sağlayıcısı nı kaydetmek için şunları kullanın:

   ```azurepowershell-interactive
   Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
   ```

   Azure CLI için, kayıt durumunu almak için aşağıdaki komutları kullanın:

   ```azurecli-interactive
   az account set -s <destination-subscription-name-or-id>
   az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
   ```

   Bir kaynak sağlayıcısı nı kaydetmek için şunları kullanın:

   ```azurecli-interactive
   az provider register --namespace Microsoft.Batch
   ```

1. Kaynakları hareket ettiren hesap en az aşağıdaki izinlere sahip olmalıdır:

   * **Microsoft.Resources/subscriptions/resourceGroups/moveResources/action** kaynak kaynak grubunda.
   * **Microsoft.Resources/subscriptions/resourceGroups/write** on destination resource group.

1. Kaynakları taşımadan önce, kaynakları taşımakta olduğunuz abonelik için abonelik kotalarını denetleyin. Kaynakların taşınması, aboneliğin sınırlarını aşacağı anlamına geliyorsa, kotada bir artış isteyip isteyemeyeceğiniz gözden geçirmeniz gerekir. Bir sınır listesi ve artış isteği nde bulunma hakkında bkz: [Azure abonelik ve hizmet sınırları, kotalar ve kısıtlamalar.](../../azure-resource-manager/management/azure-subscription-service-limits.md)

1. **Abonelikler arasında bir taşıma için, kaynak ve bağımlı kaynaklarının aynı kaynak grubunda bulunması ve birlikte taşınması gerekir.** Örneğin, yönetilen disklere sahip bir VM, VM ve yönetilen disklerin diğer bağımlı kaynaklarla birlikte birlikte taşınmasını gerektirir.

   Bir kaynağı yeni bir aboneye taşıyorsanız, kaynağın bağımlı kaynakları olup olmadığını ve aynı kaynak grubunda bulunup bulunmadıklarını denetleyin. Kaynaklar aynı kaynak grubunda değilse, kaynakların aynı kaynak grubunda birleştirilip birleştirilemeyeceğini denetleyin. Bu nedenle, kaynak grupları arasında bir taşıma işlemi kullanarak tüm bu kaynakları aynı kaynak grubuna getirin.

   Daha fazla bilgi [için, abonelikler arasında taşıma senaryosuna](#scenario-for-move-across-subscriptions)bakın.

## <a name="scenario-for-move-across-subscriptions"></a>Abonelikler arasında taşıma senaryosu

Kaynakları bir abonelikten diğerine taşımak üç adımlı bir işlemdir:

![çapraz abonelik taşıma senaryosu](./media/move-resource-group-and-subscription/cross-subscription-move-scenario.png)

İllüstrasyon amacıyla, yalnızca bir bağımlı kaynağımız var.

* Adım 1: Bağımlı kaynaklar farklı kaynak gruplarına dağıtılırsa, önce bunları tek bir kaynak grubuna taşıyın.
* Adım 2: Kaynak ve bağımlı kaynakları kaynak abonelikten hedef aboneye taşıyın.
* Adım 3: İsteğe bağlı olarak, bağımlı kaynakları hedef abonelik içindeki farklı kaynak gruplarına yeniden dağıtın.

## <a name="validate-move"></a>Hareketi doğrula

[Doğrula taşıma işlemi,](/rest/api/resources/resources/validatemoveresources) kaynakları gerçekten taşımadan taşıma senaryonuzu sınamamanızı sağlar. Hareketin başarılı olup olmadığını kontrol etmek için bu işlemi kullanın. Bir taşıma isteği gönderdiğinde doğrulama otomatik olarak çağrılır. Bu işlemi yalnızca sonuçları önceden belirlemeniz gerektiğinde kullanın. Bu işlemi çalıştırmak için aşağıdakileri yapmanız gerekir:

* kaynak kaynak grubunun adı
* hedef kaynak grubunun kaynak kimliği
* taşımak için her kaynağın kaynak kimliği
* hesabınız için [erişim belirteci](/rest/api/azure/#acquire-an-access-token)

Aşağıdaki isteği gönderin:

```HTTP
POST https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<source-group>/validateMoveResources?api-version=2019-05-10
Authorization: Bearer <access-token>
Content-type: application/json
```

İstek gövdesi ile:

```json
{
 "resources": ["<resource-id-1>", "<resource-id-2>"],
 "targetResourceGroup": "/subscriptions/<subscription-id>/resourceGroups/<target-group>"
}
```

İstek doğru biçimlendirilmişse, işlem döndürür:

```HTTP
Response Code: 202
cache-control: no-cache
pragma: no-cache
expires: -1
location: https://management.azure.com/subscriptions/<subscription-id>/operationresults/<operation-id>?api-version=2018-02-01
retry-after: 15
...
```

202 durum kodu doğrulama isteğinin kabul edildiğini gösterir, ancak taşıma işleminin başarılı olup olmadığını henüz belirlememiştir. Değer, `location` uzun süren işlemin durumunu denetlemek için kullandığınız bir URL içerir.  

Durumu kontrol etmek için aşağıdaki isteği gönderin:

```HTTP
GET <location-url>
Authorization: Bearer <access-token>
```

İşlem devam ederken, 202 durum kodunu almaya devam egenelolarak devam emzebilirsiniz. Yeniden denemeden önce `retry-after` değerde belirtilen saniye sayısını bekleyin. Taşıma işlemi başarıyla doğrulanırsa, 204 durum kodunu alırsınız. Taşıma doğrulaması başarısız olursa, aşağıdakiler gibi bir hata iletisi alırsınız:

```json
{"error":{"code":"ResourceMoveProviderValidationFailed","message":"<message>"...}}
```

## <a name="use-the-portal"></a>Portalı kullanma

Kaynakları taşımak için, bu kaynaklara sahip kaynak grubunu seçin ve sonra **Taşı** düğmesini seçin.

![kaynakları taşıma](./media/move-resource-group-and-subscription/select-move.png)

Kaynakları yeni bir kaynak grubuna mı yoksa yeni bir aboneye mi taşıyabileceğinizi seçin.

Taşınacak kaynakları ve hedef kaynak grubunu seçin. Bu kaynaklar için komut dosyalarını güncelleştirmeniz ve **Tamam'ı**seçmeniz gerektiğini kabul edin. Önceki adımda abonelik edin simgesini seçtiyseniz, hedef aboneliği de seçmeniz gerekir.

![hedef seçin](./media/move-resource-group-and-subscription/select-destination.png)

**Bildirimler'de,** taşıma işleminin çalıştığını görürsünüz.

![hareket durumunu göster](./media/move-resource-group-and-subscription/show-status.png)

Tamamlandığında, sonuç size bildirilir.

![taşıma sonucunu göster](./media/move-resource-group-and-subscription/show-result.png)

Bir hata alırsanız, [Azure kaynaklarını yeni kaynak grubuna veya aboneye taşıma sorunu giderme'ye](troubleshoot-move.md)bakın.

## <a name="use-azure-powershell"></a>Azure PowerShell kullanma

Varolan kaynakları başka bir kaynak grubuna veya aboneye taşımak için [Taşı-AzKaynak](/powershell/module/az.resources/move-azresource) komutunu kullanın. Aşağıdaki örnek, birkaç kaynağın yeni bir kaynak grubuna nasıl taşınır gösteriş yapılacağını gösterir.

```azurepowershell-interactive
$webapp = Get-AzResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

Yeni bir aboneye geçmek için `DestinationSubscriptionId` parametre için bir değer ekleyin.

Bir hata alırsanız, [Azure kaynaklarını yeni kaynak grubuna veya aboneye taşıma sorunu giderme'ye](troubleshoot-move.md)bakın.

## <a name="use-azure-cli"></a>Azure CLI kullanma

Varolan kaynakları başka bir kaynak grubuna veya aboneye taşımak için [az kaynak taşıma](/cli/azure/resource?view=azure-cli-latest#az-resource-move) komutunu kullanın. Taşınacak kaynakların kaynak ekinlerini sağlayın. Aşağıdaki örnek, birkaç kaynağın yeni bir kaynak grubuna nasıl taşınır gösteriş yapılacağını gösterir. Parametrede, `--ids` taşınacak kaynak iliklerinin boşluktan ayrılmış bir listesini sağlayın.

```azurecli
webapp=$(az resource show -g OldRG -n ExampleSite --resource-type "Microsoft.Web/sites" --query id --output tsv)
plan=$(az resource show -g OldRG -n ExamplePlan --resource-type "Microsoft.Web/serverfarms" --query id --output tsv)
az resource move --destination-group newgroup --ids $webapp $plan
```

Yeni bir aboneye geçmek `--destination-subscription-id` için parametreyi sağlayın.

Bir hata alırsanız, [Azure kaynaklarını yeni kaynak grubuna veya aboneye taşıma sorunu giderme'ye](troubleshoot-move.md)bakın.

## <a name="use-rest-api"></a>REST API’yi kullanma

Varolan kaynakları başka bir kaynak grubuna veya aboneye taşımak için [Kaynakları Taşı](/rest/api/resources/Resources/MoveResources) işlemini kullanın.

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

İstek gövdesinde, hedef kaynak grubunu ve taşınacak kaynakları belirtirsiniz.

```json
{
 "resources": ["<resource-id-1>", "<resource-id-2>"],
 "targetResourceGroup": "/subscriptions/<subscription-id>/resourceGroups/<target-group>"
}
```

Bir hata alırsanız, [Azure kaynaklarını yeni kaynak grubuna veya aboneye taşıma sorunu giderme'ye](troubleshoot-move.md)bakın.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**Soru: Genellikle birkaç dakika sürer benim kaynak taşıma işlemi, neredeyse bir saat tir. Bir sorun mu var?**

Kaynağı taşımak, farklı aşamaları olan karmaşık bir işlemdir. Taşımaya çalıştığınız kaynağın kaynak sağlayıcısından daha fazlasını içerebilir. Kaynak sağlayıcıları arasındaki bağımlılıklar nedeniyle, Azure Kaynak Yöneticisi işlemin tamamlanması için 4 saat izin verir. Bu süre, kaynak sağlayıcılarına geçici sorunlardan kurtarma şansı verir. Taşıma isteğiniz 4 saatlik süre içindeyse, işlem tamamlanmaya devam eder ve yine de başarılı olabilir. Kaynak ve hedef kaynak grupları tutarlılık sorunları önlemek için bu süre içinde kilitlenir.

**Soru: Kaynak grubum, kaynak taşıma sırasında neden 4 saat kilitli?**

4 saatlik pencere, kaynak taşınması için izin verilen maksimum süredir. Taşınan kaynaklarda yapılan değişiklikleri önlemek için, kaynak taşıma süresince hem kaynak hem de hedef kaynak grupları kilitlenir.

Bir hareket isteğinde iki aşama vardır. İlk aşamada, kaynak taşınır. İkinci aşamada, bildirimler taşınan kaynağa bağımlı olan diğer kaynak sağlayıcılarına gönderilir. Kaynak sağlayıcısı her iki aşamada da başarısız olduğunda, bir kaynak grubu tüm 4 saatlik pencere için kilitlenebilir. İzin verilen süre boyunca, Kaynak Yöneticisi başarısız adımı yeniden dener.

Bir kaynak 4 saatlik süre içinde taşınamazsa, Kaynak Yöneticisi her iki kaynak grubunun kilidini açar. Başarıyla taşınan kaynaklar hedef kaynak grubundadır. Taşınamayan kaynaklar kaynak kaynak grubuna bırakılır.

**Soru: Kaynak taşıma sırasında kilitlenmiş olan kaynak ve hedef kaynak gruplarının etkileri nelerdir?**

Kilit, her iki kaynak grubunu silmenizi, kaynak grubunda yeni bir kaynak oluşturmanızı veya taşımada yer alan kaynaklardan herhangi birini silmenizi engeller.

Aşağıdaki resim, bir kullanıcı devam eden bir hareketin parçası olan bir kaynak grubunu silmeye çalıştığında Azure portalından gelen bir hata iletisini gösterir.

![Silmeye çalışan hata iletisini taşıma](./media/move-resource-group-and-subscription/move-error-delete.png)

**Soru: "MissingMoveDependentResources" hata kodu ne anlama gelir?**

Bir kaynağı taşırken, bağımlı kaynakları nın hedef kaynak grubunda veya aboneliğinde bulunması veya taşıma isteğine dahil edilmesi gerekir. Bağımlı bir kaynak bu gereksinimi karşılamadığında Eksik Taşınır Bağımlı Kaynaklar hata kodunu alırsınız. Hata iletisinde taşıma isteğine dahil edilmesi gereken bağımlı kaynak la ilgili ayrıntılar vardır.

Örneğin, sanal bir makineyi taşımak, üç farklı kaynak sağlayıcısıyla yedi kaynak türünü taşımayı gerektirebilir. Bu kaynak sağlayıcıları ve türleri şunlardır:

* Microsoft.Compute

  * virtualMachines
  * Disk
* Microsoft.Network
  * ağArayüzler
  * publicIPAdresleri
  * ağSecurityGroups
  * sanalAğlar
* Microsoft.Storage
  * depolamaHesapları

Başka bir yaygın örnek sanal ağ taşıma içerir. Bu sanal ağla ilişkili birkaç başka kaynağı taşımanız gerekebilir. Taşıma isteği, ortak IP adreslerinin, rota tablolarının, sanal ağ ağ ağ geçitlerinin, ağ güvenlik gruplarının ve diğerlerinin taşınmasını gerektirebilir.

**Soru: Azure'da neden bazı kaynakları taşıyamıyorum?**

Şu anda Azure'daki tüm kaynaklar taşınmıyor. Taşımayı destekleyen kaynakların listesi [için](move-support-resources.md)bkz.

## <a name="next-steps"></a>Sonraki adımlar

Kaynakların taşınmasını destekleyen lerin listesi [için](move-support-resources.md)bkz.
