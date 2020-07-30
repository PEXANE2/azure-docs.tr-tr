---
title: Azure HDInsight için müşteri tarafından yönetilen anahtar disk şifrelemesi
description: Bu makalede, Azure HDInsight kümelerinde yönetilen disklerde depolanan verileri şifrelemek için Azure Key Vault ' den kendi şifreleme anahtarınızın nasıl kullanılacağı açıklanır.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/15/2020
ms.openlocfilehash: a8bb9dc5aa6ebbd4ef7fb1b9550670a3c6298333
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87387855"
---
# <a name="customer-managed-key-disk-encryption"></a>Müşteri tarafından yönetilen anahtar disk şifrelemesi

Azure HDInsight, yönetilen disklerde ve HDInsight kümesi sanal makinelerine eklenen kaynak disklerinde bulunan veriler için müşterinin yönettiği anahtar şifrelemesini destekler. Bu özellik, HDInsight kümelerinizde bekleyen verilerin güvenliğini sağlayan şifreleme anahtarlarını yönetmek için Azure Key Vault kullanmanıza olanak sağlar.

HDInsight 'taki tüm yönetilen diskler Azure Depolama Hizmeti Şifrelemesi (SSE) ile korunmaktadır. Varsayılan olarak, bu disklerdeki veriler Microsoft tarafından yönetilen anahtarlar kullanılarak şifrelenir. HDInsight için müşteri tarafından yönetilen anahtarları etkinleştirirseniz, HDInsight için şifreleme anahtarlarını kullanarak bu anahtarları kullanabilir ve Azure Key Vault aracılığıyla yönetebilirsiniz.

Bu belge, Azure depolama hesabınızda depolanan verileri gidermez. Azure depolama şifrelemesi hakkında daha fazla bilgi için bkz. [bekleyen veriler Için Azure depolama şifrelemesi](../storage/common/storage-service-encryption.md). Kümelerinizde, şifreleme anahtarlarının Microsoft tarafından yönetilen veya müşteri tarafından yönetilen olabileceği, ancak şifreleme hizmeti farklı olan bir veya daha fazla bağlı Azure depolama hesabı olabilir.

## <a name="introduction"></a>Giriş

Müşteri tarafından yönetilen anahtar şifrelemesi, küme oluşturma sırasında ek bir ücret ödemeden işlenen tek adımlı bir işlemdir. Yapmanız gereken tek şey, HDInsight 'ı Azure Key Vault ile yönetilen kimlik olarak kaydeder ve kümenizi oluştururken şifreleme anahtarını eklemektir.

Kümenin her bir düğümündeki kaynak disk ve yönetilen diskler, simetrik bir veri şifreleme anahtarı (DEK) ile şifrelenir. DEK, anahtar kasaınızdan anahtar şifreleme anahtarı (KEK) kullanılarak korunmaktadır. Şifreleme ve şifre çözme işlemleri tamamen Azure HDInsight tarafından işlenir.

Anahtar Kasası güvenlik duvarı, disk şifreleme anahtarının depolandığı anahtar kasasında etkinleştirilirse, kümenin dağıtılacağı bölgenin HDInsight bölgesel kaynak sağlayıcısı IP adresleri Anahtar Kasası Güvenlik Duvarı yapılandırmasına eklenmelidir. HDInsight güvenilen bir Azure Anahtar Kasası hizmeti olmadığından, bu gereklidir.

Anahtar kasasındaki anahtarları güvenle döndürmek için Azure portal veya Azure CLı kullanabilirsiniz. Bir anahtar döndürülerek HDInsight kümesi yeni anahtarı dakikalar içinde kullanmaya başlar. Fidye yazılımı senaryolarına ve yanlışlıkla silinmeye karşı koruma sağlamak için [geçici silme](../key-vault/general/soft-delete-overview.md) anahtar koruma özelliklerini etkinleştirin. Bu koruma özelliği olmadan anahtar kasaları desteklenmez.

