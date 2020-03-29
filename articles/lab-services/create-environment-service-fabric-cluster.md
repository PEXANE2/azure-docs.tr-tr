---
title: Azure DevTest Labs'da Hizmet Dokusu küme ortamı oluşturma
description: Bağımsız hizmet kumaş ı kümesiyle bir ortam oluşturmayı ve zamanlamaları kullanarak kümeyi nasıl başlatıp durdurarak durdurun.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: EMaher
manager: femila
editor: spelluru
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2020
ms.author: enewman
ms.openlocfilehash: 71793b81d8735c80881fc25a9b7ec31bc4fc6762
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76170332"
---
# <a name="create-an-environment-with-self-contained-service-fabric-cluster-in-azure-devtest-labs"></a>Azure DevTest Labs'da bağımsız Hizmet Kumaşı kümesiyle bir ortam oluşturun
Bu makalede, Azure DevTest Labs'da bağımsız hizmet kumaş ı kümesi bulunan bir ortamın nasıl oluşturulacaaçık olduğu hakkında bilgi verilmektedir. 

## <a name="overview"></a>Genel Bakış
DevTest Labs, Azure Kaynak Yönetimi şablonları tarafından tanımlandığı şekilde bağımsız test ortamları oluşturabilir. Bu ortamlar, sanal makineler gibi hem IaaS kaynaklarını hem de Service Fabric gibi PaaS kaynaklarını içerir. DevTest Labs, sanal makineleri kontrol etmek için komutlar sağlayarak bir ortamda sanal makineleri yönetmenize olanak tanır. Bu komutlar, sanal bir makineyi zamanlamada başlatma veya durdurma olanağı verir. Benzer şekilde, DevTest Labs da bir ortamda Servis Kumaş kümeleri yönetmenize yardımcı olabilir. Bir Hizmet Kumaşı kümesini el ile veya zamanlama yoluyla bir ortamda başlatabilir veya durdurabilirsiniz.

