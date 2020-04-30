---
title: Form tanıyıcı örnek etiketleme aracını dağıtma
titleSuffix: Azure Cognitive Services
description: Denetimli öğrenme konusunda yardımcı olmak için form tanıyıcı örnek etiketleme aracını dağıtmanıza yönelik farklı yollar edinin.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: how-to
ms.date: 04/14/2020
ms.author: pafarley
ms.openlocfilehash: 27afbafcadb4c482e97e1d003706e7d2712e63c9
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82117276"
---
# <a name="deploy-the-sample-labeling-tool"></a>Örnek etiketleme aracını dağıtma

Form tanıyıcı örnek etiketleme aracı basit bir kullanıcı arabirimi (UI) sağlayan bir uygulamadır. Bu, denetimli öğrenme amacıyla formları (belgeler) el ile etiketlemek için kullanabileceğiniz bir uygulamadır. Bu makalede, aşağıdakileri nasıl yapılacağını öğreten bağlantılar ve yönergeler sunuyoruz:

* [Örnek etiketleme aracını yerel olarak çalıştırma](#run-the-sample-labeling-tool-locally)
* [Örnek etiketleme aracını bir Azure Container Instance 'a (acı) dağıtma](#deploy-with-azure-container-instances-aci)
* [Açık kaynak OCR formu etiketleme aracını kullanma ve bunlara katkıda bulunma](#open-source-on-github)

## <a name="run-the-sample-labeling-tool-locally"></a>Örnek etiketleme aracını yerel olarak çalıştırma

Verileri etiketlemenin en hızlı yolu, örnek etiketleme aracını yerel olarak çalıştırkullanmaktır. Aşağıdaki hızlı başlangıçta, el ile etiketlenmiş verilerle özel bir modeli eğiten, form tanıyıcı REST API ve örnek etiketleme aracı kullanılmaktadır. 

* [Hızlı başlangıç: formları etiketleme, model eğitme ve örnek etiketleme aracını kullanarak bir formu analiz etme](./quickstarts/label-tool.md).

## <a name="deploy-with-azure-container-instances-aci"></a>Azure Container Instances (ACI) ile dağıtma

Başlamadan önce, örnek etiketleme aracını bir Azure Container Instance 'a (acı) dağıtmanın iki yolu olduğunu unutmayın. Örnek etiketleme aracını ACI ile çalıştırmak için her iki seçenek de kullanılır: 

* [Azure portalını kullanma](#azure-portal)
* [Azure CLI kullanma](#azure-cli)

### <a name="azure-portal"></a>Azure portalı

Azure portal kullanarak yeni bir kaynak oluşturmak için aşağıdaki adımları izleyin: 

1. [Azure Portal](https://portal.azure.com/signin/index/) oturum açın.
2. **Kaynak oluştur**’u seçin. 
3. Ardından **Web uygulaması**' nı seçin. 

   > [!div class="mx-imgBorder"]
   > ![Web uygulaması Seç](./media/quickstarts/formre-create-web-app.png)
   
4. İlk olarak, **temel bilgiler** sekmesinin seçili olduğundan emin olun. Şimdi bazı bilgiler sağlamanız gerekir: 

   > [!div class="mx-imgBorder"]
   > ![Temelleri seçin](./media/quickstarts/formre-select-basics.png)
   * Abonelik-mevcut bir Azure aboneliğini seçin
   * Kaynak grubu-var olan bir kaynak grubunu yeniden kullanabilir veya bu proje için yeni bir tane oluşturabilirsiniz. Yeni bir kaynak grubu oluşturmak önerilir.
   * Ad-Web uygulamanıza bir ad verin. 
   * Yayımla- **Docker kapsayıcısı** Seç
   * İşletim sistemi- **Linux** seçin
   * Bölge-sizin için anlamlı bir bölge seçin.
   * Linux planı-uygulama hizmetiniz için bir fiyatlandırma katmanı/planı seçin. 

   > [!div class="mx-imgBorder"]
   > ![Web uygulamanızı yapılandırma](./media/quickstarts/formre-select-docker-linux.png)

5. Sonra **Docker** sekmesini seçin. 

   > [!div class="mx-imgBorder"]
   > ![Docker 'ı seçin](./media/quickstarts/formre-select-docker.png)

6. Şimdi Docker kapsayıcınızı yapılandıralim. Aksi belirtilmediği takdirde tüm alanlar gereklidir:

   * Seçenekler- **tek kapsayıcı** seçin
   * Görüntü kaynağı- **özel kayıt defteri** seçme 
   * Sunucu URL 'SI-bunu olarak ayarlayın`https://mcr.microsoft.com`
   * Kullanıcı adı (Isteğe bağlı)-Kullanıcı adı oluşturun. 
   * Parola (Isteğe bağlı)-anımsayabileceğiniz güvenli bir parola oluşturun.
   * Image ve Tag-bunu olarak ayarla`mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool:latest`
   * Sürekli dağıtım-geliştirme ekibi örnek etiketleme aracında değişiklik yaptığında otomatik güncelleştirmeler almak istiyorsanız bunu **Açık** olarak ayarlayın.
   * Başlangıç komutu-bunu olarak ayarlayın`./run.sh eula=accept`

   > [!div class="mx-imgBorder"]
   > ![Docker 'ı yapılandırma](./media/quickstarts/formre-configure-docker.png)

7. İşte bu kadar. Ardından Web uygulamanızı dağıtmak için **gözden geçir + oluştur**' u ve ardından **Oluştur** ' u seçin. Tamamlandığında, Web uygulamanıza, kaynağınız için **genel bakış** bölümünde belirtilen URL 'de erişebilirsiniz.

> [!NOTE]
> Web uygulamanızı oluştururken yetkilendirmeyi/kimlik doğrulamasını da yapılandırabilirsiniz. Başlamak için bu gerekli değildir. 

### <a name="azure-cli"></a>Azure CLI

Azure portal kullanmaya alternatif olarak, Azure CLı kullanarak bir kaynak oluşturabilirsiniz. Devam etmeden önce [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)'yı yüklemeniz gerekir. Azure CLı ile zaten çalışıyorsanız bu adımı atlayabilirsiniz. 

Bu komutla ilgili bilmeniz gereken birkaç nokta vardır:

* `DNS_NAME_LABEL=aci-demo-$RANDOM`Rastgele bir DNS adı üretir. 
* Bu örnek, kaynak oluşturmak için kullanabileceğiniz bir kaynak grubunuz olduğunu varsayar. Aboneliğinizle `<resource_group_name>` ilişkili geçerli bir kaynak grubuyla değiştirin. 
* Kaynağı oluşturmak istediğiniz yeri belirtmeniz gerekir. Web `<region name>` uygulaması için istediğiniz bölge ile değiştirin. 
* Bu komut EULA 'Yı otomatik olarak kabul eder.

Azure CLı 'da, örnek etiketleme aracı için bir Web uygulaması kaynağı oluşturmak üzere şu komutu çalıştırın: 

```azurecli
DNS_NAME_LABEL=aci-demo-$RANDOM

az container create \
  --resource-group <resource_group_name> \
  --name <name> \
  --image mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool \
  --ports 3000 \
  --dns-name-label $DNS_NAME_LABEL \
  --location <region name> \
  --cpu 2 \
  --memory 8 \
  --command-line "./run.sh eula=accept"
```

### <a name="connect-to-azure-ad-for-authorization"></a>Yetkilendirme için Azure AD 'ye bağlanma

Web uygulamanızı Azure Active Directory bağlamanız önerilir. Bu, yalnızca geçerli kimlik bilgilerine sahip kullanıcıların oturum açabilmesini ve Web uygulamanızı kullanmasını sağlar. Azure Active Directory bağlanmak için [App Service uygulamanızı yapılandırma](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-aad) bölümündeki yönergeleri izleyin.

## <a name="open-source-on-github"></a>GitHub 'da açık kaynak

OCR formu etiketleme Aracı, GitHub 'da açık kaynaklı bir proje olarak da kullanılabilir. Araç, yanıt verme + Redux kullanılarak oluşturulan ve TypeScript 'te yazılmış bir Web uygulamasıdır. Daha fazla bilgi edinmek veya katkıda bulunmak için bkz. [OCR formu etiketleme aracı](https://github.com/microsoft/OCR-Form-Tools/blob/master/README.md).

## <a name="next-steps"></a>Sonraki adımlar

Eğitim verilerini el ile etiketlemek ve denetimli öğrenimi gerçekleştirmek üzere aracı nasıl kullanacağınızı öğrenmek için, [etiketlerle eğit](./quickstarts/label-tool.md) hızlı ' yı kullanın.
