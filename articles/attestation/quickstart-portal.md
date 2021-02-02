---
title: Azure kanıtlama 'nı Azure portal ile ayarlama
description: Azure portal kullanarak bir kanıtlama sağlayıcısı ayarlama ve yapılandırma.
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: 86adac557c6de133e95e97bfedbd302cc6a2b27e
ms.sourcegitcommit: eb546f78c31dfa65937b3a1be134fb5f153447d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99429169"
---
# <a name="quickstart-set-up-azure-attestation-with-azure-portal"></a>Hızlı başlangıç: Azure portal ile Azure kanıtlama ayarlama

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Azure portal kullanarak bir kanıtlama sağlayıcısını yönetmek için aşağıdaki adımları izleyin.

## <a name="1-attestation-provider"></a>1. kanıtlama sağlayıcısı

### <a name="11-create-an-attestation-provider"></a>1,1 kanıtlama sağlayıcısı oluşturma

#### <a name="111-to-configure-the-provider-with-unsigned-policies"></a>1.1.1 sağlayıcıyı imzasız ilkelerle yapılandırma

1.  Azure portal menüsünde veya giriş sayfasından **kaynak oluştur** ' u seçin.
2.  Arama kutusuna **kanıtlama** girin
3.  Sonuçlar listesinden **kanıtlama Microsoft Azure** seçin
4.  Microsoft Azure kanıtlama sayfasında **Oluştur** ' u seçin.
5.  Kanıtlama sağlayıcısı Oluştur sayfasında, aşağıdaki girişleri sağlayın:
    
    **Abonelik**: abonelik seçin
    
    **Kaynak grubu**: var olan bir kaynak grubunu seçin veya **Yeni oluştur** ' u seçin ve bir kaynak grubu adı girin
    
    **Ad**: benzersiz bir ad gerekiyor

    **Konum**: bir konum seçin 
    
    **İlke imzalayan sertifika dosyası**: sağlayıcıyı imzasız ilkelerle yapılandırmak için ilke imzalayan sertifikaları dosyasını karşıya yüklemeyin 
6.  Gerekli girişleri sağladıktan sonra, **gözden geçir + oluştur** ' a tıklayın.
7.  Varsa doğrulama sorunlarını giderin ve **Oluştur**' a tıklayın.

#### <a name="112-to-configure-the-provider-with-signed-policies"></a>1.1.2 imzalı ilkelerle sağlayıcıyı yapılandırmak için

1.  Azure portal menüsünde veya giriş sayfasından **kaynak oluştur** ' u seçin.
2.  Arama kutusuna **kanıtlama** girin
3.  Sonuçlar listesinden **kanıtlama Microsoft Azure** seçin
4.  Microsoft Azure kanıtlama sayfasında **Oluştur** ' u seçin.
5.  Kanıtlama sağlayıcısı Oluştur sayfasında, aşağıdaki bilgileri sağlayın:
    
    a. **Abonelik**: abonelik seçin
    
    b. **Kaynak grubu**: var olan bir kaynak grubunu seçin veya **Yeni oluştur** ' u seçin ve bir kaynak grubu adı girin
    
    c. **Ad**: benzersiz bir ad gerekiyor

    d. **Konum**: bir konum seçin 
    
    e. **İlke imzalayan sertifikaları dosyası**: kanıtlama sağlayıcısını ilke imzalama sertifikaları ile yapılandırmak için, CERT dosyasını yükleyin. Örneklere [buraya](./policy-signer-examples.md) bakın 
6.  Gerekli girişleri sağladıktan sonra, **gözden geçir + oluştur** ' a tıklayın.
7.  Varsa doğrulama sorunlarını giderin ve **Oluştur**' a tıklayın.

### <a name="12-view-attestation-provider"></a>1,2 kanıtlama sağlayıcısını görüntüle

1.  Azure portal menüsünden veya giriş sayfasından **tüm kaynaklar** ' ı seçin.
2.  Filtre kutusuna kanıtlama sağlayıcı adı girin ve seçin

### <a name="13-delete-attestation-provider"></a>1,3 kanıtlama sağlayıcısını Sil