|Küme türü |İşletim sistemi diski (yönetilen disk) |Veri diski (yönetilen disk) |Geçici veri diski (yerel SSD) |
|---|---|---|---|
|Hızlandırılmış yazma ile Kafka, HBase|[SSE şifrelemesi](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview#encryption)|SSE şifreleme + Isteğe bağlı CMK şifrelemesi|İsteğe bağlı CMK şifrelemesi|
|Diğer tüm kümeler (Spark, etkileşimli, Hadoop, hızlandırılmamış yazma olmadan HBase)|SSE şifrelemesi|Yok|İsteğe bağlı CMK şifrelemesi|

## <a name="get-started-with-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlarla çalışmaya başlama

Müşteri tarafından yönetilen anahtar etkin bir HDInsight kümesi oluşturmak için aşağıdaki adımları inceleyeceğiz:

1. Azure kaynakları için Yönetilen kimlikler oluşturma
1. Azure Key Vault oluştur
1. Anahtar oluştur
1. Erişim İlkesi Oluştur
1. Müşteri tarafından yönetilen anahtar etkinken HDInsight kümesi oluşturma
1. Şifreleme anahtarını döndürme

## <a name="create-managed-identities-for-azure-resources"></a>Azure kaynakları için Yönetilen kimlikler oluşturma

Key Vault kimlik doğrulaması için Kullanıcı tarafından atanan bir yönetilen kimlik oluşturun.

Bkz. belirli adımlar için [Kullanıcı tarafından atanan yönetilen kimlik oluşturma](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md) . Yönetilen kimliklerin Azure HDInsight 'ta nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Azure HDInsight 'Ta Yönetilen kimlikler](hdinsight-managed-identities.md). Key Vault erişim ilkesine eklediğinizde yönetilen kimlik kaynak KIMLIĞINI kaydettiğinizden emin olun.

## <a name="create-azure-key-vault"></a>Azure Key Vault oluştur

Anahtar kasası oluşturma. Bkz. belirli adımlar için [Azure Key Vault oluşturma](../key-vault/secrets/quick-create-portal.md) .

HDInsight yalnızca Azure Key Vault destekler. Kendi anahtar kasanıza sahipseniz, anahtarlarınızı Azure Key Vault içine aktarabilirsiniz. Anahtar kasasının **geçici silme** özelliğinin etkin olması gerektiğini unutmayın. Mevcut anahtarları içeri aktarma hakkında daha fazla bilgi için [anahtarlar, gizli diziler ve sertifikalar hakkında](../key-vault/about-keys-secrets-and-certificates.md)bölümünü ziyaret edin.

## <a name="create-key"></a>Anahtar oluştur

1. Yeni anahtar kasasından **Ayarlar**  >  **anahtarlar**  >  **+ Oluştur/içeri aktar**' a gidin.

    ![Azure Key Vault yeni bir anahtar oluştur](./media/disk-encryption/create-new-key.png "Azure Key Vault yeni bir anahtar oluştur")

1. Bir ad girin ve **Oluştur**' u seçin. **RSA**varsayılan **anahtar türünü** koruyun.

    ![anahtar adı oluşturur](./media/disk-encryption/create-key.png "Anahtar adı oluştur")

1. **Anahtarlar** sayfasına geri döndüğünüzde, oluşturduğunuz anahtarı seçin.

    ![Anahtar Kasası anahtar listesi](./media/disk-encryption/key-vault-key-list.png)

1. **Anahtar sürümü** sayfasını açmak için sürümü seçin. HDInsight kümesi şifrelemesi için kendi anahtarınızı kullandığınızda, anahtar URI 'sini sağlamanız gerekir. **Anahtar tanımlayıcısını** kopyalayın ve kümenizi oluşturmaya hazır olana kadar bir yere kaydedin.

    ![anahtar tanımlayıcısını al](./media/disk-encryption/get-key-identifier.png)

## <a name="create-access-policy"></a>Erişim İlkesi Oluştur

1. Yeni anahtar kasasından **Ayarlar**  >  **erişim ilkeleri**  >  **+ erişim ilkesi Ekle**' ye gidin.

    ![Yeni Azure Key Vault erişim ilkesi oluştur](./media/disk-encryption/key-vault-access-policy.png)

1. **Erişim Ilkesi Ekle** sayfasında, aşağıdaki bilgileri sağlayın:

    |Özellik |Açıklama|
    |---|---|
    |Anahtar Izinleri|**Al**, **sarmalama tuşunu kaldır**ve **tuşu sarmala**seçeneğini belirleyin.|
    |Gizli dizi Izinleri|**Al**, **Ayarla**ve **Sil**' i seçin.|
    |Sorumlu seçin|Daha önce oluşturduğunuz Kullanıcı tarafından atanan yönetilen kimliği seçin.|

    ![Azure Key Vault erişim ilkesi için sorumlusu seçin](./media/disk-encryption/azure-portal-add-access-policy.png)

