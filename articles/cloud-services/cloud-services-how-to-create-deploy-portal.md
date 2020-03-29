---
title: Bulut hizmeti oluşturma ve dağıtma | Microsoft Dokümanlar
description: Azure portalını kullanarak bir bulut hizmeti oluşturmayı ve dağıtmayı öğrenin.
services: cloud-services
documentationcenter: ''
author: tgore03
ms.service: cloud-services
ms.topic: article
ms.date: 05/18/2017
ms.author: tagore
ms.openlocfilehash: 53f53976b20359afc45abe1b25ca60325b5d6a2b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75386179"
---
# <a name="how-to-create-and-deploy-a-cloud-service"></a>Bulut hizmeti oluşturma ve dağıtma
Azure portalı, bir bulut hizmeti oluşturmanız ve dağıtmanız için iki yol sağlar: *Hızlı Oluşturma* ve *Özel Oluşturma.*

Bu makalede, yeni bir bulut hizmeti oluşturmak için Hızlı Oluştur yönteminin nasıl kullanılacağı ve ardından Azure'da bir bulut hizmeti paketi yüklemek ve dağıtmak için **Yükle'yi** kullanma nın nasıl kullanılacağı açıklanmaktadır. Bu yöntemi kullandığınızda, Azure portalı tüm gereksinimleri tamamlamak için kullanışlı bağlantılar sunar. Bulut hizmetinizi oluştururken dağıtmaya hazırsanız, Özel Oluşturma'yı kullanarak her ikisini de aynı anda yapabilirsiniz.

