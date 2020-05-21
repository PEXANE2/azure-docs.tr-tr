---
title: Sürüm notları
description: Azure SYNAPSE Analytics için sürüm notları (çalışma alanları)
services: synapse-analytics
author: julieMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: jrasnick
ms.reviewer: jrasnick
ms.openlocfilehash: 514694dc2e3f06db2fb80f6b3ba0106343be11d8
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83658510"
---
# <a name="azure-synapse-analytics-preview-release-notes"></a>Azure SYNAPSE Analytics (Önizleme) sürüm notları

Bu makalede, Azure SYNAPSE Analytics (çalışma alanları) ile ilgili sınırlamalar ve sorunlar açıklanmaktadır. İlgili bilgiler için bkz. [Azure SYNAPSE Analytics nedir (çalışma alanları)](overview-what-is.md)

[!INCLUDE [preview](includes/note-preview.md)]

## <a name="azure-synapse-workspaces"></a>Azure Synapse (çalışma alanları) 

### <a name="azure-synapse-cli"></a>Azure SYNAPSE CLı

- Sorun ve müşteri etkisi: SDK tarafından oluşturulan çalışma alanları SYNAPSE Studio 'Yu başlatamıyor

- Geçici çözüm: aşağıdaki adımları uygulayın: 
  1.    Çalıştırarak çalışma alanı oluşturun `az synapse workspace create` .
  2.    Çalıştırarak yönetilen kimlik kimliğini ayıklayın `$identity=$(az synapse workspace show --name {workspace name}  --resource-group {resource group name} --query "identity.principalId")` .
  3.    Çalışma alanını, çalıştırarak depolama hesabına rol olarak ekleyin ` az role assignment create --role "Storage Blob Data Contributor" --assignee-object-id {identity } --scope {storage account resource id}` .
  4.    Çalıştırarak güvenlik duvarı kuralı ekleyin ` az synapse firewall-rule create --name allowAll --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255 ` .

## <a name="next-steps"></a>Sonraki adımlar

* [Çalışma alanı oluşturma](quickstart-create-workspace.md)
* [SYNAPSE Studio 'Yu kullanma](quickstart-synapse-studio.md)
* [SQL havuzu oluşturma](quickstart-create-sql-pool-portal.md)
* [İsteğe bağlı SQL kullanma](quickstart-sql-on-demand.md)
* [Apache Spark havuzu oluşturma](quickstart-create-apache-spark-pool-portal.md)