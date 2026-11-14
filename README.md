# STM32F103 Geliştirme Kartı

KiCad ile tasarlanmış, **STM32F103C8T6** tabanlı kompakt bir mikrodenetleyici kartı. USB ile beslenir ve programlama, seri haberleşme ve I²C için pin başlıkları içerir.

| Özellik | Değer |
|---|---|
| Mikrodenetleyici | STM32F103C8T6 (ARM Cortex-M3, LQFP-48) |
| Kart boyutu | 37 × 40 mm |
| Katman sayısı | 2 (F.Cu sinyal, B.Cu güç/GND) |
| Besleme | USB Micro-B (5 V) → AMS1117-3.3 ile 3,3 V |
| Kristal | 16 MHz (HSE) |
| Tasarım aracı | KiCad 10 |

## Özellikler

- **USB Micro-B** bağlantısı (D+ hattında 1k5 pull-up)
- **AMS1117-3.3** lineer regülatör ile 3,3 V besleme
- **Ferrit boncuk** ile filtrelenmiş analog besleme (+3.3VA)
- **16 MHz kristal** osilatör (HSE)
- **BOOT0 seçimi** için SPDT kaydırmalı anahtar
- **Güç LED'i**
- **4 adet M2 montaj deliği**
- Tüm dekuplaj kondansatörleri MCU besleme pinlerinin yanında

## Pin Başlıkları

Kartta üç adet 4 pinli, 2,54 mm aralıklı başlık vardır:

| Başlık | Sinyaller | Kullanım |
|---|---|---|
| SWD | SWDIO, SWCLK, NRST, GND / 3V3 | ST-Link ile programlama ve debug |
| USART1 | USART1_TX, USART1_RX, GND / 3V3 | Seri haberleşme, UART bootloader |
| I²C2 | I2C2_SCL, I2C2_SDA, GND / 3V3 | Sensör ve modül bağlantısı |

> Başlıkların tam pin sırası için şemaya (`stmdeneme.pdf`) bakın.

## Malzeme Listesi (BOM)

| Adet | Parça | Değer | Kılıf |
|---|---|---|---|
| 1 | Mikrodenetleyici | STM32F103C8T6 | LQFP-48 7×7 mm |
| 1 | LDO regülatör | AMS1117-3.3 | SOT-223 |
| 1 | Kristal | 16 MHz | SMD 3225 4-pin |
| 1 | USB konnektör | Micro-B (Würth 629105150521) | SMD |
| 1 | Ferrit boncuk | 120 Ω | 0603 |
| 1 | LED | Kırmızı | 0603 |
| 1 | Kaydırmalı anahtar | SPDT (PCM12) | SMD |
| 5 | Kondansatör | 100 nF | 0402 |
| 2 | Kondansatör | 10 pF | 0402 |
| 2 | Kondansatör | 1 µF | 0402 |
| 1 | Kondansatör | 10 nF | 0402 |
| 1 | Kondansatör | 10 µF | 0603 |
| 2 | Kondansatör | 22 µF | 0805 |
| 4 | Direnç | 1,5 kΩ | 0402 |
| 1 | Direnç | 10 kΩ | 0402 |
| 3 | Pin başlığı | 1×04, 2,54 mm | THT dikey |
| 4 | Montaj deliği | 2,1 mm | — |

## Dosya Yapısı

```
├── stmdeneme.kicad_pro         # KiCad proje dosyası
├── stmdeneme.kicad_sch         # Şema
├── stmdeneme.kicad_pcb         # PCB yerleşimi
├── stmdeneme.pdf               # Şema çıktısı (PDF)
└── stmdeneme-*.pdf             # Katman çıktıları
    ├── F_Cu / B_Cu             # Üst / alt bakır
    ├── F_Mask / B_Mask         # Lehim maskesi
    ├── F_Paste                 # Lehim pastası
    ├── F_Silkscreen            # Serigrafi
    └── Edge_Cuts               # Kart kenarı
```

## Projeyi Açmak

1. [KiCad](https://www.kicad.org/download/) kurun (sürüm 10 veya üzeri).
2. Repoyu klonlayın:
   ```bash
   git clone https://github.com/kbur4k-kbur4k/Stm32pcbdesign.git
   ```
3. `stmdeneme.kicad_pro` dosyasını KiCad ile açın.

## Programlama

- **SWD:** ST-Link'i SWD başlığına bağlayın; STM32CubeProgrammer veya OpenOCD ile yükleyin.
- **UART bootloader:** BOOT0 anahtarını **1** konumuna alın, USB-TTL dönüştürücüyü USART1'e bağlayın, kartı resetleyin ve STM32CubeProgrammer (UART modu) ile yükleyin. Yüklemeden sonra BOOT0'ı **0** konumuna geri alın.
