---
title: Azure Arc etkin yönetilen örnek yüksek kullanılabilirliği
titleSuffix: Deploy Azure Arc enabled Managed Instance with high availability
description: Azure Arc etkin yönetilen örneğini yüksek kullanılabilirliğe dağıtmayı öğrenin.
author: vin-yu
ms.author: vinsonyu
ms.reviewer: mikeray
ms.date: 03/02/2021
ms.topic: conceptual
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.openlocfilehash: 92f5c900238fc5d40e22870e2f00f8adeb5d335f
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102032203"
---
# <a name="azure-arc-enabled-managed-instance-high-availability"></a>Azure Arc etkin yönetilen örnek yüksek kullanılabilirliği

Azure Arc etkinleştirilmiş yönetilen örnek, Kubernetes 'te kapsayıcılı bir uygulama olarak dağıtılır ve hizmet durumunu sürdürmek için yerleşik sistem durumu izleme, hata algılama ve yük devretme mekanizmaları sağlamak üzere durum bilgisi bulunan kümeler ve kalıcı depolama gibi Kubernetes yapılarını kullanır. Daha fazla güvenilirlik için, yüksek kullanılabilirlik yapılandırmasında ek yinelemelerle dağıtmak üzere Azure Arc etkinleştirilmiş yönetilen örneği de yapılandırabilirsiniz. İzleme, hata algılama ve otomatik yük devretme, Arc veri Hizmetleri veri denetleyicisi tarafından yönetilir. Bu hizmet, Kullanıcı müdahalesi olmadan sağlanır. tüm kullanılabilirlik grubu kurulumundan, veritabanı yansıtma uç noktaları yapılandırılarak veritabanları, kullanılabilirlik grubuna veya yük devretmeye ve yükseltme koordinasyonunu veritabanına ekleniyor. Bu belge her iki tür yüksek kullanılabilirliği araştırır.

## <a name="built-in-high-availability"></a>Yerleşik yüksek kullanılabilirlik 

Yerleşik yüksek kullanılabilirlik, uzak kalıcı depolama yapılandırıldığında ve Arc veri hizmeti dağıtımı tarafından kullanılan düğümlerle paylaşıldığında Kubernetes tarafından sağlanır. Bu yapılandırmada Kubernetes, küme Orchestrator rolünü yürütür. Bir kapsayıcı veya temeldeki düğüm içindeki yönetilen örnek başarısız olduğunda, Orchestrator kapsayıcının başka bir örneğini önyükleme ve aynı kalıcı depolamaya ekler. Bu tür, Azure Arc etkinleştirilmiş yönetilen örneği dağıtırken varsayılan olarak etkindir.

### <a name="verify-built-in-high-availability"></a>Yerleşik yüksek kullanılabilirliği doğrulama

Bu bölümde, Kubernetes tarafından sunulan yerleşik yüksek kullanılabilirliği doğrularsınız. Bu işlevi test etmek için adımları izlediğinizde, var olan bir yönetilen örneğin Pod 'u siler ve Kubernetes 'in bu eylemden kurtarır. 

### <a name="prerequisites"></a>Önkoşullar

