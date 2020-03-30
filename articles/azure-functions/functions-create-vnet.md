---
title: Azure Fonksiyonlarını Azure sanal ağıyla tümleştir
description: Bir işlevi Azure sanal ağına nasıl bağlayabileceğinizi gösteren adım adım öğretici
author: alexkarcher-msft
ms.topic: article
ms.date: 5/03/2019
ms.author: alkarche
ms.reviewer: glenga
ms.openlocfilehash: 0c70c69f547405eb8ebdcf6dcc6ae597db151e53
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75433222"
---
# <a name="tutorial-integrate-functions-with-an-azure-virtual-network"></a>Öğretici: İşlevleri bir Azure sanal ağı ile tümleştirme

Bu öğretici, bir Azure sanal ağındaki kaynaklara bağlanmak için Azure Işlevlerini nasıl kullanacağınızı gösterir. hem internete hem de sanal ağda WordPress çalıştıran bir VM'ye erişimi olan bir işlev oluşturursunuz.

> [!div class="checklist"]
> * Premium planda bir işlev uygulaması oluşturma
> * Sanal ağda VM'ye WordPress sitesi dağıtma
> * İşlev uygulamasını sanal ağa bağlayın
> * WordPress kaynaklarına erişmek için bir işlev proxy'si oluşturma
> * Sanal ağ içinden bir WordPress dosyası isteyin

## <a name="topology"></a>Topoloji

Aşağıdaki diyagram, oluşturduğunuz çözümün mimarisini gösterir:

 ![Sanal ağ tümleştirmesi için arabilgi](./media/functions-create-vnet/topology.png)

