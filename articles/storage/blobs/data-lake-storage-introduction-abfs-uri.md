---
title: Azure Data Lake Storage 2. Nesil URI'sini kullanma
description: Azure Data Lake Storage 2. Nesil URI'sini kullanma
author: normesta
ms.topic: conceptual
ms.author: normesta
ms.date: 12/06/2018
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.reviewer: jamesbak
ms.openlocfilehash: 04df30c2a97e865d23999df26768b38cb38be607
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68855555"
---
# <a name="use-the-azure-data-lake-storage-gen2-uri"></a>Azure Data Lake Storage 2. Nesil URI'sini kullanma

Azure Veri Gölü Depolama Gen2 ile uyumlu [Olan Hadoop Filesystem](https://www.aosabook.org/en/hdfs.html) sürücüsü, `abfs` düzen tanımlayıcısı (Azure Blob Dosya Sistemi) ile bilinir. Diğer Hadoop Filesystem sürücüleri ile tutarlı, ABFS sürücüsü bir Veri Gölü Depolama Gen2 yetenekli hesap içinde dosyaları ve dizinleri adreslemek için bir URI biçimi kullanır.

## <a name="uri-syntax"></a>URI sözdizimi

Veri Gölü Depolama Gen2 için URI sözdizimi, depolama hesabınızın varsayılan dosya sistemi olarak Veri Gölü Depolama Gen2'sine sahip olacak şekilde ayarlanıp ayarlanmadığına bağlıdır.

Adreslemek istediğiniz Veri Gölü Depolama Gen2 özellikli hesap hesap oluşturma sırasında varsayılan dosya sistemi olarak **ayarlanmıyorsa,** steno URI sözdizimi şudur:

<pre>abfs[s]<sup>1</sup>://&lt;file_system&gt;<sup>2</sup>@&lt;account_name&gt;<sup>3</sup>.dfs.core.windows.net/&lt;path&gt;<sup>4</sup>/&lt;file_name&gt;<sup>5</sup></pre>

1. **Şema tanımlayıcısı**: `abfs` Protokol, şema tanımlayıcısı olarak kullanılır. Güvenli bir soket katmanı (SSL) bağlantısı na bağlanma seçeneğiniz vardır. Güvenli `abfss` bir soket katmanı bağlantısıyla bağlantı kurmak için kullanın.

2. **Dosya sistemi**: Dosya ve klasörleri tutan ana konum. Bu, Azure Depolama Blobs hizmetindeki Kapsayıcılarla aynıdır.

3. **Hesap adı**: Oluşturma sırasında depolama hesabınıza verilen ad.

4. **Yollar**: Dizin yapısının ileri eğik çizgi delimited (`/`) gösterimi.

5. **Dosya adı**: Tek tek dosyanın adı. Bir dizine hitap ediyorsanız, bu parametre isteğe bağlıdır.

Ancak, adresvermek istediğiniz hesap hesap oluşturma sırasında varsayılan dosya sistemi olarak ayarlanırsa, steno URI sözdizimi şu şekildedir:

<pre>/&lt;path&gt;<sup>1</sup>/&lt;file_name&gt;<sup>2</sup></pre>

1. **Yol**: Dizin yapısının ileri eğik çizgi delimited (`/`) gösterimi.

2. **Dosya Adı**: Tek tek dosyanın adı.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure HDInsight kümeleriyle Azure Veri Gölü Depolama Gen2'yi kullanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
