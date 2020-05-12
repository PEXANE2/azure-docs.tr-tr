---
title: Azure Data Lake Storage 2. içindeki ACL 'Ler & için Azure CLı 'yi kullanma
description: Hiyerarşik bir ad alanına sahip depolama hesaplarında dizinleri ve dosya ve Dizin erişim denetim listelerini (ACL) yönetmek için Azure CLı 'yi kullanın.
services: storage
author: normesta
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: normesta
ms.reviewer: prishet
ms.openlocfilehash: 79a87f02c8730e0b2110e7475de721f11beda568
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83120615"
---
# <a name="use-azure-cli-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2"></a>Azure CLı kullanarak Azure Data Lake Storage 2. dizinleri, dosyaları ve ACL 'Leri yönetme

Bu makalede, [Azure komut satırı arabirimi 'ni (CLI)](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) hiyerarşik bir ad alanına sahip depolama hesaplarında Dizin, dosya ve izinleri oluşturmak ve yönetmek için nasıl kullanacağınız gösterilmektedir. 

[Gen1 to Gen2 Mapping](https://github.com/Azure/azure-cli-extensions/tree/master/src/storage-preview#mapping-from-adls-gen1-to-adls-gen2)  |  [Geri bildirimde](https://github.com/Azure/azure-cli-extensions/issues) bulunun

## <a name="prerequisites"></a>Ön koşullar

> [!div class="checklist"]
> * Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
> * Hiyerarşik ad alanı (HNS) etkin olan bir depolama hesabı. Bir tane oluşturmak için [Bu](data-lake-storage-quickstart-create-account.md) yönergeleri izleyin.
> * Azure CLı sürümü `2.5.1` veya üzeri.

## <a name="ensure-that-you-have-the-correct-version-of-azure-cli-installed"></a>Azure CLı 'nin doğru sürümünün yüklü olduğundan emin olun

1. [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest)açın veya Azure CLI 'yı yerel olarak [yüklediyseniz](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) , Windows PowerShell gibi bir komut konsol uygulaması açın.

2. Yüklü olan Azure CLı sürümünün `2.5.1` aşağıdaki komutu kullanarak veya daha yüksek olduğunu doğrulayın.

   ```azurecli
    az --version
   ```
   Azure CLı sürümünüz daha düşükse `2.5.1` , daha sonra yeni bir sürüm yüklersiniz. Bkz. [Azure CLI 'Yi yüklemeyi](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="connect-to-the-account"></a>Hesaba Bağlan

1. Azure CLı 'yi yerel olarak kullanıyorsanız Login komutunu çalıştırın.

   ```azurecli
   az login
   ```

   CLı varsayılan tarayıcınızı açabildiğinden, bunu yapılır ve bir Azure oturum açma sayfası yükler.

   Aksi takdirde, konumunda bir tarayıcı sayfası açın [https://aka.ms/devicelogin](https://aka.ms/devicelogin) ve terminalinizde görünen yetkilendirme kodunu girin. Ardından, tarayıcıda hesap kimlik bilgilerinizle oturum açın.

   Farklı kimlik doğrulama yöntemleri hakkında daha fazla bilgi edinmek için bkz. [Azure CLI ile blob veya kuyruk verilerine erişim yetkisi verme](../common/authorize-data-operations-cli.md).

2. Kimliğiniz birden fazla abonelikle ilişkiliyse, etkin aboneliğinizi statik Web sitenizi barındıracak depolama hesabının aboneliğine ayarlayın.

   ```azurecli
   az account set --subscription <subscription-id>
   ```

   `<subscription-id>`Yer tutucu değerini ABONELIĞINIZIN kimliğiyle değiştirin.

> [!NOTE]
> Bu makalede sunulan örnekte Azure Active Directory (AD) yetkilendirmesi gösterilmektedir. Yetkilendirme yöntemleri hakkında daha fazla bilgi edinmek için bkz. [Azure CLI ile blob veya kuyruk verilerine erişim yetkisi verme](../common/authorize-data-operations-cli.md).

## <a name="create-a-file-system"></a>Dosya sistemi oluşturma

Dosya sistemi dosyalarınız için bir kapsayıcı olarak davranır. Komutunu kullanarak bir tane oluşturabilirsiniz `az storage fs create` . 

Bu örnek adlı bir dosya sistemi oluşturur `my-file-system` .

```azurecli
az storage fs create -n my-file-system --account-name mystorageaccount --auth-mode login
```

## <a name="show-file-system-properties"></a>Dosya sistemi özelliklerini göster

Bir dosya sisteminin özelliklerini, komutunu kullanarak konsola yazdırabilirsiniz `az storage fs show` .

```azurecli
az storage fs show -n my-file-system --account-name mystorageaccount --auth-mode login
```

## <a name="list-file-system-contents"></a>Dosya sistemi içeriğini listeleme

Komutunu kullanarak bir dizinin içeriğini listeleyin `az storage fs file list` .

Bu örnek, adlı bir dosya sisteminin içeriğini listeler `my-file-system` .

```azurecli
az storage fs file list -f my-file-system --account-name mystorageaccount --auth-mode login
```

## <a name="delete-a-file-system"></a>Dosya sistemini silme

Komutunu kullanarak bir dosya sistemini silin `az storage fs delete` .

Bu örnekte adlı bir dosya sistemi silinir `my-file-system` . 

```azurecli
az storage fs delete -n my-file-system --account-name mystorageaccount --auth-mode login
```

## <a name="create-a-directory"></a>Dizin oluşturma

Komutunu kullanarak bir dizin başvurusu oluşturun `az storage fs directory create` . 

Bu örnek, adlı bir `my-directory` hesapta bulunan adlı bir dosya sistemine adlı bir dizin ekler `my-file-system` `mystorageaccount` .

```azurecli
az storage fs directory create -n my-directory -f my-file-system --account-name mystorageaccount --auth-mode login
```

## <a name="show-directory-properties"></a>Dizin özelliklerini göster

Komutunu kullanarak bir dizinin özelliklerini konsola yazdırabilirsiniz `az storage fs directory show` .

```azurecli
az storage fs directory show -n my-directory -f my-file-system --account-name mystorageaccount --auth-mode login
```

## <a name="rename-or-move-a-directory"></a>Bir dizini yeniden adlandırma veya taşıma

Komutunu kullanarak bir dizini yeniden adlandırın veya taşıyın `az storage fs directory move` .

Bu örnek, adından `my-directory` aynı dosya sistemindeki adı bir dizini yeniden adlandırır `my-new-directory` .

```azurecli
az storage fs directory move -n my-directory -f my-file-system --new-directory "my-file-system/my-new-directory" --account-name mystorageaccount --auth-mode login
```

Bu örnek, bir dizini adlı bir dosya sistemine taşır `my-second-file-system` .

```azurecli
az storage fs directory move -n my-directory -f my-file-system --new-directory "my-second-file-system/my-new-directory" --account-name mystorageaccount --auth-mode login
```

## <a name="delete-a-directory"></a>Bir dizini silme

Komutunu kullanarak bir dizini silin `az storage fs directory delete` .

Bu örnek adlı bir dizini siler `my-directory` . 

```azurecli
az storage fs directory delete -n my-directory -f my-file-system  --account-name mystorageaccount --auth-mode login 
```

## <a name="check-if-a-directory-exists"></a>Bir dizinin mevcut olup olmadığını denetle

Dosya sisteminde, komutunu kullanarak belirli bir dizinin mevcut olup olmadığını saptayın `az storage fs directory exists` .

Bu örnek, adlı bir dizinin `my-directory` dosya sisteminde mevcut olup olmadığını gösterir `my-file-system` . 

```azurecli
az storage fs directory exists -n my-directory -f my-file-system --account-name mystorageaccount --auth-mode login 
```

## <a name="download-from-a-directory"></a>Bir dizinden indir

Komutunu kullanarak bir dizinden dosya indirin `az storage fs file download` .

Bu örnek, `upload.txt` adlı bir dizinden adlı bir dosyayı indirir `my-directory` . 

```azurecli
az storage fs file download -p my-directory/upload.txt -f my-file-system -d "C:\myFolder\download.txt" --account-name mystorageaccount --auth-mode login
```

## <a name="list-directory-contents"></a>Dizin içeriğini listeleme

Komutunu kullanarak bir dizinin içeriğini listeleyin `az storage fs file list` .

Bu örnek, adlı `my-directory` `my-file-system` bir depolama hesabının dosya sisteminde bulunan adlı bir dizinin içeriğini listeler `mystorageaccount` . 

```azurecli
az storage fs file list -f my-file-system --path my-directory --account-name mystorageaccount --auth-mode login
```

## <a name="upload-a-file-to-a-directory"></a>Dizine dosya yükleme

Komutunu kullanarak bir dosyayı dizine yükleyin `az storage fs directory upload` .

Bu örnek adlı bir dizine adlı bir dosyayı yükler `upload.txt` `my-directory` . 

```azurecli
az storage fs file upload -s "C:\myFolder\upload.txt" -p my-directory/upload.txt  -f my-file-system --account-name mystorageaccount --auth-mode login
```

## <a name="show-file-properties"></a>Dosya özelliklerini göster

Bir dosyanın özelliklerini, komutunu kullanarak konsola yazdırabilirsiniz `az storage fs file show` .

```azurecli
az storage fs file show -p my-file.txt -f my-file-system --account-name mystorageaccount --auth-mode login
```

## <a name="rename-or-move-a-file"></a>Bir dosyayı yeniden adlandırma veya taşıma

Komutunu kullanarak bir dosyayı yeniden adlandırın veya taşıyın `az storage fs file move` .

Bu örnek, adından adı olan bir dosyayı yeniden adlandırır `my-file.txt` `my-file-renamed.txt` .

```azurecli
az storage fs file move -p my-file.txt -f my-file-system --new-path my-file-system/my-file-renamed.txt --account-name mystorageaccount --auth-mode login
```

## <a name="delete-a-file"></a>Dosyayı silme

Komutunu kullanarak bir dosyayı silin `az storage fs file delete` .

Bu örnek, adlı dosyayı siler`my-file.txt`

```azurecli
az storage fs file delete -p my-directory/my-file.txt -f my-file-system  --account-name mystorageaccount --auth-mode login 
```

## <a name="manage-permissions"></a>İzinleri yönet

Dizinler ve dosyalar için erişim izinlerini alabilir, ayarlayabilir ve güncelleştirebilirsiniz.

> [!NOTE]
> Komutları yetkilendirmek için Azure Active Directory (Azure AD) kullanıyorsanız, güvenlik sorumlusuna [Depolama Blobu veri sahibi rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)atandığından emin olun. ACL izinlerinin nasıl uygulandığı ve bunların nasıl değiştirileceği hakkında daha fazla bilgi edinmek için [Azure Data Lake Storage 2. erişim denetimi](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)' ne bakın.

### <a name="get-an-acl"></a>ACL al

Komutunu kullanarak bir **DIZININ** ACL 'sini alın `az storage fs access show` .

Bu örnek, bir dizinin ACL 'sini alır ve ardından ACL 'yi konsola yazdırır.

```azurecli
az storage fs access show -p my-directory -f my-file-system --account-name mystorageaccount --auth-mode login
```

Komutunu kullanarak bir **dosyanın** erişim izinlerini alın `az storage fs access show` . 

Bu örnek, bir dosyanın ACL 'sini alır ve ardından ACL 'yi konsola yazdırır.

```azurecli
az storage fs access show -p my-directory/upload.txt -f my-file-system --account-name mystorageaccount --auth-mode login
```

Aşağıdaki görüntüde bir dizinin ACL 'SI alındıktan sonra çıkış gösterilmektedir.

![ACL çıkışı al](./media/data-lake-storage-directory-file-acl-cli/get-acl.png)

Bu örnekte, sahip olan kullanıcının okuma, yazma ve yürütme izinleri vardır. Sahip olan grubun yalnızca okuma ve yürütme izinleri vardır. Erişim denetim listeleri hakkında daha fazla bilgi için bkz. [Azure Data Lake Storage 2. Access Control](data-lake-storage-access-control.md).

### <a name="set-an-acl"></a>ACL ayarla

`az storage fs access set`Bir **dizinin**ACL 'sini ayarlamak için komutunu kullanın. 

Bu örnek, ACL 'yi sahip olan Kullanıcı, sahip olan grup veya diğer kullanıcılar için bir dizinde ayarlar ve ardından ACL 'yi konsola yazdırır.

```azurecli
az storage fs access set --acl "user::rw-,group::rw-,other::-wx" -p my-directory -f my-file-system --account-name mystorageaccount --auth-mode login
```

Bu örnek, sahip olan Kullanıcı, sahip olan grup veya diğer kullanıcılar için bir dizinde *varsayılan* ACL 'yi ayarlar ve ardından ACL 'yi konsola yazdırır.

```azurecli
az storage fs access set --acl "default:user::rw-,group::rw-,other::-wx" -p my-directory -f my-file-system --account-name mystorageaccount --auth-mode login
```

`az storage fs access set`Bir **dosyanın**ACL 'sini ayarlamak için komutunu kullanın. 

Bu örnek, sahip olan Kullanıcı, sahip olan grup veya diğer kullanıcılar için bir dosyadaki ACL 'yi ayarlar ve ardından ACL 'yi konsola yazdırır.

```azurecli
az storage fs access set --acl "user::rw-,group::rw-,other::-wx" -p my-directory/upload.txt -f my-file-system --account-name mystorageaccount --auth-mode login
```

Aşağıdaki görüntüde, bir dosyanın ACL 'sini ayarlamadıktan sonra çıkış gösterilmektedir.

![ACL çıkışı al](./media/data-lake-storage-directory-file-acl-cli/set-acl-file.png)

Bu örnekte, sahip olan Kullanıcı ve sahip olan Grup yalnızca okuma ve yazma izinlerine sahiptir. Diğer tüm kullanıcılar yazma ve yürütme izinlerine sahiptir. Erişim denetim listeleri hakkında daha fazla bilgi için bkz. [Azure Data Lake Storage 2. Access Control](data-lake-storage-access-control.md).

### <a name="update-an-acl"></a>ACL güncelleştirme

Bu izni ayarlamaya yönelik başka bir yol ise komutunu kullanmaktır `az storage fs access set` . 

Bir `-permissions` ACL 'nin kısa biçimine parametresini ayarlayarak bir dizin veya DOSYANıN ACL 'sini güncelleştirin.

Bu örnek, bir **DIZININ**ACL 'sini günceller.

```azurecli
az storage fs access set --permissions rwxrwxrwx -p my-directory -f my-file-system --account-name mystorageaccount --auth-mode login
```

Bu örnek, bir **DOSYANıN**ACL 'sini günceller.

```azurecli
az storage fs access set --permissions rwxrwxrwx -p my-directory/upload.txt -f my-file-system --account-name mystorageaccount --auth-mode login
```

Ayrıca, `--owner` `group` bir kullanıcının varlık kimliğine veya Kullanıcı asıl ADıNA (UPN) veya parametrelerini ayarlayarak bir dizin ya da dosya için sahip olan Kullanıcı ve grubu güncelleştirebilirsiniz. 

Bu örnek, bir dizinin sahibini değiştirir. 

```azurecli
az storage fs access set --owner xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p my-directory -f my-file-system --account-name mystorageaccount --auth-mode login
```

Bu örnek, bir dosyanın sahibini değiştirir. 

```azurecli
az storage fs access set --owner xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p my-directory/upload.txt -f my-file-system --account-name mystorageaccount --auth-mode login
```

## <a name="see-also"></a>Ayrıca bkz.

* [Gen1 to Gen2 Mapping](https://github.com/Azure/azure-cli-extensions/tree/master/src/storage-preview#mapping-from-adls-gen1-to-adls-gen2)
* [Geri bildirim gönderme](https://github.com/Azure/azure-cli-extensions/issues)
* [Bilinen sorunlar](data-lake-storage-known-issues.md#api-scope-data-lake-client-library)


