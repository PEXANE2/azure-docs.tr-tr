---
title: Yönetilen kimlikleri VM'de yapılandırmak için SDK kullanma - Azure AD
description: Azure SDK kullanarak Azure kaynaklarının yönetilen kimliklerini bir Azure VM'de yapılandırmak ve kullanmak için adım adım yönergeler.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/28/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: d11cd51984f82bc20c02669e796d9ba21b9ed5d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74183472"
---
# <a name="configure-a-vm-with-managed-identities-for-azure-resources-using-an-azure-sdk"></a>Azure SDK kullanarak Azure kaynakları için yönetilen kimliklerle bir VM yapılandırma

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için yönetilen kimlikler, Azure Etkin Dizini'nde (AD) otomatik olarak yönetilen bir kimlik sağlar. Bu kimliği, kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmetin kimliğini doğrulamak için kullanabilirsiniz. 

Bu makalede, Azure SDK kullanarak bir Azure VM için Azure kaynakları için yönetilen kimlikleri etkinleştirmeyi ve kaldırabileceğinizi öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="azure-sdks-with-managed-identities-for-azure-resources-support"></a>Azure kaynakları desteği için yönetilen kimliklere sahip Azure SDK'ları 

Azure, bir dizi [Azure SDK'sı](https://azure.microsoft.com/downloads)aracılığıyla birden çok programlama platformlarını destekler. Bunlardan bazıları Azure kaynakları için yönetilen kimlikleri desteklemek ve kullanımı göstermek için karşılık gelen örnekler sağlamak üzere güncelleştirildi. Ek destek eklendikçe bu liste güncelleştirilir:

| SDK | Örnek |
| --- | ------ | 
| .NET   | [Azure kaynakları için yönetilen kimliklerle etkinleştirilen bir VM'den kaynak yönetme](https://azure.microsoft.com/resources/samples/aad-dotnet-manage-resources-from-vm-with-msi/) |
| Java   | [Azure kaynakları için yönetilen kimliklerle etkinleştirilen bir VM'den depolama yı yönetme](https://azure.microsoft.com/resources/samples/compute-java-manage-resources-from-vm-with-msi-in-aad-group/)|
| Node.js| [Sistem tarafından atanmış yönetilen kimlik etkinleştirilmiş bir VM oluşturma](https://azure.microsoft.com/resources/samples/compute-node-msi-vm/) |
| Python | [Sistem tarafından atanmış yönetilen kimlik etkinleştirilmiş bir VM oluşturma](https://azure.microsoft.com/resources/samples/compute-python-msi-vm/) |
| Ruby   | [Sistem tarafından atanmış bir kimlik etkinken Azure VM oluşturma](https://github.com/Azure-Samples/compute-ruby-msi-vm/) |

## <a name="next-steps"></a>Sonraki adımlar

- Azure portalı, PowerShell, CLI ve kaynak şablonlarını nasıl kullanabileceğinizi öğrenmek **için Azure VM için Kimliği Yapılandırma**altında ilgili makalelere bakın.
