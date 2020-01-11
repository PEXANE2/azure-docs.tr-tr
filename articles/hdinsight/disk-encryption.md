---
title: Azure HDInsight için müşteri tarafından yönetilen anahtar disk şifrelemesi
description: Bu makalede, Azure HDInsight kümelerinde yönetilen disklerde depolanan verileri şifrelemek için Azure Key Vault ' den kendi şifreleme anahtarınızın nasıl kullanılacağı açıklanır.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/06/2019
ms.openlocfilehash: f3414033068f99e638030efb158a7fb01706b88c
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75897721"
---
# <a name="customer-managed-key-disk-encryption"></a>Müşteri tarafından yönetilen anahtar disk şifrelemesi

Azure HDInsight, yönetilen disklerde ve HDInsight kümesi VM 'lerine eklenen kaynak disklerinde bulunan veriler için Kendi Anahtarını Getir (BYOK) şifrelemesi olarak da bilinen müşteri tarafından yönetilen anahtarları destekler. Bu özellik, HDInsight kümelerinizde bekleyen verilerin güvenliğini sağlayan şifreleme anahtarlarını yönetmek için Azure Key Vault kullanmanıza olanak sağlar. Kümelerinizde, şifreleme anahtarlarının Microsoft tarafından yönetilen veya müşteri tarafından yönetilen olabileceği, ancak şifreleme hizmeti farklı olan bir veya daha fazla bağlı Azure depolama hesabı olabilir.

Bu belge, Azure depolama hesabınızda depolanan verileri gidermez. Ao hakkında daha fazla bilgi için lütfen [bekleyen veriler Için Azure depolama şifrelemesi](../storage/common/storage-service-encryption.md)bölümüne bakın.

HDInsight 'taki tüm yönetilen diskler Azure Depolama Hizmeti Şifrelemesi (SSE) ile korunmaktadır. Varsayılan olarak, bu disklerdeki veriler Microsoft tarafından yönetilen anahtarlar kullanılarak şifrelenir. HDInsight için müşteri tarafından yönetilen anahtarları etkinleştirirseniz, HDInsight için şifreleme anahtarlarını kullanarak bu anahtarları kullanabilir ve Azure Key Vault aracılığıyla yönetebilirsiniz.

Müşteri tarafından yönetilen anahtar şifrelemesi, küme oluşturma sırasında ek bir ücret ödemeden işlenen tek adımlı bir işlemdir. Yapmanız gereken tek şey, HDInsight 'ı Azure Key Vault ile yönetilen kimlik olarak kaydeder ve kümenizi oluştururken şifreleme anahtarını eklemektir.

Kümenin her bir düğümündeki kaynak disk ve yönetilen diskler, simetrik bir veri şifreleme anahtarı (DEK) ile şifrelenir. DEK, anahtar kasaınızdan anahtar şifreleme anahtarı (KEK) kullanılarak korunmaktadır. Şifreleme ve şifre çözme işlemleri tamamen Azure HDInsight tarafından işlenir.

Anahtar kasasındaki anahtarları güvenle döndürmek için Azure portal veya Azure CLı kullanabilirsiniz. Bir anahtar döndürülerek HDInsight kümesi yeni anahtarı dakikalar içinde kullanmaya başlar. Fidye yazılımı senaryolarına ve yanlışlıkla silinmeye karşı korunmak için "geçici silme" anahtar koruma özelliklerini etkinleştirin. Bu koruma özelliği olmadan anahtar kasaları desteklenmez.

## <a name="get-started-with-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlarla çalışmaya başlama

Müşteri tarafından yönetilen anahtar etkin bir HDInsight kümesi oluşturmak için aşağıdaki adımları inceleyeceğiz:

1. Azure kaynakları için Yönetilen kimlikler oluşturma
2. Azure Key Vault ve anahtarları ayarlama
3. Müşteri tarafından yönetilen anahtar etkinken HDInsight kümesi oluşturma
4. Şifreleme anahtarını döndürme

