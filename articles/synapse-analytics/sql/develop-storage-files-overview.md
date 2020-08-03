---
title: İsteğe bağlı SQL 'de (Önizleme) depolama üzerindeki dosyalara erişin
description: SYNAPSE SQL 'de isteğe bağlı SQL (Önizleme) kaynaklarını kullanarak depolama dosyalarının sorgulanmasını açıklar.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql
ms.date: 04/19/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 3c33e2152fc120d406886d89adda26603126a8ba
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87483561"
---
# <a name="access-external-storage-in-synapse-sql-on-demand"></a>SYNAPSE SQL 'de dış depolamaya erişme (isteğe bağlı)

Bu belgede, SYNAPSE SQL 'de (isteğe bağlı) Azure depolama 'da depolanan dosyalardaki verileri nasıl okuyabileceğinizi açıklar. Kullanıcılar depolamaya erişmek için aşağıdaki seçeneklere sahiptir:

- Azure depolama 'daki dosyalar üzerinde geçici sorgular sağlayan [OPENROWSET](develop-openrowset.md) işlevi.
- Dış dosyalar kümesinin üzerine inşa edilen önceden tanımlanmış bir veri yapısı olan [dış tablo](develop-tables-external-tables.md) .

Kullanıcı, Azure AD PASSTHROUGH kimlik doğrulaması (Azure AD sorumluları için varsayılan) ve SAS kimlik doğrulaması (SQL sorumluları için varsayılan) gibi [farklı kimlik doğrulama yöntemleri](develop-storage-files-storage-access-control.md) kullanabilir.

## <a name="openrowset"></a>OPENROWSET

[OPENROWSET](develop-openrowset.md) işlevi, kullanıcının Azure Storage 'dan dosyaları okumasına olanak sağlar.

### <a name="query-files-using-openrowset"></a>OPENROWSET kullanarak dosyaları sorgulama

OPENROWSET, kullanıcıların depolama üzerinde erişimi olan Azure Storage 'daki dış dosyaları sorgulamasına olanak sağlar. SYNAPSE SQL isteğe bağlı uç noktasına bağlı olan Kullanıcı, Azure depolama 'daki dosyaların içeriğini okumak için aşağıdaki sorguyu kullanmalıdır:

```sql
SELECT * FROM
 OPENROWSET(BULK 'https://<storage_account>.dfs.core.windows.net/<container>/<path>/*.parquet', format= 'parquet') as rows
```

Kullanıcı, aşağıdaki erişim kurallarını kullanarak depolamaya erişebilir:

- Azure AD kullanıcısı-OPENROWSET, Azure depolama 'ya erişmek veya anonim erişimle depolama alanına erişmek için çağıranın Azure AD kimliğini kullanır.
- SQL User – OPENROWSET, depolamaya anonim erişimle erişir.

SQL sorumluları Ayrıca, SAS belirteçleriyle korunan dosyaları veya çalışma alanının yönetilen kimliğini doğrudan sorgulamak için OPENROWSET 'yi de kullanabilir. Bir SQL kullanıcısı bu işlevi çalıştırırsa, izne sahip bir Power User, `ALTER ANY CREDENTIAL` IŞLEVDEKI URL ile eşleşen sunucu kapsamlı bir kimlik bilgisi oluşturmalı (depolama adı ve kapsayıcısı kullanılarak) ve bu kimlik bilgisi IÇIN OPENROWSET işlevinin ÇAĞıRANıNA başvurular izni verilir:

```sql
EXECUTE AS somepoweruser

CREATE CREDENTIAL [https://<storage_account>.dfs.core.windows.net/<container>]
 WITH IDENTITY = 'SHARED ACCESS SIGNATURE', SECRET = 'sas token';

GRANT REFERENCES CREDENTIAL::[https://<storage_account>.dfs.core.windows.net/<container>] TO sqluser
```

URL ile eşleşen sunucu düzeyi KIMLIK BILGISI yoksa veya SQL kullanıcısı bu kimlik bilgisi için başvuru iznine sahip değilse, hata döndürülür. SQL sorumluları, bazı Azure AD kimliklerini kullanarak kimliğine bürünebilir.

