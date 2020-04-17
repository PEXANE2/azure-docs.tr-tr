---
title: Azure Laboratuvar Hizmetlerini kullanarak büyük veri analitiği öğretmek için bir laboratuvar ayarlayın | Microsoft Dokümanlar
description: Hortonworks Veri Platformu'nun (HDP) Docker dağıtımını kullanarak büyük veri analitiğini öğretmek için nasıl bir laboratuvar kurup kurabilirsiniz öğrenin.
services: lab-services
documentationcenter: na
author: nicolela
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/15/2020
ms.author: nicolela
ms.openlocfilehash: c499b7a0f1cd16bf57fef21742b01bda71249916
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81538784"
---
# <a name="set-up-a-lab-to-big-data-analytics-using-docker-deployment-of-hortonworks-data-platform"></a>HortonWorks Veri Platformu'nun Docker dağıtımını kullanarak büyük veri analitiği için bir laboratuvar ayarlama

Bu makalede, büyük bir veri analizi sınıföğretmek için bir laboratuvar kurmak nasıl gösterir.  Bu tür bir sınıfla, öğrenciler büyük hacimli verileri nasıl işleyeceğini ve veri öngörüleri elde etmek için makine ve istatistiksel öğrenme algoritmaları kullanmayı öğrenirler.  Öğrenciler için temel bir amaç, [Apache Hadoop'un](https://hadoop.apache.org/) büyük verileri depolamak, yönetmek ve işlemek için araçlar sağlayan açık kaynak yazılım paketi gibi veri analizi araçlarını kullanmayı öğrenmektir.

Bu laboratuvarda, öğrenciler [Hadoop Cloudera](https://www.cloudera.com/)tarafından sağlanan popüler bir ticari sürümünü kullanacak , [Hortonworks Veri Platformu (HDP)](https://www.cloudera.com/products/hdp.html)denir.  Özellikle öğrenciler, platformun ücretsiz olarak basitleştirilmiş, kullanımı kolay ve öğrenme ve deney amaçlı bir versiyonu olan [HDP Sandbox 3.0.1'i](https://www.cloudera.com/tutorials/getting-started-with-hdp-sandbox/1.html) kullanacaklar.  Bu sınıf, HDP Sandbox dağıtılmış Windows veya Linux sanal makineleri (VM) kullanabilse de, bu makalede Windows'un nasıl kullanılacağı gösterilecek.

Bu laboratuvarın bir diğer ilginç yönü de, [Docker](https://www.docker.com/) konteynerlerini kullanarak HDP Sandbox'ı laboratuvar VM'lerine konuşlandırmamız.  Her Docker kapsayıcısı, yazılım uygulamalarının içinde çalışması için kendi yalıtılmış ortamını sağlar.  Kavramsal olarak, Docker kapsayıcıları iç içe vm'ler gibidir ve [Docker Hub'da](https://www.docker.com/products/docker-hub)sağlanan kapsayıcı görüntülerine dayalı çok çeşitli yazılım uygulamalarını kolayca dağıtmak ve çalıştırmak için kullanılabilir.  Cloudera'nın HDP Sandbox için dağıtım komut [dosyası, HDP Sandbox 3.0.1 Docker görüntüsünü](https://hub.docker.com/r/hortonworks/sandbox-hdp) Docker Hub'dan otomatik olarak çeker ve iki Docker kapsayıcısı çalıştırır:
  - kum havuzu-hdp
  - kum havuzu-proxy

## <a name="lab-configuration"></a>Laboratuvar yapılandırması

Bu laboratuarı kurmak için başlamak için bir Azure aboneliğine ve laboratuvar hesabına ihtiyacınız var. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun. Azure aboneliği ni aldıktan sonra Azure Lab Hizmetleri'nde yeni bir laboratuvar hesabı oluşturabilirsiniz. Yeni bir laboratuvar hesabı oluşturma hakkında daha fazla bilgi için, [Laboratuvar Hesabı Kurma Öğretici'sine](tutorial-setup-lab-account.md)bakın.  Varolan bir laboratuvar hesabını da kullanabilirsiniz.

### <a name="lab-account-settings"></a>Laboratuvar hesap ayarları

Laboratuvar hesabı için aşağıdaki tabloda açıklanan ayarları etkinleştirin. Pazar yeri görüntülerini etkinleştirme hakkında daha fazla bilgi için laboratuvar [oluşturucularının kullanabileceği Market görüntülerini belirt'e](https://docs.microsoft.com/azure/lab-services/classroom-labs/specify-marketplace-images)bakın.

| Laboratuvar hesap ayarı | Yönergeler |
| ------------------- | ------------ |
|Pazar yeri görüntüsü| Windows 10 Pro görüntüsünü laboratuvar hesabınızda kullanmak üzere etkinleştirin.|

### <a name="lab-settings"></a>Laboratuvar ayarları

Sınıf laboratuarı kurarken aşağıdaki tablodaki ayarları kullanın.  Sınıf laboratuarı oluşturmak için daha fazla bilgi için [bkz.](tutorial-setup-classroom-lab.md)

| Laboratuvar ayarları | Değer/talimatlar |
| ------------ | ------------------ |
|Sanal Makine Boyutu| Orta (İç içe Sanallaştırma). Bu VM boyutu ilişkisel veritabanları, bellek içi önbelleğe alma ve analiz için en uygunudur.  Bu boyut iç içe sanallaştırmayı da destekler.|  
|Sanal Makine Görüntüsü| Windows 10 Pro|

> [!NOTE] 
> HDP Sandbox'ı Docker kullanarak dağıtmak için Medium (Nested Virtualization) kullanmamız gerekir:
>   - İç içe sanallaştırma ile Windows Hyper-V
>   - En az 10 GB RAM

## <a name="template-machine-configuration"></a>Şablon makine yapılandırması

Şablon makinesini ayarlamak için şunları yapacağız:
- Docker'ı yükleme
- HDP Sandbox'ı dağıtın
- Docker kapsayıcılarını otomatik olarak başlatmak için PowerShell ve Windows Görev Zamanlayıcısı'nı kullanın

### <a name="install-docker"></a>Docker'ı yükleme

Bu bölümdeki [adımlar, Cloudera'nın Docker konteynerleriyle dağıtım talimatlarına](https://www.cloudera.com/tutorials/sandbox-deployment-and-install-guide/3.html)dayanmaktadır. 

Docker kapsayıcılarını kullanmak için öncelikle Docker Desktop şablonu VM'ye yüklemeniz gerekir:

1. [Windows için Docker'ı](https://docs.docker.com/docker-for-windows/install/)yüklemek için [Önkoşullar bölümündeki](https://www.cloudera.com/tutorials/sandbox-deployment-and-install-guide/3.html#prerequisites) adımları izleyin. 

    > [!IMPORTANT] 
    > Linux kapsayıcıları yapılandırma seçeneği **yerine Windows kapsayıcıları kullan** seçeneğinin işaretsiz bırakıldığından emin olun.

1. Windows **Kapsayıcıları ve Hyper-V özelliklerinin** açık olduğundan emin olun.
   ![Windows özelliklerini etkinleştirme veya devre dışı bırakma](../media/class-type-big-data-analytics/windows-hyperv-features.png)

1. Docker'ın bellek yapılandırmasını yapılandırmak [için Windows için Bellek](https://www.cloudera.com/tutorials/sandbox-deployment-and-install-guide/3.html#memory-for-windows) bölümündeki adımları izleyin.

    > [!WARNING]
    > Docker'ı yüklerken **Yanlışlıkla Linux kapsayıcıları yerine Windows kapsayıcıları kullan** seçeneğini kontrol ederseniz, bellek yapılandırma ayarlarını görmezsiniz.  Bunu düzeltmek için, [Windows Sistemi tepsisindeki Docker simgesine tıklayarak](https://docs.docker.com/docker-for-windows/#docker-settings-dialog)Linux kapsayıcılarını kullanmaya geçebilirsiniz; Docker Desktop menüsü açıldığında, **Linux kapsayıcılarına Geçiş'i**seçin.
 
### <a name="deploy-hdp-sandbox"></a>HDP Sandbox'ı dağıtın

Bu bölümde HDP Sandbox'ı dağıtacak ve tarayıcıyı kullanarak HDP Sandbox'a da erişeceksiniz.

1. Sonraki adımları tamamlamak için tavsiye [edildiğinden, Git Bash'i](https://gitforwindows.org/) kılavuzun [Önkoşullar bölümünde](https://www.cloudera.com/tutorials/sandbox-deployment-and-install-guide/3.html#prerequisites) listelenen olarak yüklediğinizden emin olun.

1. [Cloudera'nın Docker için Dağıtım ve Yükleme Kılavuzu'nu](https://www.cloudera.com/tutorials/sandbox-deployment-and-install-guide/3.html)kullanarak aşağıdaki bölümlerdeki adımları tamamlayın:
   
   -    HDP Sandbox'ı dağıtın
   -    HDP Sandbox'ı doğrula

    > [!WARNING] 
    > HDP için en son .zip dosyasını indirdiğinizde,.zip dosyasını beyaz alan içeren bir dizin yoluna *kaydetmediğinizden* emin olun.

    > [!NOTE] 
    > Dağıtım sırasında **Drive'ın paylaşılmadığını**belirten bir özel durum alırsanız, HDP'nin Linux kapsayıcılarının yerel Windows dosyalarına erişebilmeleri için C sürücünüzü Docker ile paylaşmanız gerekir.  Bunu düzeltmek için, Docker Desktop menüsünü açmak ve **Ayarlar'ı**seçmek için [Windows Sistemi tepsisindeki Docker simgesine tıklayın.](https://docs.docker.com/docker-for-windows/#docker-settings-dialog)  **Docker'ın Ayarlar** iletişim kutusu açıldığında, **Dosya Paylaşımı> Kaynakları'nı** seçin ve **C** sürücüsünü denetleyin.  Daha sonra HDP Sandbox dağıtmak için adımları tekrarlayabilirsiniz.

1. HDP Sandbox için Docker konteynerleri dağıtıldıktan ve çalıştırıldıktan sonra, tarayıcınızı başlatarak ve Cloudera'nın [Sandbox Karşılama Sayfası'nı](https://www.cloudera.com/tutorials/learning-the-ropes-of-the-hdp-sandbox.html#welcome-page) açma ve HDP Panosu'nu açma yönergelerini izleyerek ortama erişebilirsiniz.

    > [!NOTE] 
    > Bu yönergeler, öncelikle sandbox ortamının yerel IP adresini şablon VM'nizdeki ana bilgisayar dosyasındaki ana bilgisayar dosyasındaki sandbox-hdp.hortonworks.com eşlediğinizi varsayar.  Bu eşlemi **yapmazsanız,** 'ye ' için gezinerek [http://localhost:8080](http://localhost:8080)Sandbox HoşGeldiniz sayfasına erişebilirsiniz.

### <a name="automatically-start-docker-containers-when-students-log-in"></a>Öğrenciler oturum açtıklarında Docker konteynerlerini otomatik olarak başlatın

Öğrenciler için kullanımı kolay bir deneyim sağlamak için, otomatik olarak bir PowerShell komut dosyası kullanacağız:
  - Bir öğrenci başladığında ve kendi laboratuvar VM bağlanır HDP Sandbox Docker konteyner başlar.
  - Tarayıcıyı başlatır ve Sandbox Karşılama Sayfası'na yönlendirin.
Ayrıca, bir öğrenci VM'lerine giriş yaptığında bu komut dosyasını otomatik olarak çalıştırmak için Windows Görev Zamanlayıcısı'nı da kullanırız.
Bunu ayarlamak için şu adımları izleyin: [Büyük Veri Analitiği komut dosyası.](https://github.com/Azure/azure-devtestlab/tree/master/samples/ClassroomLabs/Scripts/BigDataAnalytics/)

## <a name="cost-estimate"></a>Maliyet tahmini

Bu laboratuvarın maliyetini tahmin etmek isterseniz, aşağıdaki örneği kullanabilirsiniz.

20 saatlik planlanan ders süresi ve ödev veya ödevler için 10 saatlik kontenjanı olan 25 öğrenciden oluşan bir sınıf için, laboratuvar fiyatı:
  - 25 öğrenci * (20 + 10) saat * 55 Laboratuvar Birimi * 0,01 USD /saat = 412,50 USD

Fiyatlandırma hakkında daha fazla ayrıntı için Azure [Lab Hizmetleri Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/lab-services/)bakın.

## <a name="conclusion"></a>Sonuç

Bu makale, Docker ile dağıtılan Hortonworks Veri Platformu'nu kullanan büyük bir veri analizi sınıfı için bir laboratuvar oluşturmak için gereken adımları size iletilmiştir.  Bu sınıf türü için kurulum benzer veri analizi sınıfları için kullanılabilir.  Bu kurulum, dağıtım için Docker kullanan diğer sınıf türleri için de geçerli olabilir.

## <a name="next-steps"></a>Sonraki adımlar

Sonraki adımlar herhangi bir laboratuvar kurmak için ortak.

- [Şablon oluşturma ve yönetme](how-to-create-manage-template.md)
- [Kullanıcı ekle](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Kota ayarlama](how-to-configure-student-usage.md#set-quotas-for-users)
- [Zamanlama ayarlama](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab)
- [Öğrencilere e-posta kayıt bağlantıları](how-to-configure-student-usage.md#send-invitations-to-users)
