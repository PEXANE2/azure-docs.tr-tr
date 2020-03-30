---
title: Azure Veri Gölü Depolama Gen2'deki dosyalar & Aç'lar için Azure CLI'yi kullanın (önizleme)
description: Hiyerarşik ad alanına sahip depolama hesaplarında dizinleri ve dosya ve dizin erişim denetim listelerini (ACL) yönetmek için Azure CLI'yi kullanın.
services: storage
author: normesta
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.topic: conceptual
ms.date: 11/24/2019
ms.author: normesta
ms.reviewer: prishet
ms.openlocfilehash: ce2b4200496938e6cffb935207df8c7027eaf37a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77486143"
---
# <a name="use-azure-cli-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2-preview"></a>Azure Veri Gölü Depolama Gen2'deki dizinleri, dosyaları ve ACD'leri yönetmek için Azure CLI'yi kullanın (önizleme)

Bu makalede, hiyerarşik ad alanına sahip depolama hesaplarında dizinler, dosyalar ve izinler oluşturmak ve yönetmek için [Azure Komut Satırı Arabirimi'ni (CLI)](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) nasıl kullanacağınızı gösterir. 

> [!IMPORTANT]
> Bu `storage-preview` makalede öne çıkan uzantı şu anda genel önizlemededir.

[Örnek](https://github.com/Azure/azure-cli-extensions/tree/master/src/storage-preview#adls-gen2-support) | [Gen1- Gen2 haritalama](https://github.com/Azure/azure-cli-extensions/tree/master/src/storage-preview#mapping-from-adls-gen1-to-adls-gen2) | [Geri bildirim de ver](https://github.com/Azure/azure-cli-extensions/issues)
## <a name="prerequisites"></a>Ön koşullar

> [!div class="checklist"]
> * Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
> * Hiyerarşik ad alanı (HNS) etkinleştirilmiş bir depolama hesabı. Oluşturmak için [bu](data-lake-storage-quickstart-create-account.md) yönergeleri izleyin.
> * Azure CLI `2.0.67` sürümü veya daha yüksek.

## <a name="install-the-storage-cli-extension"></a>Depolama CLI uzantısını yükleme

1. Azure [Bulut Kabuğu'nu](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest)açın veya Azure CLI'yi yerel olarak [yüklediyseniz,](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) Windows PowerShell gibi bir komut konsolu uygulamasını açın.

2. Aşağıdaki komutu kullanarak Azure CLI sürümünün yüklü `2.0.67` veya daha yüksek olduğunu doğrulayın.

   ```azurecli
    az --version
   ```
   Azure CLI sürümünüz daha `2.0.67`düşükse, daha sonraki bir sürümü yükleyin. Bkz. [Azure CLI'yi yükleyin.](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)

3. `storage-preview` uzantısını yükleyin.

   ```azurecli
   az extension add -n storage-preview
   ```

## <a name="connect-to-the-account"></a>Hesaba bağlanın

1. Azure CLI'yi yerel olarak kullanıyorsanız, giriş komutunu çalıştırın.

   ```azurecli
   az login
   ```

   CLI varsayılan tarayıcınızı açabilirse, bunu yapar ve bir Azure oturum açma sayfası yükler.

   Aksi takdirde, terminalinizde görüntülenen yetkilendirme kodunu girin [https://aka.ms/devicelogin](https://aka.ms/devicelogin) ve bir tarayıcı sayfası açın. Ardından, tarayıcıda hesap kimlik bilgilerinizle oturum açın.

   Farklı kimlik doğrulama yöntemleri hakkında daha fazla bilgi için bkz. Azure CLI'de oturum açma.

2. Kimliğiniz birden fazla abonelikle ilişkiliyse, etkin aboneliğinizi statik web sitenizi barındıracak depolama hesabı aboneliğine ayarlayın.

   ```azurecli
   az account set --subscription <subscription-id>
   ```

   `<subscription-id>` Yer tutucu değerini aboneliğinizin kimliğiyle değiştirin.

## <a name="create-a-file-system"></a>Dosya sistemi oluşturma

Dosya sistemi, dosyalarınız için bir kapsayıcı görevi görür. Komutu `az storage container create` kullanarak bir tane oluşturabilirsiniz. 

Bu örnek, .. `my-file-system`

```azurecli
az storage container create --name my-file-system --account-name mystorageaccount
```

## <a name="create-a-directory"></a>Dizin oluşturma

Komutu `az storage blob directory create` kullanarak bir dizin başvurusu oluşturun. 

Bu örnek, adlı `my-directory` `my-file-system` `mystorageaccount`bir hesapta bulunan bir dosya sistemine adlı bir dizin ekler.

```azurecli
az storage blob directory create -c my-file-system -d my-directory --account-name mystorageaccount
```

## <a name="show-directory-properties"></a>Dizin özelliklerini göster

`az storage blob show` Komutu kullanarak bir dizinin özelliklerini konsola yazdırabilirsiniz.

```azurecli
az storage blob directory show -c my-file-system -d my-directory --account-name mystorageaccount
```

## <a name="rename-or-move-a-directory"></a>Bir dizini yeniden adlandırma veya taşıma

Komutu kullanarak bir dizini `az storage blob directory move` yeniden adlandırın veya taşıyın.

Bu örnek, bir dizini `my-directory` adından `my-new-directory`ada yeniden adlandırır.

```azurecli
az storage blob directory move -c my-file-system -d my-new-directory -s my-directory --account-name mystorageaccount
```

## <a name="delete-a-directory"></a>Bir dizini silme

Komutu `az storage blob directory delete` kullanarak bir dizini silin.

Bu örnek, adlı `my-directory`bir dizini siler. 

```azurecli
az storage blob directory delete -c my-file-system -d my-directory --account-name mystorageaccount 
```

## <a name="check-if-a-directory-exists"></a>Dizinin var olup olmadığını denetleme

Komutu kullanarak dosya sisteminde belirli bir dizinin `az storage blob directory exist` var olup olmadığını belirleyin.

Bu örnek, `my-directory` `my-file-system` dosya sisteminde adlandırılmış bir dizinin var olup olmadığını gösterir. 

```azurecli
az storage blob directory exists -c my-file-system -d my-directory --account-name mystorageaccount 
```

## <a name="download-from-a-directory"></a>Bir dizinden indirin

Komutu kullanarak bir dizinden `az storage blob directory download` dosya indirin.

Bu örnek, adlı `upload.txt` `my-directory`bir dizinden adlı bir dosyayı karşıdan yükler. 

```azurecli
az storage blob directory download -c my-file-system --account-name mystorageaccount -s "my-directory/upload.txt" -d "C:\mylocalfolder\download.txt"
```

Bu örnek, tüm bir dizin indirir.

```azurecli
az storage blob directory download -c my-file-system --account-name mystorageaccount -s "my-directory/" -d "C:\mylocalfolder" --recursive
```

## <a name="list-directory-contents"></a>Dizin içeriğini listeleme

Komutu kullanarak bir dizinin içeriğini `az storage blob directory list` listele.

Bu örnek, adlı `my-directory` `my-file-system` `mystorageaccount`bir depolama hesabının dosya sisteminde bulunan adlı bir dizinin içeriğini listeler. 

```azurecli
az storage blob directory list -c my-file-system -d my-directory --account-name mystorageaccount
```

## <a name="upload-a-file-to-a-directory"></a>Dosyayı dizine yükleme

Komutu kullanarak bir dosyayı `az storage blob directory upload` dizine yükleyin.

Bu örnek, adlı `upload.txt` `my-directory`bir dizine adlı bir dosya yükler. 

```azurecli
az storage blob directory upload -c my-file-system --account-name mystorageaccount -s "C:\mylocaldirectory\upload.txt" -d my-directory
```

Bu örnek, tüm bir dizin yükler.

```azurecli
az storage blob directory upload -c my-file-system --account-name mystorageaccount -s "C:\mylocaldirectory\" -d my-directory --recursive 
```

## <a name="show-file-properties"></a>Dosya özelliklerini göster

`az storage blob show` Komutu kullanarak dosyanın özelliklerini konsola yazdırabilirsiniz.

```azurecli
az storage blob show -c my-file-system -b my-file.txt --account-name mystorageaccount
```

## <a name="rename-or-move-a-file"></a>Bir dosyayı yeniden adlandırma veya taşıma

Komutu kullanarak dosyayı yeniden `az storage blob move` adlandırın veya taşıyın.

Bu örnek, bir dosyayı `my-file.txt` adından `my-file-renamed.txt`ada yeniden adlandırır.

```azurecli
az storage blob move -c my-file-system -d my-file-renamed.txt -s my-file.txt --account-name mystorageaccount
```

## <a name="delete-a-file"></a>Dosyayı silme

Komutu kullanarak bir `az storage blob delete` dosyayı silin.

Bu örnek, adlı bir dosyayı siler`my-file.txt`

```azurecli
az storage blob delete -c my-file-system -b my-file.txt --account-name mystorageaccount 
```

## <a name="manage-permissions"></a>İzinleri yönet

Dizinlerin ve dosyaların erişim izinlerini alabilir, ayarlayabilir ve güncelleyebilirsiniz.

> [!NOTE]
> Komutları yetkilendirmek için Azure Active Directory'yi (Azure AD) kullanıyorsanız, güvenlik yöneticinizin [Depolama Blob Veri Sahibi rolüne](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)atandığından emin olun. ACL izinlerinin nasıl uygulandığı ve bunları değiştirmenin etkileri hakkında daha fazla bilgi edinmek için [Azure Veri Gölü Depolama Gen2'de Erişim denetimine](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)bakın.

### <a name="get-directory-and-file-permissions"></a>Dizin ve dosya izinleri alma

Komutu kullanarak bir **dizinin** ACL'sini `az storage blob directory access show` alın.

Bu örnek, bir dizinin ACL'sini alır ve ardından ACL'yi konsola yazdırır.

```azurecli
az storage blob directory access show -d my-directory -c my-file-system --account-name mystorageaccount
```

`az storage blob access show` Komutu kullanarak bir **dosyanın** erişim izinlerini alın. 

Bu örnek, bir dosyanın ACL'sini alır ve acl'yi konsola yazdırır.

```azurecli
az storage blob access show -b my-directory/upload.txt -c my-file-system --account-name mystorageaccount
```

Aşağıdaki resim, bir dizinin ACL'sini aldıktan sonra çıktıyı gösterir.

![ACL çıktısı alın](./media/data-lake-storage-directory-file-acl-cli/get-acl.png)

Bu örnekte, sahibi olan kullanıcı izinleri okumuş, yazar ve yürütemiştir. Sahip olan grup yalnızca izinleri okudu ve çalıştırıyor. Erişim denetimi listeleri hakkında daha fazla bilgi için [Azure Veri Gölü Depolama Gen2'deki Erişim denetimine](data-lake-storage-access-control.md)bakın.

### <a name="set-directory-and-file-permissions"></a>Dizin ve dosya izinlerini ayarlama

Dizinin `az storage blob directory access set` ACL'sini ayarlamak için komutu **kullanın.** 

Bu örnek, ACL'yi sahibi kullanıcı, sahip grubu veya diğer kullanıcılar için bir dizinde ayarlar ve ardından ACL'yi konsola yazdırır.

```azurecli
az storage blob directory access set -a "user::rw-,group::rw-,other::-wx" -d my-directory -c my-file-system --account-name mystorageaccount
```

Bu *örnekte,* sahip olan kullanıcı, sahip grubu veya diğer kullanıcılar için varsayılan ACL dizini ayarlar ve ardından ACL'yi konsola yazdırır.

```azurecli
az storage blob directory access set -a "default:user::rw-,group::rw-,other::-wx" -d my-directory -c my-file-system --account-name mystorageaccount
```

Bir `az storage blob access set` **dosyanın**acl ayarlamak için komutu kullanın. 

Bu örnek, sahip olan kullanıcı, grup sahibi veya diğer kullanıcılar için bir dosyaüzerinde ACL ayarlar ve sonra konsola ACL yazdırır.

```azurecli
az storage blob access set -a "user::rw-,group::rw-,other::-wx" -b my-directory/upload.txt -c my-file-system --account-name mystorageaccount
```
Aşağıdaki resim, bir dosyanın ACL'sini ayarladıktan sonra çıktıyı gösterir.

![ACL çıktısı alın](./media/data-lake-storage-directory-file-acl-cli/set-acl-file.png)

Bu örnekte, sahibi kullanıcı ve sahip grubu yalnızca okuma ve yazma izinleri var. Diğer tüm kullanıcıların yazma ve yürütme izinleri vardır. Erişim denetimi listeleri hakkında daha fazla bilgi için [Azure Veri Gölü Depolama Gen2'deki Erişim denetimine](data-lake-storage-access-control.md)bakın.

### <a name="update-directory-and-file-permissions"></a>Dizin ve dosya izinlerini güncelleştirme

Bu izni ayarlamak için başka `az storage blob directory access update` bir `az storage blob access update` yolu veya komutu kullanmaktır. 

Parametreyi bir ACL'nin kısa `-permissions` formuna ayarlayarak dizinin veya dosyanın ACL'sini güncelleştirin.

Bu örnek, bir **dizinin**ACL'sini güncelleştirir.

```azurecli
az storage blob directory access update --permissions "rwxrwxrwx" -d my-directory -c my-file-system --account-name mystorageaccount
```

Bu örnek, bir **dosyanın**ACL'sini güncelleştirir.

```azurecli
az storage blob access update --permissions "rwxrwxrwx" -b my-directory/upload.txt -c my-file-system --account-name mystorageaccount
```

Ayrıca, bir kullanıcının varlık `--owner` kimliğine veya Kullanıcı Ana Adı'na (UPN) veya parametrelerini ayarlayarak bir dizin veya `group` dosyanın sahibi kullanıcıve grubunu da güncelleştirebilirsiniz. 

Bu örnek, bir dizinin sahibini değiştirir. 

```azurecli
az storage blob directory access update --owner xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -d my-directory -c my-file-system --account-name mystorageaccount
```

Bu örnek, bir dosyanın sahibini değiştirir. 

```azurecli
az storage blob access update --owner xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -b my-directory/upload.txt -c my-file-system --account-name mystorageaccount
```
## <a name="manage-user-defined-metadata"></a>Kullanıcı tanımlı meta verileri yönetme

`az storage blob directory metadata update` Komutu bir veya daha fazla ad değeri çiftiyle kullanarak bir dosyaya veya dizine kullanıcı tanımlı meta veri ekleyebilirsiniz.

Bu örnek, dizin adlı `my-directory` bir dizin için kullanıcı tanımlı meta verileri ekler.

```azurecli
az storage blob directory metadata update --metadata tag1=value1 tag2=value2 -c my-file-system -d my-directory --account-name mystorageaccount
```

Bu örnek, adlı `my-directory`dizini için kullanıcı tanımlı tüm meta verileri gösterir.

```azurecli
az storage blob directory metadata show -c my-file-system -d my-directory --account-name mystorageaccount
```

## <a name="see-also"></a>Ayrıca bkz.

* [Örnek](https://github.com/Azure/azure-cli-extensions/tree/master/src/storage-preview)
* [Gen1 - Gen2 haritalama](https://github.com/Azure/azure-cli-extensions/tree/master/src/storage-preview#mapping-from-adls-gen1-to-adls-gen2)
* [Geri bildirim gönderme](https://github.com/Azure/azure-cli-extensions/issues)
* [Bilinen sorunlar](data-lake-storage-known-issues.md#api-scope-data-lake-client-library)
* [Kaynak kodu](https://github.com/Azure/azure-cli-extensions/tree/master/src)

