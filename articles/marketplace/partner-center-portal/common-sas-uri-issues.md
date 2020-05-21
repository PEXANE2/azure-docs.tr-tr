---
title: Ortak SAS URI sorunları ve düzeltmeleri-Azure Marketi
description: Paylaşılan erişim imzaları ile çalışırken karşılaşılan yaygın sorunlar ve önerilen çözünürlükler.
author: anbene
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/09/2020
ms.openlocfilehash: 56db3562efdc0406e745fd38b73df0a473d0ecd5
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83724607"
---
# <a name="common-sas-uri-issues-and-fixes"></a>Ortak SAS URI sorunları ve düzeltmeleri

Aşağıda, paylaşılan erişim imzaları ile çalışırken (çözümünüz için karşıya yüklenen VHD 'leri tanımlamak ve paylaşmak için kullanılan) ve önerilen çözümlerin yanı sıra karşılaşılan yaygın sorunlar verilmiştir.

| **Konuda** | **Hata Iletisi** | **Düzeltme** |
| --------- | ------------------- | ------- |
| *Görüntüler kopyalanırken hata oluştu* |  |  |
| SAS URI 'sinde "?" bulunamadı | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Önerilen araçları kullanarak SAS URI 'sini güncelleştirin. |
| SAS URI 'SI içinde "St" ve "sa" parametreleri yok | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | SAS URI 'sini uygun **Başlangıç tarihi** ve **bitiş tarihi** değerleriyle güncelleştirin. |
| SAS URI 'SI içinde "SP = rl" değil | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | SAS URI 'sini, ve olarak ayarlanan izinlerle `Read` güncelleştirin `List` . |
| SAS URI 'SI, VHD adında boşluk içeriyor | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Boşluk kaldırmak için SAS URI 'sini güncelleştirin. |
| SAS URI yetkilendirme hatası | `Failure: Copying Images. Not able to download blob due to authorization error.` | SAS URI biçimini gözden geçirin ve düzeltin. Gerekirse yeniden oluşturun. |
| SAS URI 'SI "St" ve "o" parametrelerinin tam tarih-saat belirtimi yoktur | `Failure: Copying Images. Not able to download blob due to incorrect SAS Uri.` | SAS URI 'SI **Başlangıç tarihi** ve **bitiş tarihi** parametreleri ( `st` ve alt `se` dizeleri), gibi tam bir tarih-saat biçimine sahip olmalıdır `11-02-2017T00:00:00Z` . Kısaltılmış sürümler geçersiz (Azure CLı 'deki bazı komutlar varsayılan olarak kısaltılmış değerler oluşturabilir). |
|  |  |  |

Ayrıntılar için bkz. [paylaşılan erişim Imzalarını kullanma (SAS)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/).
