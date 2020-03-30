---
title: Azure Depolama yaşam döngüsünü yönetme
description: Eskiyen verileri Hot'tan Cool ve Archive katmanlarına geçiş yapmak için yaşam döngüsü ilkesi kurallarının nasıl oluşturulup oluşturulmayı öğrenin.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 05/21/2019
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.reviewer: yzheng
ms.openlocfilehash: 238c12baf55b525a24107a727d09588ef06a6bef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77598315"
---
# <a name="manage-the-azure-blob-storage-lifecycle"></a>Azure Blob depolama yaşam döngüsünü yönetme

Veri kümelerinin benzersiz yaşam döngüleri vardır. Yaşam döngüsünün başlarında, insanlar bazı verilere sık sık erişir. Ancak veriler yaşlandıkça erişim ihtiyacı büyük ölçüde azalır. Bazı veriler bulutta boşta kalır ve depolandıktan sonra nadiren erişilir. Bazı veriler oluşturulduktan günler veya aylar sonra sona ererken, diğer veri kümeleri yaşamları boyunca etkin bir şekilde okunur ve değiştirilir. Azure Blob depolama yaşam döngüsü yönetimi, GPv2 ve Blob depolama hesapları için zengin, kural tabanlı bir ilke sunar. Verilerinizi uygun erişim katmanlarına dönüştürmek veya verilerin yaşam döngüsünün sonunda süresi dolmak için ilkeyi kullanın.

Yaşam döngüsü yönetimi ilkesi şunları yapmanızı sağlar:

- Performans ve maliyet için optimize etmek için lekeleri daha serin bir depolama katmanına (sıcak, sıcak, arşive sıcak veya arşivlemek için serin) geçiş yapın
- Yaşam döngülerinin sonundaki lekeleri silme
- Depolama hesabı düzeyinde günde bir kez çalıştırılacak kuralları tanımlama
- Kapsayıcılara veya blobs alt kümesine kurallar uygulayın (önekleri filtre olarak kullanarak)

Yaşam döngüsünün ilk aşamalarında verilerin sık erişildiği, ancak yalnızca iki hafta sonra zaman zaman erişilen bir senaryo düşünün. İlk ayın ötesinde, veri kümesine nadiren erişilir. Bu senaryoda, sıcak depolama erken aşamalarında en iyisidir. Serin depolama zaman zaman erişim için en uygundur. Arşiv depolama, veriler bir aydan uzun bir süre geçtikten sonra en iyi katman seçeneğidir. Depolama katmanlarını veri çağına göre ayarlayarak, ihtiyaçlarınız için en ucuz depolama seçeneklerini tasarlayabilirsiniz. Bu geçişi gerçekleştirmek için, yaşam döngüsü yönetimi ilkesi kuralları, yaşlanan verileri daha serin katmanlara taşımak için kullanılabilir.

[!INCLUDE [storage-multi-protocol-access-preview](../../../includes/storage-multi-protocol-access-preview.md)]

## <a name="storage-account-support"></a>Depolama hesabı desteği

Yaşam döngüsü yönetimi ilkesi, Genel Amaç v2 (GPv2) hesapları, Blob depolama hesapları ve Premium Blok Blob depolama hesapları ile kullanılabilir. Azure portalında, varolan bir Genel Amaç (GPv1) hesabını GPv2 hesabına yükseltebilirsiniz. Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../common/storage-account-overview.md).  

## <a name="pricing"></a>Fiyatlandırma

