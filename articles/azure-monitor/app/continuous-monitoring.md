---
title: DevOps sürüm ardışık sisteminizin Azure Denetim Hatları ve Azure Uygulama Öngörüleri ile sürekli izlenmesi | Microsoft Dokümanlar
description: Uygulama Öngörüleri ile sürekli izlemeyi hızla ayarlamak için talimatlar sağlar
ms.topic: conceptual
ms.date: 07/16/2019
ms.openlocfilehash: e565101218b975ef2bd29b8a32a4aa1bf4300b6d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77655404"
---
# <a name="add-continuous-monitoring-to-your-release-pipeline"></a>Sürüm ardışık sisteminize sürekli izleme ekleme

Azure Pipelines, Yazılım geliştirme yaşam döngüsü boyunca DevOps sürüm ardınızın sürekli olarak izlenmesine olanak sağlamak için Azure Uygulama Öngörüleri ile tümleşir. 

Sürekli izleme yle, sürüm ardışık hatları, Uygulama Öngörüleri ve diğer Azure kaynaklarından gelen izleme verilerini birbiriyle birleştirebilir. Sürüm ardışık bir Uygulama Öngörüleri uyarısı algıladığında, ardışık iş adı, uyarı çözülene kadar dağıtımı kaplayabilir veya geri alabilir. Tüm denetimler geçerse, dağıtımlar el ile müdahaleye gerek kalmadan testten üretime kadar otomatik olarak devam edebilir. 

## <a name="configure-continuous-monitoring"></a>Sürekli izlemeyi yapılandırma