> [!NOTE]
> OPENROWSET 'in bu sürümü, varsayılan kimlik doğrulaması kullanılarak hızlı ve kolay veri araştırması için tasarlanmıştır. Kimliğe bürünme veya yönetilen kimlik özelliğinden yararlanmak için, sonraki bölümde açıklanan VERI kaynağı ile OPENROWSET kullanın.

### <a name="query-data-sources-using-openrowset"></a>OPENROWSET kullanarak veri kaynaklarını sorgulama

OPENROWSET, kullanıcının bazı dış veri kaynaklarına yerleştirilmiş dosyaları sorgulamasını sağlar:

```sql
SELECT * FROM
 OPENROWSET(BULK 'file/path/*.parquet',
 DATASOURCE = MyAzureInvoices,
 FORMAT= 'parquet') as rows
```

DENETIM VERITABANı iznine sahip Power User, veri kaynağının URL 'sini ve kullanılması gereken kimlik bilgisini belirten, depolama ve dış VERI KAYNAĞıNA erişmek için kullanılacak VERITABANı KAPSAMLı KIMLIK BILGILERI oluşturmak zorunda olacaktır:

```sql
CREATE DATABASE SCOPED CREDENTIAL AccessAzureInvoices
 WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
 SECRET = '******srt=sco&amp;sp=rwac&amp;se=2017-02-01T00:55:34Z&amp;st=201********' ;

CREATE EXTERNAL DATA SOURCE MyAzureInvoices
 WITH ( LOCATION = 'https://<storage_account>.dfs.core.windows.net/<container>/<path>/' ,
 CREDENTIAL = AccessAzureInvoices) ;
```

VERITABANı KAPSAMLı KIMLIK BILGILERI, başvurulan veri kaynağındaki dosyalara nasıl erişmekte olduğunu belirtir (Şu anda SAS ve yönetilen kimlik).

Çağıran, OPENROWSET işlevini yürütmek için aşağıdaki izinlerden birine sahip olmalıdır:

- OPENROWSET yürütme izinlerinden biri:
  - `ADMINISTER BULK OPERATIONS`, OPENROWSET işlevini yürütmek için oturum açma sağlar.
  - `ADMINISTER DATABASE BULK OPERATIONS`Veritabanı kapsamlı kullanıcının OPENROWSET işlevini yürütmesine olanak sağlar.
- Dış VERI KAYNAĞıNDA başvurulan kimlik bilgileri için VERITABANı KAPSAMLı KIMLIK bılgısıne başvurur

#### <a name="access-anonymous-data-sources"></a>Anonim veri kaynaklarına erişin

Kullanıcı, ortak erişim depolamasına başvuracak veya Azure AD PASSTHROUGH kimlik doğrulamasını kullanacak KIMLIK BILGISI olmadan dış VERI kaynağı oluşturabilir:

```sql
CREATE EXTERNAL DATA SOURCE MyAzureInvoices
 WITH ( LOCATION = 'https://<storage_account>.dfs.core.windows.net/<container>/<path>') ;
```

## <a name="external-table"></a>DıŞ TABLO

Tablo okuma izinlerine sahip olan Kullanıcı, Azure depolama klasörleri ve dosyaları kümesinin üzerine oluşturulmuş bir dış tablo kullanarak dış dosyalara erişebilir.

