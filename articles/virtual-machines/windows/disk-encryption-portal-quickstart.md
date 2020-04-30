---
title: Azure portalı ile Windows VM oluşturma ve şifreleme
description: Bu hızlı başlangıçta, bir Windows sanal makinesi oluşturmak ve şifrelemek için Azure portal kullanmayı öğrenirsiniz.
author: msmbaldwin
ms.author: mbaldwin
ms.service: virtual-machines-windows
ms.subservice: security
ms.topic: quickstart
ms.date: 10/02/2019
ms.openlocfilehash: 3a5fb354ab32dd560a94875dd84d891ce0b9d68c
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82081753"
---
# <a name="quickstart-create-and-encrypt-a-windows-virtual-machine-with-the-azure-portal"></a>Hızlı başlangıç: Azure portal bir Windows sanal makinesi oluşturma ve şifreleme

Azure sanal makineleri (VM’ler), Azure portalı üzerinden oluşturulabilir. Azure portal, sanal makineleri ve tüm ilgili kaynaklarını oluşturmaya yönelik tarayıcı tabanlı bir kullanıcı arabirimi sağlar. Bu hızlı başlangıçta, Ubuntu 18,04 LTS çalıştıran bir Windows sanal makinesini (VM) dağıtmak, şifreleme anahtarlarının depolanması için bir Anahtar Kasası oluşturmak ve VM 'yi şifrelemek için Azure portal kullanacaksınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](https://portal.azure.com) oturum açın.


## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.
1. Yeni sayfada, popüler bölümünde **Windows Server 2016 Datacenter**' ı seçin.
1. Temel bilgiler sekmesinde, proje ayrıntıları altında, doğru aboneliğin seçildiğinden emin olun ve ardından **Yeni kaynak grubu oluşturmayı**seçin. Ad olarak *Myresourcegroup* girin.
1. **Sanal makine adı**Için *myvm*yazın.
1. **Bölge**için, anahtar kasanızı oluştururken kullandığınız bölgeyi (örn. *Doğu ABD*) seçin.
1. **Boyutun** *Standart D2s v3*olduğundan emin olun.
1. **Yönetici hesabı**altında, **parola**' yı seçin. Bir Kullanıcı adı ve parola girin.
    ![ResourceGroup oluşturma ekranı](../media/disk-encryption/portal-qs-windows-vm-creation.png)
1. "Yönetim" sekmesini seçin ve bir tanılama depolama hesabınız olduğunu doğrulayın. Depolama hesabınız yoksa, "Yeni oluştur" seçeneğini belirleyin, yeni hesabınıza bir ad verin ve "Tamam" ![ResourceGroup oluşturma ekranı ' nı seçin.](../media/disk-encryption/portal-qs-vm-creation-storage.png)
1. "Gözden geçir + oluştur" a tıklayın.
1. **Sanal makine oluştur** sayfasında oluşturmak üzere olduğunuz VM'nin ayrıntılarını görüntüleyebilirsiniz. Hazır olduğunuzda **Oluştur**'u seçin.

VM'nizin dağıtılması birkaç dakika sürer. Dağıtım tamamlandıktan sonra bir sonraki bölüme geçin.

## <a name="encrypt-the-virtual-machine"></a>Sanal makineyi şifreleyin

1. VM dağıtımı tamamlandığında **Kaynağa Git**' i seçin.
1. Sol taraftaki kenar çubuğundan **diskler**' i seçin.
1. Diskler ekranında **şifreleme**' yi seçin. 

    ![diskler ve şifreleme seçimi](../media/disk-encryption/portal-qs-disks-to-encryption.png)

1. Şifreleme ekranında, **şifrelemek Için diskler**altında **işletim sistemi ve veri diskleri**' ni seçin.
1. **Şifreleme ayarları**altında, **şifreleme için bir anahtar kasası ve anahtar seçin**' i seçin.
1. **Azure Key Vault anahtar seçin** ekranında **Yeni oluştur**' u seçin.

    ![diskler ve şifreleme seçimi](../media/disk-encryption/portal-qs-keyvault-create.png)

1. **Anahtar Kasası oluştur** ekranında, kaynak grubunun VM 'yi oluşturmak Için kullandığınız kaynakla aynı olduğundan emin olun.
1. Anahtar kasanıza bir ad verin.  Azure genelindeki her Anahtar Kasası benzersiz bir ada sahip olmalıdır.
1. **Erişim ilkeleri** sekmesinde, **birim şifrelemesi Için Azure disk şifrelemesi** kutusunu işaretleyin.

    ![diskler ve şifreleme seçimi](../media/disk-encryption/portal-qs-keyvault-enable.png)

1. **İncele ve oluştur**’u seçin.  
1. Anahtar Kasası doğrulamadan geçtikten sonra **Oluştur**' u seçin. Bu, sizi **Azure Key Vault ekranından seçim tuşuna** geri döndürür.
1. **Anahtar** alanını boş bırakın ve **Seç**' i seçin.
1. Şifreleme ekranının üst kısmında **Kaydet**' e tıklayın. Bir açılan pencere, sanal makinenin yeniden başlatılacağını uyarır. **Evet**' e tıklayın.


## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, sanal makineyi ve tüm ilişkili kaynakları silebilirsiniz. Bunu yapmak için, sanal makinenin kaynak grubunu ve Sil’i seçin, ardından silinecek kaynak grubunun adını onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, şifreleme anahtarları için etkinleştirilen bir Key Vault oluşturdunuz, bir sanal makine oluşturdunuz ve sanal makineyi şifreleme için etkinleştirdiniz.  

> [!div class="nextstepaction"]
> [Azure disk şifrelemeye genel bakış](disk-encryption-overview.md)