## <a name="create-managed-identities-for-azure-resources"></a>Azure kaynakları için Yönetilen kimlikler oluşturma

Key Vault kimlik doğrulaması yapmak için, [Azure Portal](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md), [Azure PowerShell](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md), [Azure Resource Manager](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-arm.md)veya [Azure CLI](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md)kullanarak Kullanıcı tarafından atanan bir yönetilen kimlik oluşturun. Yönetilen kimliklerin Azure HDInsight 'ta nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Azure HDInsight 'Ta Yönetilen kimlikler](hdinsight-managed-identities.md). Key Vault erişim ilkesine eklediğinizde yönetilen kimlik kaynak KIMLIĞINI kaydettiğinizden emin olun.

## <a name="set-up-the-key-vault-and-keys"></a>Key Vault ve anahtarları ayarlama

HDInsight yalnızca Azure Key Vault destekler. Kendi anahtar kasanıza sahipseniz, anahtarlarınızı Azure Key Vault içine aktarabilirsiniz. Anahtarların "geçici silme" olması gerektiğini unutmayın. "Geçici silme" özelliği REST, .NET/C#, PowerShell ve Azure CLI arabirimleri aracılığıyla kullanılabilir.

1. Yeni bir Anahtar Kasası oluşturmak için [Azure Key Vault](../key-vault/key-vault-overview.md) hızlı başlangıcı ' nı izleyin. Mevcut anahtarları içeri aktarma hakkında daha fazla bilgi için [anahtarlar, gizli diziler ve sertifikalar hakkında](../key-vault/about-keys-secrets-and-certificates.md)bölümünü ziyaret edin.

1. [Az keykasa Update](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-update) CLI komutunu kullanarak anahtar kasasında "geçici silme" özelliğini etkinleştirin.

    ```azurecli
    az keyvault update --name <Key Vault Name> --enable-soft-delete
    ```

1. Anahtar oluşturun.

    a. Yeni bir anahtar oluşturmak için **Ayarlar**altındaki **anahtarlar** menüsünden **Oluştur/içeri aktar** ' ı seçin.

    ![Azure Key Vault yeni bir anahtar oluştur](./media/disk-encryption/create-new-key.png "Azure Key Vault yeni bir anahtar oluştur")

    b. Oluşturma **seçeneklerini** belirleyin ve anahtara bir ad verin.

    ![anahtar adı oluşturur](./media/disk-encryption/create-key.png "Anahtar adı oluştur")

    c. Anahtar listesinden oluşturduğunuz anahtarı seçin.

    ![Anahtar Kasası anahtar listesi](./media/disk-encryption/key-vault-key-list.png)

    d. HDInsight kümesi şifrelemesi için kendi anahtarınızı kullandığınızda, anahtar URI 'sini sağlamanız gerekir. **Anahtar tanımlayıcısını** kopyalayın ve kümenizi oluşturmaya hazır olana kadar bir yere kaydedin.

    ![anahtar tanımlayıcısını al](./media/disk-encryption/get-key-identifier.png)

1. Yönetilen kimliği Anahtar Kasası erişim ilkesine ekleyin.

    a. Yeni bir Azure Key Vault erişim ilkesi oluşturun.

    ![Yeni Azure Key Vault erişim ilkesi oluştur](./media/disk-encryption/add-key-vault-access-policy.png)

    b. **Asıl seçin**altında oluşturduğunuz Kullanıcı tarafından atanan yönetilen kimliği seçin.

    ![Azure Key Vault erişim ilkesi için sorumlusu seçin](./media/disk-encryption/add-key-vault-access-policy-select-principal.png)

    c. **Anahtar ve** **sarmalama**anahtarını **almak**için **anahtar izinleri** ayarlayın.

    ![Azure Key Vault Access policy1 için anahtar Izinleri ayarlama](./media/disk-encryption/add-key-vault-access-policy-keys.png "Azure Key Vault Access policy1 için anahtar Izinleri ayarlama")

    d. **Get**, **set**ve **Delete**için **gizli izinleri** ayarlayın.

    ![Azure Key Vault Access policy2 için anahtar Izinleri ayarlama](./media/disk-encryption/add-key-vault-access-policy-secrets.png "Azure Key Vault Access policy2 için anahtar Izinleri ayarlama")

    e. **Kaydet**’i seçin.

    ![Azure Key Vault erişim ilkesini Kaydet](./media/disk-encryption/add-key-vault-access-policy-save.png)

