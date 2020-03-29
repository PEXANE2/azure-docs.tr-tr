---
title: Sürekli tümleştirme & sürekli dağıtım - Azure IoT Edge
description: Sürekli tümleştirme ve sürekli dağıtım ayarlama - Azure DevOps ile Azure IoT Edge, Azure Boru Hatları
author: shizn
manager: philmea
ms.author: xshi
ms.date: 08/20/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 4b99e83a8e71b13183c76321c7076b85a212f021
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76510983"
---
# <a name="continuous-integration-and-continuous-deployment-to-azure-iot-edge"></a>Azure IoT Edge'e sürekli tümleştirme ve sürekli dağıtım

Azure Boru Hatları'ndaki yerleşik Azure IoT Edge görevleriyle DevOps'leri Azure IoT Edge uygulamalarınızla kolayca benimseyebilirsiniz. Bu makalede, Uygulamaları Azure IoT Edge'inize hızlı ve verimli bir şekilde oluşturmak, sınamak ve dağıtmak için Azure Pipelines'ın sürekli tümleştirme ve sürekli dağıtım özelliklerini nasıl kullanabileceğiniz gösterilebilir.

![Diyagram - GELIŞTIRME ve üretim için CI ve CD şubeleri](./media/how-to-ci-cd/cd.png)

Bu makalede, IoT Edge çözümünüz için iki ardışık işlem hattı oluşturmak için Azure Ardışık Hatları için yerleşik Azure IoT Edge görevlerini nasıl kullanacağınızı öğreneceksiniz. Azure IoT Edge görevlerinde kullanılabilecek dört eylem vardır.

* **Azure IoT Edge - Yapı Modülü görüntüleri,** IoT Edge çözüm kodunuzu alır ve kapsayıcı görüntülerini oluşturur.
* **Azure IoT Edge - Push Module görüntüleri** modül görüntülerini belirttiğiniz konteyner kayıt defterine iter.
* **Azure IoT Edge - Deployment Manifest oluştur** dağıtım.template.json dosyasını ve değişkenleri alır, ardından son IoT Edge dağıtım bildirimi dosyasını oluşturur.
* **Azure IoT Edge - IoT Edge aygıtlarına dağıtma,** tek/birden çok IoT Edge aygıtında IoT Edge dağıtımları oluşturulmasına yardımcı olur.

## <a name="prerequisites"></a>Ön koşullar

