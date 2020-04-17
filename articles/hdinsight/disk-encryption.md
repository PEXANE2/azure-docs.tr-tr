---
title: Azure HDInsight için müşteri tarafından yönetilen anahtar disk şifreleme
description: Bu makalede, Azure HDInsight kümelerinde yönetilen disklerde depolanan verileri şifrelemek için Azure Key Vault'tan kendi şifreleme anahtarınızı nasıl kullanacağınızı açıklanmaktadır.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/15/2020
ms.openlocfilehash: 732709dbcb5ebe54025a963379128f1a1e74183e
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81536310"
---
# <a name="customer-managed-key-disk-encryption"></a>Müşteri tarafından yönetilen anahtar disk şifrelemesi

Azure HDInsight, HDInsight küme sanal makinelerine bağlı yönetilen diskler ve kaynak diskleri üzerindeki veriler için müşteri tarafından yönetilen anahtar şifrelemesini destekler. Bu özellik, HDInsight kümelerinizde veri güvenliğini sağlayan şifreleme anahtarlarını yönetmek için Azure Key Vault'u kullanmanıza olanak tanır.

HDInsight'taki tüm yönetilen diskler Azure Depolama Hizmeti Şifrelemesi (SSE) ile korunur. Varsayılan olarak, bu diskler üzerindeki veriler Microsoft tarafından yönetilen anahtarlar kullanılarak şifrelenir. HDInsight için müşteri tarafından yönetilen anahtarları etkinleştiriseniz, Azure Key Vault'u kullanarak HDInsight'ın bu anahtarları kullanması ve yönetmesi için şifreleme anahtarlarını sağlarsınız.

