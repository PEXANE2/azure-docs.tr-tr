---
title: Azure 'da BareMetal örnek birimlerini bağlama
description: Azure portal veya Azure CLı ile BareMetal örnek birimlerini nasıl tanımlayacağınızı ve bunlarla etkileşime gireceğini öğrenin.
ms.topic: how-to
ms.subservice: workloads
ms.date: 03/19/2021
ms.openlocfilehash: 60a36deb00ab6c2399d25a880bce8fe624d9609f
ms.sourcegitcommit: 2c1b93301174fccea00798df08e08872f53f669c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2021
ms.locfileid: "104776250"
---
# <a name="connect-baremetal-instance-units-in-azure"></a>Azure 'da BareMetal örnek birimlerini bağlama
 
Bu makalede, [Azure Portal](https://portal.azure.com/) [Baremetal örnekleri](concepts-baremetal-infrastructure-overview.md)nasıl görüntüleyeceği gösterilmektedir. Bu makale ayrıca, dağıtılan BareMetal örnek birimlerinizle Azure portal yapabileceğiniz etkinlikleri gösterir. 
 
## <a name="register-the-resource-provider"></a>Kaynak sağlayıcısını kaydetme
BareMetal örnekleri için bir Azure Kaynak sağlayıcısı, şu anda genel önizlemede olan Azure portal örneklerin görünürlüğünü sağlar. Varsayılan olarak, BareMetal örnek dağıtımları için kullandığınız Azure aboneliği *BareMetalInfrastructure* kaynak sağlayıcısını kaydeder. Dağıtılan BareMetal örnek birimlerinizi görmüyorsanız, kaynak sağlayıcısını aboneliğinize kaydetmeniz gerekir. 

Azure portal veya Azure CLı kullanarak BareMetal örnek kaynak sağlayıcısını kaydedebilirsiniz.

### <a name="portal"></a>[Portal](#tab/azure-portal)
 
Aboneliğinizi Azure portal listeetmeniz ve sonra BareMetal örnek birimlerinizi dağıtmak için kullanılan aboneliğe çift tıklamaları gerekir.
 
1. [Azure portalında](https://portal.azure.com) oturum açın.

1. Azure portalı menüsünde **Tüm hizmetler**’i seçin.

1. **Tüm hizmetler** kutusunda, **abonelik** girin ve ardından **abonelikler**' i seçin.

1. Görüntülenecek abonelik listesinden aboneliği seçin.

1. **Kaynak sağlayıcıları** ' nı seçin ve aramaya **BareMetalInfrastructure** girin. Görüntünün gösterdiği gibi, kaynak sağlayıcının **kayıtlı** olması gerekir.
 
>[!NOTE]
>Kaynak sağlayıcı kayıtlı değilse, **Kaydet**' i seçin.
 
:::image type="content" source="media/baremetal-infrastructure-portal/register-resource-provider-azure-portal.png" alt-text="Kayıtlı BareMetal örnek birimini gösteren ekran görüntüsü":::

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı kullanmaya başlamak için:

[!INCLUDE [azure-cli-prepare-your-environment-no-header](../../includes/azure-cli-prepare-your-environment-no-header.md)]

Azure CLı aracılığıyla BareMetal örnek dağıtımı için kullandığınız Azure aboneliğinde oturum açın. `BareMetalInfrastructure` [Az Provider Register](/cli/azure/provider#az_provider_register) komutuyla kaynak sağlayıcısını kaydedin:

```azurecli
az provider register --namespace Microsoft.BareMetalInfrastructure
```

Kullanılabilir tüm sağlayıcıları görmek için [az Provider List](/cli/azure/provider#az_provider_list) komutunu kullanabilirsiniz.

---

Kaynak sağlayıcıları hakkında daha fazla bilgi için bkz. [Azure kaynak sağlayıcıları ve türleri](../azure-resource-manager/management/resource-providers-and-types.md).  

## <a name="baremetal-instance-units-in-the-azure-portal"></a>Azure portal BareMetal örnek birimleri
 
Bir BareMetal örnek dağıtım isteği gönderdiğinizde, BareMetal örneklerine bağlanmakta olduğunuz Azure aboneliğini belirtirsiniz. Karemetal örnek birimlerine göre çalışarak uygulama katmanını dağıtmak için kullandığınız aboneliği kullanın.
 
BareMetal örneklerinizin dağıtımı sırasında, dağıtım isteğinde kullandığınız Azure aboneliğinde yeni bir [Azure Kaynak grubu](../azure-resource-manager/management/manage-resources-portal.md) oluşturulur. Bu yeni kaynak grubu, belirli bir abonelikte dağıttığınız tüm BareMetal örnek birimlerinizi listeler.

### <a name="portal"></a>[Portal](#tab/azure-portal)

1. BareMetal aboneliğinde, Azure portal **kaynak grupları**' nı seçin.
 
   :::image type="content" source="media/baremetal-infrastructure-portal/view-baremetal-instance-units-azure-portal.png" alt-text="Kaynak gruplarının listesini gösteren ekran görüntüsü":::

1. Listede yeni kaynak grubunu bulun.
 
   :::image type="content" source="media/baremetal-infrastructure-portal/filter-resource-groups.png" alt-text="Filtrelenmiş kaynak grupları listesinde BareMetal örnek birimini gösteren ekran görüntüsü" lightbox="media/baremetal-infrastructure-portal/filter-resource-groups.png":::
   
   >[!TIP]
   >BareMetal örneğini dağıtmak için kullandığınız abonelikte filtre uygulayabilirsiniz. Uygun aboneliğe filtre uyguladıktan sonra, uzun kaynak grupları listeniz olabilir. - **TxXx** sonrası- **T250** gibi üç basamaklı bir çözüm arayın.

1. Ayrıntılarını göstermek için yeni kaynak grubunu seçin. Görüntüde bir BareMetal örnek birimi dağıtıldı.
   
   >[!NOTE]
   >Aynı Azure aboneliği altında birkaç BareMetal örnek kiracılar dağıttıysanız, birden çok Azure Kaynak grubu görürsünüz.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Tüm BareMetal örneklerinizi görmek için, kaynak grubunuz için [az baremetalinstance List](/cli/azure/ext/baremetal-infrastructure/baremetalinstance#ext_baremetal_infrastructure_az_baremetalinstance_list) komutunu çalıştırın:

```azurecli
az baremetalinstance list --resource-group DSM05A-T550 –output table
```

> [!TIP]
> `--output`Parametresi, tüm komutlarda kullanılabilen genel bir parametredir. **Tablo** değeri, çıktıyı kolay bir biçimde sunar. Daha fazla bilgi için bkz. [Azure CLI komutları Için çıkış biçimleri](/cli/azure/format-output-azure-cli).

---

## <a name="view-the-attributes-of-a-single-instance"></a>Tek bir örneğin özniteliklerini görüntüleme

Tek bir birimin ayrıntılarını görüntüleyebilirsiniz.

### <a name="portal"></a>[Portal](#tab/azure-portal)

BareMetal örneği listesinde, görüntülemek istediğiniz tek örneği seçin.
 
:::image type="content" source="media/baremetal-infrastructure-portal/view-attributes-single-baremetal-instance.png" alt-text="Tek bir örneğin BareMetal örnek birimi özniteliklerini gösteren ekran görüntüsü" lightbox="media/baremetal-infrastructure-portal/view-attributes-single-baremetal-instance.png":::
 
Görüntüdeki öznitelikler Azure sanal makine (VM) özniteliklerinden çok farklı değildir. Sol tarafta, kaynak grubu, Azure bölgesi ve abonelik adı ile KIMLIĞINI görürsünüz. Etiketler atadıysanız, bunları burada da görürsünüz. Varsayılan olarak, BareMetal örnek birimlerine Etiketler atanmaz.
 
Sağ tarafta, birim adı, işletim sistemi (OS), IP adresi ve CPU iş parçacığı sayısını ve bellek sayısını gösteren SKU 'YU görürsünüz. Ayrıca güç durumu ve donanım sürümünü (BareMetal örnek damgasının düzeltmesi) görürsünüz. Güç durumu, donanım biriminin açık veya kapalı olduğunu gösterir. Ancak işletim sistemi ayrıntıları, çalışıp çalışmadığını göstermez.
 
Olası donanım düzeltmeleri şunlardır:

* Düzeltme 3 (Rev 3)

* Düzeltme 4 (Rev 4)
 
* Düzeltme 4,2 (Rev 4,2)
 
>[!NOTE]
>Rev 4,2, var olan Rev 4 mimarisini kullanan en son yeniden markalı BareMetal altyapısıdır. Rev 4, Azure sanal makinesi (VM) konaklarına daha yakın bir yakınlık sağlar. Bu, Azure VM 'Ler ile karemetal örnek birimleri arasında, Rev 4 Damgalarında veya satırlarda dağıtılan ağ gecikmesi açısından önemli geliştirmeler sunar. BareMetal örneklerinizi Azure portal aracılığıyla erişebilir ve yönetebilirsiniz. Daha fazla bilgi için bkz. [Azure 'Da Baremetal altyapısı](concepts-baremetal-infrastructure-overview.md).
 
Ayrıca, sağ tarafta, dağıtılan her BareMetal örnek birimi için otomatik olarak oluşturulan [Azure yakınlık yerleşimi grubunun](../virtual-machines/co-location.md) adını bulacaksınız. Uygulama katmanını barındıran Azure VM 'lerini dağıtırken yakınlık yerleşimi grubuna başvurun. BareMetal örnek birimiyle ilişkili yakınlık yerleşimi grubunu kullandığınızda Azure VM 'lerinin BareMetal örnek birimine yakın bir şekilde dağıtılmasını sağlayabilirsiniz.
 
>[!TIP]
>Uygulama katmanını düzeltme 4. x ile aynı Azure veri merkezinde bulmak için bkz. [en iyi ağ gecikmesi Için Azure yakınlık yerleşimi grupları](/azure/virtual-machines/workloads/sap/sap-proximity-placement-scenarios).

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Bir BareMetal örneğinin ayrıntılarını görmek için [az baremetalinstance Show](/cli/azure/ext/baremetal-infrastructure/baremetalinstance#ext_baremetal_infrastructure_az_baremetalinstance_show) komutunu çalıştırın:

```azurecli
az baremetalinstance show --resource-group DSM05A-T550 --instance-name orcllabdsm01
```

Örnek adından emin değilseniz, `az baremetalinstance list` yukarıda açıklanan komutu çalıştırın.

---
 
## <a name="check-activities-of-a-single-instance"></a>Tek bir örneğin etkinliklerini denetleme
 
Tek bir birimin etkinliklerini kontrol edebilirsiniz. Kaydedilen ana etkinliklerden biri birimin yeniden başlatılmalardır. Listelenen veriler etkinliğin durumunu, tetiklenen etkinlik zaman damgasını, abonelik KIMLIĞINI ve etkinliği tetikleyen Azure kullanıcısını içerir.
 
:::image type="content" source="media/baremetal-infrastructure-portal/check-activities-single-baremetal-instance.png" alt-text="BareMetal örnek birimi etkinliklerini gösteren ekran görüntüsü" lightbox="media/baremetal-infrastructure-portal/check-activities-single-baremetal-instance.png":::
 
Ayrıca, Azure 'daki birimin meta verilerinde yapılan değişiklikler etkinlik günlüğüne kaydedilir. Yeniden başlatmanın başlatıldığı yanında, **Write BareMetallnstances** etkinliğini görebilirsiniz. Bu etkinlik BareMetal örnek biriminde hiçbir değişiklik yapmaz, ancak bu değişiklikleri Azure 'daki birimin meta verilerinde belgeler.
 
Kaydedilen başka bir etkinlik, bir örneğe etiket eklediğinizde veya bir [etiketi](../azure-resource-manager/management/tag-resources.md) sildiğinizde.
 
## <a name="add-and-delete-an-azure-tag-to-an-instance"></a>Azure etiketini bir örneğe ekleme ve silme

### <a name="portal"></a>[Portal](#tab/azure-portal)
 
Bir BareMetal örnek birimine Azure etiketleri ekleyebilir veya bunları silebilirsiniz. Etiketlerin atanabileceği yol, sanal makinelere etiket atamaya farklılık gösterir. VM 'Lerde olduğu gibi, Etiketler Azure meta verilerinde bulunur ve BareMetal örnekleri için, sanal makinelerin etiketleriyle aynı kısıtlamalara sahiptirler.
 
Etiketlerin silinmesi VM 'lerle aynı şekilde çalışır. Bir etiketi uygulamak ve silmek BareMetal örnek biriminin etkinlik günlüğünde listelenir.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

BareMetal örneklerine etiket atamak sanal makinelerle aynı şekilde çalışacaktır. Etiketler Azure meta verilerinde bulunur ve BareMetal örnekleri için, sanal makinelerin etiketleriyle aynı kısıtlamalara sahiptirler.

Bir BareMetal örnek birimine Etiketler eklemek için [az baremetalinstance Update](/cli/azure/ext/baremetal-infrastructure/baremetalinstance#ext_baremetal_infrastructure_az_baremetalinstance_update) komutunu çalıştırın:

```azurecli
az baremetalinstance update --resource-group DSM05a-T550 --instance-name orcllabdsm01 --set tags.Dept=Finance tags.Status=Normal
```

Bir etiketi kaldırmak için aynı komutu kullanın:

```azurecli
az baremetalinstance update --resource-group DSM05a-T550 --instance-name orcllabdsm01 --remove tags.Dept
```

---

## <a name="check-properties-of-an-instance"></a>Bir örneğin özelliklerini denetleme
 
Örnekleri aldığınızda, örneklerle ilgili toplanan verileri görüntülemek için Özellikler bölümüne gidebilirsiniz. Toplanan veriler arasında Azure bağlantısı, depolama arka ucu, ExpressRoute bağlantı hattı KIMLIĞI, benzersiz kaynak KIMLIĞI ve abonelik KIMLIĞI yer alır. Bu bilgileri destek isteklerinde veya depolama anlık görüntü yapılandırmasını ayarlarken kullanacaksınız.
 
Göreceğiniz diğer önemli bilgi, depolama NFS IP adresidir. BareMetal örnek yığınında depolama alanınızı, **kiracınıza** ayırır. [Depolama anlık görüntüsü yedeklemeleri için yapılandırma dosyasını](../virtual-machines/workloads/sap/hana-backup-restore.md#set-up-storage-snapshots)DÜZENLERKEN bu IP adresini kullanacaksınız.
 
:::image type="content" source="media/baremetal-infrastructure-portal/baremetal-instance-properties.png" alt-text="BareMetal örnek özelliği ayarlarını gösteren ekran görüntüsü" lightbox="media/baremetal-infrastructure-portal/baremetal-instance-properties.png":::
 
## <a name="restart-a-unit-through-the-azure-portal"></a>Azure portal aracılığıyla bir birimi yeniden başlatma

İşletim sisteminin, BareMetal örnek biriminin güç yeniden başlatılmasını gerektiren bir yeniden başlatmayı tamamlayamayabileceği çeşitli durumlar vardır.

### <a name="portal"></a>[Portal](#tab/azure-portal)

Birimin güç yeniden başlatılmasını doğrudan Azure portal yapabilirsiniz:
 
Birimin yeniden başlatılmasını onaylamak için **Yeniden Başlat** ' ı ve ardından **Evet** ' i seçin.
 
:::image type="content" source="media/baremetal-infrastructure-portal/baremetal-instance-restart.png" alt-text="BareMetal örnek birimini yeniden başlatma işlemini gösteren ekran görüntüsü":::
 
Bir BareMetal örnek birimini yeniden başlattığınızda bir gecikme yaşarsınız. Bu gecikme sırasında, güç durumu **Başlangıç** **durumundan başlangıç durumuna taşınıyor, bu** da işletim sisteminin tamamen başlatıldığı anlamına gelir. Sonuç olarak, yeniden başlatmadan sonra durum ' a geçiş yapıldıktan hemen sonra birimde oturum **açamazsınız.**

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Bir BareMetal örnek birimini yeniden başlatmak için [az baremetalinstance restart](/cli/azure/ext/baremetal-infrastructure/baremetalinstance#ext_baremetal_infrastructure_az_baremetalinstance_restart) komutunu kullanın:

```azurecli
az baremetalinstance restart --resource-group DSM05a-T550 --instance-name orcllabdsm01
```

---

>[!IMPORTANT]
>BareMetal örnek biriminizdeki bellek miktarına bağlı olarak, donanımın ve işletim sisteminin yeniden başlatılması ve yeniden başlatılması bir saate kadar sürebilir.
 
## <a name="open-a-support-request-for-baremetal-instances"></a>BareMetal örnekleri için bir destek isteği açın
 
Özellikle bir BareMetal örnek birimi için destek istekleri gönderebilirsiniz.
1. Azure portal, **Yardım + Destek** altında, **[Yeni bir destek isteği](https://rc.portal.azure.com/#create/Microsoft.Support)** oluşturun ve bilet için aşağıdaki bilgileri sağlayın:
 
   - **Sorun türü:** Sorun türü seçin
 
   - **Abonelik:** Aboneliğinizi seçin
 
   - **Hizmet:** BareMetal altyapısı
 
   - **Kaynak:** Örneğin adını belirtin
 
   - **Özet:** İsteğiniz için bir Özet sağlayın
 
   - **Sorun türü:** Sorun türü seçin
 
   - **Sorun alt türü:** Sorun için bir alt tür seçin

1. Sorununuza yönelik bir çözüm bulmak için **çözümler** sekmesini seçin. Bir çözüm bulamıyorsanız bir sonraki adıma gidin.

1. **Ayrıntılar** sekmesini seçin ve sorunun VM 'ler mi yoksa BareMetal örnek birimleri mi olduğunu seçin. Bu bilgiler, destek talebinin doğru uzmanlarına yönlendirilmesine yardımcı olur.

1. Sorunun başladığını belirtin ve örnek bölgesini seçin.

1. İstek hakkında daha fazla ayrıntı sağlayın ve gerekirse bir dosyayı karşıya yükleyin.

1. İsteği göndermek için **gözden geçir + oluştur** ' u seçin.
 
Bir destek temsilcisinin isteğinizi onaylamasını beş adede kadar iş günü sürer.

## <a name="next-steps"></a>Sonraki adımlar

İş yükleri hakkında daha fazla bilgi edinmek istiyorsanız bkz. [Baremetal iş yükü türleri](../virtual-machines/workloads/sap/get-started.md).