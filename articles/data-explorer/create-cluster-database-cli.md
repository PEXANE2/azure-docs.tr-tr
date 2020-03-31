---
title: Azure CLI ile DB & bir Azure Veri Gezgini kümesi oluşturma
description: Azure CLI'yi kullanarak Azure Veri Gezgini kümesini ve veritabanını nasıl oluşturabilirsiniz öğrenin
author: radennis
ms.author: radennis
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 6b8c2924e50da095c3bc5c7db2d2bf48ef5a27c2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77561961"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-azure-cli"></a>Azure CLI'yi kullanarak bir Azure Veri Gezgini kümesi ve veritabanı oluşturun

> [!div class="op_single_selector"]
> * [Portal](create-cluster-database-portal.md)
> * [CLI](create-cluster-database-cli.md)
> * [Powershell](create-cluster-database-powershell.md)
> * [C #](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)
> * [ARM şablonu](create-cluster-database-resource-manager.md)

Azure Veri Gezgini uygulamalar, web siteleri, IoT cihazları ve daha fazlasından akışı yapılan büyük miktarda veri üzerinde gerçek zamanlı analiz yapmaya yönelik hızlı ve tam olarak yönetilen bir veri analizi hizmetidir. Azure Veri Gezgini'ni kullanmak için öncelikle bir küme ve bu kümenin içinde bir veya daha fazla veritabanı oluşturmanız gerekir. Ardından veritabanına veri alarak (yükleyerek) sorgu çalıştırabilirsiniz. Bu makalede, Azure CLI kullanarak bir küme ve veritabanı oluşturursunuz.

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için bir Azure aboneliğine ihtiyacınız var. Hesabınız yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure CLI'yi yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu makalede Azure CLI sürümü 2.0.4 veya sonrası gerekir. Sürümünüzü kontrol etmek için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="configure-the-cli-parameters"></a>CLI parametrelerini yapılandırma

Azure Bulut BulutU'nda komutlar çalıştırıyorsanız aşağıdaki adımlar gerekmez. CLI'yi yerel olarak çalıştırıyorsanız, Azure'da oturum açmak ve geçerli aboneliğinizi ayarlamak için aşağıdaki adımları izleyin:

1. Azure'da oturum açmak için aşağıdaki komutu çalıştırın:

    ```azurecli-interactive
    az login
    ```

1. Aboneliği kümenizin oluşturulmasını istediğiniz yere ayarlayın. Kullanmak `MyAzureSub` istediğiniz Azure aboneliğinin adıyla değiştirin:

    ```azurecli-interactive
    az account set --subscription MyAzureSub
    ```

## <a name="create-the-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümesini oluşturma

1. Aşağıdaki komutu kullanarak kümeoluşturun:

    ```azurecli-interactive
    az kusto cluster create --name azureclitest --sku D11_v2 --resource-group testrg
    ```

   |**Ayar** | **Önerilen değer** | **Alan açıklaması**|
   |---|---|---|
   | ad | *azureclitest* | Kümenizin istenilen adı.|
   | Sku | *D13_v2* | Kümeniz için kullanılacak SKU. |
   | resource-group | *testrg* | Kümenin oluşturulacağı kaynak grubu adı. |

    Kümenin kapasitesi gibi kullanabileceğiniz ek isteğe bağlı parametreler vardır.

1. Kümenizin başarıyla oluşturulup oluşturulmadığını kontrol etmek için aşağıdaki komutu çalıştırın:

    ```azurecli-interactive
    az kusto cluster show --name azureclitest --resource-group testrg
    ```

Sonuç değeri `provisioningState` içeriyorsa, `Succeeded` küme başarıyla oluşturuldu.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümesinde veritabanıoluşturma

1. Aşağıdaki komutu kullanarak veritabanınızı oluşturun:

    ```azurecli-interactive
    az kusto database create --cluster-name azureclitest --name clidatabase --resource-group testrg --soft-delete-period P365D --hot-cache-period P31D
    ```

   |**Ayar** | **Önerilen değer** | **Alan açıklaması**|
   |---|---|---|
   | küme adı | *azureclitest* | Veritabanının oluşturulacağı kümenizin adı.|
   | ad | *clidatabase* | Veritabanınızın adı.|
   | resource-group | *testrg* | Kümenin oluşturulacağı kaynak grubu adı. |
   | yumuşak silme dönemi | *P365D* | Verilerin sorgu için kullanılabilir olarak tutulacağı süreyi belirtir. Daha fazla bilgi için [bekletme ilkesine](/azure/kusto/concepts/retentionpolicy) bakın. |
   | sıcak önbellek dönemi | *P31D* | Verilerin önbellekte tutulacağı süreyi belirtir. Daha fazla bilgi için [önbellek ilkesine](/azure/kusto/concepts/cachepolicy) bakın. |

1. Oluşturduğunuz veritabanını görmek için aşağıdaki komutu çalıştırın:

    ```azurecli-interactive
    az kusto database show --name clidatabase --resource-group testrg --cluster-name azureclitest
    ```

Artık bir kümeniz ve veritabanınız var.

## <a name="clean-up-resources"></a>Kaynakları temizleme

* Diğer makalelerimizi izlemeyi planlıyorsanız, oluşturduğunuz kaynakları koruyun.
* Kaynakları temizlemek için kümeyi silin. Bir kümeyi sildiğinizde, aynı zamanda tüm veritabanlarını da siler. Kümenizi silmek için aşağıdaki komutu kullanın:

    ```azurecli-interactive
    az kusto cluster delete --name azureclitest --resource-group testrg
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini Python kitaplığını kullanarak veri alma](python-ingest-data.md)
