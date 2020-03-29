---
title: Azure DevOps Hizmetleri ile test veri bilimi kodu - Ekip Veri Bilimi Süreci
description: Team Data Science Process ve Azure DevOps Hizmetleri ile UCI yetişkin gelir tahmini veri seti ile Azure'da veri bilimi kodu testi
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76722065"
---
# <a name="data-science-code-testing-on-azure-with-the-team-data-science-process-and-azure-devops-services"></a>Ekip Veri Bilimi Süreci ve Azure DevOps Hizmetleri ile Azure'da veri bilimi kodu testi
Bu makalede, bir veri bilimi iş akışında kod test etmek için ön yönergeler verir. Bu tür testler, veri bilimcilere kodlarının kalitesini ve beklenen sonucunu kontrol etmek için sistematik ve verimli bir yol sağlar. Kod testinin nasıl yapılabileceğini göstermek için daha önce yayınladığımız [UCI Yetişkin Gelir veri kümesini kullanan](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) bir Ekip Veri Bilimi Süreci (TDSP) projesini kullanıyoruz. 

## <a name="introduction-on-code-testing"></a>Kod testine giriş
"Birim testi" yazılım geliştirme için uzun süredir devam eden bir uygulamadır. Ancak veri bilimi için, "birim testi"nin ne anlama geldiğini ve bir veri bilimi yaşam döngüsünün farklı aşamaları için kodu nasıl test etmeniz gerektiği genellikle açık değildir:

* Veri hazırlama
* Veri kalitesi incelemesi
* Modelleme
* Model dağıtımı 

Bu makale, "birim testi" teriminin yerine "kod testi" ile değiştirilir. Bir veri bilimi yaşam döngüsünün belirli bir adımı için kod "beklendiği gibi" sonuçlar üreten olup olmadığını değerlendirmek için yardımcı işlevleri olarak test anlamına gelir. Testi yazan kişi, işlevin sonucuna bağlı olarak (örneğin, veri kalitesi denetimi veya modelleme) "beklendiği gibi" olanı tanımlar.

Bu makalede yararlı kaynaklar olarak başvurular sağlar.

## <a name="azure-devops-for-the-testing-framework"></a>Test çerçevesi için Azure DevOps
Bu makalede, Azure DevOps kullanarak sınama nın nasıl gerçekleştirilip otomatikleştirilen anlatılmaktadır. Alternatif araçlar kullanmaya karar verebilirsiniz. Ayrıca, Azure DevOps'leri kullanarak otomatik bir yapının nasıl ayarlanır ve aracılar oluşturabileceğimizi de gösteririz. Yapı aracıları için Azure Veri Bilimi Sanal Makineleri (DSVM) kullanıyoruz.

## <a name="flow-of-code-testing"></a>Kod testi akışı
Bir veri bilimi projesinde test kodunun genel iş akışı aşağıdaki gibi görünür: 

![Kod testinin akış şeması](./media/code-test/test-flow-chart.PNG)

    
## <a name="detailed-steps"></a>Ayrıntılı adımlar

Bir yapı aracısı ve Azure DevOps kullanarak kod testi ve otomatik yapı yı kurmak ve çalıştırmak için aşağıdaki adımları kullanın:

