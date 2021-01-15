---
title: Azure kanıtlama 'nı Azure portal ile ayarlama
description: Azure portal kullanarak bir kanıtlama sağlayıcısı ayarlama ve yapılandırma.
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: 52c0dd6825c8576d97e10d3f0568ca5fb81e789e
ms.sourcegitcommit: f5b8410738bee1381407786fcb9d3d3ab838d813
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98210823"
---
# <a name="quickstart-set-up-azure-attestation-with-azure-portal"></a>Hızlı başlangıç: Azure portal ile Azure kanıtlama ayarlama

Azure portal kullanarak bir kanıtlama sağlayıcısını yönetmek için aşağıdaki adımları izleyin.

## <a name="attestation-provider"></a>Kanıtlama sağlayıcısı

### <a name="create-an-attestation-provider"></a>Kanıtlama sağlayıcısı oluşturma

#### <a name="to-configure-the-provider-with-unsigned-policies"></a>Sağlayıcıyı imzasız ilkelerle yapılandırmak için

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

#### <a name="to-configure-the-provider-with-signed-policies"></a>Sağlayıcıyı imzalı ilkelerle yapılandırmak için

1.  Azure portal menüsünde veya giriş sayfasından **kaynak oluştur** ' u seçin.
2.  Arama kutusuna **kanıtlama** girin
3.  Sonuçlar listesinden **kanıtlama Microsoft Azure** seçin
4.  Microsoft Azure kanıtlama sayfasında **Oluştur** ' u seçin.
5.  Kanıtlama sağlayıcısı Oluştur sayfasında, aşağıdaki bilgileri sağlayın:
    
    a. **Abonelik**: abonelik seçin
    
    b. **Kaynak grubu**: var olan bir kaynak grubunu seçin veya **Yeni oluştur** ' u seçin ve bir kaynak grubu adı girin
    
    c. **Ad**: benzersiz bir ad gerekiyor

    d. **Konum**: bir konum seçin 
    
    e. **İlke imzalayan sertifikaları dosyası**: kanıtlama sağlayıcısını ilke imzalama sertifikaları ile yapılandırmak için, CERT dosyasını yükleyin. Örneklere [buraya](/azure/attestation/policy-signer-examples) bakın 
6.  Gerekli girişleri sağladıktan sonra, **gözden geçir + oluştur** ' a tıklayın.
7.  Varsa doğrulama sorunlarını giderin ve **Oluştur**' a tıklayın.

### <a name="view-attestation-provider"></a>Kanıtlama sağlayıcısını görüntüle

1.  Azure portal menüsünden veya giriş sayfasından **tüm kaynaklar** ' ı seçin.
2.  Filtre kutusuna kanıtlama sağlayıcı adı girin ve seçin

### <a name="delete-attestation-provider"></a>Kanıtlama sağlayıcısını Sil

1.  Azure portal menüsünden veya giriş sayfasından **tüm kaynaklar** ' ı seçin.
2.  Filtre kutusuna kanıtlama sağlayıcı adı girin
3.  Onay kutusunu seçin ve **Sil** ' e tıklayın.
4.  Evet yazın ve **Sil** [veya] öğesine tıklayın
1.  Azure portal menüsünden veya giriş sayfasından **tüm kaynaklar** ' ı seçin.
2.  Filtre kutusuna kanıtlama sağlayıcı adı girin
3.  Kanıtlama sağlayıcısını seçin ve genel bakış sayfasına gidin
4.  Üstteki menüden **Sil** ' e tıklayın ve **Evet** ' e tıklayın.


## <a name="attestation-policy-signers"></a>Kanıtlama ilkesi imzalayanlar

### <a name="view-policy-signer-certificates"></a>İlke imzalayan sertifikalarını görüntüleme

1.  Azure portal menüsünden veya giriş sayfasından **tüm kaynaklar** ' ı seçin.
2.  Filtre kutusuna kanıtlama sağlayıcı adı girin
3.  Kanıtlama sağlayıcısını seçin ve genel bakış sayfasına gidin
4.  Sol taraftaki kaynak menüsünde veya alt bölmedeki **ilke imzalayan sertifikalar** ' a tıklayın.
5.  **İlke imzalayan sertifikaları indir** ' e tıklayın (ilke imzalama gereksinimi olmadan oluşturulan kanıtlama sağlayıcıları için düğme devre dışı bırakılır)
6.  İndirilen metin dosyası, tüm sertifikaları JWS biçiminde olacak.
a.  İndirilen sertifika sayısını ve sertifikaları doğrulayın.

### <a name="add-policy-signer-certificate"></a>İlke imzalayan sertifikası Ekle

1.  Azure portal menüsünden veya giriş sayfasından **tüm kaynaklar** ' ı seçin.
2.  Filtre kutusuna kanıtlama sağlayıcı adı girin
3.  Kanıtlama sağlayıcısını seçin ve genel bakış sayfasına gidin
4.  Sol taraftaki kaynak menüsünde veya alt bölmedeki **ilke imzalayan sertifikalar** ' a tıklayın.
5.  Üst menüden **Ekle** ' ye tıklayın (ilke imzalama gereksinimi olmadan oluşturulan kanıtlama sağlayıcıları için düğme devre dışı bırakılır)
6.  İlke imzalayan sertifika dosyasını karşıya yükleyin ve **Ekle**' ye tıklayın. Örneklere [buraya](/azure/attestation/policy-signer-examples) bakın

