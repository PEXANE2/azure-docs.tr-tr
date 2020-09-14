---
title: Azure özel uç noktası (Önizleme) kullanarak bir Web uygulamasına özel olarak bağlanma
description: Bu makalede, Azure özel uç noktası (Önizleme) kullanılarak bir Web uygulamasına özel olarak nasıl bağlanabileceğiniz açıklanır.
author: ericgre
ms.assetid: b8c5c7f8-5e90-440e-bc50-38c990ca9f14
ms.topic: how-to
ms.date: 09/08/2020
ms.author: ericg
ms.service: app-service
ms.workload: web
ms.openlocfilehash: ccbcdbe9204120e1cf181136f566556ec30be871
ms.sourcegitcommit: 814778c54b59169c5899199aeaa59158ab67cf44
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2020
ms.locfileid: "90054543"
---
# <a name="connect-privately-to-a-web-app-by-using-azure-private-endpoint-preview"></a>Azure özel uç noktası (Önizleme) kullanarak bir Web uygulamasına özel olarak bağlanma

Azure özel uç noktası (Önizleme), Azure özel bağlantısı için temel yapı taşıdır. Özel uç nokta kullanarak Web uygulamanıza özel olarak bağlanabilirsiniz. Bu makalede, Özel uç nokta kullanarak bir Web uygulamasını dağıtmayı ve ardından bir sanal makineden (VM) Web uygulamasına bağlanmayı öğreneceksiniz.

Daha fazla bilgi için bkz. [Azure Web uygulaması için özel uç noktaları kullanma][privateendpointwebapp].

