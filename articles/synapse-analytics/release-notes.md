---
title: 'Sürüm notları: Azure SYNAPSE Analytics (çalışma alanları Önizleme)'
description: Azure SYNAPSE Analytics için sürüm notları (çalışma alanları Önizleme)
services: synapse-analytics
author: julieMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: jrasnick
ms.reviewer: jrasnick
ms.openlocfilehash: c8212af727a47ab05936c8b59c781cd49c5b0ba6
ms.sourcegitcommit: 5d7f8c57eaae91f7d9cf1f4da059006521ed4f9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89669698"
---
# <a name="azure-synapse-analytics-workspaces-preview-release-notes"></a>Azure SYNAPSE Analytics (çalışma alanları Önizleme) sürüm notları

Bu makalede, Azure SYNAPSE Analytics (çalışma alanları) ile ilgili sınırlamalar ve sorunlar açıklanmaktadır. İlgili bilgiler için bkz. [Azure SYNAPSE Analytics nedir (çalışma alanları)](overview-what-is.md)

[!INCLUDE [preview](includes/note-preview.md)]

## <a name="azure-cli"></a>Azure CLI

- Sorun ve müşteri etkisi: SDK tarafından oluşturulan çalışma alanları SYNAPSE Studio 'Yu başlatamıyor

- Geçici çözüm: aşağıdaki adımları uygulayın: 
  1.    Çalıştırarak çalışma alanı oluşturun `az synapse workspace create` .
  2.    Çalıştırarak yönetilen kimlik KIMLIĞINI ayıklayın `$identity=$(az synapse workspace show --name {workspace name}  --resource-group {resource group name} --query "identity.principalId")` .
  3.    Çalışma alanını, çalıştırarak depolama hesabına rol olarak ekleyin ` az role assignment create --role "Storage Blob Data Contributor" --assignee-object-id {identity } --scope {storage account resource id}` .
  4.    Çalıştırarak güvenlik duvarı kuralı ekleyin ` az synapse firewall-rule create --name allowAll --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255 ` .

## <a name="next-steps"></a>Sonraki adımlar

* [Azure SYNAPSE nedir?](overview-what-is.md)
* [Başlarken](get-started.md)
* [SSS](overview-faq.md)
