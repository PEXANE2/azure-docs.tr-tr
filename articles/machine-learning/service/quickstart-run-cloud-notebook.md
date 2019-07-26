---
title: 'Hızlı Başlangıç: Bulutta bir not defteri çalıştırma'
titleSuffix: Azure Machine Learning service
description: Bu öğreticide, Azure Machine Learning hizmeti ile çalışmaya başlama ve bulutta yönetilen bir not defteri sunucusunu kullanma işlemi gösterilmektedir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: quickstart
author: sdgilley
ms.author: sgilley
ms.date: 05/14/2019
ms.custom: seodec18
ms.openlocfilehash: 1124bb17abb9340b442d8d6075551ffe0dc681f7
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68371045"
---
# <a name="quickstart-use-a-cloud-based-notebook-server-to-get-started-with-azure-machine-learning"></a>Hızlı Başlangıç: Azure Machine Learning kullanmaya başlamak için bulut tabanlı bir not defteri sunucusu kullanın

Bu hızlı başlangıç, bulutta yönetilen bir not defteri sunucusu kullanarak Azure Machine Learning hizmetini nasıl kullanacağınızı öğretir. Yükleme gerekmez. SDK 'yı kendi Python ortamınıza yüklemek isterseniz, bkz [. hızlı başlangıç: Azure Machine Learning](quickstart-run-local-notebook.md)kullanmaya başlamak için kendi Not defteri sunucunuzu kullanın.

Bu hızlı başlangıç, Machine Learning (ML) denemeleri 'in izlenmesini sağlamak için [Azure Machine Learning hizmeti çalışma alanını](concept-azure-machine-learning-architecture.md) nasıl kullanacağınızı gösterir. Bu, bir [Not defteri sanal makinesi (Önizleme)](how-to-configure-environment.md#notebookvm)oluşturarak yapılır: Jupyter Not defteri sunucusu, JupyterLab ve tam hazırlanmış ml ortamı sağlayan güvenli, bulut tabanlı bir Azure iş istasyonu. Ardından, bu sanal makinede (VM) değerleri çalışma alanına kaydeden bir Python Not defteri çalıştırırsınız.

Bunu yapmak için aşağıdaki işlemleri gerçekleştirin:

