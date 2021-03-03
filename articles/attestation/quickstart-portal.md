---
title: 'Hızlı başlangıç: Azure portal kullanarak Azure kanıtlama ayarlama'
description: Bu hızlı başlangıçta, Azure portal kullanarak bir kanıtlama sağlayıcısı ayarlamayı ve yapılandırmayı öğreneceksiniz.
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: d1310e3c4b4a56a27219cce613e8f6109d32c8c2
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101729410"
---
# <a name="quickstart-set-up-azure-attestation-by-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak Azure kanıtlama ayarlama

Azure kanıtlama kullanmaya başlamak için bu hızlı başlangıcı izleyin. Azure portal kullanarak bir kanıtlama sağlayıcısını, ilke imzalayan ve bir ilkeyi yönetmeyi öğrenin.

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="attestation-provider"></a>Kanıtlama sağlayıcısı

Bu bölümde, bir kanıtlama sağlayıcısı oluşturacak ve imzasız ilkelerden veya imzalı ilkelerden yapılandıracaksınız. Ayrıca, kanıtlama sağlayıcısını görüntülemeyi ve silmeyi de öğreneceksiniz.

### <a name="create-and-configure-the-provider-with-unsigned-policies"></a>İmzasız ilkelerle sağlayıcı oluşturma ve yapılandırma

1. Azure portal menüsüne veya giriş sayfasına gidin ve **kaynak oluştur**' u seçin.
1. Arama kutusuna **kanıtlama** girin.
1. Sonuçlar listesinde **kanıtlama Microsoft Azure**' yi seçin.
1. **Microsoft Azure kanıtlama** sayfasında **Oluştur**' u seçin.
1. **Kanıtlama sağlayıcısı oluştur** sayfasında, aşağıdaki girişleri sağlayın:

   - **Abonelik**: Bir abonelik seçin.
   - **Kaynak grubu**: var olan bir kaynak grubunu seçin veya **Yeni oluştur** ' u seçin ve bir kaynak grubu adı girin.
   - **Ad**: benzersiz bir ad girin.
   - **Konum**: bir konum seçin.
   - **İlke imzalayan sertifikaları dosyası**: sağlayıcıyı imzasız ilkelerle yapılandırmak için ilke imzalayan sertifikalar dosyasını karşıya yüklemeyin.

1. Gerekli girişleri sağlamadıktan sonra, **gözden geçir + oluştur**' u seçin.
1. Doğrulama sorunları varsa, bunları giderin ve ardından **Oluştur**' u seçin.

### <a name="create-and-configure-the-provider-with-signed-policies"></a>Sağlayıcıyı imzalı ilkelerle oluşturma ve yapılandırma

1. Azure portal menüsüne veya giriş sayfasına gidin ve **kaynak oluştur**' u seçin.
1. Arama kutusuna **kanıtlama** girin.
1. Sonuçlar listesinde **kanıtlama Microsoft Azure**' yi seçin.
1. **Microsoft Azure kanıtlama** sayfasında **Oluştur**' u seçin.
1. **Kanıtlama sağlayıcısı oluştur** sayfasında, aşağıdaki bilgileri sağlayın:

   - **Abonelik**: Bir abonelik seçin.
   - **Kaynak grubu**: var olan bir kaynak grubunu seçin veya **Yeni oluştur** ' u seçin ve bir kaynak grubu adı girin.
   - **Ad**: benzersiz bir ad girin.
   - **Konum**: bir konum seçin.
   - **İlke imzalayan sertifikaları dosyası**: kanıtlama sağlayıcısını imzalı ilkelerle yapılandırmak için ilke imzalayan sertifikalar dosyasını karşıya yükleyin. [İlke imzalayan sertifikalarının örneklerine bakın](./policy-signer-examples.md).

1. Gerekli girişleri sağlamadıktan sonra, **gözden geçir + oluştur**' u seçin.
1. Doğrulama sorunları varsa, bunları giderin ve ardından **Oluştur**' u seçin.

### <a name="view-the-attestation-provider"></a>Kanıtlama sağlayıcısını görüntüleme

1. Azure portal menüsüne veya giriş sayfasına gidin ve **tüm kaynaklar**' ı seçin.
1. Filtre kutusuna kanıtlama sağlayıcısı adını girin ve seçin.

