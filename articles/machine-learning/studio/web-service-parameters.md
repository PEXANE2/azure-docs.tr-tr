---
title: Web hizmeti parametreleri-Azure Machine Learning Studio (klasik) | Microsoft Docs
description: Web hizmeti erişim sağlandığında modelinizi davranışını değiştirmek için Azure Machine Learning Web hizmeti parametrelerini kullanma
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
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79204418"
---
# <a name="use-azure-machine-learning-studio-classic-web-service-parameters"></a>Azure Machine Learning Studio (klasik) Web hizmeti parametrelerini kullanma

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Bir Azure Machine Learning web hizmeti modülleri ile yapılandırılabilir parametreler içeren bir denemeyi yayımlayarak oluşturulur. Bazı durumlarda, web hizmetinin çalıştığı sırada modülü davranışını değiştirmek isteyebilirsiniz. *Web hizmeti parametreleri* bu görevi yapmanıza izin verir. 

Ortak bir örnek, Web hizmetine erişildiğinde yayınlanan Web hizmeti kullanıcısının farklı bir veri kaynağı belirleyebilmesi için [verileri Içeri aktarma][reader] modülünü ayarlamadır. Ya da [dışarı aktarma veri][writer] modülünü farklı bir hedefin belirtime için yapılandırma. Diğer bazı örnekler, [özellik karma][feature-hashing] modülünün bit sayısını veya [filtre tabanlı özellik seçimi][filter-based-feature-selection] modülünün istenen özelliklerinin sayısını değiştirmeyi içerir. 

Web hizmeti parametreleri ayarlama ve bunları denemenizde bir veya daha fazla modül parametrelerini ile ilişkilendirin ve bunlar gerekli veya isteğe bağlı olup olmadığını belirtebilirsiniz. Web hizmeti aradıklarında web hizmeti kullanıcı daha sonra bu parametreler için değerleri sağlayabilirsiniz. 



## <a name="how-to-set-and-use-web-service-parameters"></a>Ayarlayın ve Web hizmeti parametrelerini kullanma
Parametresi bir modül için yanındaki simgeye tıklayarak ve "web hizmeti parametre olarak Ayarla" seçerek bir Web hizmeti parametre tanımlayın. Bu yeni bir Web hizmeti parametresi oluşturur ve bu modülü parametreye bağlanır. Ardından, web hizmeti erişim sağlandığında kullanıcı Web hizmeti parametresi için bir değer belirtebilir ve modülü parametresi uygulanır.

Bir Web hizmeti parametresi tanımladıktan sonra başka bir modül parametre denemede için kullanılabilir. Bir modül için bir parametre ile ilişkili bir Web hizmeti parametresi tanımlarsanız, değer aynı türde bir parametre bekliyor sürece, aynı Web hizmeti parametre başka herhangi bir modül için kullanabilirsiniz. Web hizmeti parametresi sayısal bir değerdir, örneğin, daha sonra yalnızca sayısal bir değer beklediğiniz modül parametrelerini kullanılabilir. Kullanıcı, Web hizmeti parametresi için bir değer ayarlar, tüm ilişkili modül parametrelerini için uygulanır.

Web hizmeti parametresi için varsayılan bir değer sağlamak karar verebilirsiniz. Bunu yaparsanız, parametre web hizmeti kullanıcı için isteğe bağlıdır. Varsayılan değer sağlamazsanız, web hizmeti erişim sağlandığında bir değer girmek için kullanıcı gereklidir.

Web hizmeti için API belgeleri, web hizmeti kullanıcı için Web hizmeti parametresi web hizmetine erişirken programlı olarak belirtme hakkında bilgi içerir.

> [!NOTE]
> Klasik bir Web hizmeti için API belgeleri, Machine Learning Studio (klasik) içindeki Web hizmeti **panosundaki** **API Yardım sayfası** bağlantısı aracılığıyla sağlanır. Yeni bir Web hizmetine yönelik API belgeleri, Web hizmetiniz için **kullanım ve** **Swagger API** sayfalarında [Azure Machine Learning Web Hizmetleri](https://services.azureml.net/Quickstart) portalı aracılığıyla sağlanır.
> 
> 

## <a name="example"></a>Örnek
Örnek olarak, Azure Blob depolama alanına bilgi gönderen bir [dışarı aktarma veri][writer] modülünün denediğimiz bir deneyime sahip olduğunu varsayalım. Web hizmeti "Blob yolu" adlı bir parametre tanımlarız hizmet eriştiğinde, blob depolama alanına yolunu değiştirmek web hizmeti kullanıcı verir.

1. Machine Learning Studio (klasik) ' de, [verileri dışarı aktarma][writer] modülüne tıklayarak seçin. Özellikler bölmesinde deneme tuvaline sağındaki özellikleri gösterilmektedir.
2. Depolama türünü belirtin:
   
   * **Lütfen veri hedefini belirtin**bölümünde "Azure Blob depolama" seçeneğini belirleyin.
   * **Lütfen kimlik doğrulama türünü belirtin**altında "hesap" seçeneğini belirleyin.
   * Azure blob depolama hesabı bilgilerini girin. 

3. **Kapsayıcı parametresi ile başlayan blob yolunun**sağındaki simgeye tıklayın. Şöyle görünür:
   
   ![Web hizmeti parametre simgesi](./media/web-service-parameters/icon.png)
   
   "Web hizmeti parametre olarak Ayarla" seçin.
   
   Özellikler bölmesinin alt kısmındaki **Web hizmeti parametreleri** altına, "kapsayıcı ile başlayan blob yolu" adıyla bir giriş eklenir. Bu, şu anda bu [veri verme][writer] modülü parametresiyle Ilişkili olan Web hizmeti parametresidir.
4. Web hizmeti parametresini yeniden adlandırmak için ada tıklayın, "blob yolu" yazın ve **ENTER** tuşuna basın. 
5. Web hizmeti parametresi için varsayılan bir değer sağlamak üzere, adın sağ tarafındaki simgeye tıklayın, "varsayılan değer sağla" seçeneğini belirleyin, bir değer girin (örneğin, "kapsayıcı1/output1. csv") ve **ENTER** tuşuna basın.
   
   ![Web hizmeti parametresi](./media/web-service-parameters/parameter.png)
6. **Çalıştır**’a tıklayın. 
7. Web **Hizmeti Dağıt ' a** tıklayın ve Web hizmetini dağıtmak için Web Hizmeti Dağıt **[klasik]** veya Web hizmeti dağıtma [ **Yeni]** seçeneğini belirleyin.

> [!NOTE] 
> Yeni bir web hizmetini dağıtmak için yeterli olan aboneliği, web hizmetini dağıtma olması gerekir. Daha fazla bilgi için, [Azure Machine Learning Web Hizmetleri portalını kullanarak bir Web hizmetini yönetme](manage-new-webservice.md)makalesine bakın. 

Web hizmetinin kullanıcısı artık Web hizmetine erişirken [verileri dışarı aktarma][writer] modülü için yeni bir hedef belirtebilir.

## <a name="more-information"></a>Daha fazla bilgi
Daha ayrıntılı bir örnek için [Machine Learning blogdaki](https://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) [Web hizmeti parametreleri](https://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) girişine bakın.

Machine Learning Web hizmetine erişme hakkında daha fazla bilgi için bkz. [Azure Machine Learning Web hizmeti kullanma](consume-web-services.md).

<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[reader]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[writer]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/

