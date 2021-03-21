---
title: GitHub'a Bağlan
description: Ortak veri modeli varlık başvurularınızı belirtmek için GitHub kullanın
author: djpmsft
ms.service: data-factory
ms.topic: conceptual
ms.date: 06/03/2020
ms.author: daperlov
ms.openlocfilehash: 5d555d7bc4d3aae9c016cacbe17b68c30859d99a
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "100372289"
---
# <a name="use-github-to-read-common-data-model-entity-references"></a>Ortak veri modeli varlık başvurularını okumak için GitHub kullanın

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Azure Data Factory 'daki GitHub Bağlayıcısı, yalnızca eşleme veri akışı 'nda [ortak veri modeli](format-common-data-model.md) için varlık başvurusu şemasını almak için kullanılır.

## <a name="linked-service-properties"></a>Bağlı hizmet özellikleri

GitHub bağlantılı hizmeti için aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Type özelliği **GitHub** olarak ayarlanmalıdır. | evet
| userName | GitHub Kullanıcı adı | evet |
| password | GitHub parolası | evet |

## <a name="next-steps"></a>Sonraki Adımlar

Eşleme veri akışında bir [kaynak veri kümesi](data-flow-source.md) oluşturun.