### <a name="delete-the-attestation-provider"></a>Kanıtlama sağlayıcısını silme

Kanıtlama sağlayıcısını silmenin iki yolu vardır. Seçenekleriniz şunlardır:

1. Azure portal menüsüne veya giriş sayfasına gidin ve **tüm kaynaklar**' ı seçin.
1. Filtre kutusuna kanıtlama sağlayıcısı adını girin.
1. Onay kutusunu seçin ve **Sil**' i seçin.
1. **Evet** girin ve **Sil**' i seçin.

İsterseniz şunları yapabilirsiniz:

1. Azure portal menüsüne veya giriş sayfasına gidin ve **tüm kaynaklar**' ı seçin.
1. Filtre kutusuna kanıtlama sağlayıcısı adını girin.
1. Kanıtlama sağlayıcısını seçin ve genel bakış sayfasına gidin.
1. Menü çubuğunda **Sil** ' i seçin ve **Evet**' i seçin.

## <a name="attestation-policy-signers"></a>Kanıtlama ilkesi imzalayanlar

İlke imzalayan sertifikalarını görüntülemek, eklemek ve silmek için bu bölümdeki adımları izleyin.

### <a name="view-the-policy-signer-certificates"></a>İlke imzalayan sertifikalarını görüntüleme

1. Azure portal menüsüne veya giriş sayfasına gidin ve **tüm kaynaklar**' ı seçin.
1. Filtre kutusuna kanıtlama sağlayıcısı adını girin.
1. Kanıtlama sağlayıcısını seçin ve genel bakış sayfasına gidin.
1. Pencerenin sol tarafındaki kaynak menüsünde veya alt bölmede **ilke imzalayan sertifikalar** ' ı seçin. Kimlik doğrulaması için sertifika seçmek üzere bir istem görürsünüz. Lütfen devam etmek için uygun seçeneği belirleyin.
1. **İlke imzalayan sertifikalarını indir**' i seçin. Düğme, ilke imzalama gereksinimi olmadan oluşturulan kanıtlama sağlayıcıları için devre dışı bırakılır.
1. İndirilen metin dosyası bir JWS biçimindeki tüm sertifikalara sahip olacaktır.
1. Sertifika sayısını ve indirilen sertifikaları doğrulayın.

### <a name="add-the-policy-signer-certificate"></a>İlke imzalayan sertifikasını ekleme

1.  Azure portal menüsüne veya giriş sayfasına gidin ve **tüm kaynaklar**' ı seçin.
1.  Filtre kutusuna kanıtlama sağlayıcısı adını girin.
1.  Kanıtlama sağlayıcısını seçin ve genel bakış sayfasına gidin.
1.  Pencerenin sol tarafındaki kaynak menüsünde veya alt bölmede **ilke imzalayan sertifikalar** ' ı seçin.
1.  Üstteki menüden **Ekle** ' yi seçin. Düğme, ilke imzalama gereksinimi olmadan oluşturulan kanıtlama sağlayıcıları için devre dışı bırakılır.
1.  İlke imzalayan sertifika dosyasını karşıya yükleyin ve **Ekle**' yi seçin. [İlke imzalayan sertifikalarının örneklerine bakın](./policy-signer-examples.md).

### <a name="delete-the-policy-signer-certificates"></a>İlke imzalayan sertifikalarını silme

1.  Azure portal menüsüne veya giriş sayfasına gidin ve **tüm kaynaklar**' ı seçin.
1.  Filtre kutusuna kanıtlama sağlayıcısı adını girin.
1.  Kanıtlama sağlayıcısını seçin ve genel bakış sayfasına gidin.
1.  Pencerenin sol tarafındaki kaynak menüsünde veya alt bölmede **ilke imzalayan sertifikalar** ' ı seçin.
1.  Üstteki menüden **Sil** ' i seçin. Düğme, ilke imzalama gereksinimi olmadan oluşturulan kanıtlama sağlayıcıları için devre dışı bırakılır.
1.  İlke imzalayan sertifika dosyasını karşıya yükleyin ve **Sil**' i seçin. [İlke imzalayan sertifikalarının örneklerine bakın](./policy-signer-examples.md). 

## <a name="attestation-policy"></a>Kanıtlama ilkesi

Bu bölümde, bir kanıtlama ilkesinin nasıl görüntüleneceği ve ilke imzalama gereksinimi olmadan ve ile oluşturulan ilkelerin nasıl yapılandırılacağı açıklanmaktadır.

