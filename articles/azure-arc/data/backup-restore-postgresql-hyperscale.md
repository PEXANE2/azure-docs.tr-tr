---
title: PostgreSQL için Azure Veritabanı Hiper Ölçek sunucu grupları için Yedekleme ve Geri Yükleme
description: PostgreSQL için Azure Veritabanı Hiper Ölçek sunucu grupları için Yedekleme ve Geri Yükleme
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 4fb64a2ea55744d66b203ef4d901f22ae4695e1a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91630432"
---
# <a name="backup-and-restore-for-azure-arc-enabled-postgresql-hyperscale-server-groups"></a>Azure Arc etkin PostgreSQL hiper ölçek sunucu grupları için yedekleme ve geri yükleme

Azure Arc etkin PostgreSQL hiper ölçek sunucu grubunuz için tam yedekleme/geri yükleme gerçekleştirebilirsiniz. Bunu yaptığınızda, Azure Arc etkin PostgreSQL hiper ölçek sunucu grubu 'nun tüm düğümlerinde tüm veritabanı kümesi yedeklenir ve/veya geri yüklenir.
Bir yedekleme yapmak ve geri yüklemek için, sunucu grubunuz için bir yedekleme depolama sınıfının yapılandırıldığından emin olmanız gerekir. Şimdilik, sunucu grubunu oluşturduğunuz sırada bir yedekleme depolama sınıfı belirtmeniz gerekir. Sunucu grubunuzu oluşturulduktan sonra bir yedekleme depolama sınıfı kullanacak şekilde yapılandırmak henüz mümkün değildir.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

> [!CAUTION]
> Önizleme, Postgres altyapısının sürüm 11 ' i için yedekleme/geri yüklemeyi desteklemez. Yalnızca Postgres sürüm 12 için yedekleme/geri yüklemeyi destekler.

## <a name="verify-configuration"></a>Yapılandırmayı Doğrula

İlk olarak, mevcut sunucu grubunuzun yedekleme depolama sınıfını kullanacak şekilde yapılandırılıp yapılandırılmadığını doğrulayın.

Sunucu grubunuzun adını ayarladıktan sonra aşağıdaki komutu çalıştırın:
```console
 azdata arc postgres server show -n postgres01
```
Çıktının depolama bölümüne bakın:
```console
...
"storage": {
      "backups": {
        "className": "local-storage"
      },
      "data": {
        "className": "local-storage",
        "size": "5Gi"
      },
      "logs": {
        "className": "local-storage",
        "size": "5Gi"
      }
    }
...
```
Bu komutun çıktısının "yedeklemeler" bölümünde belirtilen bir depolama sınıfının adını görürseniz, bu, sunucu grubunuzun bir yedekleme depolama sınıfı kullanacak şekilde yapılandırıldığı ve yedeklemeleri alıp geri yükleme işlemi yapabileceğiniz anlamına gelir. "Yedeklemeler" bölümünü görmüyorsanız, yedekleme depolama sınıfını yapılandırmak için sunucu grubunuzu silip yeniden oluşturmanız gerekir. Bu noktada, sunucu grubu oluşturulduktan sonra bir yedekleme depolama sınıfı yapılandırmak henüz mümkün değildir.

>[!IMPORTANT]
>Sunucu grubunuz zaten bir yedekleme depolama sınıfı kullanacak şekilde yapılandırıldıysa, sonraki adımı atlayın ve doğrudan "el ile tam yedekleme yapın" adımına geçin.

## <a name="create-a-server-group"></a>Sunucu grubu oluşturma 

Ardından, yedekleme/geri yükleme için yapılandırılmış bir sunucu grubu oluşturun.

Yedeklemeleri alabilmesi ve geri yükleyebilmeleri için, depolama sınıfıyla yapılandırılmış bir sunucu oluşturmanız gerekir.

Kubernetes kümenizde bulunan depolama sınıflarının bir listesini almak için aşağıdaki komutu çalıştırın:

```console
kubectl get sc
```

<!--The general format of create server group command is documented [here](create-postgresql-instances.md)-->

```console
azdata arc postgres server create -n <name> --workers 2 --storage-class-backups <storage class name> [--storage-class-data <storage class name>] [--storage-class-logs <storage class name>]
```

Örneğin, kubeadm tabanlı basit bir ortam oluşturduysanız:
```console
azdata arc postgres server create -n postgres01 --workers 2 --storage-class-backups local-storage
```

## <a name="take-manual-full-backup"></a>El ile tam yedekleme yapın

Ardından, el ile tam yedekleme gerçekleştirin.

Sunucu grubunuzun tüm veri ve günlük klasörlerinin tam yedeklemesini yapmak için aşağıdaki komutu çalıştırın:

```console
azdata arc postgres backup create [--name <backup name>] --server-name <server group name> [--no-wait] 
```
Burada:
- __ad__ , bir yedeklemenin adını gösterir
- __sunucu adı__ bir sunucu grubunu gösterir
- __No-wait__ , komut satırının bu komut satırı penceresini kullanmaya devam edebilmeniz için yedeklemenin tamamlanmasını bekmeyeceğini belirtir

Bu komut, Azure Arc etkin PostgreSQL hiper ölçek sunucu grubunu oluşturan tüm düğümlerde dağıtılmış tam yedeklemeyi koordine edecektir. Diğer bir deyişle, düzenleyici ve çalışan düğüminizdeki tüm verileri yedekler.

