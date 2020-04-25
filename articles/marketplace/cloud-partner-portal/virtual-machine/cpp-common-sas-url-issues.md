---
title: Azure Marketi için ortak SAS URL 'SI sorunları ve düzeltmeleri
description: Paylaşılan erişim imzası URI 'Leri ve olası çözümleri kullanma hakkında yaygın sorunları listeleyin.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/27/2018
ms.author: dsindona
ms.openlocfilehash: 723762695d34380b7f237fa9082dc470dafcc8df
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82147038"
---
# <a name="common-sas-url-issues-and-fixes"></a>Ortak SAS URL 'SI sorunları ve düzeltmeleri

> [!IMPORTANT]
> 13 Nisan 2020 ' den itibaren, Azure sanal makine tekliflerinizin yönetimini Iş Ortağı Merkezi 'ne taşımaya başlayacağız. Geçişten sonra, Iş Ortağı Merkezi 'nde tekliflerinizi oluşturup yönetirsiniz. Geçirilmiş tekliflerinizi yönetmek için [ortak SAS URL 'si sorunları ve düzeltmeleri](https://docs.microsoft.com/azure/marketplace/partner-center-portal/common-sas-uri-issues) bölümündeki yönergeleri izleyin.

Aşağıdaki tabloda, önerilen çözümlerin yanı sıra paylaşılan erişim imzaları (çözümünüz için karşıya yüklenen VHD 'leri tanımlamak ve paylaşmak için kullanılır) ile çalışırken karşılaşılan bazı yaygın sorunlar listelenmektedir.

| **Konuda** | **Hata Iletisi** | **Düzeltme** | 
| --------- | ------------------- | ------- | 
| &emsp;  *Görüntüler kopyalanırken hata oluştu* |  |  |
| SAS URL 'sinde "?" bulunamadı | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Önerilen araçları kullanarak SAS URL 'sini güncelleştirin. |
| SAS URL 'SI içinde "St" ve "sa" parametreleri yok | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | SAS URL 'sini, içinde uygun **Başlangıç tarihi** ve **bitiş tarihi** değerleriyle güncelleştirin. | 
| "SP = rl" SAS URL 'SI içinde değil | `Failure: Copying Images. Not able to download blob using provided SAS Uri` | Ve `Read` `List`olarak ayarlanan izinlerle SAS URL 'sini güncelleştirin. | 
| SAS URL 'sinde, VHD adında boşluk vardır | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Boşluk kaldırmak için SAS URL 'sini güncelleştirin. |
| SAS URL 'SI yetkilendirme hatası | `Failure: Copying Images. Not able to download blob due to authorization error` | SAS URI biçimini gözden geçirin ve düzeltin. Gerekirse yeniden oluşturun. |
| SAS URL 'SI "St" ve "o" parametrelerinin tam tarih-saat belirtimi yoktur | `Failure: Copying Images. Not able to download blob due to incorrect SAS URL` | SAS URL 'SI **Başlangıç tarihi** ve **bitiş tarihi** parametrelerinin`st` ( `se` ve alt dizelerinin), gibi tam bir tarih saat biçimi olması `11-02-2017T00:00:00Z`gerekir. Kısaltılmış sürümler geçerli değil. (Azure CLı 'deki bazı komutlar varsayılan olarak kısaltılmış değerler oluşturabilir.) | 
|  |  |  |

Daha fazla bilgi için bkz. [paylaşılan erişim Imzalarını kullanma (SAS)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/).