Premium planda çalışan işlevler, VNet Tümleştirme özelliğini içeren Azure Uygulama Hizmeti'ndeki web uygulamalarıyla aynı barındırma özelliklerine sahiptir. Sorun giderme ve gelişmiş yapılandırma da dahil olmak üzere VNet Tümleştirmesi hakkında daha fazla bilgi edinmek için [uygulamanızı bir Azure sanal ağıyla tümleştir'e](../app-service/web-sites-integrate-with-vnet.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici için, IP adresleme ve subnetting anlamak önemlidir. [Adresleme ve alt ağlatma temellerini kapsayan bu makaleile başlayabilirsiniz.](https://support.microsoft.com/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics) Daha birçok makale ve video çevrimiçi olarak mevcuttur.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="create-a-function-app-in-a-premium-plan"></a>Premium planda işlev uygulaması oluşturma

İlk olarak, [Premium planında]bir işlev uygulaması oluşturursunuz. Bu plan, sanal ağ tümleştirmesi desteklerken sunucusuz ölçek sağlar.

[!INCLUDE [functions-premium-create](../../includes/functions-premium-create.md)]  

Sağ üst köşedeki pin simgesini seçerek işlev uygulamasını panoya sabitleyebilirsiniz. Sabitleme, VM'nizi oluşturduktan sonra bu işlev uygulamasına geri dönmeyi kolaylaştırır.

## <a name="create-a-vm-inside-a-virtual-network"></a>Sanal ağ içinde VM oluşturma

Sonra, bir sanal ağ içinde WordPress çalışan bir önceden yapılandırılmış VM oluşturmak[(WordPress LEMP7 Jetware max Performans).](https://jetware.io/appliances/jetware/wordpress4_lemp7-170526/profile?us=azure) Bir WordPress VM düşük maliyet ve kolaylık nedeniyle kullanılır. Aynı senaryo, REST API'leri, Uygulama Hizmet Ortamları ve diğer Azure hizmetleri gibi sanal ağdaki tüm kaynaklarla çalışır. 

1. Portalda , **+** Sol daki gezinti bölmesinde, arama alanı `WordPress LEMP7 Max Performance`türünde bir kaynak oluşturun ve Enter tuşuna basın.

1. Arama sonuçlarında **Wordpress LEMP Max Performance'ı** seçin. **Yazılım Planı** olarak **CentOS için Wordpress LEMP Max Performance'ın** bir yazılım planını seçin ve **Oluştur'u**seçin.

1. Temel **Bilgiler** sekmesinde, görüntünün altındaki tabloda belirtildiği gibi VM ayarlarını kullanın:

    ![VM oluşturmak için temeller sekmesi](./media/functions-create-vnet/create-vm-1.png)

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | **Abonelik** | Aboneliğiniz | Kaynaklarınızın oluşturulduğu abonelik. | 
    | **[Kaynak grubu](../azure-resource-manager/management/overview.md)**  | myResourceGroup | İşlev uygulamanızla oluşturduğunuz kaynak grubunu veya kaynak grubunu seçin. `myResourceGroup` İşlev uygulaması, WordPress VM ve barındırma planı için aynı kaynak grubunu kullanarak bu öğretici ile bittiğinde kaynakları temizlemek için daha kolay hale getirir. |
    | **Sanal makine adı** | VNET-Wordpress | VM adının kaynak grubunda benzersiz olması gerekir |
    | **[Bölge](https://azure.microsoft.com/regions/)** | (Avrupa) Batı Avrupa | Size yakın veya VM'ye erişen işlevlerin yakınında bir bölge seçin. |
    | **Boyut** | B1'ler | **Boyutu Değiştir'i** seçin ve ardından 1 vCPU ve 1 GB belleğe sahip B1s standart görüntüsünü seçin. |
    | **Kimlik doğrulaması türü** | Parola | Parola kimlik doğrulamasını kullanmak için, bir **Kullanıcı Adı**, güvenli bir **Parola**ve ardından **parolayı onaylamanız**gerekir. Bu öğretici için, sorun gidermeniz gerekmedikçe VM'de oturum açmanız gerekmez. |

1. **Ağ** sekmesini seçin ve sanal ağları Yapılandır'ın altında **yeni oluştur'u**seçin.

1. **Sanal ağ oluştur'da,** görüntünün altındaki tablodaki ayarları kullanın:

    ![VM oluşturma ağ sekmesi](./media/functions-create-vnet/create-vm-2.png)

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | **Adı** | myResourceGroup-vnet | Sanal ağınız için oluşturulan varsayılan adı kullanabilirsiniz. |
    | **Adres aralığı** | 10.10.0.0/16 | Sanal ağ için tek bir adres aralığı kullanın. |
    | **Alt ağ adı** | Öğretici-Net | Alt ağ adı. |
    | **Adres aralığı** (subnet) | 10.10.1.0/24   | Alt ağ boyutu, alt ağa kaç arabirim eklenebilir tanımlar. Bu alt ağ WordPress sitesi tarafından kullanılır.  Bir `/24` alt ağ 254 ana bilgisayar adresi sağlar. |

1. Sanal ağı oluşturmak için **Tamam'ı** seçin.

1. **Ağ** sekmesinde, Genel IP için **Yok'u** seçin. **Public IP**

1. **Yönetim** sekmesini seçin, ardından **Tanılama depolama hesabında,** işlev uygulamanızla oluşturduğunuz Depolama hesabını seçin.

1. **İncele ve oluştur**’u seçin. Doğrulama tamamlandıktan sonra **Oluştur'u**seçin. VM oluşturma işlemi birkaç dakika sürer. Oluşturulan VM yalnızca sanal ağa erişebilir.

1. VM oluşturulduktan sonra, yeni VM'nizin sayfasını görüntülemek için **kaynağa git'i** seçin ve ardından **Ayarlar**altında **Ağ Oluşturma'yı** seçin.

1. **Genel IP**olmadığını doğrulayın. İşlev uygulamanızdan VM'ye bağlanmak için kullandığınız **Özel IP'ye**not edin.

    ![VM'deki ağ ayarları](./media/functions-create-vnet/vm-networking.png)

Şimdi tamamen sanal ağınızda dağıtılan bir WordPress siteniz var. Bu siteye genel internetten erişilemez.

## <a name="connect-your-function-app-to-the-virtual-network"></a>İşlev uygulamanızı sanal ağa bağlayın

Sanal ağda VM'de çalışan bir WordPress sitesiyle, işlev uygulamanızı artık bu sanal ağa bağlayabilirsiniz.

1. Yeni işlev uygulamanızda, **Platform özellikleri** > **Ağ'ı**seçin.

    ![İşlev uygulamasında ağ kullanmayı seçin](./media/functions-create-vnet/networking-0.png)

1. **VNet Tümleştirmesi**altında, **yapılandırmak için buraya tıklayın'ı**seçin.

    ![Ağ özelliğini yapılandırma durumu](./media/functions-create-vnet/Networking-1.png)

1. Sanal ağ tümleştirme sayfasında **VNet Ekle (önizleme)** seçeneğini belirleyin.

    ![VNet Tümleştirme önizlemesini ekleme](./media/functions-create-vnet/networking-2.png)

1. **Ağ Özelliği Durumu'nda,** görüntünün altındaki tablodaki ayarları kullanın:

    ![İşlev uygulaması sanal ağ ını tanımlayın](./media/functions-create-vnet/networking-3.png)

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | **Sanal Ağ** | MyResourceGroup-vnet | Bu sanal ağ daha önce oluşturduğunuz ağdır. |
    | **Alt ağ** | Yeni Alt Ağ Oluştur | İşlev uygulamanızın kullanması için sanal ağda bir alt ağ oluşturun. VNet Tümleştirmeboş bir alt ağ kullanacak şekilde yapılandırılmalıdır. Fonksiyonlarınızın VM'nizden farklı bir alt ağ kullanması önemli değildir. Sanal ağ, iki alt ağ arasındaki trafiği otomatik olarak yönlendirir. |
    | **Alt ağ adı** | Fonksiyon-Net | Yeni alt ağın adı. |
    | **Sanal ağ adresi bloğu** | 10.10.0.0/16 | WordPress sitesi tarafından kullanılan adres bloğunu seçin. Yalnızca bir adres bloğu tanımlanmış olmalıdır. |
    | **Adres aralığı** | 10.10.2.0/24   | Alt ağ boyutu, Premium plan işlev uygulamanızın ölçeklendirebileceği toplam örnek sayısını kısıtlar. Bu örnekte, kullanılabilir 254 ana bilgisayar adresine sahip bir `/24` alt ağ kullanır. Bu alt ağ aşırı sağlanmış, ancak hesaplaması kolaydır. |

1. Alt ağı eklemek için **Tamam'ı** seçin. İşlev uygulaması sayfanıza dönmek için VNet Tümleştirme ve Ağ Özelliği Durumu sayfalarını kapatın.

İşlev uygulaması artık WordPress sitesinin çalıştığı sanal ağa erişebilir. Ardından, WordPress sitesinden bir dosyayı döndürmek için [Azure İşlemesi'ni](functions-proxies.md) kullanırsınız.

## <a name="create-a-proxy-to-access-vm-resources"></a>VM kaynaklarına erişmek için proxy oluşturma

VNet Tümleştirme etkinken, istekleri sanal ağda çalışan VM'ye iletmek için işlev uygulamanızda bir proxy oluşturabilirsiniz.

1. İşlev uygulamanızda, **Proxy'leri** > **+** seçin ve ardından resmin altındaki tablodaki proxy ayarlarını kullanın:

    ![Proxy ayarlarını tanımlama](./media/functions-create-vnet/create-proxy.png)

    | Ayar  | Önerilen değer  | Açıklama      |
    | -------- | ---------------- | ---------------- |
    | **Adı** | Tesis | Adı herhangi bir değer olabilir. Proxy'yi tanımlamak için kullanılır. |
    | **Rota Şablonu** | /bitki | Bu haritaları bir VM kaynağına yönlendirin. |
    | **Arka uç URL'si** | http://<YOUR_VM_IP>/wp-content/themes/twentyseventeen/assets/images/header.jpg | Daha `<YOUR_VM_IP>` önce oluşturduğunuz WordPress VM'nizin IP adresiyle değiştirin. Bu eşleme, siteden tek bir dosya döndürür. |

1. Proxy'yi işlev uygulamanıza eklemek için **Oluştur'u** seçin.

## <a name="try-it-out"></a>Deneyin

1. Tarayıcınızda, Arka Uç URL'si **Backend URL**olarak kullandığınız URL'ye erişmeye çalışın. Beklendiği gibi, istek zaman ları dışarı. WordPress siteniz internete değil, yalnızca sanal ağınıza bağlı olduğundan bir zaman dilimi oluşur.

1. Proxy **URL** değerini yeni proxy'nizden kopyalayın ve tarayıcınızın adres çubuğuna yapıştırın. Döndürülen görüntü, sanal ağınızın içinde çalışan WordPress sitesinden dir.

    ![Bitki resim dosyası WordPress sitesinden döndürülür](./media/functions-create-vnet/plant.png)

İşlev uygulamanız hem internete hem de sanal ağınıza bağlıdır. Proxy, genel internet üzerinden bir istek alıyor ve daha sonra bağlı sanal ağa bu isteği iletmek için basit bir HTTP proxy olarak hareket ediyor. Proxy daha sonra yanıtı internet üzerinden herkese açık olarak size geri iletir.

[!INCLUDE [clean-up-section-portal](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, WordPress sitesi işlev uygulamasında bir proxy kullanarak çağrılan bir API olarak hizmet vermektedir. Kurulumu ve görselleştirmesi kolay olduğundan, bu senaryo iyi bir öğretici yapar. Sanal ağ içinde dağıtılan diğer API'leri kullanabilirsiniz. Sanal ağ içinde dağıtılan API'leri çağıran kodlu bir işlev de oluşturmuş olabilirsiniz. Daha gerçekçi bir senaryo, sanal ağda dağıtılan bir SQL Server örneğini çağırmak için veri istemcisi API'lerini kullanan bir işlevdir.

Premium planda çalışan işlevler, PremiumV2 planlarında web uygulamalarıyla aynı temel Uygulama Hizmeti altyapısını paylaşır. Azure Uygulama [Hizmeti'ndeki web uygulamalarıiçin](../app-service/overview.md) tüm belgeler Premium plan işlevleriniz için geçerlidir.

> [!div class="nextstepaction"]
> [İşlevler'deki ağ seçenekleri hakkında daha fazla bilgi edinin](./functions-networking-options.md)

[Premium planı]: functions-scale.md#premium-plan
