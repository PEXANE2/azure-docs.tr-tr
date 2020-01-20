---
title: Sanal makine ölçek kümeleriyle otomatik ölçeklendirme sorunlarını giderme
description: Sanal makine ölçek kümeleriyle otomatik ölçeklendirme sorunlarını giderin. Karşılaşılan tipik sorunları ve bunların nasıl çözümleneceğini anlayın.
author: mayanknayar
tags: azure-resource-manager
ms.assetid: c7d87b72-ee24-4e52-9377-a42f337f76fa
ms.service: virtual-machine-scale-sets
ms.tgt_pltfrm: windows
ms.topic: conceptual
ms.date: 11/16/2017
ms.author: manayar
ms.openlocfilehash: 923967a902f611ce845fbdc096fd2c02e681bb6e
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2020
ms.locfileid: "76272424"
---
# <a name="troubleshooting-autoscale-with-virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleriyle otomatik ölçeklendirme sorunlarını giderme
**Sorun** https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale: sanal makine ölçek kümelerini kullanarak Azure Resource Manager bir otomatik ölçeklendirme altyapısı oluşturdunuz; Örneğin, bunun gibi bir şablon dağıtarak, ölçek kurallarınızı tanımladınız ve bu, VM 'lere ne kadar yük yerleştirdiğinize bakılmaksızın otomatik ölçeklendirme yapmaz.

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
Göz önünde bulundurulması gereken bazı noktalar şunlardır:

