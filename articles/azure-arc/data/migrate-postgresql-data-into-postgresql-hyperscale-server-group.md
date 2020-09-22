---
title: Bir PostgreSQL veritabanından Azure Arc etkin PostgreSQL hiper ölçek sunucu grubuna veri geçirme
titleSuffix: Azure Arc enabled database services
description: Bir PostgreSQL veritabanından Azure Arc etkin PostgreSQL hiper ölçek sunucu grubuna veri geçirme
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 521fd61f18d6673e21c23dbca4cfc12d2ee4bf0b
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941763"
---
# <a name="migrate-postgresql-database-to-azure-arc-enabled-postgresql-hyperscale-server-group"></a>PostgreSQL veritabanını Azure Arc etkin PostgreSQL hiper ölçek sunucu grubuna geçirme

Bu belgede, mevcut PostgreSQL veritabanınızı (Azure Arc etkin veri hizmetlerinde barındırılmayan bir tane) Azure Arc etkin PostgreSQL hiper ölçek sunucu grubuna almak için gereken adımlar açıklanmaktadır.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Azure Arc etkin PostgreSQL hiper ölçek sunucu grubu, PostgreSQL 'in topluluk sürümüdür ve CitusData uzantısı etkinleştirilmiş olarak çalışır. Bu nedenle, Azure Arc dışında PostgreSQL üzerinde çalışan tüm araçlar, Azure Arc etkin PostgreSQL hiper ölçek sunucu grubu ile çalışmalıdır.


Bu nedenle, Postgres için bugün kullandığınız araçlar kümesiyle şunları yapabilmelisiniz:
1. Azure Arc dışında barındırılan örneğinizin Postgres veritabanınızı yedekleyin
2. Azure Arc etkin PostgreSQL hiper ölçek sunucu grubuna geri yükleme

Yapmanız gerekenler:
- sunucu parametrelerini sıfırlayın
- güvenlik bağlamlarını sıfırlayın: kullanıcıları, rolleri ve sıfırlama izinlerini yeniden oluşturun...

Bu yedekleme/geri yükleme işlemini yapmak için, Postgres için yedekleme/geri yükleme yapabilen herhangi bir aracı kullanabilirsiniz. Örnek:
- Azure Data Studio ve Postgres uzantısı
- `pgcli`
- `pgAdmin`
- `pg_dump`
- `pg_restore`
- `psql`
- ...

## <a name="example"></a>Örnek
Aracı kullanarak bu adımları gösterelim `pgAdmin` .
Aşağıdaki kurulumu göz önünde bulundurun:
- **Kaynaktaki**  
    Çıplak sunucu üzerinde şirket içinde çalışan ve JEANYDSRV adlı bir Postgres sunucusu. 12. sürüme sahiptir ve 1 satıra :::image type="content" source="media/postgres-hyperscale/migrate-pg-source.jpg" alt-text="geçiş kaynağı"::: olan MyOnPremPostgresDB adlı bir veritabanı barındırır

- **Hedefine**  
    Azure Arc ortamında çalışan ve postgres01 adlı bir Postgres sunucusu. 12. sürümdür. Standart Postgres veritabanı dışında hiçbir veritabanı yoktur.  
    :::image type="content" source="media/postgres-hyperscale/migrate-pg-destination.jpg" alt-text="Geçiş hedefi":::


### <a name="take-a-backup-of-the-source-database-on-premises"></a>Kaynak veritabanının bir yedeğini şirket içinde al

:::image type="content" source="media/postgres-hyperscale/Migrate-PG-Source-Backup.jpg" alt-text="Geçiş-kaynak-yedekleme":::

Yapılandırın:
1. Bu dosyaya bir dosya adı verin: **Mysourcebackup**
2. Biçimi **özel** 
 :::image type="content" source="media/postgres-hyperscale/Migrate-PG-Source-Backup2.jpg" alt-text="geçiş-kaynak-yedekleme-yapılandırma"::: olarak ayarlayın

Yedekleme başarıyla tamamlandı:  
:::image type="content" source="media/postgres-hyperscale/Migrate-PG-Source-Backup3.jpg" alt-text="Geçiş-kaynak-Yedekleme-tamamlandı":::

### <a name="create-an-empty-database-on-the-destination-system-in-your-azure-arc-enabled-postgresql-hyperscale-server-group"></a>Azure Arc etkin PostgreSQL hiper ölçek sunucu grubundaki hedef sistemde boş bir veritabanı oluşturma

> [!NOTE]
> Araca bir Postgres örneği kaydetmek için `pgAdmin` , Kubernetes kümenizde örneğinizin genel IP 'sini kullanmanız ve bağlantı noktasını ve güvenlik bağlamını uygun şekilde ayarlamanız gerekir. `psql`Aşağıdaki komutu çalıştırdıktan sonra bu ayrıntıları uç nokta satırına bulacaksınız:

```console
azdata arc postgres endpoint list -n postgres01
```
Bu, şunun gibi bir çıktı döndürür:
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

Hedef veritabanını **RESTORED_MyOnPremPostgresDB** adını verlim  
:::image type="content" source="media/postgres-hyperscale/migrate-pg-destination-dbcreate.jpg" alt-text="Geçir-hedef-DB-oluştur"lightbox="media/postgres-hyperscale/migrate-pg-destination-dbcreate.jpg":::

### <a name="restore-the-database-in-your-arc-setup"></a>Yay kuruluminizdeki veritabanını geri yükleme
:::image type="content" source="media/postgres-hyperscale/migrate-pg-destination-dbrestore.jpg" alt-text="Migratre-DB-geri yükleme":::

