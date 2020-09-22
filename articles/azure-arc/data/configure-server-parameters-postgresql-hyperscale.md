---
title: Azure Arc 'da PostgreSQL hiper ölçek sunucu grubunuz için Postgres motoru sunucu parametrelerini yapılandırma
titleSuffix: Azure Arc enabled data services
description: Azure Arc 'da PostgreSQL hiper ölçek sunucu grubunuz için Postgres motoru sunucu parametrelerini yapılandırma
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 4e8813647211e0adbfe43a45ae0d19dc12a4a165
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941595"
---
# <a name="set-the-database-engine-settings-for-azure-arc-enabled-postgresql-hyperscale"></a>Azure Arc etkin PostgreSQL hiper ölçek için veritabanı altyapısı ayarlarını belirleme

Bu belgede, PostgreSQL hiper ölçek sunucu grubunuzun veritabanı altyapısı ayarlarını özel (varsayılan olmayan) değerlere ayarlama adımları açıklanmaktadır. Hangi veritabanı motoru parametrelerinin ayarlanabileceği ve varsayılan değerinin ne olduğu ile ilgili ayrıntılar için [buradaki](https://www.postgresql.org/docs/current/runtime-config.html)PostgreSQL belgelerine bakın.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

> [!NOTE]
> Önizleme, aşağıdaki parametrelerin ayarlanmasını desteklemez: 
>
> - `archive_command`
> - `archive_timeout`
> - `log_directory`
> - `log_file_mode`
> - `log_filename`
> - `restore_command`
> - `shared_preload_libraries`
> - `synchronous_commit`
> - `ssl`
> - `wal_level`

## <a name="syntax"></a>Sözdizimi

Veritabanı altyapısı ayarlarını yapılandırmak için komutun genel biçimi:

```console
azdata arc postgres server edit -n <server group name>, [{--engine-settings, -e}] [{--replace-engine-settings, --re}] {'<parameter name>=<parameter value>, ...'}
```

## <a name="show-the-current-custom-values-of-the-parameters-settings"></a>Parametre ayarlarının geçerli özel değerlerini göster

## <a name="with-azdata-cli-command"></a>Azdata CLı komutuyla

```console
azdata arc postgres server show -n <server group name>
```

Örnek:

```console
azdata arc postgres server show -n postgres01
```

Bu komut, ayarladığınız parametreleri görebileceğiniz sunucu grubunun belirtimini döndürür. Engine\settings bölümü yoksa, tüm parametrelerin varsayılan değer üzerinde çalıştığı anlamına gelir:

```console
 "
...
engine": {
      "settings": {
        "default": {
          "autovacuum_vacuum_threshold": "65"
        }
      }
    },
...
```

## <a name="with-kubectl-command"></a>Kubectl komutuyla

Aşağıdaki adımları izleyin.

### <a name="1-retrieve-the-kind-of-custom-resource-definition-for-your-server-group"></a>1. sunucu grubunuz için özel kaynak tanımı türünü alın

Çalıştır:

```console
azdata arc postgres server show -n <server group name>
```

Örnek:

```console
azdata arc postgres server show -n postgres01
```

Bu komut, ayarladığınız parametreleri görebileceğiniz sunucu grubunun belirtimini döndürür. Engine\settings bölümü yoksa, tüm parametrelerin varsayılan değer üzerinde çalıştığı anlamına gelir:

```
> {
  >"apiVersion": "arcdata.microsoft.com/v1alpha1",
  >"**kind**": "**postgresql-12**",
  >"metadata": {
    >"creationTimestamp": "2020-08-25T14:32:23Z",
    >"generation": 1,
    >"name": "postgres01",
    >"namespace": "arc",
```

Burada "tür" alanını bulup değeri ayırın, örneğin: `postgresql-12` .

### <a name="2-describe-the-kubernetes-custom-resource-corresponding-to-your-server-group"></a>2. sunucu grubunuza karşılık gelen Kubernetes özel kaynağını açıkla 

Komutun genel biçimi:

```console
kubectl describe <kind of the custom resource> <server group name> -n <namespace name>
```

Örnek:

```console
kubectl describe postgresql-12 postgres01
```

Altyapı ayarları için ayarlanmış özel değerler varsa, bu ayarlar döndürülür. Örnek:

```console
Engine:
...
    Settings:
      Default:
        autovacuum_vacuum_threshold:  65
```

Motor ayarlarından herhangi biri için özel değerler ayarlanmamışsa, sonuç kümesi 'nin motor ayarları bölümü şu şekilde boş olur:

```console
Engine:
...
    Settings:
      Default:
```

## <a name="set-custom-values-for-the-engine-settings"></a>Altyapı ayarları için özel değerler ayarla

Aşağıdaki komutlar, düzenleyici düğümünün ve PostgreSQL hiper ölçeklendirmenin çalışan düğümlerinin parametrelerini aynı değerlere ayarlar. Sunucu grubunuzda rol başına parametre ayarlamak henüz mümkün değildir. Diğer bir deyişle, belirli bir parametreyi düzenleyici düğümüne ve çalışan düğümleri için başka bir değere yapılandırmak henüz mümkün değildir.

## <a name="set-a-single-parameter"></a>Tek bir parametre ayarla

```console
azdata arc server edit -n <server group name> -e <parameter name>=<parameter value>
```

Örnek:

```console
azdata arc postgres server edit -n postgres01 -e shared_buffers=8MB
```

## <a name="set-multiple-parameters-with-a-single-command"></a>Birden çok parametreyi tek bir komutla ayarlama

```console
azdata arc postgres server edit -n <server group name> -e '<parameter name>=<parameter value>, <parameter name>=<parameter value>,...'
```

Örnek:

```console
azdata arc postgres server edit -n postgres01 -e 'shared_buffers=8MB, max_connections=50'
```

## <a name="reset-a-parameter-to-its-default-value"></a>Bir parametreyi varsayılan değerine sıfırlayın

Bir parametreyi varsayılan değerine sıfırlamak için değeri bir değer belirtmeden ayarlayın. 

Örnek:

```console
azdata arc postgres server edit -n postgres01 -e shared_buffers=
```

## <a name="reset-all-parameters-to-their-default-values"></a>Tüm parametreleri varsayılan değerlerine sıfırlayın

```console
azdata arc postgres server edit -n <server group name> -e '' -re
```

Örnek:

```console
azdata arc postgres server edit -n postgres01 -e '' -re
```

## <a name="special-considerations"></a>Özel Konular

### <a name="set-a-parameter-which-value-contains-a-comma-space-or-special-character"></a>Değer virgül, boşluk veya özel karakter içeren bir parametre ayarla

```console
azdata arc postgres server edit -n <server group name> -e '<parameter name>="<parameter value>"'
```

Örnek:

```console
azdata arc postgres server edit -n postgres01 -e 'custom_variable_classes = "plpgsql,plperl"'
```

### <a name="pass-an-environment-variable-in-a-parameter-value"></a>Bir parametre değerindeki bir ortam değişkenini geçirme

Ortam değişkeni, ayarlamadan önce çözümlenmemesi için "' '" içine sarmalanmış olmalıdır.

Örnek: 

```console
azdata arc postgres server edit -n postgres01 -e 'search_path = "$user"'
```



## <a name="next-steps"></a>Sonraki adımlar
- Ölçek Genişletme [(çalışan düğümleri ekleme)](scale-out-postgresql-hyperscale-server-group.md) hakkında bilgi edinmek için sunucu grubunuz
- Sunucu grubunuzu [ölçeği artırma veya azaltma (bellek/sanal çekirdekleri artırma/azaltma)](scale-up-down-postgresql-hyperscale-server-group-using-cli.md) hakkında bilgi edinin
