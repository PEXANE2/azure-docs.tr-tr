---
title: Azure Stack Edge GPU cihazında, Arc etkin Kubernetes üzerinde PHP Konuk defteri uygulaması dağıtma | Microsoft Docs
description: Azure Stack Edge cihazınızın bir yay özellikli bir Kubernetes kümesinde Gile kullanarak, Redis ile PHP Konuk olmayan bir uygulamanın nasıl dağıtılacağını açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 08/25/2020
ms.author: alkohli
ms.openlocfilehash: 7fdd9b8ca0fd62d55f5a9412af9486bfb2b942c1
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89319301"
---
# <a name="deploy-a-php-guestbook-stateless-application-with-redis-on-arc-enabled-kubernetes-cluster-on-azure-stack-edge-gpu"></a>Azure Stack Edge GPU üzerinde Kubernetes kümesi etkin olan Redis ile PHP Konuk olmayan bir uygulaması dağıtma

Bu makalede, Kubernetes ve Azure yay kullanılarak basit, çok katmanlı bir Web uygulaması oluşturma ve dağıtma işlemlerinin nasıl yapılacağı gösterilir. Bu örnek aşağıdaki bileşenlerden oluşur:

- Konuk defteri girişlerini depolamak için tek örnekli Reddir ana
- Okuma hizmeti için birden çok çoğaltılan Redsıs örneği
- Birden çok Web ön uç örneği

Dağıtım, Azure Stack Edge cihazınızdan yay etkinleştirilmiş Kubernetes kümesinde Gilar kullanılarak yapılır. 

Bu yordam, [Azure Stack Edge cihazında Kubernetes iş yüklerini](azure-stack-edge-gpu-kubernetes-workload-management.md) Inceleyen ve [Azure Arc etkinleştirilmiş Kubernetes (Önizleme) kavramlarının kavramlarını öğrendiklerine](https://docs.microsoft.com/azure/azure-arc/kubernetes/overview)yöneliktir.


## <a name="prerequisites"></a>Ön koşullar

Durum bilgisiz uygulamayı dağıtabilmeniz için cihazınızda aşağıdaki önkoşulları ve cihaza erişmek için kullanacağınız istemciyi tamamladığınızdan emin olun:

### <a name="for-device"></a>Cihaz için

1. 1 düğümlü Azure Stack Edge cihazında oturum açma kimlik bilgileriniz var.
    1. Cihaz etkinleştirilir. Bkz. [cihazı etkinleştirme](azure-stack-edge-gpu-deploy-activate.md).
    1. Cihazda Azure portal aracılığıyla yapılandırılmış işlem rolü vardır ve bir Kubernetes kümesi vardır. Bkz. [Işlem yapılandırma](azure-stack-edge-gpu-deploy-configure-compute.md).

1. Cihazınızda mevcut Kubernetes kümesinde Azure Arc 'ı etkinleştirdiniz ve Azure portal buna karşılık gelen bir Azure Arc kaynağınız var. Ayrıntılı adımlar için bkz. [Azure Stack Edge cihazında Azure yayı 'Yi etkinleştirme](azure-stack-edge-gpu-deploy-arc-kubernetes-cluster.md).

### <a name="for-client-accessing-the-device"></a>Cihaza erişen istemci için

