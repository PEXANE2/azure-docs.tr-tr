---
title: Hızlı başlangıç-Azure CLı ile blob oluşturma
titleSuffix: Azure Storage
description: Bu hızlı başlangıçta, Azure CLı 'yı kullanarak bir blobu Azure depolama 'ya yükleme, blob indirme ve bir kapsayıcıdaki Blobları listeleme hakkında bilgi edineceksiniz.
services: storage
author: tamram
ms.service: storage
ms.subservice: blobs
ms.topic: quickstart
ms.date: 04/23/2020
ms.author: tamram
ms.openlocfilehash: 237982c2135430891022ab7a823f8374844e8d90
ms.sourcegitcommit: f7d057377d2b1b8ee698579af151bcc0884b32b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82117582"
---
# <a name="quickstart-create-download-and-list-blobs-with-azure-cli"></a>Hızlı başlangıç: Azure CLı ile Bloblar oluşturma, indirme ve listeleme

Azure CLI, Azure kaynaklarını yönetmek için Azure tarafından sunulan komut satırı deneyimidir. Azure Cloud Shell ile kendi tarayıcınızda da kullanabilirsiniz. Dilerseniz macOS, Linux veya Windows’a yükleyebilir ve komut satırından çalıştırabilirsiniz. Bu hızlı başlangıçta Azure Blob depolamaya veri yüklemek ve verileri indirmek için Azure CLI kullanma hakkında ayrıntılı bilgiler öğrenirsiniz.

[!INCLUDE [storage-multi-protocol-access-preview](../../../includes/storage-multi-protocol-access-preview.md)]

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="install-the-azure-cli-locally"></a>Azure CLı 'yı yerel olarak yükler

Azure CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu hızlı başlangıç, Azure CLı sürüm 2.0.46 veya üstünü çalıştırıyor olmanızı gerektirir. Sürümünüzü belirlemek için `az --version` çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

Azure CLı 'yi yerel olarak çalıştırıyorsanız, oturum açıp kimlik doğrulaması yapmanız gerekir. Azure Cloud Shell kullanıyorsanız bu adım gerekli değildir. Azure CLı 'da oturum açmak için tarayıcı penceresinde `az login` çalıştırın ve kimlik doğrulaması yapın:

```azurecli
az login
```

Azure CLı ile kimlik doğrulama hakkında daha fazla bilgi için bkz. [Azure CLI Ile oturum açma](/cli/azure/authenticate-azure-cli).

## <a name="authorize-access-to-blob-storage"></a>Blob depolamaya erişim yetkisi verme

Azure CLı 'dan Azure AD kimlik bilgileriyle veya depolama hesabı erişim anahtarını kullanarak blob depolamaya erişim yetkisi verebilirsiniz. Azure AD kimlik bilgilerinin kullanılması önerilir. Bu makalede, Azure AD kullanarak BLOB depolama işlemlerini yetkilendirme işlemi gösterilmektedir.

Blob depolamaya yönelik veri işlemlerine yönelik Azure CLı komutları, belirli `--auth-mode` bir işlemin yetkilendirmesini belirtmenize olanak sağlayan parametresini destekler. `--auth-mode` PARAMETRESINI Azure AD kimlik `login` bilgileriyle yetkilendirmek için olarak ayarlayın. Daha fazla bilgi için bkz. [Azure CLI ile blob veya kuyruk verilerine erişim yetkisi verme](../common/authorize-data-operations-cli.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

Yalnızca BLOB depolama veri işlemleri `--auth-mode` parametreyi destekler. Kaynak grubu veya depolama hesabı oluşturma gibi yönetim işlemleri, yetkilendirme için otomatik olarak Azure AD kimlik bilgilerini kullanır.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group) komutuyla bir Azure kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Açılı ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli
az group create \
    --name <resource-group> \
    --location <location>
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

[az storage account create](/cli/azure/storage/account) komutuyla bir genel amaçlı depolama hesabı oluşturun. Genel amaçlı depolama hesabı; bloblar, dosyalar, tablolar ve kuyruklar olmak üzere dört hizmet için de kullanılabilir.

Açılı ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli
az storage account create \
    --name <storage-account> \
    --resource-group <resource-group> \
    --location <location> \
    --sku Standard_ZRS \
    --encryption-services blob