### <a name="delete-policy-signer-certificate"></a>İlke imzalayan sertifikasını Sil

1.  Azure portal menüsünden veya giriş sayfasından **tüm kaynaklar** ' ı seçin.
2.  Filtre kutusuna kanıtlama sağlayıcı adı girin
3.  Kanıtlama sağlayıcısını seçin ve genel bakış sayfasına gidin
4.  Sol taraftaki kaynak menüsünde veya alt bölmedeki **ilke imzalayan sertifikalar** ' a tıklayın.
5.  Üstteki menüden **Sil** ' e tıklayın (ilke imzalama gereksinimi olmadan oluşturulan kanıtlama sağlayıcıları için düğme devre dışı bırakılır)
6.  İlke imzalayan sertifika dosyasını karşıya yükleyin ve **Sil**' e tıklayın. Örneklere [buraya](/azure/attestation/policy-signer-examples) bakın 

## <a name="attestation-policy"></a>Kanıtlama ilkesi

### <a name="view-attestation-policy"></a>Kanıtlama ilkesini görüntüle

1.  Azure portal menüsünden veya giriş sayfasından **tüm kaynaklar** ' ı seçin.
2.  Filtre kutusuna kanıtlama sağlayıcı adı girin
3.  Kanıtlama sağlayıcısını seçin ve genel bakış sayfasına gidin
4.  Sol taraftaki kaynak menüsünde veya alt bölmedeki **ilke** ' ye tıklayın.
5.  Tercih edilen **kanıtlama türünü** seçin ve **geçerli ilkeyi** görüntüleyin

### <a name="configure-attestation-policy"></a>Kanıtlama ilkesini yapılandırma

#### <a name="when-attestation-provider-is-created-without-policy-signing-requirement"></a>Kanıtlama sağlayıcısı, ilke imzalama gereksinimi olmadan oluşturulduğunda

##### <a name="upload-policy-in-jwt-format"></a>İlkeyi JWT biçiminde karşıya yükle

1.  Azure portal menüsünden veya giriş sayfasından **tüm kaynaklar** ' ı seçin.
2.  Filtre kutusuna kanıtlama sağlayıcı adı girin
3.  Kanıtlama sağlayıcısını seçin ve genel bakış sayfasına gidin
4.  Sol taraftaki kaynak menüsünde veya alt bölmedeki **ilke** ' ye tıklayın.
5.  Üstteki menüden **Yapılandır** ' a tıklayın
6.  Kanıtlama sağlayıcısı, ilke imzalama gereksinimi olmadan oluşturulduğunda, Kullanıcı bir ilkeyi **JWT** veya **metin** biçiminde karşıya yükleyebilir
7.  **JWT** olarak **ilke biçimi** seçin
8.  İlke dosyasını **imzasız/imzalı BIR JWT** biçiminde karşıya yükleyin ve **Kaydet**' e tıklayın. Örneklere [buraya](/azure/attestation/policy-examples) bakın
    
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
8.  İlke dosyasını **metin** biçiminde içerikle karşıya yükleyin veya metin alanına ilke Içeriğini girip **Kaydet**' e tıklayın. Örneklere [buraya](/azure/attestation/policy-examples) bakın

    Karşıya dosya yükleme seçeneği için, ilke önizlemesi metin biçiminde gösterilir ve ilke önizlemesi düzenlenemez.

8.  Yapılandırılmış ilkeyi görüntülemek için **Yenile** 'ye tıklayın

#### <a name="when-attestation-provider-is-created-with-policy-signing-requirement"></a>Kanıtlama sağlayıcısı ilke imzalama gereksinimi ile oluşturulduğunda

##### <a name="upload-policy-in-jwt-format"></a>İlkeyi JWT biçiminde karşıya yükle

1.  Azure portal menüsünden veya giriş sayfasından **tüm kaynaklar** ' ı seçin.
2.  Filtre kutusuna kanıtlama sağlayıcı adı girin
3.  Kanıtlama sağlayıcısını seçin ve genel bakış sayfasına gidin
4.  Sol taraftaki kaynak menüsünde veya alt bölmedeki **ilke** ' ye tıklayın.
5.  Üstteki menüden **Yapılandır** ' a tıklayın
6.  Kanıtlama sağlayıcısı ilke imzalama gereksinimiyle oluşturulduğunda, Kullanıcı yalnızca **IMZALı JWT biçiminde** bir ilkeyi karşıya yükleyebilir
7.  Karşıya yükleme ilkesi dosyası **IMZALı JWT biçimidir** ve **Kaydet**' e tıklayın. Örneklere [buraya](/azure/attestation/policy-examples) bakın

    Karşıya dosya yükleme seçeneği için, ilke önizlemesi metin biçiminde gösterilir ve ilke önizlemesi düzenlenemez.
    
8.  Yapılandırılmış ilkeyi görüntülemek için **Yenile** 'ye tıklayın

 










