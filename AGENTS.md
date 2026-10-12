# Repository Guidelines

## Project Structure & Module Organization

This workspace contains five independent STM32F407VG CubeIDE examples. The `1_4_V1...V4` directories cover the GPIO exercises from lessons 1-4, while `5_STM32F407VG_ADC/` covers lesson 5. Application code lives in `Core/Src/`, public headers in `Core/Inc/`, and startup assembly in `Core/Startup/`. `Drivers/` contains CMSIS and STM32 HAL sources; `Middlewares/` and `USB_HOST/` hold generated USB support. The `.ioc` file is the source of truth for CubeMX peripheral configuration. Reference PDFs are local training materials and are excluded from Git.

## Build, Test, and Development Commands

Import the required project into STM32CubeIDE and select **Project > Build Project** for the normal workflow. Generated makefiles also support command-line builds when the Arm GNU toolchain is available:

```powershell
make -C 5_STM32F407VG_ADC/Debug all
make -C 5_STM32F407VG_ADC/Debug clean
make -C 1_4_V3_STM32F407VG_GPIO3_interrupts/Debug all
```

Replace the project name as needed. Do not hand-edit generated `Debug/` or `Release/` files; regenerate them through CubeIDE.

## Coding Style & Naming Conventions

Follow the existing STM32Cube-generated C style: two-space indentation, braces on their own lines, and HAL types such as `GPIO_InitTypeDef`. Keep CubeMX-managed edits inside matching `/* USER CODE BEGIN ... */` and `/* USER CODE END ... */` blocks so regeneration preserves them. Use `MX_<PERIPHERAL>_Init` for initialization, `HAL_*_Callback` for HAL callbacks, and descriptive `snake_case` for application variables. Avoid modifying `Drivers/` or `Middlewares/` unless intentionally updating a dependency.

## Testing Guidelines

No automated test framework or coverage requirement is present. A change is complete only after its project builds without new warnings and is exercised on the target board. Document the hardware setup and verify relevant behavior (GPIO/LED state, interrupts, timer timing, ADC readings, or USB enumeration). Include serial logs or debugger observations when useful.

## Commit & Pull Request Guidelines

This workspace has no Git history from which to infer conventions. Use concise imperative commits, optionally scoped, for example `gpio3: debounce EXTI input`. Keep generated-output churn out of commits unless required. Pull requests should identify the affected CubeIDE project, summarize `.ioc` and source changes, list build and board-test results, link related issues, and attach logs or photos for observable hardware behavior.
