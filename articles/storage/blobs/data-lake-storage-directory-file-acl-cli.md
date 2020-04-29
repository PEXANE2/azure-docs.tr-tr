---
title: Azure Data Lake Storage 2. & ACL 'Leri için Azure CLı kullanma (Önizleme)
description: Hiyerarşik bir ad alanına sahip depolama hesaplarında dizinleri ve dosya ve Dizin erişim denetim listelerini (ACL) yönetmek için Azure CLı 'yi kullanın.
services: storage
author: normesta
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.topic: conceptual
ms.date: 11/24/2019
ms.author: normesta
ms.reviewer: prishet
ms.openlocfilehash: ce2b4200496938e6cffb935207df8c7027eaf37a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77486143"
---
# <a name="use-azure-cli-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2-preview"></a>Azure CLı kullanarak Azure Data Lake Storage 2. dizinleri, dosyaları ve ACL 'Leri yönetme (Önizleme)

Bu makalede, [Azure komut satırı arabirimi 'ni (CLI)](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) hiyerarşik bir ad alanına sahip depolama hesaplarında Dizin, dosya ve izinleri oluşturmak ve yönetmek için nasıl kullanacağınız gösterilmektedir. 

> [!IMPORTANT]
> Bu `storage-preview` makalede tanıtılan uzantı şu anda genel önizleme aşamasındadır.

[Örnek](https://github.com/Azure/azure-cli-extensions/tree/master/src/storage-preview#adls-gen2-support) | [Gen1 to Gen2 Mapping](https://github.com/Azure/azure-cli-extensions/tree/master/src/storage-preview#mapping-from-adls-gen1-to-adls-gen2) | [geri bildirimde](https://github.com/Azure/azure-cli-extensions/issues) bulunun
## <a name="prerequisites"></a>Ön koşullar

> [!div class="checklist"]
> * Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
> * Hiyerarşik ad alanı (HNS) etkin olan bir depolama hesabı. Bir tane oluşturmak için [Bu](data-lake-storage-quickstart-create-account.md) yönergeleri izleyin.
> * Azure CLı sürümü `2.0.67` veya üzeri.

## <a name="install-the-storage-cli-extension"></a>Storage CLı uzantısını yükler

1. [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest)açın veya Azure CLI 'yı yerel olarak [yüklediyseniz](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) , Windows PowerShell gibi bir komut konsol uygulaması açın.

2. Yüklü olan Azure CLı sürümünün aşağıdaki komutu kullanarak `2.0.67` veya daha yüksek olduğunu doğrulayın.

   ```azurecli
    az --version
   ```
   Azure CLı sürümünüz daha düşükse `2.0.67`, daha sonra yeni bir sürüm yüklersiniz. Bkz. [Azure CLI 'Yi yüklemeyi](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

3. `storage-preview` uzantısını yükleyin.

   ```azurecli
   az extension add -n storage-preview
   ```

## <a name="connect-to-the-account"></a>Hesaba Bağlan

1. Azure CLı 'yi yerel olarak kullanıyorsanız Login komutunu çalıştırın.

   ```azurecli
   az login
   ```

   CLı varsayılan tarayıcınızı açabildiğinden, bunu yapılır ve bir Azure oturum açma sayfası yükler.

   Aksi takdirde, konumunda [https://aka.ms/devicelogin](https://aka.ms/devicelogin) bir tarayıcı sayfası açın ve terminalinizde görünen yetkilendirme kodunu girin. Ardından, tarayıcıda hesap kimlik bilgilerinizle oturum açın.

   Farklı kimlik doğrulama yöntemleri hakkında daha fazla bilgi için bkz. Azure CLI'de oturum açma.

2. Kimliğiniz birden fazla abonelikle ilişkiliyse, etkin aboneliğinizi statik Web sitenizi barındıracak depolama hesabının aboneliğine ayarlayın.

   ```azurecli
   az account set --subscription <subscription-id>
   ```

   `<subscription-id>` Yer tutucu DEĞERINI aboneliğinizin kimliğiyle değiştirin.

## <a name="create-a-file-system"></a>Dosya sistemi oluşturma

Dosya sistemi dosyalarınız için bir kapsayıcı olarak davranır. `az storage container create` Komutunu kullanarak bir tane oluşturabilirsiniz. 

Bu örnek adlı `my-file-system`bir dosya sistemi oluşturur.

```azurecli
az storage container create --name my-file-system --account-name mystorageaccount
```

## <a name="create-a-directory"></a>Dizin oluşturma

`az storage blob directory create` Komutunu kullanarak bir dizin başvurusu oluşturun. 

Bu örnek, adlı bir hesapta `my-directory` bulunan adlı `my-file-system` bir dosya sistemine adlı bir dizin ekler `mystorageaccount`.

```azurecli
az storage blob directory create -c my-file-system -d my-directory --account-name mystorageaccount
```

## <a name="show-directory-properties"></a>Dizin özelliklerini göster

`az storage blob show` Komutunu kullanarak bir dizinin özelliklerini konsola yazdırabilirsiniz.

```azurecli
az storage blob directory show -c my-file-system -d my-directory --account-name mystorageaccount
```

## <a name="rename-or-move-a-directory"></a>Bir dizini yeniden adlandırma veya taşıma

`az storage blob directory move` Komutunu kullanarak bir dizini yeniden adlandırın veya taşıyın.

Bu örnek, adından `my-directory` adı olan bir dizini yeniden adlandırır. `my-new-directory`

```azurecli
az storage blob directory move -c my-file-system -d my-new-directory -s my-directory --account-name mystorageaccount
```

## <a name="delete-a-directory"></a>Bir dizini silme

`az storage blob directory delete` Komutunu kullanarak bir dizini silin.

Bu örnek adlı `my-directory`bir dizini siler. 

```azurecli
az storage blob directory delete -c my-file-system -d my-directory --account-name mystorageaccount 
```

## <a name="check-if-a-directory-exists"></a>Bir dizinin mevcut olup olmadığını denetle

Dosya sisteminde, `az storage blob directory exist` komutunu kullanarak belirli bir dizinin mevcut olup olmadığını saptayın.

Bu örnek, `my-directory` `my-file-system` adlı bir dizinin dosya sisteminde mevcut olup olmadığını gösterir. 

```azurecli
az storage blob directory exists -c my-file-system -d my-directory --account-name mystorageaccount 
```

## <a name="download-from-a-directory"></a>Bir dizinden indir

`az storage blob directory download` Komutunu kullanarak bir dizinden dosya indirin.

Bu örnek, adlı `upload.txt` `my-directory`bir dizinden adlı bir dosyayı indirir. 

```azurecli
az storage blob directory download -c my-file-system --account-name mystorageaccount -s "my-directory/upload.txt" -d "C:\mylocalfolder\download.txt"
```

Bu örnek, tüm bir dizini indirir.

```azurecli
az storage blob directory download -c my-file-system --account-name mystorageaccount -s "my-directory/" -d "C:\mylocalfolder" --recursive
```

## <a name="list-directory-contents"></a>Dizin içeriğini listeleme

`az storage blob directory list` Komutunu kullanarak bir dizinin içeriğini listeleyin.

Bu örnek, adlı bir depolama hesabının `my-directory` `my-file-system` dosya sisteminde bulunan adlı bir dizinin içeriğini listeler. `mystorageaccount` 

```azurecli
az storage blob directory list -c my-file-system -d my-directory --account-name mystorageaccount
```

## <a name="upload-a-file-to-a-directory"></a>Dizine dosya yükleme

`az storage blob directory upload` Komutunu kullanarak bir dosyayı dizine yükleyin.

Bu örnek adlı bir dizine adlı `upload.txt` bir dosyayı yükler `my-directory`. 

```azurecli
az storage blob directory upload -c my-file-system --account-name mystorageaccount -s "C:\mylocaldirectory\upload.txt" -d my-directory
```

Bu örnek, tüm bir dizini yükler.

```azurecli
az storage blob directory upload -c my-file-system --account-name mystorageaccount -s "C:\mylocaldirectory\" -d my-directory --recursive 
```

## <a name="show-file-properties"></a>Dosya özelliklerini göster

Bir dosyanın özelliklerini, `az storage blob show` komutunu kullanarak konsola yazdırabilirsiniz.

```azurecli
az storage blob show -c my-file-system -b my-file.txt --account-name mystorageaccount
```

## <a name="rename-or-move-a-file"></a>Bir dosyayı yeniden adlandırma veya taşıma

`az storage blob move` Komutunu kullanarak bir dosyayı yeniden adlandırın veya taşıyın.

Bu örnek, adından `my-file.txt` adı olan bir dosyayı yeniden adlandırır. `my-file-renamed.txt`

```azurecli
az storage blob move -c my-file-system -d my-file-renamed.txt -s my-file.txt --account-name mystorageaccount
```

## <a name="delete-a-file"></a>Dosyayı silme

`az storage blob delete` Komutunu kullanarak bir dosyayı silin.

Bu örnek, adlı dosyayı siler`my-file.txt`

```azurecli
az storage blob delete -c my-file-system -b my-file.txt --account-name mystorageaccount 
```

## <a name="manage-permissions"></a>İzinleri yönet

Dizinler ve dosyalar için erişim izinlerini alabilir, ayarlayabilir ve güncelleştirebilirsiniz.

> [!NOTE]
> Komutları yetkilendirmek için Azure Active Directory (Azure AD) kullanıyorsanız, güvenlik sorumlusuna [Depolama Blobu veri sahibi rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)atandığından emin olun. ACL izinlerinin nasıl uygulandığı ve bunların nasıl değiştirileceği hakkında daha fazla bilgi edinmek için [Azure Data Lake Storage 2. erişim denetimi](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)' ne bakın.

### <a name="get-directory-and-file-permissions"></a>Dizin ve dosya izinlerini alın

Komutunu kullanarak bir dizinin ACL 'sini alın. **directory** `az storage blob directory access show`

Bu örnek, bir dizinin ACL 'sini alır ve ardından ACL 'yi konsola yazdırır.

```azurecli
az storage blob directory access show -d my-directory -c my-file-system --account-name mystorageaccount
```

Komutunu kullanarak bir dosyanın erişim izinlerini alın. **file** `az storage blob access show` 

Bu örnek, bir dosyanın ACL 'sini alır ve ardından ACL 'yi konsola yazdırır.

```azurecli
az storage blob access show -b my-directory/upload.txt -c my-file-system --account-name mystorageaccount
```

Aşağıdaki görüntüde bir dizinin ACL 'SI alındıktan sonra çıkış gösterilmektedir.

![ACL çıkışı al](./media/data-lake-storage-directory-file-acl-cli/get-acl.png)

Bu örnekte, sahip olan kullanıcının okuma, yazma ve yürütme izinleri vardır. Sahip olan grubun yalnızca okuma ve yürütme izinleri vardır. Erişim denetim listeleri hakkında daha fazla bilgi için bkz. [Azure Data Lake Storage 2. Access Control](data-lake-storage-access-control.md).

### <a name="set-directory-and-file-permissions"></a>Dizin ve dosya izinlerini ayarlama

Bir dizinin `az storage blob directory access set` ACL 'sini ayarlamak için komutunu kullanın. **directory** 

Bu örnek, ACL 'yi sahip olan Kullanıcı, sahip olan grup veya diğer kullanıcılar için bir dizinde ayarlar ve ardından ACL 'yi konsola yazdırır.

```azurecli
az storage blob directory access set -a "user::rw-,group::rw-,other::-wx" -d my-directory -c my-file-system --account-name mystorageaccount
```

Bu örnek, sahip olan Kullanıcı, sahip olan grup veya diğer kullanıcılar için bir dizinde *varsayılan* ACL 'yi ayarlar ve ardından ACL 'yi konsola yazdırır.

```azurecli
az storage blob directory access set -a "default:user::rw-,group::rw-,other::-wx" -d my-directory -c my-file-system --account-name mystorageaccount
```

Bir dosyanın `az storage blob access set` ACL 'sini ayarlamak için komutunu kullanın. **file** 

Bu örnek, sahip olan Kullanıcı, sahip olan grup veya diğer kullanıcılar için bir dosyadaki ACL 'yi ayarlar ve ardından ACL 'yi konsola yazdırır.

```azurecli
az storage blob access set -a "user::rw-,group::rw-,other::-wx" -b my-directory/upload.txt -c my-file-system --account-name mystorageaccount
```
Aşağıdaki görüntüde, bir dosyanın ACL 'sini ayarlamadıktan sonra çıkış gösterilmektedir.

![ACL çıkışı al](./media/data-lake-storage-directory-file-acl-cli/set-acl-file.png)

Bu örnekte, sahip olan Kullanıcı ve sahip olan Grup yalnızca okuma ve yazma izinlerine sahiptir. Diğer tüm kullanıcılar yazma ve yürütme izinlerine sahiptir. Erişim denetim listeleri hakkında daha fazla bilgi için bkz. [Azure Data Lake Storage 2. Access Control](data-lake-storage-access-control.md).

### <a name="update-directory-and-file-permissions"></a>Dizin ve dosya izinlerini güncelleştirme

Bu izni ayarlamaya yönelik başka bir yol da `az storage blob directory access update` veya `az storage blob access update` komutunu kullanmaktır. 

Bir ACL 'nin kısa biçimine `-permissions` parametresini ayarlayarak bir dizin veya dosyanın ACL 'sini güncelleştirin.

Bu örnek, bir **DIZININ**ACL 'sini günceller.

```azurecli
az storage blob directory access update --permissions "rwxrwxrwx" -d my-directory -c my-file-system --account-name mystorageaccount
```

Bu örnek, bir **DOSYANıN**ACL 'sini günceller.

```azurecli
az storage blob access update --permissions "rwxrwxrwx" -b my-directory/upload.txt -c my-file-system --account-name mystorageaccount
```

Ayrıca, `--owner` bir kullanıcının varlık kimliğine veya Kullanıcı asıl ADıNA (UPN) veya `group` parametrelerini ayarlayarak bir dizin ya da dosya için sahip olan Kullanıcı ve grubu güncelleştirebilirsiniz. 

Bu örnek, bir dizinin sahibini değiştirir. 

```azurecli
az storage blob directory access update --owner xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -d my-directory -c my-file-system --account-name mystorageaccount
```

Bu örnek, bir dosyanın sahibini değiştirir. 

```azurecli
az storage blob access update --owner xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -b my-directory/upload.txt -c my-file-system --account-name mystorageaccount
```
## <a name="manage-user-defined-metadata"></a>Kullanıcı tanımlı meta verileri yönetme

Bir veya daha fazla ad-değer çiftiyle `az storage blob directory metadata update` komutunu kullanarak bir dosyaya veya dizine kullanıcı tanımlı meta veriler ekleyebilirsiniz.

Bu örnek, dizin adlı `my-directory` bir dizin için Kullanıcı tanımlı meta veri ekler.

```azurecli
az storage blob directory metadata update --metadata tag1=value1 tag2=value2 -c my-file-system -d my-directory --account-name mystorageaccount
```

Bu örnek, adlı `my-directory`dizin için Kullanıcı tanımlı tüm meta verileri gösterir.

```azurecli
az storage blob directory metadata show -c my-file-system -d my-directory --account-name mystorageaccount
```

## <a name="see-also"></a>Ayrıca bkz.

* [Örnek](https://github.com/Azure/azure-cli-extensions/tree/master/src/storage-preview)
* [Gen1 to Gen2 Mapping](https://github.com/Azure/azure-cli-extensions/tree/master/src/storage-preview#mapping-from-adls-gen1-to-adls-gen2)
* [Geri bildirim gönderme](https://github.com/Azure/azure-cli-extensions/issues)
* [Bilinen sorunlar](data-lake-storage-known-issues.md#api-scope-data-lake-client-library)
* [Kaynak kod](https://github.com/Azure/azure-cli-extensions/tree/master/src)

