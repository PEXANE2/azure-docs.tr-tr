---
title: 'Hızlı başlangıç: Azure portal Azure purview hesabı oluşturma (Önizleme)'
description: Bu hızlı başlangıçta, Azure purview hesabı oluşturma ve bu uygulamayı kullanmaya başlamak için izinleri yapılandırma açıklanmaktadır.
author: hophan
ms.author: hophan
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: quickstart
ms.date: 10/23/2020
ms.openlocfilehash: 00b504c7bcf51a69d03fb1294de4f52ef35ed163
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96555995"
---
# <a name="quickstart-create-an-azure-purview-account-in-the-azure-portal"></a>Hızlı başlangıç: Azure portal bir Azure purview hesabı oluşturun

> [!IMPORTANT]
> Azure takip görünümü şu anda ÖNIZLEME aşamasındadır. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) , Beta, önizleme veya başka bir şekilde genel kullanıma sunulmayan Azure özelliklerine uygulanan ek yasal koşulları içerir.

Bu hızlı başlangıçta bir Azure purview hesabı oluşturursunuz.

## <a name="prerequisites"></a>Ön koşullar

* Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

* Kendi [Azure Active Directory kiracınız](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure hesabınızla [Azure portalında](https://portal.azure.com) oturum açın.

## <a name="configure-your-subscription"></a>Aboneliğinizi yapılandırma

Gerekirse, aboneliğinizi Azure purview 'ın aboneliğinizde çalışacak şekilde yapılandırmak için aşağıdaki adımları izleyin:

   1. Azure portal, **abonelikleri** arayıp seçin.

   1. Abonelikler listesinden, kullanmak istediğiniz aboneliği seçin. Abonelik için yönetici erişim izni gerekiyor.

      :::image type="content" source="./media/create-catalog-portal/select-subscription.png" alt-text="Azure portal aboneliğin nasıl seçileceğini gösteren ekran görüntüsü.":::

   1. Aboneliğiniz için **kaynak sağlayıcıları**' nı seçin. **Kaynak sağlayıcıları** bölmesinde, üç kaynak sağlayıcısı için arama yapın ve kaydedin: 
       1. **Microsoft. purview**
       1. **Microsoft.Storage**
       1. **Microsoft. EventHub** 
      
      Bunlar kayıtlı değilse **Kaydet**' i seçerek kaydolun.

      :::image type="content" source="./media/create-catalog-portal/register-purview-resource-provider.png" alt-text="Microsoft nokta Azure purview kaynak sağlayıcısının Azure portal nasıl kaydedeceğinizi gösteren ekran görüntüsü.":::

## <a name="create-an-azure-purview-account-instance"></a>Azure purview hesap örneği oluşturma

1. Azure portal ' daki **takip hesapları** sayfasına gidin ve ardından **Ekle** ' yi seçerek yeni bir Azure purview hesabı oluşturun. Alternatif olarak, **takip eden hesaplar** için Market araması ' na gidebilir ve **Oluştur**' u seçebilirsiniz. Tek seferde yalnızca bir Azure purview hesabı ekleyebileceğinizi unutmayın.

   :::image type="content" source="./media/create-catalog-portal/add-purview-instance.png" alt-text="Azure portal Azure purview hesap örneğinin nasıl oluşturulacağını gösteren ekran görüntüsü.":::

1. **Temel bilgiler** sekmesinde şunları yapın:
    1. Bir **kaynak grubu** seçin.
    1. Kataloğunuz için bir **purview hesabı adı** girin. Boşluklara ve simgelere izin verilmez.
    1. Bir  **konum** seçin ve ardından **İleri: yapılandırma**' yı seçin.
1. **Yapılandırma** sekmesinde, istenen **Platform boyutunu** seçin-izin verilen değerler 4 Kapasite birimi (CU) ve 16 cu ' dır. **Sonraki: Etiketler**' i seçin.
1. **Etiketler** sekmesinde, isteğe bağlı olarak bir veya daha fazla etiket ekleyebilirsiniz. Bu Etiketler, Azure purview değil yalnızca Azure portal kullanımı içindir.
1. **& oluştur**' u seçin ve ardından **Oluştur**' u seçin. Oluşturma işleminin tamamlanmasında birkaç dakika sürer. Yeni oluşturulan Azure purview hesabı örneği, **purview hesapları** sayfasındaki listede görüntülenir.
1. Yeni hesap sağlama tamamlandığında **Kaynağa Git**' i seçin.

1. **Sağlamayla hesabını Başlat**' ı seçin.

   :::image type="content" source="./media/use-purview-studio/launch-from-portal.png" alt-text="Azure purview hesabı kataloğunu başlatmak için seçimin ekran görüntüsü.":::

## <a name="add-a-security-principal-to-a-data-plane-role"></a>Bir veri düzlemi rolüne güvenlik sorumlusu ekleme

Siz veya ekibiniz Azure purview kullanmaya başlayabilmeniz için önceden tanımlanmış veri düzlemi rollerinden birine bir veya daha fazla güvenlik sorumlusu eklenmelidir: **purview veri okuyucu**, **purview Data Curator** veya **purview veri kaynağı Yöneticisi**. Azure purview veri kataloğu izinleri hakkında daha fazla bilgi için bkz. [Katalog izinleri](catalog-permissions.md).

Bir Azure purview hesabındaki **purview Data Curator** veri düzlemi rolüne bir güvenlik sorumlusu eklemek için:

1. Azure portal takip edilecek [**hesaplar**](https://aka.ms/purviewportal) sayfasına gidin.

1. Değiştirmek istediğiniz Azure purview hesabını seçin.

1. **Purview hesabı** sayfasında, sekme **ERIŞIM denetimi (IAM)** seçeneğini belirleyin.

1. **+ Ekle** 'ye tıklayın

Ekle ' ye tıkladığınızda, hem işaretlenmiş (devre dışı) hem de işaretlenen iki seçenek görürsünüz. Bu durumda, Azure purview hesabındaki bir veri düzlemi rolüne herkes eklemek için doğru izinlere sahip değilsiniz. Azure purview hesabınızda bir sahip, Kullanıcı erişimi Yöneticisi veya rol atama yetkilisi olan başka bir kişi bulmanız gerekir. **Rol atamaları** sekmesini seçerek doğru kişilere bakabilir ve ardından aşağı kaydırarak sahibi veya Kullanıcı erişimi Yöneticisi ' ni arayabilir ve bu insanlarla iletişim kurarak bu kişilere erişebilirsiniz.

1. **Rol ataması ekle**’yi seçin.

1. , Hizmet sorumlusunun ne için kullanılacağı ile ilgili olarak veri kaynağı yönetici **rolü** veya **takip görünümü veri kaynağı Yöneticisi rolünü** görüntüleme içindeki rol türü için (Lütfen ayrıntılar için [Katalog izinleri](catalog-permissions.md) bölümüne bakın).

1. **Ata erişimi** için varsayılan, **Kullanıcı, Grup veya hizmet sorumlusu**' nı bırakın.

1. **Seçim** için, atamak istediğiniz kullanıcının adını, Azure Active Directory grubu veya hizmet sorumlusunu girin ve ardından sonuçlar bölmesinde adına tıklayın.

1. **Kaydet**'e tıklayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık bu Azure purview hesabına ihtiyacınız yoksa, aşağıdaki adımlarla silin:

1. Azure portal takip edilecek **hesaplar** sayfasına gidin.

2. Bu hızlı başlangıçtaki başlangıcında oluşturduğunuz Azure purview hesabını seçin. **Sil**' i seçin, hesabın adını girin ve **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure purview hesabı oluşturmayı öğrendiniz.

Kullanıcıların Azure purview hesabınıza erişmesine nasıl izin vereceğinizi öğrenmek için sonraki makaleye ilerleyin. 

> [!div class="nextstepaction"]
> [Azure purview hesabınıza kullanıcı ekleme](catalog-permissions.md)
