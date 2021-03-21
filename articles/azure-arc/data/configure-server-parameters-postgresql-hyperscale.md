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
ms.openlocfilehash: cdbddfc84b3f71576cfd0299f2babec859b4ef1f
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "92311057"
---
# <a name="set-the-database-engine-settings-for-azure-arc-enabled-postgresql-hyperscale"></a>Azure Arc özellikli PostgreSQL Hiper Ölçek için veritabanı altyapısı ayarlarını belirleme

Bu belgede PostgreSQL Hiper Ölçek sunucu grubunuzun veritabanı altyapısı ayarlarını özel (varsayılan olmayan) değerlere ayarlama adımları açıklanır. Hangi veritabanı motoru parametrelerinin ayarlanabileceği ve varsayılan değerinin ne olduğu ile ilgili ayrıntılar için [buradaki](https://www.postgresql.org/docs/current/runtime-config.html)PostgreSQL belgelerine bakın.

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

## <a name="syntax"></a>Syntax

Veritabanı altyapısı ayarlarını yapılandırmak için komutun genel biçimi:

```console
azdata arc postgres server edit -n <server group name>, [{--engine-settings, -e}] [{--replace-engine-settings, --re}] {'<parameter name>=<parameter value>, ...'}
```

## <a name="show-current-custom-values"></a>Geçerli özel değerleri göster

### <a name="with-azure-data-cli-azdata-command"></a>[!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)]Komutuyla

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

### <a name="with-kubectl-command"></a>Kubectl komutuyla

Aşağıdaki adımları izleyin.

1. Sunucu grubunuz için özel kaynak tanımı türünü alın

   Çalıştır:

   ```console
   azdata arc postgres server show -n <server group name>
   ```

   Örnek:

   ```console
   azdata arc postgres server show -n postgres01
   ```

   Bu komut, ayarladığınız parametreleri görebileceğiniz sunucu grubunun belirtimini döndürür. Engine\settings bölümü yoksa, tüm parametrelerin varsayılan değer üzerinde çalıştığı anlamına gelir:

   ```output
   > {
     >"apiVersion": "arcdata.microsoft.com/v1alpha1",
     >"**kind**": "**postgresql-12**",
     >"metadata": {
       >"creationTimestamp": "2020-08-25T14:32:23Z",
       >"generation": 1,
       >"name": "postgres01",
       >"namespace": "arc",  
   ```

   Çıktı sonuçlarında alanı bulun `kind` ve değeri ayırın, örneğin: `postgresql-12` .

2. Sunucu grubunuza karşılık gelen Kubernetes özel kaynağını açıkla 

   Komutun genel biçimi:

   ```console
   kubectl describe <kind of the custom resource> <server group name> -n <namespace name>
   ```

   Örnek:

   ```console
   kubectl describe postgresql-12 postgres01
   ```

   Altyapı ayarları için ayarlanmış özel değerler varsa, bunları döndürür. Örnek:

   ```output
   Engine:
   ...
    Settings:
      Default:
        autovacuum_vacuum_threshold:  65
   ```

   Motor ayarlarından herhangi biri için özel değerler ayarlandıysanız, öğesinin altyapı ayarları bölümü `resultset` Şu şekilde boştur:

   ```output
   Engine:
   ...
       Settings:
         Default:
   ```

## <a name="set-custom-values-for-engine-settings"></a>Altyapı ayarları için özel değerler ayarla

Aşağıdaki komutlar, düzenleyici düğümünün ve PostgreSQL hiper ölçeklendirmenin çalışan düğümlerinin parametrelerini aynı değerlere ayarlar. Sunucu grubunuzda rol başına parametre ayarlamak henüz mümkün değildir. Diğer bir deyişle, belirli bir parametreyi düzenleyici düğümüne ve çalışan düğümleri için başka bir değere yapılandırmak henüz mümkün değildir.

### <a name="set-a-single-parameter"></a>Tek bir parametre ayarla

```console
azdata arc server edit -n <server group name> -e <parameter name>=<parameter value>
```

Örnek:

```console
azdata arc postgres server edit -n postgres01 -e shared_buffers=8MB
```

### <a name="set-multiple-parameters-with-a-single-command"></a>Birden çok parametreyi tek bir komutla ayarlama

```console
azdata arc postgres server edit -n <server group name> -e '<parameter name>=<parameter value>, <parameter name>=<parameter value>,...'
```

Örnek:

```console
azdata arc postgres server edit -n postgres01 -e 'shared_buffers=8MB, max_connections=50'
```

### <a name="reset-a-parameter-to-its-default-value"></a>Bir parametreyi varsayılan değerine sıfırlayın

Bir parametreyi varsayılan değerine sıfırlamak için değeri bir değer belirtmeden ayarlayın. 

Örnek:

```console
azdata arc postgres server edit -n postgres01 -e shared_buffers=
```

### <a name="reset-all-parameters-to-their-default-values"></a>Tüm parametreleri varsayılan değerlerine sıfırlayın

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
