---
title: Azure özel uç noktasını kullanarak bir Web uygulamasına özel olarak bağlanma
description: Azure özel uç noktasını kullanarak bir Web uygulamasına özel olarak bağlanma
author: ericgre
ms.assetid: b8c5c7f8-5e90-440e-bc50-38c990ca9f14
ms.topic: how-to
ms.date: 06/02/2020
ms.author: ericg
ms.service: app-service
ms.workload: web
ms.openlocfilehash: 1b3ac4c79ce92f591e74821a9f355717e4b22ea4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84737402"
---
# <a name="connect-privately-to-a-web-app-using-azure-private-endpoint-preview"></a>Azure özel uç noktası (Önizleme) kullanarak bir Web uygulamasına özel olarak bağlanma

Azure özel uç noktası, Azure 'da özel bağlantı için temel yapı taşdır. Web uygulamanıza özel olarak bağlanmanızı sağlar.
Bu hızlı başlangıçta, bir Web uygulamasını özel uç nokta ile dağıtmayı ve bir sanal makineden bu Web uygulamasına bağlanmayı öğreneceksiniz.

Daha fazla bilgi için bkz. [Azure Web uygulaması Için özel uç noktaları kullanma][privatenedpointwebapp].

> [!Note]
>Önizleme, PremiumV2 Windows ve Linux Web Apps ve elastik Premium işlevleri için genel bölgelerde kullanılabilir. 

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="virtual-network-and-virtual-machine"></a>Sanal ağ ve sanal makine

Bu bölümde, Özel uç nokta aracılığıyla Web uygulamanıza erişmek için kullanılan VM 'yi barındırmak için sanal ağ ve alt ağ oluşturacaksınız.

### <a name="create-the-virtual-network"></a>Sanal ağı oluşturma

Bu bölümde, bir sanal ağ ve alt ağ oluşturacaksınız.

1. Ekranın sol üst tarafında, **kaynak oluştur**  >  **ağ**  >  **sanal ağı** ' nı veya arama kutusunda **sanal ağ** ara ' yı seçin.

1. **Sanal ağ oluştur**' da, temel bilgiler sekmesinde bu bilgileri girin veya seçin:

   > [!div class="mx-imgBorder"]
   > ![Sanal ağ oluştur][1]

1. **"İleri: IP adresleri >"** düğmesine tıklayın ve bu bilgileri girin veya seçin:

   > [!div class="mx-imgBorder"]
   >![IP adreslerini yapılandırma][2]

1. Alt ağ bölümünde **"+ alt ağ ekle"** ye tıklayın ve aşağıdaki bilgileri girip **"Ekle"** ye tıklayın

   > [!div class="mx-imgBorder"]
   >![Alt ağ ekle][3]

1. **"Gözden geçir + oluştur"** seçeneğine tıklayın

1. Doğrulama başarılı olduktan sonra **"Oluştur"** düğmesine tıklayın.

### <a name="create-virtual-machine"></a>Sanal makine oluşturma

1. Azure Portal ekranın sol üst kısmında **kaynak oluştur**  >  **işlem**  >  **sanal makinesi** ' ni seçin.

1. Sanal makine oluşturma-temel bilgiler bölümünde, bu bilgileri girin veya seçin:

   > [!div class="mx-imgBorder"]
   >![Sanal makine temel][4]

1. **"İleri: diskler"** i seçin

   Varsayılan ayarları koruyun.

1. **"İleri: ağ"** seçeneğini belirleyin, bu bilgileri seçin:

   > [!div class="mx-imgBorder"]
   >![Ağ ][5]

1. **"Gözden geçir + oluştur"** seçeneğine tıklayın

1. Doğrulama geçtiğinde ileti **"Oluştur"** düğmesine tıklayın.

## <a name="create-your-web-app-and-private-endpoint"></a>Web uygulamanızı ve özel uç noktayı oluşturma

Bu bölümde özel bir uç nokta kullanarak özel bir Web uygulaması oluşturacaksınız.

> [!Note]
>Özel uç nokta özelliği yalnızca Premium v2 SKU 'SU için kullanılabilir.

### <a name="web-app"></a>Web App

1. Azure Portal ekranın sol üst tarafında, **kaynak oluştur**  >  **Web**  >  **Web uygulaması** ' nı seçin.

1. Web uygulaması oluşturma-temel bilgiler bölümünde, bu bilgileri girin veya seçin:

   > [!div class="mx-imgBorder"]
   >![Web uygulaması temel][6]

1. **"Gözden geçir + oluştur"** u seçin

1. Doğrulama geçtiğinde ileti **"Oluştur"** düğmesine tıklayın.

### <a name="create-the-private-endpoint"></a>Özel uç nokta oluşturma

1. Web uygulaması özelliklerinde **Ayarlar**  >  **ağ** ' ı seçin ve **"özel uç nokta bağlantılarınızı yapılandırın"** seçeneğine tıklayın.

   > [!div class="mx-imgBorder"]
   >![Web uygulaması ağı][7]

1. Sihirbazda, **"+ Ekle"** ye tıklayın

   > [!div class="mx-imgBorder"]
   >![Web uygulaması özel uç noktası][8]

