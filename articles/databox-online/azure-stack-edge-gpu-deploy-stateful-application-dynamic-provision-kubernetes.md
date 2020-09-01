---
title: Kubectl 'yi kullanarak Kubernetes durum bilgisi olan uygulamayı Azure Stack Edge cihazında dinamik olarak sağlanan paylaşımda dağıtma | Microsoft Docs
description: Bir Microsoft Azure Stack Edge cihazında kubectl kullanılarak dinamik olarak sağlanan bir paylaşımdan Kubernetes durum bilgisi olan bir uygulama dağıtımının nasıl oluşturulacağını ve yönetileceğini açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 08/26/2020
ms.author: alkohli
ms.openlocfilehash: e83b2b1cb7013b9d72bd2c2de8c7636d5c9d8773
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89087043"
---
# <a name="use-kubectl-to-run-a-kubernetes-stateful-application-with-storageclass-on-your-azure-stack-edge-device"></a>Azure Stack Edge cihazınızda StorageClass ile Kubernetes durum bilgisi olan bir uygulamayı çalıştırmak için kubectl kullanın

Bu makalede, depolama ve bir dağıtımı dinamik olarak sağlamak için StorageClass kullanarak Kubernetes 'te tek örnekli durum bilgisi olan bir uygulamanın nasıl dağıtılacağı gösterilir. Dağıtım, `kubectl` var olan bir Kubernetes kümesinde komutlar kullanır ve MySQL uygulamasını dağıtır. 

