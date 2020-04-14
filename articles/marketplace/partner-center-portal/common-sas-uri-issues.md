---
title: Yaygın SAS URI sorunları ve düzeltmeleri - Azure Marketi
description: Paylaşılan erişim imzalarıyla çalışırken karşılaşılan sık karşılaşılan sorunlar ve önerilen çözümler.
author: anbene
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/09/2020
ms.openlocfilehash: fb86b1c5ec3be5a423dc2abd295aa8beb8f23f47
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81266246"
---
# <a name="common-sas-uri-issues-and-fixes"></a>Ortak SAS URI sorunları ve düzeltmeleri

> [!IMPORTANT]
> Azure Sanal Makine tekliflerinizin yönetimini Bulut İş Ortağı Portalı'ndan İş Ortağı Merkezi'ne taşıyoruz. Teklifleriniz geçirilene kadar, lütfen tekliflerinizi yönetmek için Cloud Partner Portal için [Ortak SAS URI sorunları ve düzeltmeleri](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-common-sas-url-issues) yönergelerini izleyin.

Paylaşılan erişim imzalarıyla çalışırken (çözümünüz için yüklenen VHD'leri tanımlamak ve paylaşmak için kullanılan) ve önerilen çözümlerle karşılaşılan sık karşılaşılan sorunlar şunlardır.

| **Sorunu** | **Hata İletisi** | **Düzeltme** |
| --------- | ------------------- | ------- |
| *Görüntülerin kopyalanmasında hata* |  |  |
| "?" SAS URI'de bulunmaz | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Önerilen araçları kullanarak SAS URI'yi güncelleştirin. |
| SAS URI'de olmayan "st" ve "se" parametreleri | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | SAS URI'yi uygun **Başlangıç Tarihi** ve **Bitiş Tarihi** değerleriyle güncelleştirin. |
| "sp=rl" Değil SAS URI | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | SAS URI'yi ve `Read` `List`'de ayarlanan izinlerle güncelleştirin. |
| SAS URI VHD adında beyaz boşluklar vardır | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Beyaz boşlukları kaldırmak için SAS URI'yi güncelleştirin. |
| SAS URI Yetkilendirme hatası | `Failure: Copying Images. Not able to download blob due to authorization error.` | SAS URI biçimini gözden geçirin ve düzeltin. Gerekirse yenile. |
| SAS URI "st" ve "se" parametreleri tam tarih-zaman belirtimi yok | `Failure: Copying Images. Not able to download blob due to incorrect SAS Uri.` | SAS URI **Başlangıç Tarihi** ve `se` Bitiş **Tarihi** parametreleri (ve`st` alt dizeleri) gibi `11-02-2017T00:00:00Z`tam tarih-saat biçimine sahip olmalıdır. Kısaltılmış sürümler geçersizdir (Azure CLI'deki bazı komutlar varsayılan olarak kısaltılmış değerler oluşturabilir). |
|  |  |  |

Ayrıntılar için bkz: [Paylaşılan erişim imzalarını (SAS) kullanma.](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)
