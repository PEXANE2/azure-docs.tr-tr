---
title: Azure Key Vault - CLI ile yumuşak silme nasıl kullanılır
description: CLI kod parçacıkları ile yumuşak silme örneği kullanma
services: key-vault
author: msmbaldwin
manager: rkarlin
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: mbaldwin
ms.openlocfilehash: ae6ddac61ecbcef41704f71ed5188fc547a996a3
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81616598"
---
# <a name="how-to-use-key-vault-soft-delete-with-cli"></a>Key Vault geçici silmeyi CLI ile kullanma

Azure Key Vault'un yumuşak silme özelliği silinmiş kasaların ve kasa nesnelerinin kurtarılmasına olanak tanır. Özellikle, yumuşak silme adresleri aşağıdaki senaryoları:

- Anahtar kasasının geri kazanılabilir silinmesi için destek
- Anahtar kasa nesnelerinin kurtarılabilir silinmesi için destek; anahtarlar, sırlar ve sertifikalar

## <a name="prerequisites"></a>Ön koşullar

- Azure CLI - Ortamınız için bu kuruluma sahip değilseniz, [Azure CLI'yi kullanarak Anahtar Kasası Yönet'e](manage-with-cli2.md)bakın).

CLI için Key Vault özel referans bilgileri için [Azure CLI Key Vault başvurusuna](https://docs.microsoft.com/cli/azure/keyvault)bakın.

## <a name="required-permissions"></a>Gerekli izinler

Key Vault işlemleri, rol tabanlı erişim denetimi (RBAC) izinleri ile aşağıdaki gibi ayrı ayrı yönetilir:

| İşlem | Açıklama | Kullanıcı izni |
|:--|:--|:--|
|Liste|Listeler silinen anahtar kasaları.|Microsoft.KeyVault/deletedVaults/read|
|Kurtar|Silinmiş bir anahtar kasası geri yüklenir.|Microsoft.KeyVault/vaults/write|
|Temizle|Silinen bir anahtar kasasını ve tüm içeriğini kalıcı olarak kaldırır.|Microsoft.KeyVault/locations/deletedVaults/tasfiye/action|

İzinler ve erişim denetimi hakkında daha fazla bilgi [için, anahtar kasanızı güvenli](secure-your-key-vault.md)olarak görün)

## <a name="enabling-soft-delete"></a>Yumuşak silmeyi etkinleştirme

Silinmiş bir anahtar kasasının veya anahtar kasasında depolanan nesnelerin kurtarılmasına izin vermek için "yumuşak silme"yi etkinleştirirsiniz.

> [!IMPORTANT]
> Anahtar kasasında 'yumuşak silme'yi etkinleştirmek geri dönüşü olmayan bir eylemdir. Yumuşak silme özelliği "true" olarak ayarlandıktan sonra değiştirilemez veya kaldırılamaz.  

### <a name="existing-key-vault"></a>Mevcut anahtar kasası

ContosoVault adlı varolan bir anahtar kasası için aşağıdaki gibi yumuşak silmeyi etkinleştirin. 

```azurecli
az keyvault update -n ContosoVault --enable-soft-delete true
```

### <a name="new-key-vault"></a>Yeni anahtar kasası

Yeni bir anahtar kasası için yumuşak silmeyi etkinleştirme, oluşturma komutuna soft-delete enable flag ekleyerek oluşturma zamanında yapılır.

```azurecli
az keyvault create --name ContosoVault --resource-group ContosoRG --enable-soft-delete true --location westus
```

### <a name="verify-soft-delete-enablement"></a>Yumuşak silme etkinleştirmesini doğrula

Anahtar kasasının yumuşak silme etkin olduğunu doğrulamak için *gösteri* komutunu çalıştırın ve 'Etkin Leştirilmiş Yumuşak Silme'yi arayın. Öznitelik:

```azurecli
az keyvault show --name ContosoVault
```

