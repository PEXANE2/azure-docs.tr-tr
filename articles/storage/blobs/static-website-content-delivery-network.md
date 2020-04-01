---
title: Statik bir web sitesini Azure CDN ile tümleştirme - Azure Depolama
description: Azure İçerik Dağıtım Ağı'nı (CDN) kullanarak bir Azure Depolama hesabından statik web sitesi içeriğini nasıl önbelleğe aldığınızı öğrenin.
author: normesta
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.author: normesta
ms.date: 01/22/2020
ms.openlocfilehash: 8eeff5187d27cb75b9e55eba8311dede8970bc4a
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80435227"
---
# <a name="integrate-a-static-website-with-azure-cdn"></a>Statik bir web sitesini Azure CDN ile tümleştirme

[Azure İçerik Teslim Ağı'nın (CDN)](../../cdn/cdn-overview.md) Azure depolama hesabında barındırılan statik bir web [sitesinden](storage-blob-static-website.md) içerik önbelleğe almalarını etkinleştirebilirsiniz. Azure CDN'yi statik web sitenizin özel etki alanı bitiş noktasını yapılandırmak, özel TLS/SSL sertifikaları sağlamak ve özel yeniden yazma kurallarını yapılandırmak için kullanabilirsiniz. Azure CDN'nin yapılandırılması ek ücretlerle sonuçlanır, ancak dünyanın her yerinden web sitenize tutarlı düşük gecikme süreleri sağlar. Azure CDN, kendi sertifikanızla TLS şifrelemesi de sağlar. 

Azure CDN fiyatlandırması hakkında bilgi için Azure [CDN fiyatlandırması'na](https://azure.microsoft.com/pricing/details/cdn/)bakın.

## <a name="enable-azure-cdn-for-your-static-website"></a>Statik web siteniz için Azure CDN'yi etkinleştirme

Statik web siteniz için Azure CDN'yi doğrudan depolama hesabınızdan etkinleştirebilirsiniz. CDN uç noktanız için [büyük dosya indirmeyi en iyi duruma getirme](../../cdn/cdn-optimization-overview.md#large-file-download) gibi gelişmiş yapılandırma ayarları belirtmek istiyorsanız, bir CDN profili ve uç noktası oluşturmak üzere bunun yerine [Azure CDN uzantısını](../../cdn/cdn-create-new-endpoint.md) kullanabilirsiniz.

1. Azure portalında depolama hesabınızı bulun ve hesaba genel bakışı görüntüleyin.

2. Azure CDN'yi yapılandırmak için **Blob Hizmeti** menüsü nün altındaki **Azure CDN'yi** seçin.

    **Azure CDN** sayfası görünür.

    ![CDN uç noktası oluşturma](../../cdn/media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-creation.png)

3. **CDN profil** bölümünde yeni veya varolan bir CDN profili belirtin. 

4. CDN bitiş noktası için bir fiyatlandırma katmanı belirtin. Fiyatlandırma hakkında daha fazla bilgi edinmek için [İçerik Dağıtım Ağı fiyatlandırmasına](https://azure.microsoft.com/pricing/details/cdn/)bakın. Her katmanda kullanılabilen özellikler hakkında daha fazla bilgi için Azure [CDN ürün özelliklerini karşılaştır'a](../../cdn/cdn-features.md)bakın.

5. **CDN bitiş noktası adı** alanında, CDN bitiş noktanız için bir ad belirtin. CDN bitiş noktası Azure genelinde benzersiz olmalıdır.

6. **Origin ana bilgisayar adı** alanında statik web sitesi bitiş noktası olduğunuzu belirtin. 

   Statik web sitesi bitiş noktanızı bulmak için depolama hesabınız için **Statik web sitesi** ayarlarına gidin.  Birincil bitiş noktasını kopyalayın ve CDN yapılandırmasına yapıştırın.

   > [!IMPORTANT]
   > Protokol tanımlayıcısını *(örn.* HTTPS) ve URL'deki son çizgiyi kaldırdıktan emin olun. Örneğin, statik web sitesi bitiş `https://mystorageaccount.z5.web.core.windows.net/`noktası ise, `mystorageaccount.z5.web.core.windows.net` **Origin ana bilgisayar adı** alanında belirtirsiniz.

   Aşağıdaki resimde bir örnek bitiş noktası yapılandırması gösterilmektedir:

   ![Örnek CDN bitiş noktası yapılandırması gösteren ekran görüntüsü](media/storage-blob-static-website-custom-domain/add-cdn-endpoint.png)

7. **Oluştur'u**seçin ve ardından yayılmasını bekleyin. Uç nokta oluşturulduktan sonra uç nokta listesinde görünür.

8. CDN bitiş noktasının doğru şekilde yapılandırıldığından doğrulamak için, ayarlarına gitmek için bitiş noktasını tıklatın. Depolama hesabınız için CDN genel bakışından, aşağıdaki resimde gösterildiği gibi bitiş noktası ana bilgisayar adını bulun ve bitiş noktasına gidin. CDN bitiş noktanızın biçimi `https://staticwebsitesamples.azureedge.net`.

    ![CDN bitiş noktasına genel bakışı gösteren ekran görüntüsü](media/storage-blob-static-website-custom-domain/verify-cdn-endpoint.png)

9. CDN uç noktası yayılımı tamamlandıktan sonra, CDN bitiş noktasına gezinmek, daha önce statik web sitenize yüklediğiniz index.html dosyasının içeriğini görüntüler.

10. CDN bitiş noktanızın başlangıç ayarlarını gözden geçirmek için, CDN bitiş noktanızın **Ayarlar** bölümünün altındaki **Origin'e** gidin. **Origin türü** alanının *Özel Başlangıç* olarak ayarladığını ve Origin ana **bilgisayar adı** alanının statik web sitesi bitiş noktanızı görüntülediğini görürsünüz.

    ![CDN bitiş noktası için Başlangıç ayarlarını gösteren ekran görüntüsü](media/storage-blob-static-website-custom-domain/verify-cdn-origin.png)

## <a name="remove-content-from-azure-cdn"></a>Azure CDN'den içerik kaldırma

Bir nesneyi Azure CDN'de artık önbelleğe almak istemiyorsanız, aşağıdaki adımlardan birini uygulayabilirsiniz:

* Kapsayıcıyı genel yerine özel yapma. Daha fazla bilgi için bkz. [Kapsayıcılara ve bloblara anonim okuma erişimini yönetme](storage-manage-access-to-resources.md).
* Azure portalı kullanarak CDN uç noktasını devre dışı bırakın veya silin.
* Barındırılan hizmetinizi nesne için isteklere artık yanıt vermeyecek şekilde değiştirin.

Azure CDN'de daha önce önbelleğe alınmış bir nesne, yaşam süresi sona erene veya uç nokta [temizlenene](../../cdn/cdn-purge-endpoint.md) kadar önbellekte kalır. Yaşam süresi sona erdiğinde, Azure CDN, CDN uç noktasının hala geçerli ve nesnenin hala anonim olarak erişilebilir durumda olup olmadığını belirler. Bunlar söz konusu değilse nesne artık önbelleğe alınmaz.

## <a name="next-steps"></a>Sonraki adımlar

(İsteğe bağlı) Azure CDN bitiş noktanıza özel bir etki alanı ekleyin. Bkz. [Öğretici: Azure CDN bitiş noktanıza özel bir etki alanı ekleyin.](../../cdn/cdn-map-content-to-custom-domain.md)
