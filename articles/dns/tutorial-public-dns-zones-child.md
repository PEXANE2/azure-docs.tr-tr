---
title: Azure alt DNS bölgeleri oluşturma
titleSuffix: Azure DNS
description: Azure portal 'de alt DNS bölgeleri oluşturma hakkında öğretici.
author: jonbeck
ms.assetid: be4580d7-aa1b-4b6b-89a3-0991c0cda897
ms.service: dns
ms.topic: tutorial
ms.custom: ''
ms.workload: infrastructure-services
ms.date: 7/16/2020
ms.author: jonbeck
ms.openlocfilehash: 3f35d39634470ccacffa4d35c272a82725e9001c
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89088366"
---
# <a name="tutorial-creating-a-new-child-dns-zone"></a>Öğretici: yeni bir alt DNS bölgesi oluşturma

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz: 

> [!div class="checklist"]
> * Azure portalında oturum açma.
> * Yeni DNS bölgesi aracılığıyla alt DNS bölgesi oluşturuluyor.
> * Üst DNS bölgesi üzerinden alt DNS bölgesi oluşturuluyor.
> * Yeni alt DNS bölgesi için NS temsili doğrulanıyor.



## <a name="prerequisites"></a>Önkoşullar

* Etkin aboneliği olan bir Azure hesabı.  Hesabınız yoksa, [ücretsiz olarak bir hesap oluşturabilirsiniz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Mevcut üst Azure DNS bölgesi.  

Bu öğreticinin amacı doğrultusunda, contoso.com üst bölge olarak, alt etki alanı adı olarak subdomain.contoso.com kullanacağız.  *Contoso.com* değerini üst etki alanınızın adı ve alt etki alanı ile alt etki *alanınız ile değiştirin* .  Üst DNS bölgenizi oluşturmadıysanız, [Azure Portal kullanarak DNS bölgesi oluşturma](https://docs.microsoft.com/azure/dns/dns-getstarted-portal#create-a-dns-zone)adımlarına bakın. 


## <a name="sign-in-to-azure-portal"></a>Azure portalda oturum açın

Azure hesabınızla [Azure portalında](https://portal.azure.com/) oturum açın.
Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun.

Alt DNS bölgenizi oluşturmak için iki yol vardır.
1.  "DNS bölgesi oluşturma" portalı sayfası.
1.  Üst DNS bölgesinin yapılandırma sayfasından.


## <a name="create-child-dns-zone-via-create-dns-zone"></a>DNS bölgesi oluştur aracılığıyla alt DNS bölgesi oluştur

Bu adımda, **subdomain.contoso.com** adlı yeni BIR alt DNS bölgesi oluşturacak ve var olan üst dns bölgesi **contoso.com**' a temsilci seçeceğiz. DNS bölgesi **Oluştur** sayfasındaki SEKMELERI kullanarak DNS bölgesini oluşturacaksınız.
1.  Azure portal menüsünde veya **Giriş** sayfasında **Kaynak oluştur**’u seçin. **Yeni** pencere görüntülenir.
1.  **Ağ**' ı ve ardından **DNS bölgesi** ' ni seçip düğme **Ekle** ' yi seçin.

1.  **Temel bilgiler** sekmesinde, aşağıdaki değerleri yazın veya seçin:
    * **Abonelik**: bölgeyi oluşturmak için bir abonelik seçin.
    * **Kaynak grubu**: mevcut kaynak grubunuzu girin veya **Yeni oluştur**' u seçerek yeni bir tane oluşturabilir, *myresourcegroup*girin ve **Tamam**' ı seçin. Kaynak grubu adı, Azure aboneliği dahilinde benzersiz olmalıdır.
    * Bu onay kutusunu işaretleyin: **Bu bölge, zaten Azure DNS barındırılan mevcut bir bölgenin alt öğesidir**
    * **Üst bölge aboneliği**: Bu açılan listeden, *contoso.com* üst DNS bölgesinin oluşturulduğu abonelik adını arayın ve/veya seçin.
    * **Üst bölge**: arama çubuğunda, açılan listede yüklemek için *contoso.com* yazın. Yüklendikten sonra, açılan listeden *contoso.com* seçin.
    * **Ad:** Bu öğretici örneği için alt *etki alanı* yazın. Yukarıdaki adımdan üst bölge ' yi seçtiğimiz sırada üst DNS bölge adınızın *contoso.com* otomatik olarak ada sonek olarak eklendiğinden emin olun.

1. **Sonraki: Gözden geçirme ve oluşturma**’yı seçin.
1. **Gözden geçir + oluştur** sekmesinde, Özeti gözden geçirin, doğrulama hatalarını düzeltin ve ardından **Oluştur**' u seçin.
Bölgenin oluşturulması birkaç dakika sürebilir.

 
    :::image type="content" source="./media/dns-delegate-domain-azure-dns/create-dns-zone-inline.png" alt-text="DNS bölgesi oluştur sayfasının ekran görüntüsü." lightbox="./media/dns-delegate-domain-azure-dns/create-dns-zone-expanded.png":::

## <a name="create-child-dns-zone-via-parent-dns-zone-overview-page"></a>Üst DNS bölgesine genel bakış sayfası aracılığıyla alt DNS bölgesi oluştur
Ayrıca, üst bölgeye genel bakış sayfasından **alt bölge** düğmesini kullanarak yeni BIR alt DNS bölgesi oluşturabilir ve bunu üst DNS bölgesine atayabilirsiniz. Bu düğmenin kullanılması otomatik olarak alt bölgenin ana parametrelerini otomatik olarak doldurur. 

1.  Azure portal, **tüm kaynaklar**altında, **myresourcegroup** kaynak grubundaki *contoso.com* DNS bölgesini açın. Daha kolay bulmak için **ada göre filtrele** kutusuna *contoso.com* yazabilirsiniz.
1.  DNS bölgesine Genel Bakış sayfasında **+ alt bölge** düğmesini seçin.

      :::image type="content" source="./media/dns-delegate-domain-azure-dns/create-child-zone-inline.png" alt-text="Ekran görüntüsü alt bölge düğmesi." border="true" lightbox="./media/dns-delegate-domain-azure-dns/create-child-zone-expanded.png":::

1.  DNS bölgesi oluştur sayfası açılır. Alt bölge seçeneği zaten işaretli ve üst bölge aboneliği ve üst bölgesi bu sayfada zaten doldurulmuş.
1.  Bu öğretici örneği için bu adı *alt öğe* olarak yazın. Contoso.com üst DNS bölgesi adı, otomatik olarak ada önek olarak eklendiğine dikkat edin.
1.  Ileri ' yi seçin **: Etiketler** ve sonra **İleri: İnceleme + oluştur**.
1.  **Gözden geçir + oluştur** sekmesinde, Özeti gözden geçirin, doğrulama hatalarını düzeltin ve ardından **Oluştur**' u seçin.

    :::image type="content" source="./media/dns-delegate-domain-azure-dns/create-dns-zone-child-inline.png" alt-text="Seçili alt bölge ekran görüntüsü" border="true"  lightbox="./media/dns-delegate-domain-azure-dns/create-dns-zone-child-expanded.png":::
## <a name="verify-child-dns-zone"></a>Alt DNS bölgesini doğrula
Artık yeni bir alt DNS bölge *subdomain.contoso.com* oluşturmuş olduğunuza göre. Temsilcinin doğru şekilde gerçekleştiğini doğrulamak için, alt bölgeniz için nameserver (NS) kayıtlarının, aşağıda açıklandığı gibi üst bölgede olduğunu kontrol etmek isteyeceksiniz.  

**Alt DNS bölgesinin ad sunucularını al:**

1.  Azure portal, **tüm kaynaklar**altında, **myresourcegroup** kaynak grubundaki *subdomain.contoso.com* DNS bölgesini açın. Daha kolay bulmak için **ada göre filtrele** kutusuna *subdomain.contoso.com* yazabilirsiniz.
1.  DNS bölgesine genel bakış sayfasından ad sunucularını alın. Bu örnekte, contoso.com bölgesine ad Servers *ns1-08.Azure-DNS.com, ns2-08.Azure-DNS.net, NS3-08.Azure-DNS.org*ve *NS4-08.Azure-DNS.info*atanmıştır:

      :::image type="content" source="./media/dns-delegate-domain-azure-dns/create-child-zone-ns-inline.png" alt-text="Alt bölge kullanır ekran görüntüsü" border="true" lightbox="./media/dns-delegate-domain-azure-dns/create-child-zone-ns-expanded.png":::
**Üst DNS bölgesindeki NS kaydını doğrulayın:**

Bu adımda, üst DNS bölgesine *contoso.com* ve alt bölgeler kullanır için NS kayıt kümesi girişinin oluşturulup oluşturulmadığımıza göz atın.

1. Azure portal, **tüm kaynaklar**altında, **myresourcegroup** kaynak grubundaki contoso.com DNS bölgesini açın. Daha kolay bulmak için **ada göre filtrele** kutusuna contoso.com yazabilirsiniz.
1.  *Contoso.com* DNS bölgelerine Genel Bakış sayfasında, kayıt kümelerini denetleyin.
1.  NS ve Name alt etki alanı türündeki kayıt kümesinin zaten üst DNS bölgesinde oluşturulduğunu göreceksiniz. Bu kayıt kümesinin değerlerini kontrol edin ve yukarıdaki adımda alt DNS bölgesinden elde ettiğimiz ad sunucusu listesine benzer.

     :::image type="content" source="./media/dns-delegate-domain-azure-dns/create-child-zone-ns-validate-inline.png" alt-text="Alt bölge kullanır doğrulamasının ekran görüntüsü" border="true" lightbox="./media/dns-delegate-domain-azure-dns/create-child-zone-ns-validate-expanded.png":::
## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu öğreticide oluşturduğunuz kaynaklara artık ihtiyacınız kalmadığında, **Myresourcegroup** kaynak grubunu silerek bunları kaldırın. **Myresourcegroup** kaynak grubunu açın ve **kaynak grubunu sil**' i seçin.



## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure DNS Özel Bölgeleri senaryoları](private-dns-scenarios.md)
