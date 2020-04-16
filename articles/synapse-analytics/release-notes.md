---
title: Sürüm notları
description: Azure Synapse Analytics (çalışma alanları) için sürüm notları
services: synapse-analytics
author: julieMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: jrasnick
ms.reviewer: jrasnick
ms.openlocfilehash: c1b5b9ac5d7c3f04dd3ae2e843425a5ead0d4c07
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81423862"
---
# <a name="azure-synapse-analytics-preview-release-notes"></a>Azure Synapse Analytics (önizleme) sürüm notları

Bu makalede, Azure Synapse Analytics (çalışma alanları) ile ilgili sınırlamalar ve sorunlar açıklanmaktadır. İlgili bilgiler için [Bkz. Azure Synapse Analytics (çalışma alanları) nedir](overview-what-is.md)

[!INCLUDE [preview](includes/note-preview.md)]

## <a name="azure-synapse-workspaces"></a>Azure Synapse (çalışma alanları) 

### <a name="azure-synapse-cli"></a>Azure Synapse CLI

- Sorun ve müşteri etkisi: SDK tarafından oluşturulan çalışma alanları Synapse Studio'u başlatamıyor

- Geçici çözüm: Aşağıdaki adımları tamamlayın: 
  1.    2 çalıştırarak `az synapse workspace create`çalışma alanı oluşturun.    Çalıştırarak yönetilen kimlik kimliğini ayıklama`$identity=$(az synapse workspace show --name {workspace name}  --resource-group {resource group name} --query "identity.principalId")`
  3.    Çalıştırarak depolama hesabına rol olarak çalışma alanı ekleme` az role assignment create --role "Storage Blob Data Contributor" --assignee-object-id {identity } --scope {storage account resource id}`
  4.    Çalıştırarak güvenlik duvarı kuralı ekleme` az synapse firewall-rule create --name allowAll --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255 `

## <a name="next-steps"></a>Sonraki adımlar

* [Çalışma alanı oluşturma](quickstart-create-workspace.md)
* [Synapse Studio'u kullanın](quickstart-synapse-studio.md)
* [SQL havuzu oluşturma](quickstart-create-sql-pool.md)
* [İsteğe bağlı SQL kullanma](quickstart-sql-on-demand.md)
* [Apache Spark havuzu oluşturma](quickstart-create-apache-spark-pool.md)