1. **Ekle**'yi seçin.

1. **Kaydet**'i seçin.

    ![Azure Key Vault erişim ilkesini Kaydet](./media/disk-encryption/add-key-vault-access-policy-save.png)

## <a name="create-cluster-with-customer-managed-key-disk-encryption"></a>Müşteri tarafından yönetilen anahtar disk şifrelemesi ile küme oluşturma

Artık yeni bir HDInsight kümesi oluşturmaya hazırsınız. Müşteri tarafından yönetilen anahtar, yalnızca küme oluşturma sırasında yeni kümelere uygulanabilir. Şifreleme, müşteri tarafından yönetilen anahtar kümelerinden kaldırılamaz ve müşteri tarafından yönetilen anahtar var olan kümelere eklenemez.

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

Küme oluşturma sırasında, anahtar sürümü de dahil olmak üzere tam **anahtar tanımlayıcısını**sağlayın. Örneğin, `https://contoso-kv.vault.azure.net/keys/myClusterKey/46ab702136bc4b229f8b10e8c2997fa4`. Ayrıca, yönetilen kimliği kümeye atamanız ve anahtar URI 'sini sağlamanız gerekir.

![Yeni küme oluştur](./media/disk-encryption/create-cluster-portal.png)

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma

Aşağıdaki örnek, disk şifrelemesi etkinken yeni bir Apache Spark kümesi oluşturmak için Azure CLı 'nın nasıl kullanılacağını göstermektedir. Daha fazla bilgi için bkz. [Azure CLI az HDInsight Create](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-create).

```azurecli
az hdinsight create -t spark -g MyResourceGroup -n MyCluster \
-p "HttpPassword1234!" --workernode-data-disks-per-node 2 \
--storage-account MyStorageAccount \
--encryption-key-name SparkClusterKey \
--encryption-key-version 00000000000000000000000000000000 \
--encryption-vault-uri https://MyKeyVault.vault.azure.net \
--assign-identity MyMSI
```

### <a name="using-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını kullanma

