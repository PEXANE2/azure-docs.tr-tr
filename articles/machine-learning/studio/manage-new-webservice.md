---
title: Web hizmetlerini yönetme
titleSuffix: ML Studio (classic) - Azure
description: Microsoft Azure Machine Learning Web Hizmetleri portalını kullanarak Machine Learning New ve Classic Web hizmetlerinizi yönetin. Klasik Web hizmetleri ve Yeni Web hizmetleri farklı temel teknolojilere dayandığından, her biri için biraz farklı yönetim yeteneklerine sahipsiniz.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: previous-ms.author=yahajiza, previous-author=YasinMSFT
ms.date: 02/28/2017
ms.openlocfilehash: 2277aa3de5955efe5a3e4cb938fa557352f89006
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79217969"
---
# <a name="manage-a-web-service-using-the-azure-machine-learning-studio-classic-web-services-portal"></a>Azure Machine Learning Studio (klasik) Web Hizmetleri portalını kullanarak bir web hizmetini yönetme

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Microsoft Azure Machine Learning Web Hizmetleri portalını kullanarak Machine Learning New ve Classic Web hizmetlerinizi yönetebilirsiniz. Klasik Web hizmetleri ve Yeni Web hizmetleri farklı temel teknolojilere dayandığından, her biri için biraz farklı yönetim yeteneklerine sahipsiniz.

Machine Learning Web Services portalında şunları yapabilirsiniz:

* Web hizmetinin nasıl kullanıldığını izleyin.
* Açıklamayı yapılandırın, web hizmetinin anahtarlarını güncelleştirin (yalnızca Yeni), depolama hesabı anahtarınızı güncelleştirin (yalnızca Yeni), günlüğe kaydetmeyi etkinleştirin ve örnek verileri etkinleştirin veya devre dışı kalacaksınız.
* Web hizmetini silin.
* Faturalandırma planlarını oluşturun, silin veya güncelleştirin (yalnızca Yeni).
* Uç noktaları ekleme ve silme (Yalnızca Klasik)

