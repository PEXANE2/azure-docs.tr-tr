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
ms.openlocfilehash: 3eb41b868aba7e7350f54594fa43f01eeeabebc4
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90942107"
---
# <a name="install-client-tools-for-deploying-and-managing-azure-arc-enabled-data-services"></a>Azure Arc etkin veri hizmetlerini dağıtmak ve yönetmek için istemci araçları 'nı yükler

> [!IMPORTANT]
> Yeni bir aylık sürüme güncelleştiriyorsanız, lütfen Azure Data Studio, Azure Data CLı (azdata) aracının en son sürümüne ve Azure Data CLı ve Azure Arc uzantıları ' nı Azure Data Studio için de güncelleştirdiğinizden emin olun.

Bu belge, istemci makinenize Azure Data CLı (azdata), Azure Data Studio, Azure CLı (az) ve Kubernetes CLı Aracı (kubectl) yükleme adımlarında size kılavuzluk eder.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="tools-for-creating-and-managing-azure-arc-enabled-data-services"></a>Azure Arc etkin veri Hizmetleri oluşturma ve yönetmeye yönelik araçlar 

Aşağıdaki tabloda, Azure Arc etkin veri hizmetlerinin oluşturulması ve yönetilmesi için gerekli olan genel araçlar ve bu araçların nasıl yükleneceği listelenmiştir:

| Araç | Gerekli | Açıklama | Yükleme |
|---|---|---|---|
| Azure Data CLı (azdata) | Yes | Büyük veri kümesini yüklemek ve yönetmek için komut satırı aracı. Azure Data CLı Ayrıca, komutları kullanarak Azure SQL ve SQL Server örneklerine ve Postgres sunucularına bağlanmak ve bunları sorgulamak için bir komut satırı yardımcı programı içerir `azdata sql query` (komut satırından tek bir sorgu çalıştırma), `azdata sql shell` (etkileşimli kabuk) `azdata postgres query` ve `azdata postgres shell` . | [Yükleme](/sql/azdata/install/deploy-install-azdata?toc=/azure/azure-arc/data/toc.json&bc=/azure/azure-arc/data/breadcrumb/toc.json) |
| Azure Data Studio | Yes | Azure SQL, SQL Server, PostrgreSQL ve MySQL gibi çeşitli veritabanlarına bağlanmak ve sorgu sorgulamak için zengin deneyim aracı. Azure Data Studio uzantılar, Azure Arc etkin veri Hizmetleri için bir yönetim deneyimi sağlar. | [Yükleme](https://aka.ms/getazuredatastudio) |
| Azure Data Studio için Azure Data CLı uzantısı | Yes | Henüz yoksa Azure Data CLı yükleyecek Azure Data Studio uzantısı.| Azure Data Studio 'de uzantılar galerisinden yüklemesi.|
| Azure Data Studio için Azure Arc uzantısı | Yes | Azure Arc etkin veri Hizmetleri için bir yönetim deneyimi sağlayan Azure Data Studio uzantısı. Azure Data Studio için Azure Data CLı uzantısında bir bağımlılık vardır. | Azure Data Studio 'de uzantılar galerisinden yüklemesi.|
| Azure Data Studio PostgreSQL uzantısı | Hayır | PostgreSQL için yönetim özellikleri sağlayan Azure Data Studio için PostgreSQL uzantısı. | <!--{need link} [Install](../azure-data-studio/data-virtualization-extension.md) --> Azure Data Studio 'de uzantılar galerisinden yüklemesi.|
| Azure CLı (az)<sup>1</sup> | Yes | Azure hizmetlerini yönetmeye yönelik modern komut satırı arabirimi. AKS dağıtımları ile birlikte kullanılır ve Azure Arc etkin veri Hizmetleri envanterini ve faturalandırma verilerini Azure 'a yükler. ([Daha fazla bilgi](/cli/azure/?view=azure-cli-latest&preserve-view=true)). | [Yükleme](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true) |
| Kubernetes CLı (kubectl)<sup>2</sup> | Yes | Kubernetes kümesini yönetmek için komut satırı aracı ([daha fazla bilgi](https://kubernetes.io/docs/tasks/tools/install-kubectl/)). | [Windows](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-with-powershell-from-psgallery) \| [Linux](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-using-native-package-management) |
| <sup>kıv3</sup> | Bazı örnek betikler için gereklidir. | URL 'Ler ile veri aktarmaya yönelik komut satırı aracı. | [Windows](https://curl.haxx.se/windows/) \| Linux: kıvrımlı paketi yükler |
| OC | Red Hat OpenShift ve Azure RedHat OpenShift dağıtımları için gereklidir. |`oc` , açık kaydırma komut satırı arabirimidir (CLı). | [CLı 'yı yükleme](https://docs.openshift.com/container-platform/4.4/cli_reference/openshift_cli/getting-started-cli.html#installing-the-cli)



<sup>1</sup> Azure CLI sürüm 2.0.4 veya üstünü kullanıyor olmanız gerekir. `az --version`Gerekirse sürümü bulmak için ' i çalıştırın.

<sup>2</sup> `kubectl` 1,13 veya sonraki bir sürümü kullanmanız gerekir. Ayrıca sürümünün sürümü, `kubectl` Kubernetes kümenizin bir alt veya daha düşük bir sürümünü içermelidir. İstemciye belirli bir sürüm yüklemek isterseniz `kubectl` , bkz. [ `kubectl` kıvrımlı aracılığıyla Ikili yüklemeyi](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-using-curl) (Windows 10 ' da, kıvrımlı çalıştırmak için windows PowerShell 'e değil cmd.exe kullanın).

<sup>3</sup> PowerShell kullanıyorsanız, bu Invoke-WebRequest cmdlet 'inin diğer adıdır.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Arc veri denetleyicisi oluşturma](create-data-controller.md)