Yaşam döngüsü yönetimi özelliği ücretsizdir. Müşterilerden [Liste Blobs](https://docs.microsoft.com/rest/api/storageservices/list-blobs) ve [Set Blob Tier](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) API aramaları için normal çalışma maliyeti tahsil edilir. Silme işlemi ücretsizdir. Fiyatlandırma hakkında daha fazla bilgi için Bkz. [Blob fiyatlandırması engelle.](https://azure.microsoft.com/pricing/details/storage/blobs/)

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik

Yaşam döngüsü yönetimi özelliği tüm Azure bölgelerinde kullanılabilir.

## <a name="add-or-remove-a-policy"></a>İlke ekleme veya kaldırma

Aşağıdaki yöntemlerden birini kullanarak bir ilke ekleyebilir, düzenlemeyapabilir veya kaldırabilirsiniz:

* [Azure portalında](https://portal.azure.com)
* [Azure PowerShell](https://github.com/Azure/azure-powershell/releases)
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)
* [REST API'leri](https://docs.microsoft.com/rest/api/storagerp/managementpolicies)

Bir ilke tam olarak okunabilir veya yazılabilir. Kısmi güncelleştirmeler desteklenmez. 

> [!NOTE]
> Depolama hesabınız için güvenlik duvarı kurallarını etkinleştiriseniz, yaşam döngüsü yönetimi istekleri engellenebilir. Güvenilen Microsoft hizmetleri için özel durumlar sağlayarak bu isteklerin engelini kaldırabilirsiniz. Daha fazla bilgi için, [Güvenlik Duvarlarını ve sanal ağları Yapılandır'daki](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions)Özel Durumlar bölümüne bakın.

Bu makalede, portal ve PowerShell yöntemlerini kullanarak politikanın nasıl yönetilengösterilen gösterir.  

# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portalı üzerinden ilke eklemenin iki yolu vardır. 

* [Azure portalı Liste görünümü](#azure-portal-list-view)
* [Azure portal Kodu görünümü](#azure-portal-code-view)

#### <a name="azure-portal-list-view"></a>Azure portalı Liste görünümü

1. [Azure portalında](https://portal.azure.com)oturum açın.

2. Azure portalında depolama hesabınızı arayın ve seçin. 

3. **Blob Hizmeti**altında, kurallarınızı görüntülemek veya değiştirmek için **Yaşam Döngüsü yönetimini** seçin.

4. Liste **görünümü** sekmesini seçin.

5. **Kuralı Ekle'yi** seçin ve ardından **Eylem kümesi** form alanlarını doldurun. Aşağıdaki örnekte, lekeler 30 gündür değiştirilmemişse serin depolama alanına taşınır.

   ![Azure portalında yaşam döngüsü yönetimi eylem seti sayfası](media/storage-lifecycle-management-concepts/lifecycle-management-action-set.png)

6. İsteğe bağlı bir filtre eklemek için **Filtre kümesi'ni** seçin. Ardından, filtre uygulayacak bir kapsayıcı ve klasör belirtmek için **Gözat'ı** seçin.

   ![Azure portalında yaşam döngüsü yönetimi filtresi ayar sayfası](media/storage-lifecycle-management-concepts/lifecycle-management-filter-set-browse.png)

8. İlke ayarlarını gözden geçirmek için **Gözden Geçir + ekle'yi** seçin.

9. Yeni ilke eklemek için **Ekle'yi** seçin.

#### <a name="azure-portal-code-view"></a>Azure portal Kodu görünümü
1. [Azure portalında](https://portal.azure.com)oturum açın.

2. Azure portalında depolama hesabınızı arayın ve seçin.

3. **Blob Service**altında, ilkenizi görüntülemek veya değiştirmek için **Yaşam Döngüsü yönetimini** seçin.

4. Aşağıdaki **JSON, Kod görünümü** sekmesine yapıştırılabilen bir ilke örneğidir.

   ```json
   {
     "rules": [
       {
         "name": "ruleFoo",
         "enabled": true,
         "type": "Lifecycle",
         "definition": {
           "filters": {
             "blobTypes": [ "blockBlob" ],
             "prefixMatch": [ "container1/foo" ]
           },
           "actions": {
             "baseBlob": {
               "tierToCool": { "daysAfterModificationGreaterThan": 30 },
               "tierToArchive": { "daysAfterModificationGreaterThan": 90 },
               "delete": { "daysAfterModificationGreaterThan": 2555 }
             },
             "snapshot": {
               "delete": { "daysAfterCreationGreaterThan": 90 }
             }
           }
         }
       }
     ]
   }
   ```

5. **Kaydet'i**seçin.

6. Bu JSON örneği hakkında daha fazla bilgi [için, İlke](#policy) ve [Kurallar](#rules) bölümlerine bakın.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Aşağıdaki PowerShell komut dosyası, depolama hesabınıza bir ilke eklemek için kullanılabilir. Değişken, `$rgname` kaynak grup adınız ile başharfe alınmalıdır. Değişken, `$accountName` depolama hesabı adınız ile başharfe bürünmelidir.

```powershell
#Install the latest module
Install-Module -Name Az -Repository PSGallery

#Initialize the following with your resource group and storage account names
$rgname = ""
$accountName = ""

#Create a new action object
$action = Add-AzStorageAccountManagementPolicyAction -BaseBlobAction Delete -daysAfterModificationGreaterThan 2555
$action = Add-AzStorageAccountManagementPolicyAction -InputObject $action -BaseBlobAction TierToArchive -daysAfterModificationGreaterThan 90
$action = Add-AzStorageAccountManagementPolicyAction -InputObject $action -BaseBlobAction TierToCool -daysAfterModificationGreaterThan 30
$action = Add-AzStorageAccountManagementPolicyAction -InputObject $action -SnapshotAction Delete -daysAfterCreationGreaterThan 90

# Create a new filter object
# PowerShell automatically sets BlobType as “blockblob” because it is the only available option currently
$filter = New-AzStorageAccountManagementPolicyFilter -PrefixMatch ab,cd

#Create a new rule object
#PowerShell automatically sets Type as “Lifecycle” because it is the only available option currently
$rule1 = New-AzStorageAccountManagementPolicyRule -Name Test -Action $action -Filter $filter

#Set the policy
$policy = Set-AzStorageAccountManagementPolicy -ResourceGroupName $rgname -StorageAccountName $accountName -Rule $rule1
```

# <a name="template"></a>[Şablon](#tab/template)

Azure Kaynak Yöneticisi şablonlarını kullanarak yaşam döngüsü yönetimini tanımlayabilirsiniz. Burada bir yaşam döngüsü yönetim ilkesi ile bir RA-GRS GPv2 depolama hesabı dağıtmak için örnek bir şablon.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {
    "storageAccountName": "[uniqueString(resourceGroup().id)]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "location": "[resourceGroup().location]",
      "apiVersion": "2019-04-01",
      "sku": {
        "name": "Standard_RAGRS"
      },
      "kind": "StorageV2",
      "properties": {
        "networkAcls": {}
      }
    },
    {
      "name": "[concat(variables('storageAccountName'), '/default')]",
      "type": "Microsoft.Storage/storageAccounts/managementPolicies",
      "apiVersion": "2019-04-01",
      "dependsOn": [
        "[variables('storageAccountName')]"
      ],
      "properties": {
        "policy": {...}
      }
    }
  ],
  "outputs": {}
}
```

---

## <a name="policy"></a>İlke

Yaşam döngüsü yönetimi ilkesi, JSON belgesindeki kuralların bir koleksiyonudur:

```json
{
  "rules": [
    {
      "name": "rule1",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {...}
    },
    {
      "name": "rule2",
      "type": "Lifecycle",
      "definition": {...}
    }
  ]
}
```

İlke, kurallar topluluğudur:

| Parametre adı | Parametre türü | Notlar |
|----------------|----------------|-------|
| `rules`        | Kural nesneleri dizisi | Bir ilkede en az bir kural gereklidir. Bir ilkede en fazla 100 kural tanımlayabilirsiniz.|

İlke içindeki her kuralın birkaç parametresi vardır:

| Parametre adı | Parametre türü | Notlar | Gerekli |
|----------------|----------------|-------|----------|
| `name`         | Dize |Kural adı en fazla 256 alfasayısal karakter içerebilir. Kural adı büyük/küçük harf duyarlıdır.  Bir ilke içinde benzersiz olmalıdır. | True |
| `enabled`      | Boole | Bir kuralın geçici olarak devre dışı edilmesine izin vermek için isteğe bağlı bir boolean. Varsayılan değer ayarlanmazsa doğrudur. | False | 
| `type`         | Enum değeri | Geçerli geçerli türü `Lifecycle`. | True |
| `definition`   | Yaşam döngüsü kuralını tanımlayan bir nesne | Her tanım bir filtre kümesi ve bir eylem kümesinden oluşur. | True |

## <a name="rules"></a>Kurallar

Her kural tanımı bir filtre kümesi ve bir eylem kümesi içerir. [Filtre,](#rule-filters) kural eylemlerini kapsayıcı veya nesne adları içindeki belirli bir nesne kümesiyle sınırlar. [Eylem kümesi,](#rule-actions) filtre uygulanmış nesne kümesine katman veya silme eylemlerini uygular.

### <a name="sample-rule"></a>Örnek kural

Aşağıdaki örnek kural, içinde `container1` bulunan ve 'ile `foo`başlayan nesnelerüzerindeki eylemleri çalıştırmak için hesabı filtreler.  

>[!NOTE]
>Yaşam döngüsü yönetimi yalnızca blok blob türünü destekler.  

- Son modifikasyondan 30 gün sonra katmanı soğutmak için katman blob
- Son değişiklikten 90 gün sonra arşivlemek için katman blob
- Son değişiklikten sonra blob 2.555 gün (yedi yıl) silin
- Anlık görüntü oluşturmadan 90 gün sonra blob anlık görüntülerini silme

```json
{
  "rules": [
    {
      "name": "ruleFoo",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ],
          "prefixMatch": [ "container1/foo" ]
        },
        "actions": {
          "baseBlob": {
            "tierToCool": { "daysAfterModificationGreaterThan": 30 },
            "tierToArchive": { "daysAfterModificationGreaterThan": 90 },
            "delete": { "daysAfterModificationGreaterThan": 2555 }
          },
          "snapshot": {
            "delete": { "daysAfterCreationGreaterThan": 90 }
          }
        }
      }
    }
  ]
}
```

### <a name="rule-filters"></a>Kural filtreleri

Filtreler, kural eylemlerini depolama hesabı içindeki bir blob alt kümesiyle sınırlandırır. Birden fazla filtre tanımlanırsa, `AND` mantıksal olarak tüm filtrelerde çalışır.

Filtreler aşağıdakileri içerir:

| Filtre adı | Filtre türü | Notlar | Gerekli |
|-------------|-------------|-------|-------------|
| blobTypes   | Önceden tanımlanmış enum değerleri dizisi. | Geçerli sürüm `blockBlob`destekler. | Evet |
| önekMaç | Öneklerin eşleşmesi için bir dizi dize. Her kural en fazla 10 önek tanımlayabilir. Önek dize bir kapsayıcı adı ile başlamalıdır. Örneğin, bir kural için altındaki `https://myaccount.blob.core.windows.net/container1/foo/...` tüm lekeleri eşleştirmek istiyorsanız, `container1/foo`önekiMatch' tir. | ÖnekMatch'i tanımlamazsanız, kural depolama hesabındaki tüm lekeler için geçerlidir.  | Hayır |

### <a name="rule-actions"></a>Kural eylemleri

Çalışma koşulu karşılandığında filtre uygulanmış blobs için eylemler uygulanır.

Yaşam döngüsü yönetimi, blob'ların katmanlama ve silinmesini ve blob anlık görüntülerinin silinmesini destekler. Blobs veya blob anlık görüntüler her kural için en az bir eylem tanımlayın.

| Eylem        | Baz Blob                                   | Anlık Görüntü      |
|---------------|---------------------------------------------|---------------|
| tiertoCool    | Şu anda sıcak katmanda destek blobs         | Desteklenmiyor |
| tierToArchive | Şu anda sıcak veya serin katmanda destek blobs | Desteklenmiyor |
| delete        | Destekleniyor                                   | Destekleniyor     |

>[!NOTE]
>Aynı blob üzerinde birden fazla eylem tanımlarsanız, yaşam döngüsü yönetimi blob için en az pahalı eylemi uygular. Örneğin, eylem `delete` eylemden `tierToArchive`daha ucuzdur. Eylem `tierToArchive` eylem `tierToCool`daha ucuzdur.

Çalışma koşulları yaşa göre yapılır. Temel lekeler, yaşı izlemek için son değiştirilen zamanı kullanır ve blob anlık görüntüleri yaşı izlemek için anlık görüntü oluşturma zamanını kullanır.

| Eylem çalıştırma koşulu             | Durum değeri                          | Açıklama                             |
|----------------------------------|------------------------------------------|-----------------------------------------|
| daysAfterModificationGreaterThan | Gün içinde yaşı gösteren birsonraki rağı değeri | Temel blob eylemleri için koşul     |
| günAfterCreationGreaterThan     | Gün içinde yaşı gösteren birsonraki rağı değeri | Blob anlık görüntü eylemleri için koşul |

## <a name="examples"></a>Örnekler

Aşağıdaki örnekler, yaşam döngüsü ilkesi kurallarıyla sık karşılaşılan senaryoların nasıl ele alınış gösteriş yapılacağını göstermektedir.

### <a name="move-aging-data-to-a-cooler-tier"></a>Eskiyen verileri daha serin bir katmana taşıyın

Bu örnek, önceden belirlenmiş blok `container1/foo` blobs `container2/bar`geçiş nasıl gösterir veya . İlke, depolamayı soğutmak için 30 gündür değiştirilmemiş blob'ları ve 90 gün içinde değiştirilmeyen lekeleri arşiv katmanına geçiriyor:

```json
{
  "rules": [
    {
      "name": "agingRule",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ],
          "prefixMatch": [ "container1/foo", "container2/bar" ]
        },
        "actions": {
          "baseBlob": {
            "tierToCool": { "daysAfterModificationGreaterThan": 30 },
            "tierToArchive": { "daysAfterModificationGreaterThan": 90 }
          }
        }
      }
    }
  ]
}
```

### <a name="archive-data-after-ingest"></a>Verileri aldıktan sonra arşivleme

Bazı veriler bulutta boşta kalır ve depolandıktan sonra nadiren erişilir. Aşağıdaki yaşam döngüsü ilkesi, yutuldükten kısa bir süre sonra verileri arşivlemek üzere yapılandırılır. Bu örnek, kapsayıcı `archivecontainer` içindeki depolama hesabındaki blok lekelerini bir arşiv katmanına dönüştürür. Geçiş, son değiştirilen zamandan 0 gün sonra blobs üzerinde hareket ederek gerçekleştirilir:

> [!NOTE] 
> Daha verimli olması için lekelerinizin doğrudan arşiv katmanına yüklenmesi önerilir. REST sürüm 2018-11-09 ve daha yeni veya en son blob depolama istemci kitaplıkları ile [PutBlob](https://docs.microsoft.com/rest/api/storageservices/put-blob) veya [PutBlockList](https://docs.microsoft.com/rest/api/storageservices/put-block-list) için x-ms-acess-tier başlığı kullanabilirsiniz. 

```json
{
  "rules": [
    {
      "name": "archiveRule",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ],
          "prefixMatch": [ "archivecontainer" ]
        },
        "actions": {
          "baseBlob": {
              "tierToArchive": { "daysAfterModificationGreaterThan": 0 }
          }
        }
      }
    }
  ]
}

```

### <a name="expire-data-based-on-age"></a>Yaşa göre verilerin süresinin dolması

Bazı verilerin oluşturulduktan günler veya aylar sonra süresinin dolması beklenir. Veri çağına göre silinerek verileri sona erdirecek bir yaşam döngüsü yönetim ilkesini yapılandırabilirsiniz. Aşağıdaki örnek, 365 günden eski tüm blok blobs siler bir ilke gösterir.

```json
{
  "rules": [
    {
      "name": "expirationRule",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ]
        },
        "actions": {
          "baseBlob": {
            "delete": { "daysAfterModificationGreaterThan": 365 }
          }
        }
      }
    }
  ]
}
```

### <a name="delete-old-snapshots"></a>Eski anlık görüntüleri silme

Kullanım ömrü boyunca düzenli olarak değiştirilen ve erişilen veriler için anlık görüntüler genellikle verilerin eski sürümlerini izlemek için kullanılır. Anlık görüntü yaşına bağlı olarak eski anlık görüntüleri silen bir ilke oluşturabilirsiniz. Anlık görüntü oluşturma zamanı değerlendirilerek anlık oluşturma yaşı belirlenir. Bu ilke kuralı, anlık görüntü `activedata` oluşturmadan sonra 90 gün veya daha eski olan kapsayıcı içindeki blob anlık görüntülerini siler.

```json
{
  "rules": [
    {
      "name": "snapshotRule",
      "enabled": true,
      "type": "Lifecycle",
    "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ],
          "prefixMatch": [ "activedata" ]
        },
        "actions": {
          "snapshot": {
            "delete": { "daysAfterCreationGreaterThan": 90 }
          }
        }
      }
    }
  ]
}
```

## <a name="faq"></a>SSS

**Yeni bir ilke oluşturdum, eylemler neden hemen çalışmıyor?**  
Platform, yaşam döngüsü ilkesini günde bir kez çalıştırır. Bir ilkeyi yapılandırdıktan sonra, bazı eylemlerin ilk kez çalışması 24 saat kadar sürebilir.  

**Varolan bir ilkeyi güncellersem, eylemlerin çalışması ne kadar sürer?**  
Güncelleştirilmiş ilkenin yürürlüğe girmesi 24 saat kadar sürer. İlke yürürlüğe girdiğinde, eylemlerin çalışması 24 saat kadar sürebilir. Bu nedenle, ilke eylemlerinin tamamlanması 48 saat kadar sürebilir.   

**Arşivlenmiş bir lekeyi elle yeniden sulandırdım, geçici olarak Arşiv katmanına geri taşınmasını nasıl önleyebilirim?**  
Bir blob bir erişim katmanından diğerine taşındığında, son değişiklik süresi değişmez. Arşivlenmiş bir lekeyi sıcak katmana elle yeniden sulandırırsanız, yaşam döngüsü yönetim motoru tarafından arşiv katmanına geri taşınır. Yeniden arşivlenmesini önlemek için bu lekeyi geçici olarak etkileyen kuralı devre dışı dışı kakın. Blob güvenli bir şekilde arşiv katmanına geri taşınabildiği zaman kuralı yeniden etkinleştirin. Ayrıca, sıcak veya serin katmanda kalıcı olarak kalması gerekiyorsa, lekeyi başka bir konuma kopyalayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Yanlışlıkla silme işleminden sonra verileri nasıl kurtarış edin:

- [Azure Depolama blobları için geçici silme](../blobs/storage-blob-soft-delete.md)
