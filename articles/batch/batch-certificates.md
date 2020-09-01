---
title: Sertifikaları kullanma-Azure Batch
description: Uygulamaların kimlik doğrulamasını etkinleştirmek için sertifikaları kullanma
services: batch
documentationcenter: .net
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: 63d9d4f1-8521-4bbb-b95a-c4cad73692d3
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 02/17/2020
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: 5f7d547ad549b2747ae41182ee4058b001d9e78a
ms.sourcegitcommit: d7352c07708180a9293e8a0e7020b9dd3dd153ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2020
ms.locfileid: "89146394"
---
# <a name="using-certificates-with-batch"></a>Batch ile sertifikaları kullanma

Şu anda, bir uç nokta ile kimlik doğrulaması gereken havuzlarda çalışan uygulamalarınız varsa Batch ile sertifikaların kullanılması için temel neden olur. 

Henüz bir sertifikanız yoksa, komut satırı aracını kullanarak otomatik olarak imzalanan bir sertifika oluşturabilirsiniz `makecert` .

## <a name="upload-certificates-manually-through-the-azure-portal"></a>Sertifikaları Azure portal aracılığıyla el ile yükleme

1. Bir sertifika yüklemek istediğiniz Batch hesabından **Sertifikalar** ' ı seçin ve ardından **Ekle**' yi seçin. 

2. Sertifikayı bir. pfx veya. cer uzantısıyla karşıya yükleyin. 

Karşıya yüklendikten sonra sertifika bir sertifika listesine eklenir ve parmak izini doğrulayabilirsiniz.

Artık bir Batch havuzu oluşturduğunuzda, havuz içindeki sertifikalara gidebilir ve bu havuza yüklediğiniz sertifikayı atayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Batch bir sertifika API 'sine sahiptir, [az Batch Certificate Create](/cli/azure/batch/certificate)

Key Vault kullanımı hakkında bilgi için bkz. [Batch Ile güvenli erişim Key Vault](credential-access-key-vault.md).
