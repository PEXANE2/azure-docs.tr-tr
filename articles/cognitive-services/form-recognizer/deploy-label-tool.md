---
title: Form Recognizer örnek etiketleme aracı nasıl dağıtılır?
titleSuffix: Azure Cognitive Services
description: Denetimli öğrenime yardımcı olmak için Form Tanıyıcısı örnek etiketleme aracını dağıtmanın farklı yollarını öğrenin.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: how-to
ms.date: 03/20/2020
ms.author: pafarley
ms.openlocfilehash: 795d21e05ade652b52c06d597ca4c5fef85e7245
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80152881"
---
# <a name="deploy-the-sample-labeling-tool"></a>Örnek etiketleme aracını dağıtma

Form Recognizer örnek etiketleme aracı, denetimli öğrenme amacıyla formları (belgeleri) el ile etiketlemek için kullanabileceğiniz basit bir kullanıcı arabirimi (UI) sağlayan bir uygulamadır. Bu makalede, size nasıl yapılacağını öğreten bağlantılar ve talimatlar sağlayacağız:

* [Örnek etiketleme aracını yerel olarak çalıştırma](#run-the-sample-labeling-tool-locally)
* [Örnek etiketleme aracını Azure Kapsayıcı Örneği'ne (ACI) dağıtma](#deploy-with-azure-container-instances-aci)
* [Açık kaynak ocr form etiketleme aracını kullanın ve katkıda bulunun](#open-source-on-github)

## <a name="run-the-sample-labeling-tool-locally"></a>Örnek etiketleme aracını yerel olarak çalıştırma

Verileri etiketlemeye başlamanın en hızlı yolu, örnek etiketleme aracını yerel olarak çalıştırmaktır. Aşağıdaki quickstart, el ile etiketlenmiş verilerle özel bir model eğitmek için Form Recognizer REST API'yi ve örnek etiketleme aracını kullanır. 

* [Quickstart: Formları etiketle, bir modeli eğitin ve örnek etiketleme aracını kullanarak formu analiz edin.](./quickstarts/label-tool.md)

## <a name="deploy-with-azure-container-instances-aci"></a>Azure Kapsayıcı Örnekleri (ACI) ile dağıtma

Başlamadan önce, örnek etiketleme aracını bir Azure Kapsayıcı Örneği'ne (ACI) dağıtmanın iki yolu olduğunu unutmayın. Örnek etiketleme aracını ACI ile çalıştırmak için her iki seçenek de kullanılır: 

* [Azure portalını kullanma](#azure-portal)
* [Azure CLI kullanma](#azure-cli)

### <a name="azure-portal"></a>Azure portalında

Azure portalını kullanarak yeni bir kaynak oluşturmak için aşağıdaki adımları izleyin: 

1. [Azure portalında](https://portal.azure.com/signin/index/)oturum açın.
2. **Kaynak oluştur**’u seçin. 
3. Ardından, **Web Uygulaması'nı**seçin. 

   > [!div class="mx-imgBorder"]
   > ![Web uygulamasını seçin](./media/quickstarts/formre-create-web-app.png)
   
4. İlk olarak, **Temeller** sekmesinin seçildiğinden emin olun. Şimdi, bazı bilgiler vermeniz gerekecek: 

   > [!div class="mx-imgBorder"]
   > ![Temel Bilgileri Seçin](./media/quickstarts/formre-select-basics.png)
   * Abonelik - Varolan bir Azure aboneliğini seçin
   * Kaynak Grubu - Varolan bir kaynak grubunu yeniden kullanabilir veya bu proje için yeni bir kaynak oluşturabilirsiniz. Yeni bir kaynak grubu oluşturulması önerilir.
   * Ad - Web uygulamanıza bir ad verin. 
   * Yayımla - **Docker Konteyner'i** Seçin
   * İşletim Sistemi - **Linux'u** Seçin
   * Bölge - Sizin için mantıklı bir bölge seçin.
   * Linux Planı - Uygulama hizmetiniz için bir fiyatlandırma katmanı/planı seçin. 

   > [!div class="mx-imgBorder"]
   > ![Web uygulamanızı yapılandırın](./media/quickstarts/formre-select-docker-linux.png)

5. Ardından **Docker** sekmesini seçin. 

   > [!div class="mx-imgBorder"]
   > ![Docker'ı seçin](./media/quickstarts/formre-select-docker.png)

6. Şimdi Docker kabınızı yapılandıralım. Aksi belirtilmedikçe tüm alanlar gereklidir:

   * Seçenekler - **Tek Kapsayıcı** seçin
   * Resim Kaynağı - **Özel Kayıt Defteri'ni** Seçin 
   * Sunucu URL 'i - Bunu`https://mcr.microsoft.com`
   * Kullanıcı adı (İsteğe Bağlı) - Bir kullanıcı adı oluşturun. 
   * Parola (İsteğe Bağlı) - Hatırlayacağınız güvenli bir parola oluşturun.
   * Resim ve etiket - Bunu`mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool:latest`
   * Başlangıç komutu - Bunu`./run.sh eula=accept`

   > [!div class="mx-imgBorder"]
   > ![Docker'ı yapılandır](./media/quickstarts/formre-configure-docker.png)

7. İşte bu kadar. Ardından, **Gözden Geçir + Oluştur'u**seçin, ardından web uygulamanızı dağıtmak için **Oluşturun'ü** oluşturun. Tamamlandığında, kaynağınız için **Genel Bakış'ta** sağlanan URL'den web uygulamanıza erişebilirsiniz.

> [!NOTE]
> Web uygulamanızı oluştururken yetkilendirme/kimlik doğrulaması da yapılandırabilirsiniz. Bu başlamak için gerekli değildir. 

### <a name="azure-cli"></a>Azure CLI

Azure portalını kullanmaya alternatif olarak, Azure CLI'yi kullanarak bir kaynak oluşturabilirsiniz. Devam etmeden önce [Azure CLI'yi](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)yüklemeniz gerekir. Azure CLI ile zaten çalışıyorsanız bu adımı atlayabilirsiniz. 

Bu komut hakkında bilmeniz gereken birkaç şey var:

* `DNS_NAME_LABEL=aci-demo-$RANDOM`rasgele bir DNS adı oluşturur. 
* Bu örnek, kaynak oluşturmak için kullanabileceğiniz bir kaynak grubunuz olduğunu varsayar. Aboneliğinizle ilişkili geçerli bir kaynak grubuyla değiştirin. `<resource_group_name>` 
* Kaynağı nerede oluşturmak istediğinizi belirtmeniz gerekir. Web `<region name>` uygulaması için istediğiniz bölge ile değiştirin. 
* Bu komut otomatik olarak EULA'yı kabul eder.

Azure CLI'den, örnek etiketleme aracı için bir web uygulaması kaynağı oluşturmak için bu komutu çalıştırın: 

```azurecli
DNS_NAME_LABEL=aci-demo-$RANDOM

az container create \
  --resource-group <resorunce_group_name> \
  --name <name> \
  --image mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool \
  --ports 3000 \
  --dns-name-label $DNS_NAME_LABEL \
  --location <region name> \
  --cpu 2 \
  --memory 8
  --command-line "./run.sh eula=accept"
```

### <a name="connect-to-azure-ad-for-authorization"></a>Yetkilendirme için Azure AD'ye bağlanın

Web uygulamanızı Azure Active Directory'ye bağlamanız önerilir. Bu, yalnızca geçerli kimlik bilgilerine sahip kullanıcıların oturum açabilmesini ve web uygulamanızı kullanmasını sağlar. Azure Active Directory'ye bağlanmak için [Uygulama Hizmeti uygulamanızı Yapılanyapılandır'daki](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-aad) yönergeleri izleyin.

## <a name="open-source-on-github"></a>GitHub'da açık kaynak

OCR Form Etiketleme Aracı, GitHub'da açık kaynak projesi olarak da kullanılabilir. Araç React + Redux kullanılarak oluşturulmuş bir web uygulamasıdır ve TypeScript ile yazılmıştır. Daha fazla bilgi edinmek veya katkıda bulunmak için [OCR Form Etiketleme Aracı'na](https://github.com/microsoft/OCR-Form-Tools/blob/master/README.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Eğitim verilerini el ile etiketlemek ve denetimli öğrenme gerçekleştirmek için aracı nasıl kullanacağınızı öğrenmek için hızlı bir şekilde başlatılabilen [etiketlerle Tren'i](./quickstarts/label-tool.md) kullanın.