1. [Azure DevOps'te](https://dev.azure.com)bir kuruluş ve proje seçin.
   
1. Proje sayfasının sol menüsünde, **Pipelines** > **Releases'i**seçin. 
   
1. **Yeni'nin** yanındaki oku bırakın ve **Yeni sürüm ardışık hattını**seçin. Veya, henüz bir ardışık hattınız yoksa, görünen sayfada **Yeni ardışık**
   
1. **Şablon** seç bölmesinde, sürekli **izleme yle Azure Uygulama Hizmeti dağıtımını**arayın ve seçin ve ardından **Uygula'yı**seçin. 

   ![Yeni Azure Ardışık Hatları serbest ardışık](media/continuous-monitoring/001.png)

1. Aşama **1** kutusunda, aşama görevlerini görüntülemek için köprüseçeneğini **seçin.**

   ![Sahne görevlerini görüntüleme](media/continuous-monitoring/002.png)

1. Aşama **1** yapılandırma bölmesinde, aşağıdaki alanları tamamlayın: 

    | Parametre        | Değer |
   | ------------- |:-----|
   | **Aşama adı**      | Bir sahne adı sağlayın veya **Aşama 1'de**bırakın. |
   | **Azure aboneliği** | Aşağı inin ve kullanmak istediğiniz bağlantılı Azure aboneliğini seçin.|
   | **Uygulama türü** | Aşağı inin ve uygulama türünü seçin. |
   | **Uygulama Hizmeti adı** | Azure Uygulama Hizmetinizin adını girin. |
   | **Uygulama Öngörüleri için Kaynak Grubu adı**    | Aşağı inin ve kullanmak istediğiniz kaynak grubunu seçin. |
   | **Uygulama Öngörüleri kaynak adı** | Aşağı inin ve seçtiğiniz kaynak grubu için Uygulama Öngörüleri kaynağını seçin.

1. Varsayılan uyarı kuralı ayarlarıyla ardışık hattı kaydetmek için Azure DevOps penceresinde sağ üstte **Kaydet'i** seçin. Açıklayıcı bir açıklama girin ve ardından **Tamam'ı**seçin.

## <a name="modify-alert-rules"></a>Uyarı kurallarını değiştirme

Kutunun dışında, sürekli izleme şablonuna **sahip Azure Uygulama Hizmeti dağıtımının** dört uyarı kuralı vardır: **Kullanılabilirlik**, **Başarısız istekler,** **Sunucu yanıt süresi**ve Sunucu **özel durumları.** Daha fazla kural ekleyebilir veya hizmet düzeyi gereksinimlerinizi karşılamak için kural ayarlarını değiştirebilirsiniz. 

Uyarı kuralı ayarlarını değiştirmek için:

1. Sürüm ardışık alanı sayfasının sol bölmesinde, **Uygulama Öngörülerini Yapılandır'ı**seçin.

1. Azure **Monitör Uyarıları** bölmesinde, **Uyarı kurallarının** **yanında** elips ... seçeneğini belirleyin.
   
1. Uyarı **kuralları** iletişim kutusunda, **Kullanılabilirlik**gibi bir uyarı kuralının yanındaki açılır simgeyi seçin. 
   
1. Gereksinimlerinizi karşılamak için **Eşik** ve diğer ayarları değiştirin.
   
   ![Uyarıyı değiştirme](media/continuous-monitoring/003.png)
   
1. **Tamam'ı**seçin ve ardından Azure DevOps penceresinde sağ üstte **Kaydet'i** seçin. Açıklayıcı bir açıklama girin ve ardından **Tamam'ı**seçin.

## <a name="add-deployment-conditions"></a>Dağıtım koşulları ekleme

Sürüm ardışık noktanıza dağıtım kapıları eklediğinizde, ayarladığınız eşikleri aşan bir uyarı istenmeyen sürüm promosyonunu önler. Uyarıyı çözdükten sonra dağıtım otomatik olarak devam edebilir.

Dağıtım kapıları eklemek için:

1. Ana boru hattı sayfasında, **Aşamalar**altında, hangi aşamanın sürekli bir izleme kapısına ihtiyaç duyduğuna bağlı olarak **dağıtım öncesi koşulları** veya dağıtım sonrası **koşullar** simgesini seçin.
   
   ![Dağıtım öncesi koşullar](media/continuous-monitoring/004.png)
   
1. Dağıtım **öncesi koşullar** yapılandırma bölmesinde, **Gates'i** **Etkin**olarak ayarlayın.
   
1. Dağıtım **kapılarının**yanında **Ekle'yi**seçin.
   
1. Açılan menüden **Azure Monitörünü Sorgula uyarılarını** seçin. Bu seçenek, hem Azure Monitörü hem de Uygulama Öngörüleri uyarılarına erişmenizi sağlar.
   
   ![Azure Monitör uyarılarını sorgula](media/continuous-monitoring/005.png)
   
1. **Değerlendirme seçenekleri**altında, **kapıların yeniden değerlendirilmesi ile** sonraki **kapıların başarısız olduğu zaman sonu**gibi ayarlar için istediğiniz değerleri girin. 

## <a name="view-release-logs"></a>Sürüm günlüklerini görüntüleme

Dağıtım kapısı davranışını ve diğer sürüm adımlarını sürüm günlüklerinde görebilirsiniz. Günlükleri açmak için:

1. Ardışık hatlar sayfasının sol menüsünden **Sürümler'i** seçin. 
   
1. Herhangi bir sürümü seçin. 
   
1. **Aşamalar**altında, bir sürüm özetini görüntülemek için herhangi bir aşamayı seçin. 
   
1. Günlükleri görüntülemek için, sürüm özetinde **günlükleri** görüntüle'yi seçin, herhangi bir aşamada **Başarılı** veya **Başarısız köprübağlantısını** seçin veya herhangi bir aşamanın üzerine gidip **Günlükler'i**seçin. 
   
   ![Sürüm günlüklerini görüntüleme](media/continuous-monitoring/006.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure Ardışık Hatları hakkında daha fazla bilgi için [Azure Ardışık Hatları belgelerine](https://docs.microsoft.com/azure/devops/pipelines)bakın.