Bu belge, Azure Depolama hesabınızda depolanan verilere yönelik değildir. Azure Depolama şifrelemesi hakkında daha fazla bilgi için, [veriler için Azure Depolama şifrelemesi'ne](../storage/common/storage-service-encryption.md)bakın. Kümelerinizde, şifreleme anahtarlarının Microsoft tarafından yönetildiği veya müşteri tarafından yönetilebildiği bir veya daha fazla eklenmiş Azure Depolama hesabı olabilir, ancak şifreleme hizmeti farklıdır.

## <a name="introduction"></a>Giriş

Müşteri tarafından yönetilen anahtar şifreleme, küme oluşturma sırasında ek ücret ödemeden işlenen tek adımlı bir işlemdir. Tek yapmanız gereken AZURE Key Vault ile HDInsight'ı yönetilen bir kimlik olarak kaydetmek ve kümenizi oluştururken şifreleme anahtarını eklemektir.

Kümenin her düğümündeki hem kaynak diski hem de yönetilen diskler simetrik Veri Şifreleme Anahtarı (DEK) ile şifrelenir. DEK, anahtar kasanızdan Anahtar Şifreleme Anahtarı (KEK) kullanılarak korunur. Şifreleme ve şifre çözme işlemleri tamamen Azure HDInsight tarafından işlenir.

Disk şifreleme anahtarının depolandığı anahtar kasasında anahtar kasası güvenlik duvarı etkinse, kümenin dağıtılanacağı bölge için HDInsight bölgesel Kaynak Sağlayıcısı IP adresleri anahtar kasa güvenlik duvarı yapılandırmasına eklenmelidir. HDInsight güvenilir bir Azure anahtar kasa hizmeti olmadığından bu gereklidir.

Anahtar kasasındaki anahtarları güvenli bir şekilde döndürmek için Azure portalını veya Azure CLI'yi kullanabilirsiniz. Bir anahtar döndüğünde, HDInsight kümesi birkaç dakika içinde yeni anahtarı kullanmaya başlar. Ransomware senaryolarına ve yanlışlıkla silinmeye karşı korumak için [Soft Delete](../key-vault/general/overview-soft-delete.md) temel koruma özelliklerini etkinleştirin. Bu koruma özelliği olmayan anahtar kasalar desteklenmez.

|Küme türü |İşletim Sistemi Diski (Yönetilen disk) |Veri diski (Yönetilen disk) |Geçici veri diski (Yerel SSD) |
|---|---|---|---|
|Kafka, HBase hızlandırılmış yazıyor|[SSE Şifreleme](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview#encryption)|SSE Şifreleme + İsteğe Bağlı CMK şifreleme|İsteğe bağlı CMK şifreleme|
|Diğer tüm kümeler (Spark, Interactive, Hadoop, HBase olmadan Hızlandırılmış yazıyor)|SSE Şifreleme|Yok|İsteğe bağlı CMK şifreleme|

## <a name="get-started-with-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlarla başlayın

Müşteri tarafından yönetilen anahtar etkin HDInsight kümesioluşturmak için aşağıdaki adımları gözden geçiririz:

1. Azure kaynakları için yönetilen kimlikler oluşturma
1. Azure Anahtar Kasası Oluşturma
1. Oluşturma anahtarı
1. Erişim ilkesi oluşturun
1. Müşteri tarafından yönetilen anahtar etkinken HDInsight kümesi oluşturun
1. Şifreleme anahtarını döndürme

## <a name="create-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimlikler oluşturma

Key Vault'a kimlik doğrulamak için kullanıcı tarafından atanmış yönetilen bir kimlik oluşturun.

Bkz. Belirli adımlar için [kullanıcı tarafından atanmış yönetilen bir kimlik oluşturun.](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md) Azure HDInsight'ta yönetilen kimliklerin nasıl çalıştığı hakkında daha fazla bilgi için [Azure HDInsight'ta Yönetilen kimlikler'e](hdinsight-managed-identities.md)bakın. Yönetilen kimlik kaynağı kimliğini Key Vault erişim ilkesine eklediğinizde kaydettiğinizden emin olun.

## <a name="create-azure-key-vault"></a>Azure Anahtar Kasası Oluşturma

Bir anahtar kasası oluşturma. Belirli adımlar için [Azure Anahtar Kasası Oluştur'a](../key-vault/secrets/quick-create-portal.md) bakın.

HDInsight yalnızca Azure Key Vault'u destekler. Kendi anahtar kasanız varsa, anahtarlarınızı Azure Key Vault'a aktarabilirsiniz. Anahtar kasasının Soft **delete** etkinleştirilmiş olması gerektiğini unutmayın. Varolan anahtarları alma hakkında daha fazla bilgi [için, anahtarlar, sırlar ve sertifikalar hakkında](../key-vault/about-keys-secrets-and-certificates.md)bilgi adresini ziyaret edin.

## <a name="create-key"></a>Oluşturma anahtarı

1. Yeni anahtar kasanızdan **Ayarlar** > **Tuşları** > **+ Oluştur/İçe Aktar'a**gidin.

    ![Azure Anahtar Kasası'nda yeni bir anahtar oluşturma](./media/disk-encryption/create-new-key.png "Azure Anahtar Kasası'nda yeni bir anahtar oluşturma")

1. Bir ad sağlayın, ardından **Oluştur'u**seçin. **Varsayılan ANAHTAR RSA** **Türünü** koruyun.

    ![anahtar adı oluşturur](./media/disk-encryption/create-key.png "Anahtar adı oluşturma")

1. **Keys** sayfasına döndüğünüzde, oluşturduğunuz anahtarı seçin.

    ![anahtar kasa anahtar listesi](./media/disk-encryption/key-vault-key-list.png)

1. **Anahtar Sürümü** sayfasını açmak için sürümü seçin. HDInsight küme şifrelemesi için kendi anahtarınızı kullandığınızda, URI tuşunu sağlamanız gerekir. Anahtar **tanımlayıcısını** kopyalayın ve kümenizi oluşturmaya hazır olana kadar bir yere kaydedin.

    ![anahtar tanımlayıcısı olsun](./media/disk-encryption/get-key-identifier.png)

## <a name="create-access-policy"></a>Erişim ilkesi oluşturun

1. Yeni anahtar kasanızdan **Ayarlar** > erişim ilkeleri **+ Erişim İlkesi****Ekle'ye** > gidin.

    ![Yeni Azure Anahtar Kasası erişim ilkesi oluşturun](./media/disk-encryption/key-vault-access-policy.png)

1. Erişim **ilkesi ekle** sayfasından aşağıdaki bilgileri sağlayın:

    |Özellik |Açıklama|
    |---|---|
    |Anahtar İzinler|**Al**, **Aç Tuşu**ve **Şal Tuşu'nü**seçin.|
    |Gizli İzinler|**Al**, **Ayarla**ve **Sil'i**seçin.|
    |Anapara seçin|Daha önce oluşturduğunuz kullanıcı tarafından atanan yönetilen kimliği seçin.|

    ![Azure Anahtar Kasası erişim ilkesi için Anapara Seç'i ayarlama](./media/disk-encryption/azure-portal-add-access-policy.png)

1. **Add (Ekle)** seçeneğini belirleyin.

1. **Kaydet**’i seçin.

    ![Azure Anahtar Kasası erişim ilkesini kaydet](./media/disk-encryption/add-key-vault-access-policy-save.png)

## <a name="create-cluster-with-customer-managed-key-disk-encryption"></a>Müşteri tarafından yönetilen anahtar disk şifrelemesi ile küme oluşturma

Artık yeni bir HDInsight kümesi oluşturmaya hazırsınız. Müşteri tarafından yönetilen anahtar yalnızca küme oluşturma sırasında yeni kümelere uygulanabilir. Şifreleme müşteri tarafından yönetilen anahtar kümelerinden kaldırılamaz ve müşteri tarafından yönetilen anahtar varolan kümelere ekilemez.

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

Küme oluşturma sırasında, anahtar sürümü de dahil olmak üzere tam **Anahtar tanımlayıcısını**sağlayın. Örneğin, `https://contoso-kv.vault.azure.net/keys/myClusterKey/46ab702136bc4b229f8b10e8c2997fa4`. Ayrıca, yönetilen kimliği kümeye atamanız ve URI tuşunu sağlamanız gerekir.

![Yeni küme oluşturma](./media/disk-encryption/create-cluster-portal.png)

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma

Aşağıdaki örnek, disk şifrelemeetkinleştirilmiş yeni bir Apache Spark kümesi oluşturmak için Azure CLI'nin nasıl kullanılacağını gösterir. Daha fazla bilgi için [Azure CLI az hdinsight oluşturma bilgisine](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-create)bakın.

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

Aşağıdaki örnekte, disk şifreleme etkinleştirilmiş yeni bir Apache Spark kümesi oluşturmak için Azure Kaynak Yöneticisi şablonu nasıl kullanılacağı gösterilmektedir. Daha fazla bilgi için [bkz: ARM şablonları nelerdir?](https://docs.microsoft.com/azure/azure-resource-manager/templates/overview)

Bu örnek, şablonu aramak için PowerShell'i kullanır.

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

Kaynak yönetimi şablonunun içeriği: `azuredeploy.json`

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

HDInsight kümesi tarafından oluşturulan küme tarafından kullanılan şifreleme anahtarlarını değiştirmek isteyebileceğiniz senaryolar olabilir. Bu portal üzerinden kolayca olabilir. Bu işlem için kümenin hem geçerli anahtara hem de amaçlanan yeni anahtara erişimi olmalıdır, aksi takdirde döndürme anahtarı işlemi başarısız olur.

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

Anahtarı döndürmek için, temel anahtar tonoz URI gerekir. Bunu yaptıktan sonra, portaldaki HDInsight küme özellikleri bölümüne gidin ve **Disk Şifreleme Anahtarı**URL'si altında Anahtarı **Değiştir'e** tıklayın. Yeni anahtar url'sini girin ve anahtarı döndürmek için gönderin.

![disk şifreleme anahtarını döndürme](./media/disk-encryption/change-key.png)

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma

Aşağıdaki örnek, varolan bir HDInsight kümesi için disk şifreleme anahtarının nasıl döndürüldüğünü gösterir. Daha fazla bilgi için azure [CLI az hdinsight döndür-disk şifreleme anahtarı](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-rotate-disk-encryption-key)na bakın.

```azurecli
az hdinsight rotate-disk-encryption-key \
--encryption-key-name SparkClusterKey \
--encryption-key-version 00000000000000000000000000000000 \
--encryption-vault-uri https://MyKeyVault.vault.azure.net \
--name MyCluster \
--resource-group MyResourceGroup
```

## <a name="faq-for-customer-managed-key-encryption"></a>Müşteri tarafından yönetilen anahtar şifrelemesi için SSS

**HDInsight kümesi anahtar kasama nasıl erişir?**

HDInsight, HDInsight kümesiyle ilişkilendirdiğiniz yönetilen kimliği kullanarak Azure Key Vault örneğinize erişir. Bu yönetilen kimlik küme oluşturmadan önce veya sırasında oluşturulabilir. Ayrıca, anahtarın depolandığı anahtar kasasına yönetilen kimlik erişimi izni verebilirsiniz.

**Bu özellik HDInsight'taki tüm kümeler için kullanılabilir mi?**

Müşteri tarafından yönetilen anahtar şifrelemesi, Spark 2.1 ve 2.2 dışındaki tüm küme türleri için kullanılabilir.

**Farklı diskleri veya klasörleri şifrelemek için birden çok anahtar kullanabilir miyim?**

Hayır, yönetilen tüm diskler ve kaynak diskleri aynı anahtarla şifrelenir.

**Küme anahtar kasasına veya anahtara erişimini kaybederse ne olur?**

Küme anahtara erişimini kaybederse, uyarılar Apache Ambari portalında gösterilir. Bu durumda, **Anahtar Değiştir** işlemi başarısız olur. Anahtar erişimi geri yüklendikten sonra Ambari uyarıları ortadan kaldırılır ve anahtar döndürme gibi işlemler başarıyla gerçekleştirilebilir.

![anahtar erişim Ambari uyarısı](./media/disk-encryption/ambari-alert.png)

**Tuşlar silinirse kümeyi nasıl kurtarabilirim?**

Yalnızca "Soft Delete" etkin tuşları desteklendiğinden, tuşlar anahtar kasasında kurtarılırsa, kümenin tuşlara yeniden erişmesi gerekir. Azure Key Vault anahtarını kurtarmak için [Bkz.](/powershell/module/az.keyvault/Undo-AzKeyVaultKeyRemoval) [az-keyvault-key-recover](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-recover)

**Hangi disk türleri şifrelenir? İşletim sistemi diskleri/kaynak diskleri de şifrelenmiş mi?**

Kaynak diskleri ve veri/yönetilen diskler şifrelenir. İşletim sistemi diskleri şifrelenmez.

**Bir küme ölçeklenirse, yeni düğümler müşteri tarafından yönetilen anahtarları sorunsuz bir şekilde destekler mi?**

Evet. Küme, ölçeklendirme sırasında anahtar kasasındaki anahtara erişmesi gerekir. Aynı anahtar kümedeki yönetilen diskleri ve kaynak disklerini şifrelemek için kullanılır.

**Müşteri tarafından yönetilen anahtarlar konumumda kullanılabilir mi?**

HDInsight müşteri tarafından yönetilen tüm genel bulutlarda ve ulusal bulutlarda kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Anahtar Kasası hakkında daha fazla bilgi için [Azure Anahtar Kasası nedir'e](../key-vault/general/overview.md)bakın.
* [Azure HDInsight'ta kurumsal güvenliğe genel bakış.](./domain-joined/hdinsight-security-overview.md)