Aşağıdaki örnekte, disk şifrelemesi etkin olan yeni bir Apache Spark kümesi oluşturmak için bir Azure Resource Manager şablonunun nasıl kullanılacağı gösterilmektedir. Daha fazla bilgi için bkz. [ARM şablonları nedir?](https://docs.microsoft.com/azure/azure-resource-manager/templates/overview).

Bu örnek, şablonu çağırmak için PowerShell kullanır.

```powershell
$templateFile = "azuredeploy.json"
$ResourceGroupName = "MyResourceGroup"
$clusterName = "MyCluster"
$password = ConvertTo-SecureString 'HttpPassword1234!' -AsPlainText -Force
$diskEncryptionVaultUri = "https://MyKeyVault.vault.azure.net"
$diskEncryptionKeyName = "SparkClusterKey"
$diskEncryptionKeyVersion = "00000000000000000000000000000000"
$managedIdentityName = "MyMSI"

New-AzResourceGroupDeployment `
  -Name mySpark `
  -TemplateFile $templateFile `
  -ResourceGroupName $ResourceGroupName `
  -clusterName $clusterName `
  -clusterLoginPassword $password `
` -sshPassword $password `
  -diskEncryptionVaultUri $diskEncryptionVaultUri `
  -diskEncryptionKeyName $diskEncryptionKeyName `
  -diskEncryptionKeyVersion $diskEncryptionKeyVersion `
  -managedIdentityName $managedIdentityName
```

Kaynak yönetimi şablonunun içeriği `azuredeploy.json` :

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "0.9.0.0",
  "parameters": {
    "clusterName": {
      "type": "string",
      "metadata": {
        "description": "The name of the HDInsight cluster to create."
      }
    },
    "clusterLoginUserName": {
      "type": "string",
      "defaultValue": "admin",
      "metadata": {
        "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
      }
    },
    "clusterLoginPassword": {
      "type": "securestring",
      "metadata": {
        "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "The location where all azure resources will be deployed."
      }
    },
    "sshUserName": {
      "type": "string",
      "defaultValue": "sshuser",
      "metadata": {
        "description": "These credentials can be used to remotely access the cluster."
      }
    },
    "sshPassword": {
      "type": "securestring",
      "metadata": {
        "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
      }
    },
    "headNodeSize": {
      "type": "string",
      "defaultValue": "Standard_D12_v2",
      "metadata": {
        "description": "The VM size of the head nodes."
      }
    },
    "workerNodeSize": {
      "type": "string",
      "defaultValue": "Standard_D13_v2",
      "metadata": {
        "description": "The VM size of the worker nodes."
      }
    },
    "diskEncryptionVaultUri": {
      "type": "string",
      "metadata": {
        "description": "The Key Vault DNSname."
      }
    },
    "diskEncryptionKeyName": {
      "type": "string",
      "metadata": {
        "description": "The Key Vault key name."
      }
    },
    "diskEncryptionKeyVersion": {
      "type": "string",
      "metadata": {
        "description": "The Key Vault key version for the selected key."
      }
    },
    "managedIdentityName": {
      "type": "string",
      "metadata": {
        "description": "The user-assigned managed identity."
      }
    }
  },
  "variables": {
    "defaultStorageAccount": {
      "name": "[uniqueString(resourceGroup().id)]",
      "type": "Standard_LRS"
    }
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('defaultStorageAccount').name]",
      "location": "[parameters('location')]",
      "apiVersion": "2019-06-01",
      "sku": {
        "name": "[variables('defaultStorageAccount').type]"
      },
      "kind": "Storage",
      "properties": {}
    },
    {
      "apiVersion": "2018-06-01-preview",
      "name": "[parameters('clusterName')]",
      "type": "Microsoft.HDInsight/clusters",
      "location": "[parameters('location')]",
      "properties": {
        "clusterVersion": "3.6",
        "osType": "Linux",
        "tier": "standard",
        "clusterDefinition": {
          "kind": "spark",
          "componentVersion": {
            "Spark": "2.3"
          },
          "configurations": {
            "gateway": {
              "restAuthCredential.isEnabled": true,
              "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
              "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
            }
          }
        },
        "storageProfile": {
          "storageaccounts": [
            {
              "name": "[replace(replace(reference(resourceId('Microsoft.Storage/storageAccounts', variables('defaultStorageAccount').name), '2019-06-01').primaryEndpoints.blob,'https://',''),'/','')]",
              "isDefault": true,
              "container": "[parameters('clusterName')]",
              "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('defaultStorageAccount').name), '2019-06-01').keys[0].value]"
            }
          ]
        },
        "computeProfile": {
          "roles": [
            {
              "name": "headnode",
              "minInstanceCount": 1,
              "targetInstanceCount": 2,
              "hardwareProfile": {
                "vmSize": "[parameters('headNodeSize')]"
              },
              "osProfile": {
                "linuxOperatingSystemProfile": {
                  "username": "[parameters('sshUserName')]",
                  "password": "[parameters('sshPassword')]"
                },
              },
            },
            {
              "name": "workernode",
              "targetInstanceCount": 1,
              "hardwareProfile": {
                "vmSize": "[parameters('workerNodeSize')]"
              },
              "osProfile": {
                "linuxOperatingSystemProfile": {
                  "username": "[parameters('sshUserName')]",
                  "password": "[parameters('sshPassword')]"
                },
              },
            }
          ]
        },
        "minSupportedTlsVersion": "1.2",
        "diskEncryptionProperties": {
          "vaultUri": "[parameters('diskEncryptionVaultUri')]",
          "keyName": "[parameters('diskEncryptionKeyName')]",
          "keyVersion": "[parameters('diskEncryptionKeyVersion')]",
          "msiResourceId": "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/', parameters('managedIdentityName'))]"
        }
      },
      "identity": {
        "type": "UserAssigned",
        "userAssignedIdentities": {
          "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/', parameters('managedIdentityName'))]": {}
        }
      }
    }
  ]
}
```

## <a name="rotating-the-encryption-key"></a>Şifreleme anahtarını döndürme

Oluşturulduktan sonra HDInsight kümesi tarafından kullanılan şifreleme anahtarlarını değiştirmek isteyebileceğiniz senaryolar olabilir. Bu, Portal aracılığıyla kolayca olabilir. Bu işlem için, kümenin hem geçerli anahtara hem de hedeflenen yeni anahtara erişimi olması gerekir, aksi takdirde anahtar döndürme işlemi başarısız olur.

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

Anahtarı döndürmek için temel Anahtar Kasası URI 'sine ihtiyacınız vardır. Bunu yaptıktan sonra, portalda HDInsight küme özellikleri bölümüne gidin ve **disk şifreleme anahtarı URL 'si**altında **anahtarı Değiştir** ' e tıklayın. Yeni anahtar URL 'sini girin ve anahtarı döndürmek için Gönder ' i yazın.

![disk şifreleme anahtarını döndür](./media/disk-encryption/change-key.png)

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma

Aşağıdaki örnekte, mevcut bir HDInsight kümesi için disk şifreleme anahtarının nasıl döndürülebileceğiniz gösterilmektedir. Daha fazla bilgi için bkz. [Azure CLI az HDInsight döndürme-disk-Encryption-Key](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-rotate-disk-encryption-key).

```azurecli
az hdinsight rotate-disk-encryption-key \
--encryption-key-name SparkClusterKey \
--encryption-key-version 00000000000000000000000000000000 \
--encryption-vault-uri https://MyKeyVault.vault.azure.net \
--name MyCluster \
--resource-group MyResourceGroup
```

## <a name="faq-for-customer-managed-key-encryption"></a>Müşteri tarafından yönetilen anahtar şifrelemesi hakkında SSS

**HDInsight kümesi anahtar kasayma nasıl erişir?**

HDInsight, HDInsight kümesiyle ilişkilendirdiğiniz yönetilen kimliği kullanarak Azure Key Vault örneğine erişir. Bu yönetilen kimlik, küme oluşturma işleminden önce veya sırasında oluşturulabilir. Ayrıca, anahtarın depolandığı anahtar kasasına yönetilen kimlik erişimi vermeniz gerekir.

**Bu özellik HDInsight üzerindeki tüm kümeler için kullanılabilir mi?**

Müşteri tarafından yönetilen anahtar şifrelemesi, Spark 2,1 ve 2,2 dışındaki tüm küme türlerinde kullanılabilir.

**Farklı diskleri veya klasörleri şifrelemek için birden çok anahtar kullanabilir miyim?**

Hayır, tüm yönetilen diskler ve kaynak diskleri aynı anahtarla şifrelenir.

**Küme, anahtar kasasına veya anahtara erişimi kaybederse ne olur?**

Küme anahtara erişimi kaybederse, uyarılar Apache ambarı portalında gösterilir. Bu durumda, **anahtar değiştirme** işlemi başarısız olur. Anahtar erişimi geri yüklendikten sonra, ambarı uyarıları kaldırılır ve anahtar döndürme işlemi başarılı bir şekilde gerçekleştirilebilir.

![anahtar erişim ambarı uyarısı](./media/disk-encryption/ambari-alert.png)

**Anahtarlar siliniyorsa kümeyi nasıl kurtarabilirim?**

Yalnızca "geçici silme" özelliği desteklendiğinden, anahtarlar anahtar kasasında kurtarılıyorsa, küme anahtarlara yeniden erişim elde etmelidir. Azure Key Vault anahtarını kurtarmak için, bkz. [Undo-Azkeyvaultkeykaldırması](/powershell/module/az.keyvault/Undo-AzKeyVaultKeyRemoval) veya [az-keykasa-Key-Recover](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-recover).

**Hangi disk türleri şifrelenir? İşletim sistemi diskleri/kaynak diskleri de şifrelenir mi?**

Kaynak diskler ve veri/yönetilen diskler şifrelenir. İşletim sistemi diskleri şifrelenmemiştir.

**Bir küme yukarı ölçekleniyorsa, yeni düğümler müşteri tarafından yönetilen anahtarları sorunsuz şekilde destekliyor mu?**

Evet. Kümenin, ölçek artırma sırasında anahtar kasasındaki anahtara erişmesi gerekir. Aynı anahtar, kümedeki hem yönetilen diskleri hem de kaynak disklerini şifrelemek için kullanılır.

**Müşteri tarafından yönetilen anahtarlar konumumdaki kullanılabilir mi?**

HDInsight müşteri tarafından yönetilen anahtarlar tüm genel bulutlar ve ulusal bulutlarda kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Key Vault hakkında daha fazla bilgi için bkz. [Azure Key Vault nedir](../key-vault/general/overview.md).
* [Azure HDInsight 'ta Kurumsal güvenliğe genel bakış](./domain-joined/hdinsight-security-overview.md).
