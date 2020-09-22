---
title: Kubernetes 'te Azure SQL Edge kapsayıcısı dağıtma-Azure SQL Edge
description: Kubernetes 'te Azure SQL Edge kapsayıcısı dağıtma hakkında bilgi edinin
keywords: SQL Edge, Container, Kubernetes
services: sql-edge
ms.service: sql-edge
ms.topic: tutorial
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 09/22/2020
ms.openlocfilehash: 16ad757fc00439bb390a7e0dea902901c468dd1c
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90946740"
---
# <a name="deploy-an-azure-sql-edge-container-in-kubernetes"></a>Kubernetes 'te Azure SQL Edge kapsayıcısı dağıtma

Azure SQL Edge bir Kubernetes kümesine, Kubernetes üzerinde çalışan Azure IoT Edge veya tek başına kapsayıcı Pod olarak bir IoT Edge modülü olarak dağıtılabilir. Bu makalenin geri kalanında, bir Kubernetes kümesinde tek başına kapsayıcı dağıtımına odaklanacağız. Kubernetes üzerinde Azure IoT Edge dağıtma hakkında daha fazla bilgi için, [Kubernetes (Önizleme) üzerinde Azure IoT Edge](https://microsoft.github.io/iotedge-k8s-doc/introduction.html)bakın.

Bu öğreticide, bir Kubernetes kümesindeki bir kapsayıcıda yüksek kullanılabilirliğe sahip bir Azure SQL Edge örneğinin nasıl yapılandırılacağı gösterilmektedir. 

> [!div class="checklist"]
> * SA parolası oluşturma
> * Depolama oluşturma
> * Dağıtımı oluşturma
> * SQL Server Management Studio (SSMS) ile bağlanma
> * Hata ve kurtarmayı doğrulama

Kubernetes 1,6 ve üzeri, [Depolama sınıfları](https://kubernetes.io/docs/concepts/storage/storage-classes/), [kalıcı birim talepleri](https://kubernetes.io/docs/concepts/storage/storage-classes/#persistentvolumeclaims)ve [Azure disk birimi türü](https://github.com/kubernetes/examples/tree/master/staging/volumes/azure_disk)için destek içerir. Azure SQL Edge örneklerinizi, Kubernetes 'te yerel olarak oluşturabilir ve yönetebilirsiniz. Bu makaledeki örnekte, paylaşılan disk yük devretme kümesi örneğine benzer yüksek kullanılabilirliğe sahip bir yapılandırmaya ulaşmak üzere bir [dağıtım](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) oluşturma işlemi gösterilmektedir. Bu yapılandırmada Kubernetes, küme Orchestrator rolünü yürütür. Bir kapsayıcıda bir Azure SQL Edge örneği başarısız olduğunda, Orchestrator aynı kalıcı depolamaya bağlanan kapsayıcının başka bir örneğini önyükleme.

![Bir Kubernetes kümesinde Azure SQL Edge](media/deploy-kubernetes/kubernetes-sql-edge.png)

Yukarıdaki diyagramda, `azure-sql-edge` [Pod](https://kubernetes.io/docs/concepts/workloads/pods/pod/)içindeki bir kapsayıcıdır. Kubernetes kümedeki kaynakları düzenler. Bir [çoğaltma kümesi](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/) , bir düğüm hatasından sonra Pod 'un otomatik olarak kurtarılmasını sağlar. Uygulamalar hizmete bağlanır. Bu durumda hizmet, hatasından sonra aynı kalan bir IP adresini barındıran bir yük dengeleyiciyi temsil eder `azure-sql-edge` .

Aşağıdaki diyagramda `azure-sql-edge` kapsayıcı başarısız oldu. Orchestrator olarak Kubernetes, çoğaltma kümesindeki sağlıklı örneklerin doğru sayısını garanti eder ve yapılandırmaya göre yeni bir kapsayıcı başlatır. Orchestrator aynı düğümde yeni bir pod başlatır ve `azure-sql-edge` aynı kalıcı depolamaya yeniden bağlanır. Hizmet yeniden oluşturulmuş olarak bağlanır `azure-sql-edge` .

![Pod sonrasında bir Kubernetes kümesinde Azure SQL Edge hatası](media/deploy-kubernetes/kubernetes-sql-edge-after-pod-fail.png)

Aşağıdaki diyagramda, kapsayıcıyı barındıran düğüm `azure-sql-edge` başarısız oldu. Orchestrator yeni Pod 'u farklı bir düğümde başlatır ve `azure-sql-edge` aynı kalıcı depolamaya yeniden bağlanır. Hizmet yeniden oluşturulmuş olarak bağlanır `azure-sql-edge` .

![Düğüm başarısız olduktan sonra bir Kubernetes kümesinde Azure SQL Edge](media/deploy-kubernetes/kubernetes-sql-edge-after-node-fail.png)

## <a name="prerequisites"></a>Önkoşullar

* **Kubernetes kümesi**
   - Öğretici bir Kubernetes kümesi gerektirir. Bu adımlar, kümeyi yönetmek için [kubectl](https://kubernetes.io/docs/user-guide/kubectl/) kullanır. 

   - Bu öğreticinin amacı doğrultusunda Azure Kubernetes hizmetini kullanarak Azure SQL Edge 'i dağıtacağız. İle AKS 'de tek düğümlü bir Kubernetes kümesi oluşturup bağlamak için bkz. [Azure Kubernetes Service (AKS) kümesi dağıtma](https://docs.microsoft.com/azure/aks/tutorial-kubernetes-deploy-cluster) `kubectl` . 

   >[!NOTE]
   >Düğüm hatasına karşı korunmak için, bir Kubernetes kümesi birden fazla düğüm gerektirir.

* **Azure CLI**
   - Bu öğreticideki yönergeler, Azure CLı 2.10.1 ile onaylanır.

## <a name="create-a-kubernetes-namespace-for-sql-edge-deployment"></a>SQL Edge dağıtımı için Kubernetes ad alanı oluşturma

Kubernetes kümesinde yeni bir ad alanı oluşturun. Bu ad alanı, SQL Edge ve tüm gerekli yapıtları dağıtmak için kullanılacaktır. Kubernetes ad alanları hakkında daha fazla bilgi için bkz. [ad alanları](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/).

   ```azurecli
   kubectl create namespace <namespace name>
   ```  

## <a name="create-an-sa-password"></a>SA parolası oluşturma

Kubernetes kümesinde bir SA parolası oluşturun. Kubernetes [parolalar gibi gizli](https://kubernetes.io/docs/concepts/configuration/secret/)yapılandırma bilgilerini yönetebilir.

Aşağıdaki komut, SA hesabı için bir parola oluşturur:

   ```azurecli
   kubectl create secret generic mssql --from-literal=SA_PASSWORD="MyC0m9l&xP@ssw0rd" -n <namespace name>
   ```  

   `MyC0m9l&xP@ssw0rd`Karmaşık bir parolayla değiştirin.

## <a name="create-storage"></a>Depolama oluşturma

Kubernetes kümesinde [kalıcı bir birim](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) ve [kalıcı birim talebi](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistent-volume-claim-protection) yapılandırın. Aşağıdaki adımları tamamlayın: 

1. Depolama sınıfını ve kalıcı birim talebini tanımlamak için bir bildirim oluşturun.  Bildirim, depolama alanı hazırlayıcısı, parametreleri ve [geri kazanma ilkesini](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#reclaiming)belirtir. Kubernetes kümesi, kalıcı depolamayı oluşturmak için bu bildirimi kullanır. 

   Aşağıdaki YAML örneği, bir depolama sınıfı ve kalıcı birim talebi tanımlar. `azure-disk`Bu Kubernetes kümesi Azure 'da olduğundan depolama sınıfı hazırlayıcı. Depolama hesabı türü `Standard_LRS` . Kalıcı birim talebi olarak adlandırılır `mssql-data` . Kalıcı birim talebi meta verileri, depolama sınıfına geri bağlayan bir ek açıklama içerir. 

   ```yaml
   kind: StorageClass
   apiVersion: storage.k8s.io/v1
   metadata:
        name: azure-disk
   provisioner: kubernetes.io/azure-disk
   parameters:
     storageaccounttype: Standard_LRS
     kind: managed
   ---
   kind: PersistentVolumeClaim
   apiVersion: v1
   metadata:
     name: mssql-data
     annotations:
       volume.beta.kubernetes.io/storage-class: azure-disk
   spec:
     accessModes:
     - ReadWriteOnce
     resources:
       requests:
         storage: 8Gi
   ```

   Dosyayı kaydedin (örneğin, **PVC. YAML**).

2. Kubernetes 'te kalıcı birim talebi oluşturun.

   ```azurecli
   kubectl apply -f <Path to pvc.yaml file> -n <namespace name>
   ```

   `<Path to pvc.yaml file>` , dosyayı kaydettiğiniz konumdur.

   Kalıcı birim otomatik olarak bir Azure depolama hesabı olarak oluşturulur ve kalıcı birim talebine bağlanır. 

    ![Kalıcı birim talebi komutunun ekran görüntüsü](media/deploy-kubernetes/pvc-cmd.png)

3. Kalıcı birim talebini doğrulayın.

   ```azurecli
   kubectl describe pvc <PersistentVolumeClaim>  -n <name of the namespace>
   ```

   `<PersistentVolumeClaim>` , kalıcı birim talebinin adıdır.

   Önceki adımda, kalıcı birim talebi olarak adlandırılmıştır `mssql-data` . Kalıcı birim talebi hakkındaki meta verileri görmek için şu komutu çalıştırın:

   ```azurecli
   kubectl describe pvc mssql-data  -n <namespace name>
   ```

   Döndürülen meta veriler adlı bir değer içerir `Volume` . Bu değer, Blobun adıyla eşlenir.

   ![Birim dahil döndürülen meta verilerin ekran görüntüsü](media/deploy-kubernetes/describe-volume.png)

4. Kalıcı birimi doğrulayın.

   ```azurecli
   kubectl describe pv -n <namespace name>
   ```

   `kubectl` otomatik olarak oluşturulan ve kalıcı birim talebine bağlanan kalıcı birimle ilgili meta verileri döndürür. 

## <a name="create-the-deployment"></a>Dağıtımı oluşturma

Bu örnekte, Azure SQL Edge örneğini barındıran kapsayıcı bir Kubernetes dağıtım nesnesi olarak açıklanmaktadır. Dağıtım bir çoğaltma kümesi oluşturur. Çoğaltma kümesi Pod öğesini oluşturur. 

Bu adımda, Azure SQL Edge Docker görüntüsüne göre kapsayıcıyı açıklamaya yönelik bir bildirim oluşturun. Bildirim, `mssql-data` kalıcı birim talebine ve `mssql` Kubernetes kümesine zaten uyguladığınız gizli dizi öğesine başvurur. Bildirim Ayrıca bir [hizmeti](https://kubernetes.io/docs/concepts/services-networking/service/)açıklar. Bu hizmet bir yük dengeleyicidir. Yük dengeleyici, Azure SQL Edge örneği kurtarıldıktan sonra IP adresinin devam edilmesini sağlar. 

1. Dağıtımı anlatmak için bir bildirim (YAML dosyası) oluşturun. Aşağıdaki örnek, Azure SQL Edge kapsayıcı görüntüsünü temel alan bir kapsayıcı da dahil olmak üzere bir dağıtımı açıklamaktadır.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sqledge-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: sqledge
  template:
    metadata:
      labels:
        app: sqledge
    spec:
      volumes:
        - name: sqldata
          persistentVolumeClaim:
            claimName: mssql-data
      containers:
        - name: azuresqledge
          image: mcr.microsoft.com/azure-sql-edge:latest
          ports:
            - containerPort: 1433
          volumeMounts:
            - name: sqldata
              mountPath: /var/opt/mssql
          env:
            - name: MSSQL_PID
              value: "Developer"
            - name: ACCEPT_EULA
              value: "Y"
            - name: SA_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mssql
                  key: SA_PASSWORD
            - name: MSSQL_AGENT_ENABLED
              value: "TRUE"
            - name: MSSQL_COLLATION
              value: "SQL_Latin1_General_CP1_CI_AS"
            - name: MSSQL_LCID
              value: "1033"
      terminationGracePeriodSeconds: 30
      securityContext:
        fsGroup: 10001
---
apiVersion: v1
kind: Service
metadata:
  name: sqledge-deployment
spec:
  selector:
    app: sqledge
  ports:
    - protocol: TCP
      port: 1433
      targetPort: 1433
      name: sql
  type: LoadBalancer
```

   Önceki kodu adlı yeni bir dosyaya kopyalayın `sqldeployment.yaml` . Aşağıdaki değerleri güncelleştirin: 

   * MSSQL_PID `value: "Developer"` : kapsayıcıyı Azure SQL Edge Developer Edition çalıştıracak şekilde ayarlar. Geliştirici sürümü üretim verileri için lisanslanmaz. Dağıtım üretim kullanımı için ise, sürümünü olarak ayarlayın `Premium` . 

      >[!NOTE]
      >Daha fazla bilgi için bkz. [Azure SQL Edge 'i lisanslayın](https://azure.microsoft.com/pricing/details/sql-edge/).

   * `persistentVolumeClaim`: Bu değer, Bu eşlemeler için `claimName:` kalıcı birim talebi için kullanılan ad için bir giriş gerektirir. Bu öğreticide `mssql-data` kullanılır. 

   * `name: SA_PASSWORD`: Bu bölümde tanımlandığı gibi, SA parolasını ayarlamak için kapsayıcı görüntüsünü yapılandırır.

     ```yaml
     valueFrom:
       secretKeyRef:
         name: mssql
         key: SA_PASSWORD 
     ```

     Kubernetes kapsayıcıyı dağıttığında, parola değerini almak için adlı gizli dizi anlamına gelir `mssql` . 

   >[!NOTE]
   >`LoadBalancer`Hizmet türünü kullanarak, 1433 numaralı bağlantı noktasında Azure SQL Edge örneğine uzaktan erişilebilir (Internet üzerinden).

   Dosyayı kaydedin (örneğin, **sqledgedeploy. YAML**).

2. Dağıtımı oluşturun.

   ```azurecli
   kubectl apply -f <Path to sqledgedeploy.yaml file> -n <namespace name>
   ```

   `<Path to sqldeployment.yaml file>` , dosyayı kaydettiğiniz konumdur.

   ![Dağıtım komutunun ekran görüntüsü](media/deploy-kubernetes/deploy-sql-cmd.png)

   Dağıtım ve hizmet oluşturulur. Azure SQL Edge örneği, kalıcı depolamaya bağlı bir kapsayıcıda bulunur.

   Pod 'un durumunu görüntülemek için, yazın `kubectl get pod -n <namespace name>` .

   ![Pod al komutunun ekran görüntüsü](media/deploy-kubernetes/get-sql-pod-cmd.png)

   Önceki görüntüde Pod 'ın durumu vardır `Running` . Bu durum kapsayıcının hazırlandığını gösterir. Bu işlem birkaç dakika sürebilir.

   >[!NOTE]
   >Dağıtım oluşturulduktan sonra Pod 'ın görünür olması birkaç dakika sürebilir. Gecikme, kümenin Azure SQL Edge kapsayıcı görüntüsünü Docker Hub 'ından çekmesi nedeniyle oluşur. Görüntü ilk kez çekildikten sonra, dağıtım görüntünün üzerine önbelleğe alınmış bir düğüm ise sonraki dağıtımlar daha hızlı olabilir. 

3. Hizmetlerin çalıştığını doğrulayın. Şu komutu çalıştırın:

   ```azurecli
   kubectl get services -n <namespace name>
   ```

   Bu komut, çalıştıran Hizmetleri ve hizmetlerin iç ve dış IP adreslerini döndürür. Hizmetin dış IP adresini aklınızda edin `mssql-deployment` . Azure SQL Edge 'e bağlanmak için bu IP adresini kullanın. 

   ![Hizmeti al komutunun ekran görüntüsü](media/deploy-kubernetes/get-service-cmd.png)

   Kubernetes kümesindeki nesnelerin durumu hakkında daha fazla bilgi için şunu çalıştırın:

   ```azurecli
   az aks browse --resource-group <MyResourceGroup> --name <MyKubernetesClustername>
   ```  

## <a name="connect-to-the-azure-sql-edge-instance"></a>Azure SQL Edge örneğine bağlanma

Kapsayıcıyı açıklandığı şekilde yapılandırdıysanız Azure sanal ağı dışından bir uygulamayla bağlantı oluşturabilirsiniz. `sa`Hizmet için hesabı ve dış IP adresini kullanın. Kubernetes gizli anahtarı olarak yapılandırdığınız parolayı kullanın. Azure SQL Edge örneğine bağlanma hakkında daha fazla bilgi için [Azure SQL Edge 'e bağlanma](connect.md)bölümüne bakın.

## <a name="verify-failure-and-recovery"></a>Hata ve kurtarmayı doğrulama

Hata ve kurtarmayı doğrulamak için pod 'yi silebilirsiniz. Aşağıdaki adımları uygulayın:

1. Azure SQL Edge çalıştıran Pod 'ın listesini listeleyin.

   ```azurecli
   kubectl get pods -n <namespace name>
   ```

   Azure SQL Edge çalıştıran Pod 'ın adını aklınızda edin.

2. Pod öğesini silin.

   ```azurecli
   kubectl delete pod sqledge-deployment-7df66c9999-rc9xl
   ```
   `sqledge-deployment-7df66c9999-rc9xl` , Pod adı için önceki adımdan döndürülen değerdir. 

Kubernetes, bir Azure SQL Edge örneğini kurtarmak ve kalıcı depolamaya bağlanmak için pod 'yi otomatik olarak yeniden oluşturur. `kubectl get pods`Yeni bir pod 'ın dağıtıldığını doğrulamak için kullanın. `kubectl get services`Yeni KAPSAYıCıNıN IP adresinin aynı olduğunu doğrulamak için kullanın. 

## <a name="summary"></a>Özet

Bu öğreticide, yüksek kullanılabilirlik için Azure SQL Edge kapsayıcılarını bir Kubernetes kümesine dağıtmayı öğrendiniz. 

> [!div class="checklist"]
> * SA parolası oluşturma
> * Depolama oluşturma
> * Dağıtımı oluşturma
> * Azure SQL Edge Management Studios (SSMS) ile bağlanma
> * Hata ve kurtarmayı doğrulama

## <a name="next-steps"></a>Sonraki adımlar

- [Kubernetes’e giriş](https://docs.microsoft.com/azure/aks/intro-kubernetes)
- [SQL Edge 'de ONNX ile Machine Learning ve yapay zeka](onnx-overview.md).
- [IoT Edge kullanarak SQL Edge ile uçtan uca IoT çözümü oluşturma](tutorial-deploy-azure-resources.md).
- [Azure SQL Edge 'de veri akışı](stream-data.md)

