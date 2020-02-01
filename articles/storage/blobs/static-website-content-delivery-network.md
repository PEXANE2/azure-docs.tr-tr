---
title: Azure CDN-Azure depolama ile statik bir Web sitesi tümleştirme
description: Azure Content Delivery Network (CDN) kullanarak bir Azure depolama hesabından statik Web sitesi içeriğini önbelleğe alma hakkında bilgi edinin.
author: normesta
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.author: normesta
ms.date: 01/22/2020
ms.openlocfilehash: aaf61ccbb3577036c614aa6196d2af57124550fa
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76908559"
---
# <a name="integrate-a-static-website-with-azure-cdn"></a>Statik bir Web sitesini Azure CDN tümleştirme

Azure depolama hesabında barındırılan [statik bir Web sitesinden](storage-blob-static-website.md) içerik önbelleğe almak için [Azure Content Delivery Network (CDN)](../../cdn/cdn-overview.md) etkinleştirebilirsiniz. Statik Web siteniz için özel etki alanı uç noktasını yapılandırmak, özel SSL sertifikaları sağlamak ve özel yeniden yazma kuralları yapılandırmak için Azure CDN kullanabilirsiniz. Azure CDN yapılandırma ek ücretlere neden olur, ancak tutarlı düşük gecikme süreleriyle Web sitenize yerden sağlar dünyanın. Azure CDN, ayrıca kendi sertifikanızı ile SSL şifrelemesi sağlar. 

Azure CDN fiyatlandırması hakkında daha fazla bilgi için bkz: [Azure CDN fiyatlandırması](https://azure.microsoft.com/pricing/details/cdn/).

## <a name="enable-azure-cdn-for-your-static-website"></a>Statik Web siteniz için Azure CDN etkinleştirme

Statik Web siteniz için Azure CDN doğrudan depolama hesabınızdan etkinleştirebilirsiniz. CDN uç noktanız için [büyük dosya indirmeyi en iyi duruma getirme](../../cdn/cdn-optimization-overview.md#large-file-download) gibi gelişmiş yapılandırma ayarları belirtmek istiyorsanız, bir CDN profili ve uç noktası oluşturmak üzere bunun yerine [Azure CDN uzantısını](../../cdn/cdn-create-new-endpoint.md) kullanabilirsiniz.

1. Azure portal depolama hesabınızı bulun ve hesaba genel bakış ' ı görüntüleyin.

2. Seçin **Azure CDN** altında **Blob hizmeti** Azure CDN yapılandırmak için menüsünde.

    **Azure CDN** sayfası görünür.

    ![CDN uç noktası oluşturma](../../cdn/media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-creation.png)

3. **CDN profili** bölümünde yeni veya var olan bir CDN profili belirtin. 

4. CDN uç noktası için bir fiyatlandırma katmanı belirtin. Fiyatlandırma hakkında daha fazla bilgi için bkz. [Content Delivery Network fiyatlandırması](https://azure.microsoft.com/pricing/details/cdn/). Her katmanda kullanılabilen özellikler hakkında daha fazla bilgi için bkz. [Azure CDN ürün özelliklerini karşılaştırma](../../cdn/cdn-features.md).

5. **CDN uç noktası adı** alanında, CDN uç noktanız için bir ad belirtin. CDN uç noktası Azure genelinde benzersiz olmalıdır.

6. **Kaynak ana bilgisayar adı** alanında, sizin için statik Web sitesi uç noktası olduğunuzu belirtin. 

   Statik Web sitesi uç noktanızı bulmak için, depolama hesabınız için **statik Web sitesi** ayarları ' na gidin.  Birincil uç noktayı kopyalayın ve CDN yapılandırmasına yapıştırın.

   > [!IMPORTANT]
   > Protokol tanımlayıcısını (*örn.* https) ve URL 'nin sonundaki eğik çizgiyi kaldırdığınızdan emin olun. Örneğin, statik Web sitesi uç noktası `https://mystorageaccount.z5.web.core.windows.net/`ise, **kaynak ana bilgisayar adı** alanında `mystorageaccount.z5.web.core.windows.net` belirtirsiniz.

   Aşağıdaki görüntüde örnek bir uç nokta yapılandırması gösterilmektedir:

   ![Örnek CDN uç noktası yapılandırmasını gösteren ekran görüntüsü](media/storage-blob-static-website-custom-domain/add-cdn-endpoint.png)

7. **Oluştur**' u seçin ve ardından yaymasını bekleyin. Uç nokta oluşturulduktan sonra uç nokta listesinde görünür.

8. CDN uç noktasının doğru şekilde yapılandırıldığını doğrulamak için, ayarlarına gitmek üzere uç noktaya tıklayın. Depolama hesabınız için CDN genel görünümünde, uç nokta ana bilgisayar adını bulun ve aşağıdaki görüntüde gösterildiği gibi uç noktaya gidin. CDN uç noktanızın biçimi `https://staticwebsitesamples.azureedge.net`benzer olacaktır.

    ![CDN uç noktasına genel bakış gösteren ekran görüntüsü](media/storage-blob-static-website-custom-domain/verify-cdn-endpoint.png)

9. CDN uç noktası yayma işlemi tamamlandıktan sonra, CDN uç noktasına gitmek daha önce statik Web sitenize yüklediğiniz index. html dosyasının içeriğini görüntüler.

10. CDN uç noktanıza ait kaynak ayarlarını gözden geçirmek için, CDN uç noktanıza ilişkin **Ayarlar** bölümünde **kaynak** ' a gidin. **Kaynak türü** alanının *özel kaynak* olarak ayarlandığını ve **kaynak konak adı** alanının statik Web sitesi uç noktasını görüntülediğini görürsünüz.

    ![CDN uç noktası için kaynak ayarlarını gösteren ekran görüntüsü](media/storage-blob-static-website-custom-domain/verify-cdn-origin.png)

## <a name="remove-content-from-azure-cdn"></a>Azure CDN'den içerik kaldırma

Bir nesneyi Azure CDN'de artık önbelleğe almak istemiyorsanız, aşağıdaki adımlardan birini uygulayabilirsiniz:

* Kapsayıcıyı genel yerine özel yapma. Daha fazla bilgi için bkz. [Kapsayıcılara ve bloblara anonim okuma erişimini yönetme](storage-manage-access-to-resources.md).
* Azure portalı kullanarak CDN uç noktasını devre dışı bırakın veya silin.
* Barındırılan hizmetinizi nesne için isteklere artık yanıt vermeyecek şekilde değiştirin.

Azure CDN'de daha önce önbelleğe alınmış bir nesne, yaşam süresi sona erene veya uç nokta [temizlenene](../../cdn/cdn-purge-endpoint.md) kadar önbellekte kalır. Yaşam süresi sona erdiğinde, Azure CDN, CDN uç noktasının hala geçerli ve nesnenin hala anonim olarak erişilebilir durumda olup olmadığını belirler. Bunlar söz konusu değilse nesne artık önbelleğe alınmaz.

## <a name="next-steps"></a>Sonraki adımlar

Seçim Azure CDN uç noktanıza özel bir etki alanı ekleyin. Bkz. [öğretici: Azure CDN uç noktanıza özel etki alanı ekleme](../../cdn/cdn-map-content-to-custom-domain.md).