> [!NOTE]
> Bulut hizmetinizi Azure DevOps'ten yayımlamayı planlıyorsanız, Hızlı Oluştur'u kullanın ve ardından Azure Quickstart veya panodan Azure DevOps yayımlamasını ayarlayın. Daha fazla bilgi için Azure [DevOps'leri kullanarak Azure'a Sürekli Teslim'e][TFSTutorialForCloudService]bakın veya **Hızlı Başlangıç** sayfası için yardıma bakın.
>
>

## <a name="concepts"></a>Kavramlar
Azure'da bir uygulamayı bulut hizmeti olarak dağıtmak için üç bileşen gerekir:

* **Hizmet Tanımı**  
  Bulut hizmeti tanımı dosyası (.csdef), rol sayısını da içeren hizmet modelini tanımlar.
* **Hizmet Yapılandırması**  
  Bulut hizmeti yapılandırma dosyası (.cscfg), rol örneklerinin sayısı da dahil olmak üzere bulut hizmeti ve tek tek roller için yapılandırma ayarları sağlar.
* **Servis Paketi**  
  Hizmet paketi (.cspkg) uygulama kodu ve yapılandırmaları ve hizmet tanımı dosyasını içerir.

Bu ve nasıl [burada](cloud-services-model-and-package.md)bir paket oluşturmak için hakkında daha fazla bilgi edinebilirsiniz.

## <a name="prepare-your-app"></a>Uygulamanızı hazırlayın
Bir bulut hizmeti dağıtabilesiniz, uygulama kodunuzdan bulut hizmeti paketini (.cspkg) ve bir bulut hizmeti yapılandırma dosyasından (.cscfg) oluşturmanız gerekir. Azure SDK, bu gerekli dağıtım dosyalarını hazırlamak için araçlar sağlar. SDK'yı [Azure İndirmeler](https://azure.microsoft.com/downloads/) sayfasından uygulama kodunuzu geliştirmeyi tercih ettiğiniz dilde yükleyebilirsiniz.

Üç bulut hizmeti özelliği, bir hizmet paketini dışa aktarmadan önce özel yapılandırmalar gerektirir:

* Veri şifrelemesi için Güvenli Soket katmanı (SSL) kullanan bir bulut hizmeti dağıtmak istiyorsanız, uygulamanızı SSL için [yapılandırın.](cloud-services-configure-ssl-certificate-portal.md#modify)
* Uzak Masaüstü bağlantılarını rol örneklerine yapılandırmak istiyorsanız, Uzak Masaüstü için [rolleri yapılandırın.](cloud-services-role-enable-remote-desktop-new-portal.md)
* Bulut hizmetiniz için ayrıntılı izlemeyi yapılandırmak istiyorsanız, bulut hizmeti için Azure Tanılama'yı etkinleştirin. *Minimum izleme* (varsayılan izleme düzeyi), rol örnekleri (sanal makineler) için ana bilgisayar işletim sistemlerinden toplanan performans sayaçlarını kullanır. *Verbose izleme,* uygulama işleme sırasında ortaya çıkan sorunların daha yakın analizini sağlamak için rol örnekleri içindeki performans verilerine dayalı ek ölçümler toplar. Azure Tanılama'yı nasıl etkinleştireceklerini öğrenmek için [bkz.](cloud-services-dotnet-diagnostics.md)

Web rolleri veya çalışan rollerinin dağıtımıyla bir bulut hizmeti oluşturmak için [hizmet paketini oluşturmanız](cloud-services-model-and-package.md#servicepackagecspkg)gerekir.

## <a name="before-you-begin"></a>Başlamadan önce
* Azure SDK'yı yüklemediyseniz, [Azure İndirmeler sayfasını](https://azure.microsoft.com/downloads/)açmak için **Azure SDK'yı yükle'yi** tıklatın ve ardından kodunuzu geliştirmeyi tercih ettiğiniz dil için SDK'yı indirin. (Bunu daha sonra yapma fırsatınız olacak.)
* Herhangi bir rol örneği sertifika gerektiriyorsa, sertifikaları oluşturun. Bulut hizmetleri özel anahtarlı bir .pfx dosyası gerektirir. Bulut hizmetini oluştururken ve dağıtırken sertifikaları Azure'a yükleyebilirsiniz.

## <a name="create-and-deploy"></a>Oluşturma ve dağıtma
1. [Azure portalına](https://portal.azure.com/)giriş yapın.
2. **Kaynak Oluştur > Bilgi İşlem'i**tıklatın ve ardından bulut **hizmetine**gidin ve tıklatın.

    ![Bulut hizmetinizi yayımlayın](media/cloud-services-how-to-create-deploy-portal/create-cloud-service.png)
3. Yeni **Bulut Hizmeti** bölmesine, **DNS adı**için bir değer girin.
4. Yeni bir **Kaynak Grubu** oluşturun veya varolan bir tane seçin.
5. Bir **Konum** seçin.
6. **Paket'e**tıklayın. Bu, **paket** yükle bölmesini açar. Gerekli alanları doldurun. Rollerinizden herhangi biri tek bir örnek içeriyorsa, **bir veya daha fazla rol tek bir örnek seçilse bile Dağıt'tan** emin olun.
7. **Dağıtımı başlat'ın** seçildiğinden emin olun.
8. **Paket** yükle bölmesini kapatacak **tamam'ı** tıklatın.
9. Ekleyecek sertifikanız yoksa **Oluştur'u**tıklatın.

    ![Bulut hizmetinizi yayımlayın](media/cloud-services-how-to-create-deploy-portal/select-package.png)

## <a name="upload-a-certificate"></a>Sertifika yükleme
Dağıtım paketiniz [sertifikaları kullanacak şekilde yapılandırıldıysa,](cloud-services-configure-ssl-certificate-portal.md#modify)sertifikayı şimdi yükleyebilirsiniz.

1. **Sertifikalar'ı**seçin ve **Sertifika Ekle** bölmesine, SSL sertifikası .pfx dosyasını seçin ve ardından sertifika için **Parola'yı** girin,
2. **Sertifika Ekle'yi**tıklatın ve ardından **Sertifika Ekle** bölmesine **Tamam'ı** tıklatın.
3. **Bulut Hizmeti** bölmesine **Oluştur'u** tıklatın. Dağıtım **Hazır** durumuna ulaştığında, sonraki adımlara geçebilirsiniz.

    ![Bulut hizmetinizi yayımlayın](media/cloud-services-how-to-create-deploy-portal/attach-cert.png)

## <a name="verify-your-deployment-completed-successfully"></a>Dağıtımınızın başarıyla tamamlandığını doğrulayın
1. Bulut hizmeti örneğini tıklatın.

    Durum, hizmetin **çalıştığını**göstermelidir.
2. **Essentials**altında, bulut hizmetinizi bir web tarayıcısında açmak için **Site URL'sini** tıklatın.

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy-portal/running.png)

[TFSTutorialForCloudService]: https://go.microsoft.com/fwlink/?LinkID=251796

## <a name="next-steps"></a>Sonraki adımlar
* [Bulut hizmetinizin genel yapılandırması.](cloud-services-how-to-configure-portal.md)
* Özel bir [etki alanı adı](cloud-services-custom-domain-name-portal.md)yapılandırın.
* [Bulut hizmetinizi yönetin.](cloud-services-how-to-manage-portal.md)
* [SSL sertifikalarını](cloud-services-configure-ssl-certificate-portal.md)yapılandırma .



