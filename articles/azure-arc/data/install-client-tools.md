---
title: İstemci araçlarını yükleme
description: Azdata, kubectl, Azure CLı, psql, Azure Data Studio (Insiders) ve Azure Data Studio için Arc uzantısını yükler
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 6f42f712ecca77c00020304b63f5a1b0dbd77ad0
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102172329"
---
# <a name="install-client-tools-for-deploying-and-managing-azure-arc-enabled-data-services"></a>Azure Arc özellikli veri hizmetlerini dağıtmak ve yönetmek için istemci araçlarını yükleme

> [!IMPORTANT]
> Yeni bir aylık sürüme güncelleştiriyorsanız, lütfen [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] Azure Data Studio için Azure Data Studio, araç ve [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] ve Azure Arc uzantılarının en son sürümüne de güncelleştirdiğinizden emin olun.

Bu belge, [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] istemci makinenize, Azure Data Studio, Azure CLI (az) ve Kubernetes CLI aracı 'nı (kubectl) yükleme adımlarında size kılavuzluk eder.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="tools-for-creating-and-managing-azure-arc-enabled-data-services"></a>Azure Arc etkin veri Hizmetleri oluşturma ve yönetmeye yönelik araçlar 

Aşağıdaki tabloda, Azure Arc etkin veri hizmetlerinin oluşturulması ve yönetilmesi için gerekli olan genel araçlar ve bu araçların nasıl yükleneceği listelenmiştir:

| Araç | Gerekli | Açıklama | Yükleme |
|---|---|---|---|
| [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] | Yes | Büyük veri kümesini yüklemek ve yönetmek için komut satırı aracı. [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] Ayrıca, komutları kullanarak Azure SQL ve SQL Server örneklerine ve Postgres sunucularına bağlanmak ve bunları sorgulamak için bir komut satırı yardımcı programı içerir `azdata sql query` (komut satırından tek bir sorgu çalıştırma), `azdata sql shell` (etkileşimli kabuk) `azdata postgres query` ve `azdata postgres shell` . | [Yükleme](/sql/azdata/install/deploy-install-azdata?toc=/azure/azure-arc/data/toc.json&bc=/azure/azure-arc/data/breadcrumb/toc.json) |
| Azure Data Studio | Yes | Azure SQL, SQL Server, PostrgreSQL ve MySQL gibi çeşitli veritabanlarına bağlanmak ve sorgu sorgulamak için zengin deneyim aracı. Azure Data Studio uzantılar, Azure Arc etkin veri Hizmetleri için bir yönetim deneyimi sağlar. | [Yükleme](/sql/azure-data-studio/download-azure-data-studio) |
| [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] Azure Data Studio uzantısı | Yes | Zaten yoksa yüklenecek Azure Data Studio uzantısı [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] .| Azure Data Studio 'de uzantılar galerisinden yüklemesi.|
| Azure Data Studio için Azure Arc uzantısı | Yes | Azure Arc etkin veri Hizmetleri için bir yönetim deneyimi sağlayan Azure Data Studio uzantısı. [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)]Azure Data Studio uzantısının bağımlılığı vardır. | Azure Data Studio 'de uzantılar galerisinden yüklemesi.|
| Azure Data Studio PostgreSQL uzantısı | No | PostgreSQL için yönetim özellikleri sağlayan Azure Data Studio için PostgreSQL uzantısı. | <!--{need link} [Install](../azure-data-studio/data-virtualization-extension.md) --> Azure Data Studio 'de uzantılar galerisinden yüklemesi.|
| Azure CLı (az)<sup>1</sup> | Yes | Azure hizmetlerini yönetmeye yönelik modern komut satırı arabirimi. AKS dağıtımları ile birlikte kullanılır ve Azure Arc etkin veri Hizmetleri envanterini ve faturalandırma verilerini Azure 'a yükler. ([Daha fazla bilgi](/cli/azure/)). | [Yükleme](/cli/azure/install-azure-cli) |
| Kubernetes CLı (kubectl)<sup>2</sup> | Yes | Kubernetes kümesini yönetmek için komut satırı aracı ([daha fazla bilgi](https://kubernetes.io/docs/tasks/tools/install-kubectl/)). | [Windows](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-with-powershell-from-psgallery) \| [Linux](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-using-native-package-management) |
| <sup>kıv3</sup> | Bazı örnek betikler için gereklidir. | URL 'Ler ile veri aktarmaya yönelik komut satırı aracı. | [Windows](https://curl.haxx.se/windows/) \| Linux: kıvrımlı paketi yükler |
| OC | Red Hat OpenShift ve Azure RedHat OpenShift dağıtımları için gereklidir. |`oc` , açık kaydırma komut satırı arabirimidir (CLı). | [CLı 'yı yükleme](https://docs.openshift.com/container-platform/4.4/cli_reference/openshift_cli/getting-started-cli.html#installing-the-cli)



<sup>1</sup> Azure CLI sürüm 2.0.4 veya üstünü kullanıyor olmanız gerekir. `az --version`Gerekirse sürümü bulmak için ' i çalıştırın.

<sup>2</sup> `kubectl` 1,13 veya sonraki bir sürümü kullanmanız gerekir. Ayrıca sürümünün sürümü, `kubectl` Kubernetes kümenizin bir alt veya daha düşük bir sürümünü içermelidir. İstemciye belirli bir sürüm yüklemek isterseniz `kubectl` , bkz. [ `kubectl` kıvrımlı aracılığıyla Ikili yüklemeyi](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-using-curl) (Windows 10 ' da, kıvrımlı çalıştırmak için windows PowerShell 'e değil cmd.exe kullanın).

<sup>3</sup> PowerShell kullanıyorsanız, kıvrımlı Invoke-WebRequest cmdlet 'inin diğer adıdır.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Arc veri denetleyicisi oluşturma](create-data-controller.md)