---
title: Web hizmeti parametreleri - Azure Machine Learning Studio (klasik) | Microsoft Dokümanlar
description: Web hizmetine erişildiğinde modelinizin davranışını değiştirmek için Azure Machine Learning Web Servis Parametrelerini nasıl kullanılır?
services: machine-learning
author: xiaoharper
ms.custom: seodec18
ms.author: amlstudiodocs
editor: cgronlun
ms.assetid: c49187db-b976-4731-89d6-11a0bf653db1
ms.service: machine-learning
ms.subservice: studio
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/12/2017
ms.openlocfilehash: d6ddd9603f22bd3820d18be020b9c620cf06aa42
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79204418"
---
# <a name="use-azure-machine-learning-studio-classic-web-service-parameters"></a>Azure Machine Learning Studio (klasik) web hizmeti parametrelerini kullanma

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Azure Machine Learning web hizmeti, yapılandırılabilir parametrelere sahip modüller içeren bir deneme yayımlayarak oluşturulur. Bazı durumlarda, web hizmeti çalışırken modül davranışını değiştirmek isteyebilirsiniz. *Web Hizmeti Parametreleri* bu görevi yapmanıza olanak sağlar. 

Yaygın bir örnek, yayımlanan web hizmetinin kullanıcısının web hizmetine erişildiğinde farklı bir veri kaynağı belirtebilmeleri için [Veri İçe Aktarma][reader] modülünün ayarlanmasıdır. Veya farklı bir hedef belirtilen [dışa][writer] aktarma verileri modülasyon. Diğer bazı örnekler arasında [Özellik Karma][feature-hashing] modülü için bit sayısının veya Filtre [Tabanlı Özellik Seçimi][filter-based-feature-selection] modülü için istenen özellik sayısının değiştirilmesi sayılabilir. 

Web Hizmeti Parametrelerini ayarlayabilir ve bunları denemenizde bir veya daha fazla modül parametresiyle ilişkilendirebilir ve bunların gerekli mi yoksa isteğe bağlı mı olduğunu belirtebilirsiniz. Web hizmetinin kullanıcısı, web hizmetini aradıkları zaman bu parametreler için değerler sağlayabilir. 



## <a name="how-to-set-and-use-web-service-parameters"></a>Web Hizmeti Parametreleri nasıl ayarlanır ve kullanılır?
Bir modül için parametrenin yanındaki simgeyi tıklatarak ve "Web hizmeti parametresi olarak ayarla"yı seçerek bir Web Hizmet Parametresi tanımlarsınız. Bu, yeni bir Web Hizmeti Parametresi oluşturur ve bu modül parametresine bağlanır. Daha sonra, web hizmetine erişildiğinde, kullanıcı Web Hizmeti Parametresi için bir değer belirtebilir ve modül parametresine uygulanır.

Bir Web Hizmeti Parametresi tanımladıktan sonra, denemedeki diğer modül parametreleri tarafından kullanılabilir. Bir modül için bir parametreyle ilişkili bir Web Hizmet Parametresi tanımlarsanız, parametre aynı değer türünü beklediği sürece, aynı Web Hizmet Parametresini başka bir modül için kullanabilirsiniz. Örneğin, Web Hizmeti Parametresi sayısal bir değerse, yalnızca sayısal değer bekleyen modül parametreleri için kullanılabilir. Kullanıcı Web Hizmeti Parametresi için bir değer belirlediğinde, ilişkili tüm modül parametrelerine uygulanır.

Web Hizmeti Parametresi için varsayılan değer sağlayıp sağlamamaya karar verebilirsiniz. Bunu yaparsanız, parametre web hizmetinin kullanıcı için isteğe bağlıdır. Varsayılan bir değer sağlamazsanız, web hizmetine erişildiğinde kullanıcının bir değer girmesi gerekir.

Web hizmetiiçin API dokümantasyonu, web hizmetine erişirken Web Hizmeti Parametresinin programlı olarak nasıl belirtilen web hizmeti kullanıcısına ilişkin bilgileri içerir.

