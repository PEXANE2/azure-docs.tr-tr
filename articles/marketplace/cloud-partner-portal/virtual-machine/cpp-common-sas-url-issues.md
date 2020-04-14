---
title: Azure Marketi için yaygın SAS URL sorunları ve düzeltmeleri
description: Paylaşılan erişim imzası URI'lerini ve olası çözümleri kullanarak ilgili sık karşılaşılan sorunları listele.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/27/2018
ms.author: dsindona
ms.openlocfilehash: c575389538230218e1e6e4f172ebcfbee8ee51dc
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273469"
---
# <a name="common-sas-url-issues-and-fixes"></a>Sık karşılaşılan SAS URL sorunları ve düzeltmeleri

> [!IMPORTANT]
> 13 Nisan 2020'den itibaren Azure Sanal Makine tekliflerinizin yönetimini İş Ortağı Merkezi'ne taşımaya başlayacağız. Geçişten sonra, Tekliflerinizi İş Ortağı Merkezi'nde oluşturur ve yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Ortak SAS URL sorunları ve düzeltmeler](https://aka.ms/AzureSAS_URL_FAQ) yönergeleriizleyin.

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