Geri yüklemeyi yapılandırın:
1. Geri yüklenecek yedeği içeren dosyanın üzerine gelin: **Mysourcebackup**
2. Biçimi **Custom veya tar** 
    :::image type="content" source="media/postgres-hyperscale/migrate-pg-destination-dbrestore2.jpg" alt-text="Migrate-DB-restore-configure"::: olarak ayarlayın

3. **Geri yükle**' ye tıklayın.  

   Geri yükleme başarılı.  
   :::image type="content" source="media/postgres-hyperscale/migrate-pg-destination-dbrestore3.jpg" alt-text="Geçiş-DB-geri yükleme-tamamlandı":::

### <a name="verify-that-the-database-was-successfully-restored-in-your-azure-arc-enabled-postgresql-hyperscale-server-group"></a>Azure Arc etkin PostgreSQL hiper ölçek sunucu grubuna veritabanının başarıyla geri yüklendiğini doğrulama

Aşağıdaki yöntemlerden birini kullanın:

**Kimden `pgAdmin` :**  

Azure Arc kuruluminizde barındırılan Postgres örneğini genişletin. Tabloyu, geri yüklediğiniz veritabanında görürsünüz ve verileri seçtiğinizde şirket içi örnekte olduğu gibi aynı satırı gösterir:.

   :::image type="content" source="media/postgres-hyperscale/migrate-pg-destination-dbrestoreverif.jpg" alt-text="Geçiş-DB-geri yükleme-doğrulama":::

**`psql`Azure Arc kurulumlarınızın içinden:**  

Arc kurulumlarınızın içinde, `psql` Postgres örneğinize bağlanmak için kullanabilirsiniz, veritabanı bağlamını verileri olarak ayarlayabilir `RESTORED_MyOnPremPostgresDB` ve sorgular:

1. Bağlantı dizinizden yardım almak için uç noktaları listeleyin `psql` :

   ```console
   azdata arc postgres endpoint list -n postgres01
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

1. `psql`Bağlantı dizeniz, `-d` veritabanı adını belirtmek için parametresini kullanın. Aşağıdaki komutla parola girmeniz istenir:

   ```console
   psql -d RESTORED_MyOnPremPostgresDB -U postgres -h 10.0.0.4 -p 32639
   ```

   `psql` sağlanır.

   ```output
   Password for user postgres:
   psql (10.12 (Ubuntu 10.12-0ubuntu0.18.04.1), server 12.3 (Debian 12.3-1.pgdg100+1))
   WARNING: psql major version 10, server major version 12.
         Some psql features might not work.
   SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, bits: 256, compression: off)
   Type "help" for help.

   RESTORED_MyOnPremPostgresDB=#   
   ```

1. Tabloyu seçin ve şirket içi Postgres örneğinden geri yüklediğiniz verileri görürsünüz:

   ```console
   RESTORED_MyOnPremPostgresDB=# select * from t1;
   ```

   ```output
    col1 |    col2
   ------+-------------
       1 | BobbyIsADog
   (1 row)
   ```

> [!NOTE]
> - Ölçeklendirene ve verileri PostgreSQL hiper ölçek sunucu grubunuzun çalışan düğümleri genelinde parçalara ayırmanızın yanı sıra, Azure Arc etkin PostgreSQL hiper ölçeğinde çalıştırmanın çok fazla performans avantajını görmezsiniz. [Sonraki adımlara](#next-steps)bakın.
>
> - Bugün, şirket içinde veya başka bir bulutta çalışan mevcut bir Postgres örneği olan "Azure yaya ekleme" konusunda mümkün değildir. Diğer bir deyişle, Azure Arc tarafından etkinleştirilen bir Postgres kurulumu yapmak için mevcut Postgres Örneğinizde "Azure Arc Agent" öğesinin bir sıralamasını yüklemek mümkün değildir. Bunun yerine, yeni bir Postgres örneği oluşturmanız ve verileri buna aktarmanız gerekir. Bunu yapmak için yukarıda gösterilen tekniği kullanabilir veya istediğiniz ETL aracını kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- PostgreSQL için Azure veritabanı 'nın, verileri birden çok PostgreSQL hiper ölçek düğümüne dağıtmak ve PostgreSQL için Azure veritabanı hiper ölçek 'in tüm gücünden yararlanmak için Azure veritabanı 'nın kavramlarını ve nasıl yapılır kılavuzlarını okuyun:
    * [Düğümler ve tablolar](../../postgresql/concepts-hyperscale-nodes.md)
    * [Uygulama türünü belirleme](../../postgresql/concepts-hyperscale-app-type.md)
    * [Dağıtım sütunu seçme](../../postgresql/concepts-hyperscale-choose-distribution-column.md)
    * [Tablo birlikte bulundurma](../../postgresql/concepts-hyperscale-colocation.md)
    * [Tabloları dağıtma ve değiştirme](../../postgresql/howto-hyperscale-modify-distributed-tables.md)
    * [Çok kiracılı bir veritabanı tasarlama](../../postgresql/tutorial-design-database-hyperscale-multi-tenant.md)*
    * [Gerçek zamanlı analiz panosu tasarlama](../../postgresql/tutorial-design-database-hyperscale-realtime.md)*

> * Bu belgelerde, **Azure Portal oturum açma**bölümlerini atlayın ve **Postgres-hyperscale Için bir Azure veritabanı oluşturun (Citus)**. Azure Arc dağıtımınızda kalan adımları uygulayın. Bu bölümler, Azure bulutunda PaaS hizmeti olarak sunulan PostgreSQL için Azure veritabanı 'na (Citus) özgüdür, ancak belgelerin diğer kısımları doğrudan Azure Arc etkin PostgreSQL hiper ölçeklendirmenize uygulanabilir.

- [PostgreSQL için Azure veritabanı hiper ölçek sunucu grubu ölçeğini genişletme](scale-out-postgresql-hyperscale-server-group.md)
