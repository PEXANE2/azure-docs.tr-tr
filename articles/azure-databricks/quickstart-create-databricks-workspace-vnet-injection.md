---
title: Kendi Sanal Ağınızda Azure Veri Tuğlaları çalışma alanı oluşturma
description: Bu makalede, Azure Veri Tuğlaları sanal ağınıza nasıl dağıtılanın caizdir.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.topic: conceptual
ms.date: 03/23/2020
ms.openlocfilehash: 63dd1b4d9396d340dd17a7afb92ff9c38a2b38b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80132669"
---
# <a name="quickstart-create-an-azure-databricks-workspace-in-your-own-virtual-network"></a>Hızlı başlatma: Kendi Sanal Ağınızda bir Azure Databricks çalışma alanı oluşturun

Azure Databricks'in varsayılan dağıtımı, Databricks tarafından yönetilen yeni bir sanal ağ oluşturur. Bu hızlı başlangıç, kendi sanal ağınızda azure veri tuğlaları çalışma alanı oluşturmanın nasıl olduğunu gösterir. Ayrıca, bu çalışma alanı içinde bir Apache Spark kümesi oluşturursunuz. 

Kendi sanal ağınızda neden bir Azure Databricks çalışma alanı oluşturmayı seçebileceğiniz hakkında daha fazla bilgi için Azure [Sanal Ağınızda (VNet Enjeksiyonu) Azure Veri Tuğlaları Dağıt'a](https://docs.microsoft.com/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject)bakın.

Azure aboneliğiniz yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/databricks/)oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/)oturum açın.

