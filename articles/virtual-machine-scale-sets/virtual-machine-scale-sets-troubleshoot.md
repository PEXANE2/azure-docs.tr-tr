---
title: Sanal Makine Ölçek Setleri ile Otomatik Ölçek Sorun Giderme
description: Sanal Makine Ölçek Setleri ile otomatik ölçek sorun giderme. Karşılaşılan tipik sorunları ve bunları nasıl çözeceğinizi anlayın.
author: mayanknayar
tags: azure-resource-manager
ms.assetid: c7d87b72-ee24-4e52-9377-a42f337f76fa
ms.service: virtual-machine-scale-sets
ms.tgt_pltfrm: windows
ms.topic: conceptual
ms.date: 11/16/2017
ms.author: manayar
ms.openlocfilehash: 923967a902f611ce845fbdc096fd2c02e681bb6e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76272424"
---
# <a name="troubleshooting-autoscale-with-virtual-machine-scale-sets"></a>Sanal Makine Ölçek Kümeleriyle otomatik ölçeklendirme sorunlarını giderme
**Sorun** – Sanal makine ölçek kümelerini kullanarak Azure Kaynak Yöneticisi'nde bir otomatik ölçeklendirme altyapısı oluşturdunuz https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale - örneğin, bunun gibi bir şablon dağıtarak: – ölçek kurallarınız tanımlandı ve vm'lere ne kadar yük koyarsanız koyun, otomatik ölçeklendirme yapmaz.

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
Göz önünde bulundurulması gereken bazı şeyler şunlardır:

* Her VM'de kaç vCPU var ve her vCPU yüklüyorum?
  Önceki örnek Azure Quickstart şablonu, tek bir vCPU yükleyen bir do_work.php komut dosyasına sahiptir. Standard_A1 veya D1 gibi tek vCPU VM boyutundan daha büyük bir VM kullanıyorsanız, bu yükü birden çok kez çalıştırmanız gerekir. [Azure'da Windows sanal makineleri için Boyutlar'ı](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) inceleyerek Sanal Tom'larınız için kaç vCPUs olduğunu kontrol edin
* Sanal makine ölçeği kümesinde kaç VM, her biri üzerinde iş yapıyorsunuz?
  
    Ölçeklendirme olayı, yalnızca bir ölçek kümesindeki **tüm** VM'ler deki ortalama CPU eşik değerini aştığında, otomatik ölçeklendirme kurallarında tanımlanan dahili süre içinde gerçekleşir.
* Herhangi bir ölçek olayını kaçırdın mı?
  
    Ölçek olayları için Azure portalındaki denetim günlüklerini denetleyin. Belki de gözden kaçırılan bir ölçek ve aşağı bir ölçek vardı. "Ölçekle" ile filtre uygulayabilirsiniz.
  
    ![Denetim Günlükleri][audit]
* Ölçeklendirme ve ölçeklendirme eşikleriniz yeterince farklı mı?
  
    Ortalama CPU'nun beş dakika içinde %50'den fazla olduğunu ölçeklendirmek ve ortalama CPU'nun %50'den az olduğunda ölçeklendirmek için bir kural belirlediğinizi varsayalım. CPU kullanımı eşiğe yakın olduğunda, ölçek eylemleri sürekli artan ve kümeboyutunu azalan bu ayar, "atlama" sorununa neden olur. Bu ayar nedeniyle, otomatik ölçeklendirme hizmeti ölçekleme değil olarak tezahür edebilir "atlama" önlemeye çalışır. Bu nedenle, ölçekleme ve ölçeklendirme eşiklerinizin ölçeklendirme arasında biraz boşluk sağlayacak kadar farklı olduğundan emin olun.
* Kendi JSON şablonunu mu yazdın?
  
    Hata yapmak kolaydır, bu nedenle yukarıdaki gibi bir şablonla başlayın ve işe yaradığı kanıtlanmıştır ve küçük artımlı değişiklikler yapın. 
* El ile ölçeklendirebilir misiniz veya dışarı?
  
    Sanal makine ölçeği kümesi kaynağını, VM sayısını el ile değiştirmek için farklı bir "kapasite" ayarına sahip yeniden dağıtmayı deneyin. Örnek bir şablon https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing burada: - Ölçek Setinizin kullandığı makine boyutuyla aynı makine boyutuna sahip olduğundan emin olmak için şablonu ayarlamanız gerekebilir. VM sayısını el ile başarıyla değiştirebiliyorsanız, sorunun otomatik ölçeklendirme için yalıtılmış olduğunu bilirsiniz.
* Azure Kaynak Gezgini'ndeki Microsoft.Compute/virtualMachineScaleSet ve [Azure Resource Explorer](https://resources.azure.com/) Microsoft.Insights kaynaklarınızı kontrol edin
  
    Azure Kaynak Gezgini, Azure Kaynak Yöneticisi kaynaklarınızın durumunu gösteren vazgeçilmez bir sorun giderme aracıdır. Aboneliğinizi tıklatın ve sorun giderme kaynak grubuna bakın. Bilgi İşlem kaynak sağlayıcısıaltında, oluşturduğunuz sanal makine ölçeği kümesine bakın ve dağıtım durumunu gösteren Örnek Görünümü'ne bakın. Ayrıca, sanal makine ölçeği kümesindeki VM'lerin örnek görünümünü denetleyin. Ardından, Microsoft.Insights kaynak sağlayıcısına gidin ve otomatik ölçeklendirme kurallarının doğru görünüp görünmediğini denetleyin.
