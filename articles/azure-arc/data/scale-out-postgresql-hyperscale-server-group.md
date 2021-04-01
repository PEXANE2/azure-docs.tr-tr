---
title: PostgreSQL için Azure veritabanı hiper ölçek sunucu grubu ölçeğini genişletme
description: PostgreSQL için Azure veritabanı hiper ölçek sunucu grubu ölçeğini genişletme
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 17bdae658c7095c44a7ae9f30fd85a6c45bf1546
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "96780000"
---
# <a name="scale-out-your-azure-arc-enabled-postgresql-hyperscale-server-group-by-adding-more-worker-nodes"></a>Daha fazla çalışan düğümü ekleyerek Azure Arc etkin PostgreSQL hiper ölçek sunucu grubunu ölçeklendirin
Bu belgede, Azure Arc etkin bir PostgreSQL hiper ölçek sunucu grubunun ölçeğini daraltma açıklanmaktadır. Bunu bir senaryoya göre gerçekleştirerek yapar. **Senaryo aracılığıyla çalıştırmak istemiyor ve nasıl ölçeklendirilen hakkında yalnızca okumak istiyorsanız, paragraf [ölçeği dışına](#scale-out)atlayın**.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="get-started"></a>başlarken
Azure Arc 'ın ölçeklendirme modeli olan PostgreSQL hiper ölçeğini veya PostgreSQL için Azure veritabanı hiper ölçeğini (Citus) zaten biliyorsanız, bu paragrafı atlayabilirsiniz. Bu işlemi yapmadıysanız, PostgreSQL için Azure veritabanı hiper ölçek (Citus) belge sayfasında Bu ölçeklendirme modeli hakkında bilgi okuyarak başlatmanız önerilir. PostgreSQL için Azure veritabanı hiper ölçek (Citus), Azure Arc etkin veri Hizmetleri 'nin bir parçası olarak sunulmadan Azure 'da bir hizmet olarak barındırılan (hizmet olarak platform da PAAS olarak da bilinir) aynı teknolojiden oluşur:
- [Düğümler ve tablolar](../../postgresql/concepts-hyperscale-nodes.md)
- [Uygulama türünü belirleme](../../postgresql/concepts-hyperscale-app-type.md)
- [Dağıtım sütunu seçme](../../postgresql/concepts-hyperscale-choose-distribution-column.md)
- [Tablo birlikte bulundurma](../../postgresql/concepts-hyperscale-colocation.md)
- [Tabloları dağıtma ve değiştirme](../../postgresql/howto-hyperscale-modify-distributed-tables.md)
- [Çok kiracılı bir veritabanı tasarlama](../../postgresql/tutorial-design-database-hyperscale-multi-tenant.md)*
- [Gerçek zamanlı analiz panosu tasarlama](../../postgresql/tutorial-design-database-hyperscale-realtime.md)*

> \* Yukarıdaki belgelerde, **Azure Portal oturum açma** bölümünü atlayın, & **PostgreSQL Için Azure veritabanı oluşturun-hiper ölçek (Citus)**. Azure Arc dağıtımınızda kalan adımları uygulayın. Bu bölümler, Azure bulutunda PaaS hizmeti olarak sunulan PostgreSQL için Azure veritabanı 'na (Citus) özgüdür, ancak belgelerin diğer kısımları doğrudan Azure Arc etkin PostgreSQL hiper ölçeklendirmenize uygulanabilir.

## <a name="scenario"></a>Senaryo
Bu senaryo, [Azure Arc etkin PostgreSQL hiper ölçek sunucu grubu oluşturma](create-postgresql-hyperscale-server-group.md) belgelerinde örnek olarak oluşturulan PostgreSQL hiper ölçek sunucu grubuna başvurur.

### <a name="load-test-data"></a>Test verilerini yükleme
Senaryo, [Citus Data Web sitesinden](https://www.citusdata.com/) (Citus verileri Microsoft 'un bir parçası) erişilebilen, genel kullanıma açık GitHub verilerinin bir örneğini kullanır.

#### <a name="connect-to-your-azure-arc-enabled-postgresql-hyperscale-server-group"></a>Azure Arc etkin PostgreSQL hiper ölçek sunucu grubuna bağlanma

##### <a name="list-the-connection-information"></a>Bağlantı bilgilerini listeleyin
Önce bağlantı bilgilerini alarak Azure Arc etkin PostgreSQL hiper ölçek sunucu grubuna bağlanın: Bu komutun genel biçimi
```console
azdata arc postgres endpoint list -n <server name>
```
Örnek:
```console
azdata arc postgres endpoint list -n postgres01
```

Örnek çıktı:

```console
[
  {
    "Description": "PostgreSQL Instance",
    "Endpoint": "postgresql://postgres:<replace with password>@12.345.123.456:1234"
  },
  {
    "Description": "Log Search Dashboard",
    "Endpoint": "https://12.345.123.456:12345/kibana/app/kibana#/discover?_a=(query:(language:kuery,query:'custom_resource_name:\"postgres01\"'))"
  },
  {
    "Description": "Metrics Dashboard",
    "Endpoint": "https://12.345.123.456:12345/grafana/d/postgres-metrics?var-Namespace=arc3&var-Name=postgres01"
  }
]
```

##### <a name="connect-with-the-client-tool-of-your-choice"></a>Tercih ettiğiniz istemci aracıyla bağlantı yapın.

Şu anda bir Kubernetes Pod öğesine karşılık gelen iki (veya daha fazla hiper ölçek çalışan düğümüne) sahip olduğunuzu doğrulamak için aşağıdaki sorguyu çalıştırın:

```sql
SELECT * FROM pg_dist_node;
```

```console
 nodeid | groupid |                       nodename                        | nodeport | noderack | hasmetadata | isactive | noderole | nodecluster | metadatasynced | shouldhaveshards
--------+---------+-------------------------------------------------------+----------+----------+-------------+----------+----------+-------------+----------------+------------------
      1 |       1 | pg1-1.pg1-svc.default.svc.cluster.local |     5432 | default  | f           | t        | primary  | default     | f              | t
      2 |       2 | pg1-2.pg1-svc.default.svc.cluster.local |     5432 | default  | f           | t        | primary  | default     | f              | t
(2 rows)
```

#### <a name="create-a-sample-schema"></a>Örnek şema oluşturma
Aşağıdaki sorguyu çalıştırarak iki tablo oluşturun:

```sql
CREATE TABLE github_events
(
    event_id bigint,
    event_type text,
    event_public boolean,
    repo_id bigint,
    payload jsonb,
    repo jsonb,
    user_id bigint,
    org jsonb,
    created_at timestamp
);

CREATE TABLE github_users
(
    user_id bigint,
    url text,
    login text,
    avatar_url text,
    gravatar_id text,
    display_login text
);
```

JSONB, PostgreSQL içindeki ikili biçimdeki JSON veri türüdür. Esnek bir şemayı tek bir sütunda ve PostgreSQL ile depolar. Şemanın içindeki her anahtar ve değeri indekslemek için bu şema üzerinde bir GıN dizini olacaktır. Bir GIN diziniyle, bu yük üzerinde doğrudan çeşitli koşullarla sorgu yapmak hızlı ve kolay hale gelir. Bu nedenle, verilerimizi yüklemeden önce birkaç dizin oluşturacağız.

```sql
CREATE INDEX event_type_index ON github_events (event_type);
CREATE INDEX payload_index ON github_events USING GIN (payload jsonb_path_ops);
```

Standart tabloları, her tablo için bir sorgu çalıştırın. Parça yapmak istediğimiz tabloyu ve onu parçalara eklemek istediğimiz anahtarı belirtin. User_id hem olayları hem de Kullanıcı tablosunu inceleyeceğiz:

```sql
SELECT create_distributed_table('github_events', 'user_id');
SELECT create_distributed_table('github_users', 'user_id');
```

#### <a name="load-sample-data"></a>Örnek verileri yükleme
Verileri kopyalama ile yükle... PROGRAMDAN:

```sql
COPY github_users FROM PROGRAM 'curl "https://examples.citusdata.com/users.csv"' WITH ( FORMAT CSV );
COPY github_events FROM PROGRAM 'curl "https://examples.citusdata.com/events.csv"' WITH ( FORMAT CSV );
```

#### <a name="query-the-data"></a>Verileri sorgulama
Ve artık basit bir sorgunun iki düğüm ile ne kadar süreyle sürdüğünü ölçer:

```sql
SELECT COUNT(*) FROM github_events;
```
Sorgu yürütme süresini bir yere getirin.


## <a name="scale-out"></a>Ölçeği genişletme
Genişleme komutunun genel biçimi:
```console
azdata arc postgres server edit -n <server group name> -w <target number of worker nodes>
```

> [!CAUTION]
> Önizleme sürümü ölçeği geri almayı desteklemez. Örneğin çalışan düğümlerinin sayısını azaltmak henüz mümkün değildir. Bunu yapmanız gerekiyorsa, verileri ayıklamanız/yedeklemeniz, sunucu grubunu bırakmanız, daha az sayıda çalışan düğümü olan yeni bir sunucu grubu oluşturmanız ve verileri içeri aktarmanız gerekir.

Bu örnekte, aşağıdaki komutu çalıştırarak çalışan düğümü sayısını 2 ' den 4 ' e artırdık:

```console
azdata arc postgres server edit -n postgres01 -w 4
```

Düğüm ekleme üzerine, sunucu grubu için bekleyen bir durum görürsünüz. Örnek:
```console
azdata arc postgres server list
```

```console
Name        State          Workers
----------  -------------  ---------
postgres01  Pending 4/5    4
```

Düğümler kullanılabilir olduktan sonra hiper ölçek parça yeniden dengeleyici otomatik olarak çalışır ve verileri yeni düğümlere yeniden dağıtır. Genişleme işlemi, çevrimiçi bir işlemdir. Düğümler eklendikçe ve veriler düğümler arasında yeniden dağıtılarak, veriler sorgular için kullanılabilir kalır.

### <a name="verify-the-new-shape-of-the-server-group-optional"></a>Sunucu grubunun yeni şeklini doğrulayın (isteğe bağlı)
Sunucu grubunun artık eklediğiniz ek çalışan düğümlerini kullandığını doğrulamak için aşağıdaki yöntemlerden birini kullanın.

#### <a name="with-azdata"></a>Azdata ile:
Şu komutu çalıştırın:
```console
azdata arc postgres server list
```

Ad alanınız içinde oluşturulan sunucu gruplarının listesini döndürür ve bunların çalışan düğüm sayısını belirtir. Örnek:
```console
Name        State    Workers
----------  -------  ---------
postgres01  Ready    4
```

#### <a name="with-kubectl"></a>Kubectl ile:
Şu komutu çalıştırın:
```console
kubectl get postgresql-12
```

Ad alanınız içinde oluşturulan sunucu gruplarının listesini döndürür ve bunların çalışan düğüm sayısını belirtir. Örnek:
```console
NAME         STATE   READY-PODS   EXTERNAL-ENDPOINT   AGE
postgres01   Ready   4/4          10.0.0.4:31066      4d20h
```
> [!NOTE]
> 12 yerine sürüm 11 PostgreSQL sunucu grubu oluşturduysanız, bunun yerine şu komutu çalıştırın: _kubectl Get PostgreSQL-11_

#### <a name="with-a-sql-query"></a>Bir SQL sorgusu ile:
İstediğiniz istemci aracıyla sunucu grubunuza bağlanın ve aşağıdaki sorguyu çalıştırın:

```sql
SELECT * FROM pg_dist_node;
```

```console
 nodeid | groupid |                       nodename                        | nodeport | noderack | hasmetadata | isactive | noderole | nodecluster | metadatasynced | shouldhaveshards
--------+---------+-------------------------------------------------------+----------+----------+-------------+----------+----------+-------------+----------------+------------------
      1 |       1 | pg1-1.pg1-svc.default.svc.cluster.local |     5432 | default  | f           | t        | primary  | default     | f              | t
      2 |       2 | pg1-2.pg1-svc.default.svc.cluster.local |     5432 | default  | f           | t        | primary  | default     | f              | t
      3 |       3 | pg1-3.pg1-svc.default.svc.cluster.local |     5432 | default  | f           | t        | primary  | default     | f              | t
      4 |       4 | pg1-4.pg1-svc.default.svc.cluster.local |     5432 | default  | f           | t        | primary  | default     | f              | t
(4 rows)
```

## <a name="return-to-the-scenario"></a>Senaryoya geri dön

Select count sorgusunun yürütme zamanını örnek veri kümesine göre karşılaştırmak isterseniz, aynı sayı sorgusunu kullanın. Bu, SQL deyimindeki herhangi bir değişiklik yapılmadan dört çalışan düğümü boyunca kullanılabilir.

```sql
SELECT COUNT(*) FROM github_events;
```
Yürütme süresini aklınızda edin.


> [!NOTE]
> Ortamınıza bağlı olarak-örneğin, test sunucusu grubunuzu `kubeadm` tek düğümlü bir VM üzerinde dağıttıysanız, yürütme zamanında bir ila büyüklükteki geliştirmesi görebilirsiniz. Azure Arc etkin PostgreSQL hiper Ölçeklendirmesiyle alabileceğiniz performans iyileştirmesinin türü hakkında daha iyi fikir edinmek için aşağıdaki kısa videoları izleyin:
>* [Azure PostgreSQL Hyperscale ile yüksek performanslı HTAP (Citus)](https://www.youtube.com/watch?v=W_3e07nGFxY)
>* [Azure PostgreSQL hiper ölçek & Python ile HTAP uygulamaları oluşturma (Citus)](https://www.youtube.com/watch?v=YDT8_riLLs0)


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Arc etkin PostgreSQL hiper ölçek sunucu grubu 'nun ölçeğini artırma ve azaltma (bellek, sanal çekirdek)](scale-up-down-postgresql-hyperscale-server-group-using-cli.md) hakkında bilgi edinin
- Azure Arc etkin PostgreSQL hiper ölçek sunucu grubunda sunucu parametreleri ayarlama hakkında bilgi edinin
- PostgreSQL için Azure veritabanı 'nın, verileri birden çok PostgreSQL hiper ölçek düğümüne dağıtmak ve Postgres Hyperscale için Azure veritabanı 'nın tüm gücünden yararlanmak için Azure veritabanı 'nın kavramlarını ve nasıl yapılır kılavuzlarını okuyun. :
    * [Düğümler ve tablolar](../../postgresql/concepts-hyperscale-nodes.md)
    * [Uygulama türünü belirleme](../../postgresql/concepts-hyperscale-app-type.md)
    * [Dağıtım sütunu seçme](../../postgresql/concepts-hyperscale-choose-distribution-column.md)
    * [Tablo birlikte bulundurma](../../postgresql/concepts-hyperscale-colocation.md)
    * [Tabloları dağıtma ve değiştirme](../../postgresql/howto-hyperscale-modify-distributed-tables.md)
    * [Çok kiracılı bir veritabanı tasarlama](../../postgresql/tutorial-design-database-hyperscale-multi-tenant.md)*
    * [Gerçek zamanlı analiz panosu tasarlama](../../postgresql/tutorial-design-database-hyperscale-realtime.md)*

 > \* Yukarıdaki belgelerde, **Azure Portal oturum açma** bölümünü atlayın, & **PostgreSQL Için Azure veritabanı oluşturun-hiper ölçek (Citus)**. Azure Arc dağıtımınızda kalan adımları uygulayın. Bu bölümler, Azure bulutunda PaaS hizmeti olarak sunulan PostgreSQL için Azure veritabanı 'na (Citus) özgüdür, ancak belgelerin diğer kısımları doğrudan Azure Arc etkin PostgreSQL hiper ölçeklendirmenize uygulanabilir.

- [Depolama yapılandırması ve Kubernetes depolama kavramları](storage-configuration.md)
- [Kubernetes kaynak modeli](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/scheduling/resources.md#resource-quantities)
