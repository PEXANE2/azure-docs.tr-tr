---
title: Azure Işlevleri için özel site erişimini etkinleştirme
description: Azure Işlevleri için Azure sanal ağ özel site erişimini ayarlamayı öğrenin.
author: mcollier
ms.author: mcollier
ms.service: azure-functions
ms.topic: tutorial
ms.date: 02/15/2020
ms.openlocfilehash: ada08de182791c6ecb2b83ef3b924bf40975e1ee
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "78851329"
---
# <a name="tutorial-establish-azure-functions-private-site-access"></a>Öğretici: Azure Işlevleri özel site erişimi oluşturma

Bu öğreticide, Azure Işlevleri ile [özel site erişiminin](./functions-networking-options.md#private-site-access) nasıl etkinleştirileceği gösterilmektedir. Özel site erişimini kullanarak, işlev kodunuzun yalnızca belirli bir sanal ağdan tetiklenmesi zorunlu kılabilirsiniz.

Özel site erişimi, işlev uygulamasına erişimin belirli bir sanal ağla sınırlı olması gerektiğinde senaryolar için faydalıdır. Örneğin, işlev uygulaması yalnızca belirli bir kuruluşun çalışanları veya belirtilen sanal ağ içinde (başka bir Azure Işlevi, Azure sanal makinesi veya AKS kümesi gibi) bulunan hizmetler için geçerli olabilir.

Bir Işlevler uygulamasının, sanal ağ içindeki Azure kaynaklarına erişmesi veya [hizmet uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md)aracılığıyla bağlanması gerekiyorsa, [sanal ağ tümleştirmesi](./functions-create-vnet.md) gerekir.

Bu öğreticide, işlev uygulamanız için özel site erişimini nasıl yapılandıracağınızı öğreneceksiniz:

> [!div class="checklist"]
> * Sanal makine oluşturma
> * Azure savunma hizmeti oluşturma
> * Azure İşlevleri uygulaması oluşturma
> * Sanal ağ hizmeti uç noktası yapılandırma
> * Azure Işlevi oluşturma ve dağıtma
> * İşlevi sanal ağ dışından ve içinden çağır

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="topology"></a>Topoloji

Aşağıdaki diyagramda oluşturulacak çözümün mimarisi gösterilmektedir:

![Özel site erişim çözümü için üst düzey mimari diyagramı](./media/functions-create-private-site-access/topology.png)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide IP adresleme ve alt ağ oluşturma hakkında bilgi almanız önemlidir. [Bu makaleyle, adresleme ve alt ağ oluşturma temellerini kapsayan bir](https://support.microsoft.com/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics)başlangıç yapabilirsiniz. Daha birçok makale ve video çevrimiçi olarak kullanılabilir.

## <a name="sign-in-to-azure-portal"></a>Azure portalda oturum açın

[Azure Portal](https://portal.azure.com) oturum açın.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Bu öğreticideki ilk adım, sanal ağ içinde yeni bir sanal makine oluşturmaktır.  Sanal makine, yalnızca sanal ağ içinden kullanılabilir hale gelen erişimi kısıtladıktan sonra işlevinizin erişim için kullanılacaktır.

1. **Kaynak oluştur** düğmesini seçin.

2. Arama alanına yazın `Windows Server`ve arama sonuçlarında **Windows Server** ' ı seçin.

3. Windows Server seçenekleri listesinden **Windows server 2019 Datacenter** ' ı seçin ve **Oluştur** düğmesine basın.

4. **Temel bilgiler** sekmesinde, görüntünün altındaki tabloda belirtilen VM ayarlarını kullanın:

    >[!div class="mx-imgBorder"]
    >![Yeni bir Windows sanal makinesi için temel bilgiler sekmesi](./media/functions-create-private-site-access/create-vm-3.png)

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | **Abonelik** | Aboneliğiniz | Kaynaklarınızın oluşturulduğu abonelik. |
    | [**Kaynak grubu**](../azure-resource-manager/management/overview.md) | myResourceGroup | Bu öğreticinin tüm kaynaklarını içerecek kaynak grubunu seçin.  Aynı kaynak grubunu kullanmak, bu öğreticiyi tamamladığınızda kaynakları temizlemeyi kolaylaştırır. |
    | **Sanal makine adı** | myVM | VM adının kaynak grubunda benzersiz olması gerekir |
    | [**Bölge**](https://azure.microsoft.com/regions/) | ABD Orta Kuzey ABD | Size yakın veya erişilecek işlevlere yakın bir bölge seçin. |
    | **Ortak gelen bağlantı noktaları** | Hiçbiri | İnternet 'ten sanal makineye gelen bağlantı olmadığından emin olmak için **hiçbiri** ' ni seçin. SANAL makineye uzaktan erişim, Azure savunma hizmeti aracılığıyla yapılandırılır. |

5. Yeni bir sanal ağ yapılandırmak için **ağ** sekmesini seçin ve **Yeni oluştur** ' u seçin.

    >[!div class="mx-imgBorder"]
    >![Yeni VM için yeni bir sanal ağ oluşturun](./media/functions-create-private-site-access/create-vm-networking.png)

6. **Sanal ağ oluştur**' da, görüntünün altındaki tabloda bulunan ayarları kullanın:

    >[!div class="mx-imgBorder"]
    >![Yeni VM için yeni bir sanal ağ oluşturun](./media/functions-create-private-site-access/create-vm-vnet-1.png)

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | **Adı** | myResourceGroup-VNET | Sanal ağınız için oluşturulan varsayılan adı kullanabilirsiniz. |
    | **Adres aralığı** | 10.10.0.0/16 | Sanal ağ için tek bir adres aralığı kullanın. |
    | **Alt ağ adı** | Eğitmen | Alt ağın adı. |
    | **Adres aralığı** (alt ağ) | 10.10.1.0/24 | Alt ağ boyutu, alt ağa kaç arabirim eklenebileceğini tanımlar. Bu alt ağ, VM tarafından kullanılır. Bir `/24` alt ağ 254 ana bilgisayar adresi sağlar. |

7. Sanal ağı oluşturmak için **Tamam ' ı** seçin.
8. **Ağ** sekmesine geri döndüğünüzde, **genel IP**için **hiçbirinin** seçili olmadığından emin olun.
9. **Yönetim** sekmesini seçin ve ardından **Tanılama depolama hesabı**' nda yeni **Oluştur** ' u seçerek yeni bir depolama hesabı oluşturun.
10. **Kimlik**, **otomatik olarak kapatmalar**ve **yedekleme** bölümleri için varsayılan değerleri bırakın.
11. **İncele ve oluştur**’u seçin. Doğrulama tamamlandıktan sonra **Oluştur**' u seçin. VM oluşturma işlemi birkaç dakika sürer.

## <a name="configure-azure-bastion"></a>Azure savunma 'yi yapılandırma

[Azure](https://azure.microsoft.com/services/azure-bastion/) savunma, doğrudan Azure Portal sanal MAKINELERE güvenli RDP ve SSH erişimi sağlayan, tam olarak yönetilen bir Azure hizmetidir. Azure savunma hizmeti 'nin kullanılması, RDP erişimiyle ilgili ağ ayarlarını yapılandırma gereksinimini ortadan kaldırır.

1. Portalda, kaynak grubu görünümünün üst kısmında **Ekle** ' yi seçin.
2. Arama alanına "savunma" yazın.  "Savunma" seçeneğini belirleyin.
3. Yeni bir Azure savunma kaynağı oluşturma işlemini başlatmak için **Oluştur** ' u seçin. Henüz bir `AzureBastionSubnet` alt ağ olmadığı için **sanal ağ** bölümünde bir hata iletisi görürsünüz. Alt ağ aşağıdaki adımlarda oluşturulur. Görüntünün altındaki tabloda bulunan ayarları kullanın:

    >[!div class="mx-imgBorder"]
    >![Azure savunma oluşturma başlangıcı](./media/functions-create-private-site-access/create-bastion-basics-1.png)

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | **Adı** | tasyon | Yeni savunma kaynağının adı |
    | **Bölge** | Orta Kuzey ABD | Kendinize veya işlevinizin erişeceği diğer hizmetlere yakın bir [bölge](https://azure.microsoft.com/regions/) seçin. |
    | **Sanal ağ** | myResourceGroup-VNET | Savunma kaynağının oluşturulacağı sanal ağ |
    | **Alt ağ** | AzureBastionSubnet | Sanal ağınızdaki, yeni savunma ana bilgisayar kaynağının dağıtılacağı alt ağ. Ad değerini `AzureBastionSubnet`kullanarak bir alt ağ oluşturmanız gerekir. Bu değer, Azure 'un savunma kaynaklarını hangi alt ağa dağıtacağınızı bilmesini sağlar. En az `/27` veya daha büyük bir alt ağ (`/27`, `/26`, vb.) kullanmanız gerekir. |

    > [!NOTE]
    > Azure savunma kaynağı oluşturmaya yönelik ayrıntılı bir adım adım kılavuz için, [Azure savunma ana bilgisayarı oluşturma](../bastion/bastion-create-host-portal.md) öğreticisine bakın.

4. Azure 'un Azure savunma konağını sağlayabildiği bir alt ağ oluşturun. **Alt ağ yapılandırmasını Yönet** ' i seçmek yeni bir alt ağ tanımlayabileceğiniz yeni bir pencere açar.  Yeni bir alt ağ oluşturmak için **+ alt ağ** ' ı seçin.
5. Alt ağ, ad `AzureBastionSubnet` olmalıdır ve alt ağ ön eki en az `/27`olmalıdır.  Alt ağı oluşturmak için **Tamam ' ı** seçin.

    >[!div class="mx-imgBorder"]
    >![Azure savunma ana bilgisayarı için alt ağ oluşturma](./media/functions-create-private-site-access/create-bastion-subnet-2.png)

6. Savunma **Oluştur** sayfasında, kullanılabilir alt ağlar listesinden yeni oluşturulan `AzureBastionSubnet` ' ı seçin.

    >[!div class="mx-imgBorder"]
    >![Belirli bir alt ağa sahip bir Azure savunma ana bilgisayarı oluşturma](./media/functions-create-private-site-access/create-bastion-basics-2.png)

7. **& oluştur**' u seçin. Doğrulama tamamlandıktan sonra **Oluştur**' u seçin. Azure savunma kaynağının oluşturulması birkaç dakika sürer.

## <a name="create-an-azure-functions-app"></a>Azure İşlevleri uygulaması oluşturma

Sonraki adım, [Tüketim planını](functions-scale.md#consumption-plan)kullanarak Azure 'da bir işlev uygulaması oluşturmaktır. İşlev kodunuzu bu kaynağa daha sonra öğreticide dağıtırsınız.

1. Portalda, kaynak grubu görünümünün üst kısmında **Ekle** ' yi seçin.
2. **İşlem > seçin işlev uygulaması**
3. **Temel bilgiler** bölümünde, aşağıdaki tabloda belirtilen işlev uygulaması ayarlarını kullanın.

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | **Kaynak grubu** | myResourceGroup | Bu öğreticinin tüm kaynaklarını içerecek kaynak grubunu seçin.  İşlev uygulaması ve sanal makine için aynı kaynak grubunun kullanılması, bu öğreticiyle işiniz bittiğinde kaynakları temizlemeyi kolaylaştırır. |
    | **İşlev Uygulaması adı** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı tanımlayan ad. Geçerli karakterler şunlardır-z (büyük/küçük harf duyarsız), 0-9 ve-. |
    | **Yayımlama** | Kod | Kod dosyalarını veya Docker kapsayıcısını yayımlama seçeneği. |
    | **Çalışma zamanı yığını** | Tercih edilen dil | Tercih ettiğiniz işlev programlama dilini destekleyen bir çalışma zamanı seçin. |
    | **Bölge** | Orta Kuzey ABD | Kendinize veya işlevinizin erişeceği diğer hizmetlere yakın bir [bölge](https://azure.microsoft.com/regions/) seçin. |

    **Sonraki: barındırma >** düğmesini seçin.
4. **Barındırma** bölümünde, doğru **depolama hesabı**, **işletim sistemi**ve aşağıdaki tabloda açıklanan **planı** seçin.

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | **Depolama hesabı** | Genel olarak benzersiz bir ad | İşlev uygulamanız tarafından kullanılan bir depolama hesabı oluşturun. Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir. Ayrıca, [depolama hesabı gereksinimlerini](./functions-scale.md#storage-account-requirements)karşılaması gereken mevcut bir hesabı da kullanabilirsiniz. |
    | **İşletim sistemi** | Tercih edilen işletim sistemi | Çalışma zamanı yığını seçiminize göre sizin için bir işletim sistemi önceden seçilmiştir, ancak gerekirse ayarı değiştirebilirsiniz. |
    | **Planlama** | Tüketim | [Barındırma planı](./functions-scale.md) , işlev uygulamasının nasıl ölçeklendirileceğini ve her örnek için kullanılabilir kaynakları belirler. |
5. Uygulama yapılandırma seçimlerini gözden geçirmek için **gözden geçir + oluştur** ' u seçin.
6. İşlev uygulamasını sağlamak ve dağıtmak için **Oluştur**'u seçin.

## <a name="configure-access-restrictions"></a>Erişim kısıtlamalarını yapılandırma

Bir sonraki adım, yalnızca sanal ağ üzerindeki kaynakların işlevi çağırabilmesini sağlamak için [erişim kısıtlamalarını](../app-service/app-service-ip-restrictions.md) yapılandırmaktır.

[Özel site](functions-networking-options.md#private-site-access) erişimi, işlev uygulaması ve belirtilen sanal ağ arasında bir Azure sanal ağ [hizmeti uç noktası](../virtual-network/virtual-network-service-endpoints-overview.md) oluşturularak etkinleştirilir. Erişim kısıtlamaları, hizmet uç noktaları aracılığıyla uygulanır. Hizmet uç noktaları, yalnızca belirtilen sanal ağ içinden gelen trafiğin belirlenen kaynağa erişebildiğinden emin olun. Bu durumda, belirlenen kaynak Azure Işlevidir.

1. İşlev uygulaması içinde **platform özellikleri** sekmesine geçin *. ağ özelliği* durumu bölümünü açmak için ağ bölümü üst bilgisi altındaki **ağ** bağlantısını seçin.
2. **Ağ özelliği durumu** sayfası, Azure ön kapısını, Azure CDN ve ayrıca erişim kısıtlamalarını yapılandırmaya yönelik başlangıç noktasıdır. Özel site erişimini yapılandırmak için **erişim kısıtlamalarını Yapılandır** ' ı seçin.
3. **Erişim kısıtlamaları** sayfasında, yalnızca varsayılan kısıtlamayı görürsünüz. Varsayılan değer, işlev uygulamasına erişim için herhangi bir kısıtlama yerleştirmez.  Özel bir site erişim kısıtlama yapılandırması oluşturmak için **Kural Ekle** ' yi seçin.
4. **Erişim kısıtlaması Ekle** bölmesinde, **tür** açılan kutusundan **sanal ağ** ' ı seçin, ardından daha önce oluşturulmuş sanal ağı ve alt ağı seçin.
5. **Erişim kısıtlamaları** sayfası artık yeni bir kısıtlama olduğunu gösterir. **Uç nokta durumunun** `Disabled` ile arasında `Provisioning` değiştirilmesi birkaç saniye sürebilir `Enabled`.

    >[!IMPORTANT]
    > Her işlev uygulaması, işlev uygulaması dağıtımlarını yönetmek için kullanılan bir [Gelişmiş araç (kudu) sitesine](../app-service/app-service-ip-restrictions.md#scm-site) sahiptir. Bu siteye şu şekilde bir URL 'den erişilir: `<FUNCTION_APP_NAME>.scm.azurewebsites.net`. Bu dağıtım sitesinde erişim kısıtlamaları etkinleştirilmediği için, sanal ağ içinde bir aracı sağlamaya gerek kalmadan, proje kodunuzu yerel bir geliştirici iş istasyonunda veya derleme hizmetinden de dağıtabilirsiniz.

## <a name="access-the-functions-app"></a>İşlevler uygulamasına erişme

1. Önceden oluşturulmuş işlev uygulamasına geri dönün.  **Genel bakış** bölümünde URL 'yi kopyalayın.

    >[!div class="mx-imgBorder"]
    >![Işlev uygulaması URL 'sini al](./media/functions-create-private-site-access/access-function-overview.png)

2. Artık işlev uygulamasına sanal ağınızın dışındaki bilgisayarınızdan erişmeye çalışırsanız, uygulamanın durdurulduğunu belirten bir HTTP 403 sayfası alacaksınız.  Uygulama durdurulmadı. Yanıt aslında HTTP 403 IP yasak durumundadır.
3. Artık işleviniz, sanal ağınıza bağlı olan, daha önce oluşturulmuş olan sanal makineden erişebileceksiniz. Siteye sanal makineye erişmek için Azure savunma hizmeti aracılığıyla VM 'ye bağlanmanız gerekir.  Önce **Bağlan** ' **ı seçin ve**ardından savunma ' yı seçin.
4. Sanal makinede oturum açmak için gereken kullanıcı adını ve parolayı belirtin.  **Bağlan**’ı seçin. Sanal makineyle etkileşime geçmesini sağlamak için yeni bir tarayıcı penceresi açılır.
5. Bu VM, sanal ağ üzerinden işleve eriştiği için, siteye VM 'deki Web tarayıcısından erişmek mümkündür.  İşlev uygulamasına yalnızca belirtilen sanal ağ içinden erişilebilirken, genel bir DNS girişi kalacağına dikkat edin. Yukarıda gösterildiği gibi, siteye erişmeye çalışmak HTTP 403 yanıtına neden olur.

## <a name="create-a-function"></a>İşlev oluşturma

Bu öğreticideki bir sonraki adım, HTTP ile tetiklenen bir Azure Işlevi oluşturmaktır. İşlevi bir HTTP GET veya POST aracılığıyla çağırmak, "Hello, {Name}" adlı bir Yanıt ile sonuçlanır.  

1. Azure Işlevleri uygulamanızı oluşturmak ve dağıtmak için aşağıdaki hızlı başlangıçlardan birini izleyin.

    * [Visual Studio Code](./functions-create-first-function-vs-code.md)
    * [Visual Studio](./functions-create-your-first-function-visual-studio.md)
    * [Komut satırı](./functions-create-first-azure-function-azure-cli.md)
    * [Maven (Java)](./functions-create-first-java-maven.md)

2. Azure Işlevleri projenizi yayımlarken, bu öğreticide daha önce oluşturduğunuz işlev uygulaması kaynağını seçin.
3. İşlevin dağıtıldığını doğrulayın.

    >[!div class="mx-imgBorder"]
    >![İşlev listesinde dağıtılan işlev](./media/functions-create-private-site-access/verify-deployed-function.png)

## <a name="invoke-the-function-directly"></a>İşlevi doğrudan çağır

1. İşleve erişimi test etmek için, işlev URL 'sini kopyalamanız gerekir. Dağıtılan işlevi seçin ve ardından **</> işlev URL 'Sini al**' ı seçin. Ardından, URL 'YI panonuza kopyalamak için **Kopyala** düğmesine tıklayın.

    >[!div class="mx-imgBorder"]
    >![İşlev URL 'sini kopyalayın](./media/functions-create-private-site-access/get-function-url.png)

    > [!NOTE]
    > İşlev çalıştırıldığında, portalda işlev çalışma zamanının başlayamadığını belirten bir çalışma zamanı hatası görürsünüz. İleti metnine rağmen, işlev uygulaması aslında çalışır. Bu hata, portalın çalışma zamanında denetim için sorgulanmasını önleyen yeni erişim kısıtlamalarının bir sonucudur.

2. URL 'YI bir Web tarayıcısına yapıştırın. Artık işlev uygulamasına sanal ağınızın dışındaki bir bilgisayardan erişmeye çalıştığınızda, uygulamanın durdurulduğunu belirten bir HTTP 403 yanıtı alırsınız.

## <a name="invoke-the-function-from-the-virtual-network"></a>İşlevi sanal ağdan çağır

Sanal ağ üzerinde yapılandırılan VM 'deki bir Web tarayıcısı üzerinden işleve erişim (Azure savunma hizmeti kullanılarak) başarılı olur!

>[!div class="mx-imgBorder"]
>![Azure 'a savunma aracılığıyla Azure Işlevine erişin](./media/functions-create-private-site-access/access-function-via-bastion-final.png)

[!INCLUDE [clean-up-section-portal](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar


> [!div class="nextstepaction"]
> [Işlevlerde ağ seçenekleri hakkında daha fazla bilgi edinin](./functions-networking-options.md)
