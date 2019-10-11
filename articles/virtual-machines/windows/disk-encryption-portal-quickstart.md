---
title: Azure portal ile Windows VM oluşturma ve şifreleme
description: Bu hızlı başlangıçta, bir Windows sanal makinesi oluşturmak ve şifrelemek için Azure portal kullanmayı öğrenirsiniz.
author: msmbaldwin
ms.author: mbaldwin
ms.service: security
ms.topic: quickstart
ms.date: 10/02/2019
ms.openlocfilehash: 240c0e157d7fcd8b76ee7f42f4c780361df6281d
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72246126"
---
# <a name="quickstart-create-and-encrypt-a-windows-virtual-machine-with-the-azure-portal"></a>Hızlı başlangıç: Azure portal bir Windows sanal makinesi oluşturma ve şifreleme

Azure sanal makineleri (VM 'Ler) Azure portal aracılığıyla oluşturulabilir. Azure portal, VM 'Ler ve bunlarla ilişkili kaynakları oluşturmak için tarayıcı tabanlı bir kullanıcı arabirimidir. Bu hızlı başlangıçta, Ubuntu 18,04 LTS çalıştıran bir Windows sanal makinesini (VM) dağıtmak, şifreleme anahtarlarının depolanması için bir Anahtar Kasası oluşturmak ve VM 'yi şifrelemek için Azure portal kullanacaksınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure 'da oturum açın

[Azure Portal](https://portal.azure.com)oturum açın.

## <a name="create-a-key-vault"></a>Anahtar Kasası oluşturma

1. Azure portal sol üst köşesinde **kaynak oluştur** seçeneğini belirleyin
1. Arama kutusuna **Key Vault**girin.
1. Sonuçlar listesinden **Key Vault**' yi seçin.
1. Key Vault bölümünde **Oluştur**' u seçin.
1. **Anahtar Kasası oluştur** ekranında yeni anahtar kasanız için benzersiz bir ad seçin.

    > [!Important]
    > Her Key Vault benzersiz bir adı olmalıdır. Aşağıdaki örnek, *Mya, v*adlı bir Key Vault oluşturur, ancak sizinkilerle farklı bir ad vermelisiniz.

1. Bir **abonelik**seçin.
1.  **Kaynak grubu**altında **Yeni oluştur**' u seçin. Açılır pencerede, kaynak grubunun adı için *Myresourcegroup* yazın ve ardından **Tamam**' ı seçin. 

    ![Kaynak grubu oluşturma ekranı](../media/disk-encryption/portal-qs-keyvaultcreation.png)

1. **Konum** aşağı açılan menüsünde **Doğu ABD**' yi seçin.
1. Diğer seçenekleri varsayılanlarına bırakın.
1. Sizi yeni bir ekrana götürür "erişim Ilkeleri" ni seçin.
1. "Birim şifrelemesi için Azure disk şifrelemesi erişimini etkinleştir" seçeneğinin yanındaki onay kutusunu işaretleyin.

    ![ResourceGroup oluşturma ekranı](../media/disk-encryption/portal-qs-keyvault-enable-encryption.png)

1. Erişim Ilkeleri ekranının en altında "gözden geçir + oluştur" a tıklayın.
1. İnceleme sonrasında "Oluştur" a tıklayın.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

1. Azure portal sol üst köşesinde **kaynak oluştur ' a** tıklayın.

1. Yeni sayfada, popüler bölümünde **Windows Server 2016 Datacenter**' ı seçin.
1. **Temel bilgiler** sekmesinde, **proje ayrıntıları**' nın altında, doğru aboneliğin seçildiğinden emin olun.
1. **Kaynak grubu**için, yukarıdaki anahtar kasanızı yaparken oluşturduğunuz kaynak grubunu seçin (örn. **myresourcegroup**)
1. **Sanal makine adı** Için *myvm*yazın ve şunları seçin 
1. **Bölge**için, anahtar kasanızı oluştururken kullandığınız bölgeyi (örn. **Doğu ABD**) seçin.
1. **Boyutun** *Standart D2s v3*olduğundan emin olun.
1. **Yönetici hesabı**altında, **parola**' yı seçin. Bir Kullanıcı adı ve parola girin.
    ![ResourceGroup oluşturma ekranı @ no__t-1
1. "Yönetim" sekmesini seçin ve bir tanılama depolama hesabınız olduğunu doğrulayın. Depolama hesabınız yoksa, "Yeni oluştur" u seçin, yeni hesabınıza bir ad verin ve "Tamam" ![ResourceGroup oluşturma ekranı @ no__t-1 ' i seçin
1. "Gözden geçir + oluştur" a tıklayın.
1. **Sanal makine oluştur** sayfasında, oluşturmak üzere olduğunuz VM hakkındaki ayrıntıları görebilirsiniz. Hazırsanız **Oluştur**' u seçin.

VM 'nizin dağıtılması birkaç dakika sürer. Dağıtım bittiğinde, sonraki bölüme geçin.

## <a name="encrypt-the-virtual-machine"></a>Sanal makineyi şifreleyin

1. VM dağıtımı tamamlandığında **Kaynağa Git**' i seçin.
1. Sol taraftaki kenar çubuğundan **diskler**' i seçin.
1. Diskler ekranında **şifreleme**' yi seçin. 

    ![diskler ve şifreleme seçimi](../media/disk-encryption/portal-qs-disks-to-encryption.png)

1. Şifreleme ekranında, **şifrelemek Için diskler**altında **işletim sistemi ve veri diskleri**' ni seçin.
1. **Şifreleme ayarları**altında "bir Anahtar Kasası seçin ve şifreleme için anahtar" ' a tıklayın.
1. Sağ taraftaki kenar çubuğunda, daha önce oluşturduğunuz anahtar kasasının adını * Anahtar Kasası * * değeri olarak seçin ve **Seç**' e tıklayın.

    ![diskler ve şifreleme seçimi](../media/disk-encryption/portal-qs-encrypt-vm-screen.png)
1. Şifreleme ekranının üst kısmında "Kaydet" e tıklayın. Bir açılan pencere, sanal makinenin yeniden başlatılacağını uyarır. Yordamı tamamlamak için **Evet**yüklemesini desteklemesi gerekir.


## <a name="clean-up-resources"></a>Kaynakları Temizleme

Artık gerekli değilse, kaynak grubunu, sanal makineyi ve tüm ilgili kaynakları silebilirsiniz. Bunu yapmak için, sanal makine için kaynak grubunu seçin, Sil ' i seçin ve ardından silinecek kaynak grubunun adını onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, şifreleme anahtarları için etkinleştirilen bir Key Vault oluşturdunuz, bir sanal makine oluşturdunuz ve sanal makineyi şifreleme için etkinleştirdiniz.  

> [!div class="nextstepaction"]
> [Azure disk şifrelemeye genel bakış](disk-encryption-overview.md)