---
title: Azure Key Vault ve yönetilen kimlik ile Azure Batch hesabınız için müşteri tarafından yönetilen anahtarlar yapılandırın
description: Anahtarları kullanarak Batch verilerinin nasıl şifreleneceğini öğrenin
author: pkshultz
ms.topic: how-to
ms.date: 07/17/2020
ms.author: peshultz
ms.openlocfilehash: 35780f915247e88a5de093594b653ddcebdfb06b
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89008888"
---
# <a name="configure-customer-managed-keys-for-your-azure-batch-account-with-azure-key-vault-and-managed-identity"></a>Azure Key Vault ve yönetilen kimlik ile Azure Batch hesabınız için müşteri tarafından yönetilen anahtarlar yapılandırın

Varsayılan olarak Azure Batch, sertifikalar, iş/görev meta verileri gibi Azure Batch hizmetinde depolanan tüm müşteri verilerini şifrelemek için platform tarafından yönetilen anahtarları kullanır. İsteğe bağlı olarak, Azure Batch depolanan verileri şifrelemek için kendi anahtarlarınızı (müşteri tarafından yönetilen anahtarlar) kullanabilirsiniz.

Sağladığınız anahtarların [Azure Key Vault](../key-vault/general/basic-concepts.md)oluşturulması ve müşterinin yönettiği anahtarlarla yapılandırmak istediğiniz Batch hesaplarının [Azure yönetilen kimliğiyle](../active-directory/managed-identities-azure-resources/overview.md)etkinleştirilmesi gerekir.

> [!IMPORTANT]
> Azure Batch 'de müşteri tarafından yönetilen anahtarlar için destek şu anda Batı Avrupa, Kuzey Avrupa, İsviçre Kuzey, Orta ABD, Orta Güney ABD, Orta Batı ABD, Doğu ABD, Doğu ABD 2, Batı ABD 2, US Gov Virginia ve US Gov Arizona bölgeleri için genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="create-a-batch-account-with-system-assigned-managed-identity"></a>Sistem tarafından atanan yönetilen kimlikle Batch hesabı oluşturma

### <a name="azure-portal"></a>Azure portal

