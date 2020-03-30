---
title: Azure HDInsight için müşteri tarafından yönetilen anahtar disk şifreleme
description: Bu makalede, Azure HDInsight kümelerinde yönetilen disklerde depolanan verileri şifrelemek için Azure Key Vault'tan kendi şifreleme anahtarınızı nasıl kullanacağınızı açıklanmaktadır.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/20/2020
ms.openlocfilehash: fd5308574e84ab6d2e30b9352254683b2d1d6fdd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78403572"
---
# <a name="customer-managed-key-disk-encryption"></a>Müşteri tarafından yönetilen anahtar disk şifrelemesi

Azure HDInsight, HDInsight küme sanal makinelerine bağlı yönetilen diskler ve kaynak diskleri üzerindeki veriler için müşteri tarafından yönetilen anahtar şifrelemesini destekler. Bu özellik, HDInsight kümelerinizde veri güvenliğini sağlayan şifreleme anahtarlarını yönetmek için Azure Key Vault'u kullanmanıza olanak tanır. 

HDInsight'taki tüm yönetilen diskler Azure Depolama Hizmeti Şifrelemesi (SSE) ile korunur. Varsayılan olarak, bu diskler üzerindeki veriler Microsoft tarafından yönetilen anahtarlar kullanılarak şifrelenir. HDInsight için müşteri tarafından yönetilen anahtarları etkinleştiriseniz, Azure Key Vault'u kullanarak HDInsight'ın bu anahtarları kullanması ve yönetmesi için şifreleme anahtarlarını sağlarsınız.

Bu belge, Azure Depolama hesabınızda depolanan verilere yönelik değildir. Azure Depolama şifrelemesi hakkında daha fazla bilgi için, [veriler için Azure Depolama şifrelemesi'ne](../storage/common/storage-service-encryption.md)bakın. Kümelerinizde, şifreleme anahtarlarının Microsoft tarafından yönetildiği veya müşteri tarafından yönetilebildiği bir veya daha fazla eklenmiş Azure Depolama hesabı olabilir, ancak şifreleme hizmeti farklıdır.

## <a name="introduction"></a>Giriş

Müşteri tarafından yönetilen anahtar şifreleme, küme oluşturma sırasında ek ücret ödemeden işlenen tek adımlı bir işlemdir. Tek yapmanız gereken AZURE Key Vault ile HDInsight'ı yönetilen bir kimlik olarak kaydetmek ve kümenizi oluştururken şifreleme anahtarını eklemektir.

Kümenin her düğümündeki hem kaynak diski hem de yönetilen diskler simetrik Veri Şifreleme Anahtarı (DEK) ile şifrelenir. DEK, anahtar kasanızdan Anahtar Şifreleme Anahtarı (KEK) kullanılarak korunur. Şifreleme ve şifre çözme işlemleri tamamen Azure HDInsight tarafından işlenir.

Disk şifreleme anahtarının depolandığı anahtar kasasında anahtar kasası güvenlik duvarı etkinse, kümenin dağıtılanacağı bölge için HDInsight bölgesel Kaynak Sağlayıcısı IP adresleri anahtar kasa güvenlik duvarı yapılandırmasına eklenmelidir. HDInsight güvenilir bir Azure anahtar kasa hizmeti olmadığından bu gereklidir.

Anahtar kasasındaki anahtarları güvenli bir şekilde döndürmek için Azure portalını veya Azure CLI'yi kullanabilirsiniz. Bir anahtar döndüğünde, HDInsight kümesi birkaç dakika içinde yeni anahtarı kullanmaya başlar. Ransomware senaryolarına ve yanlışlıkla silinmeye karşı korumak için [Soft Delete](../key-vault/key-vault-ovw-soft-delete.md) temel koruma özelliklerini etkinleştirin. Bu koruma özelliği olmayan anahtar kasalar desteklenmez.

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

Bir anahtar kasası oluşturma. Belirli adımlar için [Azure Anahtar Kasası Oluştur'a](../key-vault/quick-create-portal.md) bakın.

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

1. **Ekle'yi**seçin.

1. **Kaydet'i**seçin.

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

* Azure Anahtar Kasası hakkında daha fazla bilgi için [Azure Anahtar Kasası nedir'e](../key-vault/key-vault-overview.md)bakın.
* [Azure HDInsight'ta kurumsal güvenliğe genel bakış.](./domain-joined/hdinsight-security-overview.md)
