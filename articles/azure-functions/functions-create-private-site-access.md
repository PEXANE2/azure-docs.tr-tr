---
title: Azure İşlevlerine özel site erişimini etkinleştirme
description: Azure İşlevler için Azure sanal ağ özel site erişimi ayarlamayı öğrenin.
author: mcollier
ms.author: mcollier
ms.service: azure-functions
ms.topic: tutorial
ms.date: 02/15/2020
ms.openlocfilehash: ada08de182791c6ecb2b83ef3b924bf40975e1ee
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78851329"
---
# <a name="tutorial-establish-azure-functions-private-site-access"></a>Öğretici: Azure İşlevleri özel site erişimi oluşturma

Bu öğretici, Azure İşlevleri ile [özel site erişimini](./functions-networking-options.md#private-site-access) nasıl etkinleştirdiğinizi gösterir. Özel site erişimini kullanarak, işlev kodunuzu yalnızca belirli bir sanal ağdan tetiklenebilmiş olması gerekir.

Özel site erişimi, işlev uygulamasına erişimin belirli bir sanal ağla sınırlanması gereken senaryolarda yararlıdır. Örneğin, işlev uygulaması yalnızca belirli bir kuruluşun çalışanları veya belirtilen sanal ağ içinde bulunan hizmetler (başka bir Azure İşlevi, Azure Sanal Makine veya AKS kümesi gibi) için geçerli olabilir.

Bir İşlev uygulamasının sanal ağ daki Azure kaynaklarına erişmesi veya [hizmet bitiş noktaları](../virtual-network/virtual-network-service-endpoints-overview.md)üzerinden bağlanması gerekiyorsa, sanal ağ [tümleştirmesi](./functions-create-vnet.md) gerekir.

Bu öğreticide, işlev uygulamanız için özel site erişimini nasıl yapılandırabileceğinizi öğrenirsiniz:

> [!div class="checklist"]
> * Sanal makine oluşturma
> * Azure Bastion hizmeti oluşturma
> * Azure İşlevleri uygulaması oluşturma
> * Sanal ağ hizmeti bitiş noktasını yapılandırma
> * Azure İşi oluşturma ve dağıtma
> * İşlevi dışarıdan ve sanal ağ içinde çağırma

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="topology"></a>Topoloji

Aşağıdaki diyagram, oluşturulacak çözümün mimarisini gösterir:

![Özel site erişim çözümü için üst düzey mimari diyagramı](./media/functions-create-private-site-access/topology.png)

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici için, IP adresleme ve subnetting anlamak önemlidir. [Adresleme ve alt ağlatma temellerini kapsayan bu makaleile başlayabilirsiniz.](https://support.microsoft.com/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics) Daha birçok makale ve video çevrimiçi olarak mevcuttur.

## <a name="sign-in-to-azure-portal"></a>Azure portalda oturum açın

[Azure portalında](https://portal.azure.com)oturum açın.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Bu öğreticiilk adım sanal bir ağ içinde yeni bir sanal makine oluşturmaktır.  Sanal makine, yalnızca sanal ağ dan kullanılabilir erişimini kısıtladıktan sonra işlevinize erişmek için kullanılır.

1. Kaynak **Oluştur** düğmesini seçin.

2. Arama alanında, arama `Windows Server`sonuçlarında **Windows Server** yazın ve seçin.

3. Windows Server seçenekleri listesinden **Windows Server 2019 Datacenter'ı** seçin ve **Oluştur** düğmesine basın.

4. Temel **Bilgiler** sekmesinde, görüntünün altındaki tabloda belirtildiği gibi VM ayarlarını kullanın:

    >[!div class="mx-imgBorder"]
    >![Yeni bir Windows VM için temel ler sekmesi](./media/functions-create-private-site-access/create-vm-3.png)

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | **Abonelik** | Aboneliğiniz | Kaynaklarınızın oluşturulduğu abonelik. |
    | [**Kaynak grubu**](../azure-resource-manager/management/overview.md) | myResourceGroup | Bu öğretici için tüm kaynakları içerecek kaynak grubunu seçin.  Aynı kaynak grubunu kullanmak, bu öğreticiyle yaptığınız da kaynakları temizlemenizi kolaylaştırır. |
    | **Sanal makine adı** | myVM | VM adının kaynak grubunda benzersiz olması gerekir |
    | [**Bölge**](https://azure.microsoft.com/regions/) | (Abd) Kuzey Orta ABD | Size yakın veya erişilecek işlevlerin yakınında bir bölge seçin. |
    | **Genel gelen bağlantı noktaları** | None | Internet'ten VM'ye gelen bağlantı olmadığından emin olmak için **Yok'u** seçin. VM'ye uzaktan erişim Azure Bastion hizmeti aracılığıyla yapılandırılacaktır. |

5. **Ağ** sekmesini seçin ve yeni bir sanal ağ yapılandırmak için **yeni oluştur'u** seçin.

    >[!div class="mx-imgBorder"]
    >![Yeni VM için yeni bir sanal ağ oluşturma](./media/functions-create-private-site-access/create-vm-networking.png)

6. **Sanal ağ oluştur'da,** görüntünün altındaki tablodaki ayarları kullanın:

    >[!div class="mx-imgBorder"]
    >![Yeni VM için yeni bir sanal ağ oluşturma](./media/functions-create-private-site-access/create-vm-vnet-1.png)

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | **Adı** | myResourceGroup-vnet | Sanal ağınız için oluşturulan varsayılan adı kullanabilirsiniz. |
    | **Adres aralığı** | 10.10.0.0/16 | Sanal ağ için tek bir adres aralığı kullanın. |
    | **Alt ağ adı** | Öğretici | Alt ağ adı. |
    | **Adres aralığı** (subnet) | 10.10.1.0/24 | Alt ağ boyutu, alt ağa kaç arabirim eklenebilir tanımlar. Bu alt ağ VM tarafından kullanılır. Bir `/24` alt ağ 254 ana bilgisayar adresi sağlar. |

7. Sanal ağı oluşturmak için **Tamam'ı** seçin.
8. Ağ sekmesinde, Ortak **IP**için **Hiçbiri'nin** **seçildiğinden** emin olun.
9. **Yönetim** sekmesini seçin, ardından **Tanılama depolama hesabında**yeni bir Depolama hesabı oluşturmak için **yeni oluştur'u** seçin.
10. **Kimlik,** **Otomatik kapatma**ve **Yedekleme** bölümleri için varsayılan değerleri bırakın.
11. **İncele ve oluştur**’u seçin. Doğrulama tamamlandıktan sonra **Oluştur'u**seçin. VM oluşturma işlemi birkaç dakika sürer.

## <a name="configure-azure-bastion"></a>Azure Kalesi'ni Yapılandırma

[Azure Bastion,](https://azure.microsoft.com/services/azure-bastion/) sanal makinelere doğrudan Azure portalından güvenli RDP ve SSH erişimi sağlayan tam olarak yönetilen bir Azure hizmetidir. Azure Bastion hizmetini kullanmak, RDP erişimiyle ilgili ağ ayarlarını yapılandırma gereksinimini ortadan kaldırır.

1. Portalda, kaynak grubu görünümünün üst kısmında **Ekle'yi** seçin.
2. Arama alanında "Bastion" yazın.  "Bastion" seçeneğini belirleyin.
3. Yeni bir Azure Bastion kaynağı oluşturma işlemini başlatmak için **Oluştur'u** seçin. Henüz bir alt ağ olmadığı için **Sanal ağ** bölümünde `AzureBastionSubnet` bir hata iletisi fark edeceksiniz. Alt ağ aşağıdaki adımlarda oluşturulur. Resmin altındaki tablodaki ayarları kullanın:

    >[!div class="mx-imgBorder"]
    >![Azure Bastion oluşturma nın başlangıcı](./media/functions-create-private-site-access/create-bastion-basics-1.png)

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | **Adı** | myBastion | Yeni Bastion kaynağının adı |
    | **Bölge** | Orta Kuzey ABD | Kendinize veya işlevinizin erişeceği diğer hizmetlere yakın bir [bölge](https://azure.microsoft.com/regions/) seçin. |
    | **Sanal ağ** | myResourceGroup-vnet | Bastion kaynağının oluşturulacağı sanal ağ |
    | **Alt ağ** | AzureBastionSubnet | Yeni Bastion ana bilgisayarı kaynağının dağıtılacayacağı sanal ağınızdaki alt ağ. Ad değerini `AzureBastionSubnet`kullanarak bir alt ağ oluşturmanız gerekir. Bu değer, Azure'un Bastion kaynaklarını hangi alt ağa dağıtacağını bilmesini sağlar. En az `/27` veya daha büyük bir`/27`alt `/26`ağ kullanmanız gerekir ( , , vb). |

    > [!NOTE]
    > Azure Bastion kaynağı oluşturmak için ayrıntılı, adım adım kılavuz [için, Azure Kalesi ana bilgisayar](../bastion/bastion-create-host-portal.md) oluşturma öğreticisine bakın.

4. Azure'un Azure Bastion ana bilgisayarını sağabileceği bir alt ağ oluşturun. Alt **ağ yapılandırması yönet'i** seçmek, yeni bir alt ağ tanımlayabileceğiniz yeni bir bölme açar.  Yeni bir alt ağ oluşturmak için **+ Subnet'i** seçin.
5. Alt ağ adında `AzureBastionSubnet` nisbeten olmalı ve alt ağ `/27`öneki en az .  Alt ağı oluşturmak için **Tamam'ı** seçin.

    >[!div class="mx-imgBorder"]
    >![Azure Bastion ana bilgisayarı için alt ağ oluşturma](./media/functions-create-private-site-access/create-bastion-subnet-2.png)

6. Bir **Bastion oluştur** sayfasında, kullanılabilir `AzureBastionSubnet` alt ağlar listesinden yeni oluşturulanı seçin.

    >[!div class="mx-imgBorder"]
    >![Belirli bir alt nete sahip bir Azure Bastion ana bilgisayarı oluşturma](./media/functions-create-private-site-access/create-bastion-basics-2.png)

7. **Oluştur'& Gözden Geçir'i**seçin. Doğrulama tamamlandıktan sonra **Oluştur'u**seçin. Azure Bastion kaynağının oluşturulması birkaç dakika sürer.

## <a name="create-an-azure-functions-app"></a>Azure İşlevleri uygulaması oluşturma

Bir sonraki adım, [Tüketim planını](functions-scale.md#consumption-plan)kullanarak Azure'da bir işlev uygulaması oluşturmaktır. İşlev kodunuzu daha sonra öğreticide bu kaynağa dağıtırsınız.

1. Portalda, kaynak grubu görünümünün üst kısmında **Ekle'yi** seçin.
2. **İşlem > İşlev Uygulamasını** seçin
3. Temel **Bilgiler** bölümünde, aşağıdaki tabloda belirtildiği gibi işlev uygulaması ayarlarını kullanın.

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | **Kaynak Grubu** | myResourceGroup | Bu öğretici için tüm kaynakları içerecek kaynak grubunu seçin.  İşlev uygulaması ve VM için aynı kaynak grubunu kullanmak, bu öğreticiyle yaptığınız da kaynakları temizlemenizi kolaylaştırır. |
    | **İşlev Uygulaması adı** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı tanımlayan ad. Geçerli karakterler a-z (büyük/küçük harf duyarsız), 0-9 ve -'dir. |
    | **Yayımlamak** | Kod | Kod dosyalarını veya Docker kapsayıcısını yayımlama seçeneği. |
    | **Çalışma zamanı yığını** | Tercih edilen dil | Tercih ettiğiniz işlev programlama dilini destekleyen bir çalışma zamanı seçin. |
    | **Bölge** | Orta Kuzey ABD | Kendinize veya işlevinizin erişeceği diğer hizmetlere yakın bir [bölge](https://azure.microsoft.com/regions/) seçin. |

    **Sonraki: Hosting >** düğmesini seçin.
4. **Barındırma** bölümü için, uygun **Depolama hesabı**, **İşletim sistemi**seçin ve aşağıdaki tabloda açıklanan bir **plan.**

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | **Depolama hesabı** | Genel olarak benzersiz bir ad | İşlev uygulamanız tarafından kullanılan bir depolama hesabı oluşturun. Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir. Depolama [hesabı gereksinimlerini](./functions-scale.md#storage-account-requirements)karşılaması gereken varolan bir hesabı da kullanabilirsiniz. |
    | **İşletim sistemi** | Tercih edilen işletim sistemi | Çalışma zamanı yığını seçiminize göre bir işletim sistemi sizin için önceden seçilir, ancak gerekirse ayarı değiştirebilirsiniz. |
    | **Planlama** | Tüketim | [Barındırma planı,](./functions-scale.md) işlev uygulamasının nasıl ölçeklendirildiği ve her örnek için kullanılabilir kaynakları belirler. |
5. Uygulama yapılandırma seçimlerini incelemek için **Gözden Geçir + Oluştur'u** seçin.
6. İşlev uygulamasını sağlamak ve dağıtmak için **Oluştur**'u seçin.

## <a name="configure-access-restrictions"></a>Erişim kısıtlamalarını yapılandırma

Bir sonraki adım, yalnızca sanal ağdaki kaynakların işlevi çağırabilmesini sağlamak için [erişim kısıtlamalarını](../app-service/app-service-ip-restrictions.md) yapılandırmaktır.

[Özel site](functions-networking-options.md#private-site-access) erişimi, işlev uygulaması ile belirtilen sanal ağ arasında bir Azure Sanal Ağ [hizmeti bitiş noktası](../virtual-network/virtual-network-service-endpoints-overview.md) oluşturarak etkinleştirilir. Erişim kısıtlamaları hizmet bitiş noktaları üzerinden uygulanır. Hizmet bitiş noktaları, yalnızca belirtilen sanal ağ içinden kaynaklanan trafiğin belirlenen kaynağa erişmesini sağlar. Bu durumda, atanan kaynak Azure İşlevi'dir.

1. İşlev uygulamasında, Platform **özellikleri** sekmesine gidin. Ağ Özelliği Durumu bölümünü açmak için *Ağ* bölümü üstbilgisinin altındaki **Ağ** bağlantısını seçin.
2. **Ağ Özelliği Durumu** sayfası, Azure Ön Kapı, Azure CDN ve Erişim Kısıtlamaları yapılandırmak için başlangıç noktasıdır. Özel site erişimini yapılandırmak için **Access Kısıtlamalarını Yapılandır'ı** seçin.
3. Erişim **Kısıtlamaları** sayfasında yalnızca varsayılan kısıtlamayı yerinde görürsünüz. Varsayılan, işlev uygulamasına erişime herhangi bir kısıtlama getirmiyor.  Özel bir site erişim kısıtlaması yapılandırması oluşturmak için **Ekle kuralını** seçin.
4. Erişim **Kısıtlaması Ekle** bölmesinde, **Tür** açılır kutusundan **Sanal Ağ'ı** seçin ve ardından önceden oluşturulmuş sanal ağı ve alt ağı seçin.
5. **Erişim Kısıtlamaları** sayfası artık yeni bir kısıtlama olduğunu gösterir. **Bitiş Noktası durumunun** 'dan `Disabled` `Provisioning` 'a `Enabled`geçmesi birkaç saniye sürebilir

    >[!IMPORTANT]
    > Her işlev uygulaması, işlev uygulaması dağıtımlarını yönetmek için kullanılan gelişmiş bir [araç (Kudu) sitesine](../app-service/app-service-ip-restrictions.md#scm-site) sahiptir. Bu siteye bir URL'den `<FUNCTION_APP_NAME>.scm.azurewebsites.net`erişilir: . Bu dağıtım sitesinde erişim kısıtlamaları etkinleştirilmedığından, proje kodunuzu sanal ağ içinde aracı sağlamak zorunda kalmadan proje kodunuzu yerel bir geliştirici iş istasyonundan dağıtmaya veya hizmet oluşturmaya devam edebilirsiniz.

## <a name="access-the-functions-app"></a>Fonksiyonlar uygulamasına erişin

1. Daha önce oluşturulmuş işlev uygulamasına geri dönün.  Genel **Bakış** bölümünde URL'yi kopyalayın.

    >[!div class="mx-imgBorder"]
    >![İşlev uygulaması URL'sini alın](./media/functions-create-private-site-access/access-function-overview.png)

2. İşlev uygulamasına sanal ağınızın dışındaki bilgisayarınızdan şimdi erişmeye çalışırsanız, uygulamanın durdurulduğunu belirten bir HTTP 403 sayfası alırsınız.  Uygulama durdurulmuyor. Yanıt aslında bir HTTP 403 IP Yasak durumudur.
3. Artık işlevinize, sanal ağınıza bağlı olan daha önce oluşturulmuş sanal makineden erişeceksiniz. Siteye VM'den erişmek için Azure Bastion hizmeti aracılığıyla VM'ye bağlanmanız gerekir.  Önce **Bağlan'ı** seçin ve ardından **Bastion'u**seçin.
4. Sanal makineye giriş yapmak için gerekli kullanıcı adı ve parolayı sağlayın.  **Bağlan**’ı seçin. Sanal makineyle etkileşimedebilmeniz için yeni bir tarayıcı penceresi açılır.
5. Bu VM işlevi sanal ağ üzerinden erişip olduğundan, VM'deki web tarayıcısından siteye erişmek mümkündür.  İşlev uygulamasına yalnızca belirlenen sanal ağ içinden erişilebilir olmakla birlikte, ortak bir DNS girişinin kaldığını unutmayın. Yukarıda gösterildiği gibi, siteye erişmeye çalışmak HTTP 403 yanıtıyla sonuçlanır.

## <a name="create-a-function"></a>İşlev oluşturma

Bu öğreticideki bir sonraki adım, HTTP tarafından tetiklenen bir Azure İşi oluşturmaktır. İşlevin http GET veya POST aracılığıyla çağırılanması "Merhaba, {ad}" yanıtıyla sonuçlanmalıdır.  

1. Azure İşlevleri uygulamanızı oluşturmak ve dağıtmak için aşağıdaki hızlı başlangıçlardan birini izleyin.

    * [Visual Studio Code](./functions-create-first-function-vs-code.md)
    * [Visual Studio](./functions-create-your-first-function-visual-studio.md)
    * [Komut satırı](./functions-create-first-azure-function-azure-cli.md)
    * [Maven (Java)](./functions-create-first-java-maven.md)

2. Azure İşlevler projenizi yayınlarken, bu öğreticide daha önce oluşturduğunuz işlev uygulaması kaynağını seçin.
3. İşlevin dağıtılmış olduğunu doğrulayın.

    >[!div class="mx-imgBorder"]
    >![İşlevler listesinde dağıtılan işlev](./media/functions-create-private-site-access/verify-deployed-function.png)

## <a name="invoke-the-function-directly"></a>İşlevi doğrudan çağırma

1. İşleverişimini test etmek için işlev URL'sini kopyalamanız gerekir. Dağıtılan işlevi seçin ve ardından **</> İşlev URL'sini alın'ı**seçin. Ardından URL'yi panonuza kopyalamak için **Kopyala** düğmesini tıklatın.

    >[!div class="mx-imgBorder"]
    >![İşlev URL'sini kopyalama](./media/functions-create-private-site-access/get-function-url.png)

    > [!NOTE]
    > İşlev çalıştığında, portalda işlev çalışma zamanının başlatılamadığını belirten bir çalışma zamanı hatası görürsünüz. İleti metnine rağmen, işlev uygulaması gerçekten çalışıyor. Hata, portalın çalışma saatini denetlemek için sorgulanmasını engelleyen yeni erişim kısıtlamalarının bir sonucudur.

2. URL'yi bir web tarayıcısına yapıştırın. Artık işlev uygulamasına sanal ağınızın dışındaki bir bilgisayardan erişmeye çalıştığınızda, uygulamanın durdurulduğunu belirten bir HTTP 403 yanıtı alırsınız.

## <a name="invoke-the-function-from-the-virtual-network"></a>Sanal ağdan işlevi çağırma

Sanal ağda yapılandırılan VM'de bir web tarayıcısı (Azure Bastion hizmetini kullanarak) aracılığıyla fonksiyona erişmek başarı sağlar!

>[!div class="mx-imgBorder"]
>![Azure Bastion ile Azure İşlevine erişin](./media/functions-create-private-site-access/access-function-via-bastion-final.png)

[!INCLUDE [clean-up-section-portal](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar


> [!div class="nextstepaction"]
> [İşlevler'deki ağ seçenekleri hakkında daha fazla bilgi edinin](./functions-networking-options.md)