## <a name="deleting-a-soft-delete-protected-key-vault"></a>Yumuşak silme korumalı anahtar kasası silme

Bir anahtar kasasını silme komutu, yumuşak silme nin etkin olup olmadığına bağlı olarak davranışta değişir.

> [!IMPORTANT]
>Yumuşak silme etkin olmayan bir anahtar kasası için aşağıdaki komutu çalıştırarsanız, kurtarma için hiçbir seçenek olmadan bu anahtar kasasını ve tüm içeriğini kalıcı olarak silersiniz!

```azurecli
az keyvault delete --name ContosoVault
```

### <a name="how-soft-delete-protects-your-key-vaults"></a>Yumuşak silme anahtar tonlarınızı nasıl korur?

Yumuşak silme etkin:

- Silinen anahtar kasası kaynak grubundan kaldırılır ve oluşturulduğu konumla ilişkili ayrılmış bir ad alanına yerleştirilir. 
- Anahtarlar, sırlar ve sertifikalar gibi silinen nesnelere, içeren anahtar kasaları silinmiş durumda olduğu sürece erişilemez. 
- Silinmiş bir anahtar kasasının DNS adı ayrılmıştır ve aynı ada sahip yeni bir anahtar kasasının oluşturulması engellenir.  

Aboneliğinizle ilişkili silinmiş durum anahtar kasalarını aşağıdaki komutu kullanarak görüntüleyebilirsiniz:

```azurecli
az keyvault list-deleted
```
- *Kimlik,* kurtarma veya temizleme yaparken kaynağı tanımlamak için kullanılabilir. 
- *Kaynak kimliği,* bu kasanın özgün kaynak kimliğidir. Bu anahtar kasası artık silinmiş durumda olduğundan, bu kaynak kimliğiyle kaynak bulunmamaktadır. 
- *Zamanlanmış Temizleme Tarihi,* herhangi bir işlem yapılmazsa kasanın kalıcı olarak silineceği tarihtir. *Zamanlanan Temizleme Tarihini*hesaplamak için kullanılan varsayılan bekletme süresi 90 gündür.

## <a name="recovering-a-key-vault"></a>Anahtar kasası kurtarma

Anahtar kasasını kurtarmak için anahtar kasa adını, kaynak grubunu ve konumu belirtirsiniz. Kurtarma işlemi için ihtiyacınız olduğu gibi, silinen anahtar kasasının konumunu ve kaynak grubunu not edin.

```azurecli
az keyvault recover --location westus --resource-group ContosoRG --name ContosoVault
```

Anahtar kasası kurtarıldığında, anahtar kasasının orijinal kaynak kimliğiyle birlikte yeni bir kaynak oluşturulur. Özgün kaynak grubu kaldırılırsa, kurtarma yı denemeden önce aynı adla oluşturulması gerekir.

## <a name="deleting-and-purging-key-vault-objects"></a>Anahtar kasa nesnelerini silme ve temizleme

Aşağıdaki komut , 'ContosoFirstKey' tuşunu, 'ContosoVault' adlı ve yumuşak silme özelliğine sahip olan anahtar kasasında siler:

```azurecli
az keyvault key delete --name ContosoFirstKey --vault-name ContosoVault
```

Anahtar kasanız yumuşak silme için etkinleştirildiğinde, silinmiş anahtarları açıkça listelediğiniz veya aldığınız da dışında silinmiş gibi görünen bir anahtar görüntülenir. Silinen durumdaki bir anahtardaki işlemlerin çoğu, silinmiş bir anahtarı listelemek, kurtarmak veya temizlemek dışında başarısız olur. 

Örneğin, silinen anahtarları bir anahtar kasasında listelemek için aşağıdaki komutu kullanın:

```azurecli
az keyvault key list-deleted --vault-name ContosoVault
```

### <a name="transition-state"></a>Geçiş durumu 