* Tanılama uzantısı çalışıyor ve performans verileri yayıyor mu?
  
    **Güncelleme:** Azure otomatik ölçeklendirme, artık yüklenmesi gereken bir tanılama uzantısı gerektirmeyan ana bilgisayar tabanlı bir ölçüm ardışık alanı kullanmak üzere geliştirilmiştir. Yeni ardışık altbilgi kullanarak bir otomatik ölçekleme uygulaması oluşturursanız, sonraki birkaç paragraf artık geçerli değildir. Ana bilgisayar ardışık hattını kullanmak üzere dönüştürülen Azure şablonlarının https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscalebir örneğini buradan edinebilirsiniz: . 
  
    Otomatik ölçeklendirme için ana bilgisayar tabanlı ölçümlerin kullanılması aşağıdaki nedenlerden dolayı daha iyidir:
  
  * Tanılama uzantıları yüklenmeden daha az hareketli parça yüklenmesi gerekir.
  * Daha basit şablonlar. Varolan ölçek kümesi şablonuna öngörüler otomatik ölçeklendirme kuralları eklemeniz gerekiyor.
  * Daha güvenilir raporlama ve yeni VM'lerin daha hızlı başlatılması.
    
    Tanılama uzantısı kullanmaya devam etmek isteyebilirsiniz tek nedeni bellek tanılama raporlama / ölçekleme gerekiyorsa. Ana bilgisayar tabanlı ölçümler belleği bildirmez.
    
    Bunu göz önünde bulundurarak, yalnızca otomatik ölçeklendirme için tanı uzantıları kullanıyorsanız bu makalenin geri kalanını izleyin.
    
    Azure Kaynak Yöneticisi'nde otomatik ölçek, Tanılama Uzantısı adı verilen bir VM uzantısı sayesinde çalışabilir (ancak artık çalışmak zorunda değildir). Performans verilerini şablonda tanımladığınız bir depolama hesabına yayır. Bu veriler daha sonra Azure Monitor hizmeti tarafından toplanır.
    
    Insights hizmeti VM'lerden gelen verileri okuyamazsa, size bir e-posta göndermesi gerekir. Örneğin, VM'ler aşağıdaysa bir e-posta alırsınız. Azure hesabınızı oluşturduğunuzda belirttiğiniz e-posta adresinden e-postanızı kontrol ettiğinizden emin olun.
    
    Verilere kendiniz de bakabilirsiniz. Bulut gezgini kullanarak Azure depolama hesabına bakın. Örneğin, Visual [Studio Cloud Explorer'ı](https://visualstudiogallery.msdn.microsoft.com/aaef6e67-4d99-40bc-aacf-662237db85a2)kullanarak oturum açın ve kullanmakta olduğunuz Azure aboneliğini seçin. Ardından, dağıtım şablonunuzda Tanılama uzantısı tanımında başvurulan Tanılama depolama hesabı adına bakın.
    
    ![Cloud Explorer][explorer]
    
    Her VM'den gelen verilerin depolandığı bir grup tablo görürsünüz. Örnek olarak Linux ve CPU metrik alarak, en son satırları bakmak. Visual Studio bulut gezgini sorgu dilini destekler, böylece sorgu yuçalıştırabilirsiniz. Örneğin, en son olayları aldığınızdan emin olmak için "Timestamp gt datetime'2016-02-02T21:20:00Z' için bir sorgu çalıştırabilirsiniz. Saat dilimi UTC'ye karşılık gelir. Orada gördüğünüz veriler ayarladığınız ölçek kurallarına uyuyor mu? Aşağıdaki örnekte, makine 20 için CPU son beş dakika içinde% 100 artmaya başladı.
    
    ![Depolama Tabloları][tables]
    
    Veriler yoksa, sorunun VM'lerde çalışan tanı uzantısı ile ilgili olduğunu gösterir. Veriler varsa, ölçek kurallarınızla veya Öngörüler hizmetinde bir sorun olduğu anlamına gelir. [Azure Durumunu](https://azure.microsoft.com/status/)Denetleyin.
    
    Bu adımları geçtikten sonra, hala otomatik ölçeklendirme sorunları yaşıyorsanız, aşağıdaki kaynakları deneyebilirsiniz: 
    * [MSDN](https://social.msdn.microsoft.com/forums/azure/home?forum=WAVirtualMachinesforWindows)veya [Yığın taşması](https://stackoverflow.com/questions/tagged/azure) forumlarını okuyun 
    * Destek çağrısını günlüğe kaydedin. Şablonu ve performans verilerinizin görünümünü paylaşmaya hazır olun.

[audit]: ./media/virtual-machine-scale-sets-troubleshoot/image3.png
[explorer]: ./media/virtual-machine-scale-sets-troubleshoot/image1.png
[tables]: ./media/virtual-machine-scale-sets-troubleshoot/image4.png