* Azure Deposu deposu. Hesabınız yoksa, [projenizde yeni bir Git repo'su oluşturabilirsiniz.](https://docs.microsoft.com/azure/devops/repos/git/create-new-repo?view=vsts&tabs=new-nav)
* Bir IoT Edge çözümü taahhüt ve deponuza itti. Bu makaleyi test etmek için yeni bir örnek çözüm oluşturmak istiyorsanız, [Visual Studio Code'daki Geliştirme ve hata ayıklama modülleri](how-to-vs-code-develop-module.md) adımlarını izleyin veya [Visual Studio'da C# modüllerini geliştirin ve hata ayıklayın.](how-to-visual-studio-develop-csharp-module.md)

   Bu makale için tek ihtiyacınız olan Visual Studio Code veya Visual Studio'daki IoT Edge şablonları tarafından oluşturulan çözüm klasörüdür. Devam etmeden önce bu kodu oluşturmanız, itmeniz, dağıtmanız veya hata ayıklamanız gerekmez. Bu işlemleri Azure Pipelines'da ayarlarsınız.

   Yeni bir çözüm oluşturuyorsanız, önce deponuzu yerel olarak kopyalayın. Ardından, çözümü oluşturduğunuzda, çözümü doğrudan depo klasöründe oluşturmayı seçebilirsiniz. Yeni dosyaları kolayca işleyebilir ve itebilirsiniz.

* Modül görüntülerini itebileceğiniz bir konteyner kayıt defteri. [Azure Kapsayıcı Kayıt Defteri'ni](https://docs.microsoft.com/azure/container-registry/) veya bir üçüncü taraf kayıt defterini kullanabilirsiniz.
* Ayrı test ve üretim dağıtım aşamalarını test etmek için en az IoT Edge aygıtları içeren etkin bir [IoT](../iot-hub/iot-hub-create-through-portal.md) hub'ı. [Linux](quickstart-linux.md) veya [Windows'da](quickstart.md) bir IoT Edge aygıtı oluşturmak için hızlı başlangıç makalelerini takip edebilirsiniz

Azure Repos'u kullanma hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/devops/repos/git/share-your-code-in-git-vs?view=vsts)

## <a name="configure-continuous-integration"></a>Sürekli tümleştirme yapılandırması

Bu bölümde, yeni bir yapı ardışık alanı oluşturursunuz. Örnek IoT Edge çözümünde yapılan değişiklikleri iade ettiğinizde ardışık yapıyı otomatik olarak çalışacak şekilde yapılandırın ve yapı günlükleri yayımlayın.

>[!NOTE]
>Bu makalede, Azure DevOps görsel tasarımcısı kullanır. Bu bölümdeki adımları izlemeden önce, yeni YAML ardışık hat lar oluşturma deneyimi için önizleme özelliğini kapatın.
>
>1. Azure DevOps'de profil simgenizi seçin ve **ardından Öngösterim özelliklerini**seçin.
>2. **Yeni YAML boru hattı oluşturma deneyimini** kapatın.
>
>Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/devops/pipelines/create-first-pipeline)

1. Azure DevOps kuruluşunuzda oturum açın **(https:\//dev.azure.com/{kuruluşunuz}/**) ve IoT Edge çözüm deponuzu içeren projeyi açın.

   Bu makale için, **IoTEdgeRepo**adlı bir depo oluşturduk. Bu depo, **filtre modülü**adlı bir modülün koduna sahip **IoTEdgeSolution'ı** içerir.

   ![DevOps projenizi açın](./media/how-to-ci-cd/init-project.png)

2. Projenizde Azure Ardışık Hatları'na gidin. **Yapılar** sekmesini açın ve **Yeni ardışık hatlar'ı**seçin. Veya yapı ardışık hatlarınız zaten varsa **Yeni** düğmesini seçin. Sonra **Yeni yapı boru hattı**seçin.

    ![Yeni derleme işlem hattı oluşturma](./media/how-to-ci-cd/add-new-build.png)

3. Ardışık hattınızı oluşturmak için istemleri izleyin.

   1. Yeni yapı ardınız için kaynak bilgilerini sağlayın. Kaynak olarak **Azure Depogit'i** seçin ve ardından IoT Edge çözüm kodunun bulunduğu projeyi, depoyu ve dala seçin. Ardından Devam **et'i**seçin.

      ![Boru hattı kaynağınızı seçin](./media/how-to-ci-cd/pipeline-source.png)

   2. Şablon yerine **Boş iş'i** seçin.

      ![Boş bir işlemle başlayın](./media/how-to-ci-cd/start-with-empty.png)

4. Ardışık işlem yerinizi oluşturduktan sonra, boru hattı düzenleyicisine götürülürnüz. Boru hattı açıklamanızda, hedef platformunuzu temel alan doğru aracı havuzunu seçin:

   * Linux konteynerleri için amd64 platformunda modüllerinizi oluşturmak istiyorsanız, **Hosted Ubuntu 1604'ü** seçin

   * Modüllerinizi Windows 1809 kapsayıcıları için amd64 platformunda oluşturmak istiyorsanız, [Windows'da kendi kendine barındırılan aracıyı kurmanız](https://docs.microsoft.com/azure/devops/pipelines/agents/v2-windows?view=vsts)gerekir.

   * Linux konteynerleri için platform arm32v7 veya arm64 modüllerinizi oluşturmak istiyorsanız, [Linux'ta kendi kendine barındırılan bir temsilci kurmanız](https://blogs.msdn.microsoft.com/iotdev/2018/11/13/setup-azure-iot-edge-ci-cd-pipeline-with-arm-agent/)gerekir.

     ![Yapı aracı havuzuyapıla](./media/how-to-ci-cd/configure-env.png)

5. Boru hattınız **Ajan iş 1**adlı bir iş ile önceden yapılandırılmış geliyor. İşe üç görev**+** eklemek için artı işaretini ( ) seçin: **Azure IoT Edge** iki kez, Dosyaları bir kez **Kopyala** ve Yapı Yapı Larını bir kez **Yayımla.** (Ekle düğmesini görmek için her görevin adının üzerine takla **at.)**

   ![Azure IoT Edge görev ekle](./media/how-to-ci-cd/add-iot-edge-task.png)

   Dört görev de eklendiğinde, Aracı işin aşağıdaki örnek gibi görünür:

   ![Yapı ardışık alanında üç görev](./media/how-to-ci-cd/add-tasks.png)

6. Bunu sağlamak için ilk **Azure IoT Edge** görevini seçin. Bu görev, belirttiğiniz hedef platformu ile çözümdeki tüm modülleri oluşturur.

   * **Ekran adı**: Varsayılan **Azure IoT Edge - Modül görüntüleri oluşturmayı**kabul edin.
   * **Eylem**: Varsayılan **Yapı modülü görüntülerini**kabul edin.
   * **.template.json dosyası**: Elipsis 'i (**...**) seçin ve IoT Edge çözümünüzü içeren depodaki **deployment.template.json** dosyasına gidin.
   * **Varsayılan platform**: Hedef IoT Edge cihazınızı temel alan modülleriniz için uygun platformu seçin.
   * **Çıktı değişkenleri**: Çıktı değişkenleri, deployment.json dosyanızın oluşturulacağı dosya yolunu yapılandırmak için kullanabileceğiniz bir başvuru adı içerir. Referans adını **kenar**gibi unutulmaz bir şeye ayarlayın.

7. Bunu sağlamak için ikinci **Azure IoT Edge** görevini seçin. Bu görev, tüm modül görüntülerini seçtiğiniz kapsayıcı kayıt defterine iter.

   * **Ekran adı**: Eylem alanı değiştiğinde görüntü adı otomatik olarak güncelleştirilir.
   * **Eylem**: **Push modül görüntülerini**seçmek için açılır listeyi kullanın.
   * **Konteyner kayıt türü**: Modül resimlerinizi depolamak için kullandığınız konteyner kayıt defteri türünü seçin. Seçtiğiniz kayıt türüne bağlı olarak, form değişir. **Azure Kapsayıcı Kayıt Defteri'ni**seçerseniz, Azure aboneliğini ve konteyner kayıt defterinizin adını seçmek için açılır listelerini kullanın. **Genel Kapsayıcı Kayıt Defteri'ni**seçerseniz, kayıt defteri hizmeti bağlantısı oluşturmak için **Yeni'yi** seçin.
   * **.template.json dosyası**: Elipsis 'i (**...**) seçin ve IoT Edge çözümünüzü içeren depodaki **deployment.template.json** dosyasına gidin.
   * **Varsayılan platform**: Yerleşik modül görüntüleriyle aynı platformu seçin.

   Modül görüntülerinizi barındırmak için birden çok kapsayıcı kayıt defteriniz varsa, bu görevi yinelemeniz, farklı konteyner kayıt defterini seçmeniz ve bu özel kayıt defterine uygun olmayan görüntüleri atlamak için gelişmiş ayarlarda **Bypass modülünü(ler)** kullanmanız gerekir.

8. Bunu sağlamak için **Dosyaları Kopyala** görevini seçin. Yapı düzenleme dizinine dosyaları kopyalamak için bu görevi kullanın.

   * **Ekran adı**: Dosyaları kopyala: Bırak klasörü.
   * **İçindekiler**: Bu bölüme iki satır koyun `deployment.template.json` ve `**/module.json`. Bu iki dosya türü, IoT Edge dağıtım bildirimini oluşturmak için girişlerdir. Yapı hazırlama klasörüne kopyalanıp serbest bırakma ardışık alanı için yayımlanmalıdır.
   * **Hedef Klasör**: `$(Build.ArtifactStagingDirectory)`Değişkeni koyun. Açıklama hakkında bilgi edinmek için [değişkenoluştur'a](https://docs.microsoft.com/azure/devops/pipelines/build/variables?view=azure-devops&tabs=yaml#build-variables) bakın.

9. Yapı **Yapı Oluşturma** görevini, onu yeniden oluşturmak için seçin. Yolun ardışık düzeneak yayımlamak için yayımlanabilmesi için göreve yapı düzenleme dizini yolunu sağlayın.

   * **Görüntü adı**: Artefakt Yayımla: bırak.
   * **Yayımlanacak Yol**: `$(Build.ArtifactStagingDirectory)`Değişkeni koyun. Açıklama hakkında bilgi edinmek için [değişkenoluştur'a](https://docs.microsoft.com/azure/devops/pipelines/build/variables?view=azure-devops&tabs=yaml#build-variables) bakın.
   * **Eser adı**: damla.
   * **Artefakt yayımlama yeri**: Azure Pipelines.

10. **Tetikleyiciler** sekmesini açın ve **sürekli tümleştirmeyi etkinleştirmek**için kutuyu işaretleyin. Kodunuzu içeren dakarın dahil olduğundan emin olun.

    ![Sürekli tümleştirme tetikleyicisi açma](./media/how-to-ci-cd/configure-trigger.png)

11. Kaydet düğmesiyle yeni yapı ardışık hattını **kaydedin.**

Bu ardışık işlem, artık repo'nuza yeni kod itdiğinizde otomatik olarak çalışacak şekilde yapılandırıldı. Son görev, ardışık yapıtlarını yayımlama, bir sürüm ardışık tetikler. Sürüm ardışık hattını oluşturmak için bir sonraki bölüme devam edin.

## <a name="configure-continuous-deployment"></a>Sürekli dağıtımı yapılandırma

Bu bölümde, yapı ardınız yapıları nız yapılınca otomatik olarak çalışacak şekilde yapılandırılan bir sürüm ardışık alanı oluşturursunuz ve bu işlem Azure Ardışık Hatları'nda dağıtım günlüklerini gösterir.

Yeni bir ardışık hat lar oluşturun ve yeni bir aşama ekleyin

1. Serbest **Bırakmalar** sekmesinde **+ Yeni ardışık hattı**seçin. Veya, zaten sürüm ardışık hatları varsa, **+ Yeni** düğmesini seçin ve + Yeni **sürüm ardışık seçin.**  

    ![Sürüm ardışık hattı ekleme](./media/how-to-ci-cd/add-release-pipeline.png)

2. Bir şablon seçmek istendiğinde, Boş bir **iş**ile başlamayı seçin.

    ![Boş bir işle başlayın](./media/how-to-ci-cd/start-with-empty-job.png)

3. Yeni sürüm ardışık **hattınız, Aşama 1**olarak adlandırılan bir aşamayla başharflere daldı. Aşama 1'i bir test ortamı olarak **yeniden** adlandırın ve bir test ortamı olarak ele alayın. Genellikle, sürekli dağıtım boru hatları **dev,** evreleme ve **prod**dahil olmak üzere birden çok aşamaya **sahiptir.** DevOps uygulamanıza göre daha fazlasını oluşturabilirsiniz. Yeniden adlandırıldıktan sonra sahne ayrıntıları penceresini kapatın.

4. Sürümü yapı ardışık tarafından yayımlanan yapı yapı larına bağla. Yapıtlar alanına **Ekle'yi** tıklatın.

   ![Yapıtları ekleme](./media/how-to-ci-cd/add-artifacts.png)  

5. **Yapı sayfası ekle'de**kaynak türü **Oluştur'u**seçin. Ardından, projeyi ve oluşturduğunuz yapı ardışık hattını seçin. Ardından **Ekle'yi**seçin.

   ![Derleme yapıtı ekleme](./media/how-to-ci-cd/add-an-artifact.png)

6. Yapı tetikleyicilerini açın ve sürekli dağıtım tetikleyicisini etkinleştirmek için geçiş seçeneğini belirleyin. Şimdi, yeni bir yapı kullanılabilir her zaman yeni bir sürüm oluşturulur.

   ![Sürekli dağıtım tetikleyicisi yapılandırma](./media/how-to-ci-cd/add-a-trigger.png)

7. **Dev** aşaması bir iş ve sıfır görevlerle önceden yapılandırılmıştır. Ardışık iş tonu menüsünden **Görevler'i** seçin ve ardından **geliştirme** aşamasını seçin.  Bu aşamada görevleri yapılandırmak için iş ve görev sayısını seçin.

    ![Dev görevlerini yapılandırma](./media/how-to-ci-cd/view-stage-tasks.png)

8. **Dev** aşamasında, varsayılan bir **Aracı işi**görmeniz gerekir. Aracı işi yle ilgili ayrıntıları yapılandırabilirsiniz, ancak dağıtım görevi platform duyarsızolduğundan, **Etkinlis havuzunda** (veya sizin yönettiğiniz başka bir aracıyı) **Barındırılan VS2017** veya **Barındırılan Ubuntu 1604'ü** kullanabilirsiniz.

9. İki görev eklemek**+** için artı işaretini ( ) seçin. **Azure IoT Edge'i** iki kez arayın ve ekleyin.

    ![Dev için görev ekleme](./media/how-to-ci-cd/add-task-qa.png)

10. İlk **Azure IoT Edge** görevini seçin ve aşağıdaki değerlerle yapılandırın:

    * **Ekran adı**: Eylem alanı değiştiğinde görüntü adı otomatik olarak güncelleştirilir.
    * **Eylem**: **Dağıtım bildirimi oluştur'u**seçmek için açılır listeyi kullanın. Eylem değerini değiştirmek, görev görüntüleme adını eşleşecek şekilde de güncelleştirir.
    * **.template.json dosyası**: `$(System.DefaultWorkingDirectory)/Drop/drop/deployment.template.json`Yolu koyun. Yol yapı ardışık hattından yayımlanır.
    * **Varsayılan platform**: Modül görüntülerini yaparken aynı değeri seçin.
    * **Çıkış yolu**: `$(System.DefaultWorkingDirectory)/Drop/drop/configs/deployment.json`Yolu koyun. Bu yol son IoT Edge dağıtım bildirimi dosyasıdır.

    Bu yapılandırmalar, dosyadaki modül görüntü `deployment.template.json` URL'lerinin değiştirilmesine yardımcı olur. **Dağıtım Oluştur bildirimi,** değişkenlerin dosyada tanımladığınız tam `deployment.template.json` değerle değiştirilmesine de yardımcı olur. VS/VS Kodu'nda, bir `.env` dosyadaki gerçek değeri belirtirsiniz. Azure Ardışık Kodlar'da, Sürüm Ardışık Ardışık Birim Değişkenler sekmesinde değeri ayarlarsınız. Değişkenler sekmesine taşıyın ve Ad ve Değeri aşağıdaki şekilde yapılandırın.

    * **ACR_ADDRESS**: Azure Konteyner Kayıt Defteri adresiniz.
    * **ACR_PASSWORD**: Azure Konteyner Kayıt Defteri şifreniz.
    * **ACR_USER**: Azure Konteyner Kayıt Defteri kullanıcı adınız.

    Projenizde başka değişkenler varsa, bu sekmede adı ve değeri belirtebilirsiniz. **Dağıtım Oluştur bildirimi** yalnızca değişkenlerin `${VARIABLE}` tadında olduğunu tanıyabilir ve bunu `*.template.json` dosyalarınızda kullandığınızdan emin olun.

    ![Sürüm ardışık hattı için değişkenleri yapılandırma](./media/how-to-ci-cd/configure-variables.png)

11. İkinci **Azure IoT Edge** görevini seçin ve aşağıdaki değerlerle yapılandırın:

    * **Ekran adı**: Eylem alanı değiştiğinde görüntü adı otomatik olarak güncelleştirilir.
    * **Eylem**: **IoT Edge aygıtlarına Dağıt'ı**seçmek için açılır listeyi kullanın. Eylem değerini değiştirmek, görev görüntüleme adını eşleşecek şekilde de güncelleştirir.
    * **Azure aboneliği**: IoT Hub'ınızı içeren aboneliği seçin.
    * **IoT Hub adı : IoT**hub'ınızı seçin.
    * **Tek/çoklu aygıt seçin**: Sürüm ardışık hattının tek bir aygıta mı yoksa birden çok aygıta mı dağıtılmasını istediğinizi seçin.
      * Tek bir aygıta dağıtıyorsanız, **IoT Edge aygıt kimliğini**girin.
      * Birden çok aygıta dağıtım yapıyorsunuz, aygıt **hedef koşulunu**belirtin. Hedef koşul, IoT Hub'daki bir dizi IoT Edge aygıtıyla eşleşen bir filtredir. Durum olarak Aygıt Etiketleri'ni kullanmak istiyorsanız, ilgili aygıtlarınızı Güncelleştirmeniz Gerekir EtiketlerI IoT Hub aygıtı ikizi ile. Gelişmiş ayarlarda **IoT Edge dağıtım kimliğini** ve **IoT Edge dağıtım önceliğini** güncelleştirin. Birden çok aygıt için dağıtım oluşturma hakkında daha fazla bilgi için Bkz. [IoT Edge otomatik dağıtımları anlayın.](module-deployment-monitoring.md)
    * Gelişmiş Ayarları genişletin, **IoT Edge**dağıtım `$(System.TeamProject)-$(Release.EnvironmentName)`kimliği seçin, değişkeni koyun. Bu, proje ve sürüm adını IoT Edge dağıtım kimliğinizle eşler.

12. Değişikliklerinizi yeni sürüm ardışık hattına kaydetmek için **Kaydet'i** seçin. Menüden **Pipeline'ı** seçerek ardışık hatlar görünümüne dönün.

## <a name="verify-iot-edge-cicd-with-the-build-and-release-pipelines"></a>IoT Edge CI/CD'yi yapı ve sürüm boru hatlarıyla doğrulayın

Bir yapı işini tetiklemek için, bir commit to kaynak kodu deposuna itebilir veya el ile tetikleyebilirsiniz. Bu bölümde, çalıştığını sınamak için CI/CD ardışık hattını el ile tetiklersiniz. Ardından, dağıtımın başarılı olduğunu doğrulayın.

1. Bu makalenin başında oluşturduğunuz yapı ardışık bölümüne gidin.

2. Aşağıdaki ekran görüntüsünde olduğu gibi **Sıra** düğmesini seçerek yapı ardışık ardışık sisteminizde bir yapı işini tetikleyebilirsiniz.

    ![Manuel tetikleme](./media/how-to-ci-cd/manual-trigger.png)

3. İlerlemesini izlemek için yapı işini seçin. Yapı ardışık hattı başarıyla tamamlanırsa, **geliştirme** aşamasına bir sürümü tetikler.

    ![Günlükler oluşturma](./media/how-to-ci-cd/build-logs.png)

4. Başarılı **dev** sürümü, IoT Edge aygıtlarını hedeflemek için IoT Edge dağıtımı oluşturur.

    ![Dev'e bırakın](./media/how-to-ci-cd/pending-approval.png)

5. Sürüm günlüklerini görmek için **geliştirme** aşamasını tıklatın.

    ![Yayın günlükleri](./media/how-to-ci-cd/release-logs.png)

## <a name="next-steps"></a>Sonraki adımlar

* IoT Edge Edge [için Azure DevOps Projesi'nde IoT](how-to-devops-project.md) Edge DevOps en iyi uygulamalar örneği
* Tek cihazlar için veya [ölçekte IoT Edge dağıtımlarını anlamada IoT](module-deployment-monitoring.md) Edge dağıtımını anlama
* Dağıtım'da bir dağıtım oluşturmak, güncelleştirmek veya silmek [ve IoT Edge modüllerini ölçekte izlemek](how-to-deploy-monitor.md)için adımları gözden geçirin.