Yumuşak silme etkinleştirilmiş bir anahtar kasasında bir anahtarı sildiğinizde, geçişin tamamlanması birkaç saniye sürebilir. Bu geçiş sırasında, anahtarın etkin durumda veya silinen durumda olmadığı görünebilir. 

### <a name="using-soft-delete-with-key-vault-objects"></a>Anahtar kasa nesneleri ile yumuşak silme kullanma

Anahtar kasaları gibi, silinmiş bir anahtar, gizli veya sertifika, siz geri almadığınız veya temizlemediğiniz sürece 90 güne kadar silinmiş durumda kalır.

#### <a name="keys"></a>Anahtarlar

Yumuşak silinmiş bir anahtarı kurtarmak için:

```azurecli
az keyvault key recover --name ContosoFirstKey --vault-name ContosoVault
```

Yumuşak silinmiş bir anahtarı kalıcı olarak silmek (temizleme olarak da bilinir) için:

> [!IMPORTANT]
> Bir anahtarı temizlemek kalıcı olarak siler ve kurtarılamaz! 

```azurecli
az keyvault key purge --name ContosoFirstKey --vault-name ContosoVault
```

**Kurtarma** ve **temizleme** eylemlerinin, önemli bir kasa erişim ilkesinde ilişkili kendi izinleri vardır. Bir kullanıcı veya hizmet sorumlusunun **kurtarma** veya **temizleme** eylemini gerçekleştirebilmesi için, bu anahtar veya gizli için ilgili izine sahip olması gerekir. Varsayılan olarak, **'tüm'** kısayolu tüm izinleri vermek için kullanıldığında, temizleme anahtar kasasının erişim ilkesine eklenmez. Özellikle **tasfiye** izni vermelisiniz. 

#### <a name="set-a-key-vault-access-policy"></a>Önemli bir kasa erişim ilkesi ayarlama

Aşağıdaki komut, user@contoso.com *ContosoVault'ta* **tasfiye**dahil olmak üzere anahtarlar üzerinde çeşitli işlemler kullanma izni verir:

```azurecli
az keyvault set-policy --name ContosoVault --key-permissions get create delete list update import backup restore recover purge
```

>[!NOTE] 
> Yumuşak silme özelliği etkinleştirilmiş varolan bir anahtar kasanız varsa, **kurtarma** ve **temizleme** izinleri olmayabilir.

#### <a name="secrets"></a>Gizli Diziler

Anahtarlar gibi, sırlar da kendi komutlarıyla yönetilir:

- SQLPassword adlı bir sırrı silme: 
  ```azurecli
  az keyvault secret delete --vault-name ContosoVault -name SQLPassword
  ```

- Silinen tüm sırları anahtar kasasında listele: 
  ```azurecli
  az keyvault secret list-deleted --vault-name ContosoVault
  ```

- Silinen durumda bir sırrı kurtarın: 
  ```azurecli
  az keyvault secret recover --name SQLPassword --vault-name ContosoVault
  ```

- Silinmiş durumda bir sırrı temizleme: 

  > [!IMPORTANT]
  > Bir sırrı temizlemek kalıcı olarak siler ve kurtarılamaz! 

  ```azurecli
  az keyvault secret purge --name SQLPAssword --vault-name ContosoVault
  ```

## <a name="purging-a-soft-delete-protected-key-vault"></a>Yumuşak silme korumalı anahtar kasası temizleme

> [!IMPORTANT]
> Bir anahtar kasa veya içerdiği nesnelerden biri tasfiye, kalıcı olarak siler, bu da kurtarılabilir olmayacak anlamına gelir!

Temizleme işlevi, daha önce yumuşak silinmiş bir anahtar tonoz nesnesini veya tüm anahtar kasasını kalıcı olarak silmek için kullanılır. Önceki bölümde gösterildiği gibi, yumuşak silme özelliği etkin olan anahtar kasasında depolanan nesneler birden çok durumdan geçebilir:

