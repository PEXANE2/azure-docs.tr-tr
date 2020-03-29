---
title: Azure DevOps Projeleri ile CI/CD boru hattı - Azure IoT Edge | Microsoft Dokümanlar
description: Azure DevOps Projeleri, Azure'a başlamayı kolaylaştırır. Birkaç hızlı adımda seçtiğiniz bir Azure IoT Edge uygulamasını başlatmanıza yardımcı olur.
author: shizn
ms.author: xshi
ms.date: 10/09/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 66401678f03ee0043345208eb32560f589829226
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76510320"
---
# <a name="create-a-cicd-pipeline-for-iot-edge-with-azure-devops-projects"></a>Azure DevOps Projeleri ile IoT Edge için BIR CI/CD ardışık

DevOps Projects ile IoT Edge uygulamanız için sürekli tümleştirme (CI) ve sürekli teslimat (CD) yapılandırın. DevOps Projects, Azure Ardışık Hatları'nda bir yapı ve sürüm ardışık hattının ilk yapılandırmasını basitleştirir.

Etkin bir Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

DevOps Projects, Azure DevOps'te bir CI/CD ardışık hattı oluşturur. Yeni bir Azure DevOps kuruluşu oluşturabilir veya varolan bir kuruluş kullanabilirsiniz. DevOps Projects, seçtiğiniz Azure aboneliğinde Azure kaynakları da oluşturur.

