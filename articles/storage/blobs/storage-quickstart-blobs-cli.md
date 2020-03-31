---
title: Quickstart - Azure CLI ile bir leke oluşturma
titleSuffix: Azure Storage
description: Bu hızlı başlangıçta, Azure CLI'yi kullanmayı Azure Depolama'ya blob yüklemeyi, bir blob indirmeyi ve bir kaptaki lekeleri nasıl listeleyebilirsiniz.
services: storage
author: tamram
ms.service: storage
ms.subservice: blobs
ms.topic: quickstart
ms.date: 02/26/2020
ms.author: tamram
ms.openlocfilehash: 2e1b1ac2ea315759b18dc882b98837bca0a84d46
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80061436"
---
# <a name="quickstart-create-download-and-list-blobs-with-azure-cli"></a>Quickstart: Azure CLI ile blob oluşturma, indirme ve listele

Azure CLI, Azure kaynaklarını yönetmek için Azure tarafından sunulan komut satırı deneyimidir. Azure Cloud Shell ile kendi tarayıcınızda da kullanabilirsiniz. Dilerseniz macOS, Linux veya Windows’a yükleyebilir ve komut satırından çalıştırabilirsiniz. Bu hızlı başlangıçta Azure Blob depolamaya veri yüklemek ve verileri indirmek için Azure CLI kullanma hakkında ayrıntılı bilgiler öğrenirsiniz.

[!INCLUDE [storage-multi-protocol-access-preview](../../../includes/storage-multi-protocol-access-preview.md)]

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="install-the-azure-cli-locally"></a>Azure CLI'yi yerel olarak yükleme

Azure CLI'yi yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu hızlı başlatma, Azure CLI sürümünü 2.0.46 veya daha yeni bir şekilde çalıştırmanızı gerektirir. Sürümünüzü belirlemek için `az --version` çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

Azure CLI'yi yerel olarak çalıştırıyorsanız, oturum açmanız ve kimlik doğrulaması yapmanız gerekir. Azure Bulut Su Şur'u kullanıyorsanız bu adım gerekli değildir. Azure CLI'de oturum `az login` açmak için tarayıcı penceresinde çalıştırın ve kimlik doğrulaması yapın:

```azurecli
az login
```

