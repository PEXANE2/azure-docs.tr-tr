---
title: Dağıtım verilerini şifreleme
description: Kapsayıcı örnek kaynaklarınız için kalıcı verilerin şifrelemesi ve verilerin müşteri tarafından yönetilen bir anahtarla nasıl şifrelenileceği hakkında bilgi edinin
ms.topic: article
ms.date: 01/17/2020
author: dkkapur
ms.author: dekapur
ms.openlocfilehash: ad232c5d9df9f6bfae3a79dbd72e2c68143be949
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79080369"
---
# <a name="encrypt-deployment-data"></a>Dağıtım verilerini şifreleme

Bulutta Azure Kapsayıcı Örnekleri (ACI) kaynaklarını çalıştırırken, ACI hizmeti kapsayıcılarınizle ilgili verileri toplar ve devam eder. ACI, bulutta kalıcı olduğunda bu verileri otomatik olarak şifreler. Bu şifreleme, kuruluşunuzun güvenlik ve uyumluluk taahhütlerini karşılamaya yardımcı olmak için verilerinizi korur. ACI ayrıca bu verileri kendi anahtarınızla şifreleme seçeneği sunarak ACI dağıtımlarınızla ilgili veriler üzerinde daha fazla denetim sağlar.

## <a name="about-aci-data-encryption"></a>ACI veri şifreleme hakkında 

ACI'daki veriler 256 bit AES şifreleme kullanılarak şifrelenir ve şifresi çözülür. Tüm ACI dağıtımları için etkinleştirilir ve bu şifrelemeden yararlanmak için dağıtımınızı veya kapsayıcılarınızı değiştirmeniz gerekmez. Buna dağıtımla ilgili meta veriler, ortam değişkenleri, kapsayıcılarınıza geçirilen anahtarlar ve kapsayıcılarınız durdurulduktan sonra günlükler kalıcı dır, böylece bunları hala görebilirsiniz. Şifreleme kapsayıcı grup performansınızı etkilemez ve şifreleme için ek bir maliyet yoktur.

## <a name="encryption-key-management"></a>Şifreleme anahtarı yönetimi

Kapsayıcı verilerinizin şifrelemesi için Microsoft tarafından yönetilen anahtarlara güvenebilir veya şifrelemeyi kendi anahtarlarınızla yönetebilirsiniz. Aşağıdaki tablo bu seçenekleri karşılaştırır: 

|    |    Microsoft tarafından yönetilen anahtarlar     |     Müşteri tarafından yönetilen anahtarlar     |
|----|----|----|
|    Şifreleme/şifre çözme işlemleri    |    Azure    |    Azure    |
|    Anahtar depolama    |    Microsoft anahtar mağazası    |    Azure Key Vault    |
|    Anahtar döndürme sorumluluğu    |    Microsoft    |    Müşteri    |
|    Anahtar erişimi    |    Yalnızca Microsoft    |    Microsoft, Müşteri    |

Belgenin geri kalanı, ACI dağıtım verilerinizi anahtarınızla (müşteri tarafından yönetilen anahtar) şifrelemek için gereken adımları kapsar. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="encrypt-data-with-a-customer-managed-key"></a>Müşteri tarafından yönetilen bir anahtarla verileri şifreleme

### <a name="create-service-principal-for-aci"></a>ACI için Hizmet Sorumlusu Oluşturma

İlk adım, [Azure kiracınızın](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) Azure Kapsayıcı Örnekleri hizmetine izin vermek üzere atanmış bir hizmet ilkesine sahip olduğundan emin olmaktır. 

> [!IMPORTANT]
> Aşağıdaki komutu çalıştırmak ve bir hizmet ilkesini başarıyla oluşturmak için, kiracınızda hizmet ilkeleri oluşturmak için izinleriniz olduğunu onaylayın.
>

Aşağıdaki CLI komutu Azure ortamınızda ACI SP'yi ayarlar:

```azurecli-interactive
az ad sp create --id 6bb8e274-af5d-4df2-98a3-4fd78b4cafd9
```

Bu komutu çalıştıran çıktı, size "displayName" ile ayarlanmış bir hizmet ilkesi ni göstermelidir: "Azure Kapsayıcı Örnek Hizmeti."