1.  Azure portal menüsünden veya giriş sayfasından **tüm kaynaklar** ' ı seçin.
2.  Filtre kutusuna kanıtlama sağlayıcı adı girin
3.  Onay kutusunu seçin ve **Sil** ' e tıklayın.
4.  Evet yazın ve **Sil** [veya] öğesine tıklayın
1.  Azure portal menüsünden veya giriş sayfasından **tüm kaynaklar** ' ı seçin.
2.  Filtre kutusuna kanıtlama sağlayıcı adı girin
3.  Kanıtlama sağlayıcısını seçin ve genel bakış sayfasına gidin
4.  Üstteki menüden **Sil** ' e tıklayın ve **Evet** ' e tıklayın.


## <a name="2-attestation-policy-signers"></a>2. kanıtlama ilkesi imzalayanlar

### <a name="21-view-policy-signer-certificates"></a>2,1 ilke imzalayan sertifikalarını görüntüleme

1.  Azure portal menüsünden veya giriş sayfasından **tüm kaynaklar** ' ı seçin.
2.  Filtre kutusuna kanıtlama sağlayıcı adı girin
3.  Kanıtlama sağlayıcısını seçin ve genel bakış sayfasına gidin
4.  Sol taraftaki kaynak menüsünde veya alt bölmedeki **ilke imzalayan sertifikalar** ' a tıklayın.
5.  **İlke imzalayan sertifikaları indir** ' e tıklayın (ilke imzalama gereksinimi olmadan oluşturulan kanıtlama sağlayıcıları için düğme devre dışı bırakılır)
6.  İndirilen metin dosyası, tüm sertifikaları JWS biçiminde olacak.
a.  İndirilen sertifika sayısını ve sertifikaları doğrulayın.

### <a name="22-add-policy-signer-certificate"></a>2,2 ilke imzalayan sertifikası ekleme

1.  Azure portal menüsünden veya giriş sayfasından **tüm kaynaklar** ' ı seçin.
2.  Filtre kutusuna kanıtlama sağlayıcı adı girin
3.  Kanıtlama sağlayıcısını seçin ve genel bakış sayfasına gidin
4.  Sol taraftaki kaynak menüsünde veya alt bölmedeki **ilke imzalayan sertifikalar** ' a tıklayın.
5.  Üst menüden **Ekle** ' ye tıklayın (ilke imzalama gereksinimi olmadan oluşturulan kanıtlama sağlayıcıları için düğme devre dışı bırakılır)
6.  İlke imzalayan sertifika dosyasını karşıya yükleyin ve **Ekle**' ye tıklayın. Örneklere [buraya](./policy-signer-examples.md) bakın

### <a name="23-delete-policy-signer-certificate"></a>2,3 ilke imzalayan sertifikasını silme

1.  Azure portal menüsünden veya giriş sayfasından **tüm kaynaklar** ' ı seçin.
2.  Filtre kutusuna kanıtlama sağlayıcı adı girin
3.  Kanıtlama sağlayıcısını seçin ve genel bakış sayfasına gidin
4.  Sol taraftaki kaynak menüsünde veya alt bölmedeki **ilke imzalayan sertifikalar** ' a tıklayın.
5.  Üstteki menüden **Sil** ' e tıklayın (ilke imzalama gereksinimi olmadan oluşturulan kanıtlama sağlayıcıları için düğme devre dışı bırakılır)
6.  İlke imzalayan sertifika dosyasını karşıya yükleyin ve **Sil**' e tıklayın. Örneklere [buraya](./policy-signer-examples.md) bakın 

## <a name="3-attestation-policy"></a>3. kanıtlama ilkesi

### <a name="31-view-attestation-policy"></a>3,1 kanıtlama ilkesini görüntüleme

1.  Azure portal menüsünden veya giriş sayfasından **tüm kaynaklar** ' ı seçin.
2.  Filtre kutusuna kanıtlama sağlayıcı adı girin
3.  Kanıtlama sağlayıcısını seçin ve genel bakış sayfasına gidin
4.  Sol taraftaki kaynak menüsünde veya alt bölmedeki **ilke** ' ye tıklayın.
5.  Tercih edilen **kanıtlama türünü** seçin ve **geçerli ilkeyi** görüntüleyin

### <a name="32-configure-attestation-policy"></a>3,2 kanıtlama ilkesini yapılandırma

#### <a name="321-when-attestation-provider-is-created-without-policy-signing-requirement"></a>3.2.1, kanıtlama sağlayıcısı ilke imzalama gereksinimi olmadan oluşturulduğunda

##### <a name="upload-policy-in-jwt-format"></a>İlkeyi JWT biçiminde karşıya yükle

