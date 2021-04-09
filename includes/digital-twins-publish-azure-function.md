---
author: baanders
description: Visual Studio 'dan bir Azure işlevi yayımlama işlemi için dosya ekleme
ms.service: digital-twins
ms.topic: include
ms.date: 1/21/2021
ms.author: baanders
ms.openlocfilehash: 9f8ca9ab6dc184abb8d9dcd25ebe87cbd7761722
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "102630276"
---
Projeyi Azure 'da bir işlev uygulamasına yayımlamak için *Çözüm Gezgini* ' de projeyi sağ seçin ve **Yayımla**' yı seçin.

> [!IMPORTANT] 
> Azure 'da bir işlev uygulamasına yayımlama, Azure dijital TWINS 'ten bağımsız olarak aboneliğiniz üzerinde ek ücretler doğurur.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-1.png" alt-text="Sağ seçme çözüm menüsünü gösteren Visual Studio ekran görüntüsü. Yayımla menüsünde vurgulanır.":::

Aşağıdaki *Yayımla* sayfasında, **Azure** varsayılan hedef seçimini bırakın ve *İleri*' yi seçin. 

Belirli bir hedef için **Azure işlev uygulaması (Windows)** öğesini seçin ve *İleri*' yi seçin.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-2.png" alt-text="Azure işlevi Yayımla iletişim kutusunda Visual Studio ekran görüntüsü. Azure İşlev Uygulaması (Windows) belirli hedef sayfada seçilir.":::

*İşlevler örneği* sayfasında, aboneliğinizi seçin. Ardından *+* Yeni bir Azure işlevi oluşturmak için simgeyi seçin.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-3.png" alt-text="Azure işlevi Yayımla iletişim kutusunda Visual Studio ekran görüntüsü. Yeni bir işlev oluşturmak için + düğmesi Işlevler örneği sayfasında vurgulanır.":::

*İşlev uygulaması (Windows)-yeni oluştur* penceresinde, alanları aşağıdaki gibi girin:
* **Ad** , Azure 'un Azure işlevleri uygulamanızı barındırmak için kullanacağı tüketim planının adıdır. Bu aynı zamanda gerçek işlevinizi tutan işlev uygulamasının adı olur. Kendi benzersiz bir değer seçebilir veya varsayılan öneriyi bırakabilirsiniz.
* **Aboneliğin** kullanmak istediğiniz abonelikle eşleştiğinden emin olun 
* **Kaynak** grubunun kullanmak istediğiniz kaynak grubuna emin olun
* **Plan türünü** *Tüketim* olarak bırakın
* Kaynak grubunuzun konumuyla eşleşen **konumu** seçin
* *Yeni...* bağlantısını kullanarak yeni bir **Azure depolama** kaynağı oluşturun. Konumu kaynak grubunuza uyacak şekilde ayarlayın, diğer varsayılan değerleri kullanın ve "Tamam" ı seçin.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-4.png" alt-text="Azure işlevi Yayımla iletişim kutusunda Visual Studio ekran görüntüsü. Yeni bir işlev uygulamasının ayrıntıları, ad, abonelik, kaynak grubu, plan türü, konum ve Azure depolama dahil olmak üzere doldurulmuştur.":::

Ardından **Oluştur**’u seçin.

Uygulama hizmeti oluşturulurken kısa bir bekleme sonrasında iletişim kutusu, yeni işlev uygulamanız kaynak grubunuzun altına yerleştirilmiş **Işlev uygulamaları** alanında görünirken *işlevler örneği* sayfasına döndürmelidir. *Son*'u seçin.

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

İşlev uygulamanız artık Azure 'da yayımlandı.