[Azure Portal](https://portal.azure.com/), Batch hesapları oluşturduğunuzda, **Gelişmiş** sekmesinde kimlik türünde **sistem atanan** ' ı seçin.

![Sistem tarafından atanan kimlik türüne sahip yeni Batch hesabı](./media/batch-customer-managed-key/create-batch-account.png)

Hesap oluşturulduktan sonra, **özellik** bölümü altındaki **kimlik sorumlusu kimliği** alanında benzersiz bir GUID bulabilirsiniz. **Kimlik türü** gösterilecektir `SystemAssigned` .

![Kimlik sorumlusu kimlik alanındaki benzersiz GUID](./media/batch-customer-managed-key/linked-batch-principal.png)
 
### <a name="azure-cli"></a>Azure CLI

Yeni bir Batch hesabı oluşturduğunuzda, `SystemAssigned` parametresi için öğesini belirtin `--identity` .

```powershell
resourceGroupName='myResourceGroup'
accountName='mybatchaccount'

az batch account create \
    -n $accountName \
    -g $resourceGroupName \
    --locations regionName='West US 2' \
    --identity 'SystemAssigned'
```

Hesap oluşturulduktan sonra, bu hesapta sistem tarafından atanan yönetilen kimliğin etkinleştirildiğini doğrulayabilirsiniz. `PrincipalId`Key Vault bu toplu iş hesabına erişim vermek için bu değerin gerekli olacağı şekilde dikkat edin.

```powershell
az batch account show \
    -n $accountName \
    -g $resourceGroupName \
    --query identity
```

> [!NOTE]
> Bir Batch hesabında oluşturulan sistem tarafından atanan yönetilen kimlik yalnızca Key Vault müşterinin yönettiği anahtarları almak için kullanılır. Bu kimlik, Batch havuzlarında kullanılamaz.

## <a name="configure-your-azure-key-vault-instance"></a>Azure Key Vault örneğinizi yapılandırma

### <a name="create-an-azure-key-vault"></a>Azure Key Vault oluşturma

Azure Batch için müşteri tarafından yönetilen anahtarlarla Azure Key Vault bir örnek oluştururken, **geçici silme** ve **Temizleme korumasının** etkinleştirildiğinden emin olun.

![Key Vault oluşturma ekranı](./media/batch-customer-managed-key/create-key-vault.png)

### <a name="add-an-access-policy-to-your-azure-key-vault-instance"></a>Azure Key Vault örneğine bir erişim ilkesi ekleme

Azure portal, Key Vault oluşturulduktan sonra, **Ayarlar**' ın altında bulunan **erişim ilkesi** ' nde, yönetilen kimlik ' i kullanarak Batch hesabı erişimini ekleyin. **Anahtar izinleri**altında **Al**, **sarmalama tuşu** ve **sarmalama tuşunu kaldır**' ı seçin. 

![Erişim İlkesi Ekle](./media/batch-customer-managed-key/key-permissions.png)

**Asıl**altındaki **Seç** alanında, `principalId` daha önce aldığınız ' ı veya Batch hesabının adını girin.

![PrincipalId girin](./media/batch-customer-managed-key/principal-id.png)

### <a name="generate-a-key-in-azure-key-vault"></a>Azure Key Vault bir anahtar oluşturun

Azure portal, **anahtar** bölümündeki Key Vault örneğine gidin, **Oluştur/al**' ı seçin. **Anahtar türünü** `RSA` ve **RSA anahtar boyutunu** en az bit olacak şekilde seçin `2048` . `EC` anahtar türleri şu anda bir Batch hesabında müşteri tarafından yönetilen anahtar olarak desteklenmez.

![Bir anahtar oluşturma](./media/batch-customer-managed-key/create-key.png)

Anahtar oluşturulduktan sonra, yeni oluşturulan anahtara ve geçerli sürüme tıklayın, **Özellikler** bölümü altında **anahtar tanımlayıcısı** ' nı kopyalayın.  **Izin verilen işlemler**, **sarmalama anahtarı** ve **geri saral anahtarının** her ikisinin de işaretlenmiş olduğundan emin olun.

## <a name="enable-customer-managed-keys-on-azure-batch-account"></a>Azure Batch hesapta müşteri tarafından yönetilen anahtarları etkinleştirin

### <a name="azure-portal"></a>Azure portal

[Azure Portal](https://portal.azure.com/)Batch hesabı sayfasına gidin. **Şifreleme** bölümünde, **müşteri tarafından yönetilen anahtarı**etkinleştirin. Anahtar tanımlayıcıyı doğrudan kullanabilir veya anahtar kasasını seçip anahtar **kasası ve anahtar Seç**öğesine tıklayabilirsiniz.

![Şifreleme altında, müşteri tarafından yönetilen anahtarı etkinleştirin](./media/batch-customer-managed-key/encryption-page.png)

### <a name="azure-cli"></a>Azure CLI

Batch hesabı sistem tarafından atanan yönetilen kimlikle oluşturulduktan ve Key Vault Erişimi verildikten sonra, Batch hesabını `{Key Identifier}` parametresi altındaki URL ile güncelleştirin `keyVaultProperties` . **Encryption_key_source** olarak da ayarlayın `Microsoft.KeyVault` .

```powershell
az batch account set \
    -n $accountName \
    -g $resourceGroupName \
    --encryption_key_source Microsoft.KeyVault \
    --encryption_key_identifier {YourKeyIdentifier} 
```

## <a name="update-the-customer-managed-key-version"></a>Müşteri tarafından yönetilen anahtar sürümünü Güncelleştir

Bir anahtarın yeni bir sürümünü oluşturduğunuzda, Batch hesabını yeni sürümü kullanacak şekilde güncelleştirin. Şu adımları uygulayın:

1. Azure portal ' de Batch hesabınıza gidin ve şifreleme ayarlarını görüntüleyin.
2. Yeni anahtar sürümünün URI 'sini girin. Alternatif olarak, sürümü güncelleştirmek için anahtar kasasını ve anahtarı yeniden seçebilirsiniz.
3. Yaptığınız değişiklikleri kaydedin.

Ayrıca sürümü güncelleştirmek için Azure CLı 'yi de kullanabilirsiniz.

```powershell
az batch account set \
    -n $accountName \
    -g $resourceGroupName \
    --encryption_key_identifier {YourKeyIdentifierWithNewVersion} 
```
## <a name="use-a-different-key-for-batch-encryption"></a>Toplu Iş şifrelemesi için farklı bir anahtar kullanın

Toplu şifreleme için kullanılan anahtarı değiştirmek için şu adımları izleyin:

1. Batch hesabınıza gidin ve şifreleme ayarlarını görüntüleyin.
2. Yeni anahtar için URI girin. Alternatif olarak, anahtar kasasını seçip yeni bir anahtar seçebilirsiniz.
3. Yaptığınız değişiklikleri kaydedin.

Azure CLı 'yi farklı bir anahtar kullanmak için de kullanabilirsiniz.

```powershell
az batch account set \
    -n $accountName \
    -g $resourceGroupName \
    --encryption_key_identifier {YourNewKeyIdentifier} 
```
## <a name="frequently-asked-questions"></a>Sık sorulan sorular
  * **Mevcut Batch hesapları için müşteri tarafından yönetilen anahtarlar destekleniyor mu?** Hayır. Müşteri tarafından yönetilen anahtarlar yalnızca yeni Batch hesapları için desteklenir.
  * **2048 bitten daha büyük RSA anahtar boyutlarını seçebilir miyim?** Evet, RSA anahtar boyutları `3072` ve `4096` bitleri de desteklenir.
  * **Müşteri tarafından yönetilen anahtar iptal edildikten sonra hangi işlemler kullanılabilir?** Toplu işlem, müşteri tarafından yönetilen anahtara erişimi kaybederse, hesap silme işlemine izin verilir.
  * **Key Vault anahtarını yanlışlıkla sildiğimde Batch hesabımın erişimini nasıl geri yükleyebilirim?** Temizleme koruması ve geçici silme etkin olduğundan, mevcut anahtarları geri yükleyebilirsiniz. Daha fazla bilgi için bkz. [kurtarma Azure Key Vault](../key-vault/general/soft-delete-cli.md#recovering-a-key-vault).
  * **Müşteri tarafından yönetilen anahtarları devre dışı bırakabilir miyim?** Batch hesabının şifreleme türünü dilediğiniz zaman "Microsoft yönetilen anahtar" olarak ayarlayabilirsiniz. Bundan sonra, anahtarı silmek veya değiştirmek için ücretsiz olursunuz.
  * **Anahtarlarımı nasıl döndürebilirim?** Müşteri tarafından yönetilen anahtarlar otomatik olarak döndürülemez. Anahtarı döndürmek için, hesabın ilişkilendirildiği anahtar tanımlayıcısını güncelleştirin.
  * **Access 'i geri yükledikten sonra Batch hesabının yeniden çalışması için ne kadar sürer?** Erişim geri yüklendikten sonra hesabın yeniden erişilebilir olması 10 dakika kadar sürebilir.
  * **Batch hesabı, kaynaklarıma ne olur? kullanılamıyor mu?** Müşteri tarafından yönetilen anahtarlara toplu erişim kaybedildiğinde çalışan havuzların kaybolması, çalışmaya devam edecektir. Ancak, düğümler kullanılamayan bir duruma geçer ve görevler çalışmayı durdurur (ve yeniden kuyruğa olur). Erişim geri yüklendikten sonra düğümler yeniden kullanılabilir hale gelir ve görevler yeniden başlatılır.
  * **Bu şifreleme mekanizması bir toplu Iş havuzundaki VM disklerine mi uygulanabilir?** Hayır. Bulut hizmeti yapılandırma havuzları için, işletim sistemi ve geçici disk için hiçbir şifreleme uygulanmaz. Sanal makine yapılandırma havuzları için, işletim sistemi ve belirtilen veri diskleri varsayılan olarak bir Microsoft Platformu yönetilen anahtarıyla şifrelenir. Şu anda bu diskler için kendi anahtarınızı belirtemezsiniz. Microsoft Platformu yönetilen anahtarıyla bir Batch havuzu için VM 'lerin geçici diskini şifrelemek üzere, [sanal makine yapılandırma](/rest/api/batchservice/pool/add#virtualmachineconfiguration) havuzunuzdaki [diskencryptionconfiguration](/rest/api/batchservice/pool/add#diskencryptionconfiguration) özelliğini etkinleştirmeniz gerekir. Son derece hassas ortamlarda, geçici disk şifrelemeyi etkinleştirmenizi ve hassas verileri işletim sistemi ve veri disklerinde depolamayı önlemeniz önerilir. Daha fazla bilgi için bkz. [disk şifrelemesi etkinken havuz oluşturma](./disk-encryption.md)
  * **İşlem düğümlerinde kullanılabilir olan Batch hesabında sistem tarafından atanan yönetilen kimlik mi?** Hayır. Bu yönetilen kimlik Şu anda yalnızca müşteri tarafından yönetilen anahtar için Azure Key Vault erişim için kullanılır.
  