Örneğin:
```console
azdata arc postgres backup create --name MyBackup_Aug31_0730amPST --server-name postgres01
```

Yedekleme tamamlandığında yedeklemenin KIMLIĞI, adı ve durumu döndürülür. Örneğin:
```console
{
  "ID": "d134f51aa87f4044b5fb07cf95cf797f",
  "name": "MyBackup_Aug31_0730amPS",
  "state": "Done"
}
```

> [!NOTE]
> Henüz şunları yapmak mümkün değildir:
> - Otomatik yedeklemeleri zamanla
> - Bir yedeklemenin alındığı sırada ilerlemesini göster

## <a name="list-backups"></a>Yedeklemeleri listeleme

Geri yüklenebilecek yedeklemeleri listeleyin.

Geri yüklenecek yedeklemeleri listelemek için aşağıdaki komutu çalıştırın:

```console
azdata arc postgres backup list --server-name <servergroup name>
```

Örneğin:
```console
azdata arc postgres backup list --server-name postgres01
```

Şu şekilde bir çıktı döndürür:
```console
ID                                Name                      State    Timestamp
--------------------------------  ------------------------  -------  ------------------------------
d134f51aa87f4044b5fb07cf95cf797f  MyBackup_Aug31_0730amPST  Done     2020-08-31 14:30:00:00+00:00
```

Zaman damgası, yedeklemenin alındığı zaman UTC süresini gösterir.

## <a name="restore-a-backup"></a>Yedeği geri yükleme

Tüm sunucu grubunun yedeklemesini geri yüklemek için şu komutu çalıştırın:

```console
azdata arc postgres backup restore --server-name <server group name> --backup-id <backup id>
```

Burada:
- __yedekleme kimliği__ , yedekleme listesi komutunda GÖSTERILEN yedeklemenin kimliğidir (adım 3 ' e başvurun).
Bu, Azure Arc etkin PostgreSQL hiper ölçek sunucu grubunu oluşturan tüm düğümlerde dağıtılan bir tam geri yüklemeyi koordine edecektir. Diğer bir deyişle, Düzenleyicinizdeki ve çalışan düğümlerdeki tüm verileri geri yükler.

Örneğin:
```console
azdata arc postgres backup restore --server-name postgres01 --backup-id d134f51aa87f4044b5fb07cf95cf797f
```

Geri yükleme işlemi tamamlandığında komut satırına aşağıdakine benzer bir çıktı döndürür:
```console
{
  "ID": "d134f51aa87f4044b5fb07cf95cf797f",
  "state": "Done"
}
```
> [!NOTE]
> Henüz şunları yapmak mümkün değildir:
> - Adını belirterek bir yedeği geri yükleme
> - Sunucu grubunu farklı bir adla veya farklı bir sunucu grubuna geri yükleme
> - Geri yükleme işleminin ilerlemesini göster

## <a name="delete-backups"></a>Yedekleri silme
Yedekleme bekletme önizlemede ayarlanamıyor. Ancak, ihtiyacınız olmayan yedeklemeleri el ile silebilirsiniz.
Yedeklemeleri silmenin genel komutu:
```console
azdata arc postgres backup delete  [--server-name, -sn] {[--name, -n], -id}
```
burada:
- `--server-name` kullanıcının bir yedeklemeyi silmek istediği sunucu grubunun adıdır
- `--name` Silinecek yedeğin adı
- `-id`Silinecek yedeğin KIMLIĞI

> [!NOTE]
> `--name` ve `-id` birbirini dışlıyor.

Önceki paragrafta açıklandığı gibi, liste yedekleme komutunu çalıştırarak yedeklemelerinizin adını ve KIMLIĞINI elde edebilirsiniz.

Örneğin, aşağıdaki yedeklemelerin listelendiğini göz önünde bulundurun:
```console
azdata arc postgres backup list -sn postgres01
ID                                Name                    State
--------------------------------  ----------------------  -------
5b0481dfc1c94b4cac79dd56a1bb21f4  MyBackup091720200110am  Done
0cf39f1e92344e6db4cfa285d36c7b14  MyBackup091720200111am  Done
```
ve bunlardan birincbirini silmek istiyorsanız aşağıdaki komutu çalıştırın:
```console
azdata arc postgres backup delete -sn postgres01 -n MyBackup091720200110am
{
  "ID": "5b0481dfc1c94b4cac79dd56a1bb21f4",
  "name": "MyBackup091720200110am",
  "state": "Done"
}
```
Bu noktada yedeklemeleri listeyrsanız aşağıdaki çıktıyı alırsınız:
```console
azdata arc postgres backup list -sn postgres01
ID                                Name                    State
--------------------------------  ----------------------  -------
0cf39f1e92344e6db4cfa285d36c7b14  MyBackup091720200111am  Done
```

Delete komutu hakkında daha fazla ayrıntı için şunu çalıştırın:
```console
azdata arc postgres backup delete --help
```

## <a name="next-steps"></a>Sonraki adımlar
- Ölçek Genişletme [(çalışan düğümleri ekleme)](scale-out-postgresql-hyperscale-server-group.md) hakkında bilgi edinmek için sunucu grubunuz
- Sunucu grubunuzu [ölçeği artırma veya azaltma (bellek/sanal çekirdekleri artırma/azaltma)](scale-up-down-postgresql-hyperscale-server-group-using-cli.md) hakkında bilgi edinin
