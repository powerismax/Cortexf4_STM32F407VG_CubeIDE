# STM32F407VG - ćwiczenia z kursu STM32 F4

Repozytorium dokumentuje pierwsze pięć lekcji kursu STM32 F4 opublikowanego przez Forbot. Materiał szkoleniowy wprowadza biblioteki HAL, konfigurator STM32CubeMX, środowisko programistyczne oraz podstawowe peryferia mikrokontrolera. Oryginalny kurs korzysta z płytki STM32F411 Discovery i starszego środowiska SW4STM32; ćwiczenia w tym repozytorium zostały dostosowane do `STM32F407G-DISC1` oraz STM32CubeIDE.

Szczegółowy opis zakresu i wykonanych ćwiczeń znajduje się w [LESSONS_1-5.md](LESSONS_1-5.md).

## Sprzęt i narzędzia

- płytka `STM32F407G-DISC1` z mikrokontrolerem STM32F407VGTx,
- wbudowany programator ST-LINK, komunikacja SWD,
- STM32CubeIDE oraz generator konfiguracji CubeMX,
- biblioteki STM32 HAL i CMSIS,
- STM32CubeProgrammer lub STM32 ST-LINK Utility do ręcznego programowania.

CubeIDE wgrywa bezpośrednio plik `.elf`. Dla ST-LINK Utility można włączyć generowanie Intel HEX w `Project > Properties > C/C++ Build > Settings > MCU Post build outputs`.

## Organizacja repozytorium

| Katalog | Zakres |
| --- | --- |
| `1_4_V1_STM32F407VG_GPIO1/` | sterowanie diodami GPIO |
| `1_4_V2_STM32F407VG_GPIO2/` | odczyt przycisku metodą polling |
| `1_4_V3_STM32F407VG_GPIO3_interrupts/` | przycisk obsługiwany przez EXTI |
| `1_4_V4_STM32F407VG_GPIO4_interrupts+timer/` | eksperyment z debounce po przerwaniu |
| `5_STM32F407VG_ADC/` | ADC i wewnętrzny czujnik temperatury |

Każdy katalog jest niezależnym projektem CubeIDE. Kod aplikacji znajduje się w `Core/`, konfiguracja sprzętu w pliku `.ioc`, a HAL/CMSIS w `Drivers/`.

## Uruchomienie projektu

1. Sklonuj repozytorium:

   ```powershell
   git clone https://github.com/powerismax/Cortexf4_STM32F407VG_CubeIDE.git
   ```

2. W CubeIDE wybierz `File > Import > Existing Projects into Workspace` i wskaż katalog wybranego projektu.
3. Podłącz komputer do złącza USB wbudowanego ST-LINK-a.
4. Wykonaj `Project > Build Project`.
5. Użyj `Run` do zaprogramowania lub `Debug` do wgrania programu z symbolami debuggera.

Katalogi `Debug/` i `Release/` są generowane lokalnie i nie są wersjonowane.

## Pierwsza konfiguracja Git

Repozytorium utworzono lokalnie. Ponieważ używana wersja Git nie obsługiwała `git init -b main`, gałąź ustawiono osobno:

```powershell
git init
git symbolic-ref HEAD refs/heads/main
git remote add origin https://github.com/powerismax/Cortexf4_STM32F407VG_CubeIDE.git
```

Przed pierwszym commitem dodano `.gitignore`, który pomija metadane workspace, wyniki kompilacji, pliki `.elf/.hex/.bin`, logi i lokalne kopie materiałów PDF. Jeżeli pliki zostały dodane przed utworzeniem `.gitignore`, indeks można bezpiecznie odbudować:

```powershell
git rm -r --cached -f .
git add .
```

Pierwszy commit i push wykonano poleceniami:

```powershell
git commit -m "Pierwszy import lekcje 1-5"
git push -u origin main
```

Przy kolejnych zmianach wystarczy:

```powershell
git add .
git commit -m "Krótki opis zmiany"
git push
```
