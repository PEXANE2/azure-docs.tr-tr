---
title: Azure portalı ile Linux VM oluşturma ve şifreleme
description: Bu hızlı başlangıçta, bir Linux sanal makinesi oluşturmak ve şifrelemek için Azure portalını nasıl kullanacağınızı öğrenirsiniz
author: msmbaldwin
ms.author: mbaldwin
ms.service: virtual-machines-linux
ms.subservice: security
ms.topic: quickstart
ms.date: 10/02/2019
ms.openlocfilehash: 9f09f28dc63e7f061946a66beb59bd4c62be70aa
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78970568"
---
# <a name="quickstart-create-and-encrypt-a-virtual-machine-with-the-azure-portal"></a>Hızlı başlatma: Azure portalı ile sanal bir makine oluşturun ve şifreleyin

Azure sanal makineleri (VM’ler), Azure portalı üzerinden oluşturulabilir. Azure portal, sanal makineleri ve tüm ilgili kaynaklarını oluşturmaya yönelik tarayıcı tabanlı bir kullanıcı arabirimi sağlar. Bu hızlı başlangıçta, Ubuntu 18.04 LTS çalıştıran bir Linux sanal makinesini (VM) dağıtmak, şifreleme anahtarlarının depolanması için önemli bir kasa oluşturmak ve VM'yi şifrelemek için Azure portalını kullanacaksınız.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](https://portal.azure.com)oturum açın.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.
1. Yeni sayfada, Popüler **altında, Ubuntu Server 18.04 LTS'yi**seçin.
1. Temel **Bilgiler** sekmesinde, **Proje ayrıntıları**altında doğru aboneliğin seçildiğinden emin olun.
1. **Kaynak grubu**için, anahtar kasanızı yukarıda yaparken oluşturduğunuz kaynak grubunu seçin (örn. **myResourceGroup).**
1. **Sanal makine adı için** *MyVM*girin.
1. **Bölge**için, yukarıdaki anahtar kasanızı yaparken kullandığınız bölgeyi seçin (örn. **Doğu ABD).**
1. **Boyutu** *Standart D2s v3*olduğundan emin olun.
1. **Yönetici hesabı**altında, **Parola'yı**seçin. Bir kullanıcı adı ve parola girin.
    ![ResourceGroup oluşturma ekranı](./media/disk-encryption/portal-qs-vm-creation.png)
1. "Yönetim" sekmesini seçin ve bir Tanılama Depolama Hesabınız olduğunu doğrulayın. Depolama hesabınız yoksa, "Yeni Oluştur"u seçin, yeni hesabınıza bir ad ![verin ve "Ok" Kaynak Grubu oluşturma ekranını seçin](./media/disk-encryption/portal-qs-vm-creation-storage.png)
1. "Gözden Geçir + Oluştur" seçeneğini tıklayın.
1. **Sanal makine oluştur** sayfasında oluşturmak üzere olduğunuz VM'nin ayrıntılarını görüntüleyebilirsiniz. Hazır olduğunuzda **Oluştur**'u seçin.

VM'nizin dağıtılması birkaç dakika sürer. Dağıtım tamamlandıktan sonra bir sonraki bölüme geçin.

## <a name="encrypt-the-virtual-machine"></a>Sanal makineyi şifreleme

1. VM dağıtımı tamamlandığında **kaynağa git'i**seçin.
1. Sol kenar çubuğunda **Diskler'i**seçin.
1. Diskler ekranında **Şifreleme'yi**seçin. 

    ![diskler ve şifreleme seçimi](../media/disk-encryption/portal-qs-disks-to-encryption.png)

1. Şifreleme ekranında, **Diskler altında şifrelemek için,** **işletim sistemi ve veri diskleri**seçin.
1. **Şifreleme ayarları**altında, şifreleme için bir **anahtar kasası ve anahtar seçin'i**seçin.
1. Azure **Key Vault ekranından Seç tuşu** üzerinde **Yeni Oluştur'u**seçin.

    ![diskler ve şifreleme seçimi](../media/disk-encryption/portal-qs-keyvault-create.png)

1. Create **tuşu tonoz** ekranında, Kaynak Grubu'nun VM'yi oluşturmak için kullandığınız la aynı olduğundan emin olun.
1. Anahtar kasana bir isim ver.  Azure'daki her anahtar kasanın benzersiz bir adı olmalıdır.
1. Erişim **İlkeleri** sekmesinde, ses düzeyi şifreleme kutusu **için Azure Disk Şifrelemesini** işaretleyin.

    ![diskler ve şifreleme seçimi](../media/disk-encryption/portal-qs-keyvault-enable.png)

1. **İncele ve oluştur**’u seçin.  
1. Anahtar kasası doğrulamayı geçtikten sonra **Oluştur'u**seçin. Bu işlem sizi **Azure Key Vault ekranından Seç tuşuna** döndürecektir.
1. **Anahtar** alanını boş bırakın ve **Seç'i**seçin.
1. Şifreleme ekranının üst kısmında **Kaydet'i**tıklatın. Bir açılır pencere vm yeniden başlatılacak sizi uyarır. **Evet'i**tıklatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, sanal makineyi ve tüm ilişkili kaynakları silebilirsiniz. Bunu yapmak için, sanal makinenin kaynak grubunu ve Sil’i seçin, ardından silinecek kaynak grubunun adını onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmada, şifreleme anahtarları için etkinleştirilen bir Key Vault oluşturdunuz, sanal bir makine oluşturdunuz ve şifreleme için sanal makineyi etkinleştirdin.  

> [!div class="nextstepaction"]
> [Azure Disk Şifreleme'ye genel bakış](disk-encryption-overview.md)
