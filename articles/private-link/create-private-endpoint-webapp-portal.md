---
title: Azure Özel Bitiş Noktası'nı kullanarak bir Web Uygulamasına özel bağlanma
description: Azure Özel Bitiş Noktası'nı kullanarak bir Web Uygulamasına özel bağlanma
author: ericgre
ms.assetid: b8c5c7f8-5e90-440e-bc50-38c990ca9f14
ms.topic: article
ms.date: 03/12/2020
ms.author: ericg
ms.service: app-service
ms.workload: web
ms.openlocfilehash: 2f10c7378ae7681b14df6e96b6a6f1adac832d1b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80287824"
---
# <a name="connect-privately-to-a-web-app-using-azure-private-endpoint-preview"></a>Azure Özel Bitiş Noktası'nı kullanarak bir Web Uygulamasına özel bağlanma (Önizleme)

Azure Private Endpoint, Azure'daki Özel Bağlantı'nın temel yapı taşıdır. Web Uygulamanıza özel olarak bağlanmanızı sağlar.
Bu Quickstart'ta, Özel Bitiş Noktası olan bir Web Uygulamasını nasıl dağıtacağınızı ve Sanal Makine'den bu Web Uygulamasına nasıl bağlanacağınızı öğreneceksiniz.

Daha fazla bilgi için Azure [Web Uygulaması için Özel Bitiş Noktalarını Kullanma'ya][privatenedpointwebapp]bakın.

> [!Note]
>Önizleme, tüm PremiumV2 Windows ve Linux Web Uygulamaları için Doğu ABD ve Batı ABD 2 bölgelerinde mevcuttur. 

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="virtual-network-and-virtual-machine"></a>Sanal ağ ve Sanal Makine

Bu bölümde, Web Uygulamanıza Özel Bitiş Noktası üzerinden erişmek için kullanılan VM'yi barındırmak için sanal ağ ve alt ağ oluşturursunuz.

### <a name="create-the-virtual-network"></a>Sanal ağ oluşturma

Bu bölümde, sanal bir ağ ve alt ağ oluşturursunuz.

1. Ekranın sol üst tarafında, > sanal**ağ** **ağı** >  **oluştur'u**veya arama kutusunda **Sanal ağ** aramasını seçin.

1. **Sanal ağ oluştur'da**TemelLer sekmesine bu bilgileri girin veya seçin:

   > [!div class="mx-imgBorder"]
   > ![Sanal Ağ Oluşturma][1]

1. **"Sonraki: IP Adresleri >"** seçeneğini tıklayın ve bu bilgileri girin veya seçin:

   > [!div class="mx-imgBorder"]
   >![IP Adreslerini Yapılandırma][2]

1. Alt ağ bölümünde **"+ Alt Ağ Ekle"ye** tıklayın ve aşağıdaki bilgileri girin ve **"Ekle"ye** tıklayın

   > [!div class="mx-imgBorder"]
   >![Alt Ağ Ekle][3]

1. **"Gözden Geçir + oluştur" seçeneğini** tıklayın

1. Doğrulama geçtikten sonra **"Oluştur"** seçeneğini tıklayın

### <a name="create-virtual-machine"></a>Sanal makine oluşturma

1. Azure portalında ekranın sol üst tarafında, > kaynak**Oluştur** > **Sanal makine** **oluştur'u**seçin

1. Sanal makine Oluştur'da - Temel bilgiler, bu bilgileri girin veya seçin:

   > [!div class="mx-imgBorder"]
   >![Sanal Makine temel][4]

1. **"Sonraki: Diskler"i** seçin

   Varsayılan ayarları koruyun.

1. **"Next: Networking" seçeneğini belirleyin,** şu bilgileri seçin:

   > [!div class="mx-imgBorder"]
   >![Ağ ][5]

1. **"Gözden Geçir + Oluştur" seçeneğini** tıklayın

1. Doğrulama iletiyi geçtiğinde **"Oluştur" seçeneğini** tıklayın

## <a name="create-your-web-app-and-private-endpoint"></a>Web Uygulamanızı ve Özel Bitiş Noktasını Oluşturun

Bu bölümde, özel bir Bitiş Noktası kullanarak özel bir Web Uygulaması oluşturursunuz.

> [!Note]
>Özel Bitiş Noktası özelliği yalnızca Premium V2 SKU için kullanılabilir.

### <a name="web-app"></a>Web App

1. Azure portalında ekranın sol üst tarafında > kaynak**Web** > **Uygulaması** **Oluştur'u**seçin

1. Web App Oluştur - Temel Bilgiler'de bu bilgileri girin veya seçin:

   > [!div class="mx-imgBorder"]
   >![Web App temel][6]

1. **"Gözden Geçir + oluştur" seçeneğini belirleyin**

1. Doğrulama iletiyi geçtiğinde **"Oluştur" seçeneğini** tıklayın

### <a name="create-the-private-endpoint"></a>Özel bitiş noktasını oluşturma

1. Web App özelliklerinde **Ayarlar** > **Ağı'nı** seçin ve **"Özel uç nokta bağlantılarınızı yapılandırın"a** tıklayın

   > [!div class="mx-imgBorder"]
   >![Web Uygulaması ağ][7]

1. Sihirbazda **"+ ekle"ye** tıklayın

   > [!div class="mx-imgBorder"]
   >![Web App Özel Bitiş Noktası][8]

1. Abonelik, VNet ve Subnet bilgilerini doldurun ve **"Tamam"ı** tıklayın

   > [!div class="mx-imgBorder"]
   >![Web App Ağ][9]

1. Özel bitiş noktasının oluşturulmasını gözden geçirme

   > [!div class="mx-imgBorder"]
   >![Özel][10]
   >![bitiş noktasının son görünümünü gözden geçirme][11]

## <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

1. Portalın arama çubuğuna **myVm** girin
1. Bağlan **düğmesini**seçin. Bağlan düğmesini seçtikten sonra, sanal makineye bağlan açılır, **RDP'yi** seçin

   > [!div class="mx-imgBorder"]
   >![RDP düğmesi][12]

1. Azure bir Uzak Masaüstü Protokolü (.rdp) dosyası oluşturur ve **RDP dosyasını İndir'i** tıklattıktan sonra bilgisayarınıza indirir

   > [!div class="mx-imgBorder"]
   >![RDP dosyasını indirin][13]

1. İndirilen.rdp dosyasını açın.

- İstendiğinde Bağlan’ı seçin.
- VM oluştururken belirttiğiniz kullanıcı adı ve parolayı girin.

> [!Note]
> VM'yi oluşturduğunuzda girdiğiniz kimlik bilgilerini belirtmek için farklı bir hesap > daha fazla seçenek seçmeniz gerekebilir.

- Tamam'ı seçin.

1. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Sertifika uyarısı alırsanız Evet veya Devam et'i seçin.

1. VM masaüstü göründükten sonra, yerel masaüstünüze geri dönmek için en aza indirin.

## <a name="access-web-app-privately-from-the-vm"></a>Web App'e VM'den özel olarak erişin

Bu bölümde, Özel Bitiş Noktası'nı kullanarak Web Uygulamasına özel olarak bağlanabilirsiniz.

1. Arama çubuğundaki **Özel Bağlantı**türünde Private Endpoint'inizin özel IP'sini alın ve Özel Bağlantı'yı seçin

   > [!div class="mx-imgBorder"]
   >![Özel Bağlantı][14]

1. Özel Bağlantı Merkezi'nde, tüm Özel Uç Noktalarınızı listelemek için **Özel Uç Noktaları'nı** seçin

   > [!div class="mx-imgBorder"]
   >![Özel Bağlantı merkezi][15]

1. Web Uygulamanızın ve alt ağınızın Özel Bitiş Noktası bağlantısını seçin

   > [!div class="mx-imgBorder"]
   >![Özel bitiş noktası özellikleri][16]

1. Bizim durumumuzda 10.10.2.4 webappdemope.azurewebsites.net Özel Bitiş Noktanızın Özel IP'sini ve Web Uygulamanızın FQDN'sini kopyalayın

1. myVM'de, Web Uygulamasına genel IP üzerinden erişilmediğini doğrulayın. Bir tarayıcı açın ve Web App adını yapıştırın, 403 yasak hata sayfası olmalıdır

   > [!div class="mx-imgBorder"]
   >![Yasak][17]

> [!Important]
> Bu özellik önizlemede olduğundan, DNS girişini el ile yönetmeniz gerekir.

1. Ana bilgisayar girişini oluşturun, dosya gezginini açın ve ana bilgisayar dosyasını bulun

   > [!div class="mx-imgBorder"]
   >![Ana bilgisayarları dosyası][18]

1. Ev sahipleri dosyasını not defteriyle düzenleyerek özel IP adresi ve Web Uygulamanızın genel adını içeren bir giriş ekleme

   > [!div class="mx-imgBorder"]
   >![İçerik barındıran][19]

1. Dosyayı kaydetme

1. Bir tarayıcı açın ve web uygulamanızın url'sini yazın

   > [!div class="mx-imgBorder"]
   >![PE ile Web sitesi][20]

1. Web Uygulamanıza Özel Bitiş Noktası üzerinden erişiyorsunuz

## <a name="clean-up-resources"></a>Kaynakları temizleme

Özel Bitiş Noktası, Web Uygulaması ve VM'yi kullanmayı bitirdiğinizde, kaynak grubunu ve içerdiği tüm kaynakları silin:

1. Portalın üst kısmındaki Arama kutusuna ready-rg girin ve arama sonuçlarından ready-rg'yi seçin.
1. Kaynak grubunu sil'i seçin.
1. KAYNAK GRUBU ADINI Tİp İBİn'e hazır rg girin ve Sil'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu Quickstart'ta, sanal ağda bir VM, bir Web Uygulaması ve Özel Bitiş Noktası oluşturdunuz. Internet'ten bir VM'ye bağlandınız ve Özel Bağlantı'yı kullanarak Web Uygulamasına güvenli bir şekilde iletişim kurdunuz. Özel Bitiş Noktası hakkında daha fazla bilgi edinmek için [Azure Özel Bitiş Noktası nedir'e][privateendpoint]bakın.

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