- **Active**: silinmeden önce.
- **Yumuşak Silinmiş**: silindikten sonra, listelenebilir ve etkin duruma geri kurtarılabilir.
- **Kalıcı Olarak Silinmiş**: tasfiyeden sonra, kurtarılamaz.

Aynı şey anahtar kasa için de geçerli. Yumuşak silinmiş bir anahtar kasasını ve içeriğini kalıcı olarak silmek için, anahtar kasasının kendisini temizlemeniz gerekir.

### <a name="purging-a-key-vault"></a>Anahtar kasanın temizlenmesi

Anahtar kasası temizlendiğinde, anahtarlar, sırlar ve sertifikalar da dahil olmak üzere tüm içeriği kalıcı olarak silinir. Yumuşak silinmiş bir anahtar kasasını `az keyvault purge` temizlemek için komutu kullanın. Aboneliğinizin silinen anahtar kasalarının konumunu komutu `az keyvault list-deleted`kullanarak bulabilirsiniz.

```azurecli
az keyvault purge --location westus --name ContosoVault
```

### <a name="purge-permissions-required"></a>Temizleme izinleri gerekli
- Silinen bir anahtar kasasını temizlemek için, kullanıcının *Microsoft.KeyVault/locations/deletedVaults/tasfiye/action* işlemi için RBAC iznine ihtiyacı vardır. 
- Silinmiş bir anahtar kasasını listelemek için, kullanıcının *Microsoft.KeyVault/deletedVaults/read* işlemi için RBAC iznine ihtiyacı vardır. 
- Varsayılan olarak, yalnızca bir abonelik yöneticisi bu izinlere sahiptir. 

### <a name="scheduled-purge"></a>Zamanlanmış temizleme

Silinen anahtar kasa nesnelerinin listelenmesi, Key Vault tarafından ne zaman temizleneceklerini de gösterir. *Zamanlanmış Temizleme Tarihi,* hiçbir işlem yapılmazsa, anahtar kasa nesnesi kalıcı olarak ne zaman silineceğini gösterir. Varsayılan olarak, silinmiş bir anahtar kasa nesnesinin bekletme süresi 90 gündür.

>[!IMPORTANT]
>*Zamanlanmış Temizleme Tarihi* alanı tarafından tetiklenen temizlenmiş kasa nesnesi kalıcı olarak silinir. Kurtarılamaz!

## <a name="enabling-purge-protection"></a>Temizleme Korumasını Etkinleştirme

Temizleme koruması açık olduğunda, silinmiş durumdaki bir kasa veya nesne, 90 günlük saklama süresi geçene kadar temizlenemez. Bu tür kasa veya nesne hala kurtarılabilir. Bu özellik, saklama süresi geçene kadar bir kasanın veya nesnenin kalıcı olarak silinmeyeceğine dair ek güvence verir.

Yalnızca yumuşak silme de etkinse temizleme korumasını etkinleştirebilirsiniz. 

Kasa oluştururken hem yumuşak silme hem de temizleme korumasını açmak için [az keyvault oluşturma](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create) komutunu kullanın:

```azurecli
az keyvault create --name ContosoVault --resource-group ContosoRG --location westus --enable-soft-delete true --enable-purge-protection true
```

Varolan bir kasaya temizleme koruması eklemek için (zaten yumuşak silme etkinleştirilmiş), [az keyvault güncelleştirme](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-update) komutunu kullanın:

```azurecli
az keyvault update --name ContosoVault --resource-group ContosoRG --enable-purge-protection true
```

## <a name="other-resources"></a>Diğer kaynaklar

- Key Vault'un yumuşak silme özelliğine genel bakış için [Azure Key Vault yumuşak silme genel görünümüne](overview-soft-delete.md)bakın).
- Azure Anahtar Kasası kullanımına genel bir bakış [için](overview.md)bkz.

