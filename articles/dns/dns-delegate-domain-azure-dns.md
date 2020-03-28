---
title: 'Öğretici: Etki alanınızı ve alt etki alanınızı barındırın - Azure DNS'
description: Bu makalede, Azure DNS'yi DNS bölgelerinizi barındıracak şekilde nasıl yapılandırılacak öğrenin.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: tutorial
ms.date: 3/11/2019
ms.author: rohink
ms.openlocfilehash: 8f29a2bbe0eb392927dd111b13e2260111ddd18e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79238975"
---
# <a name="tutorial-host-your-domain-in-azure-dns"></a>Öğretici: Azure DNS’te etki alanınızı barındırma

Azure DNS'yi DNS etki alanınızı barındırmak ve DNS kayıtlarınızı yönetmek için kullanabilirsiniz. Etki alanlarınızı Azure'da barındırarak DNS kayıtlarınızı diğer Azure hizmetlerinde kullandığınız kimlik bilgileri, API’ler, araçlar ve faturalarla yönetebilirsiniz.

Örneğin, contoso.net etki alanını bir etki alanı adı kayıt şirketinden satın aldığınızı ve Azure DNS'de contoso.net adlı bir bölge oluşturduğunuzu varsayalım. Etki alanının sahibi olduğunuzdan, kayıt şirketiniz size etki alanınız için ad sunucusu (NS) kayıtlarını yapılandırma seçeneğini sunar. Kayıt kuruluşu bu NS kayıtlarını .net üst alanında depolar. Dünyanın dört bir yanındaki Internet kullanıcıları, contoso.net'da DNS kayıtlarını çözmeye çalıştıklarında Azure DNS bölgenizdeki etki alanınıza yönlendirilir.


Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Bir DNS bölgesi oluşturun.
> * Ad sunucularının listesini alın.
> * Etki alanını temsilciolarak.
> * Delegasyonun çalıştığını doğrulayın.


Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Azure DNS'de barındırabileceğiniz bir etki alanı adınız olmalıdır. Bu etki alanı üzerinde tam denetime sahip olmanız gerekir. Tam denetim, etki alanı için ad sunucusu (NS) kayıtlarını ayarlama olanağını kapsar.

Bu öğretici için kullanılan örnek etki alanı contoso.net, ancak kendi etki alanı adınızı kullanın.

## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