1. Microsoft Azure [portalında](https://portal.azure.com)oturum açın.

1. Sol bölmede, **kaynak oluştur'u**seçin ve ardından **DevOps Projeleri'ni**arayın.  

1. **Oluştur'u**seçin.

## <a name="create-a-new-application-pipeline"></a>Yeni bir uygulama ardışık hattı oluşturma

1. Azure IoT Edge modülü(ler) [C#](tutorial-csharp-module.md), [Düğüm.js,](tutorial-node-module.md) [Python,](tutorial-python-module.md) [C](tutorial-c-module.md) ve [Java](tutorial-java-module.md)olarak yazılabilir. Yeni bir uygulama başlatmak için tercih ettiğiniz dili seçin: **.NET**, **Düğüm.js**, **Python**, **C**, veya **Java**. Devam etmek için **İleri**’yi seçin.

   ![Yeni bir uygulama oluşturmak için dil seçin](./media/how-to-devops-project/select-language.png)

2. Uygulama çerçeveniz olarak **Basit IoT'yi** seçin ve sonra **İleri'yi**seçin.

   ![Basit IoT çerçeveyi seçin](media/how-to-devops-project/select-iot.png)

3. Uygulamanızı dağıtan Azure hizmeti olarak **IoT Edge'i** seçin ve ardından **İleri'yi**seçin.

   ![IoT Edge hizmetini seçin](media/how-to-devops-project/select-iot-edge.png)

4. Yeni bir ücretsiz Azure DevOps kuruluşu oluşturun veya varolan bir kuruluş seçin.

   1. Projeniz için bir ad sağlayın.

   2. Azure DevOps kuruluşunuzu seçin. Varolan bir kuruluşunuz yoksa, yeni bir kuruluş oluşturmak için **Ek ayarları'nı** seçin.

   3. Azure aboneliğinizi seçin.

   4. Proje adınız tarafından oluşturulan IoT Hub adını kullanın veya kendi adınızı sağlayın.

   5. Varsayılan konumu kabul edin veya size yakın birini seçin.

   6. DevOps Projects'in sizin adınıza oluşturduğu Azure kaynaklarını yapılandırmak için **Ek ayarları** seçin.

   7. Projenizi oluşturmayı bitirmek için **Bitti'yi** seçin.

   ![Uygulamanın adve oluşturma](media/how-to-devops-project/select-devops.png)

Birkaç dakika sonra DevOps Projects panosu Azure portalında görüntülenir. İlerlemeyi görmek için proje adınızı seçin. Sayfayı yenilemeniz gerekebilir. Azure DevOps kuruluşunuzdaki bir depoda örnek bir IoT Edge uygulaması ayarlanır, bir yapı yürütülür ve uygulamanız IoT Edge aygıtına dağıtılır. Bu pano, kod deponuzda, CI/CD ardışık sisteminizde ve Azure'daki uygulamanızda görünürlük sağlar.

   ![Azure portalında uygulamayı görüntüleme](./media/how-to-devops-project/devops-portal.png)

## <a name="commit-code-changes-and-execute-cicd"></a>Kod değişikliklerini işleme ve CI/CD’yi yürütme

DevOps Projects, Azure Deposu'nda projeniz için bir Git deposu oluşturdu. Bu bölümde, depoyu görüntüleyebilir ve uygulamanızda kod değişiklikleri yaparsınız.

1. Projeniz için oluşturulan repo'ya gitmek için proje panonuzun menüsünde **Depolar'ı** seçin.  

   ![Azure Repos'ta oluşturulan depogörüntüleme](./media/how-to-devops-project/view-repositories.png)

2. Kod değişiklikleri yapmak için web tarayıcısını kullanarak aşağıdaki adımlar yürür. Bunun yerine deponuzu yerel olarak klonlamak istiyorsanız, pencerenin sağ üst kısmından **Klon'u** seçin. Görsel Stüdyo Kodu'nda veya tercih ettiğiniz geliştirme aracında Git deponuzu klonlamak için sağlanan URL'yi kullanın.

3. Depo, oluşturma işleminde seçtiğiniz uygulama dilini temel alan **FilterModule** adlı bir modülün kodunu zaten içerir. **Modülleri/FilterModule/module.json** dosyasını açın.

   ![Azure Repos'ta module.json dosyalarını açma](./media/how-to-devops-project/open-module-json.png)

4. Bu dosyanın **sürüm** parametresinde [Azure DevOps yapı değişkenleri](https://docs.microsoft.com/azure/devops/pipelines/build/variables?view=vsts#build-variables) kullandığına dikkat edin. Bu yapılandırma, yeni bir yapı her çalıştığında modülün yeni bir sürümünün oluşturulmasını sağlar.

## <a name="examine-the-cicd-pipeline"></a>CI/CD boru hattını inceleyin

Önceki bölümlerde, Azure DevOps Projeleri Otomatik olarak IoT Edge uygulamanız için tam bir CI/CD ardışık yapılandırıldı. Şimdi, boru hattını gerektiği gibi keşfedin ve özelleştirin. Azure DevOps yapı ve sürüm boru hatlarıhakkında bilgi almak için aşağıdaki adımları kullanın.

1. DevOps projenizdeki yapı ardışık lıklarını görüntülemek için proje panonuzun menüsünde **Yapı Boru Hatları'nı** seçin. Bu bağlantı bir tarayıcı sekmesi açar ve Azure DevOps yeni projeniz için ardışık hatlar oluşturur.

   ![Azure Ardışık Hatlar'da yapı ardışık hatlarını görüntüleme](./media/how-to-devops-project/view-build-pipelines.png)

2. **Edit'i**seçin.

    ![Yapı boru hattını edin](media/how-to-devops-project/click-edit-button.png)

3. Açılan panelde, yapı ardışık hattınız çalıştığında oluşan görevleri inceleyebilirsiniz. Yapı ardışık hattı, Git deposundan kaynak alma, IoT Edge modül görüntüleri oluşturma, IoT Edge modüllerini konteyner kayıt defterine itme ve dağıtımlar için kullanılan çıktıları yayımlama gibi çeşitli görevleri gerçekleştirir. Azure DevOps'lerde Azure IoT Edge görevleri hakkında daha fazla bilgi edinmek [için, sürekli tümleştirme için Azure Altyapı Hatlarını Yapılandır'a](how-to-ci-cd.md#configure-continuous-integration)bakın.

4. Boru hattı ayrıntılarını açmak için yapı ardışık hattının üst kısmındaki **Ardışık Hatlar** üstbilgisini seçin. Derleme işlem hattınızın adını daha açıklayıcı bir şeyle değiştirin.

   ![Boru hattı ayrıntılarını edin](./media/how-to-devops-project/edit-build-pipeline.png)

5. **& sırayı kaydet'i**ve ardından **Kaydet'i**seçin.

6. Yapı ardışık yapı menüsünden **Tetikleyiciler'i** seçin. DevOps Projects otomatik olarak bir CI tetikleyicisi oluşturdu ve depoya her taahhüt yeni bir yapı başlatır.  İsteğe bağlı olarak dalları CI işlemine dahil etmeyi veya işlemden hariç tutmayı seçebilirsiniz.

7. **Saklama**’yı seçin. Senaryonuza bağlı olarak, belirli sayıda yapıyı tutmak veya kaldırmak için ilkeler belirtebilirsiniz.

8. **Geçmiş'i**seçin. Tarih paneli, yapıdaki son değişikliklerin denetim izini içerir. Azure Ardışık Hatları, yapı ardışık yapısında yapılan değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

9. Yapı ardışık hattını keşfetmeyi bitirdiğinizde, ilgili sürüm ardışık hattına gidin. **Ardışık Hatlar**Altında **Sürümler'i** seçin ve ardından ardışık hatlar ayrıntılarını görüntülemek için **Edit'i** seçin.

    ![Sürüm ardışık hattını görüntüleme](media/how-to-devops-project/release-pipeline.png)

10. **Yapıtlar**’ın altında **Bırak**’ı seçin. Bu yapının izlediği kaynak, önceki adımlarda incelediğiniz yapı ardışık alanının çıktısI.

11. **Bırak** simgesinin yanında, yıldırım adedine benzeyen **Sürekli dağıtım tetikleyicisini** seçin. Bu sürüm ardışık alanı, her kullanılabilir yeni yapı artifakı olduğunda dağıtım çalıştıran tetikleyiciyi etkinleştirdi. İsteğe bağlı olarak, dağıtımlarınızın el ile yürütülmesini gerektirecek şekilde tetikleyiciyi devre dışı kullanabilirsiniz.  

12. Sürüm ardışık listenizin menüsünde **Görevler'i** seçin ve açılan listeden **geliştirme** aşamasını seçin. DevOps Projects sizin için bir IoT hub'ı oluşturan, hub'da bir IoT Edge aygıtı oluşturan, yapı ardışık noktasından örnek modülü dağıtan ve IoT Edge aygıtınız olarak çalışacak sanal bir makineyi sağlayan bir sürüm aşaması oluşturdu. CD için Azure IoT Edge görevleri hakkında daha fazla bilgi edinmek için, [sürekli dağıtım için Azure Altyapı Larını Yapılandırma'ya](how-to-ci-cd.md#configure-continuous-deployment)bakın.

    ![Sürekli dağıtım görevlerini görüntüleme](media/how-to-devops-project/dev-release.png)

13. Sağda, görünüm **sürümlerini**seçin. Bu görünümde yayın geçmişi gösterilir.

14. Bu konuda daha fazla bilgi görüntülemek için bir sürümün adını seçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure Uygulama Hizmeti'ni ve oluşturduğunuz diğer ilgili kaynakları artık ihtiyacınız olmadığında silebilirsiniz. DevOps Projeleri panosundaki **Sil** işlevini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* [Sürekli tümleştirme ve Azure IoT Edge'e sürekli dağıtım](how-to-ci-cd.md) da Azure DevOps'teki Azure IoT Edge Görevleri hakkında bilgi edinin
* Tek cihazlar için veya [ölçekte IoT Edge dağıtımlarını anlamada IoT](module-deployment-monitoring.md) Edge dağıtımını anlama
* Dağıtım'da bir dağıtım oluşturmak, güncelleştirmek veya silmek [ve IoT Edge modüllerini ölçekte izlemek](how-to-deploy-monitor.md)için adımları gözden geçirin.