Bu yordam, [Azure Stack Edge cihazında Kubernetes depolamayı](azure-stack-edge-gpu-kubernetes-storage.md) Inceleyen ve [Kubernetes depolama](https://kubernetes.io/docs/concepts/storage/)kavramlarını öğrentiren kullanıcılara yöneliktir.


## <a name="prerequisites"></a>Önkoşullar

Durum bilgisi olan uygulamayı dağıtabilmeniz için cihazınızda aşağıdaki önkoşulları ve cihaza erişmek için kullanacağınız istemciyi tamamladığınızdan emin olun:

### <a name="for-device"></a>Cihaz için

- 1 düğümlü Azure Stack Edge cihazında oturum açma kimlik bilgileriniz var.
    - Cihaz etkinleştirilir. Bkz. [cihazı etkinleştirme](azure-stack-edge-gpu-deploy-activate.md).
    - Cihazda Azure portal aracılığıyla yapılandırılmış işlem rolü vardır ve bir Kubernetes kümesi vardır. Bkz. [Işlem yapılandırma](azure-stack-edge-gpu-deploy-configure-compute.md).

### <a name="for-client-accessing-the-device"></a>Cihaza erişen istemci için

- Azure Stack Edge cihazına erişmek için kullanılacak bir Windows istemci sisteminiz vardır.
    - İstemci Windows PowerShell 5,0 veya üstünü çalıştırıyor. Windows PowerShell 'in en son sürümünü indirmek için [Windows PowerShell 'ı yükleme](https://docs.microsoft.com/powershell/scripting/install/installing-windows-powershell?view=powershell-7)bölümüne gidin.
    
    - [Desteklenen bir işletim sistemine](azure-stack-edge-gpu-system-requirements.md#supported-os-for-clients-connected-to-device) sahip başka bir istemciniz de olabilir. Bu makalede, bir Windows istemcisi kullanılırken yordam açıklanmaktadır. 
    
    - [Azure Stack Edge cihazında Kubernetes kümesine erişme](azure-stack-edge-gpu-create-kubernetes-cluster.md)bölümünde açıklanan yordamı tamamladınız. Şunları yapabilirsiniz:
      - `userns1`Komutu aracılığıyla bir ad alanı oluşturuldu `New-HcsKubernetesNamespace` . 
      - `user1`Komutu aracılığıyla bir Kullanıcı oluşturuldu `New-HcsKubernetesUser` . 
      - `user1`Komutu aracılığıyla öğesine erişim verildi `userns1` `Grant-HcsKubernetesNamespaceAccess` .       
      - `kubectl`İstemcide yüklü ve `kubeconfig` dosyayı Kullanıcı yapılandırmasıyla C: \\ Users Kullanıcı \\ &lt; adı &gt; \\ . kuin olarak kaydetti. 
    
    - `kubectl`İstemci sürümünün Azure Stack Edge cihazında çalışan Kubernetes ana sürümünden daha fazla sürüm olmadığından emin olun. 
        - `kubectl version`İstemci üzerinde çalışan kubectl sürümünü denetlemek için kullanın. Tam sürümü bir yere unutmayın.
        - Azure Stack Edge cihazınızın yerel kullanıcı arabiriminde **Genel Bakış ' a** gidin ve Kubernetes yazılım numarasına göz atın. 
        - Desteklenen Kubernetes sürümünde belirtilen eşlemenin uyumluluk için bu iki sürümü doğrulayın<!-- insert link-->. 


Azure Stack Edge cihazınızda durum bilgisi olan bir uygulamayı dağıtmaya hazırlanıyor. 


## <a name="deploy-mysql"></a>MySQL dağıtma

Artık bir Kubernetes dağıtımı oluşturup bunu bir PersistentVolumeClaim (PVC) kullanarak yerleşik StorageClass öğesine bağlayarak durum bilgisi olan bir uygulamayı çalıştıracaksınız. 

`kubectl`Durum bilgisi olan uygulama dağıtımlarını oluşturmak ve yönetmek için kullandığınız tüm komutların yapılandırmayla ilişkili ad alanını belirtmesi gerekir. Bir kubectl komutunda ad alanını belirtmek için kullanın `kubectl <command> -n <your-namespace>` .

1. Ad uzayındaki Kubernetes kümenizde çalışan Pod 'lerin bir listesini alın. Pod, Kubernetes kümenizde çalışan bir uygulama kapsayıcısıdır veya işlemidir.

   ```powershell
   kubectl get pods -n <your-namespace>
   ```
    
   Komut kullanımı örneği aşağıda verilmiştir:
    
   ```powershell
    C:\Users\user>kubectl get pods -n "userns1"
    No resources found in userns1 namespace.    
    C:\Users\user>
   ```
    
   Bu çıktı, kümenizde çalışan hiçbir uygulama bulunmadığından hiçbir kaynak (pods) bulunamadığını sağlamalıdır.

1. Aşağıdaki YAML dosyalarını kullanacaksınız. `mysql-deployment.yml`Dosya MySQL çalıştıran ve PVC 'ye başvuran bir dağıtımı açıklar. Dosya, için bir birim bağlaması tanımlar `/var/lib/mysql` ve ardından 20 GB 'lik bir birim gibi görünen BIR PVC oluşturur. Dinamik bir BD sağlanır ve PVC bu BD 'e bağlanır.

    Aşağıdaki dosyayı kopyalayın ve `mysql-deployment.yml` Azure Stack Edge cihazına erişmek için kullandığınız Windows istemcisinde bir klasöre kaydedin.
    
    ```yml
    apiVersion: v1
    kind: Service
    metadata:
      name: mysql
    spec:
      ports:
      - port: 3306
      selector:
        app: mysql
      clusterIP: None
    ---
    apiVersion: apps/v1 # for versions before 1.9.0 use apps/v1beta2
    kind: Deployment
    metadata:
      name: mysql
    spec:
      selector:
        matchLabels:
          app: mysql
      strategy:
        type: Recreate
      template:
        metadata:
          labels:
            app: mysql
        spec:
          containers:
          - image: mysql:5.6
            name: mysql
            env:
              # Use secret in real usage
            - name: MYSQL_ROOT_PASSWORD
              value: password
            ports:
            - containerPort: 3306
              name: mysql
            volumeMounts:
            - name: mysql-persistent-storage
              mountPath: /var/lib/mysql
          volumes:
          - name: mysql-persistent-storage
            persistentVolumeClaim:
              claimName: mysql-pv-claim-sc
    ```
    
2. ' İ kaydettiğiniz klasöre bir dosya olarak kopyalayın ve kaydedin `mysql-pvc.yml` `mysql-deployment.yml` . Uç cihazının ekli bir veri diskinde Azure Stack olan yerleşik StorageClass 'ı kullanmak için, `storageClassName` PVC nesnesinin alanını olarak ayarlayın `ase-node-local` ve accessmodes olmalıdır `ReadWriteOnce` . 

    > [!NOTE] 
    > YAML dosyalarının doğru girintide bulunduğundan emin olun. Doğrulamak ve sonra kaydetmek için [YAML Lint](http://www.yamllint.com/) ile kontrol yapabilirsiniz.
   
    ```yml
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: mysql-pv-claim-sc
    spec:
      storageClassName: ase-node-local
      accessModes:
        - ReadWriteOnce
      resources:
        requests:
          storage: 20Gi
    ```

3. Dosyayı dağıtın `mysql-pvc.yaml` .

    `kubectl apply -f <URI path to the mysql-pv.yml file> -n <your-user-namespace>`
    
    Dağıtımın örnek bir çıktısı aşağıda verilmiştir.

    
    ```powershell
    C:\Users\user>kubectl apply -f "C:\stateful-application\mysql-pvc.yml" -n userns1
    persistentvolumeclaim/mysql-pv-claim-sc created
    C:\Users\user>
    ```
   Oluşturulan PVC 'nin adının burada olduğunu aklınızda yazın `mysql-pv-claim-sc` . Daha sonraki bir adımda kullanacaksınız. 

4. Dosyanın içeriğini dağıtın `mysql-deployment.yml` .

    `kubectl apply -f <URI path to mysql-deployment.yml file> -n <your-user-namespace>`

    Dağıtımın örnek bir çıktısı aşağıda verilmiştir.
    
    ```powershell
    C:\Users\user>kubectl apply -f "C:\stateful-application\mysql-deployment.yml" -n userns1
    service/mysql created
    deployment.apps/mysql created
    C:\Users\user>
    ```
    
5. Dağıtım hakkındaki bilgileri görüntüleyin.

    `kubectl describe deployment <app-label> -n <your-user-namespace>`
    
    ```powershell
    C:\Users\user>kubectl describe deployment mysql -n userns1
    Name:               mysql
    Namespace:          userns1
    CreationTimestamp:  Thu, 20 Aug 2020 11:14:25 -0700
    Labels:             <none>
    Annotations:        deployment.kubernetes.io/revision: 1
                        kubectl.kubernetes.io/last-applied-configuration:
                          {"apiVersion":"apps/v1","kind":"Deployment","metadata":{"annotations":{},"name":"mysql","namespace":"userns1"},"spec":{"selector":{"matchL...
    Selector:           app=mysql
    Replicas:           1 desired | 1 updated | 1 total | 1 available | 0 unavailable
    StrategyType:       Recreate
    MinReadySeconds:    0
    Pod Template:
      Labels:  app=mysql
      Containers:
       mysql:
        Image:      mysql:5.6
        Port:       3306/TCP
        Host Port:  0/TCP
        Environment:
          MYSQL_ROOT_PASSWORD:  password
        Mounts:
          /var/lib/mysql from mysql-persistent-storage (rw)
      Volumes:
       mysql-persistent-storage:
        Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
        ClaimName:  mysql-pv-claim-sc
        ReadOnly:   false
    Conditions:
      Type           Status  Reason
      ----           ------  ------
      Available      True    MinimumReplicasAvailable
      Progressing    True    NewReplicaSetAvailable
    OldReplicaSets:  <none>
    NewReplicaSet:   mysql-695c4d9dcd (1/1 replicas created)
    Events:
      Type    Reason             Age   From                   Message
      ----    ------             ----  ----                   -------
      Normal  ScalingReplicaSet  24s   deployment-controller  Scaled up replica set mysql-695c4d9dcd to 1
    C:\Users\user>
    ```
    

6. Dağıtım tarafından oluşturulan Pod 'leri listeleyin.

    `kubectl get pods -l <app=label> -n <your-user-namespace>`

    Örnek bir çıktı aşağıda verilmiştir.

    
    ```powershell
    C:\Users\user>kubectl get pods -l app=mysql -n userns1
    NAME                     READY   STATUS    RESTARTS   AGE
    mysql-695c4d9dcd-rvzff   1/1     Running   0          40s
    C:\Users\user>
    ```
    
7. PersistentVolumeClaim ' i inceleyin.

    `kubectl describe pvc <your-pvc-name>`

    Örnek bir çıktı aşağıda verilmiştir.

    
    ```powershell
    C:\Users\user>kubectl describe pvc mysql-pv-claim-sc -n userns1
    Name:          mysql-pv-claim-sc
    Namespace:     userns1
    StorageClass:  ase-node-local
    Status:        Bound
    Volume:        pvc-dc48253c-82dc-42a4-a7c6-aaddc97c9b8a
    Labels:        <none>
    Annotations:   kubectl.kubernetes.io/last-applied-configuration:
                     {"apiVersion":"v1","kind":"PersistentVolumeClaim","metadata":{"annotations":{},"name":"mysql-pv-claim-sc","namespace":"userns1"},"spec":{"...
                   pv.kubernetes.io/bind-completed: yes
                   pv.kubernetes.io/bound-by-controller: yes
                   volume.beta.kubernetes.io/storage-provisioner: rancher.io/local-path
                   volume.kubernetes.io/selected-node: k8s-3q7lhq2cl-3q7lhq2
    Finalizers:    [kubernetes.io/pvc-protection]
    Capacity:      20Gi
    Access Modes:  RWO
    VolumeMode:    Filesystem
    Mounted By:    mysql-695c4d9dcd-rvzff
    Events:
      Type    Reason                 Age                From                                                                                                Message
      ----    ------                 ----               ----                                                                                                -------
      Normal  WaitForFirstConsumer   71s (x2 over 77s)  persistentvolume-controller                                                                         waiting for first consumer to be created before binding
      Normal  ExternalProvisioning   62s                persistentvolume-controller                                                                         waiting for a volume to be created, either by external provisioner "rancher.io/local-path" or manually created by system administrator
      Normal  Provisioning           62s                rancher.io/local-path_local-path-provisioner-6b84988bf9-tx8mz_1896d824-f862-4cbf-912a-c8cc0ca05574  External provisioner is provisioning volume for claim "userns1/mysql-pv-claim-sc"
      Normal  ProvisioningSucceeded  60s                rancher.io/local-path_local-path-provisioner-6b84988bf9-tx8mz_1896d824-f862-4cbf-912a-c8cc0ca05574  Successfully provisioned volume pvc-dc48253c-82dc-42a4-a7c6-aaddc97c9b8a
    C:\Users\user>
    ```
    

## <a name="verify-mysql-is-running"></a>MySQL 'in çalıştığını doğrula

Uygulamanın çalıştığını doğrulamak için şunu yazın:

`kubectl exec <your-pod-with-the-app> -i -t -n <your-namespace> -- mysql -p`

İstendiğinde, parolayı girin. Parola `mysql-deployment` dosyanızda bulunur.

Örnek bir çıktı aşağıda verilmiştir.

```powershell
C:\Users\user>kubectl exec mysql-695c4d9dcd-rvzff -i -t -n userns1 -- mysql -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 3
Server version: 5.6.49 MySQL Community Server (GPL)

Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
mysql>
```

## <a name="delete-a-deployment"></a>Bir dağıtımı silme

Dağıtımı silmek için dağıtılan nesneleri ada göre silin. Bu nesneler dağıtım, hizmet ve PVC 'yi içerir.
 
```powershell
kubectl delete deployment <deployment-name>,svc <service-name> -n <your-namespace>
kubectl delete pvc <your-pvc-name> -n <your-namespace>
```

Dağıtımı ve hizmeti sildiğinizde oluşan örnek çıktı aşağıda verilmiştir.

```powershell
C:\Users\user>kubectl delete deployment,svc mysql -n userns1
deployment.apps "mysql" deleted
service "mysql" deleted
C:\Users\user>
```
Burada, PVC 'yi sildiğinizde oluşan örnek çıktı verilmiştir.

```powershell
C:\Users\user>kubectl delete pvc mysql-pv-claim-sc -n userns1
persistentvolumeclaim "mysql-pv-claim-sc" deleted
C:\Users\user>
```                                                                                         


## <a name="next-steps"></a>Sonraki adımlar

Kubectl aracılığıyla ağı yapılandırmayı anlamak için bkz. [Azure Stack Edge cihazında durum bilgisiz bir uygulama dağıtma](azure-stack-edge-gpu-deploy-stateless-application-iot-edge-module.md)
