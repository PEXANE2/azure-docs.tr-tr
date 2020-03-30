---
title: 'Azure Cloud Shell’den Databricks CLI kullanma '
description: Azure Veri Tuğlaları'nda işlemleri gerçekleştirmek için Azure Cloud Shell'in Databricks CLI'sini nasıl kullanacağınızı öğrenin.
services: azure-databricks
author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.workload: big-data
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: mamccrea
ms.openlocfilehash: efb0d3222bfd98b15502163979425d47fa459e07
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73605715"
---
# <a name="use-databricks-cli-from-azure-cloud-shell"></a>Azure Cloud Shell’den Databricks CLI kullanma

Databricks'teki işlemleri gerçekleştirmek için Azure Cloud Shell'in Databricks CLI'sini nasıl kullanacağınızı öğrenin.

## <a name="prerequisites"></a>Ön koşullar

* Azure Databricks çalışma alanı ve küme. Talimatlar için [bkz.](quickstart-create-databricks-workspace-portal.md) 

* Databricks'te kişisel erişim jetonu ayarlayın. Talimatlar [için, Bkz. Token yönetimi.](/azure/databricks/dev-tools/api/latest/authentication)

## <a name="use-the-azure-cloud-shell"></a>Azure Bulut Kabuğunu Kullanma

1. [Azure portalına](https://portal.azure.com)giriş yapın.
 
2. Sağ üst köşeden Bulut **Kabuğu** simgesini tıklatın.

   ![Bulut Kabuğunu Başlat](./media/databricks-cli-from-azure-cloud-shell/launch-azure-cloud-shell.png "Azure Cloud Shell'i başlatma")

3. Bulut Kabuğu ortamı için **Bash'i** seçtiğinizden emin olun. Aşağıdaki ekran görüntüsünde gösterildiği gibi açılır bırakma seçeneğinden seçim yapabilirsiniz.

   ![Cloud Shell ortamı için Bash'i seçin](./media/databricks-cli-from-azure-cloud-shell/select-bash-for-shell.png "Bash'i seçin") 

4. Databricks CLI'yi yükleyebileceğiniz sanal bir ortam oluşturun. Aşağıdaki snippet'te `databrickscli`, .

       virtualenv -p /usr/bin/python2.7 databrickscli

5. Oluşturduğunuz sanal ortama geçin.

       source databrickscli/bin/activate

6. Databricks CLI'yi yükleyin.

       pip install databricks-cli

7. Ön koşulların bir parçası olarak listelenen, oluşturduğunuz erişim belirtecisini kullanarak Databricks ile kimlik doğrulaması ayarlayın. Aşağıdaki komutu kullanın:

       databricks configure --token

    Aşağıdaki istemleri alacaksınız:

    * İlk olarak, Databricks ana bilgisayara girmeniz istenir. Değeri biçime `https://eastus2.azuredatabricks.net`girin. Burada, **Doğu ABD 2,** Azure Databricks çalışma alanınızı oluşturduğunuz Azure bölgesidir.

    * Ardından, bir belirteç girmeniz istenir. Daha önce oluşturduğunuz belirteci girin.

Bu adımları tamamladıktan sonra Azure Bulut BulutU'ndan Databricks CLI'yi kullanmaya başlayabilirsiniz.

## <a name="use-databricks-cli"></a>Databricks CLI'yi kullanma

Artık Databricks CLI'yi kullanmaya başlayabilirsiniz. Örneğin, çalışma alanınızda bulunan tüm Databricks kümelerini listelemek için aşağıdaki komutu çalıştırın.

    databricks clusters list

Databricks dosya sistemine (DBFS) erişmek için aşağıdaki komutu da kullanabilirsiniz.

    databricks fs ls


Komutlar hakkında tam bir başvuru için [Databricks CLI'ye](/azure/databricks/dev-tools/databricks-cli)bakın.


## <a name="next-steps"></a>Sonraki adımlar

* Azure CLI hakkında daha fazla bilgi edinmek için [Azure CLI'ye genel bakış](../cloud-shell/overview.md)
* Azure CLI komutlarının listesini görmek için Bkz. [Azure CLI başvurusu](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest)
* Databricks CLI komutlarının listesini görmek için [bkz.](/azure/databricks/dev-tools/databricks-cli)