## <a name="create-a-service-fabric-cluster"></a>Service Fabric kümesi oluşturma
Service Fabric kümeleri DevTest Labs ortamları kullanılarak oluşturulur. Her ortam, Git deposundaki bir Azure Kaynak Yöneticisi şablonu tarafından tanımlanır. DevTest Labs için [ortak Git deposu,](https://github.com/Azure/azure-devtestlab/tree/master/Environments/) [ServiceFabric-Cluster](https://github.com/Azure/azure-devtestlab/tree/master/Environments/ServiceFabric-LabCluster) klasöründe bir Hizmet Dokusu kümesi oluşturmak için Kaynak Yöneticisi şablonu içerir. 

1. İlk olarak, aşağıdaki makaledeki yönergeleri kullanarak Azure DevTest Labs'da bir laboratuvar oluşturun: [Bir laboratuvar oluşturun.](devtest-lab-create-lab.md) **Genel ortamlar** seçeneğinin varsayılan olarak **Açık** olduğuna dikkat edin. 
2. Service Fabric sağlayıcısının aşağıdaki adımları izleyerek aboneliğiniz için kayıtlı olduğunu doğrulayın:
    1. Sol navigasyon menüsünden **Abonelikleri** seçin ve **Aboneliğinizi** seçin
    2. **Abonelik** sayfasında, sol menüdeki **Ayarlar** **bölümündekaynak sağlayıcılarını** seçin. 
    3. **Microsoft.ServiecFabric** kayıtlı **değilse, Kaydol'u**seçin. 
3. Laboratuvarınızın **DevTest Lab** sayfasında araç çubuğuna **+ Ekle'yi** seçin. 
    
    ![Araç çubuğuna düğme ekleme](./media/create-environment-service-fabric-cluster/add-button.png)
3. Bir temel sayfa **seçin,** listede **Service Fabric Lab Cluster'ı** seçin. 

    ![Listede Servis Kumaş ı Lab Cluster'ı seçin](./media/create-environment-service-fabric-cluster/select-service-fabric-cluster.png)
4. Yapı **ayarları** sayfasında aşağıdaki adımları yapın: 
    1. Küme **ortamınız**için bir **ad** belirtin. Bu, Hizmet Kumaşı kümesinin oluşturulacağının Azure'daki kaynak grubunun adıdır. 
    2. Küme sanal makineleri için **işletim sistemini (OS)** seçin. Varsayılan değer: **Windows**.
    3. Küme için **yönetici** için bir ad belirtin. 
    4. Yönetici için bir **parola** belirtin. 
    5. **Sertifika**için, sertifika bilgilerinizi Base64 kodlanmış dize olarak girin. Sertifika oluşturmak için aşağıdaki adımları yapın:
        1. [Git deposundan](https://github.com/Azure/azure-devtestlab/tree/master/Environments/ServiceFabric-LabCluster) **Create-ClusterCertificate.ps1** dosyasını indirin. Alternatif olarak, makinenizdeki depoyu klonla. 
        2. **PowerShell**’i başlatın. 
        3. **ps1** dosyasını komutu `.\Create-ClusterCertificate.ps1`kullanarak çalıştırın. Not defterinde, bu sayfadaki sertifikayla ilgili alanları doldurmanız gereken bilgileri içeren bir metin dosyasının açıldığını görürsünüz. . 
    6. **Sertifikanın parolasını**girin.
    7. Sertifikanızın **parmak izini** belirtin.
    8. **Yapılandırılan Ayarlar** sayfasında **Ekle'yi** seçin. 

        ![Küme ayarlarını yapılandırma](./media/create-environment-service-fabric-cluster/configure-settings.png)
5. Küme oluşturulduktan sonra, önceki adımda sağladığınız ortamın adını içeren bir kaynak grubu görürsünüz. Genişlediğinizde, içinde Service Fabric kümesini görürsünüz. Kaynak grubunun durumu **Oluşturma'da**sıkışmışsa, araç çubuğunda **Yenile'yi** seçin. **Service Fabric küme** ortamı, Linux veya Windows'da 5 düğümlü 1 düğümlü bir küme oluşturur.

    Aşağıdaki örnekte, **mysfabricclusterrg,** Service Fabric kümesi için özel olarak oluşturulan kaynak grubunun adıdır. Laboratuvar ortamlarının oluşturuldukları kaynak grubunda kendi kendine yeten bir ortam olduğunu unutmayın. Bu, yalnızca yeni oluşturulan kaynak grubundaki kaynaklara veya laboratuvar tarafından [kullanılmak üzere yapılandırılan sanal ağlardaki](devtest-lab-configure-vnet.md)kaynaklara erişebilen ortamı tanımlayan şablon anlamına gelir. Yukarıdaki örnek, aynı kaynak grubunda gerekli tüm kaynakları oluşturur.

    ![Küme oluşturuldu](./media/create-environment-service-fabric-cluster/cluster-created.png)

## <a name="start-or-stop-the-cluster"></a>Kümeyi başlatma veya durdurma
Kümeyi DevTest Lab sayfasının kendisinden veya DevTest Labs tarafından sağlanan Service Fabric Cluster sayfasından başlatabilir veya durdurabilirsiniz. 

### <a name="from-the-devtest-lab-page"></a>DevTest Lab sayfasından
Laboratuvarınız için DevTest Lab sayfasında kümeyi başlatabilir veya durdurabilirsiniz. 

1. Service Fabric kümesi için aşağıdaki resimde gösterildiği gibi **üç nokta (...)** seçin: 

    ![Küme için başlatma ve durdurma komutları](./media/create-environment-service-fabric-cluster/start-stop-on-devtest-lab-page.png)

2. Kümeyi **başlatmak** ve **durdurmak** için bağlam menüsünde iki komut görürsünüz. Başlangıç komutu kümedeki tüm düğümleri başlatır. Durdurma komutu kümedeki tüm düğümleri durdurur. Bir küme durdurulduktan sonra, Hizmet Kumaşı kümesinin kendisi hazır durumda kalır, ancak başlat komutu laboratuardaki küme üzerinde yeniden verilene kadar düğümler kullanılabilir.

    Bir test ortamında Hizmet Kumaşı kümesi kullanırken dikkat edilmesi gereken birkaç nokta vardır. Düğümler yeniden başlatıldıktan sonra Service Fabric kümesinin tamamen yeniden sudalması biraz zaman alabilir. Verileri kapatmadan başlangıç olarak tutmak için, verilerin sanal makineye bağlı yönetilen bir diske kaydedilmesi gerekir. Ekli yönetilen bir disk kullanırken performans etkileri vardır, bu nedenle yalnızca test ortamları için önerilir. Veri depolama için kullanılan disk geri verilmezse, kümeüzerinde durdurma komutu verildiğinde veriler kaybolur.

### <a name="from-the-service-fabric-cluster-page"></a>Service Fabric Cluster sayfasından 
Kümeyi başlatmanın veya durdurmanın başka bir yolu daha vardır. 

1. DevTest Lab sayfasındaki ağaç görünümünde Hizmet Kumaşı kümenizi seçin. 

    ![Kümenizi seçin](./media/create-environment-service-fabric-cluster/select-cluster.png)

2. Kümenin **Hizmet Kumaş Kümesi** sayfasında, kümeyi başlatmak veya durdurmak için araç çubuğunda komutlar görürsünüz. 

    ![Hizmet Kumaş Kümesi sayfasındaki komutları başlatma veya durdurma](./media/create-environment-service-fabric-cluster/start-stop-on-cluster-page.png)

## <a name="configure-auto-startup-and-auto-shutdown-schedule"></a>Otomatik başlatma ve otomatik kapatma zamanlamasını yapılandırma
Hizmet Kumaş kümeleri de başlatılabilir veya bir zamanlama üzerinde durdurulabilir. Bu deneyim, bir laboratuvardaki sanal makineler deneyimine benzer. Paradan tasarruf etmek için varsayılan olarak, laboratuvarda oluşturulan her küme, laboratuvar [kapatma ilkesi](devtest-lab-set-lab-policy.md?#set-auto-shutdown-policy)tarafından tanımlanan anda otomatik olarak kapanır. Kümenin kapatılması gerekip gerekmediğini belirterek veya kümenin kapatılma saatini belirterek geçersiz kılınabilirsiniz. 

![Otomatik başlatma ve otomatik kapatma için varolan zamanlamalar](./media/create-environment-service-fabric-cluster/existing-schedules.png)

### <a name="opt-in-to-the-auto-start-schedule"></a>Otomatik başlatma zamanlamasına katılma
Başlangıç zamanlamasını tercih etmek için aşağıdaki adımları yapın:

1. Sol menüde **Otomatik başlat'ı** seçin
2. **Bu hizmet kumaş kümesinin otomatik başlatma için zamanlanmasına izin ver'i**seçin. **On** Bu sayfa yalnızca laboratuvar sahibi kullanıcıların sanal makinelerini veya Service Fabric kümelerini otomatik olarak başlatmalarına izin verdiyse etkinleştirilir.
3. Araç çubuğunda **Kaydet**’i seçin. 

    ![Otomatik yıldız sayfası](./media/create-environment-service-fabric-cluster/set-auto-start-settings.png)

### <a name="configure-auto-shutdown-settings"></a>Otomatik kapatma ayarlarını yapılandırma 
Kapatma ayarlarını değiştirmek için aşağıdaki adımları yapın:

1. Sol menüde **Otomatik kapatma'yı** seçin. 
2. Bu sayfada, **Etkin**için **Kapalı'yı** seçerek otomatik kapatmayı devre dışı bırakabilirsiniz. 
3. **Etkin**için **On'u** seçtiyseniz aşağıdaki adımları izleyin:
    1. Kapatma **saatini** belirtin.
    2. Saatin **saat dilimini** belirtin. 
    3. DevTest Labs'ın otomatik kapanmadan önce **bildirim** göndermesini isteyip istemediğinizi belirtin. 
    4. Bildirim seçeneği için **Evet'i** seçtiyseniz, bildirim göndermek için **Webhook URL'sini** ve/veya **e-posta adresini** belirtin. 
    5. Araç çubuğunda **Kaydet**’i seçin.

        ![Otomatik kapatma sayfası](./media/create-environment-service-fabric-cluster/auto-shutdown-settings.png)

## <a name="to-view-nodes-in-the-service-fabric-cluster"></a>Hizmet Kumaşı kümesindeki düğümleri görüntülemek için
Daha önceki adımlarda gördüğünüz Hizmet Kumaşı küme sayfası DevTest Labs sayfasına özgüdür. Kümedeki düğümleri göstermez. Küme hakkında daha fazla bilgi görmek için aşağıdaki adımları izleyin:

1. Laboratuvarınızın **DevTest Laboratuvarı** sayfasında, **sanal makinelerim** bölümündeki ağaç görünümündeki **kaynak grubunu** seçin.

    ![Kaynak grubu seçin](./media/create-environment-service-fabric-cluster/select-resource-group.png)
2. Kaynak **Grubu** sayfasında, kaynak grubundaki tüm kaynakları bir listede görürsünüz. Listeden **Hizmet Kumaşı kümenizi** seçin. 

    ![Listede kümenizi seçin](./media/create-environment-service-fabric-cluster/select-cluster-resource-group-page.png)
3. Kümeniz için **Hizmet Kumaş Kümesi** sayfasını görürsünüz. Bu, Hizmet Kumaşı'nın sağladığı sayfadır. Düğümler, düğüm türleri, vb. gibi kümeler hakkındaki tüm bilgileri görürsünüz.

    ![Servis Kumaş Kümesi giriş sayfası](./media/create-environment-service-fabric-cluster/service-fabric-cluster-page.png)

## <a name="next-steps"></a>Sonraki adımlar
Ortamlar hakkında ayrıntılar için aşağıdaki makalelere bakın: 

- [Azure Resource Manager şablonları ile çoklu VM ortamları ve PaaS kaynakları oluşturma](devtest-lab-create-environment-from-arm.md)
- [Azure DevTest Labs'da ortak ortamları yapılandırma ve kullanma](devtest-lab-configure-use-public-environments.md)
