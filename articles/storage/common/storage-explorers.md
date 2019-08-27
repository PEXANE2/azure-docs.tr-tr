---
title: Azure depolama ile çalışmaya yönelik araçlar
description: Azure depolama verilerinizi görüntülemenize ve bunlarla etkileşime geçmesini sağlayan araçların listesi.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 08/26/2019
ms.author: tamram
ms.reviewer: dineshm
ms.subservice: common
ms.openlocfilehash: 11838a50d70d1b9a0216505e9ef0958d3b18bdac
ms.sourcegitcommit: 94ee81a728f1d55d71827ea356ed9847943f7397
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70035257"
---
# <a name="azure-storage-client-tools"></a>Azure Storage İstemci Araçları
Azure Storage kullanıcıları, Azure depolama istemci aracını kullanarak verilerini görüntülemek/bunlarla etkileşime geçmek ister. Aşağıdaki tablolarda, bunu yapmanıza imkan tanıyan bir dizi araç listeliyoruz. Veri soyutlamasını listeleme ve/veya erişim olanağı sağlıyorsa, her bloğa bir "X" koyuyoruz. Tablo ayrıca araçların serbest olup olmadığını gösterir. "Deneme", ücretsiz bir deneme olduğunu gösterir ancak tam ürün ücretsizdir. "Y/N" bir sürümün ücretsiz olduğunu, satın alma için farklı bir sürüm bulunduğunu belirtir.

Kullanılabilir Azure depolama istemci araçlarının bir anlık görüntüsünü sağladık. Bu araçlar, işlevsellik içinde gelişmeye ve büyümeye devam edebilir. Düzeltmeler veya güncelleştirmeler varsa, bize bildirmek için lütfen bir yorum bırakın. Bu, burada yer alan araçların bilinmiş olması halinde de geçerlidir. bunları eklememiz kutlu olsun.

**Microsoft Azure Depolama Istemci araçları**

<table>
  <tr>
    <th rowspan="2">Azure depolama Istemci aracı</th>
    <th rowspan="2">Blok Blobu</th>
    <th rowspan="2">Sayfa Blobu</th>
    <th rowspan="2">Ekleme Blobu</th>
    <th rowspan="2">Tablolar</th>
    <th rowspan="2">Sıralar</th>
    <th rowspan="2">Dosyalar</th>
    <th rowspan="2">Boş</th>
    <th colspan="4">Platform</th>
  </tr>
  <tr>
    <td>Web</td>
    <td>Windows</td>
    <td>OSX</td>
    <td>Linux</td>
  </tr>
  <tr>
    <td><a href="https://azure.microsoft.com/features/azure-portal/">Microsoft Azure portalı</a></td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>E</td>
    <td>X</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><a href="https://storageexplorer.com/">Microsoft Azure Depolama Gezgini</a></td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>E</td>
    <td></td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
  </tr>
  <tr>
    <td><a href="https://www.visualstudio.com/features/azure-tools-vs.aspx">Microsoft Visual Studio Sunucu Gezgini</a></td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td></td>
    <td>E</td>
    <td></td>
    <td>X</td>
    <td></td>
    <td></td>
  </tr>
</table>

**Üçüncü taraf Azure depolama Istemci araçları**

Aşağıdaki üçüncü taraf araçları tarafından talep edilen işlevleri veya kaliteyi doğrulamadı ve bunların listelenmesi Microsoft tarafından bir onay göstermez.

<table>
  <tr>
    <th rowspan="2">Azure depolama Istemci aracı</th>
    <th rowspan="2">Blok Blobu</th>
    <th rowspan="2">Sayfa Blobu</th>
    <th rowspan="2">Ekleme Blobu</th>
    <th rowspan="2">Tablolar</th>
    <th rowspan="2">Sıralar</th>
    <th rowspan="2">Dosyalar</th>
    <th rowspan="2">Boş</th>
    <th colspan="4">Platform</th>
  </tr>
  <tr>
    <td>Web</td>
    <td>Windows</td>
    <td>OSX</td>
    <td>Linux</td>
  </tr>
  <tr>
    <td><a href="https://www.cerebrata.com/products/azure-management-studio/introduction">Sertifika kimliği: Azure Management Studio</a></td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>Deneme</td>
    <td></td>
    <td>X</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><a href="https://github.com/sebagomez/azurestorageexplorer">Azure Web Depolama Gezgini</a></td>
    <td>X</td>
    <td>X</td>
    <td></td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>E</td>
    <td>X</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><a href="https://www.cloudberrylab.com/explorer/microsoft-azure.aspx">Cloudbraz Gezgini</a></td>
    <td>X</td>
    <td>X</td>
    <td></td>
    <td></td>
    <td></td>
    <td>X</td>
    <td>E/H</td>
    <td></td>
    <td>X</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><a href="https://www.gapotchenko.com/cloudcombine">Bulut birleştirme</a></td>
    <td>X</td>
    <td>X</td>
    <td></td>
    <td>X</td>
    <td>X</td>
    <td></td>
    <td>Deneme</td>
    <td></td>
    <td>X</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><a href="https://clumsyleaf.com">Gerçekleştirmek AzureXplorer, CloudXplorer, TableXplorer</a></td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>E</td>
    <td></td>
    <td>X</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><a href="https://www.gladinet.com/Azure-Storage/index.htm">Gladinet bulutu</a></td>
    <td>X</td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td>Deneme</td>
    <td></td>
    <td>X</td>
    <td></td>
    <td></td>
  </tr>
</table>
