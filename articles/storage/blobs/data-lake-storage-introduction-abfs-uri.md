---
title: Azure Data Lake Storage 2. Nesil URI'sini kullanma
description: Azure blob dosya sistemi sürücüsünü (Azure Data Lake Storage 2. için Hadoop FileSystem sürücüsü) temsil eden ABFS şema tanımlayıcısı için URI sözdizimini öğrenin.
author: normesta
ms.topic: conceptual
ms.author: normesta
ms.date: 12/06/2018
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.reviewer: jamesbak
ms.openlocfilehash: 7c9c452d608bc2c5f64292d72f3eae6825bf1ce2
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87828260"
---
# <a name="use-the-azure-data-lake-storage-gen2-uri"></a>Azure Data Lake Storage 2. Nesil URI'sini kullanma

Azure Data Lake Storage 2. ile uyumlu [Hadoop FileSystem](https://www.aosabook.org/en/hdfs.html) sürücüsü, kendi düzen tanımlayıcısı `abfs` (Azure blob dosya sistemi) tarafından bilinir. Diğer Hadoop dosya sistemi sürücüleriyle tutarlı, ABFS sürücüsü Data Lake Storage 2. özellikli bir hesap içindeki dosya ve dizinleri adreslemesine yönelik bir URI biçimi kullanır.

## <a name="uri-syntax"></a>URI sözdizimi

Data Lake Storage 2. URI sözdizimi, depolama hesabınızın varsayılan dosya sistemi olarak Data Lake Storage 2. sahip olmak üzere ayarlanmış olup olmadığına bağlıdır.

Adres eklemek istediğiniz Data Lake Storage 2. yetenekli **Hesap, hesap** oluşturma sırasında varsayılan dosya sistemi olarak ayarlanmamışsa, toplu URI sözdizimi şöyledir:

<pre>abfs[s]<sup>1</sup>://&lt;file_system&gt;<sup>2</sup>@&lt;account_name&gt;<sup>3</sup>.dfs.core.windows.net/&lt;path&gt;<sup>4</sup>/&lt;file_name&gt;<sup>5</sup></pre>

1. **Düzen tanımlayıcısı**: `abfs` protokol, düzen tanımlayıcısı olarak kullanılır. Daha önce Güvenli Yuva Katmanı (SSL), bağlantı olarak bilinen bir Aktarım Katmanı Güvenliği (TLS) ile veya bunlarla bağlanma seçeneğiniz vardır. `abfss`BIR TLS bağlantısıyla bağlanmak için kullanın.

2. **Dosya sistemi**: dosya ve klasörleri tutan ana konum. Bu, Azure depolama Blobları hizmetindeki kapsayıcılarla aynıdır.

3. **Hesap adı**: oluşturma sırasında depolama hesabınıza verilen ad.

4. **Yollar**: dizin yapısının eğik çizgi sınırlı ( `/` ) temsili.

5. **Dosya adı**: tek dosyanın adı. Bir dizinin adreslenmesi durumunda bu parametre isteğe bağlıdır.

Ancak, adres eklemek istediğiniz hesap, hesap oluşturma sırasında varsayılan dosya sistemi olarak ayarlandıysa, toplu URI sözdizimi şöyledir:

<pre>/&lt;path&gt;<sup>1</sup>/&lt;file_name&gt;<sup>2</sup></pre>

1. **Yol**: dizin yapısının eğik çizgi sınırlı ( `/` ) temsili.

2. **Dosya adı**: tek dosyanın adı.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure HDInsight kümeleriyle Azure Data Lake Storage 2. Nesil hizmetini kullanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
