---
title: Azure Key Vault - PowerShell ile yumuşak silme nasıl kullanılır?
description: PowerShell kod parçacıkları ile yumuşak silme örneklerini kullanma
services: key-vault
author: msmbaldwin
manager: rkarlin
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: mbaldwin
ms.openlocfilehash: 26c309eeebd7226c6777ec41ae674587da796dd4
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78199674"
---
# <a name="how-to-use-key-vault-soft-delete-with-powershell"></a>Key Vault geçici silmeyi PowerShell ile kullanma

Azure Key Vault'un yumuşak silme özelliği silinmiş kasaların ve kasa nesnelerinin kurtarılmasına olanak tanır. Özellikle, yumuşak silme adresleri aşağıdaki senaryoları:

- Anahtar kasasının geri kazanılabilir silinmesi için destek
- Anahtar kasa nesnelerinin kurtarılabilir silinmesi için destek; anahtarlar, sırlar ve sertifikalar

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- Azure PowerShell 1.0.0 veya sonrası - Bu zaten kurulumunuz yoksa, Azure PowerShell'i yükleyin ve Azure aboneliğinizle ilişkilendirin, [Azure PowerShell'i nasıl yükleyip yapılandırabileceğinize](https://docs.microsoft.com/powershell/azure/overview)bakın. 

>[!NOTE]
> Key Vault PowerShell çıkış biçimlendirme dosyamızın, doğru sürüm yerine ortamınıza **yüklenebilecek** eski bir sürümü vardır. Biz çıkış biçimlendirme için gerekli düzeltme içerecek şekilde PowerShell güncelleştirilmiş bir sürümünü bekliyoruz ve o zaman bu konuyu güncelleştirecektir. Bu biçimlendirme sorunuyla karşılaşırsanız, geçerli geçici çözüm şudur:
> - Bu konuda açıklanan yumuşak silme özellikli özelliği görmediğinizi fark ederseniz aşağıdaki `$vault = Get-AzKeyVault -VaultName myvault; $vault.EnableSoftDelete`sorguyu kullanın: .


PowerShell için Key Vault özel referans bilgileri için Azure [Key Vault PowerShell referansına](/powershell/module/az.keyvault)bakın.

## <a name="required-permissions"></a>Gerekli izinler

Key Vault işlemleri, rol tabanlı erişim denetimi (RBAC) izinleri ile aşağıdaki gibi ayrı ayrı yönetilir:

| İşlem | Açıklama | Kullanıcı izni |
|:--|:--|:--|
|Liste|Listeler silinen anahtar kasaları.|Microsoft.KeyVault/deletedVaults/read|
|Kurtar|Silinmiş bir anahtar kasası geri yüklenir.|Microsoft.KeyVault/vaults/write|
|Temizle|Silinen bir anahtar kasasını ve tüm içeriğini kalıcı olarak kaldırır.|Microsoft.KeyVault/locations/deletedVaults/tasfiye/action|

İzinler ve erişim denetimi hakkında daha fazla bilgi [için, anahtar kasanızı güvenli](key-vault-secure-your-key-vault.md)olarak görün.

## <a name="enabling-soft-delete"></a>Yumuşak silmeyi etkinleştirme

Silinmiş bir anahtar kasasının veya anahtar kasasında depolanan nesnelerin kurtarılmasına izin vermek için "yumuşak silme"yi etkinleştirirsiniz.

> [!IMPORTANT]
> Anahtar kasasında 'yumuşak silme'yi etkinleştirmek geri döndürülemez bir eylemdir. Yumuşak silme özelliği "true" olarak ayarlandıktan sonra değiştirilemez veya kaldırılamaz.  

### <a name="existing-key-vault"></a>Mevcut anahtar kasası

ContosoVault adlı varolan bir anahtar kasası için aşağıdaki gibi yumuşak silmeyi etkinleştirin. 

```powershell
($resource = Get-AzResource -ResourceId (Get-AzKeyVault -VaultName "ContosoVault").ResourceId).Properties | Add-Member -MemberType "NoteProperty" -Name "enableSoftDelete" -Value "true"

Set-AzResource -resourceid $resource.ResourceId -Properties $resource.Properties
```

### <a name="new-key-vault"></a>Yeni anahtar kasası

Yeni bir anahtar kasası için yumuşak silmeyi etkinleştirme, oluşturma komutuna soft-delete enable flag ekleyerek oluşturma zamanında yapılır.

```powershell
New-AzKeyVault -Name "ContosoVault" -ResourceGroupName "ContosoRG" -Location "westus" -EnableSoftDelete
```

### <a name="verify-soft-delete-enablement"></a>Yumuşak silme etkinleştirmesini doğrula

Anahtar kasasının yumuşak silme etkin olduğunu doğrulamak için *gösteri* komutunu çalıştırın ve 'Etkin Leştirilmiş Yumuşak Silme'yi arayın. Öznitelik:

```powershell
Get-AzKeyVault -VaultName "ContosoVault"
```

## <a name="deleting-a-soft-delete-protected-key-vault"></a>Yumuşak silme korumalı anahtar kasası silme

Bir anahtar kasasını silme komutu, yumuşak silme nin etkin olup olmadığına bağlı olarak davranışta değişir.

> [!IMPORTANT]
>Yumuşak silme etkin olmayan bir anahtar kasası için aşağıdaki komutu çalıştırarsanız, kurtarma için hiçbir seçenek olmadan bu anahtar kasasını ve tüm içeriğini kalıcı olarak silersiniz!

```powershell
Remove-AzKeyVault -VaultName 'ContosoVault'
```

### <a name="how-soft-delete-protects-your-key-vaults"></a>Yumuşak silme anahtar tonlarınızı nasıl korur?

Yumuşak silme etkin:

- Silinen anahtar kasası kaynak grubundan kaldırılır ve oluşturulduğu konumla ilişkili ayrılmış bir ad alanına yerleştirilir. 
- Anahtarlar, sırlar ve sertifikalar gibi silinen nesnelere, içeren anahtar kasaları silinmiş durumda olduğu sürece erişilemez. 
- Silinmiş bir anahtar kasasının DNS adı ayrılmıştır ve aynı ada sahip yeni bir anahtar kasasının oluşturulması engellenir.  

Aboneliğinizle ilişkili silinmiş durum anahtar kasalarını aşağıdaki komutu kullanarak görüntüleyebilirsiniz:

```powershell
Get-AzKeyVault -InRemovedState 
```

- *Kimlik,* kurtarma veya temizleme yaparken kaynağı tanımlamak için kullanılabilir. 
- *Kaynak kimliği,* bu kasanın özgün kaynak kimliğidir. Bu anahtar kasası artık silinmiş durumda olduğundan, bu kaynak kimliğiyle kaynak bulunmamaktadır. 
- *Zamanlanmış Temizleme Tarihi,* herhangi bir işlem yapılmazsa kasanın kalıcı olarak silineceği tarihtir. *Zamanlanan Temizleme Tarihini*hesaplamak için kullanılan varsayılan bekletme süresi 90 gündür.

## <a name="recovering-a-key-vault"></a>Anahtar kasası kurtarma

Anahtar kasasını kurtarmak için anahtar kasa adını, kaynak grubunu ve konumu belirtirsiniz. Kurtarma işlemi için ihtiyacınız olduğu gibi, silinen anahtar kasasının konumunu ve kaynak grubunu not edin.

```powershell
Undo-AzKeyVaultRemoval -VaultName ContosoVault -ResourceGroupName ContosoRG -Location westus
```

Anahtar kasası kurtarıldığında, anahtar kasasının orijinal kaynak kimliğiyle birlikte yeni bir kaynak oluşturulur. Özgün kaynak grubu kaldırılırsa, kurtarma yı denemeden önce aynı adla oluşturulması gerekir.

## <a name="deleting-and-purging-key-vault-objects"></a>Anahtar kasa nesnelerini silme ve temizleme

Aşağıdaki komut , 'ContosoFirstKey' tuşunu, 'ContosoVault' adlı ve yumuşak silme özelliğine sahip olan anahtar kasasında siler:

```powershell
Remove-AzKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey
```

Anahtar kasanız yumuşak silme için etkinleştirildiğinden, silinmiş tuşları açıkça listelize etmediğiniz sürece silinmiş bir anahtar silinmiş gibi görünür. Silinen durumdaki bir anahtardaki işlemlerin çoğu, silinmiş bir anahtarı listeleme, kurtarma, temizleme dışında başarısız olur. 

Örneğin, 'ContosoVault' anahtar kasasında silinen tuşları aşağıdaki komut listeler:

```powershell
Get-AzKeyVaultKey -VaultName ContosoVault -InRemovedState
```

### <a name="transition-state"></a>Geçiş durumu 

Yumuşak silme etkinleştirilmiş bir anahtar kasasında bir anahtarı sildiğinizde, geçişin tamamlanması birkaç saniye sürebilir. Bu geçiş sırasında, anahtarın etkin durumda veya silinen durumda olmadığı görünebilir. 

### <a name="using-soft-delete-with-key-vault-objects"></a>Anahtar kasa nesneleri ile yumuşak silme kullanma

Anahtar kasaları gibi, silinmiş bir anahtar, gizli veya sertifika, siz geri almadığınız veya temizlemediğiniz sürece 90 güne kadar silinmiş durumda kalır. 

#### <a name="keys"></a>Anahtarlar

Yumuşak silinmiş bir anahtarı kurtarmak için:

```powershell
Undo-AzKeyVaultKeyRemoval -VaultName ContosoVault -Name ContosoFirstKey
```

Yumuşak silinmiş bir anahtarı kalıcı olarak silmek (temizleme olarak da bilinir) için:

> [!IMPORTANT]
> Bir anahtarı temizlemek kalıcı olarak siler ve kurtarılamaz! 

```powershell
Remove-AzKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey -InRemovedState
```

**Kurtarma** ve **temizleme** eylemlerinin, önemli bir kasa erişim ilkesinde ilişkili kendi izinleri vardır. Bir kullanıcı veya hizmet sorumlusunun **kurtarma** veya **temizleme** eylemini gerçekleştirebilmesi için, bu anahtar veya gizli için ilgili izine sahip olması gerekir. Varsayılan olarak, **'tüm'** kısayolu tüm izinleri vermek için kullanıldığında, temizleme anahtar kasasının erişim ilkesine eklenmez. Özellikle **tasfiye** izni vermelisiniz. 

#### <a name="set-a-key-vault-access-policy"></a>Önemli bir kasa erişim ilkesi ayarlama

Aşağıdaki komut, user@contoso.com *ContosoVault'ta* **tasfiye**dahil olmak üzere anahtarlar üzerinde çeşitli işlemler kullanma izni verir:

```powershell
Set-AzKeyVaultAccessPolicy -VaultName ContosoVault -UserPrincipalName user@contoso.com -PermissionsToKeys get,create,delete,list,update,import,backup,restore,recover,purge
```

>[!NOTE] 
> Yumuşak silme özelliği etkinleştirilmiş varolan bir anahtar kasanız varsa, **kurtarma** ve **temizleme** izinleri olmayabilir.

#### <a name="secrets"></a>Gizli Diziler

Anahtarlar gibi, sırlar da kendi komutlarıyla yönetilir:

- SQLPassword adlı bir sırrı silme: 
  ```powershell
  Remove-AzKeyVaultSecret -VaultName ContosoVault -name SQLPassword
  ```

- Silinen tüm sırları anahtar kasasında listele: 
  ```powershell
  Get-AzKeyVaultSecret -VaultName ContosoVault -InRemovedState
  ```

- Silinen durumda bir sırrı kurtarın: 
  ```powershell
  Undo-AzKeyVaultSecretRemoval -VaultName ContosoVault -Name SQLPAssword
  ```

- Silinmiş durumda bir sırrı temizleme: 

  > [!IMPORTANT]
  > Bir sırrı temizlemek kalıcı olarak siler ve kurtarılamaz!

  ```powershell
  Remove-AzKeyVaultSecret -VaultName ContosoVault -InRemovedState -name SQLPassword
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

Anahtar kasası temizlendiğinde, anahtarlar, sırlar ve sertifikalar da dahil olmak üzere tüm içeriği kalıcı olarak silinir. Yumuşak silinmiş bir anahtar kasasını `Remove-AzKeyVault` temizlemek için, `-InRemovedState` seçeneği olan komutu ve `-Location location` bağımsız değişkenle birlikte silinen anahtar kasasının konumunu belirterek kullanın. Silinmiş bir kasanın konumunu komutu `Get-AzKeyVault -InRemovedState`kullanarak bulabilirsiniz.

```powershell
Remove-AzKeyVault -VaultName ContosoVault -InRemovedState -Location westus
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

Kasa oluştururken hem yumuşak silme yi hem de temizleme korumasını açmak için [New-AzKeyVault](/powershell/module/az.keyvault/new-azkeyvault?view=azps-1.5.0) cmdlet'ini kullanın:

```powershell
New-AzKeyVault -Name ContosoVault -ResourceGroupName ContosoRG -Location westus -EnableSoftDelete -EnablePurgeProtection
```

Varolan bir kasaya temizleme koruması eklemek için (zaten yumuşak silme etkinleştirilmiş), [Get-AzKeyVault,](/powershell/module/az.keyvault/Get-AzKeyVault?view=azps-1.5.0) [Get-AzResource](/powershell/module/az.resources/get-azresource?view=azps-1.5.0)ve [Set-AzResource](/powershell/module/az.resources/set-azresource?view=azps-1.5.0) cmdlets kullanın:

```
($resource = Get-AzResource -ResourceId (Get-AzKeyVault -VaultName "ContosoVault").ResourceId).Properties | Add-Member -MemberType "NoteProperty" -Name "enablePurgeProtection" -Value "true"

Set-AzResource -resourceid $resource.ResourceId -Properties $resource.Properties
```

## <a name="other-resources"></a>Diğer kaynaklar

- Key Vault'un yumuşak silme özelliğine genel bakış için [Azure Key Vault yumuşak silme genel görünümüne](key-vault-ovw-soft-delete.md)bakın.
- Azure Anahtar Kasası kullanımına genel bir bakış [için](key-vault-overview.md)bkz.
