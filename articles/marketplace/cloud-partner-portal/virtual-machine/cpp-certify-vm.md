---
title: Azure Marketi için VM görüntünüzü sertifikası
description: Azure Marketi sertifikası için vm görüntüsünün nasıl test edilip gönderilebildiğini açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/26/2018
ms.author: dsindona
ms.openlocfilehash: 0225069179e0d7d94a983c5161976a5c1933fac5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80278204"
---
# <a name="certify-your-vm-image"></a>VM görüntünüzü sertifikasına

Sanal makinenizi (VM) oluşturup dağıttıktan sonra, Azure Marketi sertifikası için VM görüntüsünü test etmeli ve göndermeniz gerekir. Bu makalede, Azure *Sertifikalı Sertifika Test Aracı'nı*nereden alacağınızı, VM resminizi onaylamak için bu aracı nasıl kullanacağınızı ve doğrulama sonuçlarını VH'lerinizin bulunduğu Azure kapsayıcısına nasıl yükleyebileceğiniz açıklanmaktadır. 


## <a name="download-and-run-the-certification-test-tool"></a>Sertifika test aracını indirin ve çalıştırın

Azure Sertifikalı Sertifika Test Aracı yerel bir Windows makinesinde çalışır, ancak Azure tabanlı bir Windows veya Linux VM'yi sınar.  Kullanıcı VM görüntünüzün Microsoft Azure ile uyumlu olduğunu doğrular— VHD'nizi hazırlama yla ilgili kılavuz ve gereksinimlerin karşılandığı. Aracın çıktısı, VM sertifikası istemek üzere Bulut [İş Ortağı Portalı'na](https://cloudpartner.azure.com) yükleyeceğiniz bir uyumluluk raporudur.

1. Azure Sertifikalı için en son Sertifika Test Aracını indirin ve [yükleyin.](https://www.microsoft.com/download/details.aspx?id=44299) 
2. Sertifika aracını açın ve ardından **Yeni Testi Başlat'ı**tıklatın.
3. Test **Bilgileri** ekranından, test çalışması için bir **Test Adı** girin.
4. VM'iniz için **Platform'u** seçin `Windows Server` veya `Linux`. Platform seçiminiz kalan seçenekleri etkiler.
5. VM'niz bu veritabanı hizmetini kullanıyorsa, **Azure SQL Veritabanı** için Test onay kutusunu seçin.

   ![Cert test aracı başlangıç sayfası](./media/publishvm_025.png)


## <a name="connect-the-certification-tool-to-a-vm-image"></a>Sertifika aracını VM görüntüsüne bağlama

  Araç [PowerShell](https://docs.microsoft.com/powershell/) ile Windows tabanlı VM'lere bağlanır ve [SSH.Net](https://www.ssh.com/ssh/protocol/)aracılığıyla Linux VM'lere bağlanır.

### <a name="connect-the-certification-tool-to-a-linux-vm-image"></a>Sertifika aracını Linux VM görüntüsüne bağlama

1. **SSH Kimlik Doğrulama** modunu seçin: `Password Authentication` veya `key File Authentication`.
2. Parola tabanlı kimlik doğrulaması kullanıyorsanız, **VM DNS Adı,** **Kullanıcı adı**ve **Parola**için değerleri girin.  İsteğe bağlı olarak, varsayılan **SSH Bağlantı Noktası** numarasını değiştirebilirsiniz.

     ![Linux VM Image'ın parola kimlik doğrulaması](./media/publishvm_026.png)

3. Anahtar dosya tabanlı kimlik doğrulamasını kullanıyorsanız, **VM DNS Adı,** **Kullanıcı adı**ve **Özel anahtar** konumu için değerleri girin.  İsteğe bağlı olarak, bir **Geçiş Cümlesi** sağlayabilir veya varsayılan **SSH Bağlantı Noktası** numarasını değiştirebilirsiniz.

     ![Linux VM Image dosya kimlik doğrulaması](./media/publishvm_027.png)

### <a name="connect-the-certification-tool-to-a-windows-based-vm-image"></a>**Sertifika aracını Windows tabanlı vm görüntüsüne bağlama**
1. Tam nitelikli **VM DNS adını** girin `MyVMName.Cloudapp.net`(örneğin, ).
2. **Kullanıcı Adı** ve **Parola**için değerleri girin.

   ![Windows VM Image'ın parola kimlik doğrulaması](./media/publishvm_028.png)


## <a name="run-a-certification-test"></a>Sertifika testi çalıştırma

Sertifika aracında VM görüntünüz için parametre değerlerini sağladıktan sonra, VM'inize geçerli bir bağlantı sağlamak için **Test Bağlantısı'nı** seçin. Bağlantı doğrulandıktan sonra testi başlatmak için **İleri'yi** seçin.  Test tamamlandığında, test sonuçları (Geç/Başarısız/Uyarı) ile bir tablo görüntülenir.  Aşağıdaki örnek, bir Linux VM testinin test sonuçlarını gösterir. 

![Linux VM görüntüsü için sertifika testi sonuçları](./media/publishvm_029.png)

Testlerden herhangi biri başarısız olursa, resminiz sertifikalı *değildir.* Bu durumda, gereksinimleri ve hata iletilerini gözden geçirin, belirtilen değişiklikleri yapın ve testi yeniden çalıştırın. 

Otomatik testten sonra, **Soru Formu** ekranında VM görüntünüz hakkında ek bilgi sağlamanız gerekir.  Tamamlamanız gereken iki sekme içerir.  **Genel Değerlendirme** sekmesi **Doğru/Yanlış** soruları içerirken, **Çekirdek Özelleştirme** birden çok seçim ve serbest biçimli sorular içerir.  Her iki sekmedeki soruları tamamlayın ve **Ardından 'yı**seçin.

![Belgelendirme Aracı Anketi](./media/publishvm_030.png)

Son ekran, bir Linux VM görüntüsü için SSH erişim bilgileri ve özel durumlar arıyorsanız başarısız değerlendirmeler için bir açıklama gibi ek bilgiler sağlamanızı sağlar. 

Son olarak, test sonuçlarını indirmek için **Raporu Oluştur'u** ve ankete yanıtlarınıza ek olarak çalıştırılan test örnekleriiçin dosyaları günlüğe kaydedin. Sonuçları VHD(ler'inizle aynı kapsayıcıya kaydedin).

![Sertifika testi sonuçlarını kaydetme](./media/publishvm_031.png)


## <a name="next-steps"></a>Sonraki adımlar

Ardından, pazara gönderdiğiniz [her VHD için tek tip bir kaynak tanımlayıcıları (URI) oluşturursunuz.](./cpp-get-sas-uri.md) 
