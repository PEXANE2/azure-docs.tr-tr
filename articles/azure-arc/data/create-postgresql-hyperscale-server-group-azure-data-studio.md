---
title: Azure Data Studio kullanarak Azure Arc etkin PostgreSQL hiper ölçeği oluşturma
description: Azure Data Studio kullanarak Azure Arc etkin PostgreSQL hiper ölçeği oluşturma
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 1b1cf90efa14345fa8395f5f62fd80934b922352
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91273016"
---
# <a name="create-azure-arc-enabled-postgresql-hyperscale-using-azure-data-studio"></a>Azure Data Studio kullanarak Azure Arc etkin PostgreSQL hiper ölçeği oluşturma

Bu belge, Azure Arc etkin PostgreSQL hiper ölçek sunucu grupları sağlamak için Azure Data Studio kullanma adımlarında size yol gösterir.

[!INCLUDE [azure-arc-common-prerequisites](../../../includes/azure-arc-common-prerequisites.md)]

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="connect-to-the-azure-arc-data-controller"></a>Azure Arc veri denetleyicisine bağlanma

Bir örnek oluşturabilmeniz için önce henüz oturum açmadıysanız Azure Arc veri denetleyicisinde oturum açın.

```console
azdata login
```

Daha sonra, veri denetleyicisinin oluşturulduğu ad alanı, Kullanıcı adı ve denetleyicide oturum açmak için parola istenir.

> Ad alanını doğrulamanız gerekiyorsa, ```kubectl get pods -A``` kümedeki tüm ad alanlarının listesini almak için öğesini çalıştırabilirsiniz.

```console
Username: arcadmin
Password:
Namespace: arc
Logged in successfully to `https://10.0.0.4:30080` in namespace `arc`. Setting active context to `arc`
```

## <a name="preliminary-and-temporary-step-for-openshift-users-only"></a>Yalnızca OpenShift kullanıcıları için ön ve geçici adım

Sonraki adıma geçmeden önce bu adımı uygulayın. Varsayılan dışında bir projede PostgreSQL hiper ölçek sunucu grubunu Red Hat OpenShift 'e dağıtmak için, güvenlik kısıtlamalarını güncelleştirmek üzere kümenizde aşağıdaki komutları yürütmeniz gerekir. Bu komut, PostgreSQL hiper ölçek sunucu grubunuzu çalıştıracak hizmet hesaplarına gerekli ayrıcalıkları verir. Güvenlik bağlamı kısıtlaması (SCC) **_Arc-Data-SCC_** , Azure Arc veri denetleyicisi 'ni dağıtırken eklediğiniz bir.

```console
oc adm policy add-scc-to-user arc-data-scc -z <server-group-name> -n <namespace name>
```

_**Sunucu-grup adı** , bir sonraki adımda dağıtacaksınız sunucu grubunun adıdır._
   
OpenShift 'te SCCs hakkında daha fazla ayrıntı için lütfen [OpenShift belgelerine](https://docs.openshift.com/container-platform/4.2/authentication/managing-security-context-constraints.html)bakın.
Şimdi bir sonraki adımı uygulayabilirsiniz.

## <a name="create-an-azure-arc-enabled-postgresql-hyperscale-server-group"></a>Azure Arc özellikli PostgreSQL Hiper Ölçek sunucu grubu oluşturma

1. Azure Data Studio Başlat
1. Bağlantılar sekmesinde, sol üstteki üç noktaya tıklayın ve "yeni dağıtım" seçeneğini belirleyin.
1. Dağıtım seçenekleri ' nden **PostgreSQL hiper ölçek sunucu grubu-Azure Arc** ' ı seçin.
    >[!NOTE]
    > Henüz `azdata` yüklü değilse, CLI 'yi buraya yüklemelisiniz.
1. Gizlilik ve lisans koşullarını kabul edin ve en altta **Seç** ' e tıklayın.
1. Dağıtım PostgreSQL hiper ölçek sunucu grubu-Azure yay dikey penceresinde aşağıdaki bilgileri girin:
   - Sunucu grubu için bir ad girin
   - Sunucu grubunun _Postgres_ Yöneticisi kullanıcısı için bir parola girin ve onaylayın
   - Veri için uygun depolama sınıfını seçin
   - Günlüklere uygun depolama sınıfını seçin
   - Yedeklemeler için uygun depolama sınıfını seçin
   - Sağlanacak çalışan düğümü sayısını seçin
1. **Dağıt** düğmesine tıklayın

Bu, veri denetleyicisinde Azure Arc etkin PostgreSQL hiper ölçek sunucu grubu oluşturmayı başlatır.

Birkaç dakika içinde, oluşturma işleminin başarıyla tamamlanmış olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Data Studio kullanarak sunucu grubunuzu yönetme](manage-postgresql-hyperscale-server-group-with-azure-data-studio.md)
- [Sunucu grubunuzu izleme](monitor-grafana-kibana.md)
- PostgreSQL için Azure veritabanı 'nın, verileri birden çok PostgreSQL hiper ölçek düğümüne dağıtmak ve Postgres Hyperscale için Azure veritabanı 'nın tüm gücünden yararlanmak için Azure veritabanı 'nın kavramlarını ve nasıl yapılır kılavuzlarını okuyun. :
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
- [Kubernetes kaynak modeli](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/scheduling/resources.md#resource-quantities)

