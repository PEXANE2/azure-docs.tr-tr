---
title: Azure HANA Büyük Örnekleri Azure portalı üzerinden kontrol | Microsoft Dokümanlar
description: Portal aracılığıyla Azure HANA Büyük Örneklerini nasıl tanımlayabileceğinizi ve bunlarla nasıl etkileşimde ebileceğinizi açıklar
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/15/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3c14ff9c4f6d2bc2b1a62d1874d01950d09491c0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70099810"
---
# <a name="azure-hana-large-instances-control-through-azure-portal"></a>Azure portalı üzerinden Azure HANA Büyük Örnekler denetimi
Bu belge, [HANA Büyük Örnekleri'nin](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) [Azure portalında](https://portal.azure.com) nasıl sunulduğunu ve sizin için dağıtılan HANA Büyük Örnek birimleriyle Azure portalı üzerinden hangi etkinliklerin gerçekleştirilebileceğini kapsar. Azure portalındaki HANA Büyük Örneklerinin görünürlüğü, şu anda genel önizlemede olan HANA Büyük Örnekleri için bir Azure kaynak sağlayıcısı aracılığıyla sağlanır

## <a name="register-hana-large-instance-resource-provider"></a>HANA Büyük Örnek Kaynak Sağlayıcısını Kaydedin
Genellikle HANA Büyük Örnek dağıtımları için kullandığınız Azure aboneliğiniz HANA Büyük Örnek Kaynak Sağlayıcısı'na kaydedilir. Ancak, HANA Büyük Örnek birimlerini dağıtdığınızı göremiyorsanız, Kaynak Sağlayıcısı'nı Azure aboneliğinize kaydettirmelisiniz. HANA Büyük Örnek Kaynak sağlayıcısını kaydetmenin iki yolu vardır

### <a name="register-through-cli-interface"></a>CLI arabirimi üzerinden kaydolun
Azure CLI arabirimi üzerinden HANA Büyük Örnek dağıtımı için kullanılan Azure aboneliğinizde oturum açmanız gerekir. HANA Büyük Örnek Sağlayıcı'yı bu komutla yeniden kaydedebilirsiniz:
    
    az provider register --namespace Microsoft.HanaOnAzure

Daha fazla bilgi için [Azure kaynak sağlayıcıları ve türleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services#azure-cli) makalesine bakın


### <a name="register-through-azure-portal"></a>Azure portalı üzerinden kaydolun
Azure portalı üzerinden HANA Büyük Örnek Kaynak Sağlayıcısı'nı yeniden kaydedebilirsiniz. Aboneliğinizi Azure portalında listeleniz ve HANA Büyük Örnek biriminizi(ler) dağıtmak için kullanılan aboneliğe çift tıklamanız gerekir. Aboneliğinizin genel bakış sayfasında yer aldığınızbir sayfada, aşağıda gösterildiği gibi "Kaynak sağlayıcıları" seçeneğini belirleyin ve arama penceresine "HANA" yazın. 

![Azure portalı üzerinden HLI RP'ye kaydolun](./media/hana-li-portal/portal-register-hli-rp.png)

Gösterilen ekran görüntüsünde, kaynak sağlayıcısı zaten kayıtlıydı. Kaynak sağlayıcısının henüz kaydedilmemiş olması durumunda "yeniden kaydol" veya "kaydol" tuşuna basın.

Daha fazla bilgi için [Azure kaynak sağlayıcıları ve türleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services#azure-powershell) makalesine bakın


## <a name="display-of-hana-large-instance-units-in-the-azure-portal"></a>AZURE portalında HANA Büyük Örnek birimlerinin görüntülenmesi
HANA Büyük Örnek dağıtım isteği gönderirken, HANA Büyük Örnekleri'ne bağlandığınız Azure aboneliğini de belirtmeniz istenir. HANA Büyük Örnek birimlerine karşı çalışan SAP uygulama katmanını dağıtmak için kullandığınız aboneliği kullanmanız önerilir.
İlk HANA Büyük Örnekleriniz dağıtılırken, HANA Büyük Örneği(ler) için dağıtım isteğinde gönderdiğiniz Azure aboneliğinde yeni bir [Azure kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/manage-resources-portal) oluşturulur.  Yeni kaynak grubu, belirli abonelikte dağıttığınız tüm HANA Büyük Örnek birimlerinizi listeler.

Yeni Azure kaynak grubunu bulmak için, Azure portalının sol gezinti bölmesinde gezinerek aboneliğinizdeki kaynak grubunu listelersiniz

![Azure portalında gezinti bölmesi](./media/hana-li-portal/portal-resource-group.png)

Kaynak grupları listesinde, listeleniyorsanız, HANA Büyük Örnekleri'nin dağıtılması için kullandığınız aboneliğe filtre uygulamanız gerekebilir

![Azure portalındaki kaynak gruplarını filtreleme](./media/hana-li-portal/portal-filtering-subscription.png)

Doğru aboneliği filtreledikten sonra, kaynak gruplarının uzun bir listesi ne kadar olabilir. "xxx" **-T050**gibi üç basamaklı **-Txxx** bir post-fix ile arayın. 

Kaynak grubunu bulduğunuzda, ayrıntılarını listele. Aldığınız liste aşağıdaki gibi görünebilir:

![Azure portalında HLI listesi](./media/hana-li-portal/portal-hli-units-list.png)

Listelenen tüm birimler, aboneliğinizde dağıtılan tek bir HANA Büyük Örnek birimini temsil ediyor. Bu durumda, aboneliğinizde dağıtılan sekiz farklı HANA Büyük Örnek birimine bakAbilirsiniz.

Aynı Azure aboneliği altında birden fazla HANA Büyük Örnek kiracısı dağıttıysanız, birden çok Azure kaynak grubu bulursunuz 


## <a name="look-at-attributes-of-single-hli-unit"></a>Tek HLI Biriminin özelliklerine bakın
HANA Büyük Örnek birimleri listesinde, tek bir üniteyi tıklayabilir ve tek HANA Büyük Örnek biriminin ayrıntılarına ulaşabilirsiniz. 

Genel bakış ekranında, 'Daha fazlasını göster'i tıklattıktan sonra, ünitenin aşağıdaki gibi görünen bir sunusunu elde eeyseniz:

![Bir HLI birimine genel bakış](./media/hana-li-portal/portal-show-overview.png)

Gösterilen farklı özniteliklere bakıldığında, bu öznitelikler Azure VM özniteliklerinden çok farklı görünüyor. Sol taraftaki üstbilgide Kaynak grubunu, Azure bölgesini, abonelik adını ve kimliğinin yanı sıra eklediğiniz bazı etiketleri gösterir. Varsayılan olarak, HANA Büyük Örnek birimlerinin atanmış etiketi yoktur. Üstbilginin sağ tarafında, dağıtım yapıldığında birimin adı atanmış olarak listelenir. Ip adresinin yanı sıra işletim sistemi de gösterilir. VM'lerde olduğu gibi, CPU iş parçacığı ve bellek sayısına sahip HANA Büyük örnek birim türü de gösterilir. Farklı HANA Büyük Örnek birimleri hakkında daha fazla bilgi, burada gösterilmiştir:

- [HLI için kullanılabilir SKU'lar](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-available-skus)
- [SAP HANA (Büyük Örnekler) depolama mimarisi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-storage-architecture) 

Sağ alt taraftaki ek veriler HANA Büyük Örnek damgasının revizyonudur. Olası değerler şunlardır:

- Revizyon 3
- Revizyon 4

Revizyon 4, Azure VM'leri ile Revizyon 4 damgalarında veya satırlarında dağıtılan HANA Büyük örnek birimleri arasında ağ gecikmesinde önemli iyileştirmeler içeren HANA Büyük Örnekleri'nin en son mimarisidir.
Başka bir çok önemli bilgi, genel bakışın sağ alt köşesinde, dağıtılan her HANA Büyük Örnek birimi için otomatik olarak oluşturulan Azure Yakınlık Yerleşim Grubu'nun adı ile bulunur. SAP uygulama katmanını barındıran Azure VM'leri dağıtılırken bu Yakınlık Yerleşim Grubu'na başvurulması gerekir. HANA Büyük Örnek birimiyle ilişkili [Azure yakınlık yerleşim grubunu](https://docs.microsoft.com/azure/virtual-machines/linux/co-location) kullanarak, Azure VM'lerinin HANA Büyük Örnek birimine yakın bir yerde dağıtıldığınızdan emin olabilirsiniz. Sapma 4 barındırılan HANA Büyük Örnek birimleriyle aynı Azure veri merkezinde SAP uygulama katmanını bulmak için yakınlık yerleşim işlemi gruplarının kullanılabilir ışığı, [SAP uygulamalarıyla en iyi ağ gecikmesi için Azure Yakınlık ışı Bulma Grupları'](sap-proximity-placement-scenarios.md)nda anlatılıyor.

Üstbilginin sağ sütunundaki ek bir alan, HANA Büyük örnek biriminin güç durumu hakkında bilgi verir.

> [!NOTE]
> Güç durumu, donanım biriminin açık mı yoksa kapalı mı olduğunu açıklar. İşletim sisteminin çalışır durumda olması hakkında bilgi vermez. Bir HANA Büyük Örnek birimini yeniden başlattığınızda, birimin durumunun **Başlangıç**durumuna geçmeye **başlayarak** değiştiği küçük bir zaman yaşayacaksınız. **Başlangıç** durumunda olmak, işletim sistemi başlangıç veya işletim sistemi tamamen başlatıldı anlamına gelir. Sonuç olarak, ünitenin yeniden başlatıldıktan sonra, durum **Başlatıldı'ya**geçer girmez üniteye hemen giriş yapmayı bekleyemezsiniz.
> 

'Daha fazla bilgi gör' tuşuna baslarsanız, ek bilgiler gösterilir. Ek bir bilgi HANA Büyük Örnek damgası revizyonu görüntüleniyor, birim konuşlandırıldı var. HANA Büyük Örnek pullarının farklı düzeltmeleri için [Azure'da SAP HANA (Büyük Örnekler) nedir](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) makalesine bakın

## <a name="check-activities-of-a-single-hana-large-instance-unit"></a>Tek bir HANA Büyük Örnek biriminin faaliyetlerini denetleyin 
HANA Büyük Örnek birimlerine genel bir bakış vermenin ötesinde, belirli birimin faaliyetlerini kontrol edebilirsiniz. Bir etkinlik günlüğü şu şekilde görünebilir:

![Azure portalında gezinti bölmesi](./media/hana-li-portal/portal-activity-list.png)

Kaydedilen ana faaliyetlerden biri bir birimin yeniden başlatılmasıdır. Listelenen veriler, etkinliğin durumunu, etkinliğin tetiklendiği zaman damgasını, etkinliğin tetiklendiği abonelik kimliğini ve etkinliği tetikleyen Azure kullanıcısını içerir. 

Kaydedilen bir diğer etkinlik de Azure meta verilerindeki birimdeki değişikliklerdir. Başlatılan yeniden başlatmanın yanı sıra, **YAZMA HANAInstances**etkinliğini görebilirsiniz. Bu tür bir etkinlik HANA Büyük Örnek biriminde hiçbir değişiklik gerçekleştirmez, ancak Azure'daki birimin meta verilerindeki değişiklikleri belgeletir. Listelenen durumda, bir etiket ekledik ve sildik (sonraki bölüme bakın).

## <a name="add-and-delete-an-azure-tag-to-a-hana-large-instance-unit"></a>HANA Büyük Örnek birimine Azure etiketi ekleme ve silme
Sahip olduğunuz bir diğer olasılık da HANA Büyük Örnek birimine bir [etiket](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) eklemektir. Etiketlerin atanma şekli, Etiketleri VM'lere atamaktan farklı değildir. VM'lerde olduğu gibi, etiketler Azure meta verilerinde bulunur ve HANA Büyük Örnekleri için, VM'ler için etiketlerle aynı kısıtlamalara sahiptir.

Etiketleri silme, VM'lerde olduğu gibi çalışır. Her iki etkinlik, bir etiket uygulama ve silme belirli HANA Büyük Örnek biriminin etkinlik günlüğünde listelenir.

## <a name="check-properties-of-a-hana-large-instance-unit"></a>HANA Büyük Örnek biriminin özelliklerini denetleme
**Özellikler** bölümü, örnekler size teslim edildiğinde aldığınız önemli bilgileri içerir. Destek durumlarda gerektirebileceğiniz veya depolama anlık görüntü yapılandırmasını ayarlarken ihtiyaç duyduğunuz tüm bilgileri aldığınız bir bölümdür. Bu nedenle, bu bölüm örneğiniz etrafında veri topluluğu, örneğin Azure'a bağlantısı ve depolama arka ucudur. Bölümün üst kısmı aşağıdaki gibi görünür:


![Azure portalındaki HLI özelliklerinin üst kısmı](./media/hana-li-portal/portal-properties-top.png)

İlk birkaç veri öğesi, zaten genel bakış ekranında gördüm. Ancak verilerin önemli bir kısmı, ilk konuşlandırılan birimler teslim edilirken aldığınız ExpressRoute Circuit KIMLIĞI'dir. Bazı destek durumlarında, bu veriler istenebilir. Önemli bir veri girişi ekran görüntüsünün alt kısmında gösterilir. Görüntülenen veriler, HANA Büyük Örnek yığınındaki deponuzu **kiracınıza** yalıtan NFS depolama şefinin IP adresidir. Depolama [anlık görüntü yedeklemeleri için yapılandırma dosyasını](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-backup-restore#set-up-storage-snapshots)düzenlemeyaptığınızda bu IP adresi de gereklidir. 

Özellik bölmesinde aşağı kaydırdığınızda, HANA Büyük Örnek biriminiz için benzersiz bir kaynak kimliği veya dağıtıma atanan abonelik kimliği gibi ek veriler elde elabilirsiniz.

## <a name="restart-a-hana-large-instance-unit-through-azure-portal"></a>Azure portalı üzerinden HANA Büyük Örnek birimini yeniden başlatın
Linux işletim sisteminin yeniden başlatılmasıbaşlatılırken, işletim sisteminin yeniden başlatmayı başarıyla tamamlayamadığı çeşitli durumlar vardı. Yeniden başlatmayı zorlamak için, Microsoft işlemlerinin HANA Büyük Örnek biriminin güç yeniden başlatmasını sağlamak için bir hizmet isteği açmanız gerekiyordu. HANA Büyük Örnek biriminin güç yeniden başlatma işlevi artık Azure portalına entegre edilmiştir. HANA Büyük Örnek biriminin genel bakış bölümünde olduğunuz için, veri bölümünün üstünde yeniden başlatma düğmesini görürsünüz

![Azure portalında adımı #1 yeniden başlatın](./media/hana-li-portal/portal-restart-first-step.png)

Yeniden başlatma düğmesine bastığınızda, üniteyi gerçekten yeniden başlatmak isteyip istemediğiniz sorulur. "Evet" düğmesine basarak onayladığınız gibi, ünite yeniden başlatılacaktır.

> [!NOTE]
> Yeniden başlatma işleminde, birimin durumunun **Başlangıç**durumuna geçmeye **başlayarak** değiştiği küçük bir zaman yaşayacaksınız. **Başlangıç** durumunda olmak, işletim sistemi başlangıç veya işletim sistemi tamamen başlatıldı anlamına gelir. Sonuç olarak, ünitenin yeniden başlatıldıktan sonra, durum **Başlatıldı'ya**geçer girmez üniteye hemen giriş yapmayı bekleyemezsiniz.

> [!IMPORTANT]
> HANA Büyük Örnek biriminizdeki bellek miktarına bağlı olarak, donanımın yeniden başlatılması ve yeniden başlatılması ve işletim sisteminin bir saat kadar sürebilir


## <a name="open-a-support-request-for-hana-large-instances"></a>HANA büyük Örnekleri için bir destek isteği açın
HANA Büyük Örnek birimlerinin Azure portalı ekranından, özellikle HANA büyük Örnek birimi için destek istekleri oluşturabilirsiniz. Bağlantıyı takip ettikçe **Yeni destek isteği** 

![Azure portalında hizmet isteği adım #1 başlatma](./media/hana-li-portal/portal-initiate-support-request.png)

SAP HANA Büyük Örnekleri'nin hizmetini bir sonraki ekranda listelemek için aşağıda gösterildiği gibi 'Tüm Hizmetler'i seçmeniz gerekebilir

![Azure portalındaki tüm hizmetleri seçin](./media/hana-li-portal/portal-create-service-request.png)

Hizmetler listesinde, hizmet **SAP HANA Büyük Örnek**bulabilirsiniz. Bu hizmeti seçerken, gösterildiği gibi belirli sorun türlerini seçebilirsiniz:


![Azure portalında sorun sınıfı seçin](./media/hana-li-portal/portal-select-problem-class.png)

Farklı sorun türlerinin her birinin altında, sorununuzu daha fazla karakterize etmek için seçmeniz gereken bir dizi sorun alt türü sunulur. Alt türü seçtikten sonra, artık özne yi adlandırabilirsiniz. Seçim işlemini bitirdikten sonra, oluşturmanın bir sonraki adımına geçebilirsiniz. **Çözümler** bölümünde, HANA Büyük Örnekleri etrafında belgelere işaret edilirsiniz, bu da sorununuzun çözümüne işaret verebilir. Önerilen belgelerde sorununuzun bir çözüm bulamıyorsanız, bir sonraki adıma gidersiniz. Bir sonraki adımda, sorunun VM'lerde mi yoksa HANA Büyük Örnek birimleriyle mi olduğu sorulur. Bu bilgiler, destek isteğini doğru uzmanlara yönlendirmeye yardımcı olur. 

![Azure portalında destek durumu ayrıntıları](./media/hana-li-portal/portal-support-request-details.png)

Soruları yanıtladıkça ve ek ayrıntılar verdikçe, destek isteğini gözden geçirmek ve göndermek için bir sonraki adımı atabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure'da SAP HANA (büyük örnekler) nasıl izlenir?](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/troubleshooting-monitoring)
- [HANA tarafından izleme ve sorun giderme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-monitor-troubleshoot)

