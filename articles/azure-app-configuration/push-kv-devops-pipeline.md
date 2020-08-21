---
title: Ayarları Azure Pipelines ile uygulama yapılandırmasına gönderme
description: Anahtar değerlerini bir uygulama yapılandırma deposuna göndermek için Azure Pipelines kullanmayı öğrenin
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.topic: how-to
ms.date: 07/27/2020
ms.author: lcozzens
ms.openlocfilehash: c1142ef7b37dee916118964778f6c1db2a65d591
ms.sourcegitcommit: 6fc156ceedd0fbbb2eec1e9f5e3c6d0915f65b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2020
ms.locfileid: "88719743"
---
# <a name="push-settings-to-app-configuration-with-azure-pipelines"></a>Ayarları Azure Pipelines ile uygulama yapılandırmasına gönderme

[Azure uygulama yapılandırma gönderim](https://marketplace.visualstudio.com/items?itemName=AzureAppConfiguration.azure-app-configuration-task-push) görevi, anahtar değerlerini bir yapılandırma dosyasından uygulama yapılandırma deponuza gönderir. Bu görev, artık uygulama yapılandırma deposundaki ayarları ve uygulama yapılandırma deposuna gönderme ayarlarını çektiğinizde, işlem hattı içinde tam daire işlevlerini sağlar.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
- Uygulama yapılandırma kaynağı- [Azure Portal](https://portal.azure.com)ücretsiz olarak bir tane oluşturun.
- Azure DevOps projesi- [ücretsiz olarak bir tane oluşturun](https://go.microsoft.com/fwlink/?LinkId=2014881)
- Azure uygulama yapılandırma gönderme görevi- [Visual Studio Market](https://marketplace.visualstudio.com/items?itemName=AzureAppConfiguration.azure-app-configuration-task-push#:~:text=Navigate%20to%20the%20Tasks%20tab,the%20Azure%20App%20Configuration%20instance.)ücretsiz indirin.

## <a name="create-a-service-connection"></a>Hizmet bağlantısı oluşturma

Bir hizmet bağlantısı Azure DevOps projenizden Azure aboneliğinizdeki kaynaklara erişmenizi sağlar.

1. Azure DevOps 'da, hedef işlem hattınızı içeren projeye gidin ve sol alttaki **Proje ayarlarını** açın.
1. İşlem **hatları** altında **hizmet bağlantıları** ' nı seçin ve sağ üst köşedeki **yeni hizmet bağlantısı** ' nı seçin.
1. **Azure Resource Manager**seçin.
1. **Hizmet sorumlusu (otomatik)** seçeneğini belirleyin.
1. Aboneliğinizi ve kaynağı girin. Hizmet bağlantınıza bir ad verin.

Hizmet bağlantınız oluşturuldığına göre, kendisine atanan hizmet sorumlusunun adını bulun. Sonraki adımda bu hizmet sorumlusuna yeni bir rol ataması ekleyeceksiniz.

1. **Proje ayarları**  >  **hizmet bağlantıları**' na gidin.
1. Önceki bölümde oluşturduğunuz hizmet bağlantısını seçin.
1. **Hizmet sorumlusunu Yönet**' i seçin.
1. Listelenen **görünen adı** dikkate alın.

## <a name="add-role-assignment"></a>Rol ataması ekle

Görevin uygulama yapılandırma deposuna erişebilmesi için görev içinde kullanılan kimlik bilgilerine uygun uygulama yapılandırma rolü atamalarını atayın.

1. Hedef uygulama yapılandırma deponuza gidin. 
1. Sol tarafta **erişim denetimi (IAM)** seçeneğini belirleyin.
1. En üstte **+ Ekle** ve **Rol Ekle atamasını**seçin.
1. **Rol**altında, **uygulama yapılandırma veri sahibi**' nı seçin. Bu rol, görevin uygulama yapılandırma deposundan okuma ve yazma yapmasına izin verir. 
1. Önceki bölümde oluşturduğunuz hizmet bağlantısıyla ilişkili hizmet sorumlusunu seçin.
  
## <a name="use-in-builds"></a>Derlemelerde kullan

Bu bölümde, Azure DevOps derleme işlem hattında Azure uygulama yapılandırma gönderim görevinin nasıl kullanılacağı ele alınmaktadır.

1. **Ardışık**düzen işlem hatları ' na tıklayarak derleme ardışık düzeni sayfasına gidin  >  **Pipelines**. Derleme işlem hatları için belgeler [burada](https://docs.microsoft.com/azure/devops/pipelines/create-first-pipeline?view=azure-devops&tabs=tfs-2018-2)bulunabilir.
      - Yeni bir derleme işlem hattı oluşturuyorsanız, işlem hattının sağ tarafındaki **yardımcıyı göster** ' i seçin ve **Azure uygulama yapılandırma gönderme** görevini arayın.
      - Var olan bir derleme işlem hattını kullanıyorsanız, işlem hattını düzenlenirken **Görevler** sekmesine gidin ve **Azure uygulama yapılandırma gönderme** görevini arayın.
2. Yapılandırma dosyasından uygulama yapılandırma deposuna anahtar değerlerini göndermek için gereken parametreleri yapılandırın. **Yapılandırma dosyası yolu** parametresi dosya deposunun kökünde başlar.
3. Bir derlemeyi kaydedin ve kuyruğa alın. Yapı günlüğünde, görevin yürütülmesi sırasında oluşan tüm arızalar görüntülenir.

## <a name="use-in-releases"></a>Sürümlerde kullan

Bu bölümde, Azure uygulama yapılandırma gönderme görevinin bir Azure DevOps yayın işlem hatlarında nasıl kullanılacağı ele alınacaktır.

1. İşlem hattı Yayınları ' **nı seçerek yayın**ardışık düzeni sayfasına gidin  >  **Releases**. Yayın işlem hatlarına yönelik belgeler [burada](https://docs.microsoft.com/azure/devops/pipelines/release?view=azure-devops)bulunabilir.
1. Mevcut bir yayın işlem hattını seçin. Yoksa, yeni bir tane oluşturmak için **+ Yeni** ' yi seçin.
1. Yayın işlem hattını düzenlemek için sağ üst köşedeki **Düzenle** düğmesini seçin.
1. Görevi eklemek için **aşamayı** seçin. Aşamalar hakkında daha fazla bilgiyi [burada](https://docs.microsoft.com/azure/devops/pipelines/release/environments?view=azure-devops)bulabilirsiniz.
1. **+** Bu iş için ' i seçin ve ardından **dağıtım** sekmesinin altına **Azure uygulama yapılandırma gönderim** görevini ekleyin.
1. Anahtar değerlerinizi yapılandırma dosyanızdaki uygulama yapılandırma deponuza göndermek için görev içinde gerekli parametreleri yapılandırın. Parametrelerin açıklamaları aşağıdaki **Parametreler** bölümünde ve her parametrenin yanındaki araç ipuçlarında kullanılabilir.
1. Bir yayını kaydedin ve kuyruğa alın. Yayın günlüğünde, görevin yürütülmesi sırasında karşılaşılan tüm arızalar görüntülenir.

## <a name="parameters"></a>Parametreler

Aşağıdaki parametreler uygulama yapılandırma gönderme görevi tarafından kullanılır:

- **Azure aboneliği**: kullanılabilir Azure hizmeti bağlantılarınızı içeren bir açılır liste. Kullanılabilir Azure hizmeti bağlantıları listenizi güncelleştirmek ve yenilemek için, TextBox 'ın sağ tarafındaki **Azure aboneliğini yenile** düğmesine basın.
- **Uygulama yapılandırma adı**: seçili abonelik altında kullanılabilir yapılandırma depolarınızı yükleyen bir açılan liste. Kullanılabilir yapılandırma depoları listenizi güncelleştirmek ve yenilemek için, TextBox 'ın sağ tarafındaki **uygulama yapılandırma adını Yenile** düğmesine basın.
- **Yapılandırma dosyası yolu**: yapılandırma dosyanızın yolu. Yapılandırma dosyası seçmek için yapı yapıtına göz atabilirsiniz. ( `...` TextBox 'ın sağ tarafındaki düğme).
- **Ayırıcı**:. JSON ve. yml dosyalarını düzleştirmek için kullanılan ayırıcı.
- **Derinlik**:. JSON ve. yml dosyalarının ne kadar düzleştirilmesini sağlayacak derinlik.
- **Ön ek**: uygulama yapılandırma deposuna gönderilen her bir anahtarın başlangıcına eklenen bir dize.
- **Etiket**: uygulama yapılandırma deposundaki etiket olarak her anahtar değerine eklenen bir dize.
- **Içerik türü**: uygulama yapılandırma deposu içindeki içerik türü olarak her anahtar değerine eklenen bir dize.
- **Etiketler**: `{"tag1":"val1", "tag2":"val2"}` uygulama yapılandırma deponuza gönderilen her anahtar değerine eklenen etiketleri TANıMLAYAN biçiminde bir JSON nesnesi.
- **Depodaki tüm diğer anahtar değerlerini belirtilen önek ve etiketle Sil**: varsayılan değer **işaretli**değildir.
  - **Checked**: yapılandırma dosyasından yeni anahtar değerleri göndermeden önce belirtilen önek ve etiketle eşleşen uygulama yapılandırma deposundaki tüm anahtar değerlerini kaldırır.
  - **İşaretlenmemiş**: yapılandırma dosyasındaki tüm anahtar değerlerini uygulama yapılandırma deposuna gönderir ve uygulama yapılandırma deposundaki diğer her şeyi bozulmadan bırakır.

Gerekli parametreleri doldurduktan sonra işlem hattını çalıştırın. Belirtilen yapılandırma dosyasındaki tüm anahtar değerleri, uygulama yapılandırmasına yüklenecek.

## <a name="troubleshooting"></a>Sorun giderme

Beklenmeyen bir hata oluşursa, işlem hattı değişkeni olarak ayarlanarak hata ayıklama günlükleri etkinleştirilebilir `system.debug` `true` .

## <a name="faq"></a>SSS

**Birden çok yapılandırma dosyasını nasıl karşıya yükleyebilirim?**

Birden çok yapılandırma dosyasını uygulama yapılandırma deposuna göndermek için aynı işlem hattı içinde Azure uygulama yapılandırma gönderim görevinin birden fazla örneğini oluşturun.

**Anahtar değerlerini yapılandırma depoma göndermeye çalışırken neden 409 hatası alıyorum?**

Görev, uygulama yapılandırma deposunda kilitlenen bir anahtar değerini kaldırmaya veya üzerine yazmaya çalışırsa bir 409 çakışma hata iletisi oluşur.
