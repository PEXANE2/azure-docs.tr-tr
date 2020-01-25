---
title: Azure DevOps Hizmetleri - Team Data Science Process ile veri bilimi kodu test edin
description: Veri bilimi kod ile Team Data Science Process UCI yetişkinlere yönelik gelir tahmin veri kümesi ile Azure ve Azure DevOps hizmetleriyle test etme
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=weig, previous-ms.author=weig
ms.openlocfilehash: 9612114bb368898ccf31b2c8692869b84544b652
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76722065"
---
# <a name="data-science-code-testing-on-azure-with-the-team-data-science-process-and-azure-devops-services"></a>Team Data Science Process ile Azure ve Azure DevOps hizmetleriyle sınama veri bilimi kodu
Bu makalede, kodu test etmek için bir veri bilimi iş akışında başlangıç yönergeleri sağlar. Veri bilimcileri, bu tür bir testi beklenen sonuç kodlarını ve kalite kontrol etmek için sistematik ve etkili bir yol sağlar. Team Data Science işlem (TDSP) kullanıyoruz [UCI yetişkinlere yönelik gelir veri kümesini kullanan proje](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) biz nasıl kodu test yapılabilir göstermek için daha önce yayımlanmış. 

## <a name="introduction-on-code-testing"></a>Sınama kodu giriş
"Birim testi", yazılım geliştirme çalışmalarını uzun süredir davam bir uygulamadır. Ancak veri bilimi için, "birim testi" ne anlama geldiğini ve bir veri bilimi yaşam döngüsünün farklı aşamaları için kodu test etmeyi nasıl test etmemelisiniz, örneğin:

* Veri hazırlama
* Veri Kalitesi İnceleme
* Modelleme
* Model dağıtımı 

Bu makale, "" kod testiyle."birim testi" terimi yerine geçer Belirli bir adımında bir veri bilimi yaşam döngüsü için kod, "bekleniyor." olarak sonuçları üreten, değerlendirmek için yardımcı işlevleri test başvuruyor Test "beklendiği gibi" nedir tanımlar bağlı olarak işlevi--sonucunu örneğin yazıyor kişiye, veri kalite denetimi veya model.

Bu makalede, başvuru faydalı kaynaklar sağlar.