1. Azure Stack Edge cihazına erişmek için kullanılacak bir Windows istemci sisteminiz vardır.
  
    - İstemci Windows PowerShell 5,0 veya üstünü çalıştırıyor. Windows PowerShell 'in en son sürümünü indirmek için [Windows PowerShell 'ı yükleme](https://docs.microsoft.com/powershell/scripting/install/installing-windows-powershell?view=powershell-7)bölümüne gidin.
    
    - [Desteklenen bir işletim sistemine](azure-stack-edge-gpu-system-requirements.md#supported-os-for-clients-connected-to-device) sahip başka bir istemciniz de olabilir. Bu makalede, bir Windows istemcisi kullanılırken yordam açıklanmaktadır. 
    
1. [Azure Stack Edge cihazında Kubernetes kümesine erişme](azure-stack-edge-gpu-create-kubernetes-cluster.md)bölümünde açıklanan yordamı tamamladınız. Şunları yapabilirsiniz:
    
    - `kubectl`İstemciye yüklendi  <!--and saved the `kubeconfig` file with the user configuration to C:\\Users\\&lt;username&gt;\\.kube. -->
    
    - `kubectl`İstemci sürümünün Azure Stack Edge cihazında çalışan Kubernetes ana sürümünden daha fazla sürüm olmadığından emin olun. 
      - `kubectl version`İstemci üzerinde çalışan kubectl sürümünü denetlemek için kullanın. Tam sürümü bir yere unutmayın.
      - Azure Stack Edge cihazınızın yerel kullanıcı arabiriminde **Genel Bakış ' a** gidin ve Kubernetes yazılım numarasına göz atın. 
      - Desteklenen Kubernetes sürümünde belirtilen eşlemenin uyumluluk için bu iki sürümü doğrulayın <!--insert link-->.

1. [Azure Arc dağıtımını çalıştırmak için kullanabileceğiniz bir gide yapılandırması](https://github.com/kagoyal/dbehaikudemo)vardır. Bu örnekte, `yaml` Azure Stack Edge cihazınıza dağıtmak için aşağıdaki dosyaları kullanacaksınız.

    - `frontend-deployment.yaml`<!-- - The guestbook application has a web frontend serving the HTTP requests written in PHP. It is configured to connect to the redis-master Service for write requests and the redis-slave service for Read requests. This file describes a deployment that runs the frontend of the guestbook application.-->
    - `frontend-service.yaml` <!-- - This allows you to configure an externally visible frontend Service that can be accessed from outside the Kubernetes cluster on your device.-->
    - `redis-master-deployment.yaml` <!-- - This deployment file runs a single replica Redis master Pod. The guestbook application uses Redis to store its data. It writes its data to a Redis master instance and reads data from multiple Redis slave instances.-->
    - `redis-master-service.yaml` <!-- - The guestbook application needs to communicate to the Redis master to write its data. You need to apply a Service to proxy the traffic to the Redis master Pod. A Service defines a policy to access the Pods.-->
    - `redis-slave-deployment.yaml` <!-- - Although the Redis master is a single pod, you can make it highly available to meet traffic demands by adding replica Redis slaves. The Redis Slave Deployment specifies two replicas.-->
    - `redis-slave-service.yaml` <!-- - The guestbook application needs to communicate to Redis slaves to read data. To make the Redis slaves discoverable, you need to set up a Service. A Service provides transparent load balancing to a set of Pods.-->

<!-- az cli version requirements-->


## <a name="deploy-configuration"></a>Yapılandırmayı dağıtma

Azure yay kaynağını, Azure portal bir Gilar yapılandırması dağıtmak üzere yapılandırmak için aşağıdaki adımları izleyin: 

1. Azure portal, cihazınızdaki Kubernetes kümesinde Azure Arc 'ı etkinleştirdiğinizde oluşturduğunuz Azure Arc kaynağına gidin. 

    ![Azure yay kaynağına git](media/azure-stack-edge-gpu-connect-powershell-interface/verify-azure-arc-enabled-1.png)

1. **Yapılandırmalar** ' a gidin ve **+ yapılandırma Ekle**' yi seçin.

    ![Yapılandırmalara git](media/azure-stack-edge-gpu-connect-powershell-interface/select-configurations-1.png)

1. **Yapılandırma Ekle**' de alanlar için uygun değerleri girin ve **Uygula**' yı seçin.

    |Parametre  |Açıklama |
    |---------|---------|
    |Yapılandırma adı     | Yapılandırma kaynağının adı.        |
    |İşleç örneği adı     |Belirli bir yapılandırmayı tanımlamak için işlecin örnek adı. Ad, yalnızca küçük harf, alfasayısal, kısa çizgi ve nokta olması gereken en fazla 253 karakter dizesidir.         |
    |İşleç ad alanı     | Bu, dağıtımda belirtilen ad alanıyla eşleştiğinden **demotestkonuk defteri** olarak ayarlanır `yaml` . <br> Alan, işlecin yüklendiği ad alanını tanımlar. Ad, yalnızca küçük harf, alfasayısal, kısa çizgi ve nokta olması gereken en fazla 253 karakter dizesidir.         |
    |Depo URL 'SI     |<br>`http://github.com/username/repo`Gila yapılandırmanızın bulunduğu konum veya biçimdeki git deposunun yolu `git://github.com/username/repo` .         |
    |İşleç kapsamı     | **Ad alanı**seçin. <br>Bu, işlecinin yüklendiği kapsamı tanımlar. Bunu ad alanı olarak seçin. Operatörünüz, dağıtım YAML dosyalarında belirtilen ad alanına yüklenecek.       |
    |İşleç türü     | Varsayılan olarak bırakın. <br>Bu, varsayılan olarak, Flox olarak ayarlanan işlecin türünü belirtir.        |
    |İşleç parametreleri     | Bunu boş bırakın. <br>Bu alan Flox işlecine geçirilecek parametreleri içerir.        |
    |Helm     | Bunu **devre dışı**olarak ayarlayın. <br>Grafik tabanlı dağıtımlar yapacaksınız bu seçeneği etkinleştirin.        |


    ![Yapılandırma ekleme](media/azure-stack-edge-gpu-connect-powershell-interface/add-configuration-1.png)


1. Yapılandırma dağıtımı başlar ve **operatör durumu** **Beklemede**olarak gösterilir. 

    ![Yapılandırmalara git](media/azure-stack-edge-gpu-connect-powershell-interface/view-configurations-1.png)

1. Dağıtım birkaç dakika sürer. Dağıtım tamamlandığında, **işleç durumu** **yüklü**olarak gösterilir.

    ![Yapılandırmalara git](media/azure-stack-edge-gpu-connect-powershell-interface/view-configurations-2.png)


## <a name="verify-deployment"></a>Dağıtımı doğrulama

Gilar yapılandırması aracılığıyla dağıtım, `demotestguestbook` Git deposunda bulunan dağıtım dosyalarında belirtilen bir ad alanı oluşturur `yaml` .

1. Gilar yapılandırmasını uyguladıktan sonra, [cihazın PowerShell arabirimine bağlanın](azure-stack-edge-gpu-connect-powershell-interface.md#connect-to-the-powershell-interface).
1. Dağıtıma karşılık gelen ad alanında çalışan Pod 'yi listelemek için aşağıdaki komutu çalıştırın `demotestguestbook` .

    `kubectl get pods -n <your-namespace>`
  
    Örnek bir çıktı aşağıda verilmiştir.
  
    ```powershell
    [10.128.44.240]: PS>kubectl get pods -n demotestguestbook
    NAME                            READY   STATUS    RESTARTS   AGE
    aseoperator1-5569658644-cqtb5   1/1     Running   0          91m
    frontend-6cb7f8bd65-4xb4f       1/1     Running   0          91m
    frontend-6cb7f8bd65-q9cxj       1/1     Running   0          91m
    frontend-6cb7f8bd65-xpzs6       1/1     Running   0          91m
    memcached-86bdf9f56b-5l2fq      1/1     Running   0          91m
    redis-master-7db7f6579f-2z29w   1/1     Running   0          91m
    redis-slave-7664787fbc-lgr2n    1/1     Running   0          91m
    redis-slave-7664787fbc-vlvzn    1/1     Running   0          91m
    [10.128.44.240]: PS>
    ```  

1. Bu örnekte, ön uç hizmeti tür: LoadBalancer olarak dağıtıldı. Konuk defteri 'ni görüntülemek için bu hizmetin IP adresini bulmanız gerekecektir. Aşağıdaki komutu çalıştırın.

    `kubectl get service -n <your-namespace>`
    
    ```powershell
    [10.128.44.240]: PS>kubectl get service -n demotestguestbook
    NAME           TYPE           CLUSTER-IP       EXTERNAL-IP     PORT(S)        AGE
    frontend       LoadBalancer   10.96.79.38      10.128.44.245   80:31238/TCP   85m
    memcached      ClusterIP      10.102.47.75     <none>          11211/TCP      85m
    redis-master   ClusterIP      10.104.32.99     <none>          6379/TCP       85m
    redis-slave    ClusterIP      10.104.215.146   <none>          6379/TCP       85m
    [10.128.44.240]: PS>
    ```
1. Ön uç hizmetinin `type:LoadBalancer` bir dış IP adresi vardır. Bu IP, cihazdaki Bilgi Işlem ağ ayarlarını yapılandırırken dış hizmetler için belirttiğiniz IP adresi aralığıdır. Bu IP adresini, URL 'de Konuk defteri 'ni görüntülemek için kullanın: `https://<external-IP-address>` .

    ![Konuk defteri görüntüle](media/azure-stack-edge-gpu-connect-powershell-interface/view-guestbook-1.png)

## <a name="delete-deployment"></a>Dağıtımı Sil

Dağıtımı silmek için Azure portal yapılandırmayı silebilirsiniz. Bu, dağıtımlar ve hizmetler dahil oluşturulan nesneleri silecektir.

1. Azure portal, Azure Arc kaynak > yapılandırma ' ya gidin. 
1. Silmek istediğiniz yapılandırmayı bulun. Şunu seçin... bağlam menüsünü çağırmak ve **Sil**' i seçin.
    ![Yapılandırmayı Sil](media/azure-stack-edge-gpu-connect-powershell-interface/delete-configuration-1.png)

Yapılandırmanın silinmesi birkaç dakika sürebilir.
 
<!--```powershell
kubectl delete deployment <deployment-name> -n <your-namespace>
kubectl delete service <service-name> -n <your-namespace>
```

Here is a sample output of when you delete the deployments and the services.

```powershell
C:\Users\user>kubectl delete deployment,svc mysql -n userns1
deployment.apps "mysql" deleted
service "mysql" deleted
C:\Users\user>
```-->


## <a name="next-steps"></a>Sonraki adımlar

[Kubernetes panosunu Azure Stack Edge cihazınızdaki dağıtımları izlemek için](azure-stack-edge-gpu-monitor-kubernetes-dashboard.md) nasıl kullanacağınızı öğrenin
