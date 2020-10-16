---
title: Azure Arc 'da PostgreSQL için Azure veritabanı hiper ölçek sunucu grubu oluşturma
description: Azure Arc 'da PostgreSQL için Azure veritabanı hiper ölçek sunucu grubu oluşturma
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 7fdc367e3db298b60dc9a15453d58a738c13274a
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92108312"
---
# <a name="create-an-azure-arc-enabled-postgresql-hyperscale-server-group"></a>Azure Arc özellikli PostgreSQL Hiper Ölçek sunucu grubu oluşturma

Bu belgede, Azure Arc üzerinde bir PostgreSQL hiper ölçek sunucu grubu oluşturma adımları açıklanmaktadır.

[!INCLUDE [azure-arc-common-prerequisites](../../../includes/azure-arc-common-prerequisites.md)]

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="getting-started"></a>Başlarken
Aşağıdaki konularda zaten bilgi sahibiyseniz bu paragrafı atlayabilirsiniz.
Oluşturmaya devam etmeden önce, okumak isteyebileceğiniz önemli konular vardır:
- [Azure Arc etkin veri Hizmetleri 'ne genel bakış](overview.md)
- [Bağlantı modları ve gereksinimler](connectivity.md)
- [Depolama yapılandırması ve Kubernetes depolama kavramları](storage-configuration.md)
- [Kubernetes kaynak modeli](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/scheduling/resources.md#resource-quantities)

Bir tam ortamı sağlamadan bir şeyler yapmayı tercih ediyorsanız, Azure Kubernetes Service (AKS), AWS elastik Kubernetes Service (EKS), Google Cloud Kubernetes Engine (GKE) veya bir Azure VM 'de [Azure Arc atlamalı](https://github.com/microsoft/azure_arc#azure-arc-enabled-data-services) ile hızlı bir şekilde çalışmaya başlayabilirsiniz.


## <a name="login-to-the-azure-arc-data-controller"></a>Azure Arc veri denetleyicisinde oturum açma

Bir örnek oluşturabilmeniz için önce Azure Arc veri denetleyicisinde oturum açmalısınız. Veri denetleyicisinde zaten oturum açtıysanız, bu adımı atlayabilirsiniz.

```console
azdata login
```

Ardından Kullanıcı adı, parola ve sistem ad alanı sorulur.  

> Veri denetleyicisi oluşturmak için betiği kullandıysanız, ad alanınız **yay** olmalıdır

```console
Namespace: arc
Username: arcadmin
Password:
Logged in successfully to `https://10.0.0.4:30080` in namespace `arc`. Setting active context to `arc`
```

## <a name="preliminary-and-temporary-step-for-openshift-users-only"></a>Yalnızca OpenShift kullanıcıları için ön ve geçici adım

Sonraki adıma geçmeden önce bu adımı uygulayın. Varsayılan dışında bir projede PostgreSQL hiper ölçek sunucu grubunu Red Hat OpenShift 'e dağıtmak için, güvenlik kısıtlamalarını güncelleştirmek üzere kümenizde aşağıdaki komutları yürütmeniz gerekir. Bu komut, PostgreSQL hiper ölçek sunucu grubunuzu çalıştıracak hizmet hesaplarına gerekli ayrıcalıkları verir. Güvenlik bağlamı kısıtlaması (SCC) **_Arc-Data-SCC_** , Azure Arc veri denetleyicisi 'ni dağıtırken eklediğiniz bir.

```console
oc adm policy add-scc-to-user arc-data-scc -z <server-group-name> -n <namespace name>
```

_**Sunucu-grup adı** , bir sonraki adımda oluşturacağınız sunucu grubunun adıdır._
   
OpenShift 'te SCCs hakkında daha fazla ayrıntı için lütfen [OpenShift belgelerine](https://docs.openshift.com/container-platform/4.2/authentication/managing-security-context-constraints.html)bakın.
Şimdi bir sonraki adımı uygulayabilirsiniz.

## <a name="create-an-azure-database-for-postgresql-hyperscale-server-group"></a>PostgreSQL için Azure veritabanı hiper ölçek sunucu grubu oluşturma

Azure Arc 'da PostgreSQL için Azure veritabanı hiper ölçek sunucu grubu oluşturmak için aşağıdaki komutu kullanın:

```console
azdata arc postgres server create -n <name> --workers <# worker nodes with #>=2> --storage-class-data <storage class name> --storage-class-logs <storage class name> --storage-class-backups <storage class name>

#Example
#azdata arc postgres server create -n postgres01 --workers 2
```

> [!NOTE]
> - **Kullanılabilir başka komut satırı parametreleri vardır.  ' İ çalıştırarak seçeneklerin tüm listesini görüntüleyin `azdata arc postgres server create --help` .**
> - Yedeklemeler için kullanılan depolama sınıfı (_--Storage-Class-Backups-SCB_) sağlanmazsa veri denetleyicisinin veri depolama sınıfına varsayılan olarak ayarlanır.
> - --Volume-size-* parametreleri tarafından kabul edilen birim bir Kubernetes kaynak miktarıdır (bundan sonra, bu, bu sahip olan bu sahip (T, G, M, K, m) veya bunların iki bitlik eşdeğerinden (TI, gi, mı, ki) biridir).
> - Adların uzunluğu 12 karakter veya daha az olmalıdır ve DNS adlandırma kurallarına uymalıdır.
> - _Postgres_ standart yönetici kullanıcısının parolasını girmeniz istenir.  `AZDATA_PASSWORD`Create komutunu çalıştırmadan önce oturum ortam değişkenini ayarlayarak etkileşimli istemi atlayabilirsiniz.
> - Veri denetleyicisini aynı Terminal oturumunda AZDATA_USERNAME ve AZDATA_PASSWORD oturum ortamı değişkenlerini kullanarak dağıttıysanız, PostgreSQL hiper ölçek sunucu grubunu da dağıtmak için AZDATA_PASSWORD değerleri kullanılacaktır. Başka bir parola kullanmayı tercih ediyorsanız, (1) AZDATA_PASSWORD için değeri güncelleştirin veya (2 AZDATA_PASSWORD) bir sunucu grubu oluştururken etkileşimli olarak bir parola girmesi istenir veya değerini silin.
> - PostgreSQL hiper ölçek veritabanı altyapısının varsayılan yönetici kullanıcısının adı _Postgres_ ve bu noktada değiştirilemez.
> - PostgreSQL hiper ölçek sunucu grubu oluşturma, kaynakları hemen Azure 'da kaydetmez. [Kaynak envanteri](upload-metrics-and-logs-to-azure-monitor.md) veya [kullanım verilerini](view-billing-data-in-azure.md) Azure 'a yükleme işleminin bir parçası olarak, kaynaklar azure 'da oluşturulur ve Azure Portal kaynaklarınızı görebileceksiniz.



## <a name="list-your-azure-database-for-postgresql-server-groups-created-in-your-arc-setup"></a>Arc kurulumlarınızdan oluşturulan PostgreSQL için Azure veritabanı sunucu gruplarını listeleyin

Azure Arc 'daki PostgreSQL hiper ölçek sunucu gruplarını görüntülemek için aşağıdaki komutu kullanın:

```console
azdata arc postgres server list
```


```output
Name        State     Workers
----------  --------  ---------
postgres01  Ready     2
```

## <a name="get-the-endpoints-to-connect-to-your-azure-database-for-postgresql-server-groups"></a>PostgreSQL için Azure veritabanı sunucu gruplarına bağlanacak uç noktaları alın

Bir PostgreSQL örneğine ilişkin uç noktaları görüntülemek için aşağıdaki komutu çalıştırın:

```console
azdata arc postgres endpoint list -n <server group name>
```
Örnek:
```console
[
  {
    "Description": "PostgreSQL Instance",
    "Endpoint": "postgresql://postgres:<replace with password>@12.345.123.456:1234"
  },
  {
    "Description": "Log Search Dashboard",
    "Endpoint": "https://12.345.123.456:12345/kibana/app/kibana#/discover?_a=(query:(language:kuery,query:'custom_resource_name:\"postgres01\"'))"
  },
  {
    "Description": "Metrics Dashboard",
    "Endpoint": "https://12.345.123.456:12345/grafana/d/postgres-metrics?var-Namespace=arc3&var-Name=postgres01"
  }
]
```

PostgreSQL örnek uç noktasını, en sevdiğiniz araçtan PostgreSQL hiper ölçek sunucu grubuna bağlanmak için kullanabilirsiniz:  [Azure Data Studio](/sql/azure-data-studio/download-azure-data-studio), [pgclı](https://www.pgcli.com/) psql, pgAdmin, vb.

Test etmek için bir Azure VM kullanıyorsanız, aşağıdaki yönergeleri izleyin:

## <a name="special-note-about-azure-virtual-machine-deployments"></a>Azure sanal makine dağıtımları hakkında özel bilgi

Bir Azure sanal makinesi kullanırken, uç nokta IP adresi _genel_ IP adresini göstermez. Genel IP adresini bulmak için aşağıdaki komutu kullanın:

```console
az network public-ip list -g azurearcvm-rg --query "[].{PublicIP:ipAddress}" -o table
```

Daha sonra bağlantı kurmak için genel IP adresini bağlantı noktasıyla birleştirebilirsiniz.

Ayrıca, PostgreSQL hiper ölçek sunucu grubunun bağlantı noktasını ağ güvenlik ağ geçidi (NSG) üzerinden kullanıma sunabilirsiniz. (NSG) üzerinden trafiğe izin vermek için aşağıdaki komutu kullanarak kullanabileceğiniz bir kural eklemeniz gerekir:

Bir kural ayarlamak için NSG 'nizin adını bilmeniz gerekir. Aşağıdaki komutu kullanarak NSG 'yi belirlersiniz:

```console
az network nsg list -g azurearcvm-rg --query "[].{NSGName:name}" -o table
```

NSG 'nin adını aldıktan sonra, aşağıdaki komutu kullanarak bir güvenlik duvarı kuralı ekleyebilirsiniz. Burada örnek değerler, 30655 numaralı bağlantı noktası için bir NSG kuralı oluşturur ve **herhangi** BIR kaynak IP adresinden bağlantıya izin verir.  Bu en iyi güvenlik uygulaması değildir!  İstemci IP adresine özgü bir-kaynak-adres ön eki değeri ya da takımınızın veya kuruluşun IP adreslerini içeren bir IP adresi aralığı belirterek, şeyleri daha iyi bir şekilde kilitleyebilir.

Aşağıdaki--Destination-Port-Ranges parametresinin değerini yukarıdaki ' azdata Arc Postgres Server List ' komutundan aldığınız bağlantı noktası numarasıyla değiştirin.

```console
az network nsg rule create -n db_port --destination-port-ranges 30655 --source-address-prefixes '*' --nsg-name azurearcvmNSG --priority 500 -g azurearcvm-rg --access Allow --description 'Allow port through for db access' --destination-address-prefixes '*' --direction Inbound --protocol Tcp --source-port-ranges '*'
```

## <a name="connect-with-azure-data-studio"></a>Azure Data Studio ile bağlanma

Azure Data Studio açın ve yukarıdaki dış uç nokta IP adresini ve bağlantı noktası numarasını ve örneği oluşturduğunuz sırada belirttiğiniz parolayı kullanarak örneğinizle bağlantı yapın.  *Bağlantı türü* açılan listesinde PostgreSQL yoksa, Uzantılar sekmesinde PostgreSQL ' ı arayarak PostgreSQL uzantısını yükleyebilirsiniz.

> [!NOTE]
> Bağlantı masasındaki [Gelişmiş] düğmesine tıklayarak bağlantı noktası numarasını girmeniz gerekir.

Azure VM kullanıyorsanız, şu komutla erişilebilen _genel_ IP adresine ihtiyacınız olacağını unutmayın:

```console
az network public-ip list -g azurearcvm-rg --query "[].{PublicIP:ipAddress}" -o table
```

## <a name="connect-with-psql"></a>Psql ile bağlanma

PostgreSQL hiper ölçek sunucu grubunuza erişmek için, yukarıda aldığınız PostgreSQL hiper ölçek sunucu grubunun dış uç noktasını geçirin:

Artık psql 'e bağlanabilirsiniz:

```console
psql postgresql://postgres:<EnterYourPassword>@10.0.0.4:30655
```

## <a name="next-steps"></a>Sonraki adımlar

- PostgreSQL için Azure veritabanı 'nın, verileri birden çok PostgreSQL hiper ölçek düğümüne dağıtmak ve PostgreSQL için Azure veritabanı hiper ölçek 'in tüm gücünden yararlanmak için Azure veritabanı 'nın kavramlarını ve nasıl yapılır kılavuzlarını okuyun. :
    * [Düğümler ve tablolar](../../postgresql/concepts-hyperscale-nodes.md)
    * [Uygulama türünü belirleme](../../postgresql/concepts-hyperscale-app-type.md)
    * [Dağıtım sütunu seçme](../../postgresql/concepts-hyperscale-choose-distribution-column.md)
    * [Tablo birlikte bulundurma](../../postgresql/concepts-hyperscale-colocation.md)
    * [Tabloları dağıtma ve değiştirme](../../postgresql/howto-hyperscale-modify-distributed-tables.md)
    * [Çok kiracılı bir veritabanı tasarlama](../../postgresql/tutorial-design-database-hyperscale-multi-tenant.md)*
    * [Gerçek zamanlı analiz panosu tasarlama](../../postgresql/tutorial-design-database-hyperscale-realtime.md)*

    > \* Yukarıdaki belgelerde, **Azure Portal oturum açma**bölümünü atlayın, & **PostgreSQL Için Azure veritabanı oluşturun-hiper ölçek (Citus)**. Azure Arc dağıtımınızda kalan adımları uygulayın. Bu bölümler, Azure bulutunda PaaS hizmeti olarak sunulan PostgreSQL için Azure veritabanı 'na (Citus) özgüdür, ancak belgelerin diğer kısımları doğrudan Azure Arc etkin PostgreSQL hiper ölçeklendirmenize uygulanabilir.

- [PostgreSQL için Azure Veritabanı Hiper Ölçek sunucu grubunuzun ölçeğini genişletme](scale-out-postgresql-hyperscale-server-group.md)
- [Depolama yapılandırması ve Kubernetes depolama kavramları](storage-configuration.md)
- [Kalıcı birim taleplerini genişletme](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#expanding-persistent-volumes-claims)
- [Kubernetes kaynak modeli](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/scheduling/resources.md#resource-quantities)