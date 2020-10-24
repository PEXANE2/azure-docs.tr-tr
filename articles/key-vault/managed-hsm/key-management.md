---
title: Yönetilen bir HSM 'de anahtarları yönetme-Azure Key Vault | Microsoft Docs
description: Yönetilen bir HSM 'deki anahtarları yönetmek için bu makaleyi kullanın
services: key-vault
author: amitbapat
ms.service: key-vault
ms.subservice: managed-hsm
ms.topic: tutorial
ms.date: 09/15/2020
ms.author: ambapat
ms.openlocfilehash: 3f054638e09061c652946c9c2db1a32db73c23d9
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92521042"
---
# <a name="manage-a-managed-hsm-using-the-azure-cli"></a>Azure CLı kullanarak yönetilen bir HSM 'yi yönetme

> [!NOTE]
> Key Vault iki tür kaynağı destekler: kasa ve yönetilen HSM 'ler. Bu makale, **YÖNETILEN HSM**ile ilgilidir. Kasayı yönetme hakkında bilgi edinmek istiyorsanız lütfen bkz. [Azure CLI kullanarak Key Vault yönetme](../general/manage-with-cli2.md).

Yönetilen HSM 'ye genel bakış için bkz. [YÖNETILEN HSM nedir?](overview.md)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları tamamlayabilmeniz için aşağıdaki öğelere sahip olmanız gerekir:

* Bir Microsoft Azure aboneliği. Hesabınız yoksa, [ücretsiz deneme için kaydolabilirsiniz](https://azure.microsoft.com/pricing/free-trial).
* Azure CLı sürüm 2.12.0 veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli).
* Aboneliğinizde yönetilen bir HSM. Bkz. [hızlı başlangıç: Azure CLI kullanarak](quick-create-cli.md) yönetilen bir HSM sağlama ve etkinleştirme.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

CLı kullanarak Azure 'da oturum açmak için şunu yazabilirsiniz:

```azurecli
az login
```

CLı aracılığıyla oturum açma seçenekleri hakkında daha fazla bilgi için bkz. [Azure CLI ile oturum açma](/cli/azure/authenticate-azure-cli?view=azure-cli-latest&preserve-view=true)

> [!NOTE]
> Aşağıdaki tüm komutlarda iki kullanım yöntemi gösterilmektedir. Bir tane, **--HSM-Name** ve **--Name** (anahtar adı için) parametreleri ve uygun yerlerde anahtar adı dahil olmak üzere tüm URL 'yi belirtebileceğiniz **--id** parametresini kullanarak bir diğeri. İkinci yöntem, çağıran (bir kullanıcı veya uygulama) denetim düzlemi üzerinde okuma erişimine sahip olmadığında ve yalnızca veri düzleminde kısıtlı erişime sahip olduğunda yararlıdır.

## <a name="create-an-hsm-key"></a>HSM anahtarı oluşturma

`az keyvault key create`Bir anahtar oluşturmak için komutunu kullanın.

### <a name="create-an-rsa-key"></a>RSA anahtarı oluşturma

Aşağıdaki örnekte **, yalnızca wrapKey, unwrapKey** işlemleri (--OPS) için kullanılacak olan 3072 bitlik bir **RSA** anahtarı oluşturma işlemi gösterilmektedir. 


```azurecli-interactive
az keyvault key create --hsm-name ContosoMHSM --name myrsakey --ops wrapKey unwrapKey --kty RSA-HSM --size 3072

## OR
# Note the key name (myrsakey) in the URI

az keyvault key create --id https://ContosoMHSM.managedhsm.azure.net/keys/myrsakey --ops wrapKey unwrapKey --kty RSA-HSM --size 3072
```

