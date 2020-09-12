---
title: Windows PowerShell arabirimi aracılığıyla Microsoft Azure Stack Edge cihazını bağlama ve yönetme | Microsoft Docs
description: Windows PowerShell arabirimi aracılığıyla Azure Stack Edge 'e bağlanmayı ve bunu yönetmeyi açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 08/28/2020
ms.author: alkohli
ms.openlocfilehash: b58c38dd0257a65bad6021b6152c14a37f905e0a
ms.sourcegitcommit: 4a7a4af09f881f38fcb4875d89881e4b808b369b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89461842"
---
# <a name="manage-an-azure-stack-edge-gpu-device-via-windows-powershell"></a>Windows PowerShell aracılığıyla Azure Stack Edge GPU cihazını yönetme

Azure Stack Edge çözümü, verileri işlemenize ve ağ üzerinden Azure 'a göndermenize olanak tanır. Bu makalede Azure Stack Edge cihazınız için yapılandırma ve yönetim görevlerinin bazıları açıklanmaktadır. Cihazınızı yönetmek için Azure portal, yerel Web Kullanıcı arabirimini veya Windows PowerShell arabirimini kullanabilirsiniz.

Bu makale, cihazın PowerShell arabirimine ve bu arabirimi kullanarak gerçekleştirebileceğiniz görevlere nasıl bağlanabilirim. 


## <a name="connect-to-the-powershell-interface"></a>PowerShell arabirimine bağlanın

[!INCLUDE [Connect to admin runspace](../../includes/data-box-edge-gateway-connect-minishell.md)]

## <a name="create-a-support-package"></a>Destek paketi oluşturma

[!INCLUDE [Create a support package](../../includes/data-box-edge-gateway-create-support-package.md)]

<!--## Upload certificate

[!INCLUDE [Upload certificate](../../includes/data-box-edge-gateway-upload-certificate.md)]

You can also upload IoT Edge certificates to enable a secure connection between your IoT Edge device and the downstream devices that may connect to it. There are three IoT Edge certificates (*.pem* format) that you need to install:

- Root CA certificate or the owner CA
- Device CA certificate
- Device key certificate

The following example shows the usage of this cmdlet to install IoT Edge certificates:

```
Set-HcsCertificate -Scope IotEdge -RootCACertificateFilePath "\\hcfs\root-ca-cert.pem" -DeviceCertificateFilePath "\\hcfs\device-ca-cert.pem\" -DeviceKeyFilePath "\\hcfs\device-key-cert.pem" -Credential "username"
```
When you run this cmdlet, you will be prompted to provide the password for the network share.