* Her VM 'nin kaç tane vCPU olduğunu ve her vCPU 'Yu yüklüyorsunuz?
  Önceki örnek Azure hızlı başlangıç şablonu, tek bir vCPU yükleyen bir do_work. php komut dosyasına sahiptir. Standard_A1 veya D1 gibi tek vCPU VM boyutundan daha büyük bir VM kullanıyorsanız, bu yükü birden çok kez çalıştırmanız gerekir. [Azure 'Da Windows sanal makineleri Için boyutları inceleyerek sanal makineleriniz](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Için kaç vCPU olduğunu denetleyin
* Sanal makine ölçek kümesindeki kaç VM, her biri üzerinde iş yapıyor musunuz?
  
    Ölçek Genişletme olayı yalnızca bir ölçek kümesindeki **Tüm** VM 'LERDE ortalama CPU, otomatik ölçeklendirme kurallarında tanımlanan zaman içinde eşik değerini aşarsa meydana gelen bir olay gerçekleşir.
* Tüm ölçeklendirme olaylarını kaçırdınız mı?
  
    Ölçek olayları için Azure portal denetim günlüklerini denetleyin. Belki de bir ölçek yukarı ve bir ölçeği kaçırıldı. "Scale" ile filtre uygulayabilirsiniz.
  
    ![Denetim Günlükleri][audit]
* Ölçek Genişletme ve genişleme eşikleri yeterince farklı mi?
  
    Ortalama CPU 'nun beş dakika içinde %50 ' den büyük olduğu ve ortalama CPU %50 ' den az olduğunda ölçeği ölçeklendirmek için bir kural ayarladığınızı varsayalım. Bu ayar, CPU kullanımı eşiğe yakın olduğunda "flağa" bir sorun oluşmasına neden olur, böylece ölçek işlemleri sürekli artan ve küme boyutunu azaltır. Bu ayar nedeniyle, otomatik ölçeklendirme hizmeti "Flama" yapmayı dener, bu da ölçeklendirmiyor olarak bildirimde bulunabilir. Bu nedenle, ölçeklendirmenin ve ölçek genişletme eşiklerinizin ölçekleme arasında boşluk kullanılmasına izin vermek için yeterince farklı olduğundan emin olun.
* Kendi JSON şablonunuzu mi yazadınız?
  
    Hataları oluşturmak kolaydır, bu nedenle, üzerinde değişen bir şablonla başlayın ve küçük artımlı değişiklikler yapın. 
* El ile ölçeklendirebilir veya kapatabilirsiniz mi?
  
    VM sayısını el ile değiştirmek için sanal makine ölçek kümesi kaynağını farklı bir "kapasite" ayarıyla yeniden dağıtmaya çalışın. Örnek bir şablon aşağıda verilmiştir: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing – ölçek kümesi tarafından kullanılan makine boyutuna sahip olduğundan emin olmak için şablonu düzenlemeniz gerekebilir. VM sayısını el ile başarıyla değiştirebiliyorsanız, sorunun otomatik ölçeklendirme için yalıtılmış olduğunu bilirsiniz.
* Microsoft. COMPUTE/virtualMachineScaleSet ve Microsoft. Insights kaynaklarınızı [Azure Kaynak Gezgini](https://resources.azure.com/) denetleyin
  
    Azure Kaynak Gezgini, Azure Resource Manager kaynaklarınızın durumunu gösteren bir olmazdır sorun giderme aracıdır. Aboneliğinize tıklayın ve sorun gidermekte olduğunuz kaynak grubuna bakın. Işlem kaynak sağlayıcısı altında, oluşturduğunuz sanal makine ölçek kümesine bakın ve dağıtım durumunu gösteren örnek görünümünü denetleyin. Ayrıca, sanal makine ölçek kümesindeki VM 'lerin örnek görünümünü kontrol edin. Ardından, Microsoft. Insights kaynak sağlayıcısına gidin ve otomatik ölçeklendirme kurallarının doğru olup olmadığını denetleyin.
* Tanılama uzantısı çalışıyor ve performans verileri yaysın mı?
  
    **Güncelleştirme:** Azure otomatik ölçeklendirme, artık bir tanılama uzantısının yüklenmesini gerektirmeyen ana bilgisayar tabanlı ölçüm işlem hattını kullanacak şekilde geliştirilmiştir. Yeni işlem hattını kullanarak bir otomatik ölçeklendirme uygulaması oluşturursanız, sonraki birkaç paragraf artık uygulanmaz. Ana bilgisayar ardışık düzenini kullanmak üzere dönüştürülmüş Azure şablonlarının bir örneği şurada bulunabilir: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale. 
  
    Otomatik ölçeklendirme için ana bilgisayar tabanlı ölçümlerin kullanılması aşağıdaki nedenlerden dolayı daha iyidir:
  
  * Hiçbir Tanılama uzantısının yüklenmesi gerekmiyorsa, daha az hareketli parçalar.
  * Daha basit şablonlar. Mevcut bir ölçek kümesi şablonuna Öngörüler otomatik ölçeklendirme kuralları eklemeniz yeterlidir.
  * Daha güvenilir raporlama ve yeni VM 'Leri daha hızlı başlatma.
    
    Bir tanılama uzantısı kullanarak tutmak isteyebileceğiniz tek nedenler, bellek tanılama raporlama/ölçeklendirme gereksinimleridir. Ana bilgisayar tabanlı ölçümler belleği raporlamaz.
    
    Göz önünde bulundurularak, yalnızca otomatik ölçeklendirme için tanılama uzantıları kullanıyorsanız bu makalenin geri kalanını izleyin.
    
    Azure Resource Manager otomatik ölçeklendirme, tanılama uzantısı adlı bir VM Uzantısı aracılığıyla çalışabilir (ancak artık bu kadar). Performans verilerini şablonda tanımladığınız bir depolama hesabına yayar. Bu veriler daha sonra Azure Izleyici hizmeti tarafından toplanır.
    
    Öngörüler hizmeti VM 'lerden veri okuyamaması durumunda size bir e-posta gönderilmesi gerekir. Örneğin, VM 'Ler kapalıysa bir e-posta alırsınız. Azure hesabınızı oluştururken belirttiğiniz e-posta adresinden e-postanızı denetlediğinizden emin olun.
    
    Ayrıca verilere kendiniz de bakabilirsiniz. Bulut Gezgini 'ni kullanarak Azure depolama hesabı ' na bakın. Örneğin, [Visual Studio Cloud Explorer](https://visualstudiogallery.msdn.microsoft.com/aaef6e67-4d99-40bc-aacf-662237db85a2)'ı kullanarak oturum açın ve kullanmakta olduğunuz Azure aboneliğini seçin. Ardından, dağıtım şablonunuzda tanılama uzantısı tanımında başvurulan tanılama depolama hesabı adına bakın.
    
    ![Cloud Explorer][explorer]
    
    Her VM 'deki verilerin depolandığı bir dizi tablo görürsünüz. Linux ve CPU ölçüsünü örnek olarak almak için en son satırlara bakın. Visual Studio Cloud Explorer bir sorgu dilini destekler, böylece bir sorgu çalıştırabilirsiniz. Örneğin, en son olayları aldığınızdan emin olmak için "timestamp gt DateTime ' 2016-02-02T21:20:00Z '" için bir sorgu çalıştırabilirsiniz. Saat dilimi UTC 'ye karşılık gelir. Burada gördüğünüz veriler ayarladığınız ölçek kurallarına karşılık geliyor mu? Aşağıdaki örnekte, 5. makinenin CPU 'SU son beş dakika içinde %100 ' e kadar artmaya başladı.
    
    ![Depolama Tabloları][tables]
    
    Veriler yoksa, bu sorun, VM 'lerde çalışan tanılama uzantısının bulunduğu anlamına gelir. Veriler varsa, ölçek kurallarınızın bir sorunu ya da Öngörüler hizmeti ile ilgili bir sorun olduğunu gösterir. [Azure durumunu](https://azure.microsoft.com/status/)denetleyin.
    
    Bu adımları tamamladıktan sonra otomatik ölçeklendirme sorunlarınız varsa, aşağıdaki kaynakları deneyebilirsiniz: 
    * [MSDN](https://social.msdn.microsoft.com/forums/azure/home?forum=WAVirtualMachinesforWindows)veya [yığın taşması](https://stackoverflow.com/questions/tagged/azure) hakkındaki forumları okuyun 
    * Bir destek çağrısını günlüğe kaydedin. Şablonu paylaşmaya ve performans verilerinizin görünümüne hazırlıklı olun.

[audit]: ./media/virtual-machine-scale-sets-troubleshoot/image3.png
[explorer]: ./media/virtual-machine-scale-sets-troubleshoot/image1.png
[tables]: ./media/virtual-machine-scale-sets-troubleshoot/image4.png
