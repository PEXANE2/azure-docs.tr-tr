---
title: Azure Key Vault-CLı ile geçici silme kullanma
description: Anahtar kasalarının ve Anahtar Kasası nesnelerinin kurtarılmasına izin veren Azure Key Vault geçici silme özelliğini kullanmak için Azure CLı 'yi nasıl kullanacağınızı öğrenin.
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 08/11/2020
ms.author: sudbalas
ms.openlocfilehash: da821da08594180b9dd94728252e1a43c04fbde2
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90531670"
---
# <a name="how-to-use-key-vault-soft-delete-with-cli"></a>Key Vault geçici silmeyi CLI ile kullanma

Azure Key Vault geçici silme özelliği, silinen kasaların ve kasa nesnelerinin kurtarılmasına olanak tanır. Özellikle, geçici silme aşağıdaki senaryolara yöneliktir:

- Bir anahtar kasasının kurtarılabilir silme desteği
- Anahtar Kasası nesnelerinin kurtarılabilir silme desteği; Anahtarlar, gizlilikler ve, sertifikalar

## <a name="prerequisites"></a>Önkoşullar

- Azure CLı-ortamınız için bu kuruluma sahip değilseniz bkz. [Azure CLI kullanarak Key Vault yönetme](manage-with-cli2.md).

