---
title: Kendi sanal ağınızda Azure Databricks çalışma alanı oluşturma hızlı başlangıç
description: Bu makalede Azure Databricks sanal ağınıza nasıl dağıtabileceğiniz açıklanır.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.topic: conceptual
ms.date: 03/23/2020
ms.openlocfilehash: 63dd1b4d9396d340dd17a7afb92ff9c38a2b38b3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80132669"
---
# <a name="quickstart-create-an-azure-databricks-workspace-in-your-own-virtual-network"></a>Hızlı başlangıç: kendi sanal ağınızda Azure Databricks çalışma alanı oluşturma

Azure Databricks varsayılan dağıtımı, Databricks tarafından yönetilen yeni bir sanal ağ oluşturur. Bu hızlı başlangıçta kendi sanal ağınızda Azure Databricks çalışma alanı oluşturma gösterilmektedir. Ayrıca, bu çalışma alanı içinde bir Apache Spark kümesi oluşturursunuz. 

Kendi sanal ağınızda bir Azure Databricks çalışma alanı oluşturmayı neden seçebilirim hakkında daha fazla bilgi için bkz. [Azure sanal ağınızda (VNet ekleme) Azure Databricks dağıtma](https://docs.microsoft.com/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject).

Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/databricks/)oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

> [!Note]
> Bu öğretici **Azure Ücretsiz deneme aboneliği**kullanılarak gerçekleştirilemez.
> Ücretsiz hesabınız varsa, profilinize gidin ve aboneliğinizi **Kullandıkça Öde**ile değiştirin. Daha fazla bilgi için bkz. [Ücretsiz Azure hesabı](https://azure.microsoft.com/free/). Ardından, [harcama limitini kaldırın](https://docs.microsoft.com/azure/billing/billing-spending-limit#why-you-might-want-to-remove-the-spending-limit)ve bölgenizdeki vCPU 'lar için [bir kota artışı isteyin](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request) . Azure Databricks çalışma alanınızı oluşturduğunuzda, çalışma alanına 14 gün boyunca ücretsiz Premium Azure Databricks DBUs erişimi sağlamak için **deneme (Premium-14 gün ücretsiz DBUs)** fiyatlandırma katmanını seçebilirsiniz.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

1. Azure portal menüsünde **kaynak oluştur**' u seçin. Sonra **ağ > sanal ağ**' ı seçin.

    ![Azure portal üzerinde sanal ağ oluşturma](./media/quickstart-create-databricks-workspace-vnet-injection/create-virtual-network-portal.png)

2. **Sanal ağ oluştur**altında aşağıdaki ayarları uygulayın: 

    |Ayar|Önerilen değer|Açıklama|
    |-------|---------------|-----------|
    |Abonelik|\<Aboneliğiniz\>|Kullanmak istediğiniz Azure aboneliğini seçin.|
    |Kaynak grubu|databricks-hızlı başlangıç|**Yeni oluştur** ' u seçin ve hesabınız için yeni bir kaynak grubu adı girin.|
    |Adı|databricks-hızlı başlangıç|Sanal ağınız için bir ad seçin.|
    |Bölge|\<Kullanıcılarınıza en yakın bölgeyi seçin\>|Sanal ağınızı barındırabileceğiniz bir coğrafi konum seçin. Kullanıcılarınıza en yakın konumu kullanın.|

    ![Azure portal üzerindeki bir sanal ağın temelleri](./media/quickstart-create-databricks-workspace-vnet-injection/create-virtual-network.png)

3. **İleri ' yi seçin: IP adresleri >** ve aşağıdaki ayarları uygulayın. Ardından **gözden geçir + oluştur**' u seçin.
    
    |Ayar|Önerilen değer|Açıklama|
    |-------|---------------|-----------|
    |IPv4 adres alanı|10.2.0.0/16|CıDR gösteriminde sanal ağın adres aralığı. CıDR aralığı/16 ile/24 arasında olmalıdır|
    |Alt ağ adı|default|Sanal ağınızdaki varsayılan alt ağ için bir ad seçin.|
    |Alt Ağ Adresi aralığı|10.2.0.0/24|CIDR gösteriminde alt ağın adres aralığı. Sanal ağın adres alanı tarafından içerilmelidir. Kullanımda olan bir alt ağın adres aralığı düzenlenemiyor.|

    ![Azure portal bir sanal ağ için IP yapılandırması ayarla](./media/quickstart-create-databricks-workspace-vnet-injection/create-virtual-network-ip-config.png)

4. **Gözden geçir + oluştur** sekmesinde, sanal ağı dağıtmak için **Oluştur** ' u seçin. Dağıtım tamamlandıktan sonra sanal ağınıza gidin ve **Ayarlar**altında **Adres alanı** ' nı seçin. *Ek adres aralığı Ekle*' nin kutusunda, Ekle `10.179.0.0/16` ' yi ve ardından **Kaydet**' i seçin.

    ![Azure sanal ağ adres alanı](./media/quickstart-create-databricks-workspace-vnet-injection/add-address-space.png)

## <a name="create-an-azure-databricks-workspace"></a>Azure Databricks çalışma alanı oluşturma

1. Azure portal menüsünde **kaynak oluştur**' u seçin. Ardından **> Databricks Analytics**' i seçin.

    ![Azure portal Azure Databricks çalışma alanı oluşturma](./media/quickstart-create-databricks-workspace-vnet-injection/create-databricks-workspace-portal.png)

2. **Azure Databricks hizmeti**altında aşağıdaki ayarları uygulayın:

    |Ayar|Önerilen değer|Açıklama|
    |-------|---------------|-----------|
    |Çalışma alanı adı|databricks-hızlı başlangıç|Azure Databricks çalışma alanınız için bir ad seçin.|
    |Abonelik|\<Aboneliğiniz\>|Kullanmak istediğiniz Azure aboneliğini seçin.|
    |Kaynak grubu|databricks-hızlı başlangıç|Sanal ağ için kullandığınız kaynak grubunu seçin.|
    |Konum|\<Kullanıcılarınıza en yakın bölgeyi seçin\>|Sanal ağınızla aynı konumu seçin.|
    |Fiyatlandırma Katmanı|Standart veya Premium arasında seçim yapın.|Fiyatlandırma katmanları hakkında daha fazla bilgi için bkz. [Databricks fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/databricks/).|

    ![Azure Databricks çalışma alanı temelleri oluşturma](./media/quickstart-create-databricks-workspace-vnet-injection/create-databricks-workspace.png)

3. **Temel bilgiler** sayfasında ayarları girmeyi tamamladıktan sonra, ileri ' yi seçin **: ağ >** ve aşağıdaki ayarları uygula:

    |Ayar|Önerilen değer|Açıklama|
    |-------|---------------|-----------|
    |Sanal ağınızda Azure Databricks çalışma alanı dağıtma (VNet)|Yes|Bu ayar, sanal ağınıza bir Azure Databricks çalışma alanı dağıtmanıza olanak tanır.|
    |Sanal Ağ|databricks-hızlı başlangıç|Önceki bölümde oluşturduğunuz sanal ağı seçin.|
    |Ortak alt ağ adı|Genel-alt ağ|Varsayılan genel alt ağ adını kullanın.|
    |Ortak alt ağ CıDR aralığı|10.179.64.0/18|/26 dahil olmak üzere bir CıDR aralığı kullanın.|
    |Özel alt ağ adı|Özel-alt ağ|Varsayılan özel alt ağ adını kullanın.|
    |Özel alt ağ CıDR aralığı|10.179.0.0/18|/26 dahil olmak üzere bir CıDR aralığı kullanın.|

    ![VNet bilgilerini Azure portal Azure Databricks çalışma alanına ekleme](./media/quickstart-create-databricks-workspace-vnet-injection/create-databricks-workspace-vnet-config.png)

3. Dağıtım tamamlandıktan sonra Azure Databricks kaynağına gidin. Sanal ağ eşlemesinin devre dışı olduğuna dikkat edin. Ayrıca, Genel Bakış sayfasındaki kaynak grubu ve yönetilen kaynak grubu ' na de dikkat edin. 

    ![Azure portal Azure Databricks genel bakış](./media/quickstart-create-databricks-workspace-vnet-injection/databricks-overview-portal.png)

    Yönetilen kaynak grubu değiştirilebilir değildir ve sanal makineler oluşturmak için kullanılmaz. Yalnızca yönettiğiniz kaynak grubunda sanal makineler oluşturabilirsiniz.

    ![Azure Databricks yönetilen kaynak grubu](./media/quickstart-create-databricks-workspace-vnet-injection/managed-resource-group.png)

    Bir çalışma alanı dağıtımı başarısız olduğunda, çalışma alanı hala hatalı durumda oluşturulur. Başarısız çalışma alanını silin ve dağıtım hatalarını çözen yeni bir çalışma alanı oluşturun. Başarısız çalışma alanını sildiğinizde, yönetilen kaynak grubu ve başarıyla dağıtılan tüm kaynaklar da silinir.

## <a name="create-a-cluster"></a>Küme oluşturma

> [!NOTE]
> Azure Databricks kümesini oluşturmak için ücretsiz hesap oluşturmak istiyorsanız kümeyi oluşturmadan önce profilinize gidin ve aboneliğini **kullandıkça öde** modeline geçirin. Daha fazla bilgi için bkz. [Ücretsiz Azure hesabı](https://azure.microsoft.com/free/).

1. Azure Databricks hizmetinize dönün ve **genel bakış** sayfasında **çalışma alanını Başlat** ' ı seçin.

2. **Kümeler** > ve**küme oluştur**' u seçin. Ardından, *databricks-QuickStart-Cluster*gibi bir küme adı oluşturun ve kalan varsayılan ayarları kabul edin. **Küme oluştur**' u seçin.

    ![Azure Databricks kümesi oluşturma](./media/quickstart-create-databricks-workspace-vnet-injection/create-cluster.png)

3. Küme çalışmaya başladıktan sonra Azure portal yönetilen kaynak grubuna geri dönün. Yeni sanal makineler, diskler, IP adresi ve ağ arabirimlerine dikkat edin. IP adresleriyle ortak ve özel alt ağların her birinde bir ağ arabirimi oluşturulur.  

    ![Küme oluşturulduktan sonra yönetilen kaynak grubu Azure Databricks](./media/quickstart-create-databricks-workspace-vnet-injection/managed-resource-group2.png)

4. Azure Databricks çalışma alanınıza dönün ve oluşturduğunuz kümeyi seçin. Ardından **Spark Kullanıcı arabirimi** sayfasındaki **yürüticileri** sekmesine gidin. Sürücü ve yürüticilere ait adreslerin özel alt ağ aralığında olduğunu unutmayın. Bu örnekte, sürücü 10.179.0.6 ve yürüticileri 10.179.0.4 ve 10.179.0.5 'dir. IP adresleriniz farklı olabilir.

    ![Azure Databricks Spark UI yürüticileri](./media/quickstart-create-databricks-workspace-vnet-injection/databricks-sparkui-executors.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Makaleyi tamamladıktan sonra kümeyi sonlandırabilirsiniz. Bunu yapmak için Azure Databricks çalışma alanında sol bölmedeki **Kümeler**’i seçin. Sonlandırmak istediğiniz küme için imleci **Eylemler** sütunu altındaki üç noktanın üzerine taşıyın ve **Sonlandır** simgesini seçin. Bu, kümeyi sonlandırır.

Küme oluşturma sırasında **işlem yapılmadan dakika \_ \_ sonra Sonlandır** onay kutusunu seçtiğiniz takdirde otomatik olarak durdurulur. Böyle bir durumda, belirtilen süre boyunca etkin olmaması durumunda küme otomatik olarak durdurulur.

Kümeyi yeniden kullanmak istemiyorsanız, Azure portal oluşturduğunuz kaynak grubunu silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir sanal ağa dağıttığınız Azure Databricks bir Spark kümesi oluşturdunuz. Bir Azure Databricks Not defterinden JDBC kullanarak sanal ağda SQL Server Linux Docker kapsayıcısını sorgulama hakkında bilgi edinmek için sonraki makaleye ilerleyin.  

> [!div class="nextstepaction"]
>[Azure Databricks not defterinden sanal ağdaki SQL Server Linux Docker kapsayıcısını sorgulama](vnet-injection-sql-server.md)