> [!NOTE]
> Klasik bir web hizmeti için API belgeleri Machine Learning Studio (klasik) web hizmeti **PANOSU** **API yardım sayfası** bağlantısı üzerinden sağlanır. Yeni bir web hizmetiiçin API belgeleri, web hizmetiniz için **Tüketim** ve **Swagger API** sayfalarındaki [Azure Machine Learning Web Services](https://services.azureml.net/Quickstart) portalı aracılığıyla sağlanır.
> 
> 

## <a name="example"></a>Örnek
Örnek olarak, Azure blob depolamasına bilgi gönderen bir [Dışa Aktarma Verimodülü][writer] yle ilgili bir denememiz olduğunu varsayalım. Hizmete erişildiğinde web hizmeti kullanıcısının blob depolama alanına giden yolu değiştirmesine olanak tanıyan "Blob yolu" adlı bir Web Hizmeti Parametresi tanımlarız.

1. Machine Learning Studio'da (klasik), seçmek için [Veri İhracı][writer] modülünü tıklatın. Özellikleri, deneme tuvalinin sağındaki Özellikler bölmesinde gösterilir.
2. Depolama türünü belirtin:
   
   * **Lütfen veri hedefini belirtin**, "Azure Blob Depolama" seçeneğini belirleyin.
   * **Lütfen kimlik doğrulama türünü belirtiniz**altında "Hesap" seçeneğini belirleyin.
   * Azure blob depolama alanının hesap bilgilerini girin. 

3. **Kapsayıcı parametresi ile başlayan blob için Yol'un**sağındaki simgeyi tıklatın. Şu şekilde görünür:
   
   ![Web Hizmeti Parametre simgesi](./media/web-service-parameters/icon.png)
   
   "Web hizmeti parametresi olarak ayarla"yı seçin.
   
   Özellikler bölmesinin altındaki **Web Hizmet Parametreleri'nin** altına "Kapsayıcıyla başlayan blob yolu" adı ile bir giriş eklenir. Bu, artık bu [Dışa Aktarma Verileri][writer] modülü parametresi ile ilişkili Web Hizmet Parametresi'dir.
4. Web Hizmeti Parametresini yeniden adlandırmak için adı tıklatın, "Blob yolunu" girin ve **Enter** tuşuna basın. 
5. Web Hizmeti Parametresi için varsayılan değer sağlamak için, adın sağındaki simgeyi tıklatın, "Varsayılan değer sağlayın"ı seçin, bir değer girin (örneğin, "container1/output1.csv") ve **Enter** tuşuna basın.
   
   ![Web Servis Parametresi](./media/web-service-parameters/parameter.png)
6. **Çalıştır**’a tıklayın. 
7. **Web Hizmetini Dağıt'ı** tıklatın ve Web Hizmetini dağıtmak için **Web Hizmeti [Klasik] dağıt'ı** veya Web Hizmetini **[Yeni] dağıt'ı** seçin.

> [!NOTE] 
> Yeni bir web hizmetini dağıtmak için, web hizmetini dağıttığınız abonelikte yeterli izinlere sahip olmalısınız. Daha fazla bilgi için Azure [Machine Learning Web Services portalını kullanarak bir Web hizmetini yönetin.](manage-new-webservice.md) 

Web hizmetinin kullanıcısı artık web hizmetine erişirken [Dışa Aktarma Verileri][writer] modülü için yeni bir hedef belirleyebilir.

## <a name="more-information"></a>Daha fazla bilgi
Daha ayrıntılı bir örnek için, [Machine Learning blogundaki](https://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx)Web [Hizmeti Parametreleri](https://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) girişine bakın.

Machine Learning web hizmetine erişim hakkında daha fazla bilgi için Azure [Machine Learning Web hizmetini nasıl tüketir'](consume-web-services.md)e bakın.

<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[reader]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[writer]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/