## <a name="azure-devops-for-the-testing-framework"></a>Azure DevOps için test çerçevesi
Bu makalede, Azure DevOps kullanarak testi otomatikleştirme ve gerçekleştirmek açıklar. Diğer araçları kullanmaya karar verebilirsiniz. Azure DevOps kullanarak otomatik bir derlemeyi Ayarla ve yapı aracılarını nasıl da gösteriyoruz. Derleme aracıları için Azure veri bilimi sanal makineleri (Dsvm'leri) kullanıyoruz.

## <a name="flow-of-code-testing"></a>Kodu test akışı
Genel iş akışını bir veri bilimi proje test kod şöyle görünür: 

![Kodu test akış çizelgesi](./media/code-test/test-flow-chart.PNG)

    
## <a name="detailed-steps"></a>Ayrıntılı adımlar

Ayarlanmış ve bir yapı aracısı ve Azure DevOps kullanarak kodu test etme ve otomatik bir yapı çalıştırmak için aşağıdaki adımları kullanın:

1. Visual Studio masaüstü uygulaması, bir proje oluşturun:

    ![Visual Studio'da "Yeni Proje oluştur" Ekran](./media/code-test/create_project.PNG)

   Projenizi oluşturduktan sonra sağ bölmeden Çözüm Gezgini'nde bulabilirsiniz:
    
    ![Bir proje oluşturma adımları](./media/code-test/create_python_project_in_vs.PNG)

    ![Çözüm Gezgini](./media/code-test/solution_explorer_in_vs.PNG)

1. Proje kodunuzu Azure DevOps projesi kod deposuna akış: 

    ![Proje kod deposu](./media/code-test/create_repo.PNG)

1. Veri alımı, özellik Mühendisliği ve etiket sütun oluşturma gibi bazı veri hazırlama çalışmalarınız yaptığınızı varsayalım. Kodunuzu beklediğiniz sonuçlar üretiyor emin olmanız gerekir. Veri işleme kodunu doğru şekilde çalışıp çalışmadığını sınamak için kullanabileceğiniz bazı kodlar aşağıda verilmiştir:

    * Sütun adlarının doğru olduğunu kontrol edin:
    
      ![Sütun adlarını eşleştirmek için kod](./media/code-test/check_column_names.PNG)

    * Yanıt düzeyleri doğru olduğundan emin olun:

      ![Düzeylerini eşleştirmek için kod](./media/code-test/check_response_levels.PNG)

    * Yanıt yüzdesi makul olup olmadığını denetleyin:

      ![Kod yanıt yüzdesi](./media/code-test/check_response_percentage.PNG)

    * Her bir sütunun veri eksik oranı denetleyin:
    
      ![Kod için eksik oranı](./media/code-test/check_missing_rate.PNG)


1. Veri işleme ve mühendislik özellik çalışmalarına yaptıktan ve iyi bir modeli eğittiğimize sonra eğitilen modeli yeni veri kümelerini doğru Puanlama emin emin olun. Aşağıdaki iki sınama için öngörü düzeyleri ve etiket değerlerini dağıtımını kontrol etmek için kullanabilirsiniz:

    * Öngörü düzeyleri denetleyin:
    
      ![Kod öngörü düzeyleri](./media/code-test/check_prediction_levels.PNG)

    * Dağıtım tahmin değerleri kontrol edin:

      ![Kod tahmin değerleri](./media/code-test/check_prediction_values.PNG)

1. Tüm test işlevleri birlikte adlı bir Python komut dosyasına put **test_funcs.py**:

    ![Test işlevleri için Python betiği](./media/code-test/create_file_test_func.PNG)


1. Test kodları hazırladıktan sonra Visual Studio test ortamında ayarlayabilirsiniz.

   Adlı bir Python dosyası oluşturun **test1.py**. Bu dosyada, yapmak istediğiniz tüm testleri içeren bir sınıf oluşturun. Aşağıdaki örnek, altı testleri hazırlanmış gösterir:
    
    ![Bir sınıftaki testleri listesiyle soubor Pythonu](./media/code-test/create_file_test1_class.PNG)

1. Koyarsanız bu testler otomatik olarak bulunabileceğini **codetest.testCase** sonra sınıf adı. Sağ bölmede Test Gezgini'ni açın ve seçin **tümünü Çalıştır**. Tüm testler sırayla çalışır ve testin başarılı olup olmadığını size bildirir.

    ![Testleri çalıştırma](./media/code-test/run_tests.PNG)

1. Proje deposu için Git komutlarını kullanarak iade edin. En son işi, kısa süre içinde Azure DevOps yansıtılır.

    ![Kodu iade etme Git komutları](./media/code-test/git_check_in.PNG)

    ![Azure DevOps en son iş](./media/code-test/git_check_in_most_recent_work.PNG)

1. Otomatik yapı ayarlayın ve Azure DevOps test:

    a. Proje deposu seçin **derleme ve yayın**ve ardından **+ yeni** yeni bir yapı işlemi oluşturmak için.

    ![Yeni derleme işlemi başlatma seçimleri](./media/code-test/create_new_build.PNG)

    b. Kaynak kodu konumu, proje adı, depo ve dal bilgilerini seçmek için yönergeleri izleyin.
    
    ![Kaynak, ad, depo ve dal bilgileri](./media/code-test/fill_in_build_info.PNG)

    c. Bir şablon seçin. Hiçbir Python proje şablonu olduğundan seçerek başlayın **boş işlem**. 

    ![Şablonların listesi ve "boş işlem" düğmesi](./media/code-test/start_empty_process_template.PNG)

    d. Derleme adı ve Aracı'nı seçin. Derleme işlemini gerçekleştirmek için DSVM kullanmak istiyorsanız, varsayılan olarak bu seçeneği belirleyebilirsiniz. Ayar aracıları hakkında daha fazla bilgi için bkz. [derleme ve yayın aracıları](https://docs.microsoft.com/azure/devops/pipelines/agents/agents?view=vsts).
    
    ![Yapı ve aracı seçimleri](./media/code-test/select_agent.PNG)

    e. Seçin **+** bu derleme aşaması için bir görev eklemek için sol bölmedeki. Tüm denetimleri tamamlaması için Python betiğini **test1.py** çalıştıracağız, bu görev Python kodunu çalıştırmak Için bir PowerShell komutu kullanıyor.
    
    ![PowerShell seçiliyken "görev ekleme" bölmesi](./media/code-test/add_task_powershell.PNG)

    f. PowerShell ayrıntılarında adı ve PowerShell sürümü gibi gerekli bilgileri doldurun. Seçin **satır içi betik** türü. 
    
    **Satır Içi betik**altındaki kutuya **Python test1.py**yazabilirsiniz. Ortam değişkeninin Python için doğru şekilde ayarlandığından emin olun. Python 'un farklı bir sürümüne veya çekirdeğe ihtiyacınız varsa, yolu şekilde gösterildiği gibi açıkça belirtebilirsiniz: 
    
    ![PowerShell ayrıntıları](./media/code-test/powershell_scripts.PNG)

    g. Derleme ardışık düzeni işlemini gerçekleştirmek için **& kuyruğu kaydet** ' i seçin.

    !["& Kuyruğunu kaydet" düğmesi](./media/code-test/save_and_queue_build_definition.PNG)

Artık her seferinde yeni bir işleme kodu depoya gönderildiğinde, yapı işlemi otomatik olarak başlatılacak. (Burada depo olarak ana öğe kullanıyoruz, ancak herhangi bir dalı tanımlayabilirsiniz.) İşlem, kodda tanımlanan her şeyin düzgün çalıştığından emin olmak için aracı makinesinde **test1.py** dosyasını çalıştırır. 

Uyarılar doğru şekilde ayarlanmışsa, derleme tamamlandığında e-posta ile bildirilir. Ayrıca, Azure DevOps içindeki derleme durumunu kontrol edebilirsiniz. Başarısız olursa, derleme ayrıntılarını kontrol edin ve hangi parçanın bozuk olduğunu öğrenin.

![Yapı Başarı e-posta bildirimi](./media/code-test/email_build_succeed.PNG)

![Azure DevOps bildirimi derleme başarısı](./media/code-test/vs_online_build_succeed.PNG)

## <a name="next-steps"></a>Sonraki adımlar
* Bkz: [UCI gelir tahmin depo](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) veri bilimi senaryoları için birim testleri somut örnekleri için.
* Önceki anahat ve kendi veri bilimi projeleri UCI gelir tahmin senaryoda örneklerden izleyin.

## <a name="references"></a>Başvurular
* [Team Data Science Process](https://aka.ms/tdsp)
* [Visual Studio Test Araçları](https://www.visualstudio.com/vs/features/testing-tools/)
* [Azure DevOps test kaynakları](https://www.visualstudio.com/team-services/)
* [Veri bilimi sanal makineleri](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/)