1. Visual Studio masaüstü uygulamasında bir proje oluşturun:

    ![Visual Studio'da "yeni proje oluşturma" ekranı](./media/code-test/create_project.PNG)

   Projenizi oluşturduktan sonra, projeyi çözüm gezgininde sağ bölmede bulabilirsiniz:
    
    ![Proje oluşturma adımları](./media/code-test/create_python_project_in_vs.PNG)

    ![Çözüm Gezgini](./media/code-test/solution_explorer_in_vs.PNG)

1. Proje kodunuzu Azure DevOps proje kodu deposuna taşıyın: 

    ![Proje kodu deposu](./media/code-test/create_repo.PNG)

1. Veri alma, özellik mühendisliği ve etiket sütunları oluşturma gibi bazı veri hazırlama çalışmaları yaptığınızı varsayalım. Kodunuzun beklediğiniz sonuçları ürettiğinden emin olmak istiyorsunuz. Veri işleme kodunun düzgün çalışıp çalışmadığını sınamak için kullanabileceğiniz bazı kodlar aşağıda verebilirsiniz:

    * Sütun adlarının doğru olup olmadığını kontrol edin:
    
      ![Eşleşen sütun adları için kod](./media/code-test/check_column_names.PNG)

    * Yanıt düzeylerinin doğru olup olmadığını kontrol edin:

      ![Eşleşen düzeyleri için kod](./media/code-test/check_response_levels.PNG)

    * Yanıt yüzdenin makul olup olmadığını kontrol edin:

      ![Yanıt yüzdesi için kod](./media/code-test/check_response_percentage.PNG)

    * Verilerdeki her sütunun eksik oranını denetleyin:
    
      ![Eksik oran kodu](./media/code-test/check_missing_rate.PNG)


1. Veri işleme ve özellik mühendisliği işini yaptıktan ve iyi bir model eğittikten sonra, eğittiğiniz modelin yeni veri kümelerini doğru şekilde puanlayabilmesini unutmayın. Tahmin düzeylerini ve etiket değerlerinin dağılımını denetlemek için aşağıdaki iki testi kullanabilirsiniz:

    * Tahmin düzeylerini kontrol edin:
    
      ![Tahmin düzeylerini denetleme kodu](./media/code-test/check_prediction_levels.PNG)

    * Tahmin değerlerinin dağılımını denetleyin:

      ![Tahmin değerlerini denetleme kodu](./media/code-test/check_prediction_values.PNG)

1. Tüm test **işlevlerini test_funcs.py**adlı bir Python komut dosyasına koyun:

    ![Test işlevleri için Python komut dosyası](./media/code-test/create_file_test_func.PNG)


1. Test kodları hazırlandıktan sonra Visual Studio'da test ortamını ayarlayabilirsiniz.

   **test1.py**adlı bir Python dosyası oluşturun. Bu dosyada, yapmak istediğiniz tüm testleri içeren bir sınıf oluşturun. Aşağıdaki örnek, hazırlanan altı testi gösterir:
    
    ![Sınıftaki testlerin listesini içeren Python dosyası](./media/code-test/create_file_test1_class.PNG)

1. Bu testler, sınıf adınızın ardından **codetest.testCase** koyarsanız otomatik olarak keşfedilebilir. Sağ bölmede Test Gezgini'ni açın ve **Tümünü Çalıştır'ı**seçin. Tüm testler sırayla çalışır ve testin başarılı olup olmadığını size söyleyecektir.

    ![Testleri çalıştırma](./media/code-test/run_tests.PNG)

1. Git komutlarını kullanarak kodunuzu proje deposuna iade edin. En son çalışmalarınız kısa süre içinde Azure DevOps'a yansıyacaktır.

    ![Kodu kontrol etmek için git komutları](./media/code-test/git_check_in.PNG)

    ![Azure DevOps'deki en son çalışma](./media/code-test/git_check_in_most_recent_work.PNG)

1. Azure DevOps'lerde otomatik oluşturma ve test oluşturma:

    a. Proje deposunda, **Yapı ve Serbest Bırak'ı**seçin ve ardından yeni bir yapı işlemi oluşturmak için **+Yeni'yi** seçin.

    ![Yeni bir yapı işlemi başlatmak için seçimler](./media/code-test/create_new_build.PNG)

    b. Kaynak kodu konumunuzu, proje adınızı, deponuzu ve şube bilgilerinizi seçmek için istemleri izleyin.
    
    ![Kaynak, ad, depo ve şube bilgileri](./media/code-test/fill_in_build_info.PNG)

    c. Bir şablon seçin. Python proje şablonu olmadığından, **Boş işlemi**seçerek başlayın. 

    ![Şablonlar listesi ve "Boş işlem" düğmesi](./media/code-test/start_empty_process_template.PNG)

    d. Yapının adını ve aracıyı seçin. Yapı işlemini tamamlamak için bir DSVM kullanmak istiyorsanız varsayılanı burada seçebilirsiniz. Aracıları ayarlama hakkında daha fazla bilgi için yapı [ve sürüm aracıları](https://docs.microsoft.com/azure/devops/pipelines/agents/agents?view=vsts)hakkında bilgi.
    
    ![Yapı ve aracı seçimleri](./media/code-test/select_agent.PNG)

    e. Bu **+** yapı aşaması için bir görev eklemek için sol bölmede seçin. Tüm denetimleri tamamlamak için Python komut dosyasını **test1.py** çalıştıracağız çünkü, bu görev Python kodunu çalıştırmak için powershell komutu kullanıyor.
    
    ![PowerShell seçili "Görev ekle" bölmesi](./media/code-test/add_task_powershell.PNG)

    f. PowerShell ayrıntılarında, PowerShell'in adı ve sürümü gibi gerekli bilgileri doldurun. Tür olarak Satır Satır Komut **Dosyası'nı** seçin. 
    
    **Inline Script**altındaki kutuda, **python test1.py**yazabilirsiniz. Ortam değişkeninin Python için doğru ayarlandıkdığından emin olun. Python'un farklı bir sürümüne veya çekirdeğine ihtiyacınız varsa, yolu şekilde gösterildiği şekilde açıkça belirtebilirsiniz: 
    
    ![PowerShell ayrıntıları](./media/code-test/powershell_scripts.PNG)

    g. Yapı ardışık işlem işlemini tamamlamak için **& sırayı kaydet'i** seçin.

    !["& sırayı kaydet" düğmesi](./media/code-test/save_and_queue_build_definition.PNG)

Şimdi, yeni bir taahhüt kod deposuna her itildinde, yapı işlemi otomatik olarak başlatılır. (Burada depo olarak master kullanın, ancak herhangi bir dalı tanımlayabilirsiniz.) İşlem, kodda tanımlanan her şeyin doğru çalıştığından emin olmak için aracı makinesindeki **test1.py** dosyasını çalıştırZ. 

Uyarılar doğru şekilde ayarlanırsa, yapı tamamlandığında e-posta yla bilgilendirilirsiniz. Azure DevOps'te yapı durumunu da kontrol edebilirsiniz. Başarısız olursa, yapının ayrıntılarını kontrol edebilir ve hangi parçanın kırıldığını öğrenebilirsiniz.

![Yapı başarısının e-posta bildirimi](./media/code-test/email_build_succeed.PNG)

![Azure DevOps yapı başarısı bildirimi](./media/code-test/vs_online_build_succeed.PNG)

## <a name="next-steps"></a>Sonraki adımlar
* Veri bilimi senaryoları için birim testlerinin somut örnekleri için [UCI gelir tahmin deposuna](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) bakın.
* Kendi veri bilimi projelerinizde UCI gelir tahmini senaryosundan önceki anahat ve örnekleri izleyin.

## <a name="references"></a>Başvurular
* [Team Data Science Process](https://aka.ms/tdsp)
* [Visual Studio Test Araçları](https://www.visualstudio.com/vs/features/testing-tools/)
* [Azure Devops Test Kaynakları](https://www.visualstudio.com/team-services/)
* [Veri Bilimi Sanal Makineleri](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/)