1. Bir DNS bölgesi oluşturmak için [Azure portalına](https://portal.azure.com/) gidin. **DNS bölgelerini**arayın ve seçin.

   ![DNS bölgesi](./media/dns-delegate-domain-azure-dns/openzone650.png)

1. **DNS bölgesi oluştur'u**seçin.
1. **DNS bölgesi oluştur** sayfasında aşağıdaki değerleri girin ve **Oluştur**’u seçin:

   | **Ayar** | **Değer** | **Şey** |
   |---|---|---|
   |**Adı**|[etki alanı adınız] |Satın aldığınız etki alanı adı. Bu öğreticide örnek olarak contoso.net kullanılmıştır.|
   |**Abonelik**|[Aboneliğiniz]|Bölgenin oluşturulacağı bir abonelik seçin.|
   |**Kaynak grubu**|**Yeni oluştur:** contosoRG|Bir kaynak grubu oluşturun. Kaynak grubu adı, seçtiğiniz abonelik içinde benzersiz olmalıdır.<br>Kaynak grubunun konumunu ifade eder ve DNS bölgesini etkilemez. DNS bölge konumu her zaman "genel"dir ve gösterilmez.|
   |**Konum**|Doğu ABD||

## <a name="retrieve-name-servers"></a>Ad sunucularını alma

DNS bölgenizi Azure DNS'ye devretmeden önce, bölgenizin ad sunucularını bilmeniz gerekir. Azure DNS, her bölge oluşturmada bir havuzdan ad sunucuları ayırır.

1. Oluşturulan DNS bölgesiyle, Azure Portal **Sık Kullanılanlar** bölmesinde, **Tüm kaynaklar**’ı seçin. **Tüm kaynaklar** sayfasında DNS bölgenizi seçin. Seçtiğiniz aboneliğin içinde zaten birkaç kaynak varsa, uygulama ağ geçidine kolayca erişmek için etki alanı adınızı **Filtre'ye ad** kutusuna girebilirsiniz. 

1. DNS bölgesi sayfasından ad sunucularını alın. Bu örnekte, bölge contoso.net ad sunucuları *ns1-01.azure-dns.com*, *ns2-01.azure-dns.net*, *ns3-01.azure-dns.org*ve *ns4-01.azure-dns.info*atanmıştır:

   ![Ad sunucularının listesi](./media/dns-delegate-domain-azure-dns/viewzonens500.png)

Azure DNS, bölgenizdeki yetkili NS kayıtlarını atanan ad sunucularını içerecek şekilde otomatik olarak oluşturur.

## <a name="delegate-the-domain"></a>Etki alanını devretme

Artık DNS bölgesi oluşturulduğuna ve ad sunucularınız olduğuna göre, üst etki alanını Azure DNS ad sunucularıyla güncelleştirmeniz gerekir. Her kayıt şirketi, bir etki alanının ad sunucusu kayıtlarını değiştirmek için kendi DNS yönetim araçlarına sahiptir. 

1. Kayıt şirketinin DNS yönetim sayfasında NS kayıtlarını düzenleyin ve NS kayıtlarını Azure DNS ad sunucularıyla değiştirin.

1. Bir etki alanını Azure DNS'ye devrettiğinizde, Azure DNS'nin sağladığı ad sunucularını kullanmanız gerekir. Etki alanınızın adından bağımsız olarak dört ad sunucusunun tümünü kullanın. Etki alanı delegasyonu, etki alanınızla aynı üst düzey etki alanını kullanmak için bir ad sunucusu gerektirmez.

> [!NOTE]
> Ad sunucusu adreslerini kopyalarken adresinin sonundaki noktayı da kopyaladığınızdan emin olun. Sondaki nokta bir tam etki alanı adının sonuna gösterir. Bazı kayıt şirketleri, NS adının sonunda bu süre yoksa dönemi ekler. DNS RFC ile uyumlu olmak için, son daki dönemi içerir.

Bazen *vanity ad sunucuları*olarak da adlandırılan kendi bölgenizde ad sunucuları kullanan temsilcilikler şu anda Azure DNS'de desteklenmez.

## <a name="verify-the-delegation"></a>Temsilciyi doğrula

Temsilciliği tamamladıktan sonra, bölgeniz için Yetki Başlangıcı (SOA) kaydını sorgulamak için *nslookup* gibi bir araç kullanarak çalıştığını doğrulayabilirsiniz. SOA kaydı, bölge oluşturulurken otomatik olarak oluşturulur. Delegasyonu tamamladıktan sonra, çalıştığını başarılı bir şekilde doğrulamadan önce 10 dakika veya daha fazla beklemeniz gerekebilir. Değişikliklerin DNS sisteminde yayılması daha uzun sürebilir.

Azure DNS ad sunucularını belirtmeniz gerekmez. Temsilci seçimi doğru ayarlanmışsa normal DNS çözümleme işlemi ad sunucularını otomatik olarak bulur.

1. Komut isteminden, aşağıdaki örneğe benzer bir nslookup komutu girin:

   ```
   nslookup -type=SOA contoso.net
   ```

1. Yanıtınızın aşağıdaki nslookup çıktısına benzediğini doğrulayın:

   ```
   Server: ns1-04.azure-dns.com
   Address: 208.76.47.4

   contoso.net
   primary name server = ns1-04.azure-dns.com
   responsible mail addr = msnhst.microsoft.com
   serial = 1
   refresh = 900 (15 mins)
   retry = 300 (5 mins)
   expire = 604800 (7 days)
   default TTL = 300 (5 mins)
   ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki öğreticiye geçmeyi düşünüyorsanız **contosoRG** kaynak grubunu tutabilirsiniz. Aksi halde **contosoRG** kaynak grubunu silerek bu öğreticide oluşturulan kaynakları silebilirsiniz.

- **ContosoRG** kaynak grubunu seçin ve sonra **kaynak grubunu sil'i**seçin. 

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, etki alanınız için bir DNS bölgesi oluşturdunuz ve onu Azure DNS'ye devrettiniz. Azure DNS ve web uygulamaları hakkında daha fazla bilgi için web uygulaması öğreticileriyle devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Özel etki alanında web uygulaması için DNS kayıtları oluşturma](./dns-web-sites-custom-domain.md)