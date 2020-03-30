---
title: Azure İşlevleri için sürekli dağıtım
description: İşlevlerinizi yayınlamak için Azure Uygulama Hizmeti'nin sürekli dağıtım özelliklerini kullanın.
ms.assetid: 361daf37-598c-4703-8d78-c77dbef91643
ms.topic: conceptual
ms.date: 09/25/2019
ms.openlocfilehash: cc1e100a0c2e652ab081869409fd24dbf88017a3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277030"
---
# <a name="continuous-deployment-for-azure-functions"></a>Azure İşlevleri için sürekli dağıtım

[Kaynak denetimi tümleştirmesini](functions-deployment-technologies.md#source-control)kullanarak kodunuzu sürekli olarak dağıtmak için Azure İşlevlerini kullanabilirsiniz. Kaynak denetimi tümleştirmesi, kod güncelleştirmenin Azure'a dağıtımı tetiklediği bir iş akışına olanak tanır. Azure İşlevler'de yeniyseniz, [Azure İşlevlerine genel bakışı](functions-overview.md)inceleyerek başlayın.

Sürekli dağıtım, birden çok ve sık katkıları tümleştirdiğiniz projeler için iyi bir seçenektir. Sürekli dağıtım kullandığınızda, kodlarınız için tek bir doğruluk kaynağı tutarsınız ve bu da ekiplerin kolayca işbirliği yapmasına olanak tanır. Azure İşlevlerinde sürekli dağıtımı aşağıdaki kaynak kodu konumlarından yapılandırabilirsiniz:

* [Azure Repos](https://azure.microsoft.com/services/devops/repos/)
* [GitHub](https://github.com)
* [Bitbucket](https://bitbucket.org/)

Azure'daki işlevler için dağıtım birimi işlev uygulamasıdır. Bir işlev uygulamasındaki tüm işlevler aynı anda dağıtılır. Sürekli dağıtımı etkinleştirdikten sonra, gerçeğin kaynağı başka bir yerde olarak ayarlandığı için Azure portalındaki işlev koduna erişim *salt okunur* olarak yapılandırılır.

## <a name="requirements-for-continuous-deployment"></a>Sürekli dağıtım gereksinimleri

Sürekli dağıtımın başarılı olabilmesi için dizin yapınızın Azure Fonksiyonları'nın beklediği temel klasör yapısıyla uyumlu olması gerekir.

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

>[!NOTE]  
> Tüketim planı üzerinde çalışan Linux uygulamaları için sürekli dağıtım henüz desteklenmemektedir. 

## <a name="set-up-continuous-deployment"></a><a name="credentials"></a>Sürekli dağıtım ayarlama

Varolan bir işlev uygulaması için sürekli dağıtımı yapılandırmak için aşağıdaki adımları tamamlayın. Adımlar, GitHub deposuyla tümleştirmeyi gösterir, ancak Azure Deposu veya diğer kaynak kodu depoları için benzer adımlar geçerlidir.

1. [Azure portalındaki](https://portal.azure.com)işlev uygulamanızda Platform **özellikleri** > **Dağıtım Merkezi'ni**seçin.

    ![Dağıtım Merkezini Aç](./media/functions-continuous-deployment/platform-features.png)

2. **Dağıtım Merkezi'nde** **GitHub'ı**seçin ve ardından **Yetkilendir'i**seçin. GitHub'ı zaten yetkilendirdiyseniz **Devam et'i**seçin. 

    ![Azure Uygulama Hizmeti Dağıtım Merkezi](./media/functions-continuous-deployment/github.png)

3. GitHub'da **AzureAppService'i Yetkilendirme düğmesini** seçin. 

    ![Azure Uygulama Hizmetini Yetkilendirme](./media/functions-continuous-deployment/authorize.png)
    
    Azure portalındaki **Dağıtım Merkezi'nde** **Devam et'i**seçin.

4. Aşağıdaki yapı sağlayıcılarından birini seçin:

    * **Uygulama Hizmeti oluşturma hizmeti**: En iyi, bir yapıya ihtiyacınız olmadığında veya genel bir yapıya ihtiyacınız olduğunda.
    * **Azure Ardışık Hatları (Önizleme)**: Yapı üzerinde daha fazla denetime ihtiyaç duyduğunuzda en iyi sidir. Bu sağlayıcı şu anda önizlemede.

    ![Bir yapı sağlayıcısı seçin](./media/functions-continuous-deployment/build.png)

5. Belirttiğiniz kaynak denetim seçeneğine özgü bilgileri yapılandırın. GitHub **için, Kuruluş,** **Depo**ve **Şube**için değerleri girmeniz veya seçmeniz gerekir. Değerler, kodunuzun konumuna bağlıdır. Ardından Devam **et'i**seçin.

    ![GitHub'ı yapılandır](./media/functions-continuous-deployment/github-specifics.png)

6. Tüm ayrıntıları gözden geçirin ve ardından dağıtım yapılandırmanızı tamamlamak için **Bitir'i** seçin.

    ![Özet](./media/functions-continuous-deployment/summary.png)

İşlem tamamlandığında, belirtilen kaynaktan gelen tüm kodlar uygulamanız için dağıtılır. Bu noktada, dağıtım kaynağındaki değişiklikler, bu değişikliklerin Azure'daki işlev uygulamanızda dağıtılmasını tetikler.

## <a name="deployment-scenarios"></a>Dağıtım senaryoları

<a name="existing"></a>

### <a name="move-existing-functions-to-continuous-deployment"></a>Varolan işlevleri sürekli dağıtıma taşıma

[Azure portalında](https://portal.azure.com) işlevleri zaten yazdıysanız ve sürekli dağıtıma geçmeden önce uygulamanızın içeriğini indirmek istiyorsanız, işlev uygulamanızın **Genel Bakış** sekmesine gidin. Uygulama **içeriğini indir** düğmesini seçin.

![Uygulama içeriğini indirin](./media/functions-continuous-deployment/download.png)

> [!NOTE]
> Sürekli tümleştirmeyi yapılandırıldıktan sonra, kaynak dosyalarınızı Artık Işlevler portalında kaldırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure İşlevleri için en iyi uygulamalar](functions-best-practices.md)