>[!NOTE]
>Ayrıca, **Web hizmetleri** sekmesinde [Machine Learning Studio'da (klasik)](https://studio.azureml.net) Klasik web hizmetlerini yönetebilirsiniz.

## <a name="permissions-to-manage-new-resources-manager-based-web-services"></a>Yeni Kaynaklar Yöneticisi tabanlı web hizmetlerini yönetme izinleri

Yeni web hizmetleri Azure kaynakları olarak dağıtılır. Bu nedenle, Yeni web hizmetlerini dağıtmak ve yönetmek için doğru izinlere sahip olmalısınız.  Yeni web hizmetlerini dağıtmak veya yönetmek için, web hizmetinin dağıtıldığı abonelikte bir katılımcı veya yönetici rolü atanması gerekir. Başka bir kullanıcıyı makine öğrenimi çalışma alanına davet ederseniz, web hizmetlerini dağıtmadan veya yönetebilmeleri için önce onları abonelikteki bir katılımcı veya yönetici rolüne atamanız gerekir. 

Kullanıcı, Azure Machine Learning Web Hizmetleri portalındaki kaynaklara erişmek için doğru izinlere sahip değilse, bir web hizmeti dağıtmaya çalışırken aşağıdaki hatayı alır:

*Web Hizmeti dağıtımı başarısız oldu. Bu hesap, Çalışma Alanı'nı içeren Azure aboneliğine yeterli erişime sahip değildir. Bir Web Hizmetini Azure'a dağıtmak için, aynı hesabın Çalışma Alanına davet edilmesi ve Çalışma Alanı'nı içeren Azure aboneliğine erişim izni verilmesi gerekir.*

Çalışma alanı oluşturma hakkında daha fazla bilgi için azure [machine learning studio (klasik) çalışma alanı oluştur ve paylaş'](create-workspace.md)a bakın.

Erişim izinleri ayarlama hakkında daha fazla bilgi için Bkz. [RBAC ve Azure portalını kullanarak erişimi yönet.](../../role-based-access-control/role-assignments-portal.md)


## <a name="manage-new-web-services"></a>Yeni Web hizmetlerini yönetme
Yeni Web hizmetlerinizi yönetmek için:

1. Microsoft Azure hesabınızı kullanarak [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/quickstart) portalında oturum açın - Azure aboneliğiyle ilişkili hesabı kullanın.
2. Menüde Web **Hizmetleri'ni**tıklatın.

Bu, aboneliğiniz için dağıtılan Web hizmetlerinin listesini görüntüler. 

Bir Web hizmetini yönetmek için Web Hizmetleri'ni tıklatın. Web Hizmetleri sayfasından şunları yapabilirsiniz:

* Yönetmek için web hizmetini tıklatın.
* Güncelleştirmek için web hizmetiiçin FaturaLandırma Planı'nı tıklatın.
* Bir web hizmetini silin.
* Bir web hizmetini kopyalayın ve başka bir bölgeye dağıtın.

Bir web hizmetini tıklattığınızda, web hizmeti Quickstart sayfası açılır. Web hizmeti Quickstart sayfasında web hizmetinizi yönetmenize olanak tanıyan iki menü seçeneği vardır:

* **PANO** - Web hizmeti kullanımını görüntülemenizi sağlar.
* **CONFIGURE** - Açıklayıcı metin eklemenize, Web hizmetiyle ilişkili depolama hesabının anahtarını güncelleştirmenize ve örnek verileri etkinleştirmenize veya devre dışı bıraktığınızda izin verir.

### <a name="monitoring-how-the-web-service-is-being-used"></a>Web hizmetinin nasıl kullanıldığını izleme
**PANO** sekmesini tıklatın.

Panodan, web hizmetinizin genel kullanımını belirli bir süre içinde görüntüleyebilirsiniz. Kullanım grafiklerinin sağ üst kısmındaki Dönem açılır menüsünden görüntülenebilen dönemi seçebilirsiniz. Pano aşağıdaki bilgileri gösterir:

* **Zaman Daki İstekler,** seçili zaman dilimindeki istek sayısının adım grafiğini görüntüler. Kullanımda ani artışlar yaşanıp yaşamadığınızı belirlemenize yardımcı olabilir.
* **İstek-Yanıt İstekleri,** hizmetin seçili zaman dilimi içinde aldığı toplam İstek-Yanıt çağrısı sayısını ve bunların kaçtanesinin başarısız olduğunu görüntüler.
* **Ortalama İstek-Yanıt İşlem Süresi,** alınan istekleri yürütmek için gereken sürenin ortalamasını görüntüler.
* **Toplu İşlemler,** hizmetin seçili zaman dilimi içinde aldığı toplam Toplu İşlem sayısını ve bunların kaç tanesinin başarısız olduğunu görüntüler.
* **Ortalama İş Gecikmesi,** alınan istekleri yürütmek için gereken sürenin ortalamasını görüntüler.
* **Hatalar,** web hizmetine yapılan aramalarda oluşan toplam hata sayısını görüntüler.
* **Hizmet Maliyetleri,** hizmetle ilişkili faturalandırma planının ücretlerini görüntüler.

### <a name="configuring-the-web-service"></a>Web hizmetini yapılandırma
**CONFIGURE** menüsü seçeneğini tıklatın.

Aşağıdaki özellikleri güncelleştirebilirsiniz:

* **Açıklama,** Web hizmeti için bir açıklama girmenize olanak tanır.
* **Başlık,** Web hizmeti için bir başlık girmenizi sağlar
* **Tuşlar** birincil ve ikincil API tuşlarınızı döndürmenizi sağlar.
* **Depolama hesabı anahtarı,** Web hizmeti değişiklikleriyle ilişkili depolama hesabının anahtarını güncelleştirmenize olanak tanır. 
* **Örnek verileri etkinleştir,** İstek-Yanıt hizmetini sınamak için kullanabileceğiniz örnek verileri sağlamanıza olanak tanır. Machine Learning Studio'da (klasik) web hizmetini oluşturduysanız, örnek veriler modelinizi eğitmek için kullandığınız verilerden alınır. Hizmeti programlı bir şekilde oluşturduysanız, veriler JSON paketinin bir parçası olarak sağladığınız örnek verilerden alınır.

### <a name="managing-billing-plans"></a>Faturalandırma planlarını yönetme
Web hizmetleri Quickstart sayfasından **Planlar** menüsü seçeneğini tıklatın. Ayrıca, bu planı yönetmek için belirli Web hizmetiyle ilişkili planı da tıklatabilirsiniz.

* **Yeni** yeni bir plan oluşturmanıza olanak sağlar.
* **Plan Ekle/Kaldır örneği,** kapasite eklemek için varolan bir planı "ölçeklendirmenize" olanak tanır.
* **Yükseltme/Düşürme,** kapasite eklemek için varolan bir planı "ölçeklendirmenize" olanak tanır.
* **Silme,** bir planı silmenizi sağlar.

Panosunu görüntülemek için bir planı tıklatın. Pano, belirli bir süre boyunca anlık görüntü veya plan kullanımı sağlar. Görüntülenebilmek için süreyi seçmek için, panonun sağ üst kısmındaki **Dönem** açılır görünümünü tıklatın. 

Plan panosu aşağıdaki bilgileri sağlar:

* **Plan Açıklaması,** planla ilişkili maliyetler ve kapasite yle ilgili bilgileri görüntüler.
* **Plan Kullanımı,** plana karşı ücretlendirilen hareket ve işlem saatlerinin sayısını görüntüler.
* **Web Hizmetleri,** bu planı kullanan Web hizmetlerinin sayısını görüntüler.
* **Aramalara Göre En İyi Web Hizmeti,** plana karşı ücretlendirilen çağrılar yapan en iyi dört Web hizmetini görüntüler.
* **Compute Hrs tarafından En İyi Web Hizmetleri,** plana karşı ücretlendirilen işlem kaynaklarını kullanan en iyi dört Web hizmetlerini görüntüler.

## <a name="manage-classic-web-services"></a>Klasik Web Hizmetlerini Yönetin
> [!NOTE]
> Bu bölümdeki yordamlar, Azure Machine Learning Web Services portalı üzerinden Klasik web hizmetlerinin yönetilmesiyle ilgilidir. Machine Learning Studio (klasik) ve Azure portalı üzerinden Klasik Web hizmetlerini yönetme hakkında daha fazla bilgi için [bkz.](manage-workspace.md)
> 
> 

Klasik Web hizmetlerinizi yönetmek için:

1. Microsoft Azure hesabınızı kullanarak [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/quickstart) portalında oturum açın - Azure aboneliğiyle ilişkili hesabı kullanın.
2. Menüde Klasik **Web Hizmetleri'ni**tıklatın.

Klasik Web Hizmeti'ni yönetmek için **Klasik Web Hizmetleri'ni**tıklatın. Klasik Web Hizmetleri sayfasından şunları yapabilirsiniz:

* İlişkili uç noktaları görüntülemek için web hizmetini tıklatın.
* Bir web hizmetini silin.

Klasik Web hizmetini yönettiğinizde, uç noktaların her birini ayrı ayrı yönetirsiniz. Web Hizmetleri sayfasında bir web hizmetini tıklattığınızda, hizmetle ilişkili uç noktaların listesi açılır. 

Klasik Web Hizmeti bitiş noktası sayfasında, hizmete uç noktaları ekleyebilir ve silebilirsiniz. Uç noktaları ekleme hakkında daha fazla bilgi için [bkz.](create-endpoint.md)

Web hizmeti Quickstart sayfasını açmak için uç noktalardan birini tıklatın. Quickstart sayfasında, web hizmetinizi yönetmenize olanak tanıyan iki menü seçeneği vardır:

* **PANO** - Web hizmeti kullanımını görüntülemenizi sağlar.
* **CONFIGURE** - Açıklayıcı metin eklemenize, hata günlüğe kaydetmeyi açmanızı, Web hizmetiyle ilişkili depolama hesabının anahtarını güncelleştirmenizi ve örnek verileri etkinleştirmenizi ve devre dışı etmenizi sağlar.

### <a name="monitoring-how-the-web-service-is-being-used"></a>Web hizmetinin nasıl kullanıldığını izleme
**PANO** sekmesini tıklatın.

Panodan, web hizmetinizin genel kullanımını belirli bir süre içinde görüntüleyebilirsiniz. Kullanım grafiklerinin sağ üst kısmındaki Dönem açılır menüsünden görüntülenebilen dönemi seçebilirsiniz. Pano aşağıdaki bilgileri gösterir:

* **Zaman Daki İstekler,** seçili zaman dilimindeki istek sayısının adım grafiğini görüntüler. Kullanımda ani artışlar yaşanıp yaşamadığınızı belirlemenize yardımcı olabilir.
* **İstek-Yanıt İstekleri,** hizmetin seçili zaman dilimi içinde aldığı toplam İstek-Yanıt çağrısı sayısını ve bunların kaçtanesinin başarısız olduğunu görüntüler.
* **Ortalama İstek-Yanıt İşlem Süresi,** alınan istekleri yürütmek için gereken sürenin ortalamasını görüntüler.
* **Toplu İşlemler,** hizmetin seçili zaman dilimi içinde aldığı toplam Toplu İşlem sayısını ve bunların kaç tanesinin başarısız olduğunu görüntüler.
* **Ortalama İş Gecikmesi,** alınan istekleri yürütmek için gereken sürenin ortalamasını görüntüler.
* **Hatalar,** web hizmetine yapılan aramalarda oluşan toplam hata sayısını görüntüler.
* **Hizmet Maliyetleri,** hizmetle ilişkili faturalandırma planının ücretlerini görüntüler.

### <a name="configuring-the-web-service"></a>Web hizmetini yapılandırma
**CONFIGURE** menüsü seçeneğini tıklatın.

Aşağıdaki özellikleri güncelleştirebilirsiniz:

* **Açıklama,** Web hizmeti için bir açıklama girmenize olanak tanır. Açıklama gerekli bir alandır.
* **Günlüğe kaydetme,** bitiş noktasında hata günlüğe kaydetmeyi etkinleştirmenize veya devre dışı betmenizi sağlar. Günlük İşlemi hakkında daha [logging for Machine Learning web services](web-services-logging.md)fazla bilgi için bkz.
* **Örnek verileri etkinleştir,** İstek-Yanıt hizmetini sınamak için kullanabileceğiniz örnek verileri sağlamanıza olanak tanır. Machine Learning Studio'da (klasik) web hizmetini oluşturduysanız, örnek veriler modelinizi eğitmek için kullandığınız verilerden alınır. Hizmeti programlı bir şekilde oluşturduysanız, veriler JSON paketinin bir parçası olarak sağladığınız örnek verilerden alınır.


