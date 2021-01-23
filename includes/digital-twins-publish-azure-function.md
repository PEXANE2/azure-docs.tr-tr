---
author: baanders
description: Visual Studio 'dan bir Azure işlevi yayımlama işlemi için dosya ekleme
ms.service: digital-twins
ms.topic: include
ms.date: 1/21/2021
ms.author: baanders
ms.openlocfilehash: 63b393f519ad29baa05fef046ee1e8ba9e5330d8
ms.sourcegitcommit: 75041f1bce98b1d20cd93945a7b3bd875e6999d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98701175"
---
Projeyi Azure 'da bir işlev uygulamasına yayımlamak için *Çözüm Gezgini* ' de projeyi sağ seçin ve **Yayımla**' yı seçin.

> [!IMPORTANT] 
> Azure 'da bir işlev uygulamasına yayımlama, Azure dijital TWINS 'ten bağımsız olarak aboneliğiniz üzerinde ek ücretler doğurur.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-1.png" alt-text="Sağ seçme çözüm menüsünü gösteren Visual Studio ekran görüntüsü. Yayımla menüsünde vurgulanır.":::

Aşağıdaki *Yayımla* sayfasında, **Azure** varsayılan hedef seçimini bırakın ve *İleri*' yi ziyaret edin. 

Belirli bir hedef için **Azure işlev uygulaması (Windows)** öğesini seçin ve *İleri*' yi tıklayın.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-2.png" alt-text="Azure işlevi Yayımla iletişim kutusunda Visual Studio ekran görüntüsü. Azure İşlev Uygulaması (Windows) belirli hedef sayfada seçilir.":::

*İşlevler örneği* sayfasında, aboneliğinizi seçin. Bu, aboneliğinizdeki *kaynak gruplarıyla* bir kutu doldurmalıdır.

Örneğinizin kaynak grubunu seçin ve *+* Yeni bir Azure işlevi oluşturmak için isabet edin.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-3.png" alt-text="Azure işlevi Yayımla iletişim kutusunda Visual Studio ekran görüntüsü. Yeni bir işlev oluşturmak için + düğmesi Işlevler örneği sayfasında vurgulanır.":::

*İşlev uygulaması (Windows)-yeni oluştur* penceresinde, alanları aşağıdaki gibi girin:
* **Ad** , Azure 'un Azure işlevleri uygulamanızı barındırmak için kullanacağı tüketim planının adıdır. Bu aynı zamanda gerçek işlevinizi tutan işlev uygulamasının adı olur. Kendi benzersiz bir değer seçebilir veya varsayılan öneriyi bırakabilirsiniz.
* **Aboneliğin** kullanmak istediğiniz abonelikle eşleştiğinden emin olun 
* **Kaynak** grubunun kullanmak istediğiniz kaynak grubuna emin olun
* **Plan türünü** *Tüketim* olarak bırakın
* Kaynak grubunuzun konumuyla eşleşen **konumu** seçin
* *Yeni...* bağlantısını kullanarak yeni bir **Azure depolama** kaynağı oluşturun. Konumu kaynak grubunuza uyacak şekilde ayarlayın, diğer varsayılan değerleri kullanın ve "Tamam" düğmesine basın.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-4.png" alt-text="Azure işlevi Yayımla iletişim kutusunda Visual Studio ekran görüntüsü. Yeni bir işlev uygulamasının ayrıntıları, ad, abonelik, kaynak grubu, plan türü, konum ve Azure depolama dahil olmak üzere doldurulmuştur.":::

Ardından **Oluştur**’u seçin.

Bu, yeni işlev uygulamanızın artık kaynak grubunuzun altında göründüğünden, bunu *işlevler örneği* sayfasına geri getirmelidir. İsabet *sonu*.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-5.png" alt-text="Visual Studio 'da Azure işlevini yayımlama: Işlevler örneği (işlev uygulamasından sonra)":::

Ana Visual Studio penceresinde geri açılan *Yayımla* bölmesinde tüm bilgilerin doğru göründüğünden emin olun ve **Yayımla**' yı seçin.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-6.png" alt-text="Azure işlevi Yayımla iletişim kutusunda Visual Studio ekran görüntüsü. Yeni işlev uygulaması, işlev uygulamaları listesinde görüntülenir ve bir son düğmesi vardır.":::

> [!NOTE]
> Şöyle bir açılan pencere görürseniz: :::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-7.png" alt-text="Yayımlama kimlik bilgileri adlı Visual Studio açılır penceresinin ekran görüntüsü. Bu, bir Kullanıcı adı ve parola için alanlar ve Azure 'dan kimlik bilgilerini almaya çalışacak bir düğme içerir." border="false":::
> **Azure 'dan kimlik bilgilerini alma** ve **kaydetme** girişimini seçin.
>
> *Azure 'Da işlev sürümünü yükseltmek* için bir uyarı görürseniz veya *işlevler çalışma zamanının sürümünün Azure 'da çalışan sürümle eşleşmemesi* durumunda:
>
> En son Azure Işlevleri çalışma zamanı sürümüne yükseltmek için istemleri izleyin. Visual Studio 'nun eski bir sürümünü kullanıyorsanız bu sorun oluşabilir.

İşlev uygulamanızın Azure dijital TWINS 'e erişebilmesi için sistem tarafından yönetilen bir kimliğe sahip olması ve Azure dijital TWINS örneğinize erişmek için gereken izinlere sahip olması gerekir. Bunu daha sonra ayarlayacaksınız.
