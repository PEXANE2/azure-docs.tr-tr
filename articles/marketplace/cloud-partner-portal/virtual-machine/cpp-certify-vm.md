---
title: Azure Marketi için VM görüntünüzü onaylama
description: Azure Market sertifikası için bir VM görüntüsünü test etme ve gönderme hakkında bilgiler sağlar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/26/2018
ms.author: dsindona
ms.openlocfilehash: 81dec159a8a84adce97750666baba455ae3fdd93
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82147086"
---
# <a name="certify-your-vm-image"></a>VM görüntünüzü onaylama

> [!IMPORTANT]
> 13 Nisan 2020 ' den itibaren, Azure sanal makine tekliflerinizin yönetimini Iş Ortağı Merkezi 'ne taşımaya başlayacağız. Geçişten sonra, Iş Ortağı Merkezi 'nde tekliflerinizi oluşturup yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Azure VM görüntü sertifikadaki](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-vm-image-certification) yönergeleri izleyin.

Sanal makinenizi (VM) oluşturup dağıttıktan sonra, VM görüntüsünü Azure Market sertifikası için test etmeniz ve göndermeniz gerekir. Bu makalede, *Azure Sertifikalı sertifika sınama aracı 'nın*nereden alınacağı, sanal makine görüntünüzü onaylamak için bu aracın nasıl kullanılacağı ve doğrulama sonuçlarının VHD 'Nizin bulunduğu Azure kapsayıcısına nasıl yükleneceği açıklanmaktadır. 


## <a name="download-and-run-the-certification-test-tool"></a>Sertifika test aracını indirme ve çalıştırma

Azure Sertifikalı sertifika test aracı yerel bir Windows makinesinde çalışır, ancak Azure tabanlı bir Windows veya Linux VM 'yi sınar.  Bu, VHD 'nizi hazırlamaya yönelik yönergelerin ve gereksinimlerin karşılandığından emin olmak için, Kullanıcı VM Görüntünüzün Microsoft Azure uyumlu olduğunu doğrular. Aracın çıktısı, VM sertifikası istemek için [bulut iş ortağı portalı](https://cloudpartner.azure.com) yükleyecek olan bir uyumluluk rapordur.

1. [Azure Sertifikalı en son sertifika test aracı](https://www.microsoft.com/download/details.aspx?id=44299)'nı indirip yükleyin. 
2. Sertifika aracını açın ve ardından **Yeni test Başlat**' a tıklayın.
3. **Test bilgileri** ekranında, test çalıştırması Için bir **Test adı** girin.
4. `Windows Server` Ya `Linux`da VM 'niz için **platformu** seçin. Platform seçiminiz, kalan seçenekleri etkiler.
5. VM 'niz bu veritabanı hizmetini kullanıyorsa **Azure SQL veritabanı Için test** onay kutusunu seçin.

   ![Sertifika test aracı ilk sayfası](./media/publishvm_025.png)


## <a name="connect-the-certification-tool-to-a-vm-image"></a>Sertifika aracını bir VM görüntüsüne bağlama

  Araç, [PowerShell](https://docs.microsoft.com/powershell/) ile Windows tabanlı VM 'lere bağlanır ve [SSH.net](https://www.ssh.com/ssh/protocol/)üzerinden Linux VM 'lerine bağlanır.

### <a name="connect-the-certification-tool-to-a-linux-vm-image"></a>Sertifika aracını bir Linux VM görüntüsüne bağlama

1. **SSH kimlik doğrulama** modunu seçin: `Password Authentication` veya `key File Authentication`.
2. Parola tabanlı kimlik doğrulaması kullanıyorsanız, **VM DNS adı**, **Kullanıcı adı**ve **parola**değerlerini girin.  İsteğe bağlı olarak, varsayılan **SSH bağlantı noktası** numarasını değiştirebilirsiniz.

     ![Linux VM görüntüsünün parola doğrulaması](./media/publishvm_026.png)

3. Anahtar dosya tabanlı kimlik doğrulaması kullanıyorsanız, **VM DNS adı**, **Kullanıcı adı**ve **özel anahtar** konumu değerlerini girin.  İsteğe bağlı olarak, bir **parola** sağlayabilir veya varsayılan **SSH bağlantı noktası** numarasını değiştirebilirsiniz.

     ![Linux VM görüntüsünün dosya kimlik doğrulaması](./media/publishvm_027.png)

### <a name="connect-the-certification-tool-to-a-windows-based-vm-image"></a>**Sertifika aracını Windows tabanlı bir VM görüntüsüne bağlama**
1. Tam **VM DNS adını** (örneğin, `MyVMName.Cloudapp.net`) girin.
2. **Kullanıcı adı** ve **parola**değerlerini girin.

   ![Windows VM görüntüsünün parola doğrulaması](./media/publishvm_028.png)


## <a name="run-a-certification-test"></a>Sertifika testi çalıştırma

VM Görüntünüz için sertifika aracında parametre değerlerini verdikten sonra, sanal makinenize geçerli bir bağlantı sağlamak için **Bağlantıyı Sına** ' yı seçin. Bir bağlantı doğrulandıktan sonra, testi başlatmak için **İleri** ' yi seçin.  Test tamamlandığında, test sonuçları (başarılı/başarısız/uyarı) ile bir tablo görüntülenir.  Aşağıdaki örnek, bir Linux VM testi için test sonuçlarını gösterir. 

![Linux VM görüntüsü için sertifika test sonuçları](./media/publishvm_029.png)

Testlerin herhangi biri başarısız olursa, görüntünüz sertifikalı *değildir* . Bu durumda, gereksinimleri ve hata iletilerini gözden geçirin, belirtilen değişiklikleri yapın ve testi yeniden çalıştırın. 

Otomatik testten sonra, sanal makine görüntünüz hakkında **Soru formu** ekranında ek bilgi sağlamanız gerekir.  Gerçekleştirmeniz gereken iki sekme içerir.  **Genel değerlendirme** sekmesi **doğru/yanlış** soruları Içerir, ancak **çekirdek özelleştirmesi** birden çok seçim ve serbest biçimli soru içerir.  Her iki sekmede de soruları tamamladıktan sonra **İleri**' yi seçin.

![Sertifika araç Anketi](./media/publishvm_030.png)

Son ekran, bir Linux VM görüntüsü için SSH erişim bilgileri ve özel durumlar aradıysanız başarısız olan değerlendirmelere açıklama gibi ek bilgiler sağlamanıza olanak sağlar. 

Son olarak, anket yanıtlarınıza ek olarak yürütülen test çalışmalarının test sonuçlarını ve günlük dosyalarını indirmek için **rapor oluştur** ' a tıklayın. Sonuçları VHD 'niz ile aynı kapsayıcıya kaydedin.

![Sertifika test sonuçlarını Kaydet](./media/publishvm_031.png)


## <a name="next-steps"></a>Sonraki adımlar

Ardından, Market 'e gönderdiğiniz [her VHD için bir Tekdüzen Kaynak tanımlayıcısı (URI)](./cpp-get-sas-uri.md) oluşturacaksınız. 
