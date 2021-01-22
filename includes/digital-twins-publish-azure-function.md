---
author: baanders
description: Visual Studio 'dan bir Azure işlevi yayımlama işlemi için dosya ekleme
ms.service: digital-twins
ms.topic: include
ms.date: 1/21/2021
ms.author: baanders
ms.openlocfilehash: 069a29f49172cf3bad6b7f7b6aca28f32f5d63b3
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98683913"
---
Projeyi Azure 'da bir işlev uygulamasına yayımlamak için *Çözüm Gezgini* ' de projeyi sağ seçin ve **Yayımla**' yı seçin.

> [!IMPORTANT] 
> Azure 'da bir işlev uygulamasına yayımlama, Azure dijital TWINS 'ten bağımsız olarak aboneliğiniz üzerinde ek ücretler doğurur.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-1.png" alt-text="Visual Studio: projeyi Yayımla":::

Aşağıdaki *Yayımla* sayfasında, **Azure** varsayılan hedef seçimini bırakın ve *İleri*' yi ziyaret edin. 

Belirli bir hedef için **Azure işlev uygulaması (Windows)** öğesini seçin ve *İleri*' yi tıklayın.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-2.png" alt-text="Visual Studio 'da Azure işlevini yayımlama: belirli hedef":::

*İşlevler örneği* sayfasında, aboneliğinizi seçin. Bu, aboneliğinizdeki *kaynak gruplarıyla* bir kutu doldurmalıdır.

Örneğinizin kaynak grubunu seçin ve *+* Yeni bir Azure işlevi oluşturmak için isabet edin.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-3.png" alt-text="Visual Studio 'da Azure işlevini yayımlama: Işlevler örneği (işlev uygulamasından önce)":::

*İşlev uygulaması (Windows)-yeni oluştur* penceresinde, alanları aşağıdaki gibi girin:
* **Ad** , Azure 'un Azure işlevleri uygulamanızı barındırmak için kullanacağı tüketim planının adıdır. Bu aynı zamanda gerçek işlevinizi tutan işlev uygulamasının adı olur. Kendi benzersiz bir değer seçebilir veya varsayılan öneriyi bırakabilirsiniz.
* **Aboneliğin** kullanmak istediğiniz abonelikle eşleştiğinden emin olun 
* **Kaynak** grubunun kullanmak istediğiniz kaynak grubuna emin olun
* **Plan türünü** *Tüketim* olarak bırakın
* Kaynak grubunuzun konumuyla eşleşen **konumu** seçin
* *Yeni...* bağlantısını kullanarak yeni bir **Azure depolama** kaynağı oluşturun. Konumu kaynak grubunuza uyacak şekilde ayarlayın, diğer varsayılan değerleri kullanın ve "Tamam" düğmesine basın.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-4.png" alt-text="Visual Studio 'da Azure işlevini yayımlama: İşlev Uygulaması (Windows)-yeni oluştur":::

Ardından **Oluştur**’u seçin.

Bu, yeni işlev uygulamanızın artık kaynak grubunuzun altında göründüğünden, bunu *işlevler örneği* sayfasına geri getirmelidir. İsabet *sonu*.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-5.png" alt-text="Visual Studio 'da Azure işlevini yayımlama: Işlevler örneği (işlev uygulamasından sonra)":::

Ana Visual Studio penceresinde geri açılan *Yayımla* bölmesinde tüm bilgilerin doğru göründüğünden emin olun ve **Yayımla**' yı seçin.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-6.png" alt-text="Visual Studio 'da Azure işlevini yayımlama: yayımlama":::

> [!NOTE]
> Şu şekilde bir açılan pencere görürseniz: :::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-7.png" alt-text="Visual Studio 'Da Azure Işlevini yayımlama: kimlik bilgilerini yayımlama" border="false":::
> **Azure 'dan kimlik bilgilerini alma** ve **kaydetme** girişimini seçin.
>
> *Azure 'Da işlev sürümünü yükseltmek* için bir uyarı görürseniz veya *işlevler çalışma zamanının sürümünün Azure 'da çalışan sürümle eşleşmemesi* durumunda:
>
> En son Azure Işlevleri çalışma zamanı sürümüne yükseltmek için istemleri izleyin. Visual Studio 'nun eski bir sürümünü kullanıyorsanız bu sorun oluşabilir.

İşlev uygulamanızın Azure dijital TWINS 'e erişebilmesi için sistem tarafından yönetilen bir kimliğe sahip olması ve Azure dijital TWINS örneğinize erişmek için gereken izinlere sahip olması gerekir. Bunu daha sonra ayarlayacaksınız.