### <a name="view-an-attestation-policy"></a>Kanıtlama ilkesini görüntüleme

1.  Azure portal menüsüne veya giriş sayfasına gidin ve **tüm kaynaklar**' ı seçin.
1.  Filtre kutusuna kanıtlama sağlayıcısı adını girin.
1.  Kanıtlama sağlayıcısını seçin ve genel bakış sayfasına gidin.
1.  Pencerenin sol tarafındaki kaynak menüsünde veya alt bölmede **ilke** ' yi seçin. Kimlik doğrulaması için sertifika seçmek üzere bir istem görürsünüz. Lütfen devam etmek için uygun seçeneği belirleyin.
1.  Tercih edilen **kanıtlama türünü** seçin ve **geçerli ilkeyi** görüntüleyin.

### <a name="configure-an-attestation-policy"></a>Kanıtlama ilkesi yapılandırma

Kanıtlama sağlayıcısı bir ilke imzalama gereksinimi olmadan oluşturulduysa, bir ilkeyi JWT veya metin biçiminde karşıya yüklemek için bu adımları izleyin.

1. Azure portal menüsüne veya giriş sayfasına gidin ve **tüm kaynaklar**' ı seçin.
1. Filtre kutusuna kanıtlama sağlayıcısı adını girin.
1. Kanıtlama sağlayıcısını seçin ve genel bakış sayfasına gidin.
1. Pencerenin sol tarafındaki kaynak menüsünde veya alt bölmede **ilke** ' yi seçin.
1. Üstteki menüden **Yapılandır** ' ı seçin.
1. **Ilke biçimini** **JWT** veya **metin** olarak seçin.

   Kanıtlama sağlayıcısı, ilke imzalama gereksinimi olmadan oluşturulduysa, Kullanıcı bir ilkeyi **JWT** ya da **metin** biçiminde karşıya yükleyebilir.

      - JWT biçimi 'ni seçerseniz, ilke dosyasını **imzasız/IMZALı JWT** biçiminde ilke içeriğiyle karşıya yükleyin ve **Kaydet**' i seçin. [Bkz. ilke örnekleri](./policy-examples.md).
      - Metin biçimi seçeneğini belirlediyseniz, ilke dosyasını **metin** biçiminde içeriğe yükleyin veya metin alanına ilke Içeriğini girip **Kaydet**' i seçin. [Bkz. ilke örnekleri](./policy-examples.md).

   Karşıya dosya yükleme seçeneği için, ilke önizlemesi metin biçiminde gösterilir ve düzenlenemez.

1. Yapılandırılan ilkeyi görüntülemek için üstteki menüden **Yenile** ' yi seçin.


Kanıtlama sağlayıcısı bir ilke imzalama gereksinimi ile oluşturulduysa, bir ilkeyi JWT biçiminde karşıya yüklemek için aşağıdaki adımları izleyin.

1.  Azure portal menüsüne veya giriş sayfasına gidin ve **tüm kaynaklar**' ı seçin.
1.  Filtre kutusuna kanıtlama sağlayıcısı adını girin.
1.  Kanıtlama sağlayıcısını seçin ve genel bakış sayfasına gidin.
1.  Pencerenin sol tarafındaki kaynak menüsünde veya alt bölmede **ilke** ' yi seçin.
1.  Üstteki menüden **Yapılandır** ' ı seçin.
1.  İlke dosyasını **IMZALı JWT biçiminde** karşıya yükleyin ve **Kaydet**' i seçin. [Bkz. ilke örnekleri](./policy-examples.md).

    Kanıtlama sağlayıcısı bir ilke imzalama gereksinimi ile oluşturulduysa, Kullanıcı bir ilkeyi yalnızca **IMZALı JWT biçiminde** karşıya yükleyebilir.

    Karşıya dosya yükleme seçeneği için, ilke önizlemesi metin biçiminde gösterilir ve düzenlenemez.
    
1.  Yapılandırılan ilkeyi görüntülemek için **Yenile** ' yi seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Kanıtlama ilkesi yazma ve imzalama](author-sign-policy.md)
- [Kod örneklerini kullanarak bir SGX kuşatma atturuntest](/samples/browse/?expanded=azure&terms=attestation)

