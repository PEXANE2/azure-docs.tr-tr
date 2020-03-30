---
title: PremiumV2 katmanını yapılandırma
description: Yeni PremiumV2 fiyatlandırma katmanına ölçekleyerek Azure Uygulama Hizmeti'ndeki web, mobil ve API uygulamanız için nasıl daha iyi performans sergileyip daha iyi performans sergileyerek nasıl daha iyi performans sergileyip daha iyi performans sergileyerek öğrenin.
keywords: app service, azure app service, ölçek, ölçeklenebilir, app service planı, app service maliyeti
ms.assetid: ff00902b-9858-4bee-ab95-d3406018c688
ms.topic: article
ms.date: 07/25/2018
ms.custom: seodec18
ms.openlocfilehash: f7f9f6d5617ad0f9be69c47ce514d395534fd892
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74672221"
---
# <a name="configure-premiumv2-tier-for-azure-app-service"></a>Azure Uygulama Hizmeti için PremiumV2 katmanını yapılandırın

Yeni **PremiumV2** fiyatlandırma katmanı size daha hızlı işlemciler, SSD depolama sağlar ve mevcut fiyatlandırma katmanlarının bellekten çekirdek oranına iki katına çıkar. Performans avantajı yla, uygulamalarınızı daha az örnekte çalıştırarak paradan tasarruf edebilirsiniz. Bu makalede, **PremiumV2** katmanında bir uygulama oluşturmayı veya bir uygulamayı **PremiumV2** katmanına nasıl ölçeklendirdiğinizi öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Bir uygulamayı **PremiumV2**olarak ölçeklendirmek için, **PremiumV2'den**daha düşük bir fiyatlandırma katmanında çalışan bir Azure Uygulama Hizmeti uygulamasına sahip olmanız ve uygulamanın PremiumV2'yi destekleyen bir Uygulama Hizmeti dağıtımında çalışıyor olması gerekir.

<a name="availability"></a>

## <a name="premiumv2-availability"></a>PremiumV2 kullanılabilirliği

**PremiumV2** katmanı, Hem _Windows'da_ hem de _Linux'ta_App Service için kullanılabilir.

