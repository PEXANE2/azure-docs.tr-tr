---
title: Powershell kullanarak Bir Azure Veri Gezgini kümesi & DB oluşturma
description: PowerShell'i kullanarak Azure Veri Gezgini kümesi ni ve veritabanını nasıl oluşturabilirsiniz öğrenin
author: lucygoldbergmicrosoft
ms.author: lugoldbe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 690c3e281e65f54f240c70f7a6e5038f54102c99
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77560601"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-powershell"></a>PowerShell'i kullanarak Bir Azure Veri Gezgini kümesi ve veritabanı oluşturun

> [!div class="op_single_selector"]
> * [Portal](create-cluster-database-portal.md)
> * [CLI](create-cluster-database-cli.md)
> * [Powershell](create-cluster-database-powershell.md)
> * [C #](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)
> * [ARM şablonu](create-cluster-database-resource-manager.md)  

Azure Veri Gezgini uygulamalar, web siteleri, IoT cihazları ve daha fazlasından akışı yapılan büyük miktarda veri üzerinde gerçek zamanlı analiz yapmaya yönelik hızlı ve tam olarak yönetilen bir veri analizi hizmetidir. Azure Veri Gezgini'ni kullanmak için öncelikle bir küme ve bu kümenin içinde bir veya daha fazla veritabanı oluşturmanız gerekir. Ardından veritabanına veri alarak (yükleyerek) sorgu çalıştırabilirsiniz. Bu makalede, Powershell kullanarak bir küme ve veritabanı oluşturursunuz. Azure Veri Gezgini kümeleri ve veritabanları oluşturmak ve yapılandırmak için Windows, Linux veya [Azure Cloud Shell'de](../cloud-shell/overview.md) [Az.Kusto](/powershell/module/az.kusto/?view=azps-1.4.0#kusto) ile PowerShell cmdlets'leri ve komut dosyalarını çalıştırabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure CLI'yi yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu makalede Azure CLI sürümü 2.0.4 veya sonrası gerekir. Sürümünüzü kontrol etmek için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="configure-parameters"></a>Parametreleri yapılandırma

Azure Bulut BulutU'nda komutlar çalıştırıyorsanız aşağıdaki adımlar gerekmez. CLI'yi yerel olarak çalıştırıyorsanız, Azure'da oturum açmak ve geçerli aboneliğinizi ayarlamak için 1 & 2 adımlarını izleyin:

1. Azure'da oturum açmak için aşağıdaki komutu çalıştırın:

    ```azurepowershell-interactive
    Connect-AzAccount
    ```

1. Aboneliği kümenizin oluşturulmasını istediğiniz yerde ayarlayın:

    ```azurepowershell-interactive
     Set-AzContext -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ```
1. Azure CLI'yi yerel olarak veya Azure Bulut Kabuğu'nda çalıştırırken, Az.Kusto modüllerini cihazınıza yüklemeniz gerekir:

    ```azurepowershell-interactive
     Install-Module -Name Az.Kusto
    ```

## <a name="create-the-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümesini oluşturma

1. Aşağıdaki komutu kullanarak kümeoluşturun:

    ```azurepowershell-interactive
     New-AzKustoCluster -ResourceGroupName testrg -Name mykustocluster -Location 'Central US' -Sku D13_v2 -Capacity 10
    ```

   |**Ayar** | **Önerilen değer** | **Alan açıklaması**|
   |---|---|---|
   | Adı | *mykustocluster* | Kümenizin istenilen adı.|
   | Sku | *D13_v2* | Kümeniz için kullanılacak SKU. |
   | ResourceGroupName | *testrg* | Kümenin oluşturulacağı kaynak grubu adı. |

    Kümenin kapasitesi gibi kullanabileceğiniz ek isteğe bağlı parametreler vardır.

1. Kümenizin başarıyla oluşturulup oluşturulmadığını kontrol etmek için aşağıdaki komutu çalıştırın:

    ```azurepowershell-interactive
    Get-AzKustoCluster -Name mykustocluster -ResourceGroupName testrg
    ```

Sonuç değeri `provisioningState` içeriyorsa, `Succeeded` küme başarıyla oluşturuldu.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümesinde veritabanıoluşturma

1. Aşağıdaki komutu kullanarak veritabanınızı oluşturun:

    ```azurepowershell-interactive
    New-AzKustoDatabase -ResourceGroupName testrg -ClusterName mykustocluster -Name mykustodatabase -SoftDeletePeriod 3650:00:00:00 -HotCachePeriod 3650:00:00:00
    ```

   |**Ayar** | **Önerilen değer** | **Alan açıklaması**|
   |---|---|---|
   | ClusterName | *mykustocluster* | Veritabanının oluşturulacağı kümenizin adı.|
   | Adı | *mykustoveritabanı* | Veritabanınızın adı.|
   | ResourceGroupName | *testrg* | Kümenin oluşturulacağı kaynak grubu adı. |
   | SoftDeletePeriod | *3650:00:00:00* | Verilerin sorgu için kullanılabilir tutulacağı süre. |
   | HotÖnbellek Dönemi | *3650:00:00:00* | Verilerin önbellekte tutulacağı süre. |

1. Oluşturduğunuz veritabanını görmek için aşağıdaki komutu çalıştırın:

    ```azurepowershell-interactive
    Get-AzKustoDatabase -ClusterName mykustocluster -ResourceGroupName testrg -Name mykustodatabase
    ```

Artık bir kümeniz ve veritabanınız var.

## <a name="clean-up-resources"></a>Kaynakları temizleme

* Diğer makalelerimizi izlemeyi planlıyorsanız, oluşturduğunuz kaynakları koruyun.
* Kaynakları temizlemek için kümeyi silin. Bir kümeyi sildiğinizde, aynı zamanda tüm veritabanlarını da siler. Kümenizi silmek için aşağıdaki komutu kullanın:

    ```azurepowershell-interactive
    Remove-AzKustoCluster -ResourceGroupName testrg -Name mykustocluster
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Ek Az.Kusto komutları](/powershell/module/az.kusto/?view=azps-1.7.0#kusto)
* [Azure Veri Gezgini .NET Standart SDK'yı kullanarak veri alma (Önizleme)](net-standard-ingest-data.md)
