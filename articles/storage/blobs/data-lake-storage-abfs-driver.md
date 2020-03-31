---
title: Azure Veri Gölü Depolama Gen2 için Azure Blob Dosya Sistemi sürücüsü
description: ABFS Hadoop Filesystem sürücüsü
author: normesta
ms.topic: conceptual
ms.author: normesta
ms.reviewer: jamesbak
ms.date: 12/06/2018
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.openlocfilehash: 3db039d39ef532ea51143dc9cbdb6bd5f29d6225
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75970269"
---
# <a name="the-azure-blob-filesystem-driver-abfs-a-dedicated-azure-storage-driver-for-hadoop"></a>Azure Blob Filesystem sürücüsü (ABFS): Hadoop için özel bir Azure Depolama sürücüsü

Azure Veri Gölü Depolama Gen2'deki veriler için birincil erişim yöntemlerinden biri [Hadoop FileSystem'dir.](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/index.html) Data Lake Storage Gen2, Azure Blob Depolama kullanıcılarının yeni bir sürücüye, Azure Blob Dosya Sistemi sürücüsüne veya `ABFS`. ABFS, Apache Hadoop'un bir parçasıdır ve Hadoop'un ticari dağıtımlarının çoğunda yer alan bir dizi reklamda yer alan bir dizi şirkettir. Bu sürücüyü kullanarak, birçok uygulama ve çerçeve, Veri Gölü Depolama Gen2'ye açıkça atıfta bulunan herhangi bir kod olmadan Azure Blob Depolama'daki verilere erişebilir.

## <a name="prior-capability-the-windows-azure-storage-blob-driver"></a>Önceki özellik: Windows Azure Depolama Blob sürücüsü

Windows Azure Depolama Blob sürücüsü veya [WASB sürücüsü](https://hadoop.apache.org/docs/current/hadoop-azure/index.html) Azure Blob Depolama için özgün desteği sağladı. Bu sürücü, dosya sistemi semantiklerini (Hadoop FileSystem arabirimi nin gerektirdiği şekilde) Azure Blob Depolama tarafından açığa çıkarılan nesne deposu stili arabirimininkiyle eşleme karmaşık görevini yerine getirir. Bu sürücü, blobs depolanan verilere yüksek performanslı erişim sağlayarak bu modeli desteklemeye devam eder, ancak bu eşlemi gerçekleştiren kod önemli miktarda içerir, bu da bakımı zor hale. Ayrıca, [filesystem.rename()](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/filesystem.html#boolean_renamePath_src_Path_d) ve [FileSystem.delete()](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/filesystem.html#boolean_deletePath_p_boolean_recursive) dizinlere uygulandığında bazı işlemler, sürücünün çok sayıda işlem gerçekleştirmesini gerektirir (nesne depolarının dizinler için destek eksikliği nedeniyle) genellikle performansın düşmesine neden olur. ABFS sürücüsü WASB'ın doğal eksikliklerini gidermek için tasarlanmıştır.

## <a name="the-azure-blob-file-system-driver"></a>Azure Blob Dosya Sistemi sürücüsü

[Azure Veri Gölü Depolama REST arabirimi,](https://docs.microsoft.com/rest/api/storageservices/data-lake-storage-gen2) Azure Blob Depolama üzerinden dosya sistemi semantikini desteklemek üzere tasarlanmıştır. Hadoop FileSystem da aynı semantik desteklemek için tasarlanmış olduğu göz önüne alındığında sürücü karmaşık bir haritalama için bir gereklilik yoktur. Bu nedenle, Azure Blob Dosya Sistemi sürücüsü (veya ABFS), REST API için yalnızca bir istemci şim'dir.

Ancak, sürücünün yine de gerçekleştirmesi gereken bazı işlevler vardır:

### <a name="uri-scheme-to-reference-data"></a>Uri şeması veri referans

Hadoop'taki diğer FileSystem uygulamalarıyla tutarlı olarak, ABFS sürücüsü kendi URI düzenini tanımlar, böylece kaynaklar (dizinler ve dosyalar) belirgin bir şekilde ele alınabilir. URI şeması [Azure Veri Gölü Depolama Gen2 URI'yi kullanın'da](./data-lake-storage-introduction-abfs-uri.md)belgelenmiştir. URI'nin yapısı:`abfs[s]://file_system@account_name.dfs.core.windows.net/<path>/<path>/<file_name>`

Yukarıdaki URI biçimini kullanarak, standart Hadoop araçları ve çerçeveleri bu kaynaklara başvurmak için kullanılabilir:

```bash
hdfs dfs -mkdir -p abfs://fileanalysis@myanalytics.dfs.core.windows.net/tutorials/flightdelays/data
hdfs dfs -put flight_delays.csv abfs://fileanalysis@myanalytics.dfs.core.windows.net/tutorials/flightdelays/data/
```

Dahili olarak, ABFS sürücüsü URI'de belirtilen kaynak(lar)ı dosyalara ve dizinlere çevirir ve bu referanslarla Azure Veri Gölü Depolama REST API'sine çağrı yapar.

### <a name="authentication"></a>Kimlik doğrulaması

ABFS sürücüsü, Hadoop uygulamasının Veri Gölü Depolama Gen2 özellikli bir hesapta bulunan kaynaklara güvenli bir şekilde erişebilmesi için iki kimlik doğrulama biçimini destekler. Kullanılabilir kimlik doğrulama şemalarının tüm ayrıntıları [Azure Depolama güvenlik kılavuzunda](security-recommendations.md)verilmiştir. Bunlar:

- **Paylaşılan Anahtar:** Bu, kullanıcıların hesaptaki TÜM kaynaklara erişmelerine izin verir. Anahtar şifrelenir ve Hadoop yapılandırmasında saklanır.

- **Azure Etkin Dizin OAuth Taşıyıcı Belirteci:** Azure AD taşıyıcı belirteçleri, son kullanıcının veya yapılandırılmış Hizmet Sorumlusunun kimliği kullanılarak sürücü tarafından elde edilir ve yenilenir. Bu kimlik doğrulama modeli kullanılarak, tüm erişim, sağlanan belirteçile ilişkili kimlik kullanılarak her çağrı başına yetkilendirilir ve atanan POSIX Erişim Kontrol Listesi (ACL) karşısında değerlendirilir.

   > [!NOTE]
   > Azure Veri Gölü Depolama Gen2 yalnızca Azure AD v1.0 uç noktalarını destekler.

### <a name="configuration"></a>Yapılandırma

ABFS sürücüsü için tüm yapılandırma <code>core-site.xml</code> yapılandırma dosyasında depolanır. [Ambari](https://ambari.apache.org/)içeren Hadoop dağıtımlarında, yapılandırma web portalı veya Ambari REST API kullanılarak da yönetilebilir.

Desteklenen tüm yapılandırma girişlerinin ayrıntıları [Resmi Hadoop belgelerinde](https://hadoop.apache.org/docs/stable/hadoop-azure/abfs.html)belirtilir.

### <a name="hadoop-documentation"></a>Hadoop belgeleri

ABFS sürücüsü, [Resmi Hadoop belgelerinde](https://hadoop.apache.org/docs/stable/hadoop-azure/abfs.html) tam olarak belgelenmiştir

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Veri Tuğlaları Kümesi Oluşturma](./data-lake-storage-quickstart-create-databricks-account.md)
- [Azure Data Lake Storage 2. Nesil URI'sini kullanma](./data-lake-storage-introduction-abfs-uri.md)
