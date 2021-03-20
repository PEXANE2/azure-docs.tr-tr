---
title: PostgreSQL örneğinizi yönetmek için Azure Data Studio kullanın
description: PostgreSQL örneğinizi yönetmek için Azure Data Studio kullanın
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 7dcc0f916a15598060e034dcf62536ee13e2672e
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "92320228"
---
# <a name="use-azure-data-studio-to-manage-your-azure-arc-enabled-postgresql-hyperscale-server-group"></a>Azure Arc etkin PostgreSQL hiper ölçek sunucu grubunu yönetmek için Azure Data Studio kullanın


Bu makalede nasıl yapılacağı açıklanır:
- PostgreSQL örneklerinizi genel bakış, bağlantı dizeleri, özellikler, Kaynak Durumu gibi Pano görünümleriyle yönetin...
- verileriniz ve şemanıza göre çalışma

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="prerequisites"></a>Önkoşullar

- [Azdata, Azure Data Studio ve Azure CLı 'yı yükler](install-client-tools.md)
- **[!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)]** Ve **Azure Arc** ve **PostgreSQL** uzantılarında Azure Data Studio yükleme
- [Azure Arc veri denetleyicisi](create-data-controller-using-azdata.md) oluşturma
- Azure Data Studio Başlat

## <a name="connect-to-the-azure-arc-data-controller"></a>Azure Arc veri denetleyicisine bağlanma

Azure Data Studio ' de, **Azure Arc denetleyicileri** düğümünü genişletin ve **Connect Controller** düğmesini seçin:

Azure veri denetleyicinize bağlantı bilgilerini girin:

- **Denetleyici URL 'SI:**

    Kubernetes 'de denetleyicinize bağlanacak URL. Örneğin biçiminde girilir `https://<IP_address_of_the_controller>:<Kubernetes_port.` :

    ```console
    https://12.345.67.890:30080
    ```
- **Kullanıcı adı:**

    Denetleyiciye bağlanmak için kullandığınız kullanıcı hesabının adı. Çalıştırdığınızda, genellikle kullandığınız adı kullanın `azdata login` . Genellikle psql 'den PostgreSQL veritabanı altyapısına bağlanmak için kullandığınız PostgreSQL kullanıcısının adı değildir.
- **Parola:** Denetleyiciye bağlanmak için kullandığınız kullanıcı hesabının parolası


Azure Data Studio, Arc veri denetleyicinizi gösterir. Genişlettikten sonra, yönettiği PostgreSQL örneklerinin listesini gösterir.

## <a name="manage-your-azure-arc-enabled-postgresql-hyperscale-server-groups"></a>Azure Arc etkin PostgreSQL hiper ölçek sunucu gruplarınızı yönetme

Yönetmek istediğiniz PostgreSQL örneğine sağ tıklayın ve [Yönet] öğesini seçin

PostgreSQL pano görünümü:

Bu, bölmenin sol tarafında listelenen çeşitli panoları sunar:

- **Genel bakış:** Örneğiniz hakkında ad, PostgreSQL Yönetici Kullanıcı adı, Azure abonelik KIMLIĞI, yapılandırma, veritabanı altyapısının sürümü, Grafana ve kibana için uç noktalar gibi özet bilgiler görüntüler...
- **Bağlantı dizeleri:** Psql, Node.js, PHP, Ruby gibi PostgreSQL örneğinize bağlanmanız gerekebilecek çeşitli bağlantı dizelerini görüntüler.
- **Sorunları tanılama ve çözme:** , Sorun giderme not defterlerini genişlettiğimiz için örneğinizin sorunlarını gidermenize yardımcı olacak çeşitli kaynakları bulacağınız giriş sayfasıdır
- **Yeni destek isteği:** , Destek hizmetlerinden genel önizleme duyurusunu Başlatan yardım isteyebilmeniz için giriş sayfasıdır.

## <a name="work-with-your-data-and-schema"></a>Verileriniz ve şemanıza göre çalışma

Azure Data Studio penceresinin sol tarafında, düğüm **sunucularını** genişletin:

Ve [bağlantı ekle] öğesini seçin ve bağlantı ayrıntılarını PostgreSQL örneğinize girin:
- **Bağlantı türü:** PostgreSQL
- **Sunucu adı:** PostgreSQL örneğinizin adını girin. Örneğin: postgres01
- **Kimlik doğrulama türü:** Parolayı
- **Kullanıcı adı:** Örneğin, standart/varsayılan PostgreSQL yönetici kullanıcı adını kullanabilirsiniz. Bu alan, büyük/küçük harfe duyarlıdır.
- **Parola:** psql bağlantı dizesinde PostgreSQL Kullanıcı adının parolasını komutun çıktısında bulabilirsiniz `azdata postgres server endpoint -n postgres01`
- **Veritabanı adı:** bağlanmak istediğiniz veritabanının adını ayarlayın. __Varsayılan__ olarak ayarlanmış olmasına izin verebilirsiniz
- **Sunucu grubu:** __varsayılan__ olarak ayarlanmış olmasına izin verebilir
- **Ad (isteğe bağlı):** bunun boş olmasına izin verebilirsiniz
- **İleri**
    - **Ana BILGISAYAR IP adresi:** Kubernetes KÜMESININ genel IP adresidir
    - **Bağlantı noktası:** PostgreSQL örneğinizin dinlediği bağlantı noktasıdır. Bu bağlantı noktasını, komutun çıkışında psql bağlantı dizesinin sonunda bulabilirsiniz `azdata postgres server endpoint -n postgres01` . Kubernetes 'in dinlediği ve Azure Data Studio ' de Azure veri denetleyicisi 'ne bağlanırken girdiğiniz bağlantı noktası 30080 değil.
    - **Diğer parametreler:** Kendi kendine açık olmaları gerekir, ile göründükleri varsayılan/boş değerlerle canlı olabilirsiniz.

Sunucunuza bağlanmak için **[Tamam] ve [Bağlan]** öğesini seçin.

Bağlandıktan sonra birkaç deneyim sunulmaktadır:
- **Yeni sorgu**
- **Yeni Not defteri**
- **Sunucunuzun görüntüsünü genişletin ve veritabanınızın içindeki nesneler üzerinde gidip çalışın**
- **...**

## <a name="next-step"></a>Sonraki adım
[Sunucu grubunuzu izleme](monitor-grafana-kibana.md)