`get`İşlemin yalnızca ortak anahtar ve anahtar özniteliklerini döndürdüğünü unutmayın. Özel anahtarı (asimetrik anahtar veya anahtar malzemesi (simetrik anahtar olması durumunda) döndürmez.

### <a name="create-an-ec-key"></a>EC anahtarı oluşturma

Aşağıdaki örnekte, yalnızca **imzalama ve doğrulama** işlemleri için kullanılacak olan (--OPS) P-256 eğrisi Ile bir **EC** anahtarı oluşturma işlemi gösterilmektedir ve iki etiket, **kullanım** ve Kullanıcı **adı**vardır. Etiketler, izleme ve yönetme için anahtara ek meta veri eklemenize yardımcı olur.

```azurecli-interactive
az keyvault key create --hsm-name ContosoMHSM --name myec256key --ops sign verify  --tags ‘usage=signing] appname=myapp’ --kty EC-HSM --curve P-256

## OR
# Note the key name (myec256key) in the URI

az keyvault key create --id https://ContosoMHSM.managedhsm.azure.net/keys/myec256key --ops sign verify  --tags ‘usage=signing] appname=myapp’ --kty EC-HSM --curve P-256
```

### <a name="create-a-256-bit-symmetric-key"></a>256 bitlik bir simetrik anahtar oluşturma

Aşağıdaki örnekte, yalnızca **şifreleme ve şifre çözme** işlemleri (--OPS) için kullanılacak 256 bitlik bir **simetrik** anahtar oluşturma işlemi gösterilmektedir.

```azurecli-interactive
az keyvault key create --hsm-name ContosoMHSM --name myaeskey --ops encrypt decrypt  --tags --kty oct-HSM --size 256

## OR
# Note the key name (myaeskey) in the URI

az keyvault key create --id https://ContosoMHSM.managedhsm.azure.net/keys/myaeskey --ops encrypt decrypt  --tags ‘usage=signing] appname=myapp’ --kty oct-HSM --size 256
```

## <a name="view-key-attributes-and-tags"></a>Anahtar özniteliklerini ve etiketlerini görüntüle

`az keyvault key show`Bir anahtarın özniteliklerini, sürümlerini ve etiketlerini görüntülemek için komutunu kullanın.

```azurecli-interactive
az keyvault key show --hsm-name ContosoHSM --name myrsakey

## OR
# Note the key name (myaeskey) in the URI

az keyvault key show --id https://ContosoMHSM.managedhsm.azure.net/keys/myrsakey

```

## <a name="list-keys"></a>Anahtarları Listele

`az keyvault key list`Yönetilen BIR HSM içindeki tüm anahtarları listelemek için komutunu kullanın.

```azurecli-interactive
az keyvault key list --hsm-name ContosoHSM

## OR
# use full URI

az keyvault key list --id https://ContosoMHSM.managedhsm.azure.net/

```

## <a name="delete-a-key"></a>Bir anahtarı silme

`az keyvault key delete`Yönetilen BIR HSM 'den bir anahtarı silmek için komutunu kullanın. Geçici silmenin her zaman açık olduğunu unutmayın. Bu nedenle, silinen bir anahtar silinmiş durumda kalır ve anahtar temizlendiğinde (kalıcı olarak siliniyorsa), kurtarma mümkün olmadan, bekletme günü geçene kadar kurtarılmaz.

```azurecli-interactive
az keyvault key delete --hsm-name ContosoHSM --name myrsakey

## OR
# Note the key name (myaeskey) in the URI

az keyvault key delete --id https://ContosoMHSM.managedhsm.azure.net/keys/myrsakey
```

## <a name="list-deleted-keys"></a>Silinen anahtarları Listele

`az keyvault key list-deleted`YÖNETILEN HSM 'inizdeki tüm anahtarları silinen durumda listelemek için komutunu kullanın.

```azurecli-interactive
az keyvault key list-deleted --hsm-name ContosoHSM

## OR
# use full URI

az keyvault key list-deleted --id https://ContosoMHSM.managedhsm.azure.net/
```

## <a name="recover-undelete-a-deleted-key"></a>Silinen bir anahtarı kurtarma (geri alma)

`az keyvault key list-deleted`YÖNETILEN HSM 'inizdeki tüm anahtarları silinen durumda listelemek için komutunu kullanın. Silinen bir anahtarı kurtarırken--id parametresini kullanarak bir anahtarı kurtarmanız (geri almak) gerekirse, `recoveryId` komuttan elde edilen silinen anahtarın değerini aklınızda sağlamalısınız `az keyvault key list-deleted` .

```azurecli-interactive
az keyvault key recover --hsm-name ContosoHSM --name myrsakey

## OR
# Note the key name (myaeskey) in the URI

az keyvault key recover --id https://ContosoMHSM.managedhsm.azure.net/deletedKeys/myrsakey
```

## <a name="purge-permanently-delete-a-key"></a>Bir anahtarı temizle (kalıcı olarak sil)

`az keyvault key purge`Bir anahtarı temizlemek (kalıcı olarak silmek) için komutunu kullanın.

> [!NOTE]
> Yönetilen HSM 'nin Temizleme koruması etkinse, temizleme işlemine izin verilmez. Saklama süresi geçtiğinde anahtar otomatik olarak temizlenir.

```azurecli-interactive
az keyvault key purge --hsm-name ContosoHSM --name myrsakey

## OR
# Note the key name (myaeskey) in the URI

az keyvault key recover --id https://ContosoMHSM.managedhsm.azure.net/deletedKeys/myrsakey

```

## <a name="create-a-single-key-backup"></a>Tek bir anahtar yedeklemesi oluşturun

`az keyvault key backup`Anahtar yedeklemesi oluşturmak için kullanın. Yedekleme dosyası, kaynak HSM 'nin güvenlik etki alanına bağlı şifreli bir blob şifrelenir. Yalnızca aynı güvenlik etki alanını paylaşan HSM 'ler için geri yüklenebilir. [Güvenlik etki alanı](security-domain.md)hakkında daha fazla bilgi edinin.

```azurecli-interactive
az keyvault key backup --hsm-name ContosoHSM --name myrsakey --file myrsakey.backup

## OR
# Note the key name (myaeskey) in the URI

az keyvault key backup --id https://ContosoMHSM.managedhsm.azure.net/deletedKeys/myrsakey  --file myrsakey.backup

```

## <a name="restore-a-single-key-from-backup"></a>Yedeklemeden tek bir anahtarı geri yükleme

`az keyvault key restore`Tek bir anahtarı geri yüklemek için kullanın. Yedeklemenin oluşturulduğu kaynak HSM, anahtarın geri yüklendiği hedef HSM ile aynı güvenlik etki alanını paylaşmalıdır.

> [!NOTE]
> Etkin veya silinmiş durumda aynı ada sahip bir anahtar varsa geri yükleme başarılı olmaz.

```azurecli-interactive
az keyvault key restore --hsm-name ContosoHSM --name myrsakey --file myrsakey.bakup

## OR
# Note the key name (myaeskey) in the URI

az keyvault key recover --id https://ContosoMHSM.managedhsm.azure.net/deletedKeys/myrsakey --file myrsakey.bakup

```

## <a name="import-a-key-from-a-file"></a>Dosyadan anahtar içeri aktarma

`az keyvault key import`Bir dosyadan anahtar (yalnızca RSA ve EC) almak için komutunu kullanın. Sertifika dosyası özel anahtara sahip olmalı ve ped kodlaması (RFC [1421](https://tools.ietf.org/html/rfc1421), [1422](https://tools.ietf.org/html/rfc1422), [1423](https://tools.ietf.org/html/rfc1423), [1424](https://tools.ietf.org/html/rfc1424)) kullanması gerekir.

```azurecli-interactive
az keyvault key import --hsm-name ContosoHSM --name myrsakey --pem-file mycert.key --password 'mypassword'

## OR
# Note the key name (myaeskey) in the URI

az keyvault key recover --id https://ContosoMHSM.managedhsm.azure.net/deletedKeys/myrsakey --pem-file mycert.key --password 'mypassword'
```

Şirket içi HSM 'nizden yönetilen HSM 'ye bir anahtar aktarmak için bkz. [HSM korumalı anahtarları YÖNETILEN HSM 'ye Içeri aktarma (BYOK)](hsm-protected-keys-byok.md)

## <a name="next-steps"></a>Sonraki adımlar

- Anahtar Kasası komutlarına yönelik tüm Azure CLı başvurusu için bkz. [Key Vault CLI başvurusu](/cli/azure/keyvault).
- Programlama başvuruları için [Azure Key Vault Geliştirici Kılavuzu](../general/developers-guide.md) ' na bakın.
- [YÖNETILEN HSM rol yönetimi](role-management.md) hakkında daha fazla bilgi edinin
- [YÖNETILEN HSM en iyi uygulamaları](best-practices.md) hakkında daha fazla bilgi edinin