Azure CLI ile kimlik doğrulama hakkında daha fazla bilgi için [Azure CLI ile Oturum Aç'a](/cli/azure/authenticate-azure-cli)bakın.

## <a name="authorize-access-to-blob-storage"></a>Blob depolama alanına erişim yetkisi verme

Azure AD kimlik bilgileriyle veya depolama hesabı erişim anahtarını kullanarak Azure CLI'den Blob depolama alanına erişimi yetkilendirmek tesbit edebilirsiniz. Azure AD kimlik bilgilerini kullanman önerilir. Bu makalede, Azure AD kullanarak Blob depolama işlemleri nasıl yetkiverilmektedir.

Blob depolamasına karşı veri işlemleri için `--auth-mode` Azure CLI komutları, belirli bir işlemi nasıl yetkilendirmeyileceğini belirtmenize olanak tanıyan parametreyi destekler. Parametreyi `--auth-mode` Azure `login` AD kimlik bilgileriyle yetkilendirmek için ayarlayın. Daha fazla bilgi için bkz: [Azure CLI ile blob veya sıra verilerine erişimi Yetkilendirme.](../common/authorize-data-operations-cli.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

Yalnızca Blob depolama veri `--auth-mode` işlemleri parametreyi destekler. Kaynak grubu veya depolama hesabı oluşturma gibi yönetim işlemleri, yetkilendirme için Azure AD kimlik bilgilerini otomatik olarak kullanır.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group) komutuyla bir Azure kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Açı parantezindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli
az group create \
    --name <resource-group> \
    --location <location>
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

[az storage account create](/cli/azure/storage/account) komutuyla bir genel amaçlı depolama hesabı oluşturun. Genel amaçlı depolama hesabı; bloblar, dosyalar, tablolar ve kuyruklar olmak üzere dört hizmet için de kullanılabilir.

Açı parantezindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli
az storage account create \
    --name <storage-account> \
    --resource-group <resource-group> \
    --location <location> \
    --sku Standard_ZRS \
    --encryption blob
```

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Bloblar her zaman bir kapsayıcıya yüklenir. Dosyalarınızı klasörlerde bilgisayarınızda düzenleme şeklinize benzer kapsayıcılarda blob grupları düzenleyebilirsiniz.

[az storage container create](/cli/azure/storage/container) komutunu kullanarak blobları depolamak için bir kapsayıcı oluşturun. Açı parantezindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli
az storage container create \
    --account-name <storage-account> \
    --name <container> \
    --auth-mode login
```

## <a name="upload-a-blob"></a>Blobu karşıya yükleme

Blob depolama blok blobları, ekleme bloblarını ve sayfa bloblarını destekler. Bu hızlı başlatmadaki örnekler, blok lekeleri ile nasıl çalışılabildiğini gösterir.

İlk olarak, bir blok blob yüklemek için bir dosya oluşturun. Azure Bulut Su Şurunu kullanıyorsanız, bir dosya oluşturmak için aşağıdaki komutu kullanın:

```bash
vi helloworld
```

Dosya **açıldığında, ekle tuşuna**basın. *Type Hello dünya*, sonra **Esc tuşuna**basın . Ardından *:x*yazın, sonra **Enter**tuşuna basın.

Bu örnekte, son adımda [az storage blob upload](/cli/azure/storage/blob) komutuyla oluşturduğunuz kapsayıcıya bir blob yükleyeceksiniz. Dosya kök dizinde oluşturulduğundan, bir dosya yolu belirtmeniz gerekmez. Açı parantezindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

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

[az storage blob list](/cli/azure/storage/blob) komutuyla kapsayıcıdaki blobları listeleyin. Açı parantezindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli
az storage blob list \
    --account-name <storage-account> \
    --container-name <container> \
    --output table \
    --auth-mode login
```

## <a name="download-a-blob"></a>Blob indirme

Önceden karşıya yüklediğiniz bir blobu indirmek için [az storage blob download](/cli/azure/storage/blob) komutunu kullanın. Açı parantezindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli
az storage blob download \
    --account-name <storage-account> \
    --container-name <container> \
    --name helloworld \
    --file ~/destination/path/for/file \
    --auth-mode login
```

## <a name="data-transfer-with-azcopy"></a>AzCopy ile veri aktarımı

AzCopy komut satırı yardımcı programı, Azure Depolama için yüksek performanslı, komut dosyası veri aktarımı sunar. Blob depolama ve Azure Dosyalarına veri aktarmak için AzCopy'yi kullanabilirsiniz. AzCopy v10 hakkında daha fazla bilgi için, AzCopy en son sürümü, [AzCopy ile başlayın](../common/storage-use-azcopy-v10.md)bakın. Blob depolama ile AzCopy v10 kullanma hakkında bilgi edinmek için, [AzCopy ve Blob depolama ile veri aktarımı](../common/storage-use-azcopy-blobs.md)bakın.

Aşağıdaki örnekte, yerel bir dosyayı blob'a yüklemek için AzCopy kullanır. Örnek değerleri kendi değerlerinizle değiştirmeyi unutmayın:

```bash
azcopy login
azcopy copy 'C:\myDirectory\myTextFile.txt' 'https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt'
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Depolama hesabı da dahil olmak üzere bu hızlı başlatmanın bir parçası olarak oluşturduğunuz kaynakları silmek istiyorsanız, [az grubu silme](/cli/azure/group) komutunu kullanarak kaynak grubunu silin. Açı parantezindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli
az group delete \
    --name <resource-group> \
    --no-wait
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, dosyaları azure blob depolama alanında yerel bir dosya sistemi yle bir kapsayıcı arasında nasıl aktarabileceğinizi öğrendiniz. Azure Depolama’da bloblarla çalışma hakkında daha fazla bilgi edinmek için, Azure Blob depolamayla çalışma hakkındaki öğretici ile devam edin.

> [!div class="nextstepaction"]
> [Nasıl yapılır: Azure CLI ile blob depolama işlemleri](storage-how-to-use-blobs-cli.md)