For more information on certificates, go to [Azure IoT Edge certificates](https://docs.microsoft.com/azure/iot-edge/iot-edge-certs) or [Install certificates on a gateway](https://docs.microsoft.com/azure/iot-edge/how-to-create-transparent-gateway).-->

## <a name="view-device-information"></a>Cihaz bilgilerini görüntüle
 
[!INCLUDE [View device information](../../includes/data-box-edge-gateway-view-device-info.md)]

## <a name="view-gpu-driver-information"></a>GPU sürücü bilgilerini görüntüle

Cihazınızda işlem rolü yapılandırılmışsa, PowerShell arabirimi aracılığıyla GPU sürücü bilgilerini de alabilirsiniz. 

1. [PowerShell arabirimine bağlanın](#connect-to-the-powershell-interface).
2. `Get-HcsGpuNvidiaSmi`CIHAZıNıZıN GPU sürücü bilgilerini almak için kullanın.

    Aşağıdaki örnek, bu cmdlet 'in kullanımını gösterir:

    ```powershell
    Get-HcsGpuNvidiaSmi
    ```
    Bu cmdlet 'in örnek çıktısından sürücü bilgilerini bir yere unutmayın.

    ```powershell    
    +-----------------------------------------------------------------------------+    
    | NVIDIA-SMI 440.64.00    Driver Version: 440.64.00    CUDA Version: 10.2     |    
    |-------------------------------+----------------------+----------------------+    
    | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |    
    | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |    
    |===============================+======================+======================|    
    |   0  Tesla T4            On   | 000029CE:00:00.0 Off |                    0 |    
    | N/A   60C    P0    29W /  70W |   1539MiB / 15109MiB |      0%      Default |    
    +-------------------------------+----------------------+----------------------+    
    |   1  Tesla T4           On  | 0000AD50:00:00.0 Off |                    0 |
    | N/A   58C    P0    29W /  70W |    330MiB / 15109MiB |      0%      Default |
    +-------------------------------+----------------------+----------------------+
    ```

## <a name="enable-multi-process-service-mps"></a>Çok Işlem hizmeti 'ni (MPS) etkinleştir

NVIDIA GPU 'larda çoklu Işlem hizmeti (MPS), her bir işin GPU kaynaklarının bir yüzdesi ayrıldığı birden çok iş tarafından paylaşılabilen bir mekanizma sağlar. Azure Stack Edge cihazınızda MPS 'yi etkinleştirmek için şu adımları izleyin:

1. Başlamadan önce şunları yaptığınızdan emin olun: 

    1. [Azure Stack Edge cihazınızı](azure-stack-edge-gpu-deploy-activate.md) Azure 'Da Azure Stack edge/Data Box Gateway kaynağıyla yapılandırdık ve etkinleştirdiniz.
    1. [Bu cihazda işlem Azure Portal yapılandırdınız](azure-stack-edge-deploy-configure-compute.md#configure-compute).
    
1. [PowerShell arabirimine bağlanın](#connect-to-the-powershell-interface).
1. Cihazınızda MPS 'yi etkinleştirmek için aşağıdaki komutu kullanın.

    ```powershell
    Start-HcsGpuMPS
    ```

## <a name="reset-your-device"></a>Cihazınızı sıfırlama

[!INCLUDE [Reset your device](../../includes/data-box-edge-gateway-deactivate-device.md)]

## <a name="get-compute-logs"></a>İşlem günlüklerini al

İşlem rolü cihazınızda yapılandırılmışsa, işlem günlüklerini PowerShell arabirimi aracılığıyla da alabilirsiniz.

1. [PowerShell arabirimine bağlanın](#connect-to-the-powershell-interface).
2. `Get-AzureDataBoxEdgeComputeRoleLogs`Cihazınızın işlem günlüklerini almak için kullanın.

    Aşağıdaki örnek, bu cmdlet 'in kullanımını gösterir:

    ```powershell
    Get-AzureDataBoxEdgeComputeRoleLogs -Path "\\hcsfs\logs\myacct" -Credential "username" -FullLogCollection    
    ```

    Cmdlet 'i için kullanılan parametrelerin açıklaması aşağıdadır:
    - `Path`: İşlem günlüğü paketini oluşturmak istediğiniz paylaşıma yönelik bir ağ yolu sağlayın.
    - `Credential`: Ağ paylaşımının Kullanıcı adını sağlayın. Bu cmdlet 'i çalıştırdığınızda, paylaşma parolasını sağlamanız gerekir.
    - `FullLogCollection`: Bu parametre, günlük paketinin tüm işlem günlüklerini içermesini sağlar. Varsayılan olarak, günlük paketi yalnızca bir Günlükler alt kümesi içerir.


## <a name="change-kubernetes-pod-and-service-subnets"></a>Kubernetes Pod ve hizmet alt ağlarını değiştirme

Azure Stack Edge cihazındaki Kubernetes varsayılan olarak, sırasıyla Pod ve hizmet için 172.27.0.0/16 ve 172.28.0.0/16 alt ağlarını kullanır. Bu alt ağlar ağınızda zaten kullanılıyorsa, `Set-HcsKubeClusterNetworkInfo` Bu alt ağları değiştirmek için cmdlet 'ini çalıştırabilirsiniz.

Bu adımda, Kubernetes kümesi oluşturulduğundan Azure portal işlem yapılandırmadan önce bu yapılandırmayı gerçekleştirmek istiyorsunuz.

1. Cihazın PowerShell arabirimine bağlanın.
1. Cihazın PowerShell arabiriminden şunu çalıştırın:

    `Set-HcsKubeClusterNetworkInfo -PodSubnet <subnet details> -ServiceSubnet <subnet details>`

    Öğesini <subnet details> kullanmak istediğiniz alt ağ aralığı ile değiştirin. 

1. Bu komutu çalıştırdığınızda, `Get-HcsKubeClusterNetworkInfo` Pod ve hizmet alt ağlarının değiştiğini doğrulamak için komutunu kullanabilirsiniz.

Bu komut için örnek bir çıktı aşağıda verilmiştir.

```powershell
[10.100.10.10]: PS>Set-HcsKubeClusterNetworkInfo -PodSubnet 10.96.0.1/16 -ServiceSubnet 10.97.0.1/16
[10.100.10.10]: PS>Get-HcsKubeClusterNetworkInfo

Id                                   PodSubnet    ServiceSubnet
--                                   ---------    -------------
6dbf23c3-f146-4d57-bdfc-76cad714cfd1 10.96.0.1/16 10.97.0.1/16
[10.100.10.10]: PS>
```


## <a name="debug-kubernetes-issues-related-to-iot-edge"></a>IoT Edge ilgili Kubernetes sorunlarını ayıklama

<!--When the Kubernetes cluster is created, there are two system namespaces created: `iotedge` and `azure-arc`. --> 

<!--### Create config file for system namespace

To troubleshoot, first create the `config` file corresponding to the `iotedge` namespace with `aseuser`.

Run the `Get-HcsKubernetesUserConfig -AseUser` command and save the output as `config` file (no file extension). Save the file in the `.kube` folder of your user profile on the local machine.

Following is the sample output of the `Get-HcsKubernetesUserConfig` command.

```PowerShell
[10.100.10.10]: PS>Get-HcsKubernetesUserConfig -AseUser
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUN5RENDQWJDZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwcmRXSmwKY201bGRHVnpNQjRYRFRJd01EVXhNekl4TkRRME5sb1hEVE13TURVeE1USXhORFEwTmxvd0ZURVRNQkVHQTFVRQpBeE1LYTNWaVpYSnVaWFJsY3pDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVBBRENDQVFvQ2dnRUJBS0M1CjlJbzRSU2hudG90QUdxdjNTYmRjOVd4UmJDYlRzWXU5S0RQeU9xanVoZE1UUE9PcmROOGNoa0x4NEFyZkZaU1AKZithUmhpdWZqSE56bWhucnkvZlprRGdqQzQzRmV5UHZzcTZXeVVDV0FEK2JBdi9wSkJDbkg2MldoWGNLZ1BVMApqU1k0ZkpXenNFbzBaREhoeUszSGN3MkxkbmdmaEpEanBQRFJBNkRWb2pIaktPb29OT1J1dURvUHpiOTg2dGhUCkZaQXJMZjRvZXRzTEk1ZzFYRTNzZzM1YVhyU0g3N2JPYVVsTGpYTzFYSnpFZlZWZ3BMWE5xR1ZqTXhBMVU2b1MKMXVJL0d1K1ArY
===========CUT=========================================CUT===================
    server: https://compute.myasegpu1.wdshcsso.com:6443
    name: kubernetes
contexts:
- context:
    cluster: kubernetes
    user: aseuser
    name: aseuser@kubernetes
current-context: aseuser@kubernetes
kind: Config
preferences: {}
users:
- name: aseuser
    user:
    client-certificate-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUMwRENDQWJpZ0F3SUJBZ0lJY1hOTXRPU2VwbG93RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhNS2EzVmlaWEp1WlhSbGN6QWVGdzB5TURBMU1UTXlNVFEwTkRaYUZ3MHlNVEExTVRNeU1UVXhNVEphTUJJeApFREFPQmdOVkJBTVRCMkZ6WlhWelpYSXdnZ0VpTUEwR0NTcUdTSWIzRFFFQkFRVUFBNElCRHdBd2dnRUtBb0lCCkFRRHVjQ1pKdm9qNFIrc0U3a1EyYmVjNEJkTXdpUEhmU2R2WnNDVVY0aTRRZGY1Yzd0dkE3OVRSZkRLQTY1d08Kd0h0QWdlK3lLK0hIQ1Qyd09RbWtNek1RNjZwVFEzUlE0eVdtRDZHR1cWZWMExBR1hFUUxWWHRuTUdGCi0tLS0tRU5EIFJTQSBQUklWQVRFIEtFWS0tLS0tCg==

[10.100.10.10]: PS>
```
-->

İşlem rolü yapılandırılmış bir Azure Stack Edge cihazında, iki farklı komut kümesi kullanarak cihazı sorun gidermeye veya izlemeye izleyebilirsiniz.

- `iotedge`Komutları kullanma. Bu komutlar, cihazınız için temel işlemler için kullanılabilir.
- `kubectl`Komutları kullanma. Bu komutlar, cihazınız için kapsamlı bir işlem kümesi için kullanılabilir.

Yukarıdaki komut kümesinden birini yürütmek için [PowerShell arabirimine bağlanmanız](#connect-to-the-powershell-interface)gerekir.

### <a name="use-iotedge-commands"></a>`iotedge`Komutları kullanma

Kullanılabilir komutların listesini görmek için [PowerShell arabirimine bağlanın](#connect-to-the-powershell-interface) ve `iotedge` işlevini kullanın.

```powershell
[10.100.10.10]: PS>iotedge -?                                                                                                                           
Usage: iotedge COMMAND

Commands:
   list
   logs
   restart

[10.100.10.10]: PS>
```

Aşağıdaki tabloda, için kullanılabilen komutların kısa bir açıklaması verilmiştir `iotedge` :

|command  |Description |
|---------|---------|
|`list`     | Modülleri listeleme         |
|`logs`     | Modülün günlüklerini getirme        |
|`restart`     | Modülü durdurma ve yeniden başlatma         |


Cihazınızda çalışan tüm modülleri listelemek için `iotedge list` komutunu kullanın.

Bu komutun örnek bir çıktısı aşağıda verilmiştir. Bu komut tüm modülleri, ilişkili yapılandırmayı ve modüllerle ilişkili dış IP 'Leri listeler. Örneğin, Web **sunucusu** uygulamasına adresinden erişebilirsiniz `https://10.128.44.244` . 


```powershell
[10.100.10.10]: PS>iotedge list

NAME                   STATUS  DESCRIPTION CONFIG                                             EXTERNAL-IP
----                   ------  ----------- ------                                             -----
gettingstartedwithgpus Running Up 10 days  mcr.microsoft.com/intelligentedge/solutions:latest
iotedged               Running Up 10 days  azureiotedge/azureiotedge-iotedged:0.1.0-beta10    <none>
edgehub                Running Up 10 days  mcr.microsoft.com/azureiotedge-hub:1.0             10.128.44.243
edgeagent              Running Up 10 days  azureiotedge/azureiotedge-agent:0.1.0-beta10
webserverapp           Running Up 10 days  nginx:stable                                       10.128.44.244

[10.100.10.10]: PS>
```


### <a name="use-kubectl-commands"></a>Kubectl komutlarını kullanma

İşlem rolü yapılandırılmış bir Azure Stack Edge cihazında, `kubectl` modülleri izlemek veya sorunlarını gidermek için tüm komutlar kullanılabilir. Kullanılabilir komutların bir listesini görmek için `kubectl --help` komut penceresinden komutunu çalıştırın.

```PowerShell
C:\Users\myuser>kubectl --help

kubectl controls the Kubernetes cluster manager.

Find more information at: https://kubernetes.io/docs/reference/kubectl/overview/

Basic Commands (Beginner):
    create         Create a resource from a file or from stdin.
    expose         Take a replication controller, service, deployment or pod and expose it as a new Kubernetes Service
    run            Run a particular image on the cluster
    set            Set specific features on objects
    run-container  Run a particular image on the cluster. This command is deprecated, use "run" instead
==============CUT=============CUT============CUT========================

Usage:
    kubectl [flags] [options]

Use "kubectl <command> --help" for more information about a given command.
Use "kubectl options" for a list of global command-line options (applies to all commands).

C:\Users\myuser>
```

Komutların kapsamlı bir listesi için `kubectl` [ `kubectl` yazdırılabilecek](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)sayfasına gidin.


#### <a name="to-get-ip-of-service-or-module-exposed-outside-of-kubernetes-cluster"></a>Kubernetes kümesi dışında kullanıma sunulan hizmet veya modülün IP 'yi almak için

Kubernetes dışında sunulan bir yük dengeleme hizmetinin veya modüllerinin IP 'sini almak için aşağıdaki komutu çalıştırın:

`kubectl get svc -n iotedge`

Aşağıda, Kubernetes kümesi dışında kullanıma sunulan tüm hizmetlerin veya modüllerin bir örnek çıktısı verilmiştir. 


```powershell
[10.100.10.10]: PS>kubectl get svc -n iotedge
NAME           TYPE           CLUSTER-IP      EXTERNAL-IP     PORT(S)                                       AGE
edgehub        LoadBalancer   10.103.52.225   10.128.44.243   443:31987/TCP,5671:32336/TCP,8883:30618/TCP   34h
iotedged       ClusterIP      10.107.236.20   <none>          35000/TCP,35001/TCP                           3d8h
webserverapp   LoadBalancer   10.105.186.35   10.128.44.244   8080:30976/TCP                                16h

[10.100.10.10]: PS>
```
Dış IP sütunundaki IP adresi, hizmetin veya modülün dış uç noktasına karşılık gelir. Ayrıca, [Kubernetes panosundaki dış IP 'yi de alabilirsiniz](azure-stack-edge-gpu-monitor-kubernetes-dashboard.md#get-ip-address-for-services-or-modules).


#### <a name="to-check-if-module-deployed-successfully"></a>Modülün başarıyla dağıtılıp dağıtılmadığını denetlemek için

İşlem modülleri, iş mantığı uygulanmış kapsayıcılardır. Bir Kubernetes Pod çalıştıran birden çok kapsayıcı olabilir. 

Bir işlem modülünün başarıyla dağıtılıp dağıtılmadığını denetlemek için cihazın PowerShell arabirimine bağlanın.
Komutunu çalıştırın `get pods` ve kapsayıcının (işlem modülüne karşılık gelen) çalışıp çalışmadığını denetleyin.

Belirli bir ad alanında çalışan tüm yığınların listesini almak için aşağıdaki komutu çalıştırın:

`get pods -n <namespace>`

IoT Edge aracılığıyla dağıtılan modülleri denetlemek için şu komutu çalıştırın:

`get pods -n iotedge`

Aşağıda, ad alanında çalışan tüm yığınların örnek bir çıktısı verilmiştir `iotedge` .

```
[10.100.10.10]: PS>kubectl get pods -n iotedge
NAME                        READY   STATUS    RESTARTS   AGE
edgeagent-cf6d4ffd4-q5l2k   2/2     Running   0          20h
edgehub-8c9dc8788-2mvwv     2/2     Running   0          56m
filemove-66c49984b7-h8lxc   2/2     Running   0          56m
iotedged-675d7f4b5f-9nml4   1/1     Running   0          20h

[10.100.10.10]: PS>
```

Durum **durumu** , ad alanındaki tüm yığınların çalıştığını ve **Ready** öğesinin bir pod içinde dağıtılan kapsayıcıların sayısını belirtir. Yukarıdaki örnekte, tüm yığınların çalışıyor olması ve her bir yığından dağıtılan tüm modüllerin çalışıyor olması gerekir. 

Azure Arc aracılığıyla dağıtılan modülleri denetlemek için şu komutu çalıştırın:

`get pods -n azure-arc`

Alternatif olarak, [IoT Edge veya Azure Arc dağıtımlarını görmek Için Kubernetes panosuna bağlanabilirsiniz](azure-stack-edge-gpu-monitor-kubernetes-dashboard.md#view-module-status).


Belirli bir ad alanı için belirli bir pod 'ın daha ayrıntılı bir çıktısı için aşağıdaki komutu çalıştırabilirsiniz:

`kubectl describe pod <pod name> -n <namespace>` 

Örnek çıktı burada gösterilmiştir.

```
[10.100.10.10]: PS>kubectl describe pod filemove-66c49984b7 -n iotedge
Name:           filemove-66c49984b7-h8lxc
Namespace:      iotedge
Priority:       0
Node:           k8s-1hwf613cl-1hwf613/10.139.218.12
Start Time:     Thu, 14 May 2020 12:46:28 -0700
Labels:         net.azure-devices.edge.deviceid=myasegpu-edge
                net.azure-devices.edge.hub=myasegpu2iothub.azure-devices.net
                net.azure-devices.edge.module=filemove
                pod-template-hash=66c49984b7
Annotations:    net.azure-devices.edge.original-moduleid: filemove
Status:         Running
IP:             172.17.75.81
IPs:            <none>
Controlled By:  ReplicaSet/filemove-66c49984b7
Containers:
    proxy:
    Container ID:   docker://fd7975ca78209a633a1f314631042a0892a833b7e942db2e7708b41f03e8daaf
    Image:          azureiotedge/azureiotedge-proxy:0.1.0-beta8
    Image ID:       docker://sha256:5efbf6238f13d24bab9a2b499e5e05bc0c33ab1587d6cf6f289cdbe7aa667563
    Port:           <none>
    Host Port:      <none>
    State:          Running
        Started:      Thu, 14 May 2020 12:46:30 -0700
    Ready:          True
    Restart Count:  0
    Environment:
        PROXY_LOG:  Debug
=============CUT===============================CUT===========================
Volumes:
    config-volume:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      iotedged-proxy-config
    Optional:  false
    trust-bundle-volume:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      iotedged-proxy-trust-bundle
    Optional:  false
    myasesmb1local:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  myasesmb1local
    ReadOnly:   false
    myasesmb1:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  myasesmb1
    ReadOnly:   false
    filemove-token-pzvw8:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  filemove-token-pzvw8
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                    node.kubernetes.io/unreachable:NoExecute for 300s
Events:          <none>


[10.100.10.10]: PS>
```

#### <a name="to-get-container-logs"></a>Kapsayıcı günlüklerini almak için

Bir modülün günlüklerini almak için, cihazın PowerShell arabiriminden aşağıdaki komutu çalıştırın:

`kubectl logs <pod_name> -n <namespace> --all-containers` 

`all-containers`Bayrak tüm kapsayıcıların tüm günlüklerinin dökümünü alacak, son hataları görmenin iyi bir yolu ise seçeneğini kullanmaktır `--tail 10` .

Aşağıda örnek bir çıktı verilmiştir. 

```
[10.100.10.10]: PS>kubectl logs filemove-66c49984b7-h8lxc -n iotedge --all-containers --tail 10
DEBUG 2020-05-14T20:40:42Z: loop process - 0 events, 0.000s
DEBUG 2020-05-14T20:40:44Z: loop process - 0 events, 0.000s
DEBUG 2020-05-14T20:40:44Z: loop process - 0 events, 0.000s
DEBUG 2020-05-14T20:40:44Z: loop process - 1 events, 0.000s
DEBUG 2020-05-14T20:40:44Z: loop process - 0 events, 0.000s
DEBUG 2020-05-14T20:42:12Z: loop process - 0 events, 0.000s
DEBUG 2020-05-14T20:42:14Z: loop process - 0 events, 0.000s
DEBUG 2020-05-14T20:42:14Z: loop process - 0 events, 0.000s
DEBUG 2020-05-14T20:42:14Z: loop process - 1 events, 0.000s
DEBUG 2020-05-14T20:42:14Z: loop process - 0 events, 0.000s
05/14/2020 19:46:44: Info: Opening module client connection.
05/14/2020 19:46:45: Info: Open done.
05/14/2020 19:46:45: Info: Initializing with input: /home/input, output: /home/output, protocol: Amqp.
05/14/2020 19:46:45: Info: IoT Hub module client initialized.

[10.100.10.10]: PS>
```



## <a name="exit-the-remote-session"></a>Uzak oturumdan çık

Uzak PowerShell oturumundan çıkmak için PowerShell penceresini kapatın.

## <a name="next-steps"></a>Sonraki adımlar

- Azure portal [Azure Stack Edge](azure-stack-edge-gpu-deploy-prep.md) dağıtın.