1. Abonelik, VNet ve alt ağ bilgilerini doldurup **"Tamam"** a tıklayın

   > [!div class="mx-imgBorder"]
   >![Web uygulaması ağı][9]

1. Özel uç noktanın oluşturulmasını gözden geçirin

   > [!div class="mx-imgBorder"]
   >![][10]
   > ![ Özel uç noktanın son görünümünü gözden geçirin][11]

## <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

1. Portalın arama çubuğunda **Myvm** yazın
1. **Bağlan düğmesini**seçin. Bağlan düğmesini seçtikten sonra sanal makineye bağlan açılır, **RDP** 'yi seçin

   > [!div class="mx-imgBorder"]
   >![RDP düğmesi][12]

1. Azure, Uzak Masaüstü Protokolü (. rdp) dosyası oluşturur ve **RDP dosyasını indir** ' e tıkladıktan sonra bilgisayarınıza indirir

   > [!div class="mx-imgBorder"]
   >![RDP dosyasını indir][13]

1. İndirilen. rdp dosyasını açın.

- İstendiğinde Bağlan’ı seçin.
- VM oluştururken belirttiğiniz kullanıcı adını ve parolayı girin.

> [!Note]
> VM oluştururken girdiğiniz kimlik bilgilerini belirtmek için farklı bir hesap kullanmak > daha fazla seçenek belirlemeniz gerekebilir.

- Tamam'ı seçin.

1. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bir sertifika uyarısı alırsanız Evet ' i veya devam et ' i seçin.

1. VM masaüstü seçildikten sonra, bunu yerel masaüstünüze geri dönmek için simge durumuna küçültün.

## <a name="access-web-app-privately-from-the-vm"></a>Web uygulamasına özel olarak VM 'den erişin

Bu bölümde özel uç nokta kullanarak Web uygulamasına özel olarak bağlanırsınız.

1. Özel uç noktanızın özel IP 'sini, arama çubuğu türü **özel bağlantısı**' na ve özel bağlantı ' yı seçin.

   > [!div class="mx-imgBorder"]
   >![Özel Bağlantı][14]

1. Özel bağlantı merkezinde özel **uç noktalar** ' ı seçerek tüm özel uç noktalarınızı listeleyin

   > [!div class="mx-imgBorder"]
   >![Özel bağlantı merkezi][15]

1. Web uygulamanız ve alt ağınız için özel uç nokta bağlantısını seçin

   > [!div class="mx-imgBorder"]
   >![Özel uç nokta özellikleri][16]

1. Özel uç noktanızın özel IP 'sini ve Web uygulamanızın FQDN 'sini webappdemope.azurewebsites.net 10.10.2.4 ' de kopyalayın

1. MyVM 'de, Web uygulamasına genel IP aracılığıyla erişilebildiğini doğrulayın. Bir tarayıcı açın ve Web uygulaması adını yapıştırın, 403 yasaklanmış bir hata sayfasına sahip olmanız gerekir

   > [!div class="mx-imgBorder"]
   >![Yasak][17]

> [!Important]
> Bu özellik önizlemede olduğundan, DNS girişini el ile yönetmeniz gerekir.

1. Konak girişini oluşturun, dosya Gezgini 'ni açın ve Hosts dosyasını bulun

   > [!div class="mx-imgBorder"]
   >![Hosts dosyası][18]

1. Hosts dosyasını Notepad ile düzenleyerek Web uygulamanızın özel IP adresine ve genel adına sahip bir giriş ekleyin

   > [!div class="mx-imgBorder"]
   >![İçerik barındırır][19]

1. Dosyayı kaydetme

1. Bir tarayıcı açın ve Web uygulamanızın URL 'sini yazın

   > [!div class="mx-imgBorder"]
   >![PE ile Web sitesi][20]

1. Web uygulamanıza özel uç nokta aracılığıyla erişiyorsunuz

## <a name="clean-up-resources"></a>Kaynakları temizleme

Özel uç nokta, Web uygulaması ve VM 'yi kullanarak işiniz bittiğinde, kaynak grubunu ve içerdiği tüm kaynakları silin:

1. Portalın üst kısmındaki arama kutusuna Ready-RG yazın ve arama sonuçlarından Ready-RG seçeneğini belirleyin.
1. Kaynak grubunu sil'i seçin.
1. KAYNAK grubu adını yazmak için Ready-RG girin ve Sil ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir sanal ağ, bir Web uygulaması ve özel bir uç nokta üzerinde bir VM oluşturdunuz. Internet 'ten bir VM 'ye bağlanırsınız ve özel bağlantı kullanarak Web uygulamasına güvenli bir şekilde iletilecaksınız. Özel uç nokta hakkında daha fazla bilgi için bkz. [Azure özel uç noktası nedir?][privateendpoint].

<!--Image references-->
[1]: ./media/create-private-endpoint-webapp-portal/createnetwork.png
[2]: ./media/create-private-endpoint-webapp-portal/ipaddresses.png
[3]: ./media/create-private-endpoint-webapp-portal/subnet.png
[4]: ./media/create-private-endpoint-webapp-portal/virtualmachine.png
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

<!--Links-->
[privatenedpointwebapp]: https://docs.microsoft.com/azure/app-service/networking/private-endpoint
[privateendpoint]: https://docs.microsoft.com/azure/private-link/private-endpoint-overview
