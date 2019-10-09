---
title: Azure portal bir Linux VM oluşturma ve şifreleme
description: Bu hızlı başlangıçta, bir Linux sanal makinesi oluşturmak ve şifrelemek için Azure portal kullanmayı öğrenirsiniz.
author: msmbaldwin
ms.author: mbaldwin
ms.service: security
ms.topic: quickstart
ms.date: 10/02/2019
ms.openlocfilehash: 0249867a6af16505a692e7090639807ed3d239c9
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72031080"
---
# <a name="quickstart-create-and-encrypt-a-virtual-machine-with-the-azure-portal"></a>Hızlı başlangıç: Azure portal bir sanal makine oluşturma ve şifreleme

Azure sanal makineleri (VM’ler), Azure portalı üzerinden oluşturulabilir. Azure portal, sanal makineleri ve tüm ilgili kaynaklarını oluşturmaya yönelik tarayıcı tabanlı bir kullanıcı arabirimi sağlar. Bu hızlı başlangıçta, Ubuntu 18,04 LTS çalıştıran bir Linux sanal makinesini (VM) dağıtmak, şifreleme anahtarlarının depolanması için bir Anahtar Kasası oluşturmak ve VM 'yi şifrelemek için Azure portal kullanacaksınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açın

[Azure Portal](https://portal.azure.com)’ında oturum açın.

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** seçeneğini belirleyin
1. Arama kutusuna **Key Vault** yazın.
1. Sonuçlar listesinden **Key Vault**' yi seçin.
1. Key Vault bölümünde **Oluştur**' u seçin.
1. **Anahtar Kasası oluştur** ekranında yeni anahtar kasanız için benzersiz bir ad seçin.

    > [!Important]
    > Her Key Vault benzersiz bir adı olmalıdır. Aşağıdaki örnek, *Mya, v*adlı bir Key Vault oluşturur, ancak sizinkilerle farklı bir ad vermelisiniz.

1. Bir **abonelik**seçin.
1.  **Kaynak grubu**altında **Yeni oluştur**' u seçin. Açılan pencerede kaynak grubunun adı için *myResourceGroup* yazın ve **Tamam**'ı seçin. 

    ![Kaynak grubu oluşturma ekranı](./media/disk-encryption/portal-qs-keyvaultcreation.png)

1. **Konum** aşağı açılan menüsünde **Doğu ABD**' yi seçin.
1. Diğer seçenekleri varsayılan değerlerinde bırakın.
1. Sizi yeni bir ekrana götürür "erişim Ilkeleri" ni seçin.
1. "Birim şifrelemesi için Azure disk şifrelemesi erişimini etkinleştir" seçeneğinin yanındaki onay kutusunu işaretleyin.

    ![ResourceGroup oluşturma ekranı](./media/disk-encryption/portal-qs-keyvault-enable-encryption.png)

1. Erişim Ilkeleri ekranının en altında "gözden geçir + oluştur" a tıklayın.
1. İnceleme sonrasında "Oluştur" a tıklayın.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturun

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.

1. Yeni sayfada, popüler altında **Ubuntu Server 18,04 LTS**' yi seçin.
1. **Temel bilgiler** sekmesinde, **proje ayrıntıları**' nın altında, doğru aboneliğin seçildiğinden emin olun.
1. **Kaynak grubu**için, yukarıdaki anahtar kasanızı yaparken oluşturduğunuz kaynak grubunu seçin (örn. **myresourcegroup**)
1. **Sanal makine adı** Için *myvm*yazın ve şunları seçin 
1. **Bölge**için, anahtar kasanızı oluştururken kullandığınız bölgeyi (örn. **Doğu ABD**) seçin.
1. **Boyutun** *Standart D2s v3*olduğundan emin olun.
1. **Yönetici hesabı**altında, **parola**' yı seçin. Bir Kullanıcı adı ve parola girin.
    ![ResourceGroup oluşturma ekranı @ no__t-1
1. "Yönetim" sekmesini seçin ve bir tanılama depolama hesabınız olduğunu doğrulayın. Depolama hesabınız yoksa, "Yeni oluştur" u seçin, yeni hesabınıza bir ad verin ve "Tamam" ![ResourceGroup oluşturma ekranı @ no__t-1 ' i seçin
1. "Gözden geçir + oluştur" a tıklayın.
1. **Sanal makine oluştur** sayfasında oluşturmak üzere olduğunuz VM'nin ayrıntılarını görüntüleyebilirsiniz. Hazır olduğunuzda **Oluştur**'u seçin.

VM'nizin dağıtılması birkaç dakika sürer. Dağıtım tamamlandıktan sonra bir sonraki bölüme geçin.

## <a name="encrypt-the-virtual-machine"></a>Sanal makineyi şifreleyin

1. VM dağıtımı tamamlandığında **Kaynağa Git**' i seçin.
1. Sol taraftaki kenar çubuğundan **diskler**' i seçin.
1. Diskler ekranında **şifreleme**' yi seçin. 

    ![diskler ve şifreleme seçimi](./media/disk-encryption/portal-qs-disks-to-encryption.png)

1. Şifreleme ekranında, **şifrelemek Için diskler**altında **işletim sistemi ve veri diskleri**' ni seçin.
1. **Şifreleme ayarları**altında "bir Anahtar Kasası seçin ve şifreleme için anahtar" ' a tıklayın.
1. Sağ taraftaki kenar çubuğunda, daha önce oluşturduğunuz anahtar kasasının adını * Anahtar Kasası * * değeri olarak seçin ve **Seç**' e tıklayın.

    ![diskler ve şifreleme seçimi](./media/disk-encryption/portal-qs-encrypt-vm-screen.png)
1. Şifreleme ekranının üst kısmında "Kaydet" e tıklayın. Bir açılan pencere, sanal makinenin yeniden başlatılacağını uyarır. **Evet**'e tıklayın.


## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, sanal makineyi ve tüm ilişkili kaynakları silebilirsiniz. Bunu yapmak için, sanal makine için kaynak grubunu seçin, Sil ' i seçin ve ardından silinecek kaynak grubunun adını onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, şifreleme anahtarları için etkinleştirilen bir Key Vault oluşturdunuz, bir sanal makine oluşturdunuz ve sanal makineyi şifreleme için etkinleştirdiniz.  

> [!div class="nextstepaction"]
> [Azure disk şifrelemeye genel bakış](disk-encryption-overview.md)