CLı için Key Vault özel başvuru bilgileri için bkz. [Azure clı Key Vault başvurusu](https://docs.microsoft.com/cli/azure/keyvault).

## <a name="required-permissions"></a>Gerekli izinler

Key Vault işlemler, rol tabanlı erişim denetimi (RBAC) izinleri aracılığıyla aşağıdaki şekilde ayrı yönetilir:

| İşlem | Açıklama | Kullanıcı izni |
|:--|:--|:--|
|Liste|Silinen anahtar kasalarını listeler.|Microsoft. Keykasası/Silinleults/okuma|
|Kurtar|Silinen bir anahtar kasasını geri yükler.|Microsoft. Keykasası/Vaults/yazma|
|Temizle|Silinen bir anahtar kasasını ve tüm içeriğini kalıcı olarak kaldırır.|Microsoft. Keykasası/konumlar/Silinkaults/Temizleme/eylem|

İzinler ve erişim denetimi hakkında daha fazla bilgi için bkz. [anahtar kasanızın güvenliğini sağlama](secure-your-key-vault.md).

## <a name="enabling-soft-delete"></a>Geçici silme etkinleştiriliyor

Silinen bir anahtar kasasının kurtarılmasına veya bir anahtar kasasında depolanan nesnelere izin vermek için "geçici silme" özelliğini etkinleştirirsiniz.

> [!IMPORTANT]
> Anahtar kasasında ' Soft-DELETE ' özelliğinin etkinleştirilmesi geri alınamaz bir eylemdir. Soft-Delete özelliği "true" olarak ayarlandıktan sonra değiştirilemez veya kaldırılamaz.  

### <a name="existing-key-vault"></a>Mevcut Anahtar Kasası

Contosokasası adlı mevcut bir Anahtar Kasası için aşağıdaki gibi geçici silme özelliğini etkinleştirin. 

```azurecli
az keyvault update -n ContosoVault --enable-soft-delete true
```

### <a name="new-key-vault"></a>Yeni Anahtar Kasası

Geçici silme, varsayılan olarak tüm anahtar kasalarında otomatik olarak etkinleştirilir. 31 Aralık 2020 ' den, geçici silme etkin olmadan yeni bir Anahtar Kasası oluşturmak artık mümkün olmayacaktır.

### <a name="verify-soft-delete-enablement"></a>Geçici silme etkinleştirmeyi doğrula

Bir anahtar kasasının geçici silme özelliğinin etkin olduğunu doğrulamak için *göster* komutunu çalıştırın ve ' geçici silme etkin? ' öğesini arayın. özniteliğe

```azurecli
az keyvault show --name ContosoVault
```

## <a name="deleting-a-soft-delete-protected-key-vault"></a>Geçici silme korumalı anahtar kasasını silme

Yazılım, geçici silme özelliğinin etkin olup olmadığına bağlı olarak davranışındaki bir Anahtar Kasası değişikliğini silme komutu.

> [!IMPORTANT]
>Geçici silme etkin olmayan bir Anahtar Kasası için aşağıdaki komutu çalıştırırsanız, bu anahtar kasasını ve tüm içeriğini kurtarma seçeneği olmadan kalıcı olarak silersiniz!

```azurecli
az keyvault delete --name ContosoVault
```

### <a name="how-soft-delete-protects-your-key-vaults"></a>Yumuşak silme, anahtar kasalarınızı korur

Geçici silme etkin:

- Silinen bir Anahtar Kasası, kaynak grubundan kaldırılır ve oluşturulduğu konumla ilişkili ayrılmış bir ad alanına yerleştirilir. 
- Anahtarlar, gizli diziler ve sertifikalar gibi silinen nesneler, kapsayan Anahtar Kasası silinen durumunda olduğu sürece erişilemez olur. 
- Silinen bir Anahtar Kasası için DNS adı ayrılmıştır ve aynı ada sahip yeni bir anahtar kasasının oluşturulmasını önler.  

Aşağıdaki komutu kullanarak, aboneliğinizle ilişkili silinmiş durum anahtarı kasalarını görüntüleyebilirsiniz:

```azurecli
az keyvault list-deleted
```
- *Kimliği* , kurtarılırken veya temizlenirken kaynağı tanımlamak için kullanılabilir. 
- *Kaynak kimliği* , bu kasanın ORIJINAL kaynak kimliğidir. Bu Anahtar Kasası artık silinmiş durumda olduğundan, bu kaynak KIMLIĞINE sahip bir kaynak yok. 
- *Zamanlanan Temizleme tarihi* , hiçbir işlem yapılmaz kasanın kalıcı olarak silinme tarihidir. *Zamanlanan Temizleme tarihini*hesaplamak için kullanılan varsayılan saklama süresi 90 gündür.

## <a name="recovering-a-key-vault"></a>Anahtar kasasını kurtarma

Bir anahtar kasasını kurtarmak için Anahtar Kasası adı, kaynak grubu ve konum belirtirsiniz. Kurtarma işlemi için ihtiyaç duyduğunuz şekilde, silinen anahtar kasasının konumunu ve kaynak grubunu aklınızda bulabilirsiniz.

```azurecli
az keyvault recover --location westus --resource-group ContosoRG --name ContosoVault
```

Bir Anahtar Kasası kurtarılırken, anahtar kasasının orijinal kaynak KIMLIĞIYLE yeni bir kaynak oluşturulur. Özgün kaynak grubu kaldırılırsa, kurtarma denenmeye başlamadan önce bir ad aynı adla oluşturulmalıdır.

## <a name="deleting-and-purging-key-vault-objects"></a>Anahtar Kasası nesnelerini silme ve Temizleme

Aşağıdaki komut, "Contosokasası" adlı bir anahtar kasasında (geçici silme özelliği etkin olan) ' ContosoFirstKey ' anahtarını silecek:

```azurecli
az keyvault key delete --name ContosoFirstKey --vault-name ContosoVault
```

Anahtar kasanız, geçici silme için etkin durumdayken silinen bir anahtar hala silinmiş gibi görünür, ancak silinen anahtarları açıkça listeleseniz veya aldığınızda. Silinen bir anahtarın listelenmesi, kurtarılması veya temizlenmesi hariç olmak üzere, bir anahtar üzerindeki çoğu işlem başarısız olur. 

Örneğin, bir anahtar kasasındaki silinen anahtarları listelemek istemek için aşağıdaki komutu kullanın:

```azurecli
az keyvault key list-deleted --vault-name ContosoVault
```

### <a name="transition-state"></a>Geçiş durumu 

Geçici silme etkinken anahtar kasasındaki bir anahtarı sildiğinizde, geçişin tamamlanması birkaç saniye sürebilir. Bu geçiş sırasında, anahtar etkin durumda değil veya silinmiş durumda olabilir. 

### <a name="using-soft-delete-with-key-vault-objects"></a>Anahtar Kasası nesneleriyle geçici silme kullanma

Anahtar kasaları, silinen bir anahtar, gizli dizi ya da sertifika gibi, kurtarmadığınız veya temizolmadığınız sürece 90 güne kadar, silinen durumda kalır.

#### <a name="keys"></a>Anahtarlar

Geçici olarak silinen bir anahtarı kurtarmak için:

```azurecli
az keyvault key recover --name ContosoFirstKey --vault-name ContosoVault
```

Kalıcı olarak silinen bir anahtarı (Temizleme olarak da bilinir) kalıcı olarak silmek için:

> [!IMPORTANT]
> Bir anahtarı temizlemek onu kalıcı olarak siler ve kurtarılamaz! 

```azurecli
az keyvault key purge --name ContosoFirstKey --vault-name ContosoVault
```

**Kurtarma** ve **Temizleme** eylemlerinin bir Anahtar Kasası erişim ilkesiyle ilişkili kendi izinleri vardır. Bir kullanıcı veya hizmet sorumlusunun bir **Kurtarma** veya **Temizleme** eylemi yürütebilmesi için bu anahtar veya gizli dizi için ilgili izinlere sahip olmaları gerekir. Varsayılan olarak, tüm izinleri vermek için ' tümü ' kısayolu kullanıldığında, bir anahtar kasasının erişim ilkesine **Temizleme** eklenmez. Özellikle **Temizleme** izni vermelisiniz. 

#### <a name="set-a-key-vault-access-policy"></a>Anahtar Kasası erişim ilkesi ayarlama

Aşağıdaki komut, user@contoso.com *contosokasasındaki* anahtarlar üzerinde **Temizleme**dahil birkaç işlemi kullanma izni verir:

```azurecli
az keyvault set-policy --name ContosoVault --key-permissions get create delete list update import backup restore recover purge
```

>[!NOTE] 
> Yalnızca geçici silme özelliği etkinleştirilmiş olan bir anahtar kasanıza sahipseniz, **Kurtarma** ve **Temizleme** izinleriniz olmayabilir.

#### <a name="secrets"></a>Gizli Diziler

Anahtarlar gibi gizli dizileri kendi komutlarıyla yönetilir:

- SQLPassword adlı bir gizli dizi silin: 
  ```azurecli
  az keyvault secret delete --vault-name ContosoVault -name SQLPassword
  ```

- Bir anahtar kasasındaki tüm silinen gizli dizileri listeleyin: 
  ```azurecli
  az keyvault secret list-deleted --vault-name ContosoVault
  ```

- Silinen durumda gizli dizi kurtarma: 
  ```azurecli
  az keyvault secret recover --name SQLPassword --vault-name ContosoVault
  ```

- Silinen durumda gizli dizi temizle: 

  > [!IMPORTANT]
  > Gizli dizi Temizleme bunu kalıcı olarak siler ve kurtarılamaz! 

  ```azurecli
  az keyvault secret purge --name SQLPAssword --vault-name ContosoVault
  ```

## <a name="purging-a-soft-delete-protected-key-vault"></a>Geçici silme korumalı anahtar kasasını Temizleme

> [!IMPORTANT]
> Bir anahtar kasasını veya kapsanan nesnelerinden birini temizlemek, kalıcı olarak silinecek, aksi, kurtarılabilir olmayacaktır!

Temizleme işlevi, daha önce geçici olarak silinmiş olan bir Anahtar Kasası nesnesini veya tüm anahtar kasasını kalıcı olarak silmek için kullanılır. Önceki bölümde gösterildiği gibi, yumuşak silme özelliği etkinken bir anahtar kasasında depolanan nesneler birden fazla duruma geçebilir:

- **Etkin**: silinmeden önce.
- **Geçici**silme: silinmeden sonra, listelenmiş ve etkin duruma geri kurtarılabiliyor.
- **Kalıcı olarak silindi**: temizleme sonrasında kurtarılamaz, kurtarılamaz.

Aynı değer, Anahtar Kasası için de geçerlidir. Geçici olarak silinen bir anahtar kasasını ve içeriğini kalıcı olarak silmek için anahtar kasasının kendisini temizlemeniz gerekir.

### <a name="purging-a-key-vault"></a>Anahtar kasasını Temizleme

Bir Anahtar Kasası temizlendiğinde, anahtarlar, gizlilikler ve Sertifikalar dahil olmak üzere tüm içerikleri kalıcı olarak silinir. Geçici olarak silinen bir anahtar kasasını temizlemek için komutunu kullanın `az keyvault purge` . Aboneliğinizi Silinen anahtar kasalarınızın konumunu komutunu kullanarak bulabilirsiniz `az keyvault list-deleted` .

```azurecli
az keyvault purge --location westus --name ContosoVault
```

### <a name="purge-permissions-required"></a>Temizleme izinleri gerekli
- Silinen bir anahtar kasasını temizlemek için, kullanıcının *Microsoft. Keykasası/konumlar/deletedVaults/Temizleme/eylem* IŞLEMI için RBAC iznine sahip olması gerekir. 
- Silinen bir anahtar kasasını listelemek için, kullanıcının *Microsoft. Keykasası/deletedVaults/Read* IŞLEMI için RBAC iznine sahip olması gerekir. 
- Varsayılan olarak, yalnızca bir abonelik Yöneticisi bu izinlere sahiptir. 

### <a name="scheduled-purge"></a>Zamanlanmış temizleme

Silinen Anahtar Kasası nesnelerinin listelenmesi Ayrıca Key Vault tarafından temizlenmek üzere zamanlandığında de gösterilir. *Zamanlanan Temizleme tarihi* , hiçbir işlem yapılmaz bir Anahtar Kasası nesnesinin kalıcı olarak silineceğini gösterir. Varsayılan olarak, silinen bir Anahtar Kasası nesnesi için bekletme süresi 90 gündür.

>[!IMPORTANT]
>*Zamanlanan Temizleme tarihi* alanı tarafından tetiklenen temizlenen bir kasa nesnesi kalıcı olarak silinir. Kurtarılamaz!

## <a name="enabling-purge-protection"></a>Temizleme korumasını etkinleştirme

Temizleme koruması açık olduğunda, 90 günlük bekletme süresi geçene kadar bir kasa veya silinmiş durumdaki bir nesne kaldırılamaz. Bu kasa veya nesne yine de kurtarılabilir. Bu özellik, bir kasasının veya bir nesnenin Bekletme dönemi geçene kadar hiçbir zaman kalıcı olarak silinebileceğini güvence altına almanızı sağlar.

Temizleme korumasını yalnızca geçici silme de etkinse etkinleştirebilirsiniz. Temizleme korumasını devre dışı bırakma desteklenmiyor.

Bir kasa oluştururken hem geçici silme hem de Temizleme korumasını açmak için [az keykasa Create](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create) komutunu kullanın:

```azurecli
az keyvault create --name ContosoVault --resource-group ContosoRG --location westus --enable-soft-delete true --enable-purge-protection true
```

Mevcut bir kasaya Temizleme koruması eklemek için (zaten geçici silme etkindir), [az keykasası Update](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-update) komutunu kullanın:

```azurecli
az keyvault update --name ContosoVault --resource-group ContosoRG --enable-purge-protection true
```

## <a name="other-resources"></a>Diğer kaynaklar

- Key Vault geçici silme özelliğine genel bakış için bkz. [Azure Key Vault geçici silme genel bakış](soft-delete-overview.md).
- Azure Key Vault kullanımı için genel bir bakış için bkz. [Azure Key Vault nedir?](overview.md)

