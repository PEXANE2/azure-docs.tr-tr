---
title: Azure Işlevlerini bir sanal ağ ile yapılandırma
description: Azure Işlevleri için belirli sanal ağ görevlerinin nasıl gerçekleştirileceğini gösteren makale.
ms.topic: conceptual
ms.date: 3/13/2021
ms.custom: template-how-to
ms.openlocfilehash: a28a59a0de40bba7914d1920b42034fbbc223ddc
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104609990"
---
# <a name="how-to-configure-azure-functions-with-a-virtual-network"></a>Azure Işlevlerini bir sanal ağ ile yapılandırma

Bu makalede, işlev uygulamanızı bir sanal ağa bağlanmak ve üzerinde çalıştırmak için yapılandırma ile ilgili görevlerin nasıl gerçekleştirileceği gösterilir. Azure Işlevleri ve ağ oluşturma hakkında daha fazla bilgi edinmek için bkz. [Azure işlevleri ağ seçenekleri](functions-networking-options.md).

## <a name="restrict-your-storage-account-to-a-virtual-network"></a>Depolama hesabınızı bir sanal ağla sınırlayın 

Bir işlev uygulaması oluşturduğunuzda, blob, kuyruk ve tablo depolamayı destekleyen genel amaçlı bir Azure depolama hesabı oluşturmanız veya bağlamanız gerekir. Bu depolama hesabını hizmet uç noktaları veya özel uç nokta ile güvenli bir şekilde değiştirebilirsiniz. 

> [!NOTE]  
> Bu özellik şu anda adanmış (App Service) planı ve Premium planlar için tüm Windows sanal ağı tarafından desteklenen SKU 'Lar için geçerlidir. Tüketim planı desteklenmez. 

Özel bir ağla sınırlı bir depolama hesabı ile bir işlev ayarlamak için:

1. Hizmet uç noktaları etkin olmayan bir depolama hesabıyla bir işlev oluşturun.

1. İşlevini sanal ağınıza bağlanacak şekilde yapılandırın.

1. Farklı bir depolama hesabı oluşturun veya yapılandırın.  Bu, hizmet uç noktalarıyla güvenli hale yaptığımız depolama hesabıdır ve işlevimizi bağlayacağız.

1. Güvenli depolama hesabında [bir dosya paylaşma oluşturun](../storage/files/storage-how-to-create-file-share.md#create-file-share) .

1. Depolama hesabı için hizmet uç noktalarını veya özel uç noktayı etkinleştirin.  
    * Özel uç nokta bağlantıları kullanıyorsanız, depolama hesabının `file` ve alt kaynaklar için özel bir uç noktası olması gerekir `blob` .  Dayanıklı İşlevler gibi belirli yetenekler kullanılıyorsa, Ayrıca, `queue` `table` özel bir uç nokta bağlantısı aracılığıyla da ihtiyacınız ve erişilebilir olur.
    * Hizmet uç noktaları kullanılıyorsa, depolama hesapları için işlev uygulamalarınıza adanmış alt ağı etkinleştirin.

1. Dosya ve BLOB içeriğini işlev uygulama depolama hesabından güvenli depolama hesabına ve dosya paylaşımıyla kopyalayın.

1. Bu depolama hesabı için bağlantı dizesini kopyalayın.

1. İşlev uygulaması **yapılandırması** altındaki **uygulama ayarlarını** aşağıdakilere göre güncelleştirin:

    | Ayar adı | Değer | Yorum |
    |----|----|----|
    | `AzureWebJobsStorage`| Depolama bağlantı dizesi | Bu, güvenli depolama hesabı için bağlantı dizesidir. |
    | `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` |  Depolama bağlantı dizesi | Bu, güvenli depolama hesabı için bağlantı dizesidir. |
    | `WEBSITE_CONTENTSHARE` | Dosya paylaşımı | Proje dağıtım dosyalarının bulunduğu güvenli depolama hesabında oluşturulan dosya paylaşımının adı. |
    | `WEBSITE_CONTENTOVERVNET` | 1 | Yeni ayar |
    | `WEBSITE_VNET_ROUTE_ALL` | 1 | Tüm giden trafiği sanal ağ üzerinden zorlar. Depolama hesabı özel uç nokta bağlantıları kullanıyorsa gereklidir. |
    | `WEBSITE_DNS_SERVER` | `168.63.129.16` | Uygulama tarafından kullanılan DNS sunucusu. Depolama hesabı özel uç nokta bağlantıları kullanıyorsa gereklidir. |

1. Uygulama ayarlarını kaydetmek için **Kaydet** ' i seçin. Uygulama ayarlarının değiştirilmesi uygulamanın yeniden başlatılmasına neden olur.  

İşlev uygulaması yeniden başlatıldıktan sonra, artık güvenli bir depolama hesabına bağlanır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure İşlevleri ağ seçenekleri](functions-networking-options.md)

