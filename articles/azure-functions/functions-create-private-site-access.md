---
title: Azure Işlevleri için özel site erişimini etkinleştirme
description: Azure Işlevleri için Azure sanal ağ özel site erişimini ayarlamayı öğrenin.
author: craigshoemaker
ms.author: cshoe
ms.service: azure-functions
ms.topic: tutorial
ms.date: 06/17/2020
ms.openlocfilehash: 8e37876e0e9666097c3cf16589e64929c670b14a
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85390287"
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

1. Arama alanına **Windows Server**yazın ve arama sonuçlarında **Windows Server** ' ı seçin.

1. Windows Server seçenekleri listesinden **Windows server 2019 Datacenter** ' ı seçin ve **Oluştur** düğmesine basın.

1. _Temel bilgiler_ sekmesinde, görüntünün altındaki tabloda belirtilen VM ayarlarını kullanın:

    >[!div class="mx-imgBorder"]
    >![Yeni bir Windows sanal makinesi için temel bilgiler sekmesi](./media/functions-create-private-site-access/create-vm-3.png)

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | _Abonelik_ | Aboneliğiniz | Kaynaklarınızın oluşturulduğu abonelik. |
    | [_Kaynak grubu_](../azure-resource-manager/management/overview.md) | myResourceGroup | Bu öğreticinin tüm kaynaklarını içerecek kaynak grubunu seçin.  Aynı kaynak grubunu kullanmak, bu öğreticiyi tamamladığınızda kaynakları temizlemeyi kolaylaştırır. |
    | _Sanal makine adı_ | myVM | VM adının kaynak grubunda benzersiz olması gerekir |
    | [_Bölge_](https://azure.microsoft.com/regions/) | ABD Orta Kuzey ABD | Size yakın veya erişilecek işlevlere yakın bir bölge seçin. |
    | _Ortak gelen bağlantı noktaları_ | Hiçbiri | İnternet 'ten sanal makineye gelen bağlantı olmadığından emin olmak için **hiçbiri** ' ni seçin. SANAL makineye uzaktan erişim, Azure savunma hizmeti aracılığıyla yapılandırılır. |

1. Yeni bir sanal ağ yapılandırmak için _ağ_ sekmesini seçin ve **Yeni oluştur** ' u seçin.

    >[!div class="mx-imgBorder"]
    >![Yeni VM için yeni bir sanal ağ oluşturun](./media/functions-create-private-site-access/create-vm-networking.png)

1. _Sanal ağ oluştur_' da, görüntünün altındaki tabloda bulunan ayarları kullanın:

    >[!div class="mx-imgBorder"]
    >![Yeni VM için yeni bir sanal ağ oluşturun](./media/functions-create-private-site-access/create-vm-vnet-1.png)

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | _Adı_ | myResourceGroup-VNET | Sanal ağınız için oluşturulan varsayılan adı kullanabilirsiniz. |
    | _Adres aralığı_ | 10.10.0.0/16 | Sanal ağ için tek bir adres aralığı kullanın. |
    | _Alt ağ adı_ | Öğretici | Alt ağın adı. |
    | _Adres aralığı_ (alt ağ) | 10.10.1.0/24 | Alt ağ boyutu, alt ağa kaç arabirim eklenebileceğini tanımlar. Bu alt ağ, VM tarafından kullanılır. A/24 alt ağı 254 ana bilgisayar adresi sağlar. |

1. Sanal ağı oluşturmak için **Tamam ' ı** seçin.
1. _Ağ_ sekmesine geri döndüğünüzde, _genel IP_için **hiçbirinin** seçili olmadığından emin olun.
1. _Yönetim_ sekmesini seçin ve ardından _Tanılama depolama hesabı_' nda yeni **Oluştur** ' u seçerek yeni bir depolama hesabı oluşturun.
1. _Kimlik_, _otomatik olarak kapatmalar_ve _yedekleme_ bölümleri için varsayılan değerleri bırakın.
1. _İncele ve oluştur_’u seçin. Doğrulama tamamlandıktan sonra **Oluştur**' u seçin. VM oluşturma işlemi birkaç dakika sürer.

## <a name="configure-azure-bastion"></a>Azure savunma 'yi yapılandırma

[Azure](https://azure.microsoft.com/services/azure-bastion/) savunma, doğrudan Azure Portal sanal MAKINELERE güvenli RDP ve SSH erişimi sağlayan, tam olarak yönetilen bir Azure hizmetidir. Azure savunma hizmeti 'nin kullanılması, RDP erişimiyle ilgili ağ ayarlarını yapılandırma gereksinimini ortadan kaldırır.

1. Portalda, kaynak grubu görünümünün üst kısmında **Ekle** ' yi seçin.
1. Arama alanına, **tekne**yazın.
1. Arama sonuçlarında **savunma ' yi** seçin.
1. Yeni bir Azure savunma kaynağı oluşturma işlemini başlatmak için **Oluştur** ' u seçin. Henüz bir AzureBastionSubnet alt ağı olmadığı için _sanal ağ_ bölümünde bir hata iletisi görürsünüz. Alt ağ aşağıdaki adımlarda oluşturulur. Görüntünün altındaki tabloda bulunan ayarları kullanın:

    >[!div class="mx-imgBorder"]
    >![Azure savunma oluşturma başlangıcı](./media/functions-create-private-site-access/create-bastion-basics-1.png)

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | _Adı_ | tasyon | Yeni savunma kaynağının adı |
    | _Bölge_ | Orta Kuzey ABD | Kendinize veya işlevinizin erişeceği diğer hizmetlere yakın bir [bölge](https://azure.microsoft.com/regions/) seçin. |
    | _Sanal ağ_ | myResourceGroup-VNET | Savunma kaynağının oluşturulacağı sanal ağ |
    | _Alt ağ_ | AzureBastionSubnet | Sanal ağınızdaki, yeni savunma ana bilgisayar kaynağının dağıtılacağı alt ağ. **AzureBastionSubnet**ad değerini kullanarak bir alt ağ oluşturmanız gerekir. Bu değer, Azure 'un savunma kaynaklarını hangi alt ağa dağıtacağınızı bilmesini sağlar. En az **/27** veya daha büyük (/27,/26, vb.) bir alt ağ kullanmanız gerekir. |

    > [!NOTE]
    > Azure savunma kaynağı oluşturmaya yönelik ayrıntılı bir adım adım kılavuz için, [Azure savunma ana bilgisayarı oluşturma](../bastion/bastion-create-host-portal.md) öğreticisine bakın.

1. Azure 'un Azure savunma konağını sağlayabildiği bir alt ağ oluşturun. **Alt ağ yapılandırmasını Yönet** ' i seçmek yeni bir alt ağ tanımlayabileceğiniz yeni bir pencere açar.  Yeni bir alt ağ oluşturmak için **+ alt ağ** ' ı seçin.
1. Alt ağ **AzureBastionSubnet** adında olmalıdır ve alt ağ ön eki en az **/27**olmalıdır.  Alt ağı oluşturmak için **Tamam ' ı** seçin.

    >[!div class="mx-imgBorder"]
    >![Azure savunma ana bilgisayarı için alt ağ oluşturma](./media/functions-create-private-site-access/create-bastion-subnet-2.png)

1. Savunma _Oluştur_ sayfasında, kullanılabilir alt ağlar listesinden yeni oluşturulan **AzureBastionSubnet** ' ı seçin.

    >[!div class="mx-imgBorder"]
    >![Belirli bir alt ağa sahip bir Azure savunma ana bilgisayarı oluşturma](./media/functions-create-private-site-access/create-bastion-basics-2.png)

1. **& oluştur**' u seçin. Doğrulama tamamlandıktan sonra **Oluştur**' u seçin. Azure savunma kaynağının oluşturulması birkaç dakika sürer.

## <a name="create-an-azure-functions-app"></a>Azure İşlevleri uygulaması oluşturma

Sonraki adım, [Tüketim planını](functions-scale.md#consumption-plan)kullanarak Azure 'da bir işlev uygulaması oluşturmaktır. İşlev kodunuzu bu kaynağa daha sonra öğreticide dağıtırsınız.

1. Portalda, kaynak grubu görünümünün üst kısmında **Ekle** ' yi seçin.
1. **İşlem > seçin işlev uygulaması**
1. _Temel bilgiler_ bölümünde, aşağıdaki tabloda belirtilen işlev uygulaması ayarlarını kullanın.

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | _Kaynak grubu_ | myResourceGroup | Bu öğreticinin tüm kaynaklarını içerecek kaynak grubunu seçin.  İşlev uygulaması ve sanal makine için aynı kaynak grubunun kullanılması, bu öğreticiyle işiniz bittiğinde kaynakları temizlemeyi kolaylaştırır. |
    | _İşlev Uygulaması adı_ | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı tanımlayan ad. Geçerli karakterler şunlardır-z (büyük/küçük harf duyarsız), 0-9 ve-. |
    | _Yayımlama_ | Kod | Kod dosyalarını veya Docker kapsayıcısını yayımlama seçeneği. |
    | _Çalışma zamanı yığını_ | Tercih edilen dil | Tercih ettiğiniz işlev programlama dilini destekleyen bir çalışma zamanı seçin. |
    | _Bölge_ | Orta Kuzey ABD | Kendinize veya işlevinizin erişeceği diğer hizmetlere yakın bir [bölge](https://azure.microsoft.com/regions/) seçin. |

    **Sonraki: barındırma >** düğmesini seçin.
1. _Barındırma_ bölümü için, aşağıdaki tabloda açıklandığı gibi uygun _depolama hesabı_, _işletim sistemi_ve _planı_ seçin.

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | _Depolama hesabı_ | Genel olarak benzersiz bir ad | İşlev uygulamanız tarafından kullanılan bir depolama hesabı oluşturun. Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir. Ayrıca, [depolama hesabı gereksinimlerini](./functions-scale.md#storage-account-requirements)karşılaması gereken mevcut bir hesabı da kullanabilirsiniz. |
    | _İşletim sistemi_ | Tercih edilen işletim sistemi | Çalışma zamanı yığını seçiminize göre sizin için bir işletim sistemi önceden seçilmiştir, ancak gerekirse ayarı değiştirebilirsiniz. |
    | _Plan_ | Tüketim | [Barındırma planı](./functions-scale.md) , işlev uygulamasının nasıl ölçeklendirileceğini ve her örnek için kullanılabilir kaynakları belirler. |
1. Uygulama yapılandırma seçimlerini gözden geçirmek için **gözden geçir + oluştur** ' u seçin.
1. İşlev uygulamasını sağlamak ve dağıtmak için **Oluştur**'u seçin.

## <a name="configure-access-restrictions"></a>Erişim kısıtlamalarını yapılandırma

Bir sonraki adım, yalnızca sanal ağ üzerindeki kaynakların işlevi çağırabilmesini sağlamak için [erişim kısıtlamalarını](../app-service/app-service-ip-restrictions.md) yapılandırmaktır.

[Özel site](functions-networking-options.md#private-site-access) erişimi, işlev uygulaması ve belirtilen sanal ağ arasında bir Azure sanal ağ [hizmeti uç noktası](../virtual-network/virtual-network-service-endpoints-overview.md) oluşturularak etkinleştirilir. Erişim kısıtlamaları, hizmet uç noktaları aracılığıyla uygulanır. Hizmet uç noktaları, yalnızca belirtilen sanal ağ içinden gelen trafiğin belirlenen kaynağa erişebildiğinden emin olun. Bu durumda, belirlenen kaynak Azure Işlevidir.

1. İşlev uygulaması içinde, _Ayarlar_ bölüm üst bilgisi altında **ağ** bağlantısı ' nı seçin.
1. _Ağ_ sayfası, Azure ön kapısını, Azure CDN ve ayrıca erişim kısıtlamalarını yapılandırmak için başlangıç noktasıdır.
1. Özel site erişimini yapılandırmak için **erişim kısıtlamalarını Yapılandır** ' ı seçin.
1. _Erişim kısıtlamaları_ sayfasında, yalnızca varsayılan kısıtlamayı görürsünüz. Varsayılan değer, işlev uygulamasına erişim için herhangi bir kısıtlama yerleştirmez.  Özel bir site erişim kısıtlama yapılandırması oluşturmak için **Kural Ekle** ' yi seçin.
1. _Erişim kısıtlaması Ekle_ bölmesinde, yeni kural Için bir _ad_, _Öncelik_ve _Açıklama_ belirtin.
1. _Tür_ açılır kutusundan **sanal ağ** ' ı seçin, ardından önceden oluşturulmuş sanal ağı seçin ve ardından **öğretici** alt ağını seçin. 
    > [!NOTE]
    > Hizmet uç noktasının etkinleştirilmesi birkaç dakika sürebilir.
1. _Erişim kısıtlamaları_ sayfası artık yeni bir kısıtlama olduğunu gösterir. _Bitiş noktası durumunun_ , sağlama özelliği etkin olarak devre dışı olarak değiştirilmesi birkaç saniye sürebilir.

    >[!IMPORTANT]
    > Her işlev uygulaması, işlev uygulaması dağıtımlarını yönetmek için kullanılan bir [Gelişmiş araç (kudu) sitesine](../app-service/app-service-ip-restrictions.md#scm-site) sahiptir. Bu siteye şu şekilde bir URL 'den erişilir: `<FUNCTION_APP_NAME>.scm.azurewebsites.net` . Kudu sitesinde erişim kısıtlamalarını etkinleştirmek, proje kodunun yerel bir geliştirici iş istasyonundan dağıtılmasını önler ve sonra dağıtımı gerçekleştirmek için sanal ağ içinde bir aracı gerekir.

## <a name="access-the-functions-app"></a>İşlevler uygulamasına erişme

1. Önceden oluşturulmuş işlev uygulamasına geri dönün.  _Genel bakış_ bölümünde URL 'yi kopyalayın.

    >[!div class="mx-imgBorder"]
    >![Işlev uygulaması URL 'sini al](./media/functions-create-private-site-access/access-function-overview.png)

    Artık işlev uygulamasına sanal ağınızın dışındaki bilgisayarınızdan erişmeye çalışırsanız, erişimin yasaklanmış olduğunu belirten bir HTTP 403 sayfası alacaksınız.
1. Kaynak grubuna dönün ve önceden oluşturulmuş sanal makineyi seçin. Siteye sanal makineye erişmek için Azure savunma hizmeti aracılığıyla VM 'ye bağlanmanız gerekir.
1. **Bağlan** ' **ı seçin ve**ardından savunma ' yı seçin.
1. Sanal makinede oturum açmak için gereken kullanıcı adını ve parolayı belirtin.
1. **Bağlan**'ı seçin. Sanal makineyle etkileşime geçmesini sağlamak için yeni bir tarayıcı penceresi açılır.
VM, siteye sanal ağ üzerinden eriştiğinden, VM 'deki Web tarayıcısından siteye erişmek mümkündür.  Site yalnızca belirtilen sanal ağ içinden erişilemese de, ortak bir DNS girişi kalır.

## <a name="create-a-function"></a>İşlev oluşturma

Bu öğreticideki bir sonraki adım, HTTP ile tetiklenen bir Azure Işlevi oluşturmaktır. İşlevi bir HTTP GET veya POST aracılığıyla çağırmak, "Hello, {Name}" adlı bir Yanıt ile sonuçlanır.  

1. Azure Işlevleri uygulamanızı oluşturmak ve dağıtmak için aşağıdaki hızlı başlangıçlardan birini izleyin.

    * [Visual Studio Code](./functions-create-first-function-vs-code.md)
    * [Visual Studio](./functions-create-your-first-function-visual-studio.md)
    * [Komut satırı](./functions-create-first-azure-function-azure-cli.md)
    * [Maven (Java)](./functions-create-first-java-maven.md)

1. Azure Işlevleri projenizi yayımlarken, bu öğreticide daha önce oluşturduğunuz işlev uygulaması kaynağını seçin.
1. İşlevin dağıtıldığını doğrulayın.

    >[!div class="mx-imgBorder"]
    >![İşlev listesinde dağıtılan işlev](./media/functions-create-private-site-access/verify-deployed-function.png)

## <a name="invoke-the-function-directly"></a>İşlevi doğrudan çağır

1. İşleve erişimi test etmek için, işlev URL 'sini kopyalamanız gerekir. Dağıtılan işlevi seçin ve ardından **Işlev URL 'Sini al**' ı seçin. Ardından, URL 'YI panonuza kopyalamak için **Kopyala** düğmesine tıklayın.

    >[!div class="mx-imgBorder"]
    >![İşlev URL 'sini kopyalayın](./media/functions-create-private-site-access/get-function-url.png)

1. URL 'YI bir Web tarayıcısına yapıştırın. Artık işlev uygulamasına sanal ağınızın dışındaki bir bilgisayardan erişmeye çalıştığınızda, uygulamaya erişimin yasaklanmış olduğunu belirten bir HTTP 403 yanıtı alırsınız.

## <a name="invoke-the-function-from-the-virtual-network"></a>İşlevi sanal ağdan çağır

Sanal ağ üzerinde yapılandırılan VM 'deki bir Web tarayıcısı üzerinden işleve erişim (Azure savunma hizmeti kullanılarak) başarılı olur!

>[!div class="mx-imgBorder"]
>![Azure 'a savunma aracılığıyla Azure Işlevine erişin](./media/functions-create-private-site-access/access-function-via-bastion-final.png)

[!INCLUDE [clean-up-section-portal](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar


> [!div class="nextstepaction"]
> [Işlevlerde ağ seçenekleri hakkında daha fazla bilgi edinin](./functions-networking-options.md)
