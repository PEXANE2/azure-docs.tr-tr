---
title: Azure Data Studio panoları
description: Azure Data Studio panoları
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: c2dd0ce3c3e9a0f1f699b58ed2657394ad9acbe1
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941814"
---
# <a name="azure-data-studio-dashboards"></a>Azure Data Studio panoları

[Azure Data Studio](https://aka.ms/azuredatastudio) , Azure Arc kaynaklarınızla ilgili bilgileri görüntülemek için Azure Portal benzer bir deneyim sağlar.  Bu görünümlere **panolar** denir ve Azure Portal belirli bir kaynak hakkında neleri görebileceğinize benzer bir düzen ve seçeneklere sahip olursunuz, ancak Azure 'a yönelik bir bağlantınız olmadığı durumlarda bu bilgileri ortamınızda yerel olarak görme esnekliği sağlar.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="connecting-to-a-data-controller"></a>Veri denetleyicisine bağlanma

### <a name="prerequisites"></a>Önkoşullar

- [Azure Data Studio](https://aka.ms/getazuredatastudio) indir
- Azure Arc uzantısı yüklendi

### <a name="determine-the-data-controller-server-api-endpoint-url"></a>Data Controller Server API uç nokta URL 'sini belirleme

İlk olarak, Azure Data Studio Data Controller Service API Endpoint URL 'nize bağlamanız gerekir.

Bu uç noktayı almak için aşağıdaki komutu çalıştırabilirsiniz:

```console
kubectl get svc/controller-svc-external -n <namespace name>

#Example:
kubectl get svc/controller-svc-external -n arc
```

Aşağıdakine benzer bir çıktı görürsünüz:

```console
NAME                      TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)                                       AGE
controller-svc-external   LoadBalancer   10.0.175.137   52.154.152.24    30080:32192/TCP                               22h
```

Yük dengeleyici türü kullanıyorsanız, dış IP adresini ve 300080 numaralı bağlantı noktasını kopyalamak isteyeceksiniz.  NodePort kullanıyorsanız, Kubernetes API sunucunuzun IP adresini ve bağlantı noktası (ler) sütununda listelenen bağlantı noktası numarasını kullanmak isteyeceksiniz.

Şimdi bu bilgileri şöyle birleştirerek uç noktanıza bir URL oluşturmak isteyeceksiniz:

```console
https://<ip address>:<port>

Example:
https://52.;154.152.24:30080
```

Bir sonraki adımda kullanacağınız için IP adresinizi göz önünde bulabilirsiniz.

### <a name="connect"></a>Bağlan

1. Azure Data Studio açın

1. Sol taraftaki **Bağlantılar** sekmesini seçin

En alta doğru, **Azure Arc denetleyicileri**adlı paneli genişletin.

Yeni bir veri denetleyicisi bağlantısı eklemek için + simgesine tıklayın.

Komut paletindeki ekranın üst kısmında, 1. adımda oluşturduğunuz URL 'yi girin, ENTER ' a tıklayın.
Veri denetleyicisi için Kullanıcı adını girin.  Bu, veri denetleyicisinin dağıtımı sırasında geçirilen Kullanıcı adı değeridir.  ENTER 'a tıklayın.
Veri denetleyicisi için parolayı girin.  Bu, veri denetleyicisinin dağıtımı sırasında geçirilen parola değeridir. ENTER 'a tıklayın.

Artık bir veri denetleyicisine bağlı olduğunuza göre, veri denetleyicisi ve herhangi bir SQL yönetilen örneği veya PostgreSQL hiper ölçek sunucu grubu kaynakları için panoları görüntüleyebilirsiniz.

## <a name="view-the-data-controller-dashboard"></a>Veri denetleyicisi panosunu görüntüleme

**Yay denetleyicileri** Genişletilebilir panelinin Bağlantılar panelinde veri denetleyicisine sağ tıklayın ve **Yönet**' i seçin.

Burada ad, bölge, bağlantı modu, kaynak grubu, abonelik, denetleyici uç noktası ve ad alanı gibi veri denetleyicisi kaynağı hakkındaki ayrıntıları görebilirsiniz.  Veri denetleyicisi tarafından yönetilen tüm yönetilen veritabanı kaynaklarının listesini de görebilirsiniz.

Düzenin Azure portal gördüklerinize benzer olduğunu fark edeceksiniz.

Rahat bir şekilde, + yeni örnek düğmesine tıklayarak bir SQL yönetilen örneği veya PostgreSQL hiper ölçek sunucu grubu oluşturmayı başlatabilirsiniz.

Ayrıca, Azure portal aç düğmesine tıklayarak bu veri denetleyicisi bağlamında Azure portal açabilirsiniz.

## <a name="view-the-sql-managed-instance-dashboards"></a>SQL yönetilen örnek panolarını görüntüleme

Bazı SQL yönetilen örnekleri oluşturduysanız, bunları yöneten veri denetleyicisinin altında bulunan Azure veri denetleyicileri Genişletilebilir paneli ' ndeki Bağlantılar panelinde listelenmiş olanları görebilirsiniz.

Belirli bir örnek için SQL yönetilen örnek panosunu görüntülemek için örneğe sağ tıklayıp Yönet ' i seçin.

Bağlantı paneli sağ tarafta açılır ve bu SQL örneğine bağlanmak için oturum açma/parola ister. Bağlantı bilgilerini biliyorsanız, bu bilgileri girebilir ve Bağlan ' a tıklayabilirsiniz.  Bilmiyorsanız, Iptal ' e tıklayabilirsiniz.  Her iki durumda da bağlantı paneli kapandığında panoya yönlendirilirsiniz.

Genel Bakış sekmesinde kaynak grubu, veri denetleyicisi, abonelik KIMLIĞI, durum, bölge ve daha fazlası gibi SQL yönetilen örneği hakkındaki ayrıntıları görüntüleyebilirsiniz.  Ayrıca, bu SQL yönetilen örneği bağlamında Grafana veya kibana panolarına gitmek için tıklabileceğiniz bağlantıyı görebilirsiniz.

SQL Manage örneğine bağlanıyorsanız, burada ek bilgi görebilirsiniz.

SQL yönetilen örneğini buradan silebilir veya Azure portal SQL yönetilen örneğini görüntülemek için Azure portal açabilirsiniz.

Sol taraftaki bağlantı dizeleri sekmesine tıklarsanız, bu SQL yönetilen örneği için önceden oluşturulmuş bağlantı dizelerinin bir listesini, diğer çeşitli uygulamalara veya koda kopyalama/yapıştırma işlemlerini kolaylaştırır.

## <a name="view-the-postgresql-hyperscale-server-group-dashboards"></a>PostgreSQL hiper ölçek sunucu grubu panolarını görüntüleme

Bazı PostgreSQL hiper ölçek sunucu grupları oluşturduysanız, bunları yöneten veri denetleyicisinin altında bulunan Azure veri denetleyicileri Genişletilebilir paneli ' ndeki Bağlantılar panelinde listelenmiş olarak görebilirsiniz.

Belirli bir sunucu grubu için PostgreSQL hiper ölçek sunucu grubu panosunu görüntülemek için, sunucu grubuna sağ tıklayın ve Yönet ' i seçin.

Genel Bakış sekmesinde kaynak grubu, veri denetleyicisi, abonelik KIMLIĞI, durum, bölge ve daha fazlası gibi sunucu grubuyla ilgili ayrıntıları görüntüleyebilirsiniz.  Ayrıca, bu sunucu grubuna bağlamda Grafana veya kibana panolarına gitmek için tıklabileceğiniz bağlantıyı görebilirsiniz.

Sunucu grubunu buradan silebilir veya Azure portal sunucu grubunu görüntülemek için Azure portal açabilirsiniz.

Sol taraftaki bağlantı dizeleri sekmesine tıklarsanız, bu sunucu grubu için önceden oluşturulmuş bağlantı dizelerinin bir listesini, diğer çeşitli uygulamalara veya koda kopyalayabilir/yapıştırmanızı kolaylaştırır.

Soldaki Özellikler sekmesine tıklarsanız, ek ayrıntıları görebilirsiniz.

Sol taraftaki kaynak sistem durumu sekmesine tıklarsanız, bu sunucu grubunun geçerli üst düzey sistem durumunu görebilirsiniz.

Soldaki sorunları Tanıla ve çöz sekmesine tıklarsanız, PostgreSQL sorun giderme Not defterini başlatabilirsiniz.

Sol taraftaki yeni destek isteği sekmesine tıklarsanız, sunucu grubu bağlamında Azure portal başlatabilir ve buradan bir Azure destek isteği oluşturabilirsiniz.