Hizmet ilkesini başarıyla oluşturamıyorsanız:
* kiracınızda bunu yapmak için izinleriniz olduğunu onaylayın
* ACI'ye dağıtmak için kiracınızda zaten bir hizmet sorumlusu nun olup olmadığını denetleyin. Bunu, bu hizmet `az ad sp show --id 6bb8e274-af5d-4df2-98a3-4fd78b4cafd9` ilkesini çalıştırarak ve yerine kullanabilirsiniz

### <a name="create-a-key-vault-resource"></a>Anahtar Kasası kaynağı oluşturma

[Azure portalı](https://docs.microsoft.com/azure/key-vault/quick-create-portal#create-a-vault), [CLI](https://docs.microsoft.com/azure/key-vault/quick-create-cli)veya [PowerShell](https://docs.microsoft.com/azure/key-vault/quick-create-powershell)kullanarak bir Azure Anahtar Kasası oluşturun. 

Anahtar kasanızın özellikleri için aşağıdaki yönergeleri kullanın: 
* Ad: Benzersiz bir ad gereklidir. 
* Abonelik: Bir abonelik seçin.
* Kaynak Grubu altında, varolan bir kaynak grubu seçin veya yeni oluşturun ve bir kaynak grubu adı girin.
* Konum çekme menüsünde bir konum seçin.
* Diğer seçenekleri varsayılanlarına bırakabilir veya ek gereksinimlere göre seçim yapabilirsiniz.

> [!IMPORTANT]
> Bir ACI dağıtım şablonunu şifrelemek için müşteri tarafından yönetilen anahtarları kullanırken, aşağıdaki iki özelliğin anahtar kasası Olan Yumuşak Silme ve Temizleme'ye ayarlanması önerilir. Bu özellikler varsayılan olarak etkinleştirilmez, ancak yeni veya varolan bir anahtar kasasında PowerShell veya Azure CLI kullanılarak etkinleştirilebilir.

### <a name="generate-a-new-key"></a>Yeni bir anahtar oluşturma 

Anahtar kasanız oluşturulduktan sonra Azure portalındaki kaynağa gidin. Kaynak bıçağının sol daki gezinti menüsünde Ayarlar'ın **altında, Tuşlar'ı**tıklatın. "Anahtarlar" görünümünde, yeni bir anahtar oluşturmak için "Oluştur/İçe" seçeneğini tıklayın. Bu anahtar için benzersiz adlar ve gereksinimlerinize göre diğer tercihleri kullanın. 

![Yeni bir anahtar oluşturma](./media/container-instances-encrypt-data/generate-key.png)

### <a name="set-access-policy"></a>Erişim ilkesini ayarlama

ACI hizmetinin Anahtarınıza erişmesine izin vermek için yeni bir erişim ilkesi oluşturun.

* Anahtarınız oluşturulduktan sonra, Ayarlar altında anahtar kasa kaynak bıçaklarınıza geri dön, **Erişim İlkeleri'ni**tıklatın.
* Anahtar kasanızın "Erişim İlkeleri" sayfasında **Erişim İlkesi Ekle'yi**tıklatın.
* Anahtar *İzinlerini* **Al** ve **Açma Anahtar** ![Kümesi anahtar izinlerini içerecek şekilde ayarlama](./media/container-instances-encrypt-data/set-key-permissions.png)
* *Müdür Seç*' için Azure Kapsayıcı **Örneği Hizmetini** seçin
* En altta **Ekle'yi** tıklatın 

Erişim ilkesi artık anahtar kasanızın erişim ilkelerinde gösterilmelidir.

![Yeni erişim ilkesi](./media/container-instances-encrypt-data/access-policy.png)

### <a name="modify-your-json-deployment-template"></a>JSON dağıtım şablonunuzu değiştirme

> [!IMPORTANT]
> Dağıtım verilerini müşteri tarafından yönetilen bir anahtarla şifrelemek, şu anda kullanıma sunulan en son API sürümünde (2019-12-01) kullanılabilir. Dağıtım şablonunuzda bu API sürümünü belirtin. Bu konuda herhangi bir sorunuz varsa, lütfen Azure Destek'e ulaşın.

Anahtar kasa anahtarı ve erişim ilkesi ayarlandıktan sonra, ACI dağıtım şablonunuza aşağıdaki özellikleri ekleyin. Öğretici'de şablonla ACI kaynaklarını dağıtma hakkında daha fazla bilgi [edinin: Kaynak Yöneticisi şablonu kullanarak çok kapsayıcılı bir grubu dağıtın.](https://docs.microsoft.com/azure/container-instances/container-instances-multi-container-group) 
* Altında `resources`, `apiVersion` `2019-12-01`ayarlayın .
* Dağıtım şablonunun kapsayıcı grup özellikleri bölümünün `encryptionProperties`altına aşağıdaki değerleri içeren bir , ekle:
  * `vaultBaseUrl`: Anahtar kasanızın DNS Adı, Portal'daki anahtar kasa kaynağının genel bakış bıçaklarında bulunabilir
  * `keyName`: daha önce oluşturulan anahtarın adı
  * `keyVersion`: anahtarın geçerli sürümü. Bu, anahtarın kendisine tıklayarak bulunabilir (anahtar kasa kaynağınızın Ayarlar bölümündeki "Tuşlar" altında)
* Kapsayıcı grup özelliklerinin altında, `sku` değeri `Standard`olan bir özellik ekleyin. Özellik `sku` API sürümünde gereklidir 2019-12-01.

Aşağıdaki şablon snippet dağıtım verilerini şifrelemek için bu ek özellikleri gösterir:

```json
[...]
"resources": [
    {
        "name": "[parameters('containerGroupName')]",
        "type": "Microsoft.ContainerInstance/containerGroups",
        "apiVersion": "2019-12-01",
        "location": "[resourceGroup().location]",    
        "properties": {
            "encryptionProperties": {
                "vaultBaseUrl": "https://example.vault.azure.net",
                "keyName": "acikey",
                "keyVersion": "xxxxxxxxxxxxxxxx"
            },
            "sku": "Standard",
            "containers": {
                [...]
            }
        }
    }
]
```

Aşağıda, [Tutorial: Resource Manager şablonu kullanarak çok kapsayıcılı bir grubu dağıtma](https://docs.microsoft.com/azure/container-instances/container-instances-multi-container-group)bölümündeşablondan uyarlanmış tam bir şablon yer alır. 

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "containerGroupName": {
      "type": "string",
      "defaultValue": "myContainerGroup",
      "metadata": {
        "description": "Container Group name."
      }
    }
  },
  "variables": {
    "container1name": "aci-tutorial-app",
    "container1image": "mcr.microsoft.com/azuredocs/aci-helloworld:latest",
    "container2name": "aci-tutorial-sidecar",
    "container2image": "mcr.microsoft.com/azuredocs/aci-tutorial-sidecar"
  },
  "resources": [
    {
      "name": "[parameters('containerGroupName')]",
      "type": "Microsoft.ContainerInstance/containerGroups",
      "apiVersion": "2019-12-01",
      "location": "[resourceGroup().location]",
      "properties": {
        "encryptionProperties": {
            "vaultBaseUrl": "https://example.vault.azure.net",
            "keyName": "acikey",
            "keyVersion": "xxxxxxxxxxxxxxxx"
        },
        "sku": "Standard",  
        "containers": [
          {
            "name": "[variables('container1name')]",
            "properties": {
              "image": "[variables('container1image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              },
              "ports": [
                {
                  "port": 80
                },
                {
                  "port": 8080
                }
              ]
            }
          },
          {
            "name": "[variables('container2name')]",
            "properties": {
              "image": "[variables('container2image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              }
            }
          }
        ],
        "osType": "Linux",
        "ipAddress": {
          "type": "Public",
          "ports": [
            {
              "protocol": "tcp",
              "port": "80"
            },
            {
                "protocol": "tcp",
                "port": "8080"
            }
          ]
        }
      }
    }
  ],
  "outputs": {
    "containerIPv4Address": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.ContainerInstance/containerGroups/', parameters('containerGroupName'))).ipAddress.ip]"
    }
  }
}
```

### <a name="deploy-your-resources"></a>Kaynaklarınızı dağıtın

Şablon dosyasını masaüstünüzde oluşturduysanız ve düzenlediyseniz, dosyayı içine sürükleyerek dosyayı Bulut Kabuğu dizininize yükleyebilirsiniz. 

[az group create][az-group-create] komutuyla bir kaynak grubu oluşturun.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

[Az grubu dağıtım oluşturma][az-group-deployment-create] komutu ile şablonu dağıtın.

```azurecli-interactive
az group deployment create --resource-group myResourceGroup --template-file deployment-template.json
```

Birkaç saniye içinde Azure’dan bir ilk yanıt almanız gerekir. Dağıtım tamamlandıktan sonra, ACI hizmeti tarafından kalıcı olarak devam eden veriler sağladığınız anahtarla şifrelenir.

<!-- LINKS - Internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