```

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Bloblar her zaman bir kapsayıcıya yüklenir. Kapsayıcılardaki blob gruplarını, dosyalarınızı bilgisayarınızdaki dosyalarınıza düzenlemenize benzer şekilde düzenleyebilirsiniz.

[az storage container create](/cli/azure/storage/container) komutunu kullanarak blobları depolamak için bir kapsayıcı oluşturun. Açılı ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli
az storage container create \
    --account-name <storage-account> \
    --name <container> \
    --auth-mode login
```

## <a name="upload-a-blob"></a>Blobu karşıya yükleme

Blob depolama blok blobları, ekleme bloblarını ve sayfa bloblarını destekler. Bu hızlı başlangıçtaki örneklerde, blok bloblarla nasıl çalışılacağı gösterilmektedir.

İlk olarak, bir blok blobuna yüklemek için bir dosya oluşturun. Azure Cloud Shell kullanıyorsanız, bir dosya oluşturmak için aşağıdaki komutu kullanın:

```bash
vi helloworld
```

Dosya açıldığında, **Ekle**' ye basın. *Merhaba Dünya*yazın ve ardından **ESC**tuşuna basın. Sonra, şunu yazın *: x*yazın ve **ENTER**tuşuna basın.

Bu örnekte, son adımda [az storage blob upload](/cli/azure/storage/blob) komutuyla oluşturduğunuz kapsayıcıya bir blob yükleyeceksiniz. Dosya kök dizininde oluşturulduğundan bu yana bir dosya yolu belirtmeniz gerekmez. Açılı ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli
az storage blob upload \
    --account-name <storage-account> \
    --container-name <container> \
    --name helloworld \
    --file helloworld \
    --auth-mode login
```

Bu işlemle, daha önce oluşturulmadıysa bir blob oluşturulur, aksi takdirde üzerine yazılacaktır. Devam etmeden önce istediğiniz sayıda dosyayı karşıya yükleyin.

Aynı anda birden fazla dosya yüklemek için [az storage blob upload-batch](/cli/azure/storage/blob) komutunu kullanabilirsiniz.

## <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

[az storage blob list](/cli/azure/storage/blob) komutuyla kapsayıcıdaki blobları listeleyin. Açılı ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli
az storage blob list \
    --account-name <storage-account> \
    --container-name <container> \
    --output table \
    --auth-mode login
```

## <a name="download-a-blob"></a>Blob indirme

Önceden karşıya yüklediğiniz bir blobu indirmek için [az storage blob download](/cli/azure/storage/blob) komutunu kullanın. Açılı ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli
az storage blob download \
    --account-name <storage-account> \
    --container-name <container> \
    --name helloworld \
    --file ~/destination/path/for/file \
    --auth-mode login
```

## <a name="data-transfer-with-azcopy"></a>AzCopy ile veri aktarımı

AzCopy komut satırı yardımcı programı, Azure depolama için yüksek performanslı ve komut dosyalı veri aktarımı sağlar. BLOB depolama ve Azure dosyalarına veri aktarmak için AzCopy kullanabilirsiniz. AzCopy 'in en son sürümü olan AzCopy ile v10 arasındaki hakkında daha fazla bilgi için bkz. [AzCopy ile çalışmaya başlama](../common/storage-use-azcopy-v10.md). AzCopy ile v10 arasındaki 'i blob Storage ile kullanma hakkında bilgi edinmek için bkz. [AzCopy ve BLOB Storage ile veri aktarma](../common/storage-use-azcopy-blobs.md).

Aşağıdaki örnek, bir Blobun yerel bir dosyayı karşıya yüklemek için AzCopy kullanır. Örnek değerleri kendi değerlerinizle değiştirmeyi unutmayın:

```bash
azcopy login
azcopy copy 'C:\myDirectory\myTextFile.txt' 'https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt'
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Depolama hesabı da dahil olmak üzere bu hızlı başlangıç kapsamında oluşturduğunuz kaynakları silmek istiyorsanız, [az Group Delete](/cli/azure/group) komutunu kullanarak kaynak grubunu silin. Açılı ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli
az group delete \
    --name <resource-group> \
    --no-wait
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, dosyaları yerel bir dosya sistemi ile Azure Blob depolama alanındaki bir kapsayıcı arasında aktarmayı öğrendiniz. Azure Depolama’da bloblarla çalışma hakkında daha fazla bilgi edinmek için, Azure Blob depolamayla çalışma hakkındaki öğretici ile devam edin.

> [!div class="nextstepaction"]
> [Nasıl yapılır: Azure CLI ile blob depolama işlemleri](storage-how-to-use-blobs-cli.md)