1.  Azure portal menüsünden veya giriş sayfasından **tüm kaynaklar** ' ı seçin.
2.  Filtre kutusuna kanıtlama sağlayıcı adı girin
3.  Kanıtlama sağlayıcısını seçin ve genel bakış sayfasına gidin
4.  Sol taraftaki kaynak menüsünde veya alt bölmedeki **ilke** ' ye tıklayın.
5.  Üstteki menüden **Yapılandır** ' a tıklayın
6.  Kanıtlama sağlayıcısı, ilke imzalama gereksinimi olmadan oluşturulduğunda, Kullanıcı bir ilkeyi **JWT** veya **metin** biçiminde karşıya yükleyebilir
7.  **JWT** olarak **ilke biçimi** seçin
8.  İlke dosyasını **imzasız/imzalı BIR JWT** biçiminde karşıya yükleyin ve **Kaydet**' e tıklayın. Örneklere [buraya](./policy-examples.md) bakın
    
    Karşıya dosya yükleme seçeneği için, ilke önizlemesi metin biçiminde gösterilir ve ilke önizlemesi düzenlenemez.

7.  Yapılandırılmış ilkeyi görüntülemek için üstteki menüden **Yenile** ' ye tıklayın

##### <a name="upload-policy-in-text-format"></a>İlkeyi metin biçiminde karşıya yükle

1.  Azure portal menüsünden veya giriş sayfasından **tüm kaynaklar** ' ı seçin.
2.  Filtre kutusuna kanıtlama sağlayıcı adı girin
3.  Kanıtlama sağlayıcısını seçin ve genel bakış sayfasına gidin
4.  Sol taraftaki kaynak menüsünde veya alt bölmedeki **ilke** ' ye tıklayın.
5.  Üstteki menüden **Yapılandır** ' a tıklayın
6.  Kanıtlama sağlayıcısı, ilke imzalama gereksinimi olmadan oluşturulduğunda, Kullanıcı bir ilkeyi **JWT** veya **metin** biçiminde karşıya yükleyebilir
7.  **Ilke biçimini** **metin** olarak seçin
8.  İlke dosyasını **metin** biçiminde içerikle karşıya yükleyin veya metin alanına ilke Içeriğini girip **Kaydet**' e tıklayın. Örneklere [buraya](./policy-examples.md) bakın

    Karşıya dosya yükleme seçeneği için, ilke önizlemesi metin biçiminde gösterilir ve ilke önizlemesi düzenlenemez.

8.  Yapılandırılmış ilkeyi görüntülemek için **Yenile** 'ye tıklayın

#### <a name="322-when-attestation-provider-is-created-with-policy-signing-requirement"></a>3.2.2 ilke imzalama gereksinimi ile kanıtlama sağlayıcısı oluşturulduğunda

##### <a name="upload-policy-in-jwt-format"></a>İlkeyi JWT biçiminde karşıya yükle

1.  Azure portal menüsünden veya giriş sayfasından **tüm kaynaklar** ' ı seçin.
2.  Filtre kutusuna kanıtlama sağlayıcı adı girin
3.  Kanıtlama sağlayıcısını seçin ve genel bakış sayfasına gidin
4.  Sol taraftaki kaynak menüsünde veya alt bölmedeki **ilke** ' ye tıklayın.
5.  Üstteki menüden **Yapılandır** ' a tıklayın
6.  Kanıtlama sağlayıcısı ilke imzalama gereksinimiyle oluşturulduğunda, Kullanıcı yalnızca **IMZALı JWT biçiminde** bir ilkeyi karşıya yükleyebilir
7.  Karşıya yükleme ilkesi dosyası **IMZALı JWT biçimidir** ve **Kaydet**' e tıklayın. Örneklere [buraya](./policy-examples.md) bakın

    Karşıya dosya yükleme seçeneği için, ilke önizlemesi metin biçiminde gösterilir ve ilke önizlemesi düzenlenemez.
    
8.  Yapılandırılmış ilkeyi görüntülemek için **Yenile** 'ye tıklayın

## <a name="next-steps"></a>Sonraki adımlar

- [Kanıtlama ilkesi yazma ve imzalama](author-sign-policy.md)
- [Kod örneklerini kullanarak bir SGX kuşatma atturuntest](/samples/browse/?expanded=azure&terms=attestation)

