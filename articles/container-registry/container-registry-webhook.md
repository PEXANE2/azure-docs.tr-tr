---
title: Webhooks kayıt defteri eylemleri yanıtlamak için
description: Kayıt defteri depolarınızda itme veya çekme eylemleri gerçekleştiğinde olayları tetiklemek için web hooks'u nasıl kullanacağınızı öğrenin.
ms.topic: article
ms.date: 05/24/2019
ms.openlocfilehash: 5e6fd2d9f4c7727365a8e2fe3893aafebfeb7bd4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74454365"
---
# <a name="using-azure-container-registry-webhooks"></a>Azure Kapsayıcı Kayıt Defteri webhooks kullanma

Azure kapsayıcısı kayıt defteri, Docker Hub’ın genel Docker görüntülerini depolama yöntemine benzer şekilde özel Docker kapsayıcı görüntülerini depolar ve yönetir. Ayrıca, uygulamaları Kubernetes'e dağıtmak için bir paketleme biçimi olan [Miğfer grafikleri](container-registry-helm-repos.md) (önizleme) için depoları da barındırabilir. Kayıt defteri depolarınızdan birinde belirli eylemler gerçekleştiğinde olayları tetiklemek için webhooks'u kullanabilirsiniz. Webhooks, kayıt defteri düzeyindeki olaylara yanıt verebilir veya belirli bir depo etiketine kadar kapsama satılabilir. Coğrafi [olarak çoğaltılan](container-registry-geo-replication.md) bir kayıt defteriyle, her web kancasını belirli bir bölgesel yinelemedeki olaylara yanıt verecek şekilde yapılandırırsınız.

Webhook istekleri yle ilgili ayrıntılar için [Azure Konteyner Kayıt Defteri webhook şeması başvurusuna](container-registry-webhook-reference.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

* Azure kapsayıcısı kayıt defteri -Azure aboneliğinizde bir kapsayıcı kayıt defteri oluşturun. Örneğin, Azure [portalını](container-registry-get-started-portal.md) veya [Azure CLI'yi](container-registry-get-started-azure-cli.md)kullanın. [Azure Konteyner Kayıt Defteri SKUs'ları](container-registry-skus.md) farklı webhooks kotalarına sahiptir.
* Docker CLI - Yerel bilgisayarınızı bir Docker konağı olarak ayarlamak ve Docker CLI komutlarına erişmek için [Docker Engine](https://docs.docker.com/engine/installation/)’i yükleyin.

## <a name="create-webhook---azure-portal"></a>Webhook oluşturma - Azure portalı

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Bir webhook oluşturmak istediğiniz kapsayıcı kayıt defterine gidin.
1. **Hizmetler** **altında, Webhooks'u**seçin.
1. Webhook araç çubuğunda **Ekle'yi** seçin.
1. Webhook *Oluştur* formunu aşağıdaki bilgilerle doldurun:

| Değer | Açıklama |
|---|---|
| Webhook adı | Webhook'a vermek istediğin isim. Yalnızca harf ve sayı içerebilir ve 5-50 karakter uzunluğunda olmalıdır. |
| Konum | Coğrafi [olarak çoğaltılan](container-registry-geo-replication.md) bir kayıt defteri için, kayıt defteri çoğaltmasının Azure bölgesini belirtin. 
| Hizmet URI | Webhook POST bildirimleri göndermesi gereken URI. |
| Özel üst bilgiler | POST isteği ile birlikte aktarmak istediğiniz başlıklar. Bunlar "anahtar: değer" biçiminde olmalıdır. |
| Tetikleyici eylemler | Web hook'u tetikleyen eylemler. Eylemler resim itme, resim silme, Miğfer grafiği itme, Helm grafik silme ve görüntü karantinaiçerir. Webhook'u tetiklemek için bir veya daha fazla eylem seçebilirsiniz. |
| Durum | Oluşturulduktan sonra webhook için durum. Varsayılan olarak etkinleştirilir. |
| Kapsam | Webhook'un çalıştığı kapsam. Belirtilmemişse, kapsam kayıt defterindeki tüm olaylar içindir. Bir depo altındaki tüm etiketler için "depo:etiket" veya "depo:*" biçimini kullanarak bir depo veya etiket için belirtilebilir. |

Örnek webhook formu:

![Azure portalında ACR webhook oluşturma UI](./media/container-registry-webhook/webhook.png)

## <a name="create-webhook---azure-cli"></a>Webhook oluşturma - Azure CLI

Azure CLI'yi kullanarak bir webhook oluşturmak için [az acr webhook oluşturma](/cli/azure/acr/webhook#az-acr-webhook-create) komutunu kullanın. Aşağıdaki komut, kayıt *defteri mycontainerregistry*tüm görüntü silme olayları için bir webhook oluşturur:

```azurecli-interactive
az acr webhook create --registry mycontainerregistry --name myacrwebhook01 --actions delete --uri http://webhookuri.com
```

## <a name="test-webhook"></a>Test webhook

### <a name="azure-portal"></a>Azure portalında

Webhook'u kullanmadan önce **Ping** düğmesi ile test edebilirsiniz. Ping belirtilen bitiş noktasına genel bir POST isteği gönderir ve yanıtı günlüğe kaydeder. Ping özelliğini kullanmak, web kancasını doğru şekilde yapılandırdığınızı doğrulamanıza yardımcı olabilir.

1. Test etmek istediğiniz webhook'u seçin.
2. Üst araç çubuğunda **Ping'i**seçin.
3. **HTTP STATUS** sütunundaki bitiş noktasının yanıtını işaretleyin.

![Azure portalında ACR webhook oluşturma UI](./media/container-registry-webhook/webhook-02.png)

### <a name="azure-cli"></a>Azure CLI

Azure CLI ile bir ACR webhook test etmek için [az acr webhook ping](/cli/azure/acr/webhook#az-acr-webhook-ping) komutunu kullanın.

```azurecli-interactive
az acr webhook ping --registry mycontainerregistry --name myacrwebhook01
```

Sonuçları görmek için [az acr webhook liste olayları](/cli/azure/acr/webhook) komutunu kullanın.

```azurecli-interactive
az acr webhook list-events --registry mycontainerregistry08 --name myacrwebhook01
```

## <a name="delete-webhook"></a>Webhook'u silme

### <a name="azure-portal"></a>Azure portalında

Her webhook, Azure portalındaki webhook ve ardından **Sil** düğmesini seçerek silinebilir.

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
az acr webhook delete --registry mycontainerregistry --name myacrwebhook01
```

## <a name="next-steps"></a>Sonraki adımlar

### <a name="webhook-schema-reference"></a>Webhook şema referans

Azure Konteyner Kayıt Defteri tarafından yayılan JSON olay yüklerinin biçimi ve özellikleri hakkında ayrıntılı bilgi için webhook şeması referansına bakın:

[Azure Konteyner Kayıt Defteri webhook şema başvurusu](container-registry-webhook-reference.md)

### <a name="event-grid-events"></a>Olay Izgara etkinlikleri

Bu makalede tartışılan yerel kayıt webhook olaylarına ek olarak, Azure Kapsayıcı Kayıt Defteri olayları Olay Grid'e yatabilir:

[Hızlı başlatma: Kapsayıcı kayıt defteri olaylarını Olay Idamı'na gönderme](container-registry-event-grid-quickstart.md)
