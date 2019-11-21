---
title: 'Tutorial: Host your domain and subdomain - Azure DNS'
description: In this article, learn how to configure Azure DNS to host your DNS zones.
services: dns
author: asudbring
ms.service: dns
ms.topic: tutorial
ms.date: 3/11/2019
ms.author: allensu
ms.openlocfilehash: 062a5beaec30d510d37af436e00f4d57785245cd
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74212220"
---
# <a name="tutorial-host-your-domain-in-azure-dns"></a>Öğretici: Azure DNS’te etki alanınızı barındırma

Azure DNS'yi DNS etki alanınızı barındırmak ve DNS kayıtlarınızı yönetmek için kullanabilirsiniz. Etki alanlarınızı Azure'da barındırarak DNS kayıtlarınızı diğer Azure hizmetlerinde kullandığınız kimlik bilgileri, API’ler, araçlar ve faturalarla yönetebilirsiniz.

Örneğin, contoso.net etki alanını bir etki alanı adı kayıt şirketinden satın aldığınızı ve Azure DNS'de contoso.net adlı bir bölge oluşturduğunuzu varsayalım. Etki alanının sahibi olduğunuzdan, kayıt şirketiniz size etki alanınız için ad sunucusu (NS) kayıtlarını yapılandırma seçeneğini sunar. Kayıt kuruluşu bu NS kayıtlarını .net üst alanında depolar. Internet users around the world are then directed to your domain in your Azure DNS zone when they try to resolve DNS records in contoso.net.


Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Create a DNS zone.
> * Retrieve a list of name servers.
> * Delegate the domain.
> * Verify the delegation is working.


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

You must have a domain name available to test with that you can host in Azure DNS . Bu etki alanı üzerinde tam denetime sahip olmanız gerekir. Tam denetim, etki alanı için ad sunucusu (NS) kayıtlarını ayarlama olanağını kapsar.

The example domain used for this tutorial is contoso.net, but use your own domain name.

## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

1. Go to the [Azure portal](https://portal.azure.com/) to create a DNS zone. Search for and select **DNS zones**.

   ![DNS bölgesi](./media/dns-delegate-domain-azure-dns/openzone650.png)

1. Select **Create DNS zone**.
1. **DNS bölgesi oluştur** sayfasında aşağıdaki değerleri girin ve **Oluştur**’u seçin:

   | **Ayar** | **Değer** | **Ayrıntılar** |
   |---|---|---|
   |**Adı**|[etki alanı adınız] |Satın aldığınız etki alanı adı. Bu öğreticide örnek olarak contoso.net kullanılmıştır.|
   |**Abonelik**|[Aboneliğiniz]|Bölgenin oluşturulacağı bir abonelik seçin.|
   |**Kaynak grubu**|**Yeni oluştur:** contosoRG|Bir kaynak grubu oluşturun. Kaynak grubu adı, seçtiğiniz abonelik içinde benzersiz olmalıdır.<br>Kaynak grubunun konumunu ifade eder ve DNS bölgesini etkilemez. The DNS zone location is always "global," and isn't shown.|
   |**Konum**|Doğu ABD||

## <a name="retrieve-name-servers"></a>Ad sunucularını alma

DNS bölgenizi Azure DNS'ye devretmeden önce, bölgenizin ad sunucularını bilmeniz gerekir. Azure DNS, her bölge oluşturmada bir havuzdan ad sunucuları ayırır.

1. Oluşturulan DNS bölgesiyle, Azure Portal **Sık Kullanılanlar** bölmesinde, **Tüm kaynaklar**’ı seçin. **Tüm kaynaklar** sayfasında DNS bölgenizi seçin. If the subscription that you selected already has several resources in it, you can enter your domain name in the **Filter by name** box to easily access the application gateway. 

1. DNS bölgesi sayfasından ad sunucularını alın. Bu örnekte, contoso.net bölgesine *ns1-01.azure-dns.com*, *ns2-01.azure-dns.net*, *ns3-01.azure-dns.org* ve *ns4-01.azure-dns.info* ad sunucuları atanmıştır:

   ![Ad sunucularının listesi](./media/dns-delegate-domain-azure-dns/viewzonens500.png)

Azure DNS, bölgenizdeki yetkili NS kayıtlarını atanan ad sunucularını içerecek şekilde otomatik olarak oluşturur.

## <a name="delegate-the-domain"></a>Etki alanını devretme

Artık DNS bölgesi oluşturulduğuna ve ad sunucularınız olduğuna göre, üst etki alanını Azure DNS ad sunucularıyla güncelleştirmeniz gerekir. Her kayıt şirketi, bir etki alanının ad sunucusu kayıtlarını değiştirmek için kendi DNS yönetim araçlarına sahiptir. 

1. Kayıt şirketinin DNS yönetim sayfasında NS kayıtlarını düzenleyin ve NS kayıtlarını Azure DNS ad sunucularıyla değiştirin.

1. When you delegate a domain to Azure DNS, you must use the name servers that Azure DNS provides. Use all four name servers, regardless of the name of your domain. Domain delegation doesn't require a name server to use the same top-level domain as your domain.

> [!NOTE]
> Ad sunucusu adreslerini kopyalarken adresinin sonundaki noktayı da kopyaladığınızdan emin olun. Sondaki nokta bir tam etki alanı adının sonuna gösterir. Some registrars append the period if the NS name doesn't have it at the end. To be compliant with the DNS RFC, include the trailing period.

Delegations that use name servers in your own zone, sometimes called *vanity name servers*, aren't currently supported in Azure DNS.

## <a name="verify-the-delegation"></a>Verify the delegation

After you complete the delegation, you can verify that it's working by using a tool such as *nslookup* to query the Start of Authority (SOA) record for your zone. SOA kaydı, bölge oluşturulurken otomatik olarak oluşturulur. You might need to wait 10 minutes or more after you complete the delegation, before you can successfully verify that it's working. Değişikliklerin DNS sisteminde yayılması daha uzun sürebilir.

You don't have to specify the Azure DNS name servers. Temsilci seçimi doğru ayarlanmışsa normal DNS çözümleme işlemi ad sunucularını otomatik olarak bulur.

1. From a command prompt, enter an nslookup command similar to the following example:

   ```
   nslookup -type=SOA contoso.net
   ```

1. Verify that your response looks similar to the following nslookup output:

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

- Select the **contosoRG** resource group, and then select **Delete resource group**. 

## <a name="next-steps"></a>Sonraki adımlar

In this tutorial, you created a DNS zone for your domain and delegated it to Azure DNS. Azure DNS ve web uygulamaları hakkında daha fazla bilgi için web uygulaması öğreticileriyle devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Özel etki alanında bir web uygulaması için DNS kayıtları oluşturma](./dns-web-sites-custom-domain.md)