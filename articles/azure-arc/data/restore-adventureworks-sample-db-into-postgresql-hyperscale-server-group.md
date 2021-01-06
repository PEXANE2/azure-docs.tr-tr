---
title: AdventureWorks örnek veritabanını Azure Arc etkin PostgreSQL hiper ölçeğe geri yükleme
description: AdventureWorks örnek veritabanını Azure Arc etkin PostgreSQL hiper ölçeğe geri yükleme
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: b1ee779be118fcafd0efa2bd2718ece1c34c50d1
ms.sourcegitcommit: 19ffdad48bc4caca8f93c3b067d1cf29234fef47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2021
ms.locfileid: "97954337"
---
# <a name="restore-the-adventureworks-sample-database-to-azure-arc-enabled-postgresql-hyperscale"></a>AdventureWorks örnek veritabanını Azure Arc etkin PostgreSQL hiper ölçeğe geri yükleme

[AdventureWorks](/sql/samples/adventureworks-install-configure) , öğreticiler ve örnekler IÇIN kullanılan OLTP veritabanını içeren örnek bir veritabanıdır. [SQL Server örnekleri GitHub deposunun](https://github.com/microsoft/sql-server-samples/tree/master/samples/databases)bir parçası olarak Microsoft tarafından sağlanmış ve sürdürülür.

Açık kaynaklı bir proje, AdventureWorks veritabanını Azure Arc etkin PostgreSQL hiper Ölçeklendirmesiyle uyumlu olacak şekilde dönüştürmiştir.
- [Özgün proje](https://github.com/lorint/AdventureWorks-for-Postgres)
- [CSV dosyalarını PostgreSQL ile uyumlu olacak şekilde önceden dönüştüren projede izleyin](https://github.com/NorfolkDataSci/adventure-works-postgres)

Bu belgede, AdventureWorks örnek veritabanını PostgreSQL hiper ölçek sunucu grubunuza geri yüklemek için basit bir işlem açıklanmaktadır.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="download-the-adventureworks-backup-file"></a>AdventureWorks yedekleme dosyasını indirin

AdventureWorks. SQL dosyasını PostgreSQL hiper ölçek sunucu grubu kapsayıcınıza indirin. Bu örnekte, `kubectl exec` dosyayı kapsayıcıya indirmek Için PostgreSQL hiper ölçek sunucu grubu kapsayıcısında bir komutu uzaktan yürütmek üzere komutunu kullanacağız. Bu dosyayı tarafından erişilebilen herhangi bir konumdan indirebilirsiniz `curl` . PostgreSQL hiper ölçek sunucu grubu kapsayıcısına çekmek istediğiniz başka veritabanı yedekleme dosyalarınız varsa, bu yöntemi kullanın. PostgreSQL hiper ölçek sunucu grubu kapsayıcısında olduktan sonra veritabanını, şemayı oluşturmak ve verileri doldurmak kolaydır.

Dosyaları indirmek için aşağıdaki gibi bir komut çalıştırın. çalıştırmadan önce pod adı ve ad alanı adının değerini değiştirin:

> [!NOTE]
>  Dosyayı GitHub 'dan indirmek için Kapsayıcınızın 443 üzerinden Internet bağlantısı olması gerekir.

> [!NOTE]
>  Postgres Hyperscale sunucu grubunun düzenleyici düğümünün Pod adını kullanın. Adı <server group name> -0 ' dır.  Pod adından emin değilseniz komutu çalıştırın `kubectl get pod`

```console
kubectl exec <PostgreSQL pod name> -n <namespace name> -c postgres  -- /bin/bash -c "cd /tmp && curl -k -O https://raw.githubusercontent.com/microsoft/azure_arc/main/azure_arc_data_jumpstart/aks/arm_template/postgres_hs/AdventureWorks.sql"

#Example:
#kubectl exec postgres02-0 -n arc -c postgres -- /bin/bash -c "cd /tmp && curl -k -O https://raw.githubusercontent.com/microsoft/azure_arc/main/azure_arc_data_jumpstart/aks/arm_template/postgres_hs/AdventureWorks.sql"
```

## <a name="step-2-restore-the-adventureworks-database"></a>2. Adım: AdventureWorks veritabanını geri yükleme

Benzer şekilde, veritabanını oluşturmak ve yüklemek için PostgreSQL hiper ölçek sunucu grubu kapsayıcılarına dahil olan psql CLı aracını kullanmak için bir kubectl exec komutu çalıştırabilirsiniz.

İlk olarak, Pod adı ve ad alanı adı değerlerini çalıştırmadan önce boş veritabanını oluşturmak için bu gibi bir komut çalıştırın.

```console
kubectl exec <PostgreSQL pod name> -n <namespace name> -c postgres -- psql --username postgres -c 'CREATE DATABASE "adventureworks";'

#Example
#kubectl exec postgres02-0 -n arc -c postgres -- psql --username postgres -c 'CREATE DATABASE "adventureworks";'
```

Ardından, çalıştırmadan önce pod adı ve ad alanı adı değerlerini yerine getirmek için aşağıdaki gibi bir komut çalıştırın.

```console
kubectl exec <PostgreSQL pod name> -n <namespace name> -c postgres -- psql --username postgres -d adventureworks -f /tmp/AdventureWorks.sql

#Example
#kubectl exec postgres02-0 -n arc -c postgres -- psql --username postgres -d adventureworks -f /tmp/AdventureWorks.sql
```


> **Not: siz ölçeklendirene ve veri/tabloları PostgreSQL hiper ölçek sunucu grubunuzun çalışan düğümlerinde parçalamanızın yanı sıra, Azure Arc etkin PostgreSQL hiper ölçeğinde çalıştırmanın çok fazla performans avantajını görmezsiniz. [Önerilen sonraki adımlar](#suggested-next-steps)bölümüne bakın.**

## <a name="suggested-next-steps"></a>Önerilen sonraki adımlar
- PostgreSQL için Azure veritabanı 'nın, verileri birden çok PostgreSQL hiper ölçek düğümüne dağıtmak ve PostgreSQL için Azure veritabanı hiper ölçek 'in tüm gücünden yararlanmak için Azure veritabanı 'nın kavramlarını ve nasıl yapılır kılavuzlarını okuyun. :
    * [Düğümler ve tablolar](../../postgresql/concepts-hyperscale-nodes.md)
    * [Uygulama türünü belirleme](../../postgresql/concepts-hyperscale-app-type.md)
    * [Dağıtım sütunu seçme](../../postgresql/concepts-hyperscale-choose-distribution-column.md)
    * [Tablo birlikte bulundurma](../../postgresql/concepts-hyperscale-colocation.md)
    * [Tabloları dağıtma ve değiştirme](../../postgresql/howto-hyperscale-modify-distributed-tables.md)
    * [Çok kiracılı bir veritabanı tasarlama](../../postgresql/tutorial-design-database-hyperscale-multi-tenant.md)*
    * [Gerçek zamanlı analiz panosu tasarlama](../../postgresql/tutorial-design-database-hyperscale-realtime.md)*

   > \* Yukarıdaki belgelerde, **Azure Portal oturum açma** bölümünü atlayın, & **PostgreSQL Için Azure veritabanı oluşturun-hiper ölçek (Citus)**. Azure Arc dağıtımınızda kalan adımları uygulayın. Bu bölümler, Azure bulutunda PaaS hizmeti olarak sunulan PostgreSQL için Azure veritabanı 'na (Citus) özgüdür, ancak belgelerin diğer kısımları doğrudan Azure Arc etkin PostgreSQL hiper ölçeklendirmenize uygulanabilir.

- [PostgreSQL için Azure Veritabanı Hiper Ölçek sunucu grubunuzun ölçeğini genişletme](scale-out-postgresql-hyperscale-server-group.md)
