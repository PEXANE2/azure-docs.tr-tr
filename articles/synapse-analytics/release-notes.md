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
ms.openlocfilehash: 059e77c063d00ef850a171507ca2e06422ade426
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82191779"
---
# <a name="azure-synapse-analytics-preview-release-notes"></a>Azure SYNAPSE Analytics (Önizleme) sürüm notları

Bu makalede, Azure SYNAPSE Analytics (çalışma alanları) ile ilgili sınırlamalar ve sorunlar açıklanmaktadır. İlgili bilgiler için bkz. [Azure SYNAPSE Analytics nedir (çalışma alanları)](overview-what-is.md)

[!INCLUDE [preview](includes/note-preview.md)]

## <a name="azure-synapse-workspaces"></a>Azure Synapse (çalışma alanları) 

### <a name="azure-synapse-cli"></a>Azure SYNAPSE CLı

- Sorun ve müşteri etkisi: SDK tarafından oluşturulan çalışma alanları SYNAPSE Studio 'Yu başlatamıyor

- Geçici çözüm: aşağıdaki adımları uygulayın: 
  1.    Çalıştırarak `az synapse workspace create`çalışma alanı oluşturun.
  2.    Çalıştırarak `$identity=$(az synapse workspace show --name {workspace name}  --resource-group {resource group name} --query "identity.principalId")`yönetilen kimlik kimliğini ayıklayın.
  3.    Çalışma alanını, çalıştırarak ` az role assignment create --role "Storage Blob Data Contributor" --assignee-object-id {identity } --scope {storage account resource id}`depolama hesabına rol olarak ekleyin.
  4.    Çalıştırarak ` az synapse firewall-rule create --name allowAll --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255 `güvenlik duvarı kuralı ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Çalışma alanı oluşturma](quickstart-create-workspace.md)
* [SYNAPSE Studio 'Yu kullanma](quickstart-synapse-studio.md)
* [SQL havuzu oluşturma](quickstart-create-sql-pool.md)
* [İsteğe bağlı SQL kullanma](quickstart-sql-on-demand.md)
* [Apache Spark havuzu oluşturma](quickstart-create-apache-spark-pool.md)