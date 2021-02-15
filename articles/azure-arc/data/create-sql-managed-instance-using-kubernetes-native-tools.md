---
title: Kubernetes araçlarını kullanarak bir SQL yönetilen örneği oluşturma
description: Kubernetes araçlarını kullanarak bir SQL yönetilen örneği oluşturma
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: vin-yu
ms.author: vinsonyu
ms.reviewer: mikeray
ms.date: 02/11/2021
ms.topic: how-to
ms.openlocfilehash: cade888d951c2071f8f40c145e28eed3c3a5d27c
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100384257"
---
# <a name="create-azure-sql-managed-instance-using-kubernetes-tools"></a>Kubernetes araçlarını kullanarak Azure SQL yönetilen örneği oluşturma

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="prerequisites"></a>Önkoşullar

Zaten bir [Azure Arc veri denetleyicisi](./create-data-controller.md)oluşturmuş olmanız gerekir.

Kubernetes araçlarını kullanarak bir SQL yönetilen örneği oluşturmak için Kubernetes araçları 'nın yüklü olması gerekir.  Bu makaledeki örneklerde kullanılacak `kubectl` , ancak benzer yaklaşımlar, Kubernetes panosu gibi diğer Kubernetes araçlarıyla `oc` veya `helm` Bu araçlar ve Kubernetes YAML/JSON hakkında bilginiz varsa kullanılabilir.

