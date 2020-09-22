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
ms.openlocfilehash: af0cdb814433b739b15d79bec9cb399cf0a2fef7
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941858"
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

## <a name="create-an-azure-arc-enabled-postgresql-hyperscale-server-group"></a>Azure Arc etkin PostgreSQL hiper ölçek sunucu grubu oluşturma

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

- [PostgreSQL için Azure veritabanı hiper ölçek sunucu grubu ölçeğini genişletme](scale-out-postgresql-hyperscale-server-group.md)
- [Depolama yapılandırması ve Kubernetes depolama kavramları](storage-configuration.md)
- [Kalıcı birim taleplerini genişletme](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#expanding-persistent-volumes-claims)
- [Kubernetes kaynak modeli](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/scheduling/resources.md#resource-quantities)

