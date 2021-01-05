---
title: Hizmet sınırlamaları
titleSuffix: Azure Machine Learning
description: Kapasite planlaması ve Azure Machine Learning istek ve yanıtlara yönelik maksimum limitlerde kullanılan hizmet limitleri.
services: machine-learning
author: andscho
ms.author: andscho
ms.reviewer: mldocs
ms.topic: reference
ms.service: machine-learning
ms.subservice: core
ms.date: 12/21/2020
ms.openlocfilehash: 132c4b223b99f7110cd4553bb0dd93f1e58f4cdd
ms.sourcegitcommit: a89a517622a3886b3a44ed42839d41a301c786e0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2020
ms.locfileid: "97733391"
---
# <a name="service-limits-in-azure-machine-learning"></a>Azure Machine Learning hizmet limitleri

Bu bölümde Azure Machine Learning içindeki temel kotalar ve azaltma eşikleri listelenmektedir.

## <a name="workspaces"></a>Çalışma alanları
| Sınır | Değer |
| --- | --- |
| Çalışma alanı adı | 2-32 karakter |

## <a name="runs"></a>Çalıştırmalar
| Sınır | Değer |
| --- | --- |
| Çalışma alanı başına çalıştırma | 10 milyon |
| RunId/ParentRunId | 256 karakter |
| Veri Containerıd | 261 karakter |
| DisplayName |256 karakter|
| Açıklama |5.000 karakter|
| Özellik sayısı |50 |
| Özellik anahtarının uzunluğu |100 karakter |
| Özellik değerinin uzunluğu |1.000 karakter |
| Etiket sayısı |50 |
| Etiket anahtarının uzunluğu |100 |
| Etiket değeri uzunluğu |1.000 karakter |
| CancelUri/CompleteUri/Diagnosticsurı |1.000 karakter |
| Hata iletisi uzunluğu |3.000 karakter |
| Uyarı iletisi uzunluğu |300 karakter |
| Giriş veri kümesi sayısı |200 |
| Çıkış veri kümesi sayısı |20 |


## <a name="metrics"></a>Ölçümler
| Sınır | Değer |
| --- | --- |
| Çalışma başına ölçüm adları |50|
| Ölçüm başına ölçüm satırı adı |10 milyon|
| Ölçüm satırı başına sütun sayısı |15|
| Ölçüm sütun adı uzunluğu |255 karakter |
| Ölçüm sütunu değer uzunluğu |255 karakter |
| Toplu iş başına ölçüm satırları karşıya yüklendi | 250 |

> [!NOTE]
> Değişkenleri ölçüm adıyla biçimlendirtiğinden, çalışma başına ölçüm adı sınırına ulaşdıysanız, bir sütunun değişken değeri olduğu ve ikinci sütunun ölçüm değeri olduğu bir satır ölçümü kullanmak yerine göz önünde bulundurun.

## <a name="artifacts"></a>Artifacts

| Sınır | Değer |
| --- | --- |
| Çalışma başına yapıt sayısı |10 milyon|
| Yapıt yolunun en fazla uzunluğu |5.000 karakter |

## <a name="limit-increases"></a>Sınır artışları
Bazı sınırlar, [Destek ile iletişim](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/)kurarak bireysel çalışma alanları için artırılabilir. 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Machine Learning ortamınızı yapılandırma](how-to-configure-environment.md)