[Dış tablo oluşturma izinlerine](https://docs.microsoft.com/sql/t-sql/statements/create-external-table-transact-sql?view=sql-server-ver15#permissions) sahip olan Kullanıcı (örneğin Create Table ve HERHANGI BIR KIMLIK bilgisini DEĞIŞTIRME veya VERITABANı KAPSAMLı KIMLIK bilgilerine başvuru), Azure depolama veri kaynağının en üstünde bir tablo oluşturmak için aşağıdaki betiği kullanabilir:

```sql
CREATE EXTERNAL TABLE [dbo].[DimProductexternal]
( ProductKey int, ProductLabel nvarchar, ProductName nvarchar )
WITH
(
LOCATION='/DimProduct/year=*/month=*' ,
DATA_SOURCE = AzureDataLakeStore ,
FILE_FORMAT = TextFileFormat
) ;
```

DENETIM VERITABANı iznine sahip olan Kullanıcı, veri kaynağının URL 'sini ve kullanılması gereken kimlik bilgisini belirten, depolama ve dış VERI KAYNAĞıNA erişmek için kullanılacak VERITABANı KAPSAMLı KIMLIK BILGILERI oluşturmak zorunda olacaktır:

```sql
CREATE DATABASE SCOPED CREDENTIAL cred
 WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
 SECRET = '******srt=sco&sp=rwac&se=2017-02-01T00:55:34Z&st=201********' ;

CREATE EXTERNAL DATA SOURCE AzureDataLakeStore
 WITH ( LOCATION = 'https://<storage_account>.dfs.core.windows.net/<container>/<path>' ,
 CREDENTIAL = cred
 ) ;
```

VERITABANı KAPSAMLı KIMLIK BILGILERI başvurulan veri kaynağındaki dosyalara nasıl erişekullanacağınızı belirtir.

### <a name="read-external-files-with-external-table"></a>Dış tablo içeren dış dosyaları okuma

DıŞ tablo, standart SQL SELECT ifadesini kullanarak veri kaynağı aracılığıyla başvurulan dosyalardaki verileri okumanızı sağlar:

```sql
SELECT *
FROM dbo.DimProductsExternal
```

Çağıran, verileri okumak için aşağıdaki izinlere sahip olmalıdır:
- `SELECT`Dış tablodaki izin
- `REFERENCES DATABASE SCOPED CREDENTIAL``DATA SOURCE`varsa izin`CREDENTIAL`

## <a name="permissions"></a>İzinler

Aşağıdaki tabloda, yukarıda listelenen işlemler için gerekli izinler listelenmektedir.

| Sorgu | Gerekli izinler|
| --- | --- |
| Veri kaynağı olmadan OPENROWSET (toplu) | `ADMINISTER BULK OPERATIONS`, `ADMINISTER DATABASE BULK OPERATIONS` , veya SQL oturum açma, \<URL> SAS korumalı depolama IÇIN kimlik bilgilerine sahip olmalıdır: |
| Kimlik bilgisi olmadan veri kaynağıyla OPENROWSET (toplu) | `ADMINISTER BULK OPERATIONS`veya `ADMINISTER DATABASE BULK OPERATIONS` , |
| Kimlik bilgileriyle veri kaynağıyla OPENROWSET (toplu) | `REFERENCES DATABASE SCOPED CREDENTIAL`ve `ADMINISTER BULK OPERATIONS` bunlardan biri`ADMINISTER DATABASE BULK OPERATIONS` |
| DıŞ VERI KAYNAĞı OLUŞTUR | `ALTER ANY EXTERNAL DATA SOURCE` ve `REFERENCES DATABASE SCOPED CREDENTIAL` |
| DıŞ TABLO OLUŞTUR | `CREATE TABLE`, `ALTER ANY SCHEMA` , `ALTER ANY EXTERNAL FILE FORMAT` ve`ALTER ANY EXTERNAL DATA SOURCE` |
| DıŞ TABLODAN SEÇIM YAPıN | `SELECT TABLE` ve `REFERENCES DATABASE SCOPED CREDENTIAL` |
| CETAS | Tablo oluşturmak için- `CREATE TABLE` , `ALTER ANY SCHEMA` , `ALTER ANY DATA SOURCE` , ve `ALTER ANY EXTERNAL FILE FORMAT` . Verileri okumak için: `ADMINISTER BULK OPERATIONS` `REFERENCES CREDENTIAL` `SELECT TABLE` Depolama üzerindeki her tablo/görünüm/işlev/sorgu + R/W iznine sahip |

## <a name="next-steps"></a>Sonraki adımlar

Artık şu nasıl yapılır makaleleriyle devam etmeye hazırsınız:

- [Depolamadaki verileri sorgulama](query-data-storage.md)

- [CSV dosyasını sorgulama](query-single-csv-file.md)

- [Klasörleri ve birden çok dosyayı sorgulama](query-folders-multiple-csv-files.md)

- [Belirli dosyaları sorgula](query-specific-files.md)

- [Parquet dosyalarını sorgulama](query-parquet-files.md)

- [İç içe türler sorgula](query-parquet-nested-types.md)

- [JSON dosyalarını sorgulama](query-json-files.md)

- [Görünümleri oluşturma ve kullanma](create-use-views.md)