- Kubernetes kümesinin [paylaşılan, uzak depolama birimi](storage-configuration.md#factors-to-consider-when-choosing-your-storage-configuration) olması gerekir 
- Bir çoğaltmayla dağıtılan Azure Arc etkin yönetilen örnek (varsayılan)

1. Pods 'leri görüntüleyin. 

   ```console
   kubectl get pods -n <namespace of data controller>
   ```

2. Yönetilen örnek Pod öğesini silin.

   ```console
   kubectl delete pod <name of managed instance>-0 -n <namespace of data controller>
   ```

   Örneğin:

   ```output
   user@pc:/# kubectl delete pod sql1-0 -n arc
   pod "sql1-0" deleted
   ```

3. Yönetilen örneğin kurtarmakta olduğunu doğrulamak için pod 'yi görüntüleyin.

   ```console
   kubectl get pods -n <namespace of data controller>
   ```

   Örneğin:

   ```output
   user@pc:/# kubectl get pods -n arc
   NAME                 READY   STATUS    RESTARTS   AGE
   sql1-0               2/3     Running   0          22s
   ```

Pod içindeki tüm kapsayıcılar kurtarıldıktan sonra, yönetilen örneğe bağlanabilirsiniz.

## <a name="deploy-with-always-on-availability-groups"></a>Always on kullanılabilirlik gruplarıyla dağıtma

Daha fazla güvenilirlik için, yüksek kullanılabilirlik yapılandırmasında ek yinelemelerle dağıtmak üzere Azure Arc etkinleştirilmiş yönetilen örneği yapılandırabilirsiniz. 

Kullanılabilirlik gruplarının etkinleştirileceği yetenekler:

- Birden çok çoğaltmalarla dağıtıldığında adlı tek bir kullanılabilirlik grubu `containedag` oluşturulur. Varsayılan olarak, `containedag` birincil dahil olmak üzere üç kopyaya sahiptir. Kullanılabilirlik grubu için tüm CRUD işlemleri, kullanılabilirlik grubu oluşturma veya çoğaltmaları oluşturulan kullanılabilirlik grubuna katma dahil olmak üzere dahili olarak yönetilir. Ek kullanılabilirlik grupları, Azure Arc etkinleştirilmiş yönetilen örneğinde oluşturulamıyor.

- Tüm veritabanları, ve gibi tüm Kullanıcı ve sistem veritabanları dahil olmak üzere kullanılabilirlik grubuna otomatik olarak `master` eklenir `msdb` . Bu özellik, kullanılabilirlik grubu çoğaltmaları genelinde tek sistem bir görünüm sağlar. `containedag_master`Örneğe doğrudan bağlanıyorsanız, ve veritabanlarına her ikisine de dikkat edin `containedag_msdb` . `containedag_*`Veritabanları `master` `msdb` kullanılabilirlik grubunun içinde ve içinde temsil eder.

- Kullanılabilirlik grubu içindeki veritabanlarına bağlanmak için otomatik olarak bir dış uç nokta sağlanır. Bu uç nokta, `<managed_instance_name>-svc-external` kullanılabilirlik grubu dinleyicisinin rolünü yürütür.

### <a name="deploy"></a>Dağıtma

Yönetilen bir örneği kullanılabilirlik gruplarıyla dağıtmak için aşağıdaki komutu çalıştırın.

```console
azdata arc sql mi create -n <name of instance> --replicas 3
```

### <a name="check-status"></a>Durumu kontrol etme
Örnek dağıtıldıktan sonra, örneğinizin durumunu denetlemek için aşağıdaki komutları çalıştırın:

```console
azdata arc sql mi list
azdata arc sql mi show -n <name of instance>
```

Örnek çıktı:

```output
user@pc:/# azdata arc sql mi list
ExternalEndpoint    Name    Replicas    State
------------------  ------  ----------  -------
20.131.31.58,1433   sql2    3/3         Ready

user@pc:/#  azdata arc sql mi show -n sql2
{
...
  "status": {
    "AGStatus": "Healthy",
    "externalEndpoint": "20.131.31.58,1433",
    "logSearchDashboard": "link to logs dashboard",
    "metricsDashboard": "link to metrics dashboard",
    "readyReplicas": "3/3",
    "state": "Ready"
  }
}
```

Ek sayısına `Replicas` ve `AGstatus` kullanılabilirlik grubunun sistem durumunu gösteren alana dikkat edin. Tüm çoğaltmalar çalışır duruma ve eşitlenirse, bu değer olur `healthy` . 

### <a name="restore-a-database"></a>Veritabanını geri yükleme 
Bir veritabanını bir kullanılabilirlik grubuna geri yüklemek için ek adımlar gereklidir. Aşağıdaki adımlarda bir veritabanını yönetilen bir örneğe geri yükleme ve bir kullanılabilirlik grubuna ekleme gösterilmektedir. 

1. Yeni bir Kubernetes hizmeti oluşturarak birincil örnek dış uç noktasını kullanıma sunun.

    Yönetilen örneğe bağlanarak birincil çoğaltmayı barındıran Pod 'ı belirleme ve çalıştırma:

    ```sql
    SELECT @@SERVERNAME
    ```
    Kubernetes kümeniz nodePort Hizmetleri kullanıyorsa, aşağıdaki komutu çalıştırarak birincil örneğe Kubernetes hizmetini oluşturun. `podName`Önceki adımda döndürülen sunucunun adıyla değiştirin. Bu, `serviceName` Kubernetes hizmeti için tercih edilen ad ile oluşturulur.

    ```bash
    kubectl -n <namespaceName> expose pod <podName> --port=1533  --name=<serviceName> --type=NodePort
    ```

    Bir LoadBalancer hizmeti için, oluşturulan hizmetin türü dışında aynı komutu çalıştırın `LoadBalancer` . Örnek: 

    ```bash
    kubectl -n <namespaceName> expose pod <podName> --port=1533  --name=<serviceName> --type=LoadBalancer
    ```

    Bu komutun, birincili barındıran Pod 'ın Azure Kubernetes hizmetine karşı çalıştırıldığı bir örneği aşağıda verilmiştir `sql2-0` :

    ```bash
    kubectl -n arc-cluster expose pod sql2-0 --port=1533  --name=sql2-0-p --type=LoadBalancer
    ```

    Oluşturulan Kubernetes hizmetinin IP 'sini alın:

    ```bash
    kubectl get services -n <namespaceName>
    ```
2. Veritabanını birincil örnek uç noktasına geri yükleyin.

    Veritabanı yedek dosyasını birincil örnek kapsayıcısına ekleyin.

    ```console
    kubectl cp <source file location> <pod name>:var/opt/mssql/data/<file name> -n <namespace name>
    ```

    Örnek

    ```console
    kubectl cp /home/WideWorldImporters-Full.bak sql2-1:var/opt/mssql/data/WideWorldImporters-Full.bak -c arc-sqlmi -n arc
    ```

    Aşağıdaki komutu çalıştırarak veritabanı yedekleme dosyasını geri yükleyin.

    ```sql 
    RESTORE DATABASE test FROM DISK = '/var/opt/mssql/data/<file name>.bak'
    WITH MOVE '<database name>' to '/var/opt/mssql/data/<file name>.mdf'  
    ,MOVE '<database name>' to '/var/opt/mssql/data/<file name>_log.ldf'  
    ,RECOVERY, REPLACE, STATS = 5;  
    GO
    ```
    
    Örnek

    ```sql
    RESTORE Database WideWorldImporters
    FROM DISK = '/var/opt/mssql/data/WideWorldImporters-Full.BAK'
    WITH
    MOVE 'WWI_Primary' TO '/var/opt/mssql/data/WideWorldImporters.mdf',
    MOVE 'WWI_UserData' TO '/var/opt/mssql/data/WideWorldImporters_UserData.ndf',
    MOVE 'WWI_Log' TO '/var/opt/mssql/data/WideWorldImporters.ldf',
    MOVE 'WWI_InMemory_Data_1' TO '/var/opt/mssql/data/WideWorldImporters_InMemory_Data_1',
    RECOVERY, REPLACE, STATS = 5;  
    GO
    ```

3. Veritabanını kullanılabilirlik grubuna ekleyin.

    Veritabanının AG 'ye eklenebilmesi için tam kurtarma modunda çalışması ve bir günlük yedeklemesinin alınması gerekir. Geri yüklenen veritabanını kullanılabilirlik grubuna eklemek için aşağıdaki TSQL deyimlerini çalıştırın.

    ```sql
    ALTER DATABASE <databaseName> SET RECOVERY FULL;
    BACKUP DATABASE <databaseName> TO DISK='<filePath>'
    ALTER AVAILABILITY GROUP containedag ADD DATABASE <databaseName>
    ```

    Aşağıdaki örnek, `WideWorldImporters` örneği üzerinde geri yüklenmiş adlı bir veritabanını ekler:

    ```sql
    ALTER DATABASE WideWorldImporters SET RECOVERY FULL;
    BACKUP DATABASE WideWorldImporters TO DISK='/var/opt/mssql/data/WideWorldImporters.bak'
    ALTER AVAILABILITY GROUP containedag ADD DATABASE WideWorldImporters
    ```

> [!IMPORTANT]
> En iyi uygulama olarak, bu komutu çalıştırarak yukarıda oluşturulan Kubernetes hizmetini silerek temizlik yapmanız gerekir:
>
>```bash
>kubectl delete svc sql2-0-p -n arc
>```

### <a name="limitations"></a>Sınırlamalar

Azure Arc etkin yönetilen örnek kullanılabilirlik grupları, [büyük veri kümesi kullanılabilirlik gruplarıyla aynı sınırlamalara sahiptir. Daha fazla bilgi için buraya tıklayın.](/sql/big-data-cluster/deployment-high-availability#known-limitations)

## <a name="next-steps"></a>Sonraki adımlar

[Azure Arc ETKIN SQL yönetilen örneği özellikleri ve](managed-instance-features.md) özellikleri hakkında daha fazla bilgi edinin
