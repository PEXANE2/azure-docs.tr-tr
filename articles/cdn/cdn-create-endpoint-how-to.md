---
title: Azure CDN bitiş noktası oluşturma | Microsoft Dokümanlar
description: Bu makalede, gelişmiş ayarlar da dahil olmak üzere yeni bir Azure İçerik Teslim Ağı (CDN) bitiş noktası nasıl oluşturulacak gösterilmektedir.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.service: azure-cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: allensu
ms.custom: mvc
ms.openlocfilehash: 0a130a433c68d0d5cc8c26eae4b81ff264eb0ca2
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81254024"
---
# <a name="create-an-azure-cdn-endpoint"></a>Azure CDN bitiş noktası oluşturma
Bu makalede, varolan bir CDN profilinde bir [Azure İçerik Teslim Ağı (CDN)](cdn-overview.md) bitiş noktası oluşturmak için tüm ayarları açıklanmaktadır. Bir profil ve bitiş noktası oluşturduktan sonra, müşterilerinize içerik sunmaya başlayabilirsiniz. Profil ve bitiş noktası oluşturmaya hızlı bir başlangıç için [Bkz. Hızlı Başlangıç: Azure CDN profili ve bitiş noktası oluşturun.](cdn-create-new-endpoint.md)

## <a name="prerequisites"></a>Ön koşullar
CDN bitiş noktası oluşturmadan önce, bir veya daha fazla CDN uç noktası içerebilen en az bir CDN profili oluşturmuş olmalısınız. CDN uç noktalarınızı internet etki alanı, web uygulaması veya başka ölçütlere göre düzenlemek için birden çok profil kullanabilirsiniz. CDN fiyatlandırması CDN profil düzeyinde uygulandığından, Azure CDN fiyatlandırma katmanlarının bir karışımını kullanmak istiyorsanız birden çok CDN profili oluşturmanız gerekir. CDN profili oluşturmak için [bkz.](cdn-create-new-endpoint.md#create-a-new-cdn-profile)

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma
Azure hesabınızla [Azure portalında](https://portal.azure.com) oturum açın.

## <a name="create-a-new-cdn-endpoint"></a>Yeni bir CDN uç noktası oluşturma

1. [Azure portalında](https://portal.azure.com), CDN profilinize gidin. Önceki adımda bunu panoya sabitlemiş olabilirsiniz. Aksi takdirde, **Tüm hizmetler**’i, sonra da **CDN profilleri**’ni seçerek bulabilirsiniz. **CDN profilleri** bölmesinde, uç noktanızı eklemeyi planladığınız profili seçin. 
   
    CDN profili bölmesi görünür.

2. **Bitiş Noktası'nı**seçin.
   
    ![CDN bitiş noktasını seçin](./media/cdn-create-endpoint-how-to/cdn-select-endpoint.png)
   
    **Uç nokta ekleyin** sayfası görüntülenir.
   
    ![Bitiş noktası sayfası ekleme](./media/cdn-create-endpoint-how-to/cdn-add-endpoint-page.png)

3. **Ad** için, yeni CDN uç noktasına yönelik benzersiz bir ad girin. Bu ad, önbelleğe alınmış kaynaklarınızı etki alanı _ \<bitiş noktası adından _>.azureedge.net'da erişmek için kullanılır.

4. **Başlangıç türü için,** aşağıdaki kaynak türlerinden birini seçin: 
   - Azure Depolama için **Depolama**
   - Azure Bulut Hizmetleri için **bulut hizmeti**
   - Azure Web Uygulamaları için **Web Uygulaması**
   - Herkese açık diğer web sunucusu (Azure veya başka bir yerde barındırılan) için **özel menşe**

5. **Origin ana bilgisayar adı**için, başlangıç sunucu etki alanınızı seçin veya girin. Açılır, adım 4'te belirttiğiniz türdeki tüm kullanılabilir başlangıç sunucularını listeler. Başlangıç türünüz olarak **Özel menşeseçtiyseniz,** özel kaynak sunucunuzun etki alanını girin.
    
6. **Başlangıç yolu**için, önbellek yapmak istediğiniz kaynaklara giden yolu girin. Adım 5'te belirttiğiniz etki alanında herhangi bir kaynağın önbelleğe almasına izin vermek için bu ayarı boş bırakın.
    
7. **Kaynak ana bilgisayar üst bilgisi** için, Azure CDN'nin her bir istekle göndermesini istediğiniz ana bilgisayar üst bilgisini girin veya varsayılan değeri bırakın.
   
   > [!NOTE]
   > Azure Storage ve Web Apps gibi bazı kaynak türleri, ana bilgisayar üst bilgisinin kaynağın etki alanı ile eşleşmesini gerektirir. Etki alanından farklı ana bilgisayar üst bilgisi gerektiren bir kaynağa sahip değilseniz varsayılan değeri bırakmanız gerekir.
   > 
    
8. **Protokol** ve **Başlangıç bağlantı noktası**için, kaynak sunucusundaki kaynaklara erişmek için kullanılacak protokolleri ve bağlantı noktalarını belirtin. En az bir protokol (HTTP veya HTTPS) seçilmelidir. HTTPS içeriğine erişmek için CDN tarafından sağlanan etki alanını_\<(bitiş adı>_.azureedge.net) kullanın. 
   
   > [!NOTE]
   > **Başlangıç bağlantı noktası** değeri, yalnızca bitiş noktasının başlangıç sunucusundan bilgi almak için kullandığı bağlantı noktasını belirler. Uç noktanın kendisi, **Kaynak bağlantı noktası** değerinden bağımsız olarak, yalnızca varsayılan HTTP ve HTTPS bağlantı noktalarındaki (80 ve 443) uç istemciler tarafından kullanılabilir.  
   > 
   > **Akamai'den Azure CDN** profillerindeki uç noktalar, kaynak bağlantı noktaları için tam TCP bağlantı noktası aralığına izin vermez. İzin verilmeyen kaynak bağlantı noktalarının listesi için bkz. [Akamai'den Azure CDN İzin Verilen Kaynak Bağlantı Noktaları](/previous-versions/azure/mt757337(v=azure.100)).  
   > 
   > Azure CDN özel etki alanları için HTTPS **desteği, Akamai ürünlerinden Azure CDN'de** desteklenmez. Daha fazla bilgi için bkz. [Azure CDN özel etki alanı üzerinde HTTPS'yi yapılandırma](cdn-custom-ssl.md).
    
9. **Optimize için,** bitiş noktasının teslim etmesini istediğiniz senaryo ve içerik türüyle en iyi eşleşen bir optimizasyon türü seçin. Daha fazla bilgi [için, içerik teslim türü için Azure CDN'sini Optimize Et'e](cdn-optimization-overview.md)bakın.

    Profil türüne göre aşağıdaki optimizasyon türü ayarları desteklenir:
    - **Microsoft profillerinden Azure CDN Standardı:**
       - [**Genel web teslimatı**](cdn-optimization-overview.md#general-web-delivery)

    - **Verizon'dan Azure CDN Standardı** ve **Verizon profillerinden Azure CDN Premium:**
       - [**Genel web teslimatı**](cdn-optimization-overview.md#general-web-delivery)
       - [**Dinamik alan ivmesi**](cdn-optimization-overview.md#dynamic-site-acceleration)

    - **Akamai profillerinden Azure CDN Standardı:**
       - [**Genel web teslimatı**](cdn-optimization-overview.md#general-web-delivery)
       - [**Genel medya akışı**](cdn-optimization-overview.md#general-media-streaming)
       - [**İsteğe bağlı medya akışı üzerine video**](cdn-optimization-overview.md#video-on-demand-media-streaming)
       - [**Büyük dosya indirme**](cdn-optimization-overview.md#large-file-download)
       - [**Dinamik alan ivmesi**](cdn-optimization-overview.md#dynamic-site-acceleration)

10. Yeni uç nokta oluşturmak için **Ekle**’yi seçin.
   
    Uç nokta oluşturulduktan sonra, profile yönelik uç noktalar listesinde görünür.
    
    ![CDN uç noktası](./media/cdn-create-new-endpoint/cdn-endpoint-success.png)
    
    Kaydın yayılması zaman alacağından, uç nokta hemen kullanılabilir olmaz: 
    - **Microsoft’tan Azure CDN Standart** profilleri için yayma işlemi genellikle 10 dakikada tamamlanır. 
    - **Akamai’den Azure CDN Standart** profilleri için yayma işlemi genellikle bir dakika içinde tamamlanır. 
    - **Verizon’dan Azure CDN Standart** ve **Verizon’dan Azure CDN Premium** profilleri için yayma işlemi genellikle 90 dakika içinde tamamlanır. 
   
    Son nokta yapılandırması olay noktası (POP) sunucularına yayılmadan önce CDN etki alanı adını kullanmaya çalışırsanız, http 404 yanıt durumu alabilirsiniz. Bitiş noktanızı oluşturmanızın üzerinden birkaç saat geçtiyse ve hala bir 404 yanıt durumu alıyorsanız, [404 durum kodunu döndüren Sorun Giderme Azure CDN uç noktalarına](cdn-troubleshoot-endpoint.md)bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık ihtiyaç duyulmadığında bir bitiş noktasını silmek için, onu seçin ve sonra **Sil'i**seçin. 

## <a name="next-steps"></a>Sonraki adımlar
Özel etki alanları hakkında bilgi edinmek için, CDN bitiş noktanıza özel bir etki alanı ekleme eğitimine devam edin.

> [!div class="nextstepaction"]
> [Özel etki alanı ekleme](cdn-map-content-to-custom-domain.md)