## <a name="create-cluster-with-customer-managed-key-disk-encryption"></a>Müşteri tarafından yönetilen anahtar disk şifrelemesi ile küme oluşturma

Artık yeni bir HDInsight kümesi oluşturmaya hazırsınız. Müşteri tarafından yönetilen anahtar, yalnızca küme oluşturma sırasında yeni kümelere uygulanabilir. Şifreleme, müşteri tarafından yönetilen anahtar kümelerinden kaldırılamaz ve müşteri tarafından yönetilen anahtar var olan kümelere eklenemez.

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

Küme oluşturma sırasında, anahtar sürümü de dahil olmak üzere tam anahtar URL 'sini sağlayın. Örneğin, `https://contoso-kv.vault.azure.net/keys/myClusterKey/46ab702136bc4b229f8b10e8c2997fa4`. Ayrıca, yönetilen kimliği kümeye atamanız ve anahtar URI 'sini sağlamanız gerekir.

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma

Aşağıdaki örnek, disk şifrelemesi etkinken yeni bir Apache Spark kümesi oluşturmak için Azure CLı 'nın nasıl kullanılacağını göstermektedir. Daha fazla bilgi için bkz. [Azure CLI az HDInsight Create](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-create) documentation.

```azurecli
az hdinsight create -t spark -g MyResourceGroup -n MyCluster \
-p "HttpPassword1234!" --workernode-data-disks-per-node 2 \
--storage-account MyStorageAccount \
--encryption-key-name SparkClusterKey \
--encryption-key-version 00000000000000000000000000000000 \
--encryption-vault-uri https://MyKeyVault.vault.azure.net \
--assign-identity MyMSI
```

## <a name="rotating-the-encryption-key"></a>Şifreleme anahtarını döndürme

Oluşturulduktan sonra HDInsight kümesi tarafından kullanılan şifreleme anahtarlarını değiştirmek isteyebileceğiniz senaryolar olabilir. Bu, Portal aracılığıyla kolayca olabilir. Bu işlem için, kümenin hem geçerli anahtara hem de hedeflenen yeni anahtara erişimi olması gerekir, aksi takdirde anahtar döndürme işlemi başarısız olur.

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

Anahtarı döndürmek için, yeni anahtarın tam URL 'sine sahip olmanız gerekir (bkz. [Key Vault ve anahtarlarını kurulumun](#set-up-the-key-vault-and-keys)3. adımında). Bunu yaptıktan sonra, portalda HDInsight küme özellikleri bölümüne gidin ve **disk şifreleme anahtarı URL 'si**altında **anahtarı Değiştir** ' e tıklayın. Yeni anahtar URL 'sini girin ve anahtarı döndürmek için Gönder ' i yazın.

![disk şifreleme anahtarını döndür](./media/disk-encryption/change-key.png)

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma

Aşağıdaki örnekte, mevcut bir HDInsight kümesi için disk şifreleme anahtarının nasıl döndürülebileceğiniz gösterilmektedir. Daha fazla ayrıntı için bkz. [Azure CLI az HDInsight döndürme-disk-Encryption-Key](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-rotate-disk-encryption-key) .

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

* [Azure HDInsight 'ta Kurumsal güvenliğe genel bakış](/domain-joined/hdinsight-security-overview.md)
