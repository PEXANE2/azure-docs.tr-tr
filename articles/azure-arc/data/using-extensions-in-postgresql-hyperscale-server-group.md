---
title: PostgreSQL uzantılarını kullanma
description: PostgreSQL uzantılarını kullanma
titleSuffix: Azure Arc enabled data services
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 3b9c3c66e58ae51773a959aba0b2c76d97b44445
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92309515"
---
# <a name="use-postgresql-extensions-in-your-azure-arc-enabled-postgresql-hyperscale-server-group"></a>Azure Arc etkin PostgreSQL hiper ölçek sunucu grubunda PostgreSQL uzantılarını kullanma

PostgreSQL, uzantıları ile kullandığınızda en iyi seçenektir. Aslında, kendi Hyperscale işlevsellikten oluşan bir anahtar öğesi, `citus` Varsayılan olarak yüklenen Microsoft tarafından sunulan uzantıdır. Bu, Postgres 'in verileri birden çok düğüm arasında saydam bir şekilde parçalamayı sağlar.


[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="list-of-extensions"></a>Uzantı listesi
İçindeki uzantıların yanı sıra [`contrib`](https://www.postgresql.org/docs/12/contrib.html) , Azure Arc etkin PostgreSQL hiper ölçek sunucu grubunun kapsayıcılarında bulunan uzantıların listesi şunlardır:
- `citus`, v: 9,4
- `pg_cron`, v: 1,2
- `plpgsql`, v: 1,0
- `postgis`, v: 3.0.2
- `plv8`, v: 2.3.14

Bu liste gelişir ve bu belgede güncelleştirmeler gönderilir. Yukarıda listelenenlerin ötesinde uzantı eklemeniz henüz mümkün değildir.

Bu kılavuz, bu uzantıların ikisini kullanmak için bir senaryo alır:
- [PostGIS](https://postgis.net/)
- [`pg_cron`](https://github.com/citusdata/pg_cron)


## <a name="manage-extensions"></a>Uzantıları yönetme

### <a name="enable-extensions"></a>Uzantıları etkinleştir
Bu adım, parçası olan uzantılar için gerekli değildir `contrib` .
Uzantıları etkinleştirmek için komutun genel biçimi:

#### <a name="enable-an-extension-at-the-creation-time-of-a-server-group"></a>Bir uzantıyı sunucu grubunun oluşturulma zamanında etkinleştirin:
```console
azdata arc postgres server create -n <name of your postgresql server group> --extensions <extension names>
```
#### <a name="enable-an-extension-on-an-instance-that-already-exists"></a>Zaten var olan bir örnek üzerinde uzantı etkinleştirin:
```console
azdata arc postgres server edit -n <name of your postgresql server group> --extensions <extension names>
```

#### <a name="get-the-list-of-extensions-enabled"></a>Etkinleştirilen uzantıların listesini al:
Aşağıdaki komuttan birini çalıştırın.

##### <a name="with-azure-data-cli-azdata"></a>Kullanılarak [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)]
```console
azdata arc postgres server show -n <server group name>
```
Çıktıyı kaydırın ve sunucu grubunuzun belirtimlerindeki engine\extensions bölümlerine dikkat edin. Örneğin:
```console
"engine": {
      "extensions": [
        {
          "name": "citus"
        },
        {
          "name": "pg_cron"
        }
      ]
    },
```
##### <a name="with-kubectl"></a>Kubectl ile
```console
kubectl describe postgresql-12s/postgres02
```
Çıktıyı kaydırın ve sunucu grubunuzun belirtimlerindeki engine\extensions bölümlerine dikkat edin. Örneğin:
```console
Engine:
    Extensions:
      Name:  citus
      Name:  pg_cron
```


### <a name="create-extensions"></a>Uzantı Oluştur:
İstediğiniz istemci aracıyla sunucu grubunuza bağlanın ve standart PostgreSQL sorgusunu çalıştırın:
```console
CREATE EXTENSION <extension name>;
```

### <a name="get-the-list-of-extension-created-in-your-server-group"></a>Sunucu grubunuzda oluşturulan uzantının listesini alın:
İstediğiniz istemci aracıyla sunucu grubunuza bağlanın ve standart PostgreSQL sorgusunu çalıştırın:
```console
select * from pg_extension;
```

### <a name="drop-an-extension-from-your-server-group"></a>Sunucu grubunuza bir uzantı bırakın:
İstediğiniz istemci aracıyla sunucu grubunuza bağlanın ve standart PostgreSQL sorgusunu çalıştırın:
```console
drop extension <extension name>;
```

## <a name="use-the-postgis-and-the-pg_cron-extensions"></a>PostGIS ve Pg_cron uzantılarını kullanma

### <a name="the-postgis-extension"></a>PostGIS uzantısı

Var olan bir sunucu grubunda PostGIS uzantısını etkinleştirebiliriz veya zaten etkinleştirilmiş bir uzantıya sahip yeni bir tane oluşturabilirsiniz:

**Bir sunucu grubunun oluşturulma zamanında bir uzantıyı etkinleştirme:**
```console
azdata arc postgres server create -n <name of your postgresql server group> --extensions <extension names>

#Example:
azdata arc postgres server create -n pg2 -w 2 --extensions postgis
```

**Zaten var olan bir örnek üzerinde uzantı Etkinleştiriliyor:**
```console
azdata arc postgres server edit -n <name of your postgresql server group> --extensions <extension names>

#Example:
azdata arc postgres server edit --extensions postgis -n pg2
```

Hangi uzantıların yüklendiğini doğrulamak için, aşağıdaki standart PostgreSQL komutunu, Azure Data Studio gibi en sevdiğiniz PostgreSQL istemci araclarınızla birlikte bağladıktan sonra kullanın:
```console
select * from pg_extension;
```

İlk olarak bir PostGIS örneği için, planlama & şehir departmanından alınan [örnek verileri](http://duspviz.mit.edu/tutorials/intro-postgis/) alın. `apt-get install unzip`Test için sanal makineyi kullanırken unzip 'i yüklemek için çalıştırmanız gerekebilir.

```console
wget http://duspviz.mit.edu/_assets/data/intro-postgis-datasets.zip
unzip intro-postgis-datasets.zip
```

Veritabanımıza bağlanalım ve PostGIS uzantısını oluşturalım:

```console
CREATE EXTENSION postgis;
```

> [!NOTE]
> Paketteki uzantılardan birini (örneğin,,,...) kullanmak isterseniz, `postgis` `postgis_raster` `postgis_topology` `postgis_sfcgal` `fuzzystrmatch` önce PostGIS uzantısını oluşturmanız ve ardından diğer uzantıyı oluşturmanız gerekir. Örneğin: `CREATE EXTENSION postgis` ; `CREATE EXTENSION postgis_raster` ;

Ve şemayı oluşturun:

```sql
CREATE TABLE coffee_shops (
  id serial NOT NULL,
  name character varying(50),
  address character varying(50),
  city character varying(50),
  state character varying(50),
  zip character varying(10),
  lat numeric,
  lon numeric,
  geom geometry(POINT,4326)
);
CREATE INDEX coffee_shops_gist ON coffee_shops USING gist (geom);
```

Şimdi, coffee_shops tablosunu dağıtılmış hale getirerek PostGIS 'yi ölçek genişletme işleviyle birleştirebiliriz:

```sql
SELECT create_distributed_table('coffee_shops', 'id');
```

Bazı verileri yükleyelim:

```console
\copy coffee_shops(id,name,address,city,state,zip,lat,lon) from cambridge_coffee_shops.csv CSV HEADER;
```

Ve `geom` PostGIS veri türünde alanı doğru kodlanmış Enlem ve Boylam ile doldurabilirsiniz `geometry` :

```sql
UPDATE coffee_shops SET geom = ST_SetSRID(ST_MakePoint(lon,lat),4326);
```

Şimdi, MıT 'e en yakın kafeterleri listelebiliriz (77 Massachusetts Ave, 42,359055,-71,093500):

```sql
SELECT name, address FROM coffee_shops ORDER BY geom <-> ST_SetSRID(ST_MakePoint(-71.093500,42.359055),4326);
```


### <a name="the-pg_cron-extension"></a>Pg_cron uzantısı

`pg_cron`PostGIS 'nin yanı sıra PostgreSQL sunucu grubumuzu etkinleştirelim:

```console
azdata postgres server update -n pg2 -ns arc --extensions postgis,pg_cron
```

Bu işlem, düğümleri yeniden başlatıp ek uzantıları yüklediğine yönelik olarak 2-3 dakika sürebilir.

Şimdi yeniden bağlanabiliyoruz ve uzantıyı oluşturuyoruz `pg_cron` :

```sql
CREATE EXTENSION pg_cron;
```

Test amacıyla, `the_best_coffee_shop` önceki tablomızdan rastgele bir ad alan bir tablo oluşturur `coffee_shops` ve tablo içeriğini ayarlar:

```sql
CREATE TABLE the_best_coffee_shop(name text);
```

`cron.schedule`Rastgele bir tablo adı almak için, daha az SAYıDA SQL deyimi kullanabilir (dağıtılmış bir sorgu sonucunu depolamak için geçici bir tablo kullanımına dikkat edin) ve şu şekilde saklayın `the_best_coffee_shop` :

```sql
SELECT cron.schedule('* * * * *', $$
  TRUNCATE the_best_coffee_shop;
  CREATE TEMPORARY TABLE tmp AS SELECT name FROM coffee_shops ORDER BY random() LIMIT 1;
  INSERT INTO the_best_coffee_shop SELECT * FROM tmp;
  DROP TABLE tmp;
$$);
```

Şimdi de bir dakika sonra farklı bir ad alacaksınız:

```sql
SELECT * FROM the_best_coffee_shop;
```

```console
      name
-----------------
 B & B Snack Bar
(1 row)
```

Sözdizimi hakkında tam Ayrıntılar için [pg_cron Benioku dosyasına](https://github.com/citusdata/pg_cron) bakın.

>[!NOTE]
>Uzantının düşürülme işlemi desteklenmez `citus` . `citus`Hiper ölçek deneyimi sağlamak için uzantı gereklidir.

## <a name="next-steps"></a>Sonraki adımlar:
- [Plv8](https://plv8.github.io/) hakkındaki belgeleri okuyun
- [PostGIS](https://postgis.net/) 'de belgeleri okuyun
- Belgeleri okuyun [`pg_cron`](https://github.com/citusdata/pg_cron)
