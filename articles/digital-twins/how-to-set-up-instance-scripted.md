---
title: Örnek ayarlama ve kimlik doğrulaması (betik ile)
titleSuffix: Azure Digital Twins
description: Doğru kimlik doğrulaması da dahil olmak üzere Azure dijital TWINS hizmeti örneğini ayarlama bölümüne bakın. Betikleştirilmiş sürüm.
author: baanders
ms.author: baanders
ms.date: 7/22/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 522096b921faf34130f0c37f727d89c7bf95c530
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87337917"
---
# <a name="set-up-an-azure-digital-twins-instance-and-authentication-scripted"></a>Azure dijital TWINS örneği ve kimlik doğrulaması (komut dosyası) ayarlama

[!INCLUDE [digital-twins-setup-selector.md](../../includes/digital-twins-setup-selector.md)]

Bu makalede, örneği oluşturma ve kimlik doğrulamasını ayarlama dahil olmak üzere **Yeni bir Azure dijital TWINS örneği ayarlama**adımları ele alınmaktadır. Bu makaleyi tamamladıktan sonra, programlamaya başlamak için kullanabileceğiniz bir Azure dijital TWINS örneğine sahip olursunuz.

Bu makalenin bu sürümü, işlemi kolaylaştıran [ **otomatikleştirilmiş bir dağıtım betiği** örneği](https://docs.microsoft.com/samples/azure-samples/digital-twins-samples/digital-twins-samples/) çalıştırarak bu adımları tamamlar. Komut dosyasının arka planda çalıştığı adımları el ile görüntülemek için, bu makalenin el ile sürümüne bakın: [*nasıl yapılır: örnek ve kimlik doğrulaması ayarlama (el ile)*](how-to-set-up-instance-manual.md).

[!INCLUDE [digital-twins-setup-starter.md](../../includes/digital-twins-setup-starter.md)]

## <a name="run-the-deployment-script"></a>Dağıtım betiğini çalıştırma

Bu makalede bir Azure dijital TWINS örneği ve gerekli kimlik doğrulama yarı otomatik olarak dağıtmak için bir Azure dijital TWINS kod örneği kullanılmaktadır. Ayrıca, kendi komut dosyası etkileşimlerinizi yazmak için bir başlangıç noktası olarak da kullanılabilir.

Örnek betik, PowerShell 'de yazılmıştır. Bu örnek bağlantıyı inceleyerek makinenize indirebileceğiniz [Azure dijital TWINS örneklerinin](https://docs.microsoft.com/samples/azure-samples/digital-twins-samples/digital-twins-samples/)bir parçasıdır. Bu, söz konusu örnek bağlantısına giderek ve başlık altındaki *posta indirme* düğmesini seçmiş olursunuz.

İndirilen örnek klasörde, dağıtım betiği _Azure_Digital_Twins_samples.zip > betikler > **deploy.ps1** _bulunur.

Dağıtım betiğini Cloud Shell ' de çalıştırma adımları aşağıda verilmiştir.
1. Tarayıcınızda bir [Azure Cloud Shell](https://shell.azure.com/) penceresine gidin. Şu komutu kullanarak oturum açın:
    ```azurecli-interactive
    az login
    ```
    CLı varsayılan tarayıcınızı açabildiğinden, bunu yapılır ve bir Azure oturum açma sayfası yükler. Aksi takdirde, konumunda bir tarayıcı sayfası açın *https://aka.ms/devicelogin* ve terminalinizde görünen yetkilendirme kodunu girin.
 
2. Oturum açtıktan sonra, Cloud Shell pencere simgesi çubuğuna bakın. "Dosyaları karşıya yükle/Indir" simgesini seçin ve "karşıya yükle" yi seçin.

    :::image type="content" source="media/how-to-set-up-instance/cloud-shell-upload.png" alt-text="Karşıya yükleme seçeneğinin seçimini gösteren Cloud Shell pencere":::

    Makinenizde _**deploy.ps1**_ dosyasına gidin ve "Aç" düğmesine basın. Bu işlem, dosyayı Cloud Shell penceresinde çalıştırabilmeniz için Cloud Shell dosyasına yükler.

3. Komutu Cloud Shell penceresine göndererek betiği çalıştırın `./deploy.ps1` . Komut dosyası otomatik kurulum adımları üzerinden çalışırken, aşağıdaki değerleri geçirmeniz istenir:
    * Örnek için: kullanılacak Azure aboneliğinizin *ABONELIK kimliği*
    * Örnek için: örneği dağıtmak istediğiniz *konum* . Azure dijital TWINS 'i destekleyen bölgeleri görmek için [*bölgeye göre kullanılabilen Azure ürünlerini*](https://azure.microsoft.com/global-infrastructure/services/?products=digital-twins)ziyaret edin.
    * Örnek için: bir *kaynak grubu* adı. Var olan bir kaynak grubunu kullanabilir veya oluşturmak için yeni bir ad girebilirsiniz.
    * Örnek için: Azure dijital TWINS örneğiniz için bir *ad* . Yeni örneğin adı, aboneliğinizin bölgesi içinde benzersiz olmalıdır (yani, aboneliğiniz seçtiğiniz adı kullanan bölgede başka bir Azure dijital TWINS örneğine sahipse, farklı bir ad seçmeniz istenir).
    * Uygulama kaydı için: kayıt ile ilişkilendirilecek bir *Azure AD uygulama görünen adı* . Bu uygulama kaydı, [Azure dijital TWINS API 'leri](how-to-use-apis-sdks.md)için erişim izinlerini yapılandırdığınız yerdir. Daha sonra, istemci uygulaması uygulama kaydında kimlik doğrulaması yapacak ve bu nedenle API 'lere yapılandırılmış erişim izinleri verilmeyecektir.
    * Uygulama kaydı için: Azure AD uygulaması için bir *Azure AD uygulama yanıt URL 'si* . Kullanabilirsiniz `http://localhost` .

Betik bir Azure dijital TWINS örneği oluşturur, Azure kullanıcısını örneğe *Azure Digital TWINS Owner (Önizleme)* rolünü atar ve istemci uygulamanızın kullanması Için BIR Azure AD uygulama kaydı ayarlayın.

Betikten çıktı günlüğü alıntısı aşağıda verilmiştir:

:::image type="content" source="media/how-to-set-up-instance/deployment-script-output.png" alt-text="Dağıtım betiği çalıştırıldığında giriş ve çıkış günlüğünü gösteren Cloud Shell pencere" lightbox="media/how-to-set-up-instance/deployment-script-output.png":::

Betik başarılı bir şekilde tamamlanırsa, nihai çıktıyı görürsünüz `Deployment completed successfully` . Aksi takdirde, hata iletisini adreslayın ve betiği yeniden çalıştırın. Bu işlem, önceden tamamladığınız adımları atlar ve kaldığınız yerden yeniden giriş istemeye başlar.

Komut dosyası tamamlandıktan sonra, artık kullanıma hazır bir Azure dijital TWINS örneğiniz ve izinleri yönetmek için ayarlanmıştır.

## <a name="collect-important-values"></a>Önemli değerleri topla

Uygulama kaydından, daha sonra [Azure dijital TWINS API 'lerinde istemci uygulamasının kimliğini doğrulamak](how-to-authenticate-client.md)için gerekli olan iki önemli değer vardır. 

Bunları bulmak için Azure portal Azure AD uygulama kaydına genel bakış sayfasına gitmek üzere [Bu bağlantıyı](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) izleyin. Bu sayfa, aboneliğinizde oluşturulmuş tüm uygulama kayıtlarını gösterir.

Bu listede yeni oluşturduğunuz uygulama kaydını görmeniz gerekir. Ayrıntılarını açmak için seçin:

:::image type="content" source="media/how-to-set-up-instance/app-important-values.png" alt-text="Uygulama kaydı için önemli değerlerin Portal görünümü":::

**Sayfanızda gösterilen** *uygulama (istemci) kimliğini* ve *Dizin (kiracı) kimliğini* bir yere göz atın. İstemci uygulamaları için kod yazmak üzere bir kişi değilseniz, bu değerleri olacak kişiyle paylaşmanız gerekir.

## <a name="verify-success"></a>Başarıyı doğrula

Komut dosyası tarafından ayarlanan kaynaklarınızın ve izinlerin oluşturulmasını doğrulamak istiyorsanız, [Azure Portal](https://portal.azure.com)bunlara bakabilirsiniz.

### <a name="verify-instance"></a>Örneği doğrula

Örneğinizin oluşturulduğunu doğrulamak için Azure portal [Azure dijital TWINS sayfasına](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.DigitalTwins%2FdigitalTwinsInstances) gidin. Bu sayfada tüm Azure dijital TWINS örnekleri listelenir. Listede yeni oluşturduğunuz örneğin adını arayın.

### <a name="verify-user-role-assignment"></a>Kullanıcı rolü atamasını doğrula

[!INCLUDE [digital-twins-setup-verify-role-assignment.md](../../includes/digital-twins-setup-verify-role-assignment.md)]

### <a name="verify-app-registration"></a>Uygulama kaydını doğrulama

[!INCLUDE [digital-twins-setup-verify-app-registration-1.md](../../includes/digital-twins-setup-verify-app-registration-1.md)]

İlk olarak, Azure dijital TWINS izinleri ayarlarının kayıt üzerinde doğru şekilde ayarlandığını doğrulayın. Bunu yapmak için, menü çubuğundan *bildirim* ' ı seçerek uygulama kaydının bildirim kodunu görüntüleyin. Kod penceresinin en altına kaydırın ve altındaki bu alanları bulun `requiredResourceAccess` . Değerler aşağıdaki ekran görüntüsünde olanlarla eşleşmelidir:

[!INCLUDE [digital-twins-setup-verify-app-registration-2.md](../../includes/digital-twins-setup-verify-app-registration-2.md)]

## <a name="other-possible-steps-for-your-organization"></a>Kuruluşunuz için olası diğer adımlar

[!INCLUDE [digital-twins-setup-additional-requirements.md](../../includes/digital-twins-setup-additional-requirements.md)]

## <a name="next-steps"></a>Sonraki adımlar

İstemci uygulamasının kimlik doğrulama kodunu yazarak istemci uygulamanızı örneğinize bağlama konusuna bakın:
* [*Nasıl yapılır: uygulama kimlik doğrulama kodunu yazma*](how-to-authenticate-client.md)