> [!Note]
> Özel uç nokta (Önizleme), PremiumV2 katmanlı Windows Web uygulamaları, Linux Web uygulamaları ve Azure Işlevleri Premium planı (bazen elastik Premium plan olarak adlandırılır) için genel bölgelerde kullanılabilir. 

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Başlamadan önce [Azure Portal](https://portal.azure.com)oturum açın.

## <a name="create-a-virtual-network-and-virtual-machine"></a>Sanal ağ ve sanal makine oluşturma

Bu bölümde, bir Web uygulamasına özel bir uç nokta aracılığıyla erişmek için kullanacağınız bir VM 'yi barındırmak için bir sanal ağ ve alt ağ oluşturursunuz.

### <a name="create-the-virtual-network"></a>Sanal ağı oluşturma

Sanal ağ ve alt ağ oluşturmak için aşağıdakileri yapın:

1. Sol bölmede **kaynak oluştur**  >  **ağ**  >  **sanal ağ**' ı seçin.

1. **Sanal ağ oluştur** bölmesinde **temel bilgiler** sekmesini seçin ve ardından burada gösterilen bilgileri girin:

   > [!div class="mx-imgBorder"]
   > ![Azure portal "sanal ağ oluştur" bölmesinin ekran görüntüsü.][1]

1. **IP adresleri** sekmesini seçin ve ardından burada gösterilen bilgileri girin:

   > [!div class="mx-imgBorder"]
   > ![Sanal ağ oluştur bölmesinde "IP adresleri" sekmesinin ekran görüntüsü.][2]

1. **Alt ağ** bölümünde, **alt ağ ekle**' yi seçin, burada gösterilen bilgileri girin ve **Ekle**' yi seçin.

   > [!div class="mx-imgBorder"]
   > !["Alt ağ ekle" bölmesinin ekran görüntüsü.][3]

1. **Gözden geçir + oluştur**’u seçin.

1. Doğrulama başarılı olduktan sonra **Oluştur**' u seçin.

### <a name="create-the-virtual-machine"></a>Sanal makineyi oluşturma

Sanal makineyi oluşturmak için aşağıdakileri yapın:

1. Azure Portal, sol bölmede **kaynak oluşturma**  >  **işlem**  >  **sanal makinesi**' ni seçin.

1. **Sanal makine temel kavramları oluştur** bölmesinde, burada gösterilen bilgileri girin:

   > [!div class="mx-imgBorder"]
   > !["Sanal makine oluşturma" bölmesinin ekran görüntüsü.][4]

1. **İleri ' yi seçin: diskler**.

1. **Diskler** bölmesinde, varsayılan ayarları koruyun ve ardından İleri ' yi seçin **: ağ**.

1. **Ağ** bölmesine, burada gösterilen bilgileri girin:

   > [!div class="mx-imgBorder"]
   > !["Sanal makine oluşturma" bölmesindeki "ağ" sekmesinin ekran görüntüsü.][5]

1. **Gözden geçir + oluştur**’u seçin.

1. Doğrulama başarılı olduktan sonra **Oluştur**' u seçin.

## <a name="create-a-web-app-and-a-private-endpoint"></a>Web uygulaması ve özel uç nokta oluşturma

Bu bölümde, özel bir uç nokta kullanan özel bir Web uygulaması oluşturacaksınız.

> [!Note]
> Özel uç nokta özelliği yalnızca PremiumV2 katmanı için kullanılabilir.

### <a name="create-the-web-app"></a>Web uygulaması oluşturma

1. Azure Portal, sol bölmedeki **kaynak oluştur**  >  **Web**  >  **Web uygulaması**' nı seçin.

1. **Web uygulaması** bölmesinde **temel bilgiler** sekmesini seçin ve ardından burada gösterilen bilgileri girin:

   > [!div class="mx-imgBorder"]
   > !["Web uygulaması" bölmesindeki "temel bilgiler" sekmesinin ekran görüntüsü.][6]

1. **Gözden geçir + oluştur**’u seçin.

1. Doğrulama başarılı olduktan sonra **Oluştur**' u seçin.

### <a name="create-the-private-endpoint"></a>Özel uç nokta oluşturma

1. Web uygulaması özelliklerinde, **Ayarlar**altında **ağ**' ı seçin ve ardından **Özel uç nokta bağlantıları (Önizleme)** altında **Özel uç nokta bağlantılarınızı Yapılandır**' ı seçin.

   > [!div class="mx-imgBorder"]
   > ![Web uygulaması ağ bölmesinde "özel uç nokta bağlantılarınızı yapılandırma" bağlantısının ekran görüntüsü.][7]

1. **Özel uç nokta bağlantıları (Önizleme)** sihirbazında **Ekle**' yi seçin.

   > [!div class="mx-imgBorder"]
   > !["Özel uç nokta bağlantıları (Önizleme)" sihirbazındaki Ekle düğmesinin ekran görüntüsü.][8]

1. **Abonelik**, **sanal ağ**ve **alt ağ** açılan listelerinde doğru bilgileri seçin ve ardından **Tamam**' ı seçin.

   > [!div class="mx-imgBorder"]
   > !["Özel uç nokta Ekle (Önizleme)" bölmesinin ekran görüntüsü.][9]

1. Özel uç nokta oluşturma işleminin ilerlemesini izleyin.

   > [!div class="mx-imgBorder"]
   > ![Özel uç nokta ekleme ilerleme durumunun ekran görüntüsü. ][10]
   >  ![ Yeni oluşturulan özel uç noktanın ekran görüntüsü.][11]

## <a name="connect-to-the-vm-from-the-internet"></a>İnternet 'ten sanal makineye bağlanma

1. Azure portal **arama** kutusuna **myvm**yazın.
1. **Bağlan**' ı seçin ve ardından **RDP**' yi seçin.

   > [!div class="mx-imgBorder"]
   > !["MyVM" bölmesindeki "RDP" düğmesinin ekran görüntüsü.][12]

1. **RDP Ile Bağlan** bölmesinde **RDP dosyasını indir**' i seçin.  

   > [!div class="mx-imgBorder"]
   > !["RDP ile bağlan" bölmesindeki "RDP dosyasını Indir" düğmesinin ekran görüntüsü.][13]

   Azure bir Uzak Masaüstü Protokolü (RDP) dosyası oluşturur ve bilgisayarınıza indirir.   

1. İndirilen RDP dosyasını açın.

   a. İstemde, **Bağlan**' ı seçin.  
   b. VM oluştururken belirttiğiniz kullanıcı adını ve parolayı girin.

     > [!Note]
     > Bu kimlik bilgilerini kullanmak için **diğer seçimler**' i seçmeniz gerekebilir,  >  **farklı bir hesap kullanın**.

1. **Tamam**’ı seçin.

   > [!Note]
   > Oturum açma işlemi sırasında bir sertifika uyarısı alırsanız **Evet** ' i veya **devam et**' i seçin.

1. VM masaüstü penceresi göründüğünde, bunu yerel masaüstünüze geri dönmek için simge durumuna küçültün.

## <a name="access-the-web-app-privately-from-the-vm"></a>Web uygulamasına özel olarak VM 'den erişin

Bu bölümde özel uç nokta kullanarak Web uygulamasına özel olarak bağlanırsınız.

1. Özel uç noktanızın özel IP 'sini almak için, **arama** kutusuna **özel bağlantı** yazın ve ardından sonuçlar listesinde **özel bağlantı**' yı seçin.

   > [!div class="mx-imgBorder"]
   > ![Arama sonuçları listesindeki "özel bağlantı" bağlantısının ekran görüntüsü.][14]

1. Özel bağlantı merkezinde, sol bölmede Özel uç **noktalar** ' ı seçerek özel uç noktalarınızı görüntüleyin.

   > [!div class="mx-imgBorder"]
   > ![Özel bağlantı merkezindeki özel uç noktalar listesinin ekran görüntüsü.][15]

1. Web uygulamanıza ve alt ağıza bağlanan özel uç noktayı seçin.

   > [!div class="mx-imgBorder"]
   > ![Özel bir uç nokta için Özellikler bölmesinin ekran görüntüsü.][16]

1. Özel uç noktanızın özel IP 'sini ve Web uygulamanızın tam etki alanı adını (FQDN) kopyalayın. Önceki örnekte, özel KIMLIĞI *`webappdemope.azurewebsites.net 10.10.2.4`* .

1. **Myvm** bölmesinde, Web UYGULAMASıNA genel IP üzerinden erişildiğini doğrulayın. Bunu yapmak için bir tarayıcı açın ve Web uygulaması adını yapıştırın. Sayfada bir "hata 403-Yasak" iletisi görüntülenmelidir.

   > [!div class="mx-imgBorder"]
   > !["Hata 403-Yasak" hata sayfasının ekran görüntüsü.][17]

   > [!Important]
   > Bu özellik önizlemede olduğundan, etki alanı adı hizmeti (DNS) girişini el ile yönetmeniz gerekir.

   DNS için aşağıdakilerden birini yapın:
 
   - Azure DNS özel bölge hizmetini kullanın.  

     a. Adlı bir DNS özel bölgesi oluşturun *`privatelink.azurewebsites.net`* ve sonra sanal ağa bağlayın.  
     b. Özel uç noktanızın IP adresiyle iki bir kayıt (yani, uygulama adı ve hizmet Denetim Yöneticisi [SCM] adı) oluşturun.  
     > [!div class="mx-imgBorder"]
     > ![DNS özel bölge kayıtlarının ekran görüntüsü.][21]  

   - VM 'nin *Hosts* dosyasını kullanın.  

     a. Konaklar girdisini oluşturun, dosya Gezgini 'ni açın ve *Hosts* dosyasına bakın.  
     > [!div class="mx-imgBorder"]
     > ![Dosya Gezgini 'nde ana bilgisayar dosyasını gösteren ekran görüntüsü.][18]  
     b. *Ana bilgisayar* dosyasını bir metin düzenleyicisinde düzenleyerek Web UYGULAMANıZıN özel IP adresini ve genel adını içeren bir giriş ekleyin.  
     > [!div class="mx-imgBorder"]
     > ![Hosts dosyası metninin ekran görüntüsü.][19]  
     c. Dosyayı kaydedin.

1. Bir tarayıcıda, Web uygulamanızın URL 'sini yazın.

   > [!div class="mx-imgBorder"]
   > ![Web uygulaması görüntüleyen bir tarayıcının ekran görüntüsü.][20]

Artık özel uç nokta aracılığıyla Web uygulamanıza erişiyorsunuz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Özel uç nokta, Web uygulaması ve VM 'yi kullanarak işiniz bittiğinde, kaynak grubunu ve içerdiği tüm kaynakları silin.

1. Azure portal, **arama** kutusuna, **Ready-RG**yazın ve ardından sonuçlar listesinde **Ready-RG** ' yi seçin.

1. **Kaynak grubunu sil**'i seçin.

1. **Kaynak grubu adını yazın**altında, **Ready-RG**girin ve **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir sanal ağ, bir Web uygulaması ve özel bir uç nokta üzerinde bir VM oluşturdunuz. İnternet 'ten bir VM 'ye bağlanırsınız ve özel bağlantı kullanarak Web uygulamasına güvenli bir şekilde iletilecaksınız. 

Özel uç nokta (Önizleme) hakkında daha fazla bilgi için bkz. [Azure özel uç noktası nedir?][privateendpoint].

<!--Image references-->
[1]: ./media/create-private-endpoint-webapp-portal/createnetwork.png
[2]: ./media/create-private-endpoint-webapp-portal/ipaddresses.png
[3]: ./media/create-private-endpoint-webapp-portal/subnet.png
[4]: ./media/create-private-endpoint-webapp-portal/virtual-machine.png
[5]: ./media/create-private-endpoint-webapp-portal/vmnetwork.png
[6]: ./media/create-private-endpoint-webapp-portal/webapp.png
[7]: ./media/create-private-endpoint-webapp-portal/webappnetworking.png
[8]: ./media/create-private-endpoint-webapp-portal/webapppe.png
[9]: ./media/create-private-endpoint-webapp-portal/webapppenetwork.png
[10]: ./media/create-private-endpoint-webapp-portal/inprogress.png
[11]: ./media/create-private-endpoint-webapp-portal/webapppefinal.png
[12]: ./media/create-private-endpoint-webapp-portal/rdp.png
[13]: ./media/create-private-endpoint-webapp-portal/rdpdownload.png
[14]: ./media/create-private-endpoint-webapp-portal/pl.png
[15]: ./media/create-private-endpoint-webapp-portal/plcenter.png
[16]: ./media/create-private-endpoint-webapp-portal/privateendpointproperties.png
[17]: ./media/create-private-endpoint-webapp-portal/forbidden.png
[18]: ./media/create-private-endpoint-webapp-portal/explorer.png
[19]: ./media/create-private-endpoint-webapp-portal/hosts.png
[20]: ./media/create-private-endpoint-webapp-portal/webappwithpe.png
[21]: ./media/create-private-endpoint-webapp-portal/dns-private-zone-records.png


<!--Links-->
[privateendpointwebapp]: https://docs.microsoft.com/azure/app-service/networking/private-endpoint
[privateendpoint]: https://docs.microsoft.com/azure/private-link/private-endpoint-overview
