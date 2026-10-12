# Lekcje 1-5

Dokument podsumowuje pierwsze pięć części kursu STM32 F4 oraz wskazuje, które elementy zostały wykonane w projektach z tego repozytorium. Projekty używają STM32F407G-DISC1, dlatego konfiguracja pinów i urządzenia docelowego różni się od oryginalnej płytki STM32F411 Discovery użytej w kursie.

## Lekcja 1 - wprowadzenie do STM32 F4, HAL i Cube

Pierwsza część przedstawia zakres szkolenia, wymagany sprzęt i sposób pracy. Główne założenie to praktyczna nauka peryferiów STM32 przy użyciu:

- HAL jako warstwy abstrakcji sprzętu,
- CubeMX do graficznej konfiguracji pinów, zegarów i peryferiów,
- płytki Discovery z wbudowanym ST-LINK-iem,
- kolejnych ćwiczeń obejmujących GPIO, ADC, timery, UART, I2C, SPI i USB.

Ta lekcja nie tworzy jeszcze osobnego programu. Stanowi kontekst dla czterech wariantów GPIO zapisanych w katalogach `1_4_V1...V4`.

## Lekcja 2 - narzędzia programistyczne

Materiał wyjaśnia rolę STM32CubeMX, bibliotek HAL, środowiska programistycznego i narzędzi do obserwowania zmiennych. CubeMX generuje kod inicjalizacyjny, a HAL udostępnia funkcje takie jak `HAL_GPIO_WritePin()` czy `HAL_ADC_Start()` bez ręcznej obsługi wszystkich rejestrów.

Kurs korzystał historycznie z SW4STM32 i STMStudio. W tym repozytorium ich rolę przejmuje głównie STM32CubeIDE: generuje projekt, buduje ELF, programuje układ przez ST-LINK i udostępnia debugger. Wygoda HAL przyspiesza pracę, ale nadal wymaga rozumienia konfiguracji zegarów, pinów oraz ograniczeń peryferiów.

## Lekcja 3 - sprzęt i konfiguracja środowiska

Lekcja omawia płytkę Discovery, wbudowany ST-LINK, instalację narzędzi i tworzenie projektu z konfiguracji CubeMX. W repozytorium wybrano:

- płytkę `STM32F407G-DISC1`,
- mikrokontroler `STM32F407VGTx` w obudowie LQFP100,
- pamięć Flash od adresu `0x08000000`,
- programowanie przez SWD i wbudowany ST-LINK.

Każdy projekt zachowuje plik `.ioc`, skrypty linkera oraz pliki `.project/.cproject`, dzięki czemu można go ponownie zaimportować do CubeIDE. Kod własny należy umieszczać między znacznikami `USER CODE BEGIN/END`, aby regeneracja CubeMX go nie usunęła.

## Lekcja 4 - GPIO, przycisk i przerwania

Ćwiczenia lekcji 4 są zapisane jako cztery kolejne warianty.

### V1 - wyjścia GPIO

Projekt `1_4_V1_STM32F407VG_GPIO1/` steruje czterema diodami płytki na pinach `PD12-PD15`. Pokazuje ustawianie i zerowanie wyjść przez `HAL_GPIO_WritePin()`, przełączanie przez `HAL_GPIO_TogglePin()` oraz tworzenie widocznych sekwencji za pomocą `HAL_Delay()`.

### V2 - odczyt przycisku przez polling

Projekt `1_4_V2_STM32F407VG_GPIO2/` konfiguruje przycisk użytkownika na `PA0` jako wejście. Pętla główna cyklicznie wywołuje `HAL_GPIO_ReadPin()` i ustawia stan zielonej diody. Wariant pokazuje prostotę pollingu, ale także jego wadę: procesor stale sprawdza wejście.

### V3 - przerwanie zewnętrzne EXTI

Projekt `1_4_V3_STM32F407VG_GPIO3_interrupts/` konfiguruje `PA0` jako EXTI wyzwalane zboczem narastającym i opadającym. Obsługa trafia do `HAL_GPIO_EXTI_Callback()`, gdzie zliczane są wywołania i aktualizowany jest stan diody. Pełne naciśnięcie i zwolnienie może wywołać callback dwa razy.

### V4 - eksperyment z eliminacją drgań

Projekt `1_4_V4_STM32F407VG_GPIO4_interrupts+timer/` rozwija wariant EXTI o flagę `debounce_flag`. Kod celowo dokumentuje, że eliminacja drgań bez poprawnej podstawy czasu nie działa niezawodnie. Pomimo członu `+timer` w nazwie projektu nie ma obecnie konfiguracji `TIM` ani funkcji `MX_TIM..._Init()`; jest to etap eksperymentalny przed debounce opartym na timerze.

## Lekcja 5 - ADC i pomiar temperatury

Projekt `5_STM32F407VG_ADC/` konfiguruje `ADC1` do odczytu wewnętrznego czujnika temperatury. Używa rozdzielczości 12 bitów, preskalera zegara `PCLK/8` i czasu próbkowania 480 cykli.

Program:

1. uruchamia konwersję przez `HAL_ADC_Start()`,
2. oczekuje na wynik przez `HAL_ADC_PollForConversion()`,
3. pobiera próbkę funkcją `HAL_ADC_GetValue()`,
4. przelicza wynik na napięcie `Vsense`,
5. wyznacza przybliżoną temperaturę na podstawie `V25` i `Avg_slope`,
6. przygotowuje tekst temperatury i sygnalizuje działanie pomarańczową diodą.

Materiał kursu omawia również przerwania ADC, DMA oraz wizualizację danych w STMStudio. Aktualny projekt repozytorium implementuje jednak tylko wariant z pollingiem - nie zawiera uchwytu DMA ani `HAL_ADC_Start_DMA()`. Utworzony bufor tekstowy `Data` nie jest jeszcze wysyłany przez UART lub USB.

## Kolejne kroki

- dokończyć debounce przycisku z użyciem timera,
- ujednolicić wewnętrzną nazwę projektu V4 i nazwę jego pliku `.ioc`,
- rozbudować lekcję 5 o przerwania ADC i DMA,
- dodać UART do wysyłania wyniku temperatury,
- dokumentować wynik testu na sprzęcie przy każdej kolejnej lekcji.
