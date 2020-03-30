---
title: Azure Marketi için yaygın SAS URL sorunları ve düzeltmeleri
description: Paylaşılan erişim imzası URI'lerini ve olası çözümleri kullanarak ilgili sık karşılaşılan sorunları listele.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/27/2018
ms.author: dsindona
ms.openlocfilehash: 47702959474a352a8e13710ec850f789dee4d517
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80278170"
---
# <a name="common-sas-url-issues-and-fixes"></a>Sık karşılaşılan SAS URL sorunları ve düzeltmeleri

Aşağıdaki tablo, paylaşılan erişim imzalarıyla çalışırken karşılaşılan bazı yaygın sorunları (çözümünüz için yüklenen VHD'leri tanımlamak ve paylaşmak için kullanılır) ve önerilen çözümlerle birlikte listeler.

| **Sorunu** | **Hata İletisi** | **Düzeltme** | 
| --------- | ------------------- | ------- | 
| &emsp;  *Görüntülerin kopyalanmasında hata* |  |  |
| "?" SAS URL'sinde bulunmuyor | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Önerilen araçları kullanarak SAS URL'sini güncelleştirin. |
| SAS URL'sinde olmayan "st" ve "se" parametreleri | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | SAS URL'sini uygun **Başlangıç Tarihi** ve **Bitiş Tarihi** değerleriyle güncelleştirin. | 
| "sp=rl" SAS URL'de değil | `Failure: Copying Images. Not able to download blob using provided SAS Uri` | SAS URL'sini ve `Read` `List`'' olarak ayarlanmış izinlerle güncelleştirin. | 
| SAS URL'de VHD adında beyaz boşluklar var | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Beyaz boşlukları kaldırmak için SAS URL'sini güncelleştirin. |
| SAS URL Yetkilendirme hatası | `Failure: Copying Images. Not able to download blob due to authorization error` | SAS URI biçimini gözden geçirin ve düzeltin. Gerekirse yenile. |
| SAS URL "st" ve "se" parametreleri tam tarih-saat belirtimi yok | `Failure: Copying Images. Not able to download blob due to incorrect SAS URL` | SAS URL **Başlangıç Tarihi** ve `se` Bitiş **Tarihi** parametreleri (ve`st` alt dizeleri) `11-02-2017T00:00:00Z`gibi tam tarih biçimine sahip olması gerekir. Kısaltılmış sürümler geçerli değildir. (Azure CLI'deki bazı komutlar varsayılan olarak kısaltılmış değerler oluşturabilir.) | 
|  |  |  |

Daha fazla bilgi için bkz. [paylaşılan erişim imzalarını (SAS) kullanma.](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)