[Kubectl aracını kurma](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

## <a name="overview"></a>Genel Bakış

Bir SQL yönetilen örneği oluşturmak için, sistem yöneticinizin oturum açma ve parolasını güvenli bir şekilde depolamak üzere bir Kubernetes gizli anahtarı ve sqlmanagedınstance özel kaynak tanımına dayalı olarak bir SQL yönetilen örnek özel kaynağı oluşturmanız gerekir.

## <a name="create-a-yaml-file"></a>YAML dosyası oluşturma

Kendi özel SQL yönetilen örneğiniz YAML Dosyanızı oluşturmak için bir başlangıç noktası olarak, [YAML dosyasını şablon](https://raw.githubusercontent.com/microsoft/azure_arc/main/arc_data_services/deploy/yaml/sqlmi.yaml) olarak kullanabilirsiniz.  Bu dosyayı yerel bilgisayarınıza indirip bir metin düzenleyicisinde açın.  YAML dosyaları için söz dizimi vurgulamasını destekleyen [vs Code](https://code.visualstudio.com/download) gibi bir metin Düzenleyicisi kullanmak faydalıdır.

Bu örnek bir YAML dosyasıdır:

```yaml
apiVersion: v1
data:
  password: <your base64 encoded password>
  username: <your base64 encoded user name. 'sa' is not allowed>
kind: Secret
metadata:
  name: sql1-login-secret
type: Opaque
---
apiVersion: sql.arcdata.microsoft.com/v1alpha1
kind: sqlmanagedinstance
metadata:
  name: sql1
spec:
  limits:
    memory: 4Gi
    vcores: "4"
  requests:
    memory: 2Gi
    vcores: "1"
  service:
    type: LoadBalancer
  storage:
    data:
      className: default
      size: 5Gi
    logs:
      className: default
      size: 1Gi
```

### <a name="customizing-the-login-and-password"></a>Oturum açma ve parolayı özelleştirme

Bir Kubernetes parolası, Kullanıcı adı ve diğeri parola için Base64 kodlamalı dize olarak depolanır.  Bir Sistem Yöneticisi oturum açma ve parola kodlamanız ve bunları ve yer tutucu konumuna yerleştirmeniz gerekir `data.password` `data.username` .  `<` `>` Şablonda sunulan ve sembollerini eklemeyin.

> [!NOTE]
> En iyi güvenlik için, ' sa ' değerini kullanarak oturum açma için izin verilmez.
> [Parola karmaşıklığı ilkesini](/sql/relational-databases/security/password-policy#password-complexity)izleyin.

İstediğiniz kullanıcı adınızı ve parolanızı Base64 olarak kodlamak için çevrimiçi bir araç kullanabilir veya platformunuza bağlı olarak yerleşik CLı araçlarını kullanabilirsiniz.

PowerShell

```console
[Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes('<your string to encode here>'))

#Example
#[Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes('example'))

```

Linux/macOS

```console
echo '<your string to encode here>' | base64

#Example
# echo 'example' | base64
```

### <a name="customizing-the-name"></a>Adı özelleştirme

Şablon, Name özniteliği için ' SQL1 ' değerine sahip.  Bunu değiştirebilirsiniz, ancak DNS adlandırma standartlarını izleyen karakterler olmalıdır.  Ayrıca, eşleştirilecek gizli anahtar adını da değiştirmelisiniz.  Örneğin, SQL yönetilen örneğinin adını ' SQL2 ' olarak değiştirirseniz, gizli anahtar adını ' SQL1-Login-Secret ' iken ' SQL2-Login-Secret ' olarak değiştirmeniz gerekir

### <a name="customizing-the-resource-requirements"></a>Kaynak gereksinimlerini özelleştirme

Kaynak gereksinimlerini (RAM ve çekirdek sınırları ve istekleri) gerektiği gibi değiştirebilirsiniz.  

> [!NOTE]
> [Kubernetes Resource idare](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#resource-units-in-kubernetes)hakkında daha fazla bilgi edinebilirsiniz.

Kaynak sınırları ve istekleri için gereksinimler:
- Fatura amaçları için çekirdek sınırı değeri **gereklidir** .
- Kaynak isteklerinin ve limitlerin geri kalanı isteğe bağlıdır.
- Çekirdek sınırı ve isteği belirtilmişse pozitif bir tamsayı değeri olmalıdır.
- Belirtilmişse, çekirdekler isteği için en az 2 çekirdek gereklidir.
- Bellek değeri biçimi Kubernetes gösterimini izler.  
- Belirtilmişse bellek isteği için en az 2Gı gereklidir.
- Genel bir kılavuz olarak, üretim kullanım örnekleri için her 1 çekirdek için 4 GB RAM olmalıdır.

### <a name="customizing-service-type"></a>Hizmet türünü özelleştirme

İstenirse, hizmet türü NodePort olarak değiştirilebilir.  Rastgele bir bağlantı noktası numarası atanacaktır.

### <a name="customizing-storage"></a>Depolamayı özelleştirme

Depolama sınıflarını, ortamınızda eşleşecek şekilde özelleştirebilirsiniz.  Hangi depolama sınıflarının kullanılabilir olduğundan emin değilseniz, `kubectl get storageclass` bunları görüntülemek için komutunu çalıştırabilirsiniz.  Şablonun ' default ' varsayılan değeri vardır.  Bu, varsayılan _olarak_ ' default ' _adlı_ bir depolama sınıfı olduğu anlamına gelir.  Ayrıca, isteğe bağlı olarak depolamanın boyutunu değiştirebilirsiniz.  [Depolama yapılandırması](./storage-configuration.md)hakkında daha fazla bilgi edinebilirsiniz.

## <a name="creating-the-sql-managed-instance"></a>SQL yönetilen örneği oluşturma

SQL yönetilen örnek YAML dosyasını özelleştirmiş olduğunuza göre, aşağıdaki komutu çalıştırarak SQL yönetilen örneğini oluşturabilirsiniz:

```console
kubectl create -n <your target namespace> -f <path to your yaml file>

#Example
#kubectl create -n arc -f C:\arc-data-services\sqlmi.yaml
```


## <a name="monitoring-the-creation-status"></a>Oluşturma durumunu izleme

SQL yönetilen örneği oluşturma işleminin tamamlanması birkaç dakika sürer. İlerleme durumunu başka bir Terminal penceresinde aşağıdaki komutlarla izleyebilirsiniz:

> [!NOTE]
>  Aşağıdaki örnek komutlarda, ' Arc ' adlı ' SQL1 ' ve Kubernetes ad alanı adında bir SQL yönetilen örneği oluşturduğunuzu varsayalım.  Farklı bir ad alanı/SQL yönetilen örnek adı kullandıysanız, ' yay ' ve ' sqlmi ' değerini adlarınızla değiştirebilirsiniz.

```console
kubectl get sqlmi/sql1 --namespace arc
```

```console
kubectl get pods --namespace arc
```

Ayrıca, aşağıdaki gibi bir komut çalıştırarak belirli bir pod 'un oluşturma durumunu da denetleyebilirsiniz.  Bu, özellikle herhangi bir sorunu gidermek için kullanışlıdır.

```console
kubectl describe po/<pod name> --namespace arc

#Example:
#kubectl describe po/sql1-0 --namespace arc
```

## <a name="troubleshooting-creation-problems"></a>Oluşturma sorunlarını giderme

Oluşturma ile herhangi bir sorun yaşarsanız, lütfen [sorun giderme kılavuzuna](troubleshoot-guide.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Arc etkin SQL yönetilen örneğine bağlanma](connect-managed-instance.md)