* Çalışma alanı oluşturun.
* Çalışma alanınızda bir not defteri VM 'si oluşturun.
* Jupyıter Web arabirimini açın.
* Pi 'yi tahmin etmek için kod içeren bir not defteri açın ve her yinelemede hataları günlüğe kaydeder.
* Not defterini çalıştırın.
* Günlüğe kaydedilen hata değerlerini çalışma alanınızda görüntüleyin. Aşağıdaki örnek, çalışma alanının bir betikte oluşturulan bilgileri izlemenize nasıl yardımcı olduğunu gösterir.

Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning hizmetinin ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Bir Azure Machine Learning hizmeti çalışma alanınız varsa, [sonraki bölüme](#create-notebook)atlayın. Aksi takdirde, şimdi bir tane oluşturun.

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="create-notebook"></a>Not defteri VM 'si oluşturma

 Jupyıter not defterlerini kullanmaya başlamak için çalışma alanınızdan bir bulut kaynağı oluşturun. Bu kaynak, Azure Machine Learning hizmetini çalıştırmak için gereken her şeyi önceden yapılandırılmış bulut tabanlı bir platformdur.

1. Çalışma alanınızı [Azure Portal](https://portal.azure.com/)açın. Portalda çalışma alanınızı bulma konusunda emin değilseniz, bkz. [çalışma alanınızı görüntüleme](how-to-manage-workspace.md#view).

1. Çalışma alanı sayfanızda, sol taraftaki **Not defteri VM 'leri** seçin.

1. Bir not defteri VM 'si oluşturmak için **+ Yeni** ' yi seçin.  

     ![Yeni VM 'yi seçin](./media/quickstart-run-cloud-notebook/add-workstation.png)

1. VM 'niz için bir ad sağlayın. Ardından **Oluştur**’u seçin.

    > [!NOTE]
    > Not defteri sanal makinenizin adı 2 ile 16 karakter arasında olmalıdır. Harfler, rakamlar ve kısa çizgiler geçerli karakterlerdir. Ad, Azure aboneliğinizde benzersiz olmalıdır.

    ![Yeni VM oluşturma](media/quickstart-run-cloud-notebook/create-new-workstation.png)

1. Durum **çalışmaya**dönüşene kadar yaklaşık 4 ila 5 dakika bekleyin.


## <a name="open-the-jupyter-web-interface"></a>Jupyıter Web arabirimini açın

VM 'niz çalıştırıldıktan sonra, Jupyter Web arabirimini açmak için **Not defteri VM 'leri** bölümünü kullanın.

1. VM 'niz için **URI** sütununda **Jupyıter** ' ı seçin.  

    ![Jupyter Not defteri sunucusunu başlatma](./media/quickstart-run-cloud-notebook/start-server.png)

    Bağlantı, Not defteri sunucunuzu başlatır ve yeni bir tarayıcı sekmesinde Jupyter Not defteri Web sayfasını açar.  Bu bağlantı yalnızca VM 'yi oluşturan kişi için çalışır.  Çalışma alanının her bir kullanıcısının kendi sanal makinesini oluşturması gerekir.

1. Jupyter Not defteri Web sayfasında, en üst klasörün adı, Kullanıcı adınız. Bu klasörü seçin.

    > [!TIP]
    > Bu klasör, dizüstü makınenın kendisi yerine çalışma alanınızdaki [depolama kapsayıcısında](concept-workspace.md#resources) bulunur.  Not defteri sanal makinesini silebilir ve çalışmalarınızı devam edebilirsiniz.  Daha sonra yeni bir not defteri VM 'si oluşturduğunuzda, bu dosya aynı klasöre yüklenir.  Çalışma alanınızı başkalarıyla paylaşıyorsanız, klasörünüzü görürler ve onu görürsünüz. 

1. Samples klasör adı bir sürüm numarası içerir (örneğin,**1.0.33.1**). Örnekler klasörünü seçin.

1. **Hızlı başlangıç** klasörünü seçin.

## <a name="run-the-notebook"></a>Not defterini çalıştırma

Pi 'yi tahmin eden bir not defteri çalıştırın ve hatayı çalışma alanınıza kaydeder.

1. Not defterini açmak için **01. Run-denemeler. ipynb** öğesini seçin.

1. "Çekirdek bulunamadı" uyarısı görürseniz, çekirdek **Python 3,6-AzureML** ' u seçin (listenin yaklaşık olarak alt yarısında bulunur) ve çekirdeği ayarlayın.

1. İlk kod hücresini seçin ve sonra **Çalıştır**' ı seçin.

    > [!NOTE]
    > Kod hücrelerinde öncesinde köşeli ayraçlar vardır. Köşeli ayraçlar boş ( __[]__ ) ise, kod çalıştırılmadı. Kod çalışırken bir yıldız işareti ( __[*]__ ) görüntülenir. Kod tamamlandıktan sonra **[1]** numarası belirir.  Bu sayı size hücrelerin çalıştırıldığı sırayı söyler.
    >
    > Bir hücreyi çalıştırmak için **SHIFT + enter** kısayolunu kullanın.

    ![İlk kod hücresini Çalıştır](media/quickstart-run-cloud-notebook/cell1.png)

1. İkinci kod hücresini çalıştırın. Kimlik doğrulaması için yönergeler görürseniz, kodu kopyalayın ve oturum açmak için bağlantıyı izleyin. Oturum açtıktan sonra tarayıcınız bu ayarı anımsayacaktır.  

    ![Kimlik Doğrula](media/quickstart-run-cloud-notebook/authenticate.png)

1. Kod hücresi başarıyla çalıştırıldığında, __[2]__ hücre numarası görüntülenir. Oturum açmanız gerekiyorsa, başarılı bir kimlik doğrulama durumu iletisi görürsünüz.   Oturum açmanız yoksa, bu hücre için herhangi bir çıktı görmezsiniz. Yalnızca hücrenin başarıyla çalıştığını göstermek için görüntülenen sayıyı görürsünüz.

    ![Başarı iletisi](media/quickstart-run-cloud-notebook/success.png)

1. Kod hücrelerinin geri kalanını çalıştırın. Her hücre çalışmayı bitirdiğinde, hücre numarası görünür. Yalnızca son hücrede diğer çıktılar görüntülenir.  

    En büyük kod hücresinde `run.log` birden fazla yerde görünür. Her `run.log` biri değerini çalışma alanınıza ekler.

## <a name="view-logged-values"></a>Günlüğe kaydedilen değerleri görüntüleme

1. `run` Hücrenin çıktısı Azure Portal bir bağlantı içerir ve burada deneme sonuçlarını çalışma alanınızda görüntüleyebilirsiniz.

    ![Denemeleri görüntüleme](./media/quickstart-run-cloud-notebook/view-exp.png)

1. Çalışma alanınızda çalıştırma hakkındaki bilgileri görüntülemek için **Azure portalına bağla** ' yı seçin. Bu bağlantı, çalışma alanınızı Azure portal açar.

1. Gördüğünüz günlüğe kaydedilen değerlerin çizimleri, çalışma alanında otomatik olarak oluşturulmuştur. Aynı ad parametresine sahip birden fazla değeri günlüğe kaydettiğinizde otomatik olarak bir çizim oluşturulur. Örnek aşağıda verilmiştir:

   ![Geçmişi görüntüleme](./media/quickstart-run-cloud-notebook/web-results.png)

Pi 'nin yaklaşık bir değeri rastgele değerler kullandığından, çiztlarınız farklı görünebilir.  

## <a name="clean-up-resources"></a>Kaynakları temizleme

### <a name="stop-the-notebook-vm"></a>Not defteri VM 'sini durdur

Maliyeti azaltmak için kullanmadığınız durumlarda Not defteri VM 'sini durdurun.  

1. Çalışma alanınızda, **Not defteri VM 'leri**' ni seçin.

   ![VM sunucusunu durdur](./media/quickstart-run-cloud-notebook/stop-server.png)

1. Listeden VM’yi seçin.

1. **Durdur**' u seçin.

1. Sunucuyu yeniden kullanmaya hazırsanız **Başlat**' ı seçin.

### <a name="delete-everything"></a>Her şeyi sil

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

Ayrıca, kaynak grubunu koruyabilir ancak tek bir çalışma alanını silebilirsiniz. Çalışma alanı özelliklerini görüntüleyin ve **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu görevleri tamamladıktan sonra Jupyter Notebook Web sayfasına gidin. **Hızlı başlangıç** klasöründe, bir Web hizmetini dağıtmayı öğrenmek için **02. deploy-Web-Service. ipynb** Not defterini açın ve çalıştırın.

Diğer Python paketlerini Jupyter ortamınıza yüklemek istediğinizde, bu kodu bir not defteri içinde kullanın:

```
!source activate py36 && pip install <packagename>
```

Ayrıca, Jupyter Notebook Web sayfasında, Azure Machine Learning hizmeti hakkında daha fazla bilgi edinmek için örnekler klasöründeki diğer not defterlerine göz atabilirsiniz.

Derinlemesine bir iş akışı deneyimi için, bir modeli eğitme ve dağıtmaya yönelik Machine Learning öğreticileri izleyin:  

> [!div class="nextstepaction"]
> [Öğretici: Görüntü sınıflandırma modelini eğitme](tutorial-train-models-with-aml.md)