**PremiumV2** çoğu Azure bölgesinde kullanılabilir. Bölgenizde kullanılabilir olup olmadığını görmek için [Azure Bulut BulutU'nda](../cloud-shell/overview.md)aşağıdaki Azure CLI komutunu çalıştırın:

```azurecli-interactive
az appservice list-locations --sku P1V2
```

<a name="create"></a>

## <a name="create-an-app-in-premiumv2-tier"></a>PremiumV2 katmanında uygulama oluşturma

Bir Uygulama Hizmeti uygulamasının fiyatlandırma katmanı, çalıştığı [Uygulama Hizmeti planında](overview-hosting-plans.md) tanımlanır. Tek başına veya uygulama oluşturmanın bir parçası olarak bir Uygulama Hizmeti planı oluşturabilirsiniz.

<a href="https://portal.azure.com" target="_blank">Azure portalında</a>Uygulama Hizmeti planını yapılandırırken **Fiyatlandırma katmanını**seçin. 

**Üretim'i**seçin, ardından **P1V2**, **P2V2**veya **P3V2'yi**seçin, ardından **Uygula'yı**tıklatın.

![](media/app-service-configure-premium-tier/scale-up-tier-select.png)

> [!IMPORTANT] 
> **P1V2**, **P2V2**ve **P3V2'yi** seçenek olarak görmüyorsanız veya seçenekler soluksa, **PremiumV2** büyük olasılıkla Uygulama Hizmeti planını içeren temel Uygulama Hizmeti dağıtımında kullanılamaz. Bkz. Daha fazla ayrıntı için [desteklenmeyen bir kaynak grubundan ve bölge birleşiminden](#unsupported) ölçeklendirin.

## <a name="scale-up-an-existing-app-to-premiumv2-tier"></a>Mevcut bir uygulamayı PremiumV2 katmanına ölçeklendirin

Mevcut bir uygulamayı **PremiumV2** katmanına ölçeklemeden önce **PremiumV2'nin** kullanılabilir olduğundan emin olun. Daha fazla bilgi için [PremiumV2 kullanılabilirliğine](#availability)bakın. Kullanılamıyorsa, [desteklenmeyen bir kaynak grubundan ve bölge birleşiminden ölçeklendir'e](#unsupported)bakın.

Barındırma ortamınıza bağlı olarak, ölçekleme için ekstra adımlar gerekebilir. 

Azure <a href="https://portal.azure.com" target="_blank">portalında</a>Uygulama Hizmeti uygulama sayfanızı açın.

Uygulama Hizmeti uygulama sayfanızın sol navigasyonunda, **Ölçeklendir (Uygulama Hizmeti planı)** seçeneğini belirleyin.

![](media/app-service-configure-premium-tier/scale-up-tier-portal.png)

**Üretim'i**seçin, ardından **P1V2**, **P2V2**veya **P3V2'yi**seçin, ardından **Uygula'yı**tıklatın.

![](media/app-service-configure-premium-tier/scale-up-tier-select.png)

İşleminiz başarıyla biterse, uygulamanızın genel bakış sayfası artık bir **PremiumV2** katmanında olduğunu gösterir.

![](media/app-service-configure-premium-tier/finished.png)

### <a name="if-you-get-an-error"></a>Bir hata alırsanız

Bazı Uygulama Hizmeti planları, temel Uygulama Hizmeti dağıtımı PremiumV2'yi desteklemiyorsa PremiumV2 katmanına kadar ölçeklendiremez.  Bkz. Daha fazla ayrıntı için [desteklenmeyen bir kaynak grubundan ve bölge birleşiminden](#unsupported) ölçeklendirin.

<a name="unsupported"></a>

## <a name="scale-up-from-an-unsupported-resource-group-and-region-combination"></a>Desteklenmeyen kaynak grubundan ve bölge birleşiminden ölçeklendirme

Uygulamanız **PremiumV2'nin** kullanılamadığı bir Uygulama Hizmeti dağıtımında çalışıyorsa veya uygulamanız şu anda **PremiumV2'yi**desteklemeyen bir bölgede çalışıyorsa, **PremiumV2'den**yararlanmak için uygulamanızı yeniden dağıtmanız gerekir.  İki seçeneğiniz vardır:

- **Yeni** bir kaynak grubu oluşturun ve ardından oluşturma işlemi sırasında istediğiniz Azure bölgesini seçerek **yeni** kaynak grubunda **yeni** bir uygulama ve Uygulama Hizmeti planı oluşturun.  Yeni uygulama hizmet planının oluşturulduğu anda **PremiumV2** planını **seçmeniz gerekir.**  Bu, kaynak grubu, Uygulama Hizmeti planı ve Azure bölgesinin **birleşiminin, PremiumV2'yi**destekleyen bir Uygulama Hizmeti dağıtımında App Service planının oluşturulmasıyla sonuçlanmasını sağlar.  Ardından, uygulama kodunuzu yeni oluşturulan uygulama ve uygulama hizmet planına yeniden dağıtın. İstenirseniz, daha sonra maliyetten tasarruf etmek için App Service planını **PremiumV2'den** küçültebilirsiniz ve gelecekte **PremiumV2'yi**kullanarak tekrar başarıyla ölçeklendirebilirsiniz.
- Uygulamanız zaten mevcut bir **Premium** katmanda çalışıyorsa, uygulamanızı tüm uygulama ayarları, bağlantı dizeleri ve dağıtım yapılandırmasıyla **PremiumV2**kullanan yeni bir uygulama hizmet planına kopyalayabilirsiniz.

    ![](media/app-service-configure-premium-tier/clone-app.png)

    Klon **uygulaması** sayfasında, istediğiniz bölgede **PremiumV2'yi** kullanarak bir Uygulama Hizmeti planı oluşturabilir ve klonlamak istediğiniz uygulama ayarlarını ve yapılandırmayı belirtebilirsiniz.

## <a name="automate-with-scripts"></a>Betiklerle otomatikleştirme

[Azure CLI](/cli/azure/install-azure-cli) veya [Azure PowerShell'i](/powershell/azure/overview)kullanarak **PremiumV2** katmanında uygulama oluşturmayı komut dosyalarıyla otomatikleştirebilirsiniz.

### <a name="azure-cli"></a>Azure CLI

Aşağıdaki komut _P1V2'de_bir Uygulama Hizmeti planı oluşturur. Cloud Shell'de çalıştırabilirsiniz. Seçenekler `--sku` Için P1V2, _P2V2_, ve _P3V2_.

```azurecli-interactive
az appservice plan create \
    --resource-group <resource_group_name> \
    --name <app_service_plan_name> \
    --sku P1V2
```

### <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Aşağıdaki komut _P1V2'de_bir Uygulama Hizmeti planı oluşturur. Seçenekler için `-WorkerSize` _Small_küçük, _orta_ve _büyük._

```powershell
New-AzAppServicePlan -ResourceGroupName <resource_group_name> `
    -Name <app_service_plan_name> `
    -Location <region_name> `
    -Tier "PremiumV2" `
    -WorkerSize "Small"
```
## <a name="more-resources"></a>Diğer kaynaklar

[Azure’da uygulamanın ölçeğini artırma](manage-scale-up.md)  
[Örnek sayısını el ile veya otomatik olarak ölçeklendirme](../monitoring-and-diagnostics/insights-how-to-scale.md)
