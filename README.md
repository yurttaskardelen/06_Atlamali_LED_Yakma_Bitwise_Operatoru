# 06_Atlamali_LED_Yakma_Bitwise_Operatoru (v2)

Bu proje, **STM32F407-Discovery** kartı üzerinde 4 adet LED kullanarak 3 aşamalı, karmaşık bir animasyon senaryosu gerçekleştirir.

Bu depo, **v1 (Uzun Kod)** projesindeki senaryonun **refactor** edilmiş (yeniden düzenlenmiş) halidir. v1'de her pin için ayrı ayrı `HAL_GPIO_WritePin` komutu kullanılırken, bu projede C dilinin **Bitwise OR (`|`) operatörü** kullanılarak aynı anda birden fazla pinin kontrol edilmesi ve kod satırlarının önemli ölçüde kısaltılması amaçlanmıştır.

> **💡 Kodun Gelişim Aşamaları**
>
> * **v1 (Temel Yöntem):** Her komutun açıkça, tekrar edilerek yazıldığı uzun kod.
>   ➡️ **[05_Atlamali_LED_Yakma_Uzun_Kod](https://github.com/yurttaskardelen/05_Atlamali_LED_Yakma_Uzun_Kod)**
>
> * **v3 (Dizi & Döngü Yöntemi):** Bu senaryonun `for` döngüleri ve diziler kullanılarak en verimli ve ölçeklenebilir hale getirildiği profesyonel yöntem.
>   ➡️ **[07_Atlamali_LED_Yakma_Dizi_For](https://github.com/yurttaskardelen/07_Atlamali_LED_Yakma_Dizi_For)**

---

### 🎯 Proje Senaryosu

Animasyon, bir önceki `05_` projesiyle (pinler hariç) aynı 3 ana aşamadan oluşur ve `while(1)` içinde sürekli tekrar eder:

1.  **Aşama 1 (Sıralı Yanıp Sönme):**
    * Dört LED (`PA2`, `PA4`, `PA1`, `PA3`) sırayla 1'er saniye yanar ve bir sonrakine geçmeden önce söner.
2.  **Aşama 2 (İkili Yanıp Sönme):**
    * Önce `PA2` & `PA4` LED'leri birlikte 4 saniye yanar, sonra söner.
    * Ardından `PA1` & `PA3` LED'leri birlikte 4 saniye yanar, sonra söner.
3.  **Aşama 3 (Toplu Yanıp Sönme):**
    * Dört LED'in tümü (`PA1`, `PA2`, `PA3`, `PA4`) 4 saniye boyunca birlikte yanar.
    * Ardından tüm LED'ler 4 saniye boyunca sönük kalır.
4.  Döngü başa döner.

**Zamanlama:**
* **Sıralı Yanma (Aşama 1):** 1000 ms (1 saniye)
* **İkili Yanma (Aşama 2):** 4000 ms (4 saniye)
* **Toplu Yanma (Aşama 3):** 4000 ms (4 saniye)
* **Toplu Sönme (Aşama 3):** 4000 ms (4 saniye)

---

### 🛠️ Gerekli Donanım

* **1x** STM32F407-Discovery Geliştirme Kartı
* **4x** Tercih edilen renklerde LED
* **4x** 220 ya da 330 Ohm Direnç (LED'ler için ön direnç)
* Breadboard ve Jumper kablolar

---

### 🔌 Devre Şeması

LED'lerin anot (uzun) bacakları STM32 pinlerine, katot (kısa) bacakları ise direnç üzerinden GND hattına bağlanmalıdır.

| LED (Senaryo Sırası) | Direnç | STM32 Pini |
| :--- | :--- | :--- |
| LED 1 | 220 Ohm | `PA2` |
| LED 2 | 220 Ohm | `PA4` |
| LED 3 | 220 Ohm | `PA1` |
| LED 4 | 220 Ohm | `PA3` |
| (Tümü) | - | `GND` |

<img width="473" height="404" alt="Pin_Baglantilari" src="https://github.com/user-attachments/assets/d1e46952-8afa-4526-878a-974330d2baa7" />


### Kod Bloğu

Bu "Bitwise" versiyonunda, `HAL_GPIO_WritePin` fonksiyonuna birden fazla pin göndermek için `|` (Bitwise OR) operatörü kullanılır. Bu sayede 4 satır `WritePin` komutu yerine 1 satır kullanılır.

<img width="1052" height="701" alt="06" src="https://github.com/user-attachments/assets/dbbd1452-a99a-4bb6-afaa-e03a28db74d9" />

---

### 🚀 Nasıl Kullanılır?

1.  Bu depoyu klonlayın (`git clone ...`).
2.  STM32CubeIDE yazılımını açın.
3.  `File > Open Projects from File System...` seçeneği ile proje klasörünü seçin.
4.  Proje içindeki `.ioc` dosyasını açarak pin yapılandırmasını inceleyebilirsiniz.
5.  Derleyin (Build) ve ST-Link V2 üzerinden kartınıza yükleyin (Run).