> [!Note]
> Bu öğretici **Azure Ücretsiz Deneme Aboneliği**kullanılarak gerçekleştirilemez.
> Ücretsiz bir hesabınız varsa, profilinize gidin ve aboneliğinizi istediğiniz **kadar öde**olarak değiştirin. Daha fazla bilgi için bkz. [Ücretsiz Azure hesabı](https://azure.microsoft.com/free/). Ardından, [harcama sınırını kaldırın](https://docs.microsoft.com/azure/billing/billing-spending-limit#why-you-might-want-to-remove-the-spending-limit)ve bölgenizdeki VCPU'lar için kota artışı [isteyin.](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request) Azure Databricks çalışma alanınızı oluşturduğunuzda, çalışma alanına ücretsiz Premium Azure Databricks DBUs'a 14 gün süreyle erişim sağlamak için **Deneme (Premium - 14 Gün Ücretsiz DBUs)** fiyatlandırma katmanını seçebilirsiniz.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

1. Azure portalı menüsünden **kaynak oluştur'u**seçin. Ardından **Ağ > Sanal ağı**seçin.

    ![Azure portalında sanal ağ oluşturma](./media/quickstart-create-databricks-workspace-vnet-injection/create-virtual-network-portal.png)

2. **Sanal ağ oluştur**altında aşağıdaki ayarları uygulayın: 

    |Ayar|Önerilen değer|Açıklama|
    |-------|---------------|-----------|
    |Abonelik|\<Aboneliğiniz\>|Kullanmak istediğiniz Azure aboneliğini seçin.|
    |Kaynak grubu|databricks-quickstart|**Yeni Oluştur'u** seçin ve hesabınız için yeni bir kaynak grubu adı girin.|
    |Adı|databricks-quickstart|Sanal ağınız için bir ad seçin.|
    |Bölge|\<Kullanıcılarınıza en yakın bölgeyi seçin\>|Sanal ağınızı barındırabileceğiniz bir coğrafi konum seçin. Kullanıcılarınıza en yakın konumu kullanın.|

    ![Azure portalında sanal ağ için temel bilgiler](./media/quickstart-create-databricks-workspace-vnet-injection/create-virtual-network.png)

3. **Sonraki'ni seçin: IP Adresleri >** ve aşağıdaki ayarları uygulayın. Sonra **Gözden Geçir + oluştur'u**seçin.
    
    |Ayar|Önerilen değer|Açıklama|
    |-------|---------------|-----------|
    |IPv4 adres alanı|10.2.0.0/16|Sanal ağın ADRES aralığı CIDR gösteriminde. CIDR aralığı /16 ve /24 arasında olmalıdır|
    |Alt ağ adı|default|Sanal ağınızdaki varsayılan alt ağ için bir ad seçin.|
    |Alt Ağ Adresi aralığı|10.2.0.0/24|CIDR gösteriminde alt ağın adres aralığı. Sanal ağın adres alanı tarafından içermelidir. Kullanılan bir alt netin adres aralığı düzenlenemez.|

    ![Azure portalında sanal ağ için IP yapılandırmaları ayarlama](./media/quickstart-create-databricks-workspace-vnet-injection/create-virtual-network-ip-config.png)

4. Gözden **Geçir + oluştur** sekmesinde, sanal ağı dağıtmak için **Oluştur'u** seçin. Dağıtım tamamlandıktan sonra sanal ağınıza gidin ve **Ayarlar**altında **Adres alanını** seçin. *Ek adres aralığı ekle*yazan kutuya `10.179.0.0/16` ekle ve **Kaydet'i**seçin.

    ![Azure sanal ağ adres alanı](./media/quickstart-create-databricks-workspace-vnet-injection/add-address-space.png)

## <a name="create-an-azure-databricks-workspace"></a>Azure Databricks çalışma alanı oluşturma

1. Azure portalı menüsünden **kaynak oluştur'u**seçin. Ardından **Analytics > Databricks'i**seçin.

    ![Azure portalında Azure Databricks çalışma alanı oluşturma](./media/quickstart-create-databricks-workspace-vnet-injection/create-databricks-workspace-portal.png)

2. **Azure Databricks Hizmeti**altında aşağıdaki ayarları uygulayın:

    |Ayar|Önerilen değer|Açıklama|
    |-------|---------------|-----------|
    |Çalışma alanı adı|databricks-quickstart|Azure Databricks çalışma alanınız için bir ad seçin.|
    |Abonelik|\<Aboneliğiniz\>|Kullanmak istediğiniz Azure aboneliğini seçin.|
    |Kaynak grubu|databricks-quickstart|Sanal ağ için kullandığınız kaynak grubunu seçin.|
    |Konum|\<Kullanıcılarınıza en yakın bölgeyi seçin\>|Sanal ağınızla aynı konumu seçin.|
    |Fiyatlandırma Katmanı|Standart veya Premium arasında seçim yapın.|Fiyatlandırma katmanları hakkında daha fazla bilgi için [Databricks fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/databricks/)bakın.|

    ![Azure Databricks çalışma alanı temelleri oluşturma](./media/quickstart-create-databricks-workspace-vnet-injection/create-databricks-workspace.png)

3. **Temel Ayarlar** sayfasında ayarları girmeyi bitirdikten **sonra, Sonraki: Ağ >'ni** seçin ve aşağıdaki ayarları uygulayın:

    |Ayar|Önerilen değer|Açıklama|
    |-------|---------------|-----------|
    |Sanal Ağınızda Azure Databricks çalışma alanını dağıtma (VNet)|Evet|Bu ayar, sanal ağınızda bir Azure Databricks çalışma alanı dağıtmanıza olanak tanır.|
    |Sanal Ağ|databricks-quickstart|Önceki bölümde oluşturduğunuz sanal ağı seçin.|
    |Genel Alt Ağ Adı|genel alt ağ|Varsayılan ortak alt ağ adını kullanın.|
    |Kamu Subnet CIDR Serisi|10.179.64.0/18|/26'ya kadar ve dahil olmak üzere bir CIDR aralığı kullanın.|
    |Özel Subnet Adı|özel-alt ağ|Varsayılan özel alt ağ adını kullanın.|
    |Özel Subnet CIDR Serisi|10.179.0.0/18|/26'ya kadar ve dahil olmak üzere bir CIDR aralığı kullanın.|

    ![Azure portalındaki Azure Databricks çalışma alanına VNet bilgileri ekleme](./media/quickstart-create-databricks-workspace-vnet-injection/create-databricks-workspace-vnet-config.png)

3. Dağıtım tamamlandıktan sonra Azure Veri Tuğlaları kaynağına gidin. Sanal ağ eşlemenin devre dışı bırakıldığına dikkat edin. Ayrıca, genel bakış sayfasındaki kaynak grubuna ve yönetilen kaynak grubuna da dikkat edin. 

    ![Azure Veri Tuğlaları'na Azure portalına genel bakış](./media/quickstart-create-databricks-workspace-vnet-injection/databricks-overview-portal.png)

    Yönetilen kaynak grubu değiştirilebilir değildir ve sanal makineler oluşturmak için kullanılmaz. Yalnızca yönettiğiniz kaynak grubunda sanal makineler oluşturabilirsiniz.

    ![Azure Databricks yönetilen kaynak grubu](./media/quickstart-create-databricks-workspace-vnet-injection/managed-resource-group.png)

    Çalışma alanı dağıtımı başarısız olduğunda, çalışma alanı yine de başarısız durumda oluşturulur. Başarısız çalışma alanını silin ve dağıtım hatalarını çözen yeni bir çalışma alanı oluşturun. Başarısız çalışma alanını sildiğinizde, yönetilen kaynak grubu ve başarıyla dağıtılan kaynaklar da silinir.

## <a name="create-a-cluster"></a>Küme oluşturma

> [!NOTE]
> Azure Databricks kümesini oluşturmak için ücretsiz hesap oluşturmak istiyorsanız kümeyi oluşturmadan önce profilinize gidin ve aboneliğini **kullandıkça öde** modeline geçirin. Daha fazla bilgi için bkz. [Ücretsiz Azure hesabı](https://azure.microsoft.com/free/).

1. Azure Databricks hizmetinize dönün ve Genel **Bakış** sayfasında **Çalışma Alanını Başlat'ı** seçin.

2. **Kümeler** > **+ Küme Oluştur'u**seçin. Ardından *databricks-quickstart-cluster*gibi bir küme adı oluşturun ve kalan varsayılan ayarları kabul edin. **Küme Oluştur'u**seçin.

    ![Azure Veri Tuğlaları kümesi oluşturma](./media/quickstart-create-databricks-workspace-vnet-injection/create-cluster.png)

3. Küme çalışmaya başladığında, Azure portalındaki yönetilen kaynak grubuna dönün. Yeni sanal makinelere, disklere, IP Adresine ve ağ arabirimlerine dikkat edin. IP adreslerine sahip ortak ve özel alt ağların her birinde bir ağ arabirimi oluşturulur.  

    ![Azure Databricks küme oluşturma dan sonra kaynak grubunu yönetti](./media/quickstart-create-databricks-workspace-vnet-injection/managed-resource-group2.png)

4. Azure Databricks çalışma alanınıza dönün ve oluşturduğunuz kümeyi seçin. Ardından, **Spark UI** sayfasındaki **Yürütücüler** sekmesine gidin. Sürücü ve uygulayıcıların adreslerinin özel alt ağ aralığında olduğuna dikkat edin. Bu örnekte, sürücü 10.179.0.6 ve uygulayıcıları 10.179.0.4 ve 10.179.0.5 vardır. IP adresleriniz farklı olabilir.

    ![Azure Databricks Spark UI uygulayıcıları](./media/quickstart-create-databricks-workspace-vnet-injection/databricks-sparkui-executors.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Makaleyi tamamladıktan sonra kümeyi sonlandırabilirsiniz. Bunu yapmak için Azure Databricks çalışma alanında sol bölmedeki **Kümeler**’i seçin. Sonlandırmak istediğiniz küme için imleci **Eylemler** sütunu altındaki üç noktanın üzerine taşıyın ve **Sonlandır** simgesini seçin. Bu kümedurur.

Kümeyi oluştururken **etkinlik dakikalarından \_ \_ sonra Sonlandırma** onay kutusunu seçmeniz koşuluyla, kümeyi el ile sonlandırmazsanız otomatik olarak durur. Böyle bir durumda, belirtilen süre boyunca etkin olmaması durumunda küme otomatik olarak durdurulur.

Kümeyi yeniden kullanmak istemiyorsanız, Azure portalında oluşturduğunuz kaynak grubunu silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, sanal ağa dağıttığınız Azure Veri Tuğlaları'nda bir Kıvılcım kümesi oluşturdunuz. Azure Databricks dizüstü bilgisayarından JDBC kullanarak sanal ağdaki bir SQL Server Linux Docker kapsayıcısını nasıl sorgulayabilirsiniz öğrenmek için bir sonraki makaleye ilerleyin.  

> [!div class="nextstepaction"]
>[Azure Databricks not defterinden sanal ağdaki SQL Server Linux Docker kapsayıcısını sorgulama](vnet-injection-sql-server